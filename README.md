<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Factura 3DCraftRD</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-firestore-compat.js"></script>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg, #1a202c, #2d3748);
      color: white;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
      padding: 40px 20px;
    }

    .container {
      max-width: 900px;
      width: 100%;
      background-color: #2d3748;
      border-radius: 12px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
      padding: 30px;
      border: 1px solid #4a5568;
    }

    .header {
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 30px;
      padding-bottom: 20px;
      border-bottom: 2px solid #ff9f43;
      gap: 20px;
    }

    .logo-container {
      display: flex;
      align-items: center;
      gap: 15px;
    }

    .header img {
      width: 60px;
      height: auto;
    }

    .brand-name {
      font-size: 2.5rem;
      font-weight: bold;
      color: #ff9f43;
      text-shadow: 0 2px 4px rgba(0,0,0,0.3);
    }

    h1 {
      margin: 0;
      font-size: 2.2rem;
      color: white;
      text-align: center;
    }

    form input[type="number"],
    form input[type="text"],
    form select {
      width: 100%;
      padding: 12px 15px;
      border: 1px solid #4a5568;
      border-radius: 8px;
      margin-top: 5px;
      background-color: #3c4758;
      color: white;
      font-size: 1rem;
      transition: all 0.3s ease;
    }

    form input[type="number"]:focus,
    form input[type="text"]:focus,
    form select:focus {
      outline: none;
      border-color: #ff9f43;
      box-shadow: 0 0 0 3px rgba(255, 159, 67, 0.2);
    }

    label {
      display: block;
      margin-top: 18px;
      font-weight: 600;
      color: #e2e8f0;
    }

    .results {
      margin-top: 30px;
      background-color: #3c4758;
      padding: 25px;
      border-radius: 10px;
      box-shadow: inset 0 2px 5px rgba(0,0,0,0.2);
    }

    .results div {
      margin-bottom: 12px;
      display: flex;
      justify-content: space-between;
      padding: 8px 0;
      border-bottom: 1px solid #4a5568;
    }

    .results h3 {
      margin-top: 0;
      padding-bottom: 15px;
      margin-bottom: 15px;
      border-bottom: 2px solid #ff9f43;
      color: #ff9f43;
      font-size: 1.4rem;
    }

    .customer-results {
      background: linear-gradient(to right, #2c5282, #2a4365);
      border-left: 5px solid #ff9f43;
    }

    .customer-results div:last-child {
      border-bottom: none;
      font-weight: bold;
      font-size: 1.2rem;
      padding-top: 15px;
      color: #ff9f43;
    }

    .btn-export,
    .btn-clear,
    .btn-save,
    .btn-lang,
    .btn-dashboard,
    .btn-template {
      display: inline-block;
      margin-top: 10px;
      margin-right: 10px;
      padding: 12px 20px;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    }

    .btn-export {
      background: linear-gradient(to right, #4CAF50, #2E7D32);
      color: white;
    }

    .btn-save {
      background: linear-gradient(to right, #2196F3, #0D47A1);
      color: white;
    }

    .btn-clear {
      background: linear-gradient(to right, #f44336, #b71c1c);
      color: white;
    }

    .btn-lang {
      background: linear-gradient(to right, #9C27B0, #6A1B9A);
      color: white;
    }

    .btn-dashboard {
      background: linear-gradient(to right, #FF9800, #F57C00);
      color: white;
    }

    .btn-template {
      background: linear-gradient(to right, #607D8B, #455A64);
      color: white;
    }

    .btn-export:hover,
    .btn-save:hover,
    .btn-clear:hover,
    .btn-lang:hover,
    .btn-dashboard:hover,
    .btn-template:hover {
      transform: translateY(-3px);
      box-shadow: 0 6px 8px rgba(0, 0, 0, 0.3);
    }

    .history {
      margin-top: 30px;
      background-color: #3c4758;
      padding: 20px;
      border-radius: 10px;
      box-shadow: inset 0 2px 5px rgba(0,0,0,0.2);
    }

    .history-item {
      margin-bottom: 12px;
      padding: 15px;
      border-radius: 8px;
      background-color: #2d3748;
      transition: all 0.3s ease;
      cursor: pointer;
    }

    .history-item:hover {
      background-color: #4a5568;
      transform: translateX(5px);
    }
    
    .invoice-header {
      display: flex;
      justify-content: space-between;
      margin-bottom: 20px;
      flex-wrap: wrap;
      gap: 20px;
      padding: 20px;
      background-color: #3c4758;
      border-radius: 10px;
    }
    
    .invoice-header div {
      flex: 1;
      min-width: 250px;
    }
    
    .section-title {
      color: #ff9f43;
      margin-top: 25px;
      margin-bottom: 10px;
      font-size: 1.3rem;
      padding-left: 10px;
      border-left: 4px solid #ff9f43;
    }

    .buttons-container {
      display: flex;
      gap: 15px;
      flex-wrap: wrap;
      margin-top: 30px;
      justify-content: center;
    }

    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0,0,0,0.7);
      overflow: auto;
    }

    .modal-content {
      background-color: #2d3748;
      margin: 5% auto;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.5);
      width: 80%;
      max-width: 900px;
      border: 1px solid #4a5568;
      position: relative;
    }

    .close {
      position: absolute;
      right: 25px;
      top: 15px;
      color: #aaa;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }

    .close:hover {
      color: white;
    }

    .chart-container {
      margin-top: 20px;
      height: 400px;
      background-color: #3c4758;
      border-radius: 8px;
      padding: 15px;
    }

    .tabs {
      display: flex;
      margin-bottom: 20px;
      border-bottom: 2px solid #4a5568;
    }

    .tab {
      padding: 10px 20px;
      cursor: pointer;
      background-color: #3c4758;
      border: none;
      color: white;
      font-weight: 600;
      transition: all 0.3s ease;
    }

    .tab.active {
      background-color: #4a5568;
      color: #ff9f43;
      border-bottom: 3px solid #ff9f43;
    }

    .tab-content {
      display: none;
    }

    .tab-content.active {
      display: block;
    }

    .customer-card {
      background-color: #3c4758;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 15px;
      transition: all 0.3s ease;
    }

    .customer-card:hover {
      background-color: #4a5568;
      transform: translateY(-3px);
    }

    .template-card {
      background-color: #3c4758;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 15px;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .template-card:hover {
      background-color: #4a5568;
      transform: translateY(-3px);
    }

    .search-box {
      width: 100%;
      padding: 12px 15px;
      border: 1px solid #4a5568;
      border-radius: 8px;
      margin-bottom: 20px;
      background-color: #3c4758;
      color: white;
      font-size: 1rem;
    }

    @media (max-width: 600px) {
      .header {
        flex-direction: column;
        text-align: center;
      }
      
      .logo-container {
        justify-content: center;
      }
      
      .invoice-header {
        flex-direction: column;
      }
      
      .buttons-container {
        flex-direction: column;
      }
      
      .modal-content {
        width: 95%;
        margin: 10% auto;
        padding: 15px;
      }
    }
  </style>
</head>
<body>

<div class="container">
  <div class="header">
    <div class="logo-container">
      <img src="logo.png" alt="Logo 3DCraftRD" />
      <div class="brand-name">3DCraftRD</div>
    </div>
    <h1 id="app-title">Factura</h1>
  </div>

  <div style="text-align:right; margin-bottom:15px;">
    <button class="btn-lang" onclick="toggleLang()" id="btn-lang">Toggle Language</button>
    <button class="btn-dashboard" onclick="openDashboard()" id="btn-dashboard">Dashboard</button>
  </div>

  <div class="invoice-header">
    <div>
      <label for="customerName" id="lbl-customer-name">Nombre del Cliente</label>
      <input type="text" id="customerName" placeholder="Ingrese el nombre del cliente" list="customersList" />
      <datalist id="customersList"></datalist>
    </div>
    <div>
      <label for="invoiceNumber" id="lbl-invoice-number">Número de Factura</label>
      <input type="text" id="invoiceNumber" readonly />
    </div>
  </div>

  <form id="calculator-form">
    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
      <div>
        <label for="filamentType" id="lbl-filament-type">Tipo de Filamento</label>
        <select id="filamentType">
          <option value="1600">PLA (RD$1,600/kg)</option>
          <option value="2000">PETG (RD$2,000/kg)</option>
          <option value="2250">ABS (RD$2,250/kg)</option>
          <option value="2150">ASA (RD$2,150/kg)</option>
          <option value="custom">Personalizado</option>
        </select>

        <label for="filamentPrice" id="lbl-filament-price">Precio del filamento (RD$/kg)</label>
        <input type="number" id="filamentPrice" value="1600" />

        <label for="weight" id="lbl-weight">Peso del modelo (g)</label>
        <input type="number" id="weight" value="150" />

        <label for="electricityPrice" id="lbl-electricity-price">Precio de la electricidad (RD$/kWh)</label>
        <input type="number" step="0.01" id="electricityPrice" value="2.80" />

        <label for="printHours" id="lbl-print-hours">Horas de impresión</label>
        <input type="number" id="printHours" value="6" />
      </div>
      
      <div>
        <label for="laborCost" id="lbl-labor-cost">Mano de obra (RD$)</label>
        <input type="number" id="laborCost" value="150" />

        <label for="otherCosts" id="lbl-other-costs">Otros costos (RD$)</label>
        <input type="number" id="otherCosts" value="100" />

        <label for="maintenanceCost" id="lbl-maintenance-cost">Costo de mantenimiento (RD$)</label>
        <input type="number" id="maintenanceCost" value="50" />

        <label for="printerDepreciation" id="lbl-printer-depreciation">Depreciación de impresora (RD$/hora)</label>
        <input type="number" step="0.01" id="printerDepreciation" value="14.00" />

        <label for="margin" id="lbl-margin">Margen de ganancia (%)</label>
        <input type="number" id="margin" value="40" />

        <label for="exchangeRate" id="lbl-exchange-rate">Tasa de cambio USD a RD$</label>
        <input type="number" id="exchangeRate" value="58.00" />
      </div>
    </div>
  </form>

  <div class="section-title" id="lbl-admin-results">Resultados (Administrador)</div>
  <div class="results">
    <div><span id="lbl-material-cost">Costo del material:</span> <span>RD$<span id="materialCost">0.00</span></span></div>
    <div><span id="lbl-energy-cost">Costo de energía eléctrica:</span> <span>RD$<span id="energyCost">0.00</span></span></div>
    <div><span id="lbl-printing-cost">Costo de impresión:</span> <span>RD$<span id="printingCost">0.00</span></span></div>
    <div><span id="lbl-maintenance-cost-admin">Costo de mantenimiento:</span> <span>RD$<span id="maintenanceCostAdmin">0.00</span></span></div>
    <div><span id="lbl-labor-cost-admin">Costo de mano de obra:</span> <span>RD$<span id="laborCostAdmin">0.00</span></span></div>
    <div><span id="lbl-other-costs-admin">Otros costos:</span> <span>RD$<span id="otherCostsAdmin">0.00</span></span></div>
    <div><span id="lbl-total-cost">Costo total:</span> <span>RD$<span id="totalCost">0.00</span></span></div>
    <div><span id="lbl-suggested-price">Precio sugerido:</span> <span>RD$<span id="suggestedPrice">0.00</span></span></div>
    <div><span id="lbl-net-profit">Ganancia neta:</span> <span>RD$<span id="netProfit">0.00</span></span></div>
  </div>

  <div class="section-title" id="lbl-customer-results">Detalles de la Factura (Cliente)</div>
  <div class="results customer-results">
    <div><span id="lbl-customer-material-cost">Materiales utilizados:</span> <span>RD$<span id="customerMaterialCost">0.00</span></span></div>
    <div><span id="lbl-customer-energy-cost">Consumo energético:</span> <span>RD$<span id="customerEnergyCost">0.00</span></span></div>
    <div><span id="lbl-customer-printing-cost">Tiempo de máquina:</span> <span>RD$<span id="customerPrintingCost">0.00</span></span></div>
    <div><span id="lbl-customer-service-cost">Servicio técnico:</span> <span>RD$<span id="customerServiceCost">0.00</span></span></div>
    <div><span id="lbl-customer-total-price">Precio Total:</span> <span>RD$<span id="customerTotalPrice">0.00</span></span></div>
  </div>

  <div class="buttons-container">
    <button class="btn-export" onclick="exportToPDF()" id="btn-export">Exportar a PDF</button>
    <button class="btn-save" onclick="saveToHistory()" id="btn-save">Guardar en Historial</button>
    <button class="btn-clear" onclick="clearForm()" id="btn-clear">Limpiar Formulario</button>
    <button class="btn-template" onclick="saveTemplate()" id="btn-template">Guardar Plantilla</button>
  </div>

  <div class="history" id="history-section">
    <h3 id="lbl-history">Historial de Impresiones</h3>
    <div id="history-list"></div>
  </div>
</div>

<!-- Modal Dashboard -->
<div id="dashboardModal" class="modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('dashboardModal')">&times;</span>
    <h2 id="lbl-dashboard-title">Dashboard Analytics</h2>
    
    <div class="tabs">
      <button class="tab active" onclick="openTab(event, 'stats-tab')" id="tab-stats">Estadísticas</button>
      <button class="tab" onclick="openTab(event, 'customers-tab')" id="tab-customers">Clientes</button>
      <button class="tab" onclick="openTab(event, 'templates-tab')" id="tab-templates">Plantillas</button>
    </div>
    
    <div id="stats-tab" class="tab-content active">
      <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
        <div>
          <h3 id="lbl-monthly-profit">Ganancias Mensuales</h3>
          <div class="chart-container">
            <canvas id="monthlyProfitChart"></canvas>
          </div>
        </div>
        <div>
          <h3 id="lbl-filament-usage">Uso de Filamento</h3>
          <div class="chart-container">
            <canvas id="filamentUsageChart"></canvas>
          </div>
        </div>
      </div>
      <div style="margin-top: 20px;">
        <h3 id="lbl-recent-activity">Actividad Reciente</h3>
        <div id="recent-activity" style="max-height: 300px; overflow-y: auto;"></div>
      </div>
    </div>
    
    <div id="customers-tab" class="tab-content">
      <input type="text" id="customerSearch" class="search-box" placeholder="Buscar clientes..." oninput="searchCustomers()" />
      <div id="customers-list"></div>
    </div>
    
    <div id="templates-tab" class="tab-content">
      <input type="text" id="templateSearch" class="search-box" placeholder="Buscar plantillas..." oninput="searchTemplates()" />
      <div id="templates-list"></div>
    </div>
  </div>
</div>

<script>
  // Configuración de Firebase
  const firebaseConfig = {
    apiKey: "AIzaSyDh3ERqBfRMjGliGuKxL16wPoGjARQ5L0Q",
    authDomain: "facturas-fa510.firebaseapp.com",
    databaseURL: "https://facturas-fa510-default-rtdb.firebaseio.com",
    projectId: "facturas-fa510",
    storageBucket: "facturas-fa510.appspot.com",
    messagingSenderId: "1045051500553",
    appId: "1:1045051500553:web:180cfd9978cf1f2d2c30d8",
    measurementId: "G-58CQ3L3XNN"
  };

  // Inicializar Firebase
  firebase.initializeApp(firebaseConfig);
  const db = firebase.firestore();

  // Generar número de factura único
  function generateInvoiceNumber() {
    const now = new Date();
    const year = now.getFullYear().toString().slice(-2);
    const month = (now.getMonth() + 1).toString().padStart(2, '0');
    const day = now.getDate().toString().padStart(2, '0');
    const hours = now.getHours().toString().padStart(2, '0');
    const minutes = now.getMinutes().toString().padStart(2, '0');
    const seconds = now.getSeconds().toString().padStart(2, '0');
    return `FAC-${year}${month}${day}-${hours}${minutes}${seconds}`;
  }

  // Inicializar número de factura
  document.getElementById('invoiceNumber').value = generateInvoiceNumber();

  // Traducciones
  const translations = {
    es: {
      "app-title": "Factura",
      "lbl-customer-name": "Nombre del Cliente",
      "lbl-invoice-number": "Número de Factura",
      "lbl-electricity-price": "Precio de la electricidad (RD$/kWh)",
      "lbl-print-hours": "Horas de impresión",
      "lbl-weight": "Peso del modelo (g)",
      "lbl-filament-price": "Precio del filamento (RD$/kg)",
      "lbl-filament-type": "Tipo de Filamento",
      "lbl-labor-cost": "Mano de obra (RD$)",
      "lbl-other-costs": "Otros costos (RD$)",
      "lbl-maintenance-cost": "Costo de mantenimiento (RD$)",
      "lbl-printer-depreciation": "Depreciación de impresora (RD$/hora)",
      "lbl-margin": "Margen de ganancia (%)",
      "lbl-exchange-rate": "Tasa de cambio USD a RD$",
      "lbl-material-cost": "Costo del material:",
      "lbl-energy-cost": "Costo de energía eléctrica:",
      "lbl-printing-cost": "Costo de impresión:",
      "lbl-maintenance-cost-admin": "Costo de mantenimiento:",
      "lbl-labor-cost-admin": "Costo de mano de obra:",
      "lbl-other-costs-admin": "Otros costos:",
      "lbl-total-cost": "Costo total:",
      "lbl-suggested-price": "Precio sugerido:",
      "lbl-net-profit": "Ganancia neta:",
      "lbl-customer-material-cost": "Materiales utilizados:",
      "lbl-customer-energy-cost": "Consumo energético:",
      "lbl-customer-printing-cost": "Tiempo de máquina:",
      "lbl-customer-service-cost": "Servicio técnico:",
      "lbl-customer-total-price": "Precio Total:",
      "btn-export": "Exportar a PDF",
      "btn-save": "Guardar en Historial",
      "btn-clear": "Limpiar Formulario",
      "btn-lang": "Cambiar Idioma",
      "btn-dashboard": "Dashboard",
      "btn-template": "Guardar Plantilla",
      "lbl-history": "Historial de Impresiones",
      "lbl-admin-results": "Resultados (Administrador)",
      "lbl-customer-results": "Detalles de la Factura (Cliente)",
      "lbl-dashboard-title": "Dashboard Analytics",
      "lbl-monthly-profit": "Ganancias Mensuales",
      "lbl-filament-usage": "Uso de Filamento",
      "lbl-recent-activity": "Actividad Reciente",
      "tab-stats": "Estadísticas",
      "tab-customers": "Clientes",
      "tab-templates": "Plantillas",
      "customer-search-placeholder": "Buscar clientes...",
      "template-search-placeholder": "Buscar plantillas..."
    },
    en: {
      "app-title": "Invoice",
      "lbl-customer-name": "Customer Name",
      "lbl-invoice-number": "Invoice Number",
      "lbl-electricity-price": "Electricity Price (RD$/kWh)",
      "lbl-print-hours": "Print Hours",
      "lbl-weight": "Model Weight (g)",
      "lbl-filament-price": "Filament Price (RD$/kg)",
      "lbl-filament-type": "Filament Type",
      "lbl-labor-cost": "Labor Cost (RD$)",
      "lbl-other-costs": "Other Costs (RD$)",
      "lbl-maintenance-cost": "Maintenance Cost (RD$)",
      "lbl-printer-depreciation": "Printer Depreciation (RD$/hour)",
      "lbl-margin": "Profit Margin (%)",
      "lbl-exchange-rate": "Exchange Rate USD to RD$",
      "lbl-material-cost": "Material Cost:",
      "lbl-energy-cost": "Electricity Cost:",
      "lbl-printing-cost": "Printing Cost:",
      "lbl-maintenance-cost-admin": "Maintenance Cost:",
      "lbl-labor-cost-admin": "Labor Cost:",
      "lbl-other-costs-admin": "Other Costs:",
      "lbl-total-cost": "Total Cost:",
      "lbl-suggested-price": "Suggested Price:",
      "lbl-net-profit": "Net Profit:",
      "lbl-customer-material-cost": "Materials used:",
      "lbl-customer-energy-cost": "Energy consumption:",
      "lbl-customer-printing-cost": "Machine time:",
      "lbl-customer-service-cost": "Technical service:",
      "lbl-customer-total-price": "Total Price:",
      "btn-export": "Export to PDF",
      "btn-save": "Save to History",
      "btn-clear": "Clear Form",
      "btn-lang": "Toggle Language",
      "btn-dashboard": "Dashboard",
      "btn-template": "Save Template",
      "lbl-history": "Print History",
      "lbl-admin-results": "Results (Admin)",
      "lbl-customer-results": "Invoice Details (Customer)",
      "lbl-dashboard-title": "Dashboard Analytics",
      "lbl-monthly-profit": "Monthly Profits",
      "lbl-filament-usage": "Filament Usage",
      "lbl-recent-activity": "Recent Activity",
      "tab-stats": "Statistics",
      "tab-customers": "Customers",
      "tab-templates": "Templates",
      "customer-search-placeholder": "Search customers...",
      "template-search-placeholder": "Search templates..."
    }
  };

  let currentLang = navigator.language.startsWith('es') ? 'es' : 'en';
  let monthlyProfitChart = null;
  let filamentUsageChart = null;

  function translatePage() {
    Object.keys(translations[currentLang]).forEach(key => {
      const element = document.getElementById(key);
      if (element) {
        element.innerText = translations[currentLang][key];
      }
      
      // Actualizar placeholders
      if (key === "customer-search-placeholder") {
        document.getElementById("customerSearch").placeholder = translations[currentLang][key];
      }
      if (key === "template-search-placeholder") {
        document.getElementById("templateSearch").placeholder = translations[currentLang][key];
      }
    });
  }

  function toggleLang() {
    currentLang = currentLang === 'es' ? 'en' : 'es';
    translatePage();
    calculateResults();
  }

  // Cálculos
  const results = {
    materialCost: document.getElementById("materialCost"),
    energyCost: document.getElementById("energyCost"),
    printingCost: document.getElementById("printingCost"),
    maintenanceCostAdmin: document.getElementById("maintenanceCostAdmin"),
    laborCostAdmin: document.getElementById("laborCostAdmin"),
    otherCostsAdmin: document.getElementById("otherCostsAdmin"),
    totalCost: document.getElementById("totalCost"),
    suggestedPrice: document.getElementById("suggestedPrice"),
    netProfit: document.getElementById("netProfit"),
    customerMaterialCost: document.getElementById("customerMaterialCost"),
    customerEnergyCost: document.getElementById("customerEnergyCost"),
    customerPrintingCost: document.getElementById("customerPrintingCost"),
    customerServiceCost: document.getElementById("customerServiceCost"),
    customerTotalPrice: document.getElementById("customerTotalPrice")
  };

  function calculateResults() {
    const electricityPrice = parseFloat(document.getElementById("electricityPrice").value) || 0;
    const printHours = parseFloat(document.getElementById("printHours").value) || 0;
    const weight = parseFloat(document.getElementById("weight").value) || 0;
    const filamentPrice = parseFloat(document.getElementById("filamentPrice").value) || 0;
    const laborCost = parseFloat(document.getElementById("laborCost").value) || 0;
    const otherCosts = parseFloat(document.getElementById("otherCosts").value) || 0;
    const maintenanceCost = parseFloat(document.getElementById("maintenanceCost").value) || 0;
    const printerDepreciation = parseFloat(document.getElementById("printerDepreciation").value) || 0;
    const margin = parseFloat(document.getElementById("margin").value) || 0;

    const printerPowerKw = 0.15; // kW

    const energyConsumption = printerPowerKw * printHours;
    const energyCost = energyConsumption * electricityPrice;
    const printingCost = (electricityPrice * printerPowerKw + printerDepreciation) * printHours;
    const materialCost = (weight / 1000) * filamentPrice;
    const totalCost = materialCost + energyCost + printingCost + laborCost + otherCosts + maintenanceCost;
    const suggestedPrice = totalCost * (1 + margin / 100);
    const netProfit = suggestedPrice - totalCost;

    // Resultados para administrador
    results.materialCost.textContent = materialCost.toFixed(2);
    results.energyCost.textContent = energyCost.toFixed(2);
    results.printingCost.textContent = printingCost.toFixed(2);
    results.maintenanceCostAdmin.textContent = maintenanceCost.toFixed(2);
    results.laborCostAdmin.textContent = laborCost.toFixed(2);
    results.otherCostsAdmin.textContent = otherCosts.toFixed(2);
    results.totalCost.textContent = totalCost.toFixed(2);
    results.suggestedPrice.textContent = suggestedPrice.toFixed(2);
    results.netProfit.textContent = netProfit.toFixed(2);

    // Resultados para cliente (ajustados para que sumen exactamente el precio total)
    const serviceCost = laborCost + otherCosts + maintenanceCost;
    const adjustedPrintingCost = printingCost * (suggestedPrice / totalCost);
    const adjustedMaterialCost = materialCost * (suggestedPrice / totalCost);
    const adjustedEnergyCost = energyCost * (suggestedPrice / totalCost);
    const adjustedServiceCost = serviceCost * (suggestedPrice / totalCost);
    
    results.customerMaterialCost.textContent = adjustedMaterialCost.toFixed(2);
    results.customerEnergyCost.textContent = adjustedEnergyCost.toFixed(2);
    results.customerPrintingCost.textContent = adjustedPrintingCost.toFixed(2);
    results.customerServiceCost.textContent = adjustedServiceCost.toFixed(2);
    results.customerTotalPrice.textContent = suggestedPrice.toFixed(2);
  }

  function exportToPDF() {
    // Verificar si jsPDF está cargado
    if (typeof window.jspdf === 'undefined') {
      alert(currentLang === 'es' ? 'Error: La biblioteca jsPDF no está cargada. Por favor recarga la página.' : 'Error: jsPDF library not loaded. Please reload the page.');
      return;
    }
    
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    
    const customerName = document.getElementById("customerName").value || (currentLang === 'es' ? "Cliente no especificado" : "Unspecified customer");
    const invoiceNumber = document.getElementById("invoiceNumber").value || "FAC-00000";
    const lang = translations[currentLang];
    
    // Encabezado con logo y nombre
    doc.setFontSize(18);
    doc.setFont(undefined, 'bold');
    
    // Añadir logo (si está disponible)
    try {
      doc.addImage("logo.png", "PNG", 20, 15, 25, 25);
    } catch (e) {
      console.log("Logo no encontrado, continuando sin él");
    }
    
    doc.text("3DCraftRD", 105, 20, null, null, "center");
    doc.setFontSize(14);
    doc.text(lang["app-title"], 105, 27, null, null, "center");
    
    // Información de la factura
    doc.setFontSize(12);
    doc.text(`${lang["lbl-invoice-number"]}: ${invoiceNumber}`, 20, 45);
    doc.text(`${lang["lbl-customer-name"]}: ${customerName}`, 20, 52);
    
    // Fecha
    const now = new Date();
    const dateStr = now.toLocaleDateString();
    doc.text(`${currentLang === 'es' ? 'Fecha' : 'Date'}: ${dateStr}`, 180, 45, null, null, "right");
    
    // Resultados para el cliente
    doc.setFontSize(16);
    doc.setFont(undefined, 'bold');
    doc.text(lang["lbl-customer-results"], 20, 70);
    
    doc.setFontSize(12);
    doc.setFont(undefined, 'normal');
    
    let yPos = 80;
    doc.text(`${lang["lbl-customer-material-cost"]}: RD$${results.customerMaterialCost.textContent}`, 30, yPos);
    yPos += 10;
    doc.text(`${lang["lbl-customer-energy-cost"]}: RD$${results.customerEnergyCost.textContent}`, 30, yPos);
    yPos += 10;
    doc.text(`${lang["lbl-customer-printing-cost"]}: RD$${results.customerPrintingCost.textContent}`, 30, yPos);
    yPos += 10;
    doc.text(`${lang["lbl-customer-service-cost"]}: RD$${results.customerServiceCost.textContent}`, 30, yPos);
    yPos += 15;
    
    // Precio total
    doc.setFontSize(16);
    doc.setFont(undefined, 'bold');
    doc.text(`${lang["lbl-customer-total-price"]}: RD$${results.customerTotalPrice.textContent}`, 30, yPos);
    
    // Nota al pie
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    doc.text(currentLang === 'es' ? "Gracias por su preferencia - 3DCraftRD" : "Thank you for your business - 3DCraftRD", 105, 280, null, null, "center");
    
    // Guardar el PDF
    doc.save(`${currentLang === 'es' ? 'Factura' : 'Invoice'}_${invoiceNumber}.pdf`);
  }

  async function saveToHistory() {
    const customerName = document.getElementById("customerName").value || (currentLang === 'es' ? "Sin nombre" : "No name");
    const invoiceNumber = document.getElementById("invoiceNumber").value || generateInvoiceNumber();
    
    // Guardar en Firestore
    try {
      const invoiceData = {
        date: new Date().toISOString(),
        customerName: customerName,
        invoiceNumber: invoiceNumber,
        electricityPrice: parseFloat(document.getElementById("electricityPrice").value) || 0,
        printHours: parseFloat(document.getElementById("printHours").value) || 0,
        weight: parseFloat(document.getElementById("weight").value) || 0,
        filamentPrice: parseFloat(document.getElementById("filamentPrice").value) || 0,
        filamentType: document.getElementById("filamentType").value,
        laborCost: parseFloat(document.getElementById("laborCost").value) || 0,
        otherCosts: parseFloat(document.getElementById("otherCosts").value) || 0,
        maintenanceCost: parseFloat(document.getElementById("maintenanceCost").value) || 0,
        printerDepreciation: parseFloat(document.getElementById("printerDepreciation").value) || 0,
        margin: parseFloat(document.getElementById("margin").value) || 0,
        totalCost: parseFloat(results.totalCost.textContent) || 0,
        suggestedPrice: parseFloat(results.suggestedPrice.textContent) || 0,
        exchangeRate: parseFloat(document.getElementById("exchangeRate").value) || 0
      };
      
      // Guardar factura
      await db.collection("invoices").add(invoiceData);
      
      // Actualizar o crear cliente
      const customerQuery = await db.collection("customers").where("name", "==", customerName).get();
      
      if (customerQuery.empty) {
        // Nuevo cliente
        await db.collection("customers").add({
          name: customerName,
          totalSpent: parseFloat(results.suggestedPrice.textContent) || 0,
          invoices: [invoiceNumber],
          lastPurchase: new Date().toISOString(),
          firstPurchase: new Date().toISOString()
        });
      } else {
        // Cliente existente
        const customerDoc = customerQuery.docs[0];
        await db.collection("customers").doc(customerDoc.id).update({
          totalSpent: customerDoc.data().totalSpent + parseFloat(results.suggestedPrice.textContent) || 0,
          invoices: [...customerDoc.data().invoices, invoiceNumber],
          lastPurchase: new Date().toISOString()
        });
      }
      
      // Mostrar notificación
      alert(currentLang === 'es' ? 'Factura guardada con éxito!' : 'Invoice saved successfully!');
      
      // Generar nuevo número de factura para el próximo
      document.getElementById('invoiceNumber').value = generateInvoiceNumber();
      
      // Actualizar historial
      renderHistory();
    } catch (error) {
      console.error("Error saving invoice:", error);
      alert(currentLang === 'es' ? 'Error al guardar la factura' : 'Error saving invoice');
    }
  }

  async function renderHistory() {
    const list = document.getElementById("history-list");
    list.innerHTML = "";
    
    try {
      const querySnapshot = await db.collection("invoices")
        .orderBy("date", "desc")
        .limit(10)
        .get();
      
      if (querySnapshot.empty) {
        list.innerHTML = `<div style="text-align:center; padding:20px; color:#a0aec0;">${currentLang === 'es' ? 'No hay registros en el historial' : 'No records in history'}</div>`;
        return;
      }
      
      querySnapshot.forEach(doc => {
        const item = doc.data();
        const div = document.createElement("div");
        div.className = "history-item";
        div.innerHTML = `
          <strong>${new Date(item.date).toLocaleString()}</strong><br>
          <div style="display:flex; justify-content:space-between; margin-top:8px;">
            <div>
              <strong>${currentLang === 'es' ? 'Cliente' : 'Customer'}:</strong> ${item.customerName}<br>
              <strong>${currentLang === 'es' ? 'Factura' : 'Invoice'}:</strong> ${item.invoiceNumber}
            </div>
            <div style="text-align:right;">
              <strong>${currentLang === 'es' ? 'Total' : 'Total'}:</strong> RD$${item.suggestedPrice.toFixed(2)}
            </div>
          </div>
        `;
        div.addEventListener("click", () => loadInvoiceFromHistory(doc.id, item));
        list.appendChild(div);
      });
    } catch (error) {
      console.error("Error loading history:", error);
      list.innerHTML = `<div style="text-align:center; padding:20px; color:#a0aec0;">${currentLang === 'es' ? 'Error al cargar el historial' : 'Error loading history'}</div>`;
    }
  }

  function loadInvoiceFromHistory(docId, data) {
    document.getElementById("customerName").value = data.customerName;
    document.getElementById("electricityPrice").value = data.electricityPrice;
    document.getElementById("printHours").value = data.printHours;
    document.getElementById("weight").value = data.weight;
    document.getElementById("filamentPrice").value = data.filamentPrice;
    document.getElementById("filamentType").value = data.filamentType || "1600";
    document.getElementById("laborCost").value = data.laborCost;
    document.getElementById("otherCosts").value = data.otherCosts;
    document.getElementById("maintenanceCost").value = data.maintenanceCost || 0;
    document.getElementById("printerDepreciation").value = data.printerDepreciation || 14.00;
    document.getElementById("margin").value = data.margin;
    document.getElementById("exchangeRate").value = data.exchangeRate || 58.00;
    document.getElementById("invoiceNumber").value = data.invoiceNumber;
    
    calculateResults();
  }

  function clearForm() {
    document.getElementById("customerName").value = "";
    document.getElementById("electricityPrice").value = "2.80";
    document.getElementById("printHours").value = "6";
    document.getElementById("weight").value = "150";
    document.getElementById("filamentPrice").value = "1600";
    document.getElementById("filamentType").value = "1600";
    document.getElementById("laborCost").value = "150";
    document.getElementById("otherCosts").value = "100";
    document.getElementById("maintenanceCost").value = "50";
    document.getElementById("printerDepreciation").value = "14.00";
    document.getElementById("margin").value = "40";
    document.getElementById("exchangeRate").value = "58.00";
    document.getElementById('invoiceNumber').value = generateInvoiceNumber();
    calculateResults();
    
    // Mostrar notificación
    alert(currentLang === 'es' ? 'Formulario limpiado!' : 'Form cleared!');
  }

  // Funciones del Dashboard
  function openDashboard() {
    document.getElementById("dashboardModal").style.display = "block";
    loadDashboardData();
  }

  function closeModal(modalId) {
    document.getElementById(modalId).style.display = "none";
  }

  function openTab(evt, tabName) {
    const tabContents = document.getElementsByClassName("tab-content");
    for (let i = 0; i < tabContents.length; i++) {
      tabContents[i].classList.remove("active");
    }
    
    const tabs = document.getElementsByClassName("tab");
    for (let i = 0; i < tabs.length; i++) {
      tabs[i].classList.remove("active");
    }
    
    document.getElementById(tabName).classList.add("active");
    evt.currentTarget.classList.add("active");
  }

  async function loadDashboardData() {
    try {
      // Cargar datos para gráficos
      const invoicesSnapshot = await db.collection("invoices")
        .orderBy("date", "desc")
        .limit(30)
        .get();
      
      const customersSnapshot = await db.collection("customers")
        .orderBy("lastPurchase", "desc")
        .get();
      
      const templatesSnapshot = await db.collection("templates")
        .orderBy("name")
        .get();
      
      // Procesar datos para gráficos
      const invoicesData = invoicesSnapshot.docs.map(doc => doc.data());
      const customersData = customersSnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
      const templatesData = templatesSnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
      
      // Actualizar pestaña de clientes
      renderCustomersList(customersData);
      
      // Actualizar pestaña de plantillas
      renderTemplatesList(templatesData);
      
      // Actualizar actividad reciente
      renderRecentActivity(invoicesData);
      
      // Crear gráficos
      createMonthlyProfitChart(invoicesData);
      createFilamentUsageChart(invoicesData);
      
    } catch (error) {
      console.error("Error loading dashboard data:", error);
    }
  }

  function renderCustomersList(customers) {
    const list = document.getElementById("customers-list");
    list.innerHTML = "";
    
    if (customers.length === 0) {
      list.innerHTML = `<div style="text-align:center; padding:20px; color:#a0aec0;">${currentLang === 'es' ? 'No hay clientes registrados' : 'No customers found'}</div>`;
      return;
    }
    
    customers.forEach(customer => {
      const div = document.createElement("div");
      div.className = "customer-card";
      div.innerHTML = `
        <h3>${customer.name}</h3>
        <div style="display: flex; justify-content: space-between; margin-top: 10px;">
          <div>
            <strong>${currentLang === 'es' ? 'Total gastado:' : 'Total spent:'}</strong> RD$${customer.totalSpent.toFixed(2)}<br>
            <strong>${currentLang === 'es' ? 'Facturas:' : 'Invoices:'}</strong> ${customer.invoices.length}
          </div>
          <div style="text-align: right;">
            <strong>${currentLang === 'es' ? 'Última compra:' : 'Last purchase:'}</strong><br>
            ${new Date(customer.lastPurchase).toLocaleDateString()}
          </div>
        </div>
      `;
      div.addEventListener("click", () => {
        document.getElementById("customerName").value = customer.name;
        closeModal("dashboardModal");
      });
      list.appendChild(div);
    });
  }

  function renderTemplatesList(templates) {
    const list = document.getElementById("templates-list");
    list.innerHTML = "";
    
    if (templates.length === 0) {
      list.innerHTML = `<div style="text-align:center; padding:20px; color:#a0aec0;">${currentLang === 'es' ? 'No hay plantillas guardadas' : 'No templates found'}</div>`;
      return;
    }
    
    templates.forEach(template => {
      const div = document.createElement("div");
      div.className = "template-card";
      div.innerHTML = `
        <h3>${template.name}</h3>
        <div style="margin-top: 10px;">
          <strong>${currentLang === 'es' ? 'Tipo de filamento:' : 'Filament type:'}</strong> ${getFilamentTypeName(template.filamentPrice)}<br>
          <strong>${currentLang === 'es' ? 'Precio sugerido:' : 'Suggested price:'}</strong> RD$${template.suggestedPrice.toFixed(2)}
        </div>
      `;
      div.addEventListener("click", () => loadTemplate(template));
      list.appendChild(div);
    });
  }

  function getFilamentTypeName(price) {
    switch(price) {
      case 1600: return "PLA";
      case 2000: return "PETG";
      case 2250: return "ABS";
      case 2150: return "ASA";
      default: return currentLang === 'es' ? "Personalizado" : "Custom";
    }
  }

  function loadTemplate(template) {
    document.getElementById("electricityPrice").value = template.electricityPrice || "2.80";
    document.getElementById("printHours").value = template.printHours || "6";
    document.getElementById("weight").value = template.weight || "150";
    document.getElementById("filamentPrice").value = template.filamentPrice || "1600";
    document.getElementById("filamentType").value = template.filamentPrice || "1600";
    document.getElementById("laborCost").value = template.laborCost || "150";
    document.getElementById("otherCosts").value = template.otherCosts || "100";
    document.getElementById("maintenanceCost").value = template.maintenanceCost || "50";
    document.getElementById("printerDepreciation").value = template.printerDepreciation || "14.00";
    document.getElementById("margin").value = template.margin || "40";
    document.getElementById("exchangeRate").value = template.exchangeRate || "58.00";
    
    calculateResults();
    closeModal("dashboardModal");
    
    alert(currentLang === 'es' ? `Plantilla "${template.name}" cargada` : `Template "${template.name}" loaded`);
  }

  async function saveTemplate() {
    const templateName = prompt(currentLang === 'es' ? "Ingrese un nombre para la plantilla:" : "Enter a name for the template:");
    if (!templateName) return;
    
    try {
      await db.collection("templates").add({
        name: templateName,
        electricityPrice: parseFloat(document.getElementById("electricityPrice").value) || 0,
        printHours: parseFloat(document.getElementById("printHours").value) || 0,
        weight: parseFloat(document.getElementById("weight").value) || 0,
        filamentPrice: parseFloat(document.getElementById("filamentPrice").value) || 0,
        laborCost: parseFloat(document.getElementById("laborCost").value) || 0,
        otherCosts: parseFloat(document.getElementById("otherCosts").value) || 0,
        maintenanceCost: parseFloat(document.getElementById("maintenanceCost").value) || 0,
        printerDepreciation: parseFloat(document.getElementById("printerDepreciation").value) || 0,
        margin: parseFloat(document.getElementById("margin").value) || 0,
        exchangeRate: parseFloat(document.getElementById("exchangeRate").value) || 0,
        suggestedPrice: parseFloat(results.suggestedPrice.textContent) || 0,
        createdAt: new Date().toISOString()
      });
      
      alert(currentLang === 'es' ? 'Plantilla guardada con éxito!' : 'Template saved successfully!');
    } catch (error) {
      console.error("Error saving template:", error);
      alert(currentLang === 'es' ? 'Error al guardar la plantilla' : 'Error saving template');
    }
  }

  function renderRecentActivity(invoices) {
    const container = document.getElementById("recent-activity");
    container.innerHTML = "";
    
    if (invoices.length === 0) {
      container.innerHTML = `<div style="text-align:center; padding:20px; color:#a0aec0;">${currentLang === 'es' ? 'No hay actividad reciente' : 'No recent activity'}</div>`;
      return;
    }
    
    invoices.slice(0, 5).forEach(invoice => {
      const div = document.createElement("div");
      div.style.padding = "10px";
      div.style.borderBottom = "1px solid #4a5568";
      div.innerHTML = `
        <div style="display: flex; justify-content: space-between;">
          <div>
            <strong>${invoice.customerName}</strong><br>
            <small>${new Date(invoice.date).toLocaleString()}</small>
          </div>
          <div style="text-align: right;">
            <strong>RD$${invoice.suggestedPrice.toFixed(2)}</strong><br>
            <small>${invoice.invoiceNumber}</small>
          </div>
        </div>
      `;
      container.appendChild(div);
    });
  }

  function createMonthlyProfitChart(invoices) {
    // Agrupar por mes
    const monthlyData = {};
    invoices.forEach(invoice => {
      const date = new Date(invoice.date);
      const monthYear = `${date.getFullYear()}-${(date.getMonth() + 1).toString().padStart(2, '0')}`;
      
      if (!monthlyData[monthYear]) {
        monthlyData[monthYear] = 0;
      }
      
      monthlyData[monthYear] += invoice.suggestedPrice - invoice.totalCost;
    });
    
    const labels = Object.keys(monthlyData).sort();
    const data = labels.map(label => monthlyData[label]);
    
    const ctx = document.getElementById('monthlyProfitChart').getContext('2d');
    
    if (monthlyProfitChart) {
      monthlyProfitChart.destroy();
    }
    
    monthlyProfitChart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: labels,
        datasets: [{
          label: currentLang === 'es' ? 'Ganancias' : 'Profits',
          data: data,
          backgroundColor: 'rgba(255, 159, 67, 0.7)',
          borderColor: 'rgba(255, 159, 67, 1)',
          borderWidth: 1
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          y: {
            beginAtZero: true,
            title: {
              display: true,
              text: 'RD$'
            }
          }
        }
      }
    });
  }

  function createFilamentUsageChart(invoices) {
    // Agrupar por tipo de filamento
    const filamentData = {
      "PLA": 0,
      "PETG": 0,
      "ABS": 0,
      "ASA": 0,
      [currentLang === 'es' ? 'Personalizado' : 'Custom']: 0
    };
    
    invoices.forEach(invoice => {
      const filamentPrice = invoice.filamentPrice || 1600;
      let type;
      
      if (filamentPrice === 1600) type = "PLA";
      else if (filamentPrice === 2000) type = "PETG";
      else if (filamentPrice === 2250) type = "ABS";
      else if (filamentPrice === 2150) type = "ASA";
      else type = currentLang === 'es' ? 'Personalizado' : 'Custom';
      
      filamentData[type] += (invoice.weight || 0) / 1000; // Convertir a kg
    });
    
    const labels = Object.keys(filamentData);
    const data = Object.values(filamentData);
    
    const ctx = document.getElementById('filamentUsageChart').getContext('2d');
    
    if (filamentUsageChart) {
      filamentUsageChart.destroy();
    }
    
    filamentUsageChart = new Chart(ctx, {
      type: 'pie',
      data: {
        labels: labels,
        datasets: [{
          data: data,
          backgroundColor: [
            'rgba(54, 162, 235, 0.7)',
            'rgba(255, 99, 132, 0.7)',
            'rgba(75, 192, 192, 0.7)',
            'rgba(153, 102, 255, 0.7)',
            'rgba(255, 206, 86, 0.7)'
          ],
          borderColor: [
            'rgba(54, 162, 235, 1)',
            'rgba(255, 99, 132, 1)',
            'rgba(75, 192, 192, 1)',
            'rgba(153, 102, 255, 1)',
            'rgba(255, 206, 86, 1)'
          ],
          borderWidth: 1
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            position: 'right',
          },
          tooltip: {
            callbacks: {
              label: function(context) {
                return `${context.label}: ${context.raw.toFixed(2)} kg`;
              }
            }
          }
        }
      }
    });
  }

  function searchCustomers() {
    const searchTerm = document.getElementById("customerSearch").value.toLowerCase();
    const customers = document.querySelectorAll("#customers-list .customer-card");
    
    customers.forEach(customer => {
      const name = customer.querySelector("h3").textContent.toLowerCase();
      if (name.includes(searchTerm)) {
        customer.style.display = "block";
      } else {
        customer.style.display = "none";
      }
    });
  }

  function searchTemplates() {
    const searchTerm = document.getElementById("templateSearch").value.toLowerCase();
    const templates = document.querySelectorAll("#templates-list .template-card");
    
    templates.forEach(template => {
      const name = template.querySelector("h3").textContent.toLowerCase();
      if (name.includes(searchTerm)) {
        template.style.display = "block";
      } else {
        template.style.display = "none";
      }
    });
  }

  async function loadCustomersForAutocomplete() {
    try {
      const querySnapshot = await db.collection("customers").orderBy("name").get();
      const datalist = document.getElementById("customersList");
      datalist.innerHTML = "";
      
      querySnapshot.forEach(doc => {
        const customer = doc.data();
        const option = document.createElement("option");
        option.value = customer.name;
        datalist.appendChild(option);
      });
    } catch (error) {
      console.error("Error loading customers:", error);
    }
  }

  // Manejar cambio de tipo de filamento
  document.getElementById("filamentType").addEventListener("change", function() {
    if (this.value !== "custom") {
      document.getElementById("filamentPrice").value = this.value;
      calculateResults();
    }
  });

  // Inicialización
  window.onload = async function() {
    translatePage();
    calculateResults();
    await renderHistory();
    await loadCustomersForAutocomplete();
    
    // Asignar eventos a los inputs
    document.querySelectorAll("input").forEach(input => {
      input.addEventListener("input", calculateResults);
    });
    
    document.getElementById("filamentPrice").addEventListener("input", function() {
      if (this.value !== document.getElementById("filamentType").value) {
        document.getElementById("filamentType").value = "custom";
      }
    });
  };
</script>
</body>
</html>
