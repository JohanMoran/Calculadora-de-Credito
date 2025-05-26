<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Simulador de Amortización de Crédito</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.1/moment.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.1/locale/es-mx.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary-color: #2c3e50;
      --secondary-color: #3498db;
      --accent-color: #2980b9;
      --light-bg: #f8fafc;
      --dark-text: #2d3748;
      --light-text: #4a5568;
      --border-color: #e2e8f0;
      --success-color: #38a169;
      --error-color: #e53e3e;
    }
    
    body {
      font-family: 'Roboto', sans-serif;
      background: var(--light-bg);
      color: var(--dark-text);
      padding: 2rem;
      max-width: 1200px;
      margin: 0 auto;
      line-height: 1.6;
    }
    
    .container {
      background: white;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
      padding: 2rem;
      margin-bottom: 2rem;
    }
    
    h2, h3, h4 {
      color: var(--primary-color);
      margin-top: 0;
    }
    
    h2 {
      font-size: 1.8rem;
      font-weight: 700;
      margin-bottom: 1.5rem;
      border-bottom: 2px solid var(--border-color);
      padding-bottom: 0.5rem;
    }
    
    h3 {
      font-size: 1.4rem;
      font-weight: 600;
      margin-bottom: 1rem;
    }
    
    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
      color: var(--light-text);
      font-size: 0.95rem;
    }
    
    input, select {
      width: 100%;
      padding: 0.75rem;
      border: 1px solid var(--border-color);
      border-radius: 6px;
      font-size: 1rem;
      transition: border 0.3s ease;
      max-width: 300px;
    }
    
    input:focus, select:focus {
      outline: none;
      border-color: var(--secondary-color);
      box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1);
    }
    
    .form-group {
      margin-bottom: 1.25rem;
    }
    
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }
    
    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      padding: 0.75rem 1.5rem;
      font-size: 1rem;
      font-weight: 500;
      border-radius: 6px;
      cursor: pointer;
      transition: all 0.3s ease;
      border: none;
    }
    
    .btn-primary {
      background-color: var(--secondary-color);
      color: white;
    }
    
    .btn-primary:hover {
      background-color: var(--accent-color);
      transform: translateY(-1px);
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    .btn-secondary {
      background-color: white;
      color: var(--secondary-color);
      border: 1px solid var(--secondary-color);
    }
    
    .btn-secondary:hover {
      background-color: var(--light-bg);
    }
    
    .btn-icon {
      margin-right: 8px;
    }
    
    .abonos {
      background-color: #f0f7ff;
      border-radius: 8px;
      padding: 1.5rem;
      margin-top: 2rem;
      border-left: 4px solid var(--secondary-color);
    }
    
    .abono-list {
      list-style: none;
      padding: 0;
      margin: 1rem 0 0 0;
    }
    
    .abono-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0.75rem;
      background: white;
      border-radius: 6px;
      margin-bottom: 0.5rem;
      border: 1px solid var(--border-color);
    }
    
    .abono-item-content {
      flex: 1;
    }
    
    .abono-item-remove {
      background: none;
      border: none;
      color: var(--error-color);
      cursor: pointer;
      font-size: 0.8rem;
      padding: 0.25rem;
      margin-left: 0.5rem;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      transition: all 0.2s ease;
    }
    
    .abono-item-remove:hover {
      background: rgba(229, 62, 62, 0.1);
    }
    
    table {
      width: 100%;
      border-collapse: collapse;
      margin: 1.5rem 0;
      font-size: 0.9rem;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
    }
    
    th {
      background-color: var(--primary-color);
      color: white;
      font-weight: 500;
      padding: 0.75rem;
      text-align: left;
    }
    
    td {
      padding: 0.75rem;
      border-bottom: 1px solid var(--border-color);
      text-align: right;
    }
    
    tr:nth-child(even) {
      background-color: var(--light-bg);
    }
    
    tr:hover {
      background-color: #edf2f7;
    }
    
    .resumen {
      background-color: white;
      border-radius: 8px;
      padding: 1.5rem;
      margin-top: 2rem;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
      border-left: 4px solid var(--success-color);
    }
    
    .resumen-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1.5rem;
      margin-top: 1rem;
    }
    
    .resumen-item {
      background: var(--light-bg);
      padding: 1rem;
      border-radius: 6px;
    }
    
    .resumen-item-label {
      font-size: 0.85rem;
      color: var(--light-text);
      margin-bottom: 0.5rem;
    }
    
    .resumen-item-value {
      font-size: 1.25rem;
      font-weight: 600;
      color: var(--primary-color);
    }
    
    .action-buttons {
      display: flex;
      gap: 1rem;
      margin-top: 1.5rem;
      flex-wrap: wrap;
    }
    
    @media (max-width: 768px) {
      body {
        padding: 1rem;
      }
      
      .container {
        padding: 1.25rem;
      }
      
      .grid {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Simulador de Amortización</h2>
    
    <div class="grid">
      <div class="form-group">
        <label for="monto">Monto del Crédito</label>
        <input type="number" id="monto" placeholder="Ej. 100,000" />
      </div>
      <div class="form-group">
        <label for="tasa">Tasa de Interés Anual (%)</label>
        <input type="number" id="tasa" step="0.01" placeholder="Ej. 12.5" />
      </div>
      <div class="form-group">
        <label for="plazo">Plazo (meses)</label>
        <input type="number" id="plazo" placeholder="Ej. 36" />
      </div>
      <div class="form-group">
        <label for="fechaInicio">Fecha de Inicio del Crédito</label>
        <input type="date" id="fechaInicio" />
      </div>
    </div>
    
    <button class="btn btn-primary" onclick="calcularAmortizacion()">
      <span class="btn-icon">📊</span> Calcular Amortización
    </button>
    
    <div class="abonos">
      <h3>Abonos Extraordinarios</h3>
      <div class="grid">
        <div class="form-group">
          <label for="fechaAbono">Fecha del Abono</label>
          <input type="date" id="fechaAbono" />
        </div>
        <div class="form-group">
          <label for="montoAbono">Monto del Abono</label>
          <input type="number" id="montoAbono" placeholder="Ej. 10,000" />
        </div>
      </div>
      <button class="btn btn-secondary" onclick="agregarAbono()">
        <span class="btn-icon">➕</span> Agregar Abono
      </button>
      <ul id="listaAbonos" class="abono-list"></ul>
    </div>
    
    <div id="resumen" class="resumen" style="display: none;"></div>
    
    <div id="tabla"></div>
    
    <div class="action-buttons">
      <button class="btn btn-primary" onclick="descargarExcel()">
        <span class="btn-icon">📊</span> Exportar a Excel
      </button>
      <button class="btn btn-primary" onclick="descargarPDF()">
        <span class="btn-icon">📄</span> Generar Reporte PDF
      </button>
    </div>
  </div>

  <script>
    // Configurar moment.js en español
    moment.locale('es-mx');
    
    let abonos = [];
    let datosTabla = [];
    let fechaInicioCredito = null;
    let totalAbonosExtra = 0;

    // Función para formatear números como moneda mexicana
    function formatoMoneda(num) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(num);
    }

    // Función para formatear fechas
    function formatoFecha(fecha) {
      return moment(fecha).format('LL');
    }

    function agregarAbono() {
      const fechaAbono = document.getElementById('fechaAbono').value;
      const monto = parseFloat(document.getElementById('montoAbono').value);
      
      if (!fechaAbono) {
        alert("Por favor seleccione una fecha válida para el abono");
        return;
      }
      
      if (!monto || monto <= 0) {
        alert("Por favor ingrese un monto válido (mayor que 0)");
        return;
      }
      
      abonos.push({ fecha: fechaAbono, monto });
      totalAbonosExtra += monto;
      mostrarAbonos();
      document.getElementById('fechaAbono').value = '';
      document.getElementById('montoAbono').value = '';
      
      // Recalcular la amortización si ya existe una tabla
      if (datosTabla.length > 0) {
        calcularAmortizacion();
      }
    }

    function eliminarAbono(index) {
      totalAbonosExtra -= abonos[index].monto;
      abonos.splice(index, 1);
      mostrarAbonos();
      
      // Recalcular la amortización si ya existe una tabla
      if (datosTabla.length > 0) {
        calcularAmortizacion();
      }
    }

    function mostrarAbonos() {
      const lista = document.getElementById('listaAbonos');
      lista.innerHTML = '';
      
      if (abonos.length === 0) {
        const li = document.createElement('li');
        li.textContent = 'No hay abonos extraordinarios agregados';
        li.style.color = 'var(--light-text)';
        li.style.fontStyle = 'italic';
        lista.appendChild(li);
        return;
      }
      
      // Ordenar abonos por fecha
      abonos.sort((a, b) => new Date(a.fecha) - new Date(b.fecha)).forEach((abono, index) => {
        const li = document.createElement('li');
        li.className = 'abono-item';
        li.innerHTML = `
          <div class="abono-item-content">
            <strong>${formatoFecha(abono.fecha)}</strong>: ${formatoMoneda(abono.monto)}
          </div>
          <button class="abono-item-remove" onclick="eliminarAbono(${index})" title="Eliminar abono">
            ✕
          </button>
        `;
        lista.appendChild(li);
      });
    }

    function calcularAmortizacion() {
      const montoInicial = parseFloat(document.getElementById('monto').value);
      const tasaAnual = parseFloat(document.getElementById('tasa').value);
      const plazo = parseInt(document.getElementById('plazo').value);
      fechaInicioCredito = document.getElementById('fechaInicio').value;

      // Validaciones
      if (!montoInicial || montoInicial <= 0) {
        alert("Por favor ingrese un monto de crédito válido");
        return;
      }
      
      if (!tasaAnual || tasaAnual <= 0) {
        alert("Por favor ingrese una tasa de interés válida");
        return;
      }
      
      if (!plazo || plazo <= 0) {
        alert("Por favor ingrese un plazo válido");
        return;
      }
      
      if (!fechaInicioCredito) {
        alert("Por favor seleccione una fecha de inicio válida");
        return;
      }

      const tasaMensual = tasaAnual / 12 / 100;
      const cuota = montoInicial * (tasaMensual / (1 - Math.pow(1 + tasaMensual, -plazo)));

      let saldo = montoInicial;
      let totalIntereses = 0;
      let totalCapital = 0;
      totalAbonosExtra = 0; // Reiniciar el total de abonos
      datosTabla = [];

      let tablaHTML = `
        <div class="container">
          <h3>Tabla de Amortización Detallada</h3>
          <table>
            <thead>
              <tr>
                <th>Fecha de Pago</th>
                <th>Pago Mensual</th>
                <th>Interés</th>
                <th>Capital</th>
                <th>Abono Extra</th>
                <th>Saldo</th>
              </tr>
            </thead>
            <tbody>
      `;

      let fechaPago = moment(fechaInicioCredito);
      let mes = 1;
      
      while (saldo > 0.01 && mes <= 600) {
        const interes = saldo * tasaMensual;
        let capital = cuota - interes;
        if (capital > saldo) capital = saldo;
        saldo -= capital;
        totalIntereses += interes;
        totalCapital += capital;

        // Verificar si hay abonos en esta fecha
        const fechaPagoStr = fechaPago.format('YYYY-MM-DD');
        const abonoExtra = abonos.find(a => a.fecha === fechaPagoStr);
        let abono = 0;
        let abonoTexto = '-';
        if (abonoExtra) {
          abono = Math.min(abonoExtra.monto, saldo);
          saldo -= abono;
          totalCapital += abono;
          totalAbonosExtra += abono;
          abonoTexto = formatoMoneda(abono);
        }

        datosTabla.push([
          fechaPago.toDate(),
          cuota,
          interes,
          capital,
          abono,
          saldo
        ]);

        tablaHTML += `
          <tr>
            <td>${formatoFecha(fechaPago.toDate())}</td>
            <td>${formatoMoneda(cuota)}</td>
            <td>${formatoMoneda(interes)}</td>
            <td>${formatoMoneda(capital)}</td>
            <td>${abonoTexto}</td>
            <td>${saldo > 0 ? formatoMoneda(saldo) : formatoMoneda(0)}</td>
          </tr>
        `;

        // Avanzar al siguiente mes
        fechaPago.add(1, 'month');
        mes++;
        if (saldo <= 0.01) break;
      }

      tablaHTML += '</tbody></table></div>';
      document.getElementById('tabla').innerHTML = tablaHTML;

      document.getElementById('resumen').innerHTML = `
        <h3>Resumen Ejecutivo</h3>
        <div class="resumen-grid">
          <div class="resumen-item">
            <div class="resumen-item-label">Fecha de Inicio</div>
            <div class="resumen-item-value">${formatoFecha(fechaInicioCredito)}</div>
          </div>
          <div class="resumen-item">
            <div class="resumen-item-label">Plazo Liquidado</div>
            <div class="resumen-item-value">${mes - 1} meses</div>
          </div>
          <div class="resumen-item">
            <div class="resumen-item-label">Total Pagado</div>
            <div class="resumen-item-value">${formatoMoneda(totalIntereses + totalCapital)}</div>
          </div>
          <div class="resumen-item">
            <div class="resumen-item-label">Total a Capital</div>
            <div class="resumen-item-value">${formatoMoneda(totalCapital)}</div>
          </div>
          <div class="resumen-item">
            <div class="resumen-item-label">Total Intereses</div>
            <div class="resumen-item-value">${formatoMoneda(totalIntereses)}</div>
          </div>
          <div class="resumen-item">
            <div class="resumen-item-label">Abonos Extraordinarios</div>
            <div class="resumen-item-value">${formatoMoneda(totalAbonosExtra)}</div>
          </div>
        </div>
      `;
      
      document.getElementById('resumen').style.display = 'block';
    }

    function descargarExcel() {
      if (datosTabla.length === 0) {
        alert("Primero calcule la amortización para generar el reporte.");
        return;
      }

      const wb = XLSX.utils.book_new();
      const ws = XLSX.utils.aoa_to_sheet([
        ["Fecha de Pago", "Pago Mensual", "Interés", "Capital", "Abono Extra", "Saldo"],
        ...datosTabla.map(fila => [
          moment(fila[0]).format('LL'),
          fila[1],
          fila[2],
          fila[3],
          fila[4],
          fila[5]
        ])
      ]);
      
      // Formato de moneda para las columnas numéricas
      const range = XLSX.utils.decode_range(ws['!ref']);
      for (let C = 1; C <= 5; ++C) {
        for (let R = 1; R <= range.e.r; ++R) {
          const cell = XLSX.utils.encode_cell({r:R, c:C});
          if (!ws[cell]) continue;
          ws[cell].z = '"$"#,##0.00_);("$"#,##0.00)';
        }
      }
      
      XLSX.utils.book_append_sheet(wb, ws, "Amortización");
      
      // Crear hoja de resumen
      const resumen = document.getElementById("resumen").innerText.split("\n");
      const resumenData = resumen.filter(line => line.trim() !== '').map(line => [line]);
      const wsResumen = XLSX.utils.aoa_to_sheet(resumenData);
      XLSX.utils.book_append_sheet(wb, wsResumen, "Resumen");
      
      XLSX.writeFile(wb, "reporte_amortizacion.xlsx");
    }

    async function descargarPDF() {
      if (datosTabla.length === 0) {
        alert("Primero calcule la amortización para generar el reporte.");
        return;
      }

      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: 'a4'
      });
      
      // Configuración de estilos
      const primaryColor = [44, 62, 80];
      const secondaryColor = [52, 152, 219];
      const lightColor = [248, 250, 252];
      
      // Logo y encabezado
      doc.setFont('helvetica', 'bold');
      doc.setFontSize(16);
      doc.setTextColor(...primaryColor);
      doc.text("Reporte de Amortización de Crédito", 105, 20, { align: 'center' });
      
      doc.setFontSize(10);
      doc.setTextColor(100, 100, 100);
      doc.text("Generado el " + moment().format('LL'), 190, 15, { align: 'right' });
      
      // Resumen ejecutivo
      doc.setFontSize(12);
      doc.setTextColor(...primaryColor);
      doc.setFont('helvetica', 'bold');
      doc.text("Resumen Ejecutivo", 20, 35);
      
      doc.setFont('helvetica', 'normal');
      doc.setTextColor(0, 0, 0);
      
      const resumen = document.getElementById("resumen").innerText.split("\n").filter(line => line.trim() !== '');
      let y = 45;
      
      resumen.forEach((line, i) => {
        if (i === 0) {
          doc.setFont('helvetica', 'bold');
          doc.setFontSize(14);
          doc.text(line, 20, y);
          doc.setFontSize(12);
          doc.setFont('helvetica', 'normal');
          y += 8;
        } else {
          doc.text(line, 20, y);
          y += 7;
        }
      });
      
      // Tabla de amortización
      y += 10;
      doc.setFont('helvetica', 'bold');
      doc.setFontSize(14);
      doc.setTextColor(...primaryColor);
      doc.text("Detalle de Amortización", 20, y);
      y += 10;
      
      // Encabezados de tabla
      const headers = ["Fecha", "Pago", "Interés", "Capital", "Extra", "Saldo"];
      const columnWidths = [30, 25, 25, 25, 25, 30];
      let x = 20;
      
      doc.setFillColor(...primaryColor);
      doc.setTextColor(255, 255, 255);
      doc.setFontSize(10);
      
      headers.forEach((header, i) => {
        doc.rect(x, y, columnWidths[i], 8, 'F');
        doc.text(header, x + 2, y + 5);
        x += columnWidths[i];
      });
      
      // Filas de datos
      doc.setFont('helvetica', 'normal');
      doc.setTextColor(0, 0, 0);
      y += 8;
      doc.setFontSize(8);
      
      datosTabla.forEach((fila, i) => {
        if (y > 270) {
          doc.addPage();
          y = 20;
          // Volver a dibujar encabezados en nueva página
          x = 20;
          doc.setFillColor(...primaryColor);
          doc.setTextColor(255, 255, 255);
          doc.setFont('helvetica', 'bold');
          headers.forEach((header, i) => {
            doc.rect(x, y, columnWidths[i], 8, 'F');
            doc.text(header, x + 2, y + 5);
            x += columnWidths[i];
          });
          doc.setFont('helvetica', 'normal');
          doc.setTextColor(0, 0, 0);
          y += 8;
        }
        
        x = 20;
        fila.forEach((valor, j) => {
          let formattedValue;
          if (j === 0) {
            formattedValue = moment(valor).format('MMM YYYY');
          } else {
            formattedValue = formatoMoneda(valor);
          }
          doc.text(formattedValue.toString(), x + 2, y + 5);
          x += columnWidths[j];
        });
        y += 6;
      });
      
      // Pie de página profesional
      doc.setFontSize(10);
      doc.setTextColor(100, 100, 100);
      doc.text("© " + new Date().getFullYear() + " - Simulador de Créditos Profesional", 105, 285, { align: 'center' });
      
      doc.save("reporte_amortizacion.pdf");
    }
    
    // Configurar fecha mínima para los inputs de fecha (hoy)
    document.getElementById('fechaInicio').min = new Date().toISOString().split('T')[0];
    document.getElementById('fechaAbono').min = new Date().toISOString().split('T')[0];
    
    // Mostrar lista de abonos vacía al cargar
    mostrarAbonos();
  </script>
</body>
</html>
