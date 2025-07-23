<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Transporte Fácil</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(to right, #1e3c72, #2a5298);
      color: #fff;
      margin: 0;
      padding: 0;
      line-height: 1.6;
    }

    .motorista-info {
      background-color: #003366;
      padding: 15px;
      text-align: center;
      font-size: 18px;
      font-weight: bold;
      color: #ffcc00;
      text-transform: uppercase;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }

    header {
      background-color: #ff6b00;
      padding: 20px;
      text-align: center;
      color: #fff;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      margin-bottom: 20px;
    }

    header h1 {
      margin: 0;
      font-size: 32px;
      text-transform: uppercase;
    }

    /* Estilos para os contêineres de conteúdo */
    .container-section {
      background: #ffffff;
      color: #333;
      padding: 25px;
      border-radius: 15px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      max-width: 700px; /* Largura um pouco maior para acomodar tudo */
      margin: 30px auto;
    }

    .container-section h2 {
      text-align: center;
      color: #ff6b00;
      margin-bottom: 20px;
      font-size: 24px;
    }

    form {
      display: grid;
      grid-template-columns: 1fr;
      gap: 15px;
    }

    label {
      font-weight: bold;
      margin-top: 5px;
      display: block;
      color: #555;
    }

    input[type="text"],
    input[type="tel"],
    input[type="number"],
    select {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #ccc;
      box-sizing: border-box;
      transition: border-color 0.3s;
    }

    input[type="text"]:focus,
    input[type="tel"]:focus,
    input[type="number"]:focus,
    select:focus {
      border-color: #007bff;
      outline: none;
    }

    .actions {
      display: flex;
      justify-content: space-between;
      gap: 10px;
      margin-top: 5px;
    }

    .actions button {
      flex: 1;
      background-color: #007bff;
      color: #fff;
      border: none;
      padding: 12px;
      font-size: 16px;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }

    .actions button:hover {
      background-color: #0056b3;
    }

    button[type="submit"] {
      background-color: #ff6b00;
      color: #fff;
      border: none;
      margin-top: 20px;
      padding: 15px;
      font-size: 18px;
      font-weight: bold;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }

    button[type="submit"]:hover {
      background-color: #e65a00;
    }

    .success {
      text-align: center;
      color: green;
      font-weight: bold;
      margin-top: 15px;
      padding: 10px;
      background-color: #e6ffe6;
      border-radius: 8px;
      border: 1px solid #a3e6a3;
    }

    /* Estilos para a seção de assentos */
    .seat-reservation-container h2 {
      color: #007bff; /* Azul para destaque na seção de poltronas */
    }

    .seat-map {
      display: grid;
      grid-template-columns: repeat(5, 1fr); /* 5 poltronas por linha para mais compactação */
      gap: 10px;
      margin-top: 20px;
      padding: 10px;
      border: 1px solid #eee;
      border-radius: 8px;
      background-color: #f9f9f9;
    }

    .seat {
      width: 50px; /* Tamanho ajustado para 5 colunas */
      height: 50px; /* Tamanho ajustado */
      background-color: #e0e0e0;
      border: 1px solid #ccc;
      border-radius: 5px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s, border-color 0.3s;
      box-shadow: 1px 1px 3px rgba(0,0,0,0.1);
    }

    .seat.available {
      background-color: #d4edda; /* Verde claro */
      border-color: #28a745;
    }

    .seat.occupied {
      background-color: #f8d7da; /* Vermelho claro */
      border-color: #dc3545;
      cursor: not-allowed;
    }

    /* Estilos para a legenda das poltronas */
    .seat-legend {
      display: flex;
      justify-content: center;
      flex-wrap: wrap; /* Permite quebrar linha em telas menores */
      margin-top: 20px;
      gap: 20px;
      font-size: 0.9em;
      padding-top: 15px;
      border-top: 1px dashed #eee;
    }

    .legend-item {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .legend-item .seat {
      width: 25px; /* Tamanho menor para a legenda */
      height: 25px; /* Tamanho menor */
      cursor: default;
      box-shadow: none;
    }

    /* Estilos para a lista de agendamentos (passageiros com poltrona) */
    .agendamentos-container h2 {
      color: #007bff; /* Azul para destaque */
    }

    #listaAgendamentos {
      list-style: none; /* Remove os marcadores padrão da lista */
      padding: 0;
    }

    #listaAgendamentos li {
      background-color: #f0f8ff; /* Azul claro para os itens */
      margin-bottom: 10px;
      padding: 15px;
      border-radius: 8px;
      border: 1px solid #cceeff;
      display: flex;
      flex-direction: column; /* Para empilhar as informações em telas menores */
      gap: 5px;
      font-size: 1.1em;
    }

    #listaAgendamentos li strong {
      color: #333;
    }

    #listaAgendamentos li span.info-passageiro {
      font-size: 0.95em;
      color: #666;
    }

    #listaAgendamentos li span.valor-passageiro {
      font-weight: bold;
      color: #28a745; /* Verde para os valores */
      margin-top: 5px;
      text-align: right;
      width: 100%; /* Ocupa toda a largura para alinhamento */
    }

    /* Estilos para a tabela de passageiros e valores (geral) */
    .lista-container h2 {
      color: #ff6b00;
    }

    #tabelaPassageiros {
      width: 100%;
      border-collapse: collapse;
      margin-top: 15px;
      font-size: 0.95em;
    }

    #tabelaPassageiros th,
    #tabelaPassageiros td {
      border: 1px solid #ddd;
      padding: 10px;
      text-align: left;
    }

    #tabelaPassageiros th {
      background-color: #f2f2f2;
      font-weight: bold;
      color: #555;
    }

    .total-a-receber {
      text-align: right;
      font-weight: bold;
      font-size: 1.2em;
      margin-top: 20px;
      padding-top: 15px;
      border-top: 2px solid #eee;
      color: #333;
    }

    .lista-container button {
      background-color: #28a745; /* Cor verde para exportar */
      color: #fff;
      border: none;
      margin-top: 20px;
      padding: 12px;
      font-weight: bold;
      border-radius: 8px;
      width: 100%;
      cursor: pointer;
      transition: background 0.3s;
    }

    .lista-container button:hover {
      background-color: #218838;
    }

    .acao-remover {
      background-color: #dc3545; /* Cor vermelha para remover */
      color: #fff;
      border: none;
      padding: 6px 12px; /* Aumentado para melhor toque */
      border-radius: 5px;
      cursor: pointer;
      transition: background 0.3s;
      font-size: 0.9em;
    }

    .acao-remover:hover {
      background-color: #c82333;
    }

    /* Estilos para a lista de todos os passageiros cadastrados */
    .all-passengers-container h2 {
      color: #17a2b8; /* Uma cor azul-ciano para destaque */
    }

    #listaTodosPassageiros {
      list-style: disc; /* Usar marcadores de disco */
      padding-left: 20px; /* Recuo para os marcadores */
    }

    #listaTodosPassageiros li {
      background-color: #e0f7fa; /* Azul muito claro */
      margin-bottom: 8px;
      padding: 10px 15px;
      border-radius: 6px;
      border: 1px solid #b2ebf2;
      font-size: 1em;
      color: #333;
    }

    /* Responsividade */
    @media (max-width: 768px) {
      .container-section {
        margin: 20px 10px;
        padding: 15px;
      }
      header h1 {
        font-size: 26px;
      }
      .seat-map {
        grid-template-columns: repeat(3, 1fr); /* 3 poltronas por linha em telas menores */
      }
      .seat {
        width: 45px;
        height: 45px;
      }
      .seat-legend {
        flex-direction: column;
        align-items: flex-start;
        gap: 10px;
      }
      #tabelaPassageiros th,
      #tabelaPassageiros td {
        padding: 8px;
        font-size: 0.85em;
      }
      .actions {
        flex-direction: column;
      }
      .actions button {
        margin-top: 5px;
      }
      #listaAgendamentos li {
        flex-direction: column;
        align-items: flex-start;
      }
      #listaAgendamentos li span.valor-passageiro {
        text-align: left;
        margin-top: 10px;
      }
    }

    @media (max-width: 480px) {
      header h1 {
        font-size: 22px;
      }
      .seat-map {
        grid-template-columns: repeat(2, 1fr); /* 2 poltronas por linha em telas muito pequenas */
      }
      .seat {
        width: 60px; /* Maior para melhor toque */
        height: 60px;
      }
      .total-a-receber {
        font-size: 1.1em;
      }
      button[type="submit"] {
        font-size: 16px;
        padding: 12px;
      }
    }
  </style>
</head>
<body>

  <div class="motorista-info">
    Motorista: João da Silva
  </div>

  <header>
    <h1>Transporte Fácil</h1>
  </header>

  <div class="container-section seat-reservation-container">
    <h2>Reservar Poltrona</h2>
    <div id="seatMap" class="seat-map">
      </div>
    <div class="seat-legend">
      <div class="legend-item"><span class="seat available"></span> Disponível</div>
      <div class="legend-item"><span class="seat occupied"></span> Ocupada</div>
      </div>
  </div>

  <div class="container-section">
    <h2>Registrar Pedido</h2>
    <form id="pedidoForm">
      <label for="tipo">Tipo de Serviço</label>
      <select name="tipo" id="tipo" required>
        <option value="passageiro">Passageiro</option>
        <option value="encomenda">Encomenda</option>
      </select>

      <label for="cliente">Nome do Cliente / Receptor</label>
      <input type="text" name="cliente" id="cliente" required placeholder="Nome completo" />

      <div class="actions">
        <button type="button" onclick="handleSaveClientName()">Gravar Nome</button>
        <button type="button" onclick="handleRemoveClientName()">Remover Nome</button>
      </div>

      <label for="origem">Origem</label>
      <select name="origem" id="origem" required>
        <option value="">Selecione a origem</option>
        <option value="Anísio de Abreu-PI">Anísio de Abreu-PI</option>
        <option value="São Braz-PI">São Braz-PI</option>
        <option value="Tranqueira">Tranqueira</option>
        <option value="Tanque Velho">Tanque Velho</option>
        <option value="Novo Horizonte">Novo Horizonte</option>
        <option value="Gameleira">Gameleira</option>
        <option value="São Raimundo Nonato-PI">São Raimundo Nonato-PI</option>
      </select>

      <label for="destino">Destino / Local de Entrega</label>
      <select name="destino" id="destino" required>
        <option value="">Selecione o destino</option>
        <option value="Anísio de Abreu-PI">Anísio de Abreu-PI</option>
        <option value="São Braz-PI">São Braz-PI</option>
        <option value="Tranqueira">Tranqueira</option>
        <option value="Tanque Velho">Tanque Velho</option>
        <option value="Novo Horizonte">Novo Horizonte</option>
        <option value="Gameleira">Gameleira</option>
        <option value="São Raimundo Nonato-PI">São Raimundo Nonato-PI</option>
      </select>

      <label for="valor">Valor (R$)</label>
      <input type="number" name="valor" id="valor" step="0.01" value="0.00" required />

      <label for="telefone">Telefone</label>
      <input type="tel" name="telefone" id="telefone" required placeholder="(XX) XXXXX-XXXX" />

      <button type="submit">Registrar Pedido</button>
      <div class="success" id="mensagem"></div>
    </form>
  </div>

  <div class="container-section agendamentos-container">
    <h2>Passageiros Agendados</h2>
    <ul id="listaAgendamentos">
      <li>Nenhum passageiro agendado para hoje.</li>
    </ul>
  </div>

  <div class="container-section all-passengers-container">
    <h2>Todos os Passageiros Cadastrados</h2>
    <ul id="listaTodosPassageiros">
      <li>Nenhum passageiro cadastrado ainda.</li>
    </ul>
  </div>

  <div class="container-section lista-container">
    <h2>Detalhes de Pedidos do Dia</h2>
    <table id="tabelaPedidos">
      <thead>
        <tr>
          <th>Cliente</th>
          <th>Tipo</th>
          <th>Poltrona</th>
          <th>Origem</th>
          <th>Destino</th>
          <th>Valor</th>
          <th>Telefone</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        </tbody>
    </table>
    <div class="total-a-receber">
      Total Geral a Receber: <span id="valorTotal">R$ 0,00</span>
    </div>
    <button type="button" onclick="exportDailyReportToPDF()">Exportar Relatório Diário para PDF</button>
  </div>

  <script>
    // --- Variáveis Globais e Inicialização ---
    let orders = JSON.parse(localStorage.getItem('transportOrders')) || [];
    let occupiedSeats = JSON.parse(localStorage.getItem('occupiedSeats')) || {};
    const TOTAL_SEATS = 20; // Total number of seats

    /**
     * Initializes the application state on page load.
     * Loads saved client name, renders seats, and refreshes all data displays.
     */
    window.onload = function () {
      loadClientName();
      renderSeatMap();
      refreshAllDataDisplays();
    };

    // --- Client Name Management Functions ---
    /**
     * Loads the saved client name from local storage into the input field.
     */
    function loadClientName() {
      const savedName = localStorage.getItem("clientName");
      if (savedName) {
        document.getElementById("cliente").value = savedName;
      }
    }

    /**
     * Saves the current client name from the input field to local storage.
     */
    function handleSaveClientName() {
      const clientName = document.getElementById("cliente").value.trim();
      if (clientName) {
        localStorage.setItem("clientName", clientName);
        alert("Nome gravado com sucesso!");
      } else {
        alert("Por favor, digite um nome para gravar.");
      }
    }

    /**
     * Removes the client name from local storage and clears the input field.
     */
    function handleRemoveClientName() {
      localStorage.removeItem("clientName");
      document.getElementById("cliente").value = "";
      alert("Nome removido com sucesso!");
    }

    // --- Form Submission Handling ---
    document.getElementById("pedidoForm").addEventListener("submit", function (e) {
      e.preventDefault(); // Prevent default form submission

      const form = e.target;
      const orderType = form.elements["tipo"].value;
      const clientName = form.elements["cliente"].value.trim();
      const origin = form.elements["origem"].value;
      const destination = form.elements["destino"].value;
      const value = parseFloat(form.elements["valor"].value);
      const phoneNumber = form.elements["telefone"].value.trim();

      // Basic validation
      if (!clientName || !origin || !destination || !phoneNumber) {
        alert("Por favor, preencha todos os campos obrigatórios.");
        return;
      }
      if (isNaN(value) || value < 0) {
        alert("Por favor, insira um valor válido.");
        return;
      }

      let assignedSeat = null;

      // Assign seat only for passengers
      if (orderType === "passageiro") {
        assignedSeat = assignNextAvailableSeat();
        if (assignedSeat === null) {
          alert("Todas as poltronas estão ocupadas! Não é possível adicionar mais passageiros.");
          return; // Stop if no seats are available
        }
      }

      const newOrder = {
        id: Date.now(), // Unique ID for each order
        client: clientName,
        type: orderType,
        origin: origin,
        destination: destination,
        value: value,
        phoneNumber: phoneNumber,
        seat: assignedSeat,
        date: new Date().toLocaleDateString('pt-BR') // Current date for daily reports
      };

      orders.push(newOrder); // Add new order to the global array
      saveData(); // Save all data to local storage
      refreshAllDataDisplays(); // Update all UI elements

      document.getElementById("mensagem").textContent = "Pedido registrado com sucesso!";
      // Reset form, but keep saved client name if present
      form.reset();
      loadClientName();
      document.getElementById("valor").value = "0.00"; // Reset value field
    });

    // --- Data Storage and Retrieval ---
    /**
     * Saves the current state of orders and occupied seats to local storage.
     */
    function saveData() {
      localStorage.setItem('transportOrders', JSON.stringify(orders));
      localStorage.setItem('occupiedSeats', JSON.stringify(occupiedSeats));
    }

    // --- UI Rendering Functions ---
    /**
     * Refreshes all data display sections (main table, scheduled passengers, all registered passengers).
     */
    function refreshAllDataDisplays() {
      renderMainOrdersTable();
      renderScheduledPassengersList();
      renderAllRegisteredPassengersList();
      renderSeatMap(); // Ensure seat map is always updated
    }

    /**
     * Renders the main table of daily orders (passengers and packages).
     */
    function renderMainOrdersTable() {
      const tableBody = document.querySelector("#tabelaPedidos tbody");
      tableBody.innerHTML = ''; // Clear table before re-rendering
      let totalAmountDue = 0;

      const currentDate = new Date().toLocaleDateString('pt-BR');
      const dailyOrders = orders.filter(order => order.date === currentDate);

      if (dailyOrders.length === 0) {
        const row = tableBody.insertRow();
        const cell = row.insertCell(0);
        cell.colSpan = 8; // Span across all columns
        cell.textContent = 'Nenhum pedido registrado para hoje.';
        cell.style.textAlign = 'center';
        cell.style.fontStyle = 'italic';
      } else {
        dailyOrders.forEach(order => {
          const row = tableBody.insertRow();
          row.insertCell(0).textContent = order.client;
          row.insertCell(1).textContent = order.type === 'passageiro' ? 'Passageiro' : 'Encomenda';
          row.insertCell(2).textContent = order.seat ? `Poltrona ${order.seat}` : 'N/A';
          row.insertCell(3).textContent = order.origin;
          row.insertCell(4).textContent = order.destination;
          row.insertCell(5).textContent = `R$ ${order.value.toFixed(2).replace('.', ',')}`;
          row.insertCell(6).textContent = order.phoneNumber;

          const actionsCell = row.insertCell(7);
          const removeButton = document.createElement('button');
          removeButton.textContent = 'Remover';
          removeButton.classList.add('acao-remover');
          removeButton.onclick = () => removeOrder(order.id); // Use unique ID for removal
          actionsCell.appendChild(removeButton);

          totalAmountDue += order.value;
        });
      }

      document.getElementById("valorTotal").textContent = `R$ ${totalAmountDue.toFixed(2).replace('.', ',')}`;
    }

    /**
     * Removes an order from the list by its unique ID and updates UI.
     * @param {number} orderId - The unique ID of the order to remove.
     */
    function removeOrder(orderId) {
      if (!confirm("Tem certeza que deseja remover este pedido?")) {
        return;
      }

      const indexToRemove = orders.findIndex(order => order.id === orderId);

      if (indexToRemove !== -1) {
        const removedOrder = orders[indexToRemove];

        // Free up the seat if it was a passenger order
        if (removedOrder.type === "passageiro" && removedOrder.seat) {
          delete occupiedSeats[removedOrder.seat];
        }

        orders.splice(indexToRemove, 1); // Remove the order from the array
        saveData(); // Save updated data
        refreshAllDataDisplays(); // Refresh all UI elements
      }
    }

    /**
     * Renders the visual seat map, showing available and occupied seats.
     */
    function renderSeatMap() {
      const seatMapDiv = document.getElementById('seatMap');
      seatMapDiv.innerHTML = ''; // Clear map before re-rendering

      for (let i = 1; i <= TOTAL_SEATS; i++) {
        const seatDiv = document.createElement('div');
        seatDiv.classList.add('seat');
        seatDiv.textContent = i;

        if (occupiedSeats[i]) {
          seatDiv.classList.add('occupied');
        } else {
          seatDiv.classList.add('available');
        }
        seatMapDiv.appendChild(seatDiv);
      }
    }

    /**
     * Assigns the next available seat number.
     * @returns {number|null} The assigned seat number, or null if no seats are available.
     */
    function assignNextAvailableSeat() {
      for (let i = 1; i <= TOTAL_SEATS; i++) {
        if (!occupiedSeats[i]) {
          occupiedSeats[i] = true; // Mark seat as occupied
          return i; // Return seat number
        }
      }
      return null; // No available seats
    }

    /**
     * Renders the list of scheduled passengers with assigned seats for the current day.
     */
    function renderScheduledPassengersList() {
      const scheduledList = document.getElementById('listaAgendamentos');
      scheduledList.innerHTML = ''; // Clear list before re-rendering

      const currentDate = new Date().toLocaleDateString('pt-BR');
      const dailyScheduledPassengers = orders.filter(order =>
        order.date === currentDate && order.type === 'passageiro' && order.seat
      ).sort((a, b) => a.seat - b.seat); // Sort by seat number

      if (dailyScheduledPassengers.length === 0) {
        const listItem = document.createElement('li');
        listItem.textContent = 'Nenhum passageiro agendado para hoje.';
        scheduledList.appendChild(listItem);
        return;
      }

      dailyScheduledPassengers.forEach(order => {
        const listItem = document.createElement('li');
        listItem.innerHTML = `
          <strong>${order.client}</strong>
          <span class="info-passageiro">
            Poltrona: ${order.seat} | Origem: ${order.origin} | Destino: ${order.destination}
          </span>
          <span class="valor-passageiro">R$ ${order.value.toFixed(2).replace('.', ',')}</span>
        `;
        scheduledList.appendChild(listItem);
      });
    }

    /**
     * Renders a list of all unique registered passenger names.
     */
    function renderAllRegisteredPassengersList() {
      const allPassengersList = document.getElementById('listaTodosPassageiros');
      allPassengersList.innerHTML = ''; // Clear list before re-rendering

      const allRegisteredPassengers = orders.filter(order => order.type === 'passageiro');

      if (allRegisteredPassengers.length === 0) {
        const listItem = document.createElement('li');
        listItem.textContent = 'Nenhum passageiro cadastrado ainda.';
        allPassengersList.appendChild(listItem);
        return;
      }

      // Get unique client names and sort them alphabetically
      const uniqueNames = [...new Set(allRegisteredPassengers.map(order => order.client))].sort();

      uniqueNames.forEach(name => {
        const listItem = document.createElement('li');
        listItem.textContent = name;
        allPassengersList.appendChild(listItem);
      });
    }

    // --- PDF Export Function ---
    /**
     * Exports a daily report of orders to a PDF document.
     */
    async function exportDailyReportToPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();

      const currentDate = new Date().toLocaleDateString('pt-BR');
      const driverName = document.querySelector('.motorista-info').textContent.replace('Motorista: ', '');

      doc.setFontSize(18);
      doc.text(`Relatório Diário de Viagens`, 105, 20, null, null, 'center');
      doc.setFontSize(12);
      doc.text(`Data: ${currentDate}`, 10, 30);
      doc.text(`Motorista: ${driverName}`, 10, 37);
      doc.text(`Total Geral a Receber: ${document.getElementById("valorTotal").textContent}`, 10, 44);

      const dailyOrdersForPDF = orders.filter(order => order.date === currentDate);

      if (dailyOrdersForPDF.length === 0) {
        alert("Não há pedidos para exportar no dia de hoje.");
        return;
      }

      const tableColumn = ["Cliente", "Tipo", "Poltrona", "Origem", "Destino", "Valor", "Telefone"];
      const tableRows = dailyOrdersForPDF.map(order => [
        order.client,
        order.type === 'passageiro' ? 'Passageiro' : 'Encomenda',
        order.seat ? `Poltrona ${order.seat}` : 'N/A',
        order.origin,
        order.destination,
        `R$ ${order.value.toFixed(2).replace('.', ',')}`,
        order.phoneNumber
      ]);

      doc.autoTable({
        head: [tableColumn],
        body: tableRows,
        startY: 55,
        styles: {
          fontSize: 10,
          cellPadding: 3,
          halign: 'left'
        },
        headStyles: {
          fillColor: [255, 107, 0],
          textColor: [255, 255, 255],
          fontStyle: 'bold'
        },
        alternateRowStyles: {
          fillColor: [240, 240, 240]
        },
        margin: { top: 10, bottom: 10, left: 10, right: 10 }
      });

      doc.save(`relatorio_viagens_${currentDate.replace(/\//g, '-')}.pdf`);
    }

    // --- Event Listener for Service Type Change ---
    document.getElementById('tipo').addEventListener('change', function() {
      const valueInput = document.getElementById('valor');
      if (this.value === 'encomenda') {
        valueInput.value = '0.00';
      } else {
        valueInput.value = ''; // Clear for user to input passenger value
      }
    });
  </script>
</body>
</html>
