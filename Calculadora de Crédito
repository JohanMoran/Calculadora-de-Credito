<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Calculadora de Crédito Amortizado</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f4f7fa;
      color: #333;
      padding: 2rem;
      max-width: 1000px;
      margin: auto;
    }
    h2 {
      text-align: center;
      color: #2c3e50;
    }
    label {
      display: block;
      margin-top: 1rem;
    }
    input, button, select {
      padding: 0.5rem;
      margin-top: 0.2rem;
      font-size: 1rem;
      width: 100%;
      max-width: 300px;
    }
    .form-group {
      margin-bottom: 1rem;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1.5rem;
      font-size: 0.9rem;
    }
    table, th, td {
      border: 1px solid #ccc;
    }
    th {
      background: #2c3e50;
      color: white;
      padding: 0.6rem;
    }
    td {
      padding: 0.5rem;
      text-align: right;
    }
    .resumen {
      margin-top: 2rem;
      background: #ecf0f1;
      padding: 1rem;
      border-radius: 8px;
    }
    .abonos {
      background: #e3f2fd;
      padding: 1rem;
      border-radius: 8px;
      margin-top: 1rem;
    }
    .btn {
      margin-top: 1rem;
      background: #3498db;
      color: white;
      border: none;
      cursor: pointer;
      padding: 0.6rem 1rem;
      border-radius: 6px;
    }
    .btn:hover {
      background: #2980b9;
    }
    .abono-list li {
      margin-bottom: 5px;
    }
  </style>
</head>
<body>

  <div class="form-group">
    <label>Monto del Crédito:</label>
    <input type="number" id="monto" placeholder="Ej. 100000" />
  </div>
  <div class="form-group">
    <label>Tasa de Interés Anual (%):</label>
    <input type="number" id="tasa" step="0.01" placeholder="Ej. 12.5" />
  </div>
  <div class="form-group">
    <label>Plazo (meses):</label>
    <input type="number" id="plazo" placeholder="Ej. 36" />
  </div>

  <div class="abonos">
    <h3>Abonos Extraordinarios</h3>
    <label>Mes del Abono:</label>
    <input type="number" id="mesAbono" placeholder="Ej. 5" />
    <label>Monto del Abono:</label>
    <input type="number" id="montoAbono" placeholder="Ej. 10000" />
    <button class="btn" onclick="agregarAbono()">Agregar Abono</button>
    <ul id="listaAbonos" class="abono-list"></ul>
  </div>

  <button class="btn" onclick="calcularAmortizacion()">Calcular Amortización</button>

  <div class="resumen" id="resumen"></div>

  <div id="tabla"></div>

  <button class="btn" onclick="descargarExcel()">📥 Descargar Excel</button>
  <button class="btn" onclick="descargarPDF()">📄 Descargar PDF</button>

  <script>
    let abonos = [];

    // Función para formatear números como moneda mexicana
    function formatoMoneda(num) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(num);
    }

    function agregarAbono() {
      const mes = parseInt(document.getElementById('mesAbono').value);
      const monto = parseFloat(document.getElementById('montoAbono').value);
      if (mes && monto && mes > 0 && monto > 0) {
        abonos.push({ mes, monto });
        mostrarAbonos();
        document.getElementById('mesAbono').value = '';
        document.getElementById('montoAbono').value = '';
      }
    }

    function eliminarAbono(index) {
      abonos.splice(index, 1);
      mostrarAbonos();
    }

    function mostrarAbonos() {
      const lista = document.getElementById('listaAbonos');
      lista.innerHTML = '';
      abonos.forEach((abono, index) => {
        const li = document.createElement('li');
        li.innerHTML = `Mes ${abono.mes}: ${formatoMoneda(abono.monto)} <button onclick="eliminarAbono(${index})">❌</button>`;
        lista.appendChild(li);
      });
    }

    let datosTabla = [];

    function calcularAmortizacion() {
      const montoInicial = parseFloat(document.getElementById('monto').value);
      const tasaAnual = parseFloat(document.getElementById('tasa').value);
      const plazo = parseInt(document.getElementById('plazo').value);

      const tasaMensual = tasaAnual / 12 / 100;
      const cuota = montoInicial * (tasaMensual / (1 - Math.pow(1 + tasaMensual, -plazo)));

      let saldo = montoInicial;
      let totalIntereses = 0;
      let totalCapital = 0;
      datosTabla = [];

      let tablaHTML = `
        <table>
          <thead>
            <tr>
              <th>Mes</th>
              <th>Pago Mensual</th>
              <th>Interés</th>
              <th>Capital</th>
              <th>Abono Extra</th>
              <th>Saldo</th>
            </tr>
          </thead>
          <tbody>
      `;

      let mes = 1;
      while (saldo > 0.01 && mes <= 600) {
        const interes = saldo * tasaMensual;
        let capital = cuota - interes;
        if (capital > saldo) capital = saldo;
        saldo -= capital;
        totalIntereses += interes;
        totalCapital += capital;

        const abonoExtra = abonos.find(a => a.mes === mes);
        let abono = 0;
        let abonoTexto = '-';
        if (abonoExtra) {
          abono = Math.min(abonoExtra.monto, saldo);
          saldo -= abono;
          totalCapital += abono;
          abonoTexto = formatoMoneda(abono);
        }

        datosTabla.push([mes, cuota, interes, capital, abono, saldo]);

        tablaHTML += `
          <tr>
            <td>${mes}</td>
            <td>${formatoMoneda(cuota)}</td>
            <td>${formatoMoneda(interes)}</td>
            <td>${formatoMoneda(capital)}</td>
            <td>${abonoTexto}</td>
            <td>${saldo > 0 ? formatoMoneda(saldo) : formatoMoneda(0)}</td>
          </tr>
        `;

        mes++;
        if (saldo <= 0.01) break;
      }

      tablaHTML += '</tbody></table>';
      document.getElementById('tabla').innerHTML = tablaHTML;

      document.getElementById('resumen').innerHTML = `
        <h3>Resumen Ejecutivo</h3>
        <p><strong>Plazo Liquidado:</strong> ${mes - 1} meses</p>
        <p><strong>Total Pagado:</strong> ${formatoMoneda(totalIntereses + totalCapital)}</p>
        <p><strong>Total Abonado a Capital:</strong> ${formatoMoneda(totalCapital)}</p>
        <p><strong>Total Intereses Pagados:</strong> ${formatoMoneda(totalIntereses)}</p>
      `;
    }

    function descargarExcel() {
      if (datosTabla.length === 0) return alert("Primero calcula la amortización.");

      const wb = XLSX.utils.book_new();
      const ws = XLSX.utils.aoa_to_sheet([
        ["Mes", "Pago Mensual", "Interés", "Capital", "Abono Extra", "Saldo"],
        ...datosTabla.map(fila => [
          fila[0],
          fila[1].toFixed(2),
          fila[2].toFixed(2),
          fila[3].toFixed(2),
          fila[4].toFixed(2),
          fila[5].toFixed(2)
        ])
      ]);
      XLSX.utils.book_append_sheet(wb, ws, "Amortización");
      XLSX.writeFile(wb, "tabla_amortizacion.xlsx");
    }

    async function descargarPDF() {
      if (datosTabla.length === 0) return alert("Primero calcula la amortización.");

      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'landscape',
        unit: 'mm',
        format: 'a4'
      });
      
      // Estilo formal para el PDF
      doc.setFont('helvetica', 'bold');
      doc.setFontSize(16);
      doc.setTextColor(0, 51, 102); // Azul oscuro
      doc.text("Reporte de Amortización de Crédito", 148, 15, { align: 'center' });
      
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.setFont('helvetica', 'normal');
      
      // Resumen ejecutivo
      doc.setFont('helvetica', 'bold');
      doc.text("Resumen Ejecutivo", 20, 30);
      doc.setFont('helvetica', 'normal');
      
      const resumen = document.getElementById("resumen").innerText.split("\n");
      resumen.forEach((line, i) => {
        doc.text(line, 20, 40 + i * 6);
      });
      
      // Tabla de amortización
      doc.setFont('helvetica', 'bold');
      doc.text("Tabla de Amortización Detallada", 20, 70);
      doc.setFont('helvetica', 'normal');
      
      // Encabezados de tabla
      const headers = ["Mes", "Pago Mensual", "Interés", "Capital", "Abono Extra", "Saldo"];
      let x = 20;
      const columnWidths = [15, 30, 30, 30, 30, 30];
      
      doc.setFillColor(0, 51, 102);
      doc.setTextColor(255, 255, 255);
      doc.setFont('helvetica', 'bold');
      
      headers.forEach((header, i) => {
        doc.rect(x, 75, columnWidths[i], 8, 'F');
        doc.text(header, x + 2, 80);
        x += columnWidths[i];
      });
      
      // Filas de datos
      doc.setFont('helvetica', 'normal');
      doc.setTextColor(0, 0, 0);
      let y = 85;
      
      datosTabla.forEach((fila, i) => {
        if (y > 190) {
          doc.addPage('landscape');
          y = 20;
          // Volver a dibujar encabezados en nueva página
          x = 20;
          doc.setFillColor(0, 51, 102);
          doc.setTextColor(255, 255, 255);
          doc.setFont('helvetica', 'bold');
          headers.forEach((header, i) => {
            doc.rect(x, y, columnWidths[i], 8, 'F');
            doc.text(header, x + 2, y + 5);
            x += columnWidths[i];
          });
          doc.setFont('helvetica', 'normal');
          doc.setTextColor(0, 0, 0);
          y += 15;
        }
        
        x = 20;
        fila.forEach((valor, j) => {
          const formattedValue = j === 0 ? valor : formatoMoneda(valor);
          doc.text(formattedValue.toString(), x + 2, y + 5);
          x += columnWidths[j];
        });
        y += 7;
      });
      
      // Pie de página profesional
      const fecha = new Date().toLocaleDateString('es-MX');
      doc.setFontSize(10);
      doc.setTextColor(100, 100, 100);
      doc.text(`Generado el ${fecha}`, 270, 200, { align: 'right' });
      
      doc.save("reporte_amortizacion.pdf");
    }
  </script>
</body>
</html>
