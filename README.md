<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Cotizador - EMPRESA CUNDO</title>
<style>
  /* ...mismos estilos de antes, agrega este snippet para los botones PDF y mostrar cotizaciones */
  .botones-superiores {
    display: flex;
    gap: 10px;
    margin-bottom: 15px;
  }
  .btn-pdf {
    background-color: #d35400;
    color: white;
  }
  .btn-pdf:hover {
    background-color: #e67e22;
  }
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

    <section class="seccion-cliente">
      <!-- sección para datos del cliente igual (igual que antes, omito por espacio) -->
    </section>
    <section class="seccion-productos">
      <h2 class="seccion-titulo">PRODUCTOS Y/O SERVICIOS</h2>
      <!-- busqueda y formulario producto igual -->
      <div id="tablaProductosContenedor">
        <div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>
      </div>
      <div class="resumen-totales" id="resumenTotales" style="display:none;">
        <div class="resumen-linea"><div>NETO</div><div id="totalNeto">$0.00</div></div>
        <div class="resumen-linea"><div>IVA (19%)</div><div id="totalIva">$0.00</div></div>
        <div class="resumen-linea total"><div>TOTAL</div><div id="totalGeneral">$0.00</div></div>
      </div>
    </section>
  </div>

  <!-- Modal Articulos, Modal edición articulo, iguales -->

  <div id="modalCotizaciones" style="display:none; position:fixed; top:0; left:0; width:100vw; height:100vh; background:rgba(0,0,0,0.5); z-index:10000; overflow:auto;">
    <div style="background:white; max-width:700px; margin:40px auto; padding:20px; border-radius:8px; position:relative;">
      <button onclick="cerrarCotizaciones()" style="position:absolute;top:15px;right:20px; background:#e74c3c; color:white; border:none; border-radius:4px; padding:5px 10px; cursor:pointer;">×</button>
      <h2 style="margin-bottom:15px; text-transform: uppercase;">COTIZACIONES EMITIDAS</h2>
      <div id="listaCotizaciones"></div>
    </div>
  </div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script>
// Instancia gestores y variables (igual que antes)
class GestorCotizaciones {
  constructor(){
    this.prefijo='CO';
    this.numeroBase=100500;
    this.cargarNumeroCotizacion();
  }
  cargarNumeroCotizacion(){
    const numeroGuardado=localStorage.getItem('ultimaCotizacion');
    if(numeroGuardado){this.numeroBase=parseInt(numeroGuardado);}
    this.actualizarDisplay();
  }
  obtenerNumeroActual(){return `${this.prefijo}${this.numeroBase}`;}
  siguienteCotizacion(){
    this.numeroBase++;
    localStorage.setItem('ultimaCotizacion', this.numeroBase);
    this.actualizarDisplay();
    return this.obtenerNumeroActual();
  }
  actualizarDisplay(){
    const el=document.getElementById('numeroCotizacion');
    if(el) el.textContent=this.obtenerNumeroActual();
  }
}
// Gestores Clientes y Productos igual (no se repite por brevedad; copia los anteriores)

const gestorCotizaciones=new GestorCotizaciones();
const gestorClientes=new GestorClientes();
const gestorProductos=new GestorProductos();

let clienteActual=null;
let modoEdicion=false;
let productosEnCotizacion=[];
let cotizacionesEmitidas = JSON.parse(localStorage.getItem('cotizacionesEmitidas')||'[]');

window.onload = actualizarTablaProductos; // Mostrar tabla al cargar si hay productos

// Funciones cliente y productos igual que antes => copiar la lógica previa sin cambio

// Agrega producto real a la cotización y refresca tabla y resumen
function agregarProductoACotizacion(codigo, producto){
  const existe = productosEnCotizacion.find(p => p.codigo === codigo);
  if(existe){
    existe.cantidad++;
    existe.total=(existe.valorNeto*existe.cantidad).toFixed(2);
  } else {
    productosEnCotizacion.push({
      codigo:producto.codigo,
      descripcion:producto.descripcion,
      cantidad:1,
      valorNeto:producto.valorNeto,
      total:producto.valorNeto.toFixed(2)
    });
  }
  actualizarTablaProductos();
}

// Actualiza tabla y resumen de totales
function actualizarTablaProductos(){
  const container=document.getElementById('tablaProductosContenedor');
  if(productosEnCotizacion.length===0){
    container.innerHTML='<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>';
    document.getElementById('resumenTotales').style.display='none';
    return;
  }
  let html=`<table><thead><tr>
    <th>CÓDIGO</th><th>DESCRIPCIÓN</th><th style="width:80px;">CANTIDAD</th><th style="width:120px;">VALOR NETO</th><th style="width:120px;">TOTAL</th><th style="width:80px;">ACCIÓN</th>
  </tr></thead><tbody>`;
  productosEnCotizacion.forEach((p,i)=>{
    html+=`<tr>
      <td>${p.codigo}</td>
      <td>${p.descripcion}</td>
      <td><input type="number" min="1" value="${p.cantidad}" onchange="actualizarCantidad(${i}, this.value)"></td>
      <td class="valor-numerico">$${parseFloat(p.valorNeto).toLocaleString('es-CL',{minimumFractionDigits:2, maximumFractionDigits:2})}</td>
      <td class="valor-numerico">$${parseFloat(p.total).toLocaleString('es-CL',{minimumFractionDigits:2, maximumFractionDigits:2})}</td>
      <td class="celda-acciones"><button class="btn-eliminar" onclick="eliminarProducto(${i})">ELIMINAR</button></td>
    </tr>`;
  });
  html+='</tbody></table>';
  container.innerHTML=html;
  actualizarResumenTotales();
}

function actualizarCantidad(i,cantidad){
  cantidad=parseInt(cantidad);
  if(isNaN(cantidad)||cantidad<1){alert('Cantidad inválida');actualizarTablaProductos(); return;}
  productosEnCotizacion[i].cantidad=cantidad;
  productosEnCotizacion[i].total=(productosEnCotizacion[i].valorNeto*cantidad).toFixed(2);
  actualizarTablaProductos();
}

function eliminarProducto(i){
  productosEnCotizacion.splice(i,1);
  actualizarTablaProductos();
}

function actualizarResumenTotales(){
  const neto = productosEnCotizacion.reduce((acc,p)=>acc+parseFloat(p.total),0);
  const iva = +(neto*0.19).toFixed(2);
  const total = +(neto+iva).toFixed(2);
  document.getElementById('totalNeto').textContent='$'+neto.toLocaleString('es-CL',{minimumFractionDigits:2, maximumFractionDigits:2});
  document.getElementById('totalIva').textContent='$'+iva.toLocaleString('es-CL',{minimumFractionDigits:2, maximumFractionDigits:2});
  document.getElementById('totalGeneral').textContent='$'+total.toLocaleString('es-CL',{minimumFractionDigits:2, maximumFractionDigits:2});
  document.getElementById('resumenTotales').style.display='block';
}

// PDF y Cotizaciones Emitidas:

async function generarPDF(){
  if(!clienteActual){
    alert('Debe ingresar y guardar los datos del cliente antes de generar la cotización.');
    return;
  }
  if(productosEnCotizacion.length===0){
    alert('Debe agregar al menos un producto o servicio para generar la cotización.');
    return;
  }

  // Avanzar correlativo y guardar en localStorage
  const numCotizacion = gestorCotizaciones.siguienteCotizacion();

  // Guardamos esta cotización emitida con datos básicos
  const neto = productosEnCotizacion.reduce((acc,p) => acc + parseFloat(p.total),0);
  cotizacionesEmitidas.push({
    numero: numCotizacion,
    razonSocial: clienteActual.razonSocial,
    neto: neto.toFixed(2),
    fecha: new Date().toISOString()
  });
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));

  // Generar pdf con jsPDF
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();

  doc.setFontSize(18);
  doc.text('COTIZACION', 14, 15);
  doc.setFontSize(12);
  doc.text(`EMPRESA CUNDO`, 14, 25);
  doc.text(`N°: ${numCotizacion}`, 160, 25);

  doc.setFontSize(14);
  doc.text('Datos del cliente', 14, 35);

  let y = 40;
  // Información cliente
  doc.setFontSize(10);
  doc.text(`RUT: ${clienteActual.rut}`, 14, y); y+=7;
  doc.text(`Razón Social: ${clienteActual.razonSocial}`, 14, y); y+=7;
  doc.text(`Giro: ${clienteActual.giro}`, 14, y); y+=7;
  doc.text(`Dirección: ${clienteActual.direccion}`, 14, y); y+=7;
  doc.text(`Contacto: ${clienteActual.nombreContacto}`, 14, y); y+=7;
  doc.text(`Celular: ${clienteActual.celular}`, 14, y); y+=7;
  doc.text(`Mail: ${clienteActual.mail}`, 14, y); y+=10;

  doc.setFontSize(14);
  doc.text('Productos y Servicios', 14, y); y+=7;

  // Tabla de productos
  const colWidths = [30, 60, 20, 30, 30];
  doc.autoTable({
    startY: y,
    head: [['CÓDIGO','DESCRIPCIÓN','CANTIDAD','VALOR NETO','TOTAL']],
    body: productosEnCotizacion.map(p => [p.codigo, p.descripcion, p.cantidad.toString(), `$${p.valorNeto.toFixed(2)}`, `$${p.total}`]),
    theme: 'grid',
    styles: { fontSize: 9, cellPadding: 2 },
    headStyles: { fillColor: [52,73,94] },
    columnStyles: {
      0: { cellWidth: colWidths[0] },
      1: { cellWidth: colWidths[1] },
      2: { cellWidth: colWidths[2], halign: 'center' },
      3: { cellWidth: colWidths[3], halign: 'right' },
      4: { cellWidth: colWidths[4], halign: 'right' },
    }
  });

  // Agregar Resumen Neto, IVA y Total
  let finalY = doc.lastAutoTable.finalY + 10;
  const netoStr = `$${neto.toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}`;
  const iva = +(neto*0.19).toFixed(2);
  const ivaStr = `$${iva.toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}`;
  const total = +(neto+iva).toFixed(2);
  const totalStr = `$${total.toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}`;

  doc.setFontSize(12);
  doc.text('Resumen', 150, finalY);
  doc.setFontSize(10);
  doc.text('Neto:', 150, finalY + 7);
  doc.text(netoStr, 190, finalY +7, { align: 'right' });
  doc.text('IVA (19%):', 150, finalY + 14);
  doc.text(ivaStr, 190, finalY +14, { align: 'right' });
  doc.text('Total:', 150, finalY + 21);
  doc.text(totalStr, 190, finalY +21, { align: 'right' });

  doc.save(`Cotizacion_${numCotizacion}.pdf`);
  alert('PDF generado y número de cotización actualizado.');
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones');
  const contenedor = document.getElementById('listaCotizaciones');
  if (cotizacionesEmitidas.length === 0) {
    contenedor.innerHTML = '<p style="text-transform:uppercase; text-align:center;">No hay cotizaciones emitidas aún.</p>';
  } else {
    let html = '<table style="width:100%; border-collapse: collapse;">'
      + '<thead><tr><th style="border:1px solid #ddd; padding:8px;">N° Cotización</th>'
      + '<th style="border:1px solid #ddd; padding:8px;">Razón Social</th>'
      + '<th style="border:1px solid #ddd; padding:8px; text-align:right;">Monto Neto</th></tr></thead><tbody>';
    cotizacionesEmitidas.forEach(c => {
      html += `<tr>
        <td style="border:1px solid #ddd; padding:8px;">${c.numero}</td>
        <td style="border:1px solid #ddd; padding:8px;">${c.razonSocial}</td>
        <td style="border:1px solid #ddd; padding:8px; text-align:right;">$${parseFloat(c.neto).toLocaleString('es-CL',{minimumFractionDigits:2, maximumFractionDigits:2})}</td>
      </tr>`;
    });
    html += '</tbody></table>';
    contenedor.innerHTML = html;
  }
  modal.style.display = 'block';
}
function cerrarCotizaciones() {
  document.getElementById('modalCotizaciones').style.display = 'none';
}
</script>
<!-- jsPDF necesita autoTable plugin -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
</body>
</html>
