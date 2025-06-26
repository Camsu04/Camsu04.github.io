<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Factura 3DCraftRD</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script> 
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
      max-width: 700px;
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
    form input[type="text"] {
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
    form input[type="text"]:focus {
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
    .btn-lang {
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

    .btn-export:hover,
    .btn-save:hover,
    .btn-clear:hover,
    .btn-lang:hover {
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
    <button class="btn-lang" onclick="toggleLang()">Toggle Language</button>
  </div>

  <div class="invoice-header">
    <div>
      <label for="customerName" id="lbl-customer-name">Nombre del Cliente</label>
      <input type="text" id="customerName" placeholder="Ingrese el nombre del cliente" />
    </div>
    <div>
      <label for="invoiceNumber" id="lbl-invoice-number">Número de Factura</label>
      <input type="text" id="invoiceNumber" readonly />
    </div>
  </div>

  <form id="calculator-form">
    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
      <div>
        <label for="electricityPrice" id="lbl-electricity-price">Precio de la electricidad (RD$/kWh)</label>
        <input type="number" step="0.01" id="electricityPrice" value="2.80" />

        <label for="printHours" id="lbl-print-hours">Horas de impresión</label>
        <input type="number" id="printHours" value="6" />

        <label for="weight" id="lbl-weight">Peso del modelo (g)</label>
        <input type="number" id="weight" value="150" />

        <label for="filamentPrice" id="lbl-filament-price">Precio del filamento (RD$/kg)</label>
        <input type="number" id="filamentPrice" value="800" />
      </div>
      
      <div>
        <label for="laborCost" id="lbl-labor-cost">Mano de obra (RD$)</label>
        <input type="number" id="laborCost" value="150" />

        <label for="otherCosts" id="lbl-other-costs">Otros costos (RD$)</label>
        <input type="number" id="otherCosts" value="100" />

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
    <div><span id="lbl-total-cost">Costo total:</span> <span>RD$<span id="totalCost">0.00</span></span></div>
    <div><span id="lbl-suggested-price">Precio sugerido:</span> <span>RD$<span id="suggestedPrice">0.00</span></span></div>
    <div><span id="lbl-net-profit">Ganancia neta:</span> <span>RD$<span id="netProfit">0.00</span></span></div>
  </div>

  <div class="section-title" id="lbl-customer-results">Detalles de la Factura (Cliente)</div>
  <div class="results customer-results">
    <div><span id="lbl-customer-material-cost">Costo del material:</span> <span>RD$<span id="customerMaterialCost">0.00</span></span></div>
    <div><span id="lbl-customer-energy-cost">Costo de energía eléctrica:</span> <span>RD$<span id="customerEnergyCost">0.00</span></span></div>
    <div><span id="lbl-customer-printing-cost">Costo de impresión:</span> <span>RD$<span id="customerPrintingCost">0.00</span></span></div>
    <div><span id="lbl-customer-total-price">Precio Total:</span> <span>RD$<span id="customerTotalPrice">0.00</span></span></div>
  </div>

  <div class="buttons-container">
    <button class="btn-export" onclick="exportToPDF()" id="btn-export">Exportar a PDF</button>
    <button class="btn-save" onclick="saveToHistory()" id="btn-save">Guardar en Historial</button>
    <button class="btn-clear" onclick="clearForm()" id="btn-clear">Limpiar Formulario</button>
  </div>

  <div class="history" id="history-section">
    <h3 id="lbl-history">Historial de Impresiones</h3>
    <div id="history-list"></div>
  </div>
</div>

<script>
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

  const translations = {
    es: {
      "app-title": "Factura",
      "lbl-customer-name": "Nombre del Cliente",
      "lbl-invoice-number": "Número de Factura",
      "lbl-electricity-price": "Precio de la electricidad (RD$/kWh)",
      "lbl-print-hours": "Horas de impresión",
      "lbl-weight": "Peso del modelo (g)",
      "lbl-filament-price": "Precio del filamento (RD$/kg)",
      "lbl-labor-cost": "Mano de obra (RD$)",
      "lbl-other-costs": "Otros costos (RD$)",
      "lbl-margin": "Margen de ganancia (%)",
      "lbl-exchange-rate": "Tasa de cambio USD a RD$",
      "lbl-material-cost": "Costo del material:",
      "lbl-energy-cost": "Costo de energía eléctrica:",
      "lbl-printing-cost": "Costo de impresión:",
      "lbl-total-cost": "Costo total:",
      "lbl-suggested-price": "Precio sugerido:",
      "lbl-net-profit": "Ganancia neta:",
      "lbl-customer-material-cost": "Costo del material:",
      "lbl-customer-energy-cost": "Costo de energía eléctrica:",
      "lbl-customer-printing-cost": "Costo de impresión:",
      "lbl-customer-total-price": "Precio Total:",
      "btn-export": "Exportar a PDF",
      "btn-save": "Guardar en Historial",
      "btn-clear": "Limpiar Formulario",
      "lbl-history": "Historial de Impresiones",
      "lbl-admin-results": "Resultados (Administrador)",
      "lbl-customer-results": "Detalles de la Factura (Cliente)"
    },
    en: {
      "app-title": "Invoice",
      "lbl-customer-name": "Customer Name",
      "lbl-invoice-number": "Invoice Number",
      "lbl-electricity-price": "Electricity Price (RD$/kWh)",
      "lbl-print-hours": "Print Hours",
      "lbl-weight": "Model Weight (g)",
      "lbl-filament-price": "Filament Price (RD$/kg)",
      "lbl-labor-cost": "Labor Cost (RD$)",
      "lbl-other-costs": "Other Costs (RD$)",
      "lbl-margin": "Profit Margin (%)",
      "lbl-exchange-rate": "Exchange Rate USD to RD$",
      "lbl-material-cost": "Material Cost:",
      "lbl-energy-cost": "Electricity Cost:",
      "lbl-printing-cost": "Printing Cost:",
      "lbl-total-cost": "Total Cost:",
      "lbl-suggested-price": "Suggested Price:",
      "lbl-net-profit": "Net Profit:",
      "lbl-customer-material-cost": "Material Cost:",
      "lbl-customer-energy-cost": "Electricity Cost:",
      "lbl-customer-printing-cost": "Printing Cost:",
      "lbl-customer-total-price": "Total Price:",
      "btn-export": "Export to PDF",
      "btn-save": "Save to History",
      "btn-clear": "Clear Form",
      "lbl-history": "Print History",
      "lbl-admin-results": "Results (Admin)",
      "lbl-customer-results": "Invoice Details (Customer)"
    }
  };

  let currentLang = navigator.language.startsWith('es') ? 'es' : 'en';

  function translatePage() {
    Object.keys(translations[currentLang]).forEach(key => {
      const element = document.getElementById(key);
      if (element) {
        element.innerText = translations[currentLang][key];
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
    totalCost: document.getElementById("totalCost"),
    suggestedPrice: document.getElementById("suggestedPrice"),
    netProfit: document.getElementById("netProfit"),
    customerMaterialCost: document.getElementById("customerMaterialCost"),
    customerEnergyCost: document.getElementById("customerEnergyCost"),
    customerPrintingCost: document.getElementById("customerPrintingCost"),
    customerTotalPrice: document.getElementById("customerTotalPrice")
  };

  function calculateResults() {
    const electricityPrice = parseFloat(document.getElementById("electricityPrice").value) || 0;
    const printHours = parseFloat(document.getElementById("printHours").value) || 0;
    const weight = parseFloat(document.getElementById("weight").value) || 0;
    const filamentPrice = parseFloat(document.getElementById("filamentPrice").value) || 0;
    const laborCost = parseFloat(document.getElementById("laborCost").value) || 0;
    const otherCosts = parseFloat(document.getElementById("otherCosts").value) || 0;
    const margin = parseFloat(document.getElementById("margin").value) || 0;

    const printerPowerKw = 0.15; // kW
    const depreciationPerHour = 14; // RD$

    const energyConsumption = printerPowerKw * printHours;
    const energyCost = energyConsumption * electricityPrice;
    const printingCost = (electricityPrice * printerPowerKw + depreciationPerHour) * printHours;
    const materialCost = (weight / 1000) * filamentPrice;
    const totalCost = materialCost + energyCost + laborCost + otherCosts;
    const suggestedPrice = totalCost * (1 + margin / 100);
    const netProfit = suggestedPrice - totalCost;

    // Resultados para administrador
    results.materialCost.textContent = materialCost.toFixed(2);
    results.energyCost.textContent = energyCost.toFixed(2);
    results.printingCost.textContent = printingCost.toFixed(2);
    results.totalCost.textContent = totalCost.toFixed(2);
    results.suggestedPrice.textContent = suggestedPrice.toFixed(2);
    results.netProfit.textContent = netProfit.toFixed(2);

    // Resultados para cliente
    results.customerMaterialCost.textContent = materialCost.toFixed(2);
    results.customerEnergyCost.textContent = energyCost.toFixed(2);
    results.customerPrintingCost.textContent = printingCost.toFixed(2);
    results.customerTotalPrice.textContent = suggestedPrice.toFixed(2);
  }

  function exportToPDF() {
    // Verificar si jsPDF está cargado
    if (typeof window.jspdf === 'undefined') {
      alert('Error: La biblioteca jsPDF no está cargada. Por favor recarga la página.');
      return;
    }
    
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    
    const customerName = document.getElementById("customerName").value || "Cliente no especificado";
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
    doc.text(`Fecha: ${dateStr}`, 180, 45, null, null, "right");
    
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
    yPos += 15;
    
    // Precio total
    doc.setFontSize(16);
    doc.setFont(undefined, 'bold');
    doc.text(`${lang["lbl-customer-total-price"]}: RD$${results.customerTotalPrice.textContent}`, 30, yPos);
    
    // Nota al pie
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    doc.text("Gracias por su preferencia - 3DCraftRD", 105, 280, null, null, "center");
    
    // Guardar el PDF
    doc.save(`Factura_${invoiceNumber}.pdf`);
  }

  function saveToHistory() {
    const history = JSON.parse(localStorage.getItem("3dcraftrd_history") || "[]");
    const now = new Date().toLocaleString();
    const data = {
      date: now,
      customerName: document.getElementById("customerName").value || "Sin nombre",
      invoiceNumber: document.getElementById("invoiceNumber").value || generateInvoiceNumber(),
      electricityPrice: document.getElementById("electricityPrice").value,
      printHours: document.getElementById("printHours").value,
      weight: document.getElementById("weight").value,
      filamentPrice: document.getElementById("filamentPrice").value,
      laborCost: document.getElementById("laborCost").value,
      otherCosts: document.getElementById("otherCosts").value,
      margin: document.getElementById("margin").value,
      totalCost: results.totalCost.textContent,
      suggestedPrice: results.suggestedPrice.textContent
    };
    history.unshift(data);
    localStorage.setItem("3dcraftrd_history", JSON.stringify(history));
    renderHistory();
    
    // Generar nuevo número de factura para el próximo
    document.getElementById('invoiceNumber').value = generateInvoiceNumber();
    
    // Mostrar notificación
    alert(currentLang === 'es' ? 'Guardado en el historial!' : 'Saved to history!');
  }

  function renderHistory() {
    const list = document.getElementById("history-list");
    const history = JSON.parse(localStorage.getItem("3dcraftrd_history") || "[]");
    list.innerHTML = "";
    
    if (history.length === 0) {
      list.innerHTML = `<div style="text-align:center; padding:20px; color:#a0aec0;">${currentLang === 'es' ? 'No hay registros en el historial' : 'No records in history'}</div>`;
      return;
    }
    
    history.forEach(item => {
      const div = document.createElement("div");
      div.className = "history-item";
      div.innerHTML = `
        <strong>${item.date}</strong><br>
        <div style="display:flex; justify-content:space-between; margin-top:8px;">
          <div>
            <strong>${currentLang === 'es' ? 'Cliente' : 'Customer'}:</strong> ${item.customerName}<br>
            <strong>${currentLang === 'es' ? 'Factura' : 'Invoice'}:</strong> ${item.invoiceNumber}
          </div>
          <div style="text-align:right;">
            <strong>${currentLang === 'es' ? 'Total' : 'Total'}:</strong> RD$${item.suggestedPrice}
          </div>
        </div>
      `;
      list.appendChild(div);
    });
  }

  function clearForm() {
    document.getElementById("customerName").value = "";
    document.getElementById("electricityPrice").value = "2.80";
    document.getElementById("printHours").value = "6";
    document.getElementById("weight").value = "150";
    document.getElementById("filamentPrice").value = "800";
    document.getElementById("laborCost").value = "150";
    document.getElementById("otherCosts").value = "100";
    document.getElementById("margin").value = "40";
    document.getElementById('invoiceNumber').value = generateInvoiceNumber();
    calculateResults();
    
    // Mostrar notificación
    alert(currentLang === 'es' ? 'Formulario limpiado!' : 'Form cleared!');
  }

  // Inicialización
  window.onload = function() {
    translatePage();
    calculateResults();
    renderHistory();
    
    // Asignar eventos a los inputs
    document.querySelectorAll("input").forEach(input => {
      input.addEventListener("input", calculateResults);
    });
  };
</script>
</body>
</html>
