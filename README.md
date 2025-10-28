<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Cotizador - EMPRESA CUNDO</title>
<style>
/* ... usar el mismo CSS que ya tienes ... */
</style>
</head>
<body>
  <div class="cotizador-container">
    <header class="cotizador-header">
      <div class="empresa-nombre">EMPRESA CUNDO</div>
      <div class="numero-cotizacion">COTIZACIÓN N°: <span id="numeroCotizacion">CO100500</span></div>
    </header>

    <div class="botones-superiores">
      <button class="btn btn-articulos" onclick="abrirArticulos()">ARTÍCULOS</button>
      <button class="btn btn-pdf" onclick="generarPDF()">PDF</button>
      <button class="btn btn-buscar" onclick="mostrarCotizaciones()">COTIZACIONES</button>
    </div>
    <!-- ... El resto de tu HTML de cliente/productos/totales/modales ... -->
    <!-- ... igual que siempre ... -->
    <div id="modalCotizaciones">
      <div>
        <button onclick="cerrarCotizaciones()" style="position:absolute;top:15px;right:20px;background:#e74c3c;color:white;border:none;border-radius:4px;padding:5px 10px;cursor:pointer;font-size:18px;font-weight:bold;">×</button>
        <h2 style="margin-bottom:15px;text-transform:uppercase;">COTIZACIONES EMITIDAS</h2>
        <div id="listaCotizaciones"></div>
      </div>
    </div>
  </div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
<script>
// ... El resto de tus clases y lógica cliente/prods/artículos igual a antes ...

let cotizacionesEmitidas = JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]');

// --- FUNCIONES DE COTIZACIÓN ---
function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones');
  const cont = document.getElementById('listaCotizaciones');
  
  if (cotizacionesEmitidas.length === 0) {
    cont.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 20px;">NO HAY COTIZACIONES EMITIDAS</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    cotizacionesEmitidas.forEach((c, index) => {
      html += `<tr>
        <td>${c.numero}</td>
        <td>${c.razonSocial}</td>
        <td style="text-align:right;">$${parseFloat(c.neto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td style="text-align:center;">
          <button class="btn-ver" onclick="verCotizacion(${index})">VER PDF</button>
          <button class="btn-editar-cot" onclick="editarCotizacionGuardada(${index})">EDITAR</button>
          <button class="btn-eliminar" onclick="borrarCotizacion(${index})">BORRAR</button>
        </td>
      </tr>`;
    });
    html += '</tbody></table>';
    cont.innerHTML = html;
  }
  modal.style.display = 'block';
}

function verCotizacion(index) {
  const cotizacion = cotizacionesEmitidas[index];
  if (!cotizacion) { alert('COTIZACIÓN NO ENCONTRADA'); return; }
  generarPDFDocumento(cotizacion);
}

function editarCotizacionGuardada(index) {
  const cotizacion = cotizacionesEmitidas[index];
  if (!cotizacion) { alert('COTIZACIÓN NO ENCONTRADA'); return; }
  // Tu lógica de edición aquí
}

function borrarCotizacion(index) {
  if (!confirm('¿Está seguro que desea borrar esta cotización?')) return;
  cotizacionesEmitidas.splice(index, 1);
  // ------ LocalStorage ------
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  // ------ Firebase (solo si integra luego) ------
  // guardarCotizacionesEnFirebase();
  mostrarCotizaciones();
}

function cerrarCotizaciones() {
  document.getElementById('modalCotizaciones').style.display = 'none';
}

// ... El resto de tus funciones y lógica ...

</script>
</body>
</html>
