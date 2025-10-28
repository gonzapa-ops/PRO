<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Cotizador - EMPRESA CUNDO</title>
<style>
  * {margin: 0; padding: 0; box-sizing: border-box;}
  body {font-family: 'Arial', sans-serif; background: #f5f5f5; padding: 20px;}
  .cotizador-container {max-width: 1200px; margin: auto; background: #fff; box-shadow: 0 0 10px rgba(0,0,0,0.1); padding: 30px;}
  .cotizador-header {background: #2c3e50; color: white; padding: 15px 30px; display: flex; justify-content: space-between; align-items: center; border-bottom: 3px solid #3498db; margin-bottom: 20px;}
  .empresa-nombre {font-weight: bold; font-size: 20px; letter-spacing: 1px;}
  .numero-cotizacion {font-weight: bold; color: #3498db;}
  .seccion-titulo {font-weight: bold; font-size: 18px; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 2px solid #3498db; text-transform: uppercase;}
  table {width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 13px;}
  th, td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase;}
  th {background: #34495e; color: white; font-weight: bold;}
  tr:nth-child(even) {background: #f9f9f9;}
  tr:hover {background: #f0f0f0;}
  .valor-numerico {text-align: right; font-weight: bold;}
  .celda-acciones {text-align: center;}
  button {cursor: pointer; border: none; border-radius: 4px; padding: 8px 15px; font-weight: bold; text-transform: uppercase;}
  .btn-buscar {background: #3498db; color: white;}
  .btn-buscar:hover {background: #2980b9;}
  .btn-limpiar {background: #95a5a6; color: white;}
  .btn-limpiar:hover {background: #7f8c8d;}
  .btn-eliminar {background: #e74c3c; color: white; font-size: 12px; padding: 5px 10px;}
  .btn-eliminar:hover {background: #c0392b;}
  .btn-articulos {background: #16a085; color: white;}
  .btn-articulos:hover {background: #138d75;}
  .btn-pdf {background-color: #d35400; color: white;}
  .btn-pdf:hover {background-color: #e67e22;}
  .btn-agregar {background: #27ae60; color: white; padding: 8px 15px; font-size: 13px;}
  .btn-agregar:hover {background: #229954;}
  .btn-editar {background: #f39c12; color: white; padding: 8px 15px; font-size: 13px;}
  .btn-editar:hover {background: #e67e22;}
  .btn-cancelar {background: #e74c3c; color: white; padding: 8px 15px; font-size: 13px; margin-left: 10px;}
  .btn-cancelar:hover {background: #c0392b;}
  .formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none; animation: fadeIn 0.3s;}
  .formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
  @keyframes fadeIn {from {opacity: 0;} to {opacity: 1;}}
  .campo-grupo {margin-bottom: 15px;}
  .campo-grupo label {display: block; font-weight: bold; color: #2c3e50; margin-bottom: 5px; font-size: 14px; text-transform: uppercase;}
  .campo-grupo input {width: 100%; padding: 10px; font-size: 14px; border: 2px solid #ddd; border-radius: 4px; transition: border-color 0.3s; text-transform: uppercase;}
  .campo-grupo input:focus {outline: none; border-color: #3498db;}
  .campo-grupo input:disabled {background-color: #ecf0f1; cursor: not-allowed;}
  .campo-grupo input.link-articulo, #linkProducto {text-transform: lowercase;}
  .fila-campos {display: grid; grid-template-columns: 1fr 1fr; gap: 15px;}
  .mensaje {padding: 10px 15px; border-radius: 4px; margin-bottom: 15px; font-size: 14px; text-transform: uppercase;}
  .mensaje-exito {background-color: #d4edda; color: #155724; border: 1px solid #c3e6cb;}
  .mensaje-error {background-color: #f8d7da; color: #721c24; border: 1px solid #f5c6cb;}
  .mensaje-info {background-color: #d1ecf1; color: #0c5460; border: 1px solid #bee5eb;}
  .resumen-cliente {display: none; background-color: #e8f5e9; padding: 15px; border-radius: 4px; border-left: 4px solid #4caf50; position: relative;}
  .resumen-cliente.activo {display: block;}
  .resumen-cliente h4 {color: #2c3e50; margin-bottom: 10px; text-transform: uppercase;}
  .resumen-cliente p {margin: 5px 0; font-size: 14px; color: #555; text-transform: uppercase;}
  .resumen-cliente strong {color: #2c3e50; text-transform: uppercase;}
  .botones-resumen {margin-top: 15px; display: flex; gap: 10px;}
  .botones-formulario {display: flex; gap: 10px; margin-top: 10px;}
  .seccion-cliente {margin-bottom: 30px; border: 1px solid #ddd; border-radius: 5px; padding: 20px; background-color: #fafafa;}
  .busqueda-rut {display: flex; gap: 10px; margin-bottom: 20px; align-items: center;}
  .busqueda-rut input {flex: 1; padding: 10px; font-size: 16px; border: 2px solid #ddd; border-radius: 4px; transition: border-color 0.3s; text-transform: uppercase;}
  .busqueda-rut input:focus {outline: none; border-color: #3498db;}
  .seccion-productos {margin-bottom: 30px; border: 1px solid #ddd; border-radius: 5px; padding: 20px; background-color: #fafafa;}
  .busqueda-producto {display: flex; gap: 10px; margin-bottom: 20px; align-items: center;}
  .busqueda-producto input {flex: 1; padding: 10px; font-size: 16px; border: 2px solid #ddd; border-radius: 4px; transition: border-color 0.3s; text-transform: uppercase;}
  .busqueda-producto input:focus {outline: none; border-color: #3498db;}
  .tabla-productos, .tabla-articulos {width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 13px;}
  .tabla-productos th, .tabla-articulos th {padding: 10px; text-align: left; font-weight: bold; text-transform: uppercase; border: 1px solid #ddd;}
  .tabla-productos td, .tabla-articulos td {padding: 10px; border: 1px solid #ddd; text-transform: uppercase;}
  .tabla-productos tbody tr:nth-child(even), .tabla-articulos tbody tr:nth-child(even) {background-color: #f9f9f9;}
  .tabla-productos tbody tr:hover, .tabla-articulos tbody tr:hover {background-color: #f0f0f0;}
  .tabla-productos input, .tabla-articulos input {width: 100%; padding: 6px; border: 1px solid #ddd; border-radius: 3px; text-transform: uppercase; font-size: 12px;}
  .tabla-productos input:focus, .tabla-articulos input:focus {outline: none; border-color: #3498db;}
  .tabla-productos input[type="number"], .tabla-articulos input[type="number"] {text-align: right;}
  .btn-pequeno {padding: 5px 10px; font-size: 12px;}
  .formulario-producto h4, .formulario-editar-articulo h4 {text-transform: uppercase; color: #856404; margin-bottom: 15px; background: #fff3cd; padding: 15px; border-radius: 4px; border-left: 4px solid #f39c12;}
  .fila-campos-producto {display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;}
  .fila-campos-producto-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 15px;}
  .fila-campos-producto .campo-grupo, .fila-campos-producto-tres .campo-grupo {margin-bottom: 0;}
  .botones-producto {display: flex; gap: 10px; margin-top: 10px;}
  .alerta-sin-productos {background-color: #e8f4f8; padding: 15px; border-radius: 4px; border-left: 4px solid #3498db; text-align: center; color: #2c3e50; text-transform: uppercase; font-weight: bold;}
  #modalArticulos {display:none; position:fixed; top:0; left:0; width:100vw; height:100vh; background:rgba(0,0,0,0.5); z-index:9999; overflow: auto;}
  .articulos-content {background:white; border-radius:8px; margin:40px auto; padding:25px; max-width:1000px; position:relative;}
  .modal-titulo {font-size:22px; font-weight:bold; margin-bottom:20px; text-transform:uppercase;}
  .btn-cerrar-modal {position:absolute; top:14px; right:25px; background:#e74c3c; color:white; font-size:18px; border:none; border-radius:4px; cursor:pointer; padding:4px 10px;}
  .botones-superiores {display:flex; gap:10px; margin-bottom:20px; flex-wrap: wrap;}
  #modalCotizaciones {display:none; position:fixed; top:0; left:0; width:100vw; height:100vh; background:rgba(0,0,0,0.5); z-index:10000; overflow:auto;}
  #modalCotizaciones > div {background:white; max-width:700px; margin:40px auto; padding:20px; border-radius:8px; position:relative;}
  .resumen-totales {
    max-width: 400px;
    margin-left: auto;
    border-top: 3px solid #3498db;
    padding-top: 15px;
    text-transform: uppercase;
  }
  .resumen-linea {
    display: flex;
    justify-content: space-between;
    margin: 5px 0;
    font-weight: bold;
    font-size: 14px;
  }
  .resumen-linea.total {
    font-size: 18px;
    color: #2c3e50;
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
      <h2 class="seccion-titulo">DATOS DEL CLIENTE</h2>
      <div class="busqueda-rut">
        <input type="text" id="inputRut" placeholder="INGRESE RUT (EJ: 78070615-7)" maxlength="12" />
        <button class="btn btn-buscar" onclick="buscarCliente()">BUSCAR</button>
        <button class="btn btn-limpiar" onclick="limpiarFormulario()">LIMPIAR</button>
      </div>
      <div id="mensaje"></div>
      <div id="resumenCliente" class="resumen-cliente"></div>
      <div id="formularioCliente" class="formulario-cliente">
        <div class="campo-grupo"><label for="rut">RUT *</label><input type="text" id="rut" disabled /></div>
        <div class="campo-grupo"><label for="razonSocial">RAZÓN SOCIAL *</label><input type="text" id="razonSocial" required /></div>
        <div class="campo-grupo"><label for="giro">GIRO *</label><input type="text" id="giro" required /></div>
        <div class="campo-grupo"><label for="direccion">DIRECCIÓN DE FACTURACIÓN *</label><input type="text" id="direccion" required /></div>
        <div class="fila-campos">
          <div class="campo-grupo"><label for="nombreContacto">NOMBRE DE CONTACTO *</label><input type="text" id="nombreContacto" required /></div>
          <div class="campo-grupo"><label for="celular">CELULAR *</label><input type="tel" id="celular" placeholder="+56912345678" required /></div>
        </div>
        <div class="campo-grupo"><label for="mail">MAIL *</label><input type="email" id="mail" required /></div>
        <div class="botones-formulario">
          <button class="btn btn-buscar" onclick="guardarCliente()"><span id="textoBotonGuardar">GUARDAR CLIENTE</span></button>
          <button class="btn btn-cancelar" id="btnCancelarEdicion" onclick="cancelarEdicion()" style="display: none;">CANCELAR</button>
        </div>
      </div>
    </section>

    <section class="seccion-productos">
      <h2 class="seccion-titulo">PRODUCTOS Y/O SERVICIOS</h2>
      <div class="busqueda-producto">
        <input type="text" id="inputCodigoProducto" placeholder="INGRESE CÓDIGO DEL PRODUCTO O SERVICIO" maxlength="20" />
        <button class="btn btn-buscar" onclick="buscarProducto()">BUSCAR</button>
      </div>
      <div id="mensajeProducto"></div>
      <div id="formularioProducto" class="formulario-producto">
        <h4>CREAR NUEVO PRODUCTO/SERVICIO</h4>
        <div class="fila-campos-producto">
          <div class="campo-grupo"><label for="codigoProducto">CÓDIGO *</label><input type="text" id="codigoProducto" disabled /></div>
          <div class="campo-grupo"><label for="descripcionProducto">DESCRIPCIÓN *</label><input type="text" id="descripcionProducto" required /></div>
        </div>
        <div class="fila-campos-producto-tres">
          <div class="campo-grupo"><label for="costoProducto">COSTO *</label><input type="number" id="costoProducto" min="0" step="0.01" required /></div>
          <div class="campo-grupo"><label for="porcentajeProducto">PORCENTAJE (%) *</label><input type="number" id="porcentajeProducto" min="0" step="0.01" value="0" required /></div>
          <div class="campo-grupo"><label for="valorNetoProducto">VALOR NETO (AUTOCALCULADO)</label><input type="number" id="valorNetoProducto" disabled /></div>
        </div>
        <div class="fila-campos-producto">
          <div class="campo-grupo"><label for="proveedorProducto">PROVEEDOR *</label><input type="text" id="proveedorProducto" required /></div>
          <div class="campo-grupo"><label for="linkProducto">LINK *</label><input type="text" id="linkProducto" class="link-articulo" required /></div>
        </div>
        <div class="botones-producto">
          <button class="btn btn-agregar" onclick="guardarProducto()">GUARDAR PRODUCTO</button>
          <button class="btn btn-cancelar btn-pequeno" onclick="cancelarProducto()">CANCELAR</button>
        </div>
      </div>
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

  <div id="modalArticulos">
    <div class="articulos-content">
      <button class="btn-cerrar-modal" onclick="cerrarArticulos()">×</button>
      <div class="modal-titulo">ARTÍCULOS (PRODUCTOS Y SERVICIOS REGISTRADOS)</div>
      <div id="tablaArticulosContenedor"></div>
      <div id="formularioEditarArticulo" class="formulario-editar-articulo"></div>
    </div>
  </div>

  <div id="modalCotizaciones">
    <div>
      <button onclick="cerrarCotizaciones()" style="position:absolute;top:15px;right:20px;background:#e74c3c;color:white;border:none;border-radius:4px;padding:5px 10px;cursor:pointer;">×</button>
      <h2 style="margin-bottom:15px;text-transform:uppercase;">COTIZACIONES EMITIDAS</h2>
      <div id="listaCotizaciones"></div>
    </div>
  </div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
<script>
class GestorCotizaciones {
  constructor() {this.prefijo = 'CO'; this.numeroBase = 100500; this.cargarNumeroCotizacion();}
  cargarNumeroCotizacion() {const numeroGuardado = localStorage.getItem('ultimaCotizacion'); if (numeroGuardado) this.numeroBase = parseInt(numeroGuardado); this.actualizarDisplay();}
  obtenerNumeroActual() {return `${this.prefijo}${this.numeroBase}`;}
  siguienteCotizacion() {this.numeroBase++; localStorage.setItem('ultimaCotizacion', this.numeroBase); this.actualizarDisplay(); return this.obtenerNumeroActual();}
  actualizarDisplay() {const elemento = document.getElementById('numeroCotizacion'); if (elemento) elemento.textContent = this.obtenerNumeroActual();}
}
class GestorClientes {
  constructor() {this.clientes = this.cargarClientes();}
  cargarClientes() {const clientesGuardados = localStorage.getItem('clientes'); return clientesGuardados ? JSON.parse(clientesGuardados) : {};}
  guardarClientes() {localStorage.setItem('clientes', JSON.stringify(this.clientes));}
  buscarPorRut(rut) {return this.clientes[rut] || null;}
  agregarCliente(rut, datos) {this.clientes[rut] = datos; this.guardarClientes();}
  actualizarCliente(rut, datos) {this.clientes[rut] = datos; this.guardarClientes();}
}
class GestorProductos {
  constructor() {this.productos = this.cargarProductos();}
  cargarProductos() {const productosGuardados = localStorage.getItem('productos'); return productosGuardados ? JSON.parse(productosGuardados) : {};}
  guardarProductos() {localStorage.setItem('productos', JSON.stringify(this.productos));}
  buscarPorCodigo(codigo) {return this.productos[codigo] || null;}
  agregarProducto(codigo, datos) {this.productos[codigo] = datos; this.guardarProductos();}
  actualizarProducto(codigo, datos) {this.productos[codigo] = datos; this.guardarProductos();}
  obtenerTodos() {return this.productos;}
  eliminarProducto(codigo) {delete this.productos[codigo]; this.guardarProductos();}
}
const gestorCotizaciones = new GestorCotizaciones();
const gestorClientes = new GestorClientes();
const gestorProductos = new GestorProductos();
let clienteActual = null;
let modoEdicion = false;
let productosEnCotizacion = [];
let cotizacionesEmitidas = JSON.parse(localStorage.getItem('cotizacionesEmitidas')||'[]');
let articuloEdicion = null;

document.getElementById('inputRut').addEventListener('input', e => {let valor = e.target.value.replace(/[^0-9kK]/g, ''); if (valor.length > 1) valor = valor.slice(0, -1) + '-' + valor.slice(-1); e.target.value = valor.toUpperCase();});
function buscarCliente() {const rut = document.getElementById('inputRut').value.trim(); if (!rut) return mostrarMensaje('POR FAVOR INGRESE UN RUT', 'error'); if (!validarRut(rut)) return mostrarMensaje('EL FORMATO DEL RUT NO ES VÁLIDO', 'error'); const cliente = gestorClientes.buscarPorRut(rut); if (cliente) {clienteActual = cliente; modoEdicion = false; mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito'); mostrarResumenCliente(cliente); document.getElementById('formularioCliente').classList.remove('activo');} else {mostrarMensaje('CLIENTE NO ENCONTRADO. POR FAVOR COMPLETE LOS DATOS PARA CREAR EL CLIENTE.', 'info'); document.getElementById('resumenCliente').classList.remove('activo'); document.getElementById('formularioCliente').classList.add('activo'); document.getElementById('rut').value = rut; document.getElementById('textoBotonGuardar').textContent = 'GUARDAR CLIENTE'; document.getElementById('btnCancelarEdicion').style.display = 'none'; limpiarCamposFormulario(); modoEdicion = false;}}
function validarRut(rut) {return /^[0-9]{7,8}-[0-9kK]$/.test(rut);}
function mostrarMensaje(texto, tipo) {const mensaje = document.getElementById('mensaje'); mensaje.className = `mensaje mensaje-${tipo}`; mensaje.textContent = texto; mensaje.style.display = 'block';}
function mostrarResumenCliente(cliente) {const resumen = document.getElementById('resumenCliente'); resumen.className = 'resumen-cliente activo'; resumen.innerHTML = `<h4>✓ CLIENTE REGISTRADO</h4><p><strong>RUT:</strong> ${cliente.rut}</p><p><strong>RAZÓN SOCIAL:</strong> ${cliente.razonSocial}</p><p><strong>GIRO:</strong> ${cliente.giro}</p><p><strong>DIRECCIÓN:</strong> ${cliente.direccion}</p><p><strong>CONTACTO:</strong> ${cliente.nombreContacto}</p><p><strong>CELULAR:</strong> ${cliente.celular}</p><p><strong>MAIL:</strong> ${cliente.mail}</p><div class="botones-resumen"><button class="btn btn-editar" onclick="editarCliente()">✏️ EDITAR DATOS</button></div>`;}
function editarCliente() {if (!clienteActual) return; modoEdicion = true; document.getElementById('rut').value = clienteActual.rut; document.getElementById('razonSocial').value = clienteActual.razonSocial; document.getElementById('giro').value = clienteActual.giro; document.getElementById('direccion').value = clienteActual.direccion; document.getElementById('nombreContacto').value = clienteActual.nombreContacto; document.getElementById('celular').value = clienteActual.celular; document.getElementById('mail').value = clienteActual.mail; document.getElementById('resumenCliente').classList.remove('activo'); document.getElementById('formularioCliente').classList.add('activo'); document.getElementById('textoBotonGuardar').textContent = 'GUARDAR CAMBIOS'; document.getElementById('btnCancelarEdicion').style.display = 'inline-block'; mostrarMensaje('MODO DE EDICIÓN ACTIVADO. MODIFIQUE LOS CAMPOS NECESARIOS.', 'info');}
function cancelarEdicion() {if (!clienteActual) return; modoEdicion = false; document.getElementById('formularioCliente').classList.remove('activo'); mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito'); mostrarResumenCliente(clienteActual);}
function guardarCliente() {const rut = document.getElementById('rut').value; const razonSocial = document.getElementById('razonSocial').value.trim(); const giro = document.getElementById('giro').value.trim(); const direccion = document.getElementById('direccion').value.trim(); const nombreContacto = document.getElementById('nombreContacto').value.trim(); const celular = document.getElementById('celular').value.trim(); const mail = document.getElementById('mail').value.trim(); if (!razonSocial || !giro || !direccion || !nombreContacto || !celular || !mail) return mostrarMensaje('POR FAVOR COMPLETE TODOS LOS CAMPOS OBLIGATORIOS (*)', 'error'); if (!validarEmail(mail)) return mostrarMensaje('EL FORMATO DEL CORREO ELECTRÓNICO NO ES VÁLIDO', 'error'); const cliente = {rut, razonSocial: razonSocial.toUpperCase(), giro: giro.toUpperCase(), direccion: direccion.toUpperCase(), nombreContacto: nombreContacto.toUpperCase(), celular, mail: mail.toUpperCase()}; if (modoEdicion) gestorClientes.actualizarCliente(rut, cliente); else gestorClientes.agregarCliente(rut, cliente); mostrarMensaje(modoEdicion ? 'DATOS DEL CLIENTE ACTUALIZADOS EXITOSAMENTE' : 'CLIENTE GUARDADO EXITOSAMENTE', 'exito'); clienteActual = cliente; modoEdicion = false; mostrarResumenCliente(cliente); document.getElementById('formularioCliente').classList.remove('activo');}
function validarEmail(email) {return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);}
function limpiarFormulario() {document.getElementById('inputRut').value = ''; document.getElementById('mensaje').style.display = 'none'; document.getElementById('resumenCliente').classList.remove('activo'); document.getElementById('formularioCliente').classList.remove('activo'); document.getElementById('btnCancelarEdicion').style.display = 'none'; limpiarCamposFormulario(); clienteActual = null; modoEdicion = false;}
function limpiarCamposFormulario() {document.getElementById('razonSocial').value = ''; document.getElementById('giro').value = ''; document.getElementById('direccion').value = ''; document.getElementById('nombreContacto').value = ''; document.getElementById('celular').value = ''; document.getElementById('mail').value = '';}

document.getElementById('inputCodigoProducto').addEventListener('input', e => {e.target.value = e.target.value.toUpperCase();});
document.getElementById('linkProducto').addEventListener('input', e => {e.target.value = e.target.value.toLowerCase();});
document.getElementById('costoProducto').addEventListener('change', calcularValorNeto);
document.getElementById('porcentajeProducto').addEventListener('change', calcularValorNeto);
function calcularValorNeto() {const costo = parseFloat(document.getElementById('costoProducto').value) || 0; const porcentaje = parseFloat(document.getElementById('porcentajeProducto').value) || 0; const valorNeto = costo + (costo * (porcentaje / 100)); document.getElementById('valorNetoProducto').value = valorNeto.toFixed(2);}
function buscarProducto() {const codigo = document.getElementById('inputCodigoProducto').value.trim(); if (!codigo) return mostrarMensajeProducto('POR FAVOR INGRESE UN CÓDIGO', 'error'); const producto = gestorProductos.buscarPorCodigo(codigo); if (producto) {mostrarMensajeProducto('PRODUCTO ENCONTRADO. AGREGUE CANTIDAD.', 'exito'); document.getElementById('formularioProducto').classList.remove('activo'); agregarProductoACotizacion(codigo, producto); document.getElementById('inputCodigoProducto').value = '';} else {mostrarMensajeProducto('PRODUCTO NO ENCONTRADO. POR FAVOR COMPLETE LOS DATOS PARA CREAR EL PRODUCTO.', 'info'); document.getElementById('formularioProducto').classList.add('activo'); document.getElementById('codigoProducto').value = codigo; document.getElementById('descripcionProducto').value = ''; document.getElementById('costoProducto').value = ''; document.getElementById('porcentajeProducto').value = '0'; document.getElementById('valorNetoProducto').value = ''; document.getElementById('proveedorProducto').value = ''; document.getElementById('linkProducto').value = '';}}
function mostrarMensajeProducto(texto, tipo) {const mensaje = document.getElementById('mensajeProducto'); mensaje.className = `mensaje mensaje-${tipo}`; mensaje.textContent = texto; mensaje.style.display = 'block';}
function guardarProducto() {const codigo = document.getElementById('codigoProducto').value.trim(); const descripcion = document.getElementById('descripcionProducto').value.trim(); const costo = parseFloat(document.getElementById('costoProducto').value); const porcentaje = parseFloat(document.getElementById('porcentajeProducto').value) || 0; const valorNeto = parseFloat(document.getElementById('valorNetoProducto').value); const proveedor = document.getElementById('proveedorProducto').value.trim(); const link = document.getElementById('linkProducto').value.trim().toLowerCase(); if (!codigo || !descripcion || isNaN(costo) || costo <= 0 || !proveedor || !link) return mostrarMensajeProducto('POR FAVOR COMPLETE TODOS LOS CAMPOS OBLIGATORIOS (*)', 'error'); if (isNaN(valorNeto) || valorNeto <= 0) return mostrarMensajeProducto('EL VALOR NETO DEBE SER MAYOR A 0', 'error'); const producto = {codigo, descripcion: descripcion.toUpperCase(), costo: parseFloat(costo.toFixed(2)), porcentaje: parseFloat(porcentaje.toFixed(2)), valorNeto: parseFloat(valorNeto.toFixed(2)), proveedor: proveedor.toUpperCase(), link: link}; gestorProductos.agregarProducto(codigo, producto); mostrarMensajeProducto('PRODUCTO GUARDADO EXITOSAMENTE', 'exito'); cancelarProducto(); agregarProductoACotizacion(codigo, producto);}
function cancelarProducto() {document.getElementById('formularioProducto').classList.remove('activo'); document.getElementById('codigoProducto').value = ''; document.getElementById('descripcionProducto').value = ''; document.getElementById('costoProducto').value = ''; document.getElementById('porcentajeProducto').value = '0'; document.getElementById('valorNetoProducto').value = ''; document.getElementById('proveedorProducto').value = ''; document.getElementById('linkProducto').value = ''; document.getElementById('inputCodigoProducto').value = ''; document.getElementById('mensajeProducto').style.display = 'none';}
function agregarProductoACotizacion(codigo, producto) {const existente = productosEnCotizacion.find(p => p.codigo === codigo); if (existente) {existente.cantidad++; existente.total = +(existente.cantidad * existente.valorNeto).toFixed(2);} else {productosEnCotizacion.push({codigo: producto.codigo, descripcion: producto.descripcion, cantidad: 1, valorNeto: producto.valorNeto, total: producto.valorNeto.toFixed(2)});} actualizarTablaProductos();}
function actualizarTablaProductos() {const contenedor = document.getElementById('tablaProductosContenedor'); if (productosEnCotizacion.length === 0) {contenedor.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>'; document.getElementById('resumenTotales').style.display = 'none'; return;} let html = `<table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th style="width:70px;">CANTIDAD</th><th style="width:90px;">VALOR NETO</th><th style="width:90px;">TOTAL</th><th style="width:60px;">ACCIÓN</th></tr></thead><tbody>`; productosEnCotizacion.forEach((p, i) => {html += `<tr><td>${p.codigo}</td><td>${p.descripcion}</td><td><input type="number" min="1" value="${p.cantidad}" onchange="actualizarCantidad(${i}, this.value)"></td><td class="valor-numerico">$${parseFloat(p.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(p.total).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="celda-acciones"><button class="btn-eliminar" onclick="eliminarProducto(${i})">ELIMINAR</button></td></tr>`;});\nhtml += '</tbody></table>'; contenedor.innerHTML = html; actualizarResumenTotales();}
function actualizarCantidad(i, cantidad) {cantidad = parseInt(cantidad); if (isNaN(cantidad) || cantidad < 1) {alert('Cantidad inválida'); actualizarTablaProductos(); return;} productosEnCotizacion[i].cantidad = cantidad; productosEnCotizacion[i].total = +(productosEnCotizacion[i].valorNeto * cantidad).toFixed(2); actualizarTablaProductos();}
function eliminarProducto(i) {productosEnCotizacion.splice(i, 1); actualizarTablaProductos();}
function actualizarResumenTotales() {const neto = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.total), 0); const iva = +(neto * 0.19).toFixed(2); const total = +(neto + iva).toFixed(2); document.getElementById('totalNeto').textContent = '$' + neto.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2}); document.getElementById('totalIva').textContent = '$' + iva.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2}); document.getElementById('totalGeneral').textContent = '$' + total.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2}); document.getElementById('resumenTotales').style.display = 'block';}

function abrirArticulos() {document.getElementById('modalArticulos').style.display = 'block'; listarArticulos(); document.getElementById('formularioEditarArticulo').classList.remove('activo');}
function cerrarArticulos() {document.getElementById('modalArticulos').style.display = 'none';}
function listarArticulos() {const todos = gestorProductos.obtenerTodos(); let html = `<table class="tabla-articulos"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO</th><th>PORCENTAJE (%)</th><th>VALOR NETO</th><th>UTILIDAD</th><th>PROVEEDOR</th><th>LINK</th><th>ACCIÓN</th></tr></thead><tbody>`; const claves = Object.keys(todos); if (claves.length === 0) {html += `<tr><td colspan="9" style="text-align:center; color:#888;">NO HAY ARTÍCULOS</td></tr>`;} claves.forEach(codigo => {const art = todos[codigo]; const utilidad = (parseFloat(art.valorNeto) - parseFloat(art.costo)).toFixed(2); html += `<tr><td>${art.codigo}</td><td>${art.descripcion}</td><td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">${parseFloat(art.porcentaje).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(art.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(utilidad).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td>${art.proveedor}</td><td style="text-transform:lowercase;">${art.link}</td><td><button class="btn btn-editar btn-pequeno" onclick="editarArticulo('${art.codigo.replace(/'/g, "")}')">EDITAR</button><button class="btn btn-eliminar btn-pequeno" onclick="eliminarArticulo('${art.codigo.replace(/'/g, "")}')">ELIMINAR</button></td></tr>`;});\nhtml += '</tbody></table>'; document.getElementById('tablaArticulosContenedor').innerHTML = html;}
function editarArticulo(codigo) {const art = gestorProductos.buscarPorCodigo(codigo); if (!art) return; articuloEdicion = art; document.getElementById('formularioEditarArticulo').className = 'formulario-editar-articulo activo'; document.getElementById('formularioEditarArticulo').innerHTML = `<h4>EDITAR ARTÍCULO</h4><div class="fila-campos-producto"><div class="campo-grupo"><label>CÓDIGO</label><input type="text" value="${art.codigo}" disabled></div><div class="campo-grupo"><label>DESCRIPCIÓN</label><input type="text" id="editarDescripcion" value="${art.descripcion}"></div></div><div class="fila-campos-producto-tres"><div class="campo-grupo"><label>COSTO</label><input type="number" id="editarCosto" value="${art.costo}"></div><div class="campo-grupo"><label>PORCENTAJE (%)</label><input type="number" id="editarPorcentaje" value="${art.porcentaje}"></div><div class="campo-grupo"><label>VALOR NETO (AUTOCALCULADO)</label><input type="number" id="editarValorNeto" value="${art.valorNeto}" disabled></div></div><div class="fila-campos-producto"><div class="campo-grupo"><label>PROVEEDOR</label><input type="text" id="editarProveedor" value="${art.proveedor}"></div><div class="campo-grupo"><label>LINK</label><input type="text" id="editarLink" class="link-articulo" value="${art.link}"></div></div><div class="botones-producto"><button class="btn btn-agregar" onclick="guardarEdicionArticulo()">GUARDAR CAMBIOS</button><button class="btn btn-cancelar btn-pequeno" onclick="cancelarEdicionArticulo()">CANCELAR</button></div>`; document.getElementById('editarCosto').addEventListener('change', calcularValorNetoEdicion); document.getElementById('editarPorcentaje').addEventListener('change', calcularValorNetoEdicion); document.getElementById('editarLink').addEventListener('input', e => {e.target.value = e.target.value.toLowerCase();});}
function calcularValorNetoEdicion() {const costo = parseFloat(document.getElementById('editarCosto').value) || 0; const porcentaje = parseFloat(document.getElementById('editarPorcentaje').value) || 0; const neto = costo + (costo * (porcentaje / 100)); document.getElementById('editarValorNeto').value = neto.toFixed(2);}
function guardarEdicionArticulo() {if (!articuloEdicion) return; const codigo = articuloEdicion.codigo; const descripcion = document.getElementById('editarDescripcion').value.trim().toUpperCase(); const costo = parseFloat(document.getElementById('editarCosto').value) || 0; const porcentaje = parseFloat(document.getElementById('editarPorcentaje').value) || 0; const valorNeto = parseFloat(document.getElementById('editarValorNeto').value) || 0; const proveedor = document.getElementById('editarProveedor').value.trim().toUpperCase(); const link = document.getElementById('editarLink').value.trim().toLowerCase(); if (!descripcion || isNaN(costo) || isNaN(porcentaje) || isNaN(valorNeto) || !proveedor || !link) {alert('Complete todos los campos correctamente'); return;} const nuevo = {codigo, descripcion, costo, porcentaje, valorNeto, proveedor, link}; gestorProductos.actualizarProducto(codigo, nuevo); articuloEdicion = null; document.getElementById('formularioEditarArticulo').classList.remove('activo'); listarArticulos();}
function cancelarEdicionArticulo() {articuloEdicion = null; document.getElementById('formularioEditarArticulo').classList.remove('activo');}
function eliminarArticulo(codigo) {if (confirm('¿Está seguro que desea eliminar este artículo?')) {gestorProductos.eliminarProducto(codigo); listarArticulos();}}

async function generarPDF() {if (!clienteActual) {alert('Debe ingresar y guardar los datos del cliente antes de generar la cotización.'); return;} if (productosEnCotizacion.length === 0) {alert('Debe agregar al menos un producto o servicio para generar la cotización.'); return;} const numCotizacion = gestorCotizaciones.siguienteCotizacion(); const neto = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.total), 0); cotizacionesEmitidas.push({numero: numCotizacion, razonSocial: clienteActual.razonSocial, neto: neto.toFixed(2), fecha: new Date().toISOString()}); localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas)); const { jsPDF } = window.jspdf; const doc = new jsPDF(); doc.setFontSize(18); doc.text('COTIZACION', 14, 15); doc.setFontSize(12); doc.text(`EMPRESA CUNDO`, 14, 25); doc.text(`N°: ${numCotizacion}`, 160, 25); doc.setFontSize(14); doc.text('Datos del cliente', 14, 35); let y = 40; doc.setFontSize(10); doc.text(`RUT: ${clienteActual.rut}`, 14, y); y += 7; doc.text(`Razón Social: ${clienteActual.razonSocial}`, 14, y); y += 7; doc.text(`Giro: ${clienteActual.giro}`, 14, y); y += 7; doc.text(`Dirección: ${clienteActual.direccion}`, 14, y); y += 7; doc.text(`Contacto: ${clienteActual.nombreContacto}`, 14, y); y += 7; doc.text(`Celular: ${clienteActual.celular}`, 14, y); y += 7; doc.text(`Mail: ${clienteActual.mail}`, 14, y); y += 10; doc.setFontSize(14); doc.text('Productos y Servicios', 14, y); y += 7; const colWidths = [30, 60, 20, 30, 30]; doc.autoTable({startY: y, head: [['CÓDIGO', 'DESCRIPCIÓN', 'CANTIDAD', 'VALOR NETO', 'TOTAL']], body: productosEnCotizacion.map(p => [p.codigo, p.descripcion, p.cantidad.toString(), `$${p.valorNeto.toFixed(2)}`, `$${p.total}`]), theme: 'grid', styles: {fontSize: 9, cellPadding: 2}, headStyles: {fillColor: [52, 73, 94]}, columnStyles: {0: {cellWidth: colWidths[0]}, 1: {cellWidth: colWidths[1]}, 2: {cellWidth: colWidths[2], halign: 'center'}, 3: {cellWidth: colWidths[3], halign: 'right'}, 4: {cellWidth: colWidths[4], halign: 'right'}}}); let finalY = doc.lastAutoTable.finalY + 10; const netoStr = `$${neto.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`; const iva = +(neto * 0.19).toFixed(2); const ivaStr = `$${iva.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`; const total = +(neto + iva).toFixed(2); const totalStr = `$${total.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`; doc.setFontSize(12); doc.text('Resumen', 150, finalY); doc.setFontSize(10); doc.text('Neto:', 150, finalY + 7); doc.text(netoStr, 190, finalY + 7, {align: 'right'}); doc.text('IVA (19%):', 150, finalY + 14); doc.text(ivaStr, 190, finalY + 14, {align: 'right'}); doc.text('Total:', 150, finalY + 21); doc.text(totalStr, 190, finalY + 21, {align: 'right'}); doc.save(`Cotizacion_${numCotizacion}.pdf`); alert('PDF generado y número de cotización actualizado.');}

function mostrarCotizaciones() {const modal = document.getElementById('modalCotizaciones'); const contenedor = document.getElementById('listaCotizaciones'); if (cotizacionesEmitidas.length === 0) {contenedor.innerHTML = '<p style="text-transform:uppercase; text-align:center;">No hay cotizaciones emitidas aún.</p>';} else {let html = '<table style="width:100%; border-collapse: collapse;"><thead><tr><th style="border:1px solid #ddd; padding:8px;">N° Cotización</th><th style="border:1px solid #ddd; padding:8px;">Razón Social</th><th style="border:1px solid #ddd; padding:8px; text-align:right;">Monto Neto</th></tr></thead><tbody>'; cotizacionesEmitidas.forEach(c => {html += `<tr><td style="border:1px solid #ddd; padding:8px;">${c.numero}</td><td style="border:1px solid #ddd; padding:8px;">${c.razonSocial}</td><td style="border:1px solid #ddd; padding:8px; text-align:right;">$${parseFloat(c.neto).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td></tr>`;}); html += '</tbody></table>'; contenedor.innerHTML = html;} modal.style.display = 'block';}
function cerrarCotizaciones() {document.getElementById('modalCotizaciones').style.display = 'none';}
</script>
</body>
</html>
