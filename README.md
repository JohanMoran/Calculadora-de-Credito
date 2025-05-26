<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Calculadora de Amortización</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 2rem; background: #f7f7f7; }
    input, button, select { padding: 0.5rem; margin: 0.5rem; }
    table { width: 100%; border-collapse: collapse; margin-top: 1rem; background: white; }
    th, td { border: 1px solid #ccc; padding: 0.5rem; text-align: right; }
    th { background-color: #eee; }
    .section { background: white; padding: 1rem; margin-top: 1rem; border-radius: 0.5rem; box-shadow: 0 0 5px rgba(0,0,0,0.1); }
  </style>
</head>
<body>
  <h1>Calculadora de Amortización con Abonos a Capital</h1>

  <div class="section">
    <label>Monto del Crédito: $<input type="number" id="monto" /></label><br>
    <label>Tasa de Interés Anual (%): <input type="number" id="tasa" step="0.01" /></label><br>
    <label>Plazo (meses): <input type="number" id="plazo" /></label><br>
  </div>

  <div class="section">
    <h3>Abonos Extraordinarios a Capital</h3>
    <label>Mes del abono (ej: 3): <input type="number" id="mesAbono" min="1" /></label>
    <label>Monto del abono: $<input type="number" id="montoAbono" /></label>
    <button onclick="agregarAbono()">Agregar Abono</button>
    <ul id="listaAbonos"></ul>
  </div>

  <button onclick="calcularAmortizacion()">Calcular</button>

  <div id="resumen" class="section"></div>
  <div id="tabla" class="section"></div>

  <script>
    let abonos = [];

    function agregarAbono() {
      const mes = parseInt(document.getElementById('mesAbono').value);
      const monto = parseFloat(document.getElementById('montoAbono').value);

      if (mes && monto) {
        abonos.push({ mes, monto });
        mostrarAbonos();
      }
    }

    function mostrarAbonos() {
      const lista = document.getElementById('listaAbonos');
      lista.innerHTML = '';
      abonos.forEach((abono, index) => {
        const li = document.createElement('li');
        li.textContent = `Mes ${abono.mes}: $${abono.monto.toFixed(2)}`;
        lista.appendChild(li);
      });
    }

    function calcularAmortizacion() {
      const montoInicial = parseFloat(document.getElementById('monto').value);
      const tasaAnual = parseFloat(document.getElementById('tasa').value);
      const plazo = parseInt(document.getElementById('plazo').value);

      const tasaMensual = tasaAnual / 12 / 100;
      const cuota = montoInicial * (tasaMensual / (1 - Math.pow(1 + tasaMensual, -plazo)));

      let saldo = montoInicial;
      let totalIntereses = 0;
      let totalCapital = 0;
      let tablaHTML = `
        <table>
          <thead>
            <tr>
              <th>Mes</th>
              <th>Pago Mensual</th>
              <th>Interés</th>
              <th>Capital</th>
              <th>Abono Extra</th>
              <th>Saldo Restante</th>
            </tr>
          </thead>
          <tbody>
      `;

      let mes = 1;

      while (saldo > 0.01 && mes <= 600) { // límite arbitrario por seguridad
        const interes = saldo * tasaMensual;
        let capital = cuota - interes;

        // Si la cuota es mayor al saldo restante, ajusta
        if (capital > saldo) {
          capital = saldo;
        }

        saldo -= capital;
        totalIntereses += interes;
        totalCapital += capital;

        // Aplica abono extraordinario si corresponde
        const abonoExtra = abonos.find(a => a.mes === mes);
        let abonoTexto = '';
        if (abonoExtra) {
          let abono = abonoExtra.monto;
          if (abono > saldo) abono = saldo;
          saldo -= abono;
          totalCapital += abono;
          abonoTexto = `$${abono.toFixed(2)}`;
        } else {
          abonoTexto = '-';
        }

        tablaHTML += `
          <tr>
            <td>${mes}</td>
            <td>$${cuota.toFixed(2)}</td>
            <td>$${interes.toFixed(2)}</td>
            <td>$${capital.toFixed(2)}</td>
            <td>${abonoTexto}</td>
            <td>$${saldo > 0 ? saldo.toFixed(2) : '0.00'}</td>
          </tr>
        `;

        mes++;
        if (saldo <= 0.01) break;
      }

      tablaHTML += '</tbody></table>';
      document.getElementById('tabla').innerHTML = tablaHTML;

      // Resumen
      const resumenHTML = `
        <h3>Resumen</h3>
        <p><strong>Plazo liquidado:</strong> ${mes - 1} meses</p>
        <p><strong>Total pagado:</strong> $${(totalIntereses + totalCapital).toFixed(2)}</p>
        <p><strong>Total intereses pagados:</strong> $${totalIntereses.toFixed(2)}</p>
        <p><strong>Total abonado a capital:</strong> $${totalCapital.toFixed(2)}</p>
      `;
      document.getElementById('resumen').innerHTML = resumenHTML;
    }
  </script>
</body>
</html>
