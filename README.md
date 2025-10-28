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
.cotizador-header {background: #2c3e50; color: white; padding: 8px 30px; display: flex; justify-content: space-between; align-items: center; border-bottom: 3px solid #3498db; margin-bottom: 20px;}
.empresa-nombre {font-weight: bold; font-size: 16px; letter-spacing: 1px;}
.numero-cotizacion {font-weight: bold; color: #3498db; font-size: 13px;}
.seccion-titulo {font-weight: bold; font-size: 18px; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 2px solid #3498db; text-transform: uppercase;}
table {width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 13px;}
th, td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase;}
th {background: #34495e; color: white; font-weight: bold;}
tr:nth-child(even) {background: #f9f9f9;}
tr:hover {background: #f0f0f0;}
.valor-numerico {text-align: right; font-weight: bold;}
.celda-acciones {text-align: center;}
button {cursor: pointer; border: none; border-radius: 4px; font-weight: bold; text-transform: uppercase; transition: background-color 0.3s;}
.btn-buscar {background: #3498db; color: white; padding: 8px 12px; font-size: 12px;}
.btn-buscar:hover {background: #2980b9;}
.btn-limpiar {background: #95a5a6; color: white; padding: 8px 12px; font-size: 12px;}
.btn-limpiar:hover {background: #7f8c8d;}
.btn-eliminar {background: #e74c3c; color: white; font-size: 11px; padding: 5px 10px;}
.btn-eliminar:hover {background: #c0392b;}
.btn-articulos {background: #16a085; color: white; padding: 8px 12px; font-size: 12px;}
.btn-articulos:hover {background: #138d75;}
.btn-pdf {background: #d35400; color: white; padding: 8px 12px; font-size: 12px;}
.btn-pdf:hover {background: #e67e22;}
.btn-agregar {background: #27ae60; color: white; padding: 8px 12px; font-size: 12px;}
.btn-agregar:hover {background: #229954;}
.btn-editar {background: #f39c12; color: white; padding: 8px 12px; font-size: 12px;}
.btn-editar:hover {background: #e67e22;}
.btn-cancelar {background: #e74c3c; color: white; padding: 8px 12px; font-size: 12px;}
.btn-cancelar:hover {background: #c0392b;}
.btn-ver {background: #9b59b6; color: white; padding: 5px 8px; font-size: 10px; margin-right: 3px;}
.btn-ver:hover {background: #8e44ad;}
.btn-editar-cot {background: #f39c12; color: white; padding: 5px 8px; font-size: 10px; margin-right: 3px;}
.btn-editar-cot:hover {background: #e67e22;}
.formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none; animation: fadeIn 0.3s;}
.formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
@keyframes fadeIn {from {opacity: 0;} to {opacity: 1;}}
.campo-grupo {margin-bottom: 15px; position: relative;}
.campo-grupo label {display: block; font-weight: bold; color: #2c3e50; margin-bottom: 5px; font-size: 13px; text-transform: uppercase;}
.campo-grupo input {width: 100%; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 4px; transition: border-color 0.3s; text-transform: uppercase;}
.campo-grupo input:focus {outline: none; border-color: #3498db;}
.campo-grupo input:disabled {background-color: #ecf0f1; cursor: not-allowed;}
.campo-grupo input.link-articulo, #linkProducto {text-transform: lowercase;}
.fila-campos {display: grid; grid-template-columns: 1fr 1fr; gap: 15px;}
.mensaje {padding: 10px 15px; border-radius: 4px; margin-bottom: 15px; font-size: 12px; text-transform: uppercase;}
.mensaje-exito {background-color: #d4edda; color: #155724; border: 1px solid #c3e6cb;}
.mensaje-error {background-color: #f8d7da; color: #721c24; border: 1px solid #f5c6cb;}
.mensaje-info {background-color: #d1ecf1; color: #0c5460; border: 1px solid #bee5eb;}
.resumen-cliente {display: none; background-color: #e8f5e9; padding: 15px; border-radius: 4px; border-left: 4px solid #4caf50;}
.resumen-cliente.activo {display: block;}
.resumen-cliente h4 {color: #2c3e50; margin-bottom: 10px; text-transform: uppercase;}
.resumen-cliente p {margin: 5px 0; font-size: 13px; color: #555; text-transform: uppercase;}
.resumen-cliente strong {color: #2c3e50;}
.botones-resumen {margin-top: 15px; display: flex; gap: 10px;}
.botones-formulario {display: flex; gap: 10px; margin-top: 10px;}
.seccion-cliente {margin-bottom: 30px; border: 1px solid #ddd; border-radius: 5px; padding: 20px; background-color: #fafafa;}
.busqueda-rut {display: flex; gap: 10px; margin-bottom: 20px; align-items: center;}
.busqueda-rut input {flex: 1; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 4px; text-transform: uppercase;}
.busqueda-rut input:focus {outline: none; border-color: #3498db;}
.seccion-productos {margin-bottom: 30px; border: 1px solid #ddd; border-radius: 5px; padding: 20px; background-color: #fafafa;}
.busqueda-producto {display: flex; gap: 10px; margin-bottom: 20px; align-items: center; position: relative;}
.busqueda-producto input {flex: 1; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 4px; text-transform: uppercase;}
.busqueda-producto input:focus {outline: none; border-color: #3498db;}
.autocomplete-list {display: none; position: absolute; top: 100%; left: 0; right: 70px; background: white; border: 1px solid #ddd; border-top: none; max-height: 200px; overflow-y: auto; z-index: 1000; border-radius: 0 0 4px 4px;}
.autocomplete-list.activo {display: block;}
.autocomplete-item {padding: 10px; cursor: pointer; background: white; font-size: 12px; text-transform: uppercase; border-bottom: 1px solid #eee;}
.autocomplete-item:hover {background: #3498db; color: white;}
.autocomplete-item strong {font-weight: bold; color: #2c3e50;}
.autocomplete-item:hover strong {color: white;}
.formulario-producto {background: #fff3cd; padding: 15px; border-radius: 4px; border-left: 4px solid #f39c12; margin-bottom: 20px;}
.formulario-producto h4 {text-transform: uppercase; color: #856404; margin-bottom: 15px;}
.fila-campos-producto {display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;}
.fila-campos-producto-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 15px;}
.fila-campos-producto .campo-grupo, .fila-campos-producto-tres .campo-grupo {margin-bottom: 0;}
.botones-producto {display: flex; gap: 10px; margin-top: 10px;}
.alerta-sin-productos {background-color: #e8f4f8; padding: 15px; border-radius: 4px; border-left: 4px solid #3498db; text-align: center; color: #2c3e50; text-transform: uppercase; font-weight: bold;}
#modalArticulos {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.5); z-index: 9999; overflow: auto;}
.articulos-content {background: white; border-radius: 8px; margin: 40px auto; padding: 25px; max-width: 1000px; position: relative;}
.modal-titulo {font-size: 22px; font-weight: bold; margin-bottom: 20px; text-transform: uppercase;}
.btn-cerrar-modal {position: absolute; top: 14px; right: 25px; background: #e74c3c; color: white; font-size: 18px; border: none; border-radius: 4px; cursor: pointer; padding: 4px 10px;}
.botones-superiores {display: flex; gap: 8px; margin-bottom: 20px; flex-wrap: wrap; align-items: center;}
#modalCotizaciones {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.5); z-index: 10000; overflow: auto;}
#modalCotizaciones > div {background: white; max-width: 900px; margin: 40px auto; padding: 20px; border-radius: 8px; position: relative;}
.resumen-totales {max-width: 400px; margin-left: auto; border-top: 3px solid #3498db; padding-top: 15px; text-transform: uppercase; margin-bottom: 20px;}
.resumen-linea {display: flex; justify-content: space-between; margin: 5px 0; font-weight: bold; font-size: 13px;}
.resumen-linea.total {font-size: 16px; color: #2c3e50;}
.resumen-utilidad {background-color: #fff3cd; padding: 15px; border-radius: 4px; border-left: 4px solid #f39c12; margin-top: 20px;}
.resumen-utilidad h4 {color: #856404; text-transform: uppercase; margin-bottom: 10px; font-size: 14px;}
.utilidad-item {display: flex; justify-content: space-between; margin: 8px 0; font-size: 13px; color: #333;}
.utilidad-item strong {color: #2c3e50;}
.formulario-editar-articulo {background: #fff3cd; padding: 15px; border-radius: 4px; border-left: 4px solid #f39c12; margin-bottom: 20px;}
.formulario-editar-articulo h4 {text-transform: uppercase; color: #856404; margin-bottom: 15px;}
.tabla-cotizaciones {width: 100%; border-collapse: collapse; background: white;}
.tabla-cotizaciones th {background: #34495e; color: white; padding: 12px; text-align: left; text-transform: uppercase; font-weight: bold;}
.tabla-cotizaciones td {border: 1px solid #ddd; padding: 12px; text-transform: uppercase;}
.tabla-cotizaciones tr:nth-child(even) {background: #f9f9f9;}
.tabla-cotizaciones tr:hover {background: #f0f0f0;}
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
        <div class="campo-grupo"><label>RUT *</label><input type="text" id="rut" disabled /></div>
        <div class="campo-grupo"><label>RAZÓN SOCIAL *</label><input type="text" id="razonSocial" /></div>
        <div class="campo-grupo"><label>GIRO *</label><input type="text" id="giro" /></div>
        <div class="campo-grupo"><label>DIRECCIÓN DE FACTURACIÓN *</label><input type="text" id="direccion" /></div>
        <div class="fila-campos">
          <div class="campo-grupo"><label>NOMBRE DE CONTACTO *</label><input type="text" id="nombreContacto" /></div>
          <div class="campo-grupo"><label>CELULAR *</label><input type="tel" id="celular" placeholder="+56912345678" /></div>
        </div>
        <div class="campo-grupo"><label>MAIL *</label><input type="email" id="mail" /></div>
        <div class="botones-formulario">
          <button class="btn btn-buscar" onclick="guardarCliente()"><span id="textoBotonGuardar">GUARDAR CLIENTE</span></button>
          <button class="btn btn-cancelar" id="btnCancelarEdicion" onclick="cancelarEdicion()" style="display: none;">CANCELAR</button>
        </div>
      </div>
    </section>

    <section class="seccion-productos">
      <h2 class="seccion-titulo">PRODUCTOS Y/O SERVICIOS</h2>
      <div class="busqueda-producto">
        <input type="text" id="inputCodigoProducto" placeholder="INGRESE CÓDIGO O DESCRIPCIÓN DEL PRODUCTO" maxlength="50" />
        <div id="autocompleteLista" class="autocomplete-list"></div>
        <button class="btn btn-buscar" onclick="buscarProducto()">BUSCAR</button>
      </div>
      <div id="mensajeProducto"></div>
      <div id="formularioProducto" class="formulario-producto" style="display: none;">
        <h4>CREAR NUEVO PRODUCTO/SERVICIO</h4>
        <div class="fila-campos-producto">
          <div class="campo-grupo"><label>CÓDIGO *</label><input type="text" id="codigoProducto" disabled /></div>
          <div class="campo-grupo"><label>DESCRIPCIÓN *</label><input type="text" id="descripcionProducto" /></div>
        </div>
        <div class="fila-campos-producto-tres">
          <div class="campo-grupo"><label>COSTO *</label><input type="number" id="costoProducto" min="0" step="0.01" /></div>
          <div class="campo-grupo"><label>PORCENTAJE (%) *</label><input type="number" id="porcentajeProducto" min="0" step="0.01" value="0" /></div>
          <div class="campo-grupo"><label>VALOR NETO (AUTOCALCULADO)</label><input type="number" id="valorNetoProducto" disabled /></div>
        </div>
        <div class="fila-campos-producto">
          <div class="campo-grupo"><label>PROVEEDOR *</label><input type="text" id="proveedorProducto" /></div>
          <div class="campo-grupo"><label>LINK *</label><input type="text" id="linkProducto" class="link-articulo" /></div>
        </div>
        <div class="botones-producto">
          <button class="btn btn-agregar" onclick="guardarProducto()">GUARDAR PRODUCTO</button>
          <button class="btn btn-cancelar" onclick="cancelarProducto()">CANCELAR</button>
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
      <div id="utilidadResumen" style="display:none;"></div>
    </section>
  </div>

  <div id="modalArticulos">
    <div class="articulos-content">
      <button class="btn-cerrar-modal" onclick="cerrarArticulos()">×</button>
      <div class="modal-titulo">ARTÍCULOS (PRODUCTOS Y SERVICIOS REGISTRADOS)</div>
      <div id="tablaArticulosContenedor"></div>
      <div id="formularioEditarArticulo" class="formulario-editar-articulo" style="display: none;"></div>
    </div>
  </div>

  <div id="modalCotizaciones">
    <div>
      <button onclick="cerrarCotizaciones()" style="position:absolute;top:15px;right:20px;background:#e74c3c;color:white;border:none;border-radius:4px;padding:5px 10px;cursor:pointer;font-size:18px;font-weight:bold;">×</button>
      <h2 style="margin-bottom:15px;text-transform:uppercase;">COTIZACIONES EMITIDAS</h2>
      <div id="listaCotizaciones"></div>
    </div>
  </div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>

<script>
class GestorCotizaciones {
  constructor() {this.prefijo = 'CO'; this.numeroBase = 100500; this.cargarNumeroCotizacion();}
  cargarNumeroCotizacion() {const n = localStorage.getItem('ultimaCotizacion'); if (n) this.numeroBase = parseInt(n); this.actualizarDisplay();}
  obtenerNumeroActual() {return `${this.prefijo}${this.numeroBase}`;}
  siguienteCotizacion() {this.numeroBase++; localStorage.setItem('ultimaCotizacion', this.numeroBase); this.actualizarDisplay(); return this.obtenerNumeroActual();}
  actualizarDisplay() {const el = document.getElementById('numeroCotizacion'); if (el) el.textContent = this.obtenerNumeroActual();}
}

class GestorClientes {
  constructor() {this.clientes = this.cargarClientes();}
  cargarClientes() {const c = localStorage.getItem('clientes'); return c ? JSON.parse(c) : {};}
  guardarClientes() {localStorage.setItem('clientes', JSON.stringify(this.clientes));}
  buscarPorRut(rut) {return this.clientes[rut] || null;}
  agregarCliente(rut, datos) {this.clientes[rut] = datos; this.guardarClientes();}
  actualizarCliente(rut, datos) {this.clientes[rut] = datos; this.guardarClientes();}
}

class GestorProductos {
  constructor() {this.productos = this.cargarProductos();}
  cargarProductos() {const p = localStorage.getItem('productos'); return p ? JSON.parse(p) : {};}
  guardarProductos() {localStorage.setItem('productos', JSON.stringify(this.productos));}
  buscarPorCodigo(codigo) {return this.productos[codigo] || null;}
  agregarProducto(codigo, datos) {this.productos[codigo] = datos; this.guardarProductos();}
  actualizarProducto(codigo, datos) {this.productos[codigo] = datos; this.guardarProductos();}
  obtenerTodos() {return this.productos;}
  eliminarProducto(codigo) {delete this.productos[codigo]; this.guardarProductos();}
  buscarPorCodigoODescripcion(termino) {
    termino = termino.toUpperCase().trim();
    const todos = this.obtenerTodos();
    return Object.values(todos).filter(prod => 
      prod.codigo.includes(termino) || prod.descripcion.includes(termino)
    ).slice(0, 10);
  }
}

const gestorCotizaciones = new GestorCotizaciones();
const gestorClientes = new GestorClientes();
const gestorProductos = new GestorProductos();

let clienteActual = null;
let modoEdicion = false;
let productosEnCotizacion = [];
let cotizacionesEmitidas = JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]');
let articuloEdicion = null;

document.getElementById('inputRut').addEventListener('input', function(e) {
  let v = e.target.value.replace(/[^0-9kK]/g, '');
  if (v.length > 1) v = v.slice(0, -1) + '-' + v.slice(-1);
  e.target.value = v.toUpperCase();
});

document.getElementById('inputCodigoProducto').addEventListener('input', function(e) {
  e.target.value = e.target.value.toUpperCase();
  mostrarAutocomplete();
});

document.getElementById('inputCodigoProducto').addEventListener('keydown', function(e) {
  if (e.key === 'Enter') buscarProducto();
});

function mostrarAutocomplete() {
  const input = document.getElementById('inputCodigoProducto').value.trim();
  const lista = document.getElementById('autocompleteLista');
  
  if (!input || input.length < 1) {
    lista.classList.remove('activo');
    return;
  }
  
  const resultados = gestorProductos.buscarPorCodigoODescripcion(input);
  
  if (resultados.length === 0) {
    lista.classList.remove('activo');
    return;
  }
  
  let html = '';
  resultados.forEach(prod => {
    html += `<div class="autocomplete-item" onclick="seleccionarProductoAutocomplete('${prod.codigo}')">
      <strong>${prod.codigo}</strong> - ${prod.descripcion}
    </div>`;
  });
  
  lista.innerHTML = html;
  lista.classList.add('activo');
}

function seleccionarProductoAutocomplete(codigo) {
  document.getElementById('inputCodigoProducto').value = codigo;
  document.getElementById('autocompleteLista').classList.remove('activo');
  buscarProducto();
}

document.addEventListener('click', function(e) {
  if (!e.target.closest('.busqueda-producto')) {
    document.getElementById('autocompleteLista').classList.remove('activo');
  }
});

function buscarCliente() {
  const rut = document.getElementById('inputRut').value.trim();
  if (!rut) return mostrarMensaje('POR FAVOR INGRESE UN RUT', 'error');
  if (!validarRut(rut)) return mostrarMensaje('EL FORMATO DEL RUT NO ES VÁLIDO', 'error');
  const cliente = gestorClientes.buscarPorRut(rut);
  if (cliente) {
    clienteActual = cliente;
    modoEdicion = false;
    mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
    mostrarResumenCliente(cliente);
    document.getElementById('formularioCliente').classList.remove('activo');
  } else {
    mostrarMensaje('CLIENTE NO ENCONTRADO. COMPLETE LOS DATOS PARA CREAR.', 'info');
    document.getElementById('resumenCliente').classList.remove('activo');
    document.getElementById('formularioCliente').classList.add('activo');
    document.getElementById('rut').value = rut;
    document.getElementById('textoBotonGuardar').textContent = 'GUARDAR CLIENTE';
    document.getElementById('btnCancelarEdicion').style.display = 'none';
    limpiarCamposFormulario();
    modoEdicion = false;
  }
}

function validarRut(rut) {return /^[0-9]{7,8}-[0-9kK]$/.test(rut);}

function mostrarMensaje(texto, tipo) {
  const m = document.getElementById('mensaje');
  m.className = `mensaje mensaje-${tipo}`;
  m.textContent = texto;
  m.style.display = 'block';
}

function mostrarResumenCliente(cliente) {
  const r = document.getElementById('resumenCliente');
  r.className = 'resumen-cliente activo';
  r.innerHTML = `<h4>✓ CLIENTE REGISTRADO</h4><p><strong>RUT:</strong> ${cliente.rut}</p><p><strong>RAZÓN SOCIAL:</strong> ${cliente.razonSocial}</p><p><strong>GIRO:</strong> ${cliente.giro}</p><p><strong>DIRECCIÓN:</strong> ${cliente.direccion}</p><p><strong>CONTACTO:</strong> ${cliente.nombreContacto}</p><p><strong>CELULAR:</strong> ${cliente.celular}</p><p><strong>MAIL:</strong> ${cliente.mail}</p><div class="botones-resumen"><button class="btn btn-editar" onclick="editarCliente()">✏️ EDITAR DATOS</button></div>`;
}

function editarCliente() {
  if (!clienteActual) return;
  modoEdicion = true;
  document.getElementById('rut').value = clienteActual.rut;
  document.getElementById('razonSocial').value = clienteActual.razonSocial;
  document.getElementById('giro').value = clienteActual.giro;
  document.getElementById('direccion').value = clienteActual.direccion;
  document.getElementById('nombreContacto').value = clienteActual.nombreContacto;
  document.getElementById('celular').value = clienteActual.celular;
  document.getElementById('mail').value = clienteActual.mail;
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('formularioCliente').classList.add('activo');
  document.getElementById('textoBotonGuardar').textContent = 'GUARDAR CAMBIOS';
  document.getElementById('btnCancelarEdicion').style.display = 'inline-block';
  mostrarMensaje('MODO EDICIÓN. MODIFIQUE LOS CAMPOS.', 'info');
}

function cancelarEdicion() {
  if (!clienteActual) return;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
  mostrarResumenCliente(clienteActual);
}

function guardarCliente() {
  const rut = document.getElementById('rut').value;
  const rs = document.getElementById('razonSocial').value.trim();
  const gi = document.getElementById('giro').value.trim();
  const di = document.getElementById('direccion').value.trim();
  const nc = document.getElementById('nombreContacto').value.trim();
  const ce = document.getElementById('celular').value.trim();
  const ma = document.getElementById('mail').value.trim();
  if (!rs || !gi || !di || !nc || !ce || !ma) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  if (!validarEmail(ma)) return mostrarMensaje('EMAIL INVÁLIDO', 'error');
  const cliente = {rut, razonSocial: rs.toUpperCase(), giro: gi.toUpperCase(), direccion: di.toUpperCase(), nombreContacto: nc.toUpperCase(), celular: ce, mail: ma.toUpperCase()};
  if (modoEdicion) gestorClientes.actualizarCliente(rut, cliente);
  else gestorClientes.agregarCliente(rut, cliente);
  mostrarMensaje(modoEdicion ? 'CLIENTE ACTUALIZADO' : 'CLIENTE GUARDADO', 'exito');
  clienteActual = cliente;
  modoEdicion = false;
  mostrarResumenCliente(cliente);
  document.getElementById('formularioCliente').classList.remove('activo');
}

function validarEmail(e) {return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(e);}

function limpiarFormulario() {
  document.getElementById('inputRut').value = '';
  document.getElementById('mensaje').style.display = 'none';
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('formularioCliente').classList.remove('activo');
  document.getElementById('btnCancelarEdicion').style.display = 'none';
  limpiarCamposFormulario();
  clienteActual = null;
  modoEdicion = false;
}

function limpiarCamposFormulario() {
  document.getElementById('razonSocial').value = '';
  document.getElementById('giro').value = '';
  document.getElementById('direccion').value = '';
  document.getElementById('nombreContacto').value = '';
  document.getElementById('celular').value = '';
  document.getElementById('mail').value = '';
}

document.getElementById('costoProducto').addEventListener('change', calcularValorNeto);
document.getElementById('porcentajeProducto').addEventListener('change', calcularValorNeto);

function calcularValorNeto() {
  const c = parseFloat(document.getElementById('costoProducto').value) || 0;
  const p = parseFloat(document.getElementById('porcentajeProducto').value) || 0;
  const vn = c + (c * (p / 100));
  document.getElementById('valorNetoProducto').value = vn.toFixed(2);
}

function buscarProducto() {
  const cod = document.getElementById('inputCodigoProducto').value.trim();
  if (!cod) return mostrarMensajeProducto('INGRESE UN CÓDIGO O DESCRIPCIÓN', 'error');
  const prod = gestorProductos.buscarPorCodigo(cod);
  if (prod) {
    mostrarMensajeProducto('PRODUCTO ENCONTRADO', 'exito');
    document.getElementById('formularioProducto').style.display = 'none';
    agregarProductoACotizacion(cod, prod);
    document.getElementById('inputCodigoProducto').value = '';
  } else {
    mostrarMensajeProducto('NO ENCONTRADO. CREE UNO NUEVO.', 'info');
    document.getElementById('formularioProducto').style.display = 'block';
    document.getElementById('codigoProducto').value = cod;
    document.getElementById('descripcionProducto').value = '';
    document.getElementById('costoProducto').value = '';
    document.getElementById('porcentajeProducto').value = '0';
    document.getElementById('valorNetoProducto').value = '';
    document.getElementById('proveedorProducto').value = '';
    document.getElementById('linkProducto').value = '';
  }
}

function mostrarMensajeProducto(t, ti) {
  const m = document.getElementById('mensajeProducto');
  m.className = `mensaje mensaje-${ti}`;
  m.textContent = t;
  m.style.display = 'block';
}

function guardarProducto() {
  const cod = document.getElementById('codigoProducto').value.trim();
  const desc = document.getElementById('descripcionProducto').value.trim();
  const cos = parseFloat(document.getElementById('costoProducto').value);
  const por = parseFloat(document.getElementById('porcentajeProducto').value) || 0;
  const vn = parseFloat(document.getElementById('valorNetoProducto').value);
  const prov = document.getElementById('proveedorProducto').value.trim();
  const link = document.getElementById('linkProducto').value.trim().toLowerCase();
  if (!cod || !desc || isNaN(cos) || cos <= 0 || !prov || !link) return mostrarMensajeProducto('COMPLETE TODOS LOS CAMPOS', 'error');
  if (isNaN(vn) || vn <= 0) return mostrarMensajeProducto('VALOR NETO INVÁLIDO', 'error');
  const prod = {codigo: cod, descripcion: desc.toUpperCase(), costo: parseFloat(cos.toFixed(2)), porcentaje: parseFloat(por.toFixed(2)), valorNeto: parseFloat(vn.toFixed(2)), proveedor: prov.toUpperCase(), link: link};
  gestorProductos.agregarProducto(cod, prod);
  mostrarMensajeProducto('PRODUCTO GUARDADO', 'exito');
  cancelarProducto();
  agregarProductoACotizacion(cod, prod);
}

function cancelarProducto() {
  document.getElementById('formularioProducto').style.display = 'none';
  document.getElementById('codigoProducto').value = '';
  document.getElementById('descripcionProducto').value = '';
  document.getElementById('costoProducto').value = '';
  document.getElementById('porcentajeProducto').value = '0';
  document.getElementById('valorNetoProducto').value = '';
  document.getElementById('proveedorProducto').value = '';
  document.getElementById('linkProducto').value = '';
  document.getElementById('inputCodigoProducto').value = '';
  document.getElementById('mensajeProducto').style.display = 'none';
}

function agregarProductoACotizacion(cod, prod) {
  const ex = productosEnCotizacion.find(p => p.codigo === cod);
  if (ex) {
    ex.cantidad++;
    ex.total = +(ex.cantidad * ex.valorNetaConDescuento).toFixed(2);
  } else {
    productosEnCotizacion.push({codigo: prod.codigo, descripcion: prod.descripcion, cantidad: 1, valorNeto: prod.valorNeto, costo: prod.costo, descuento: 0, valorNetaConDescuento: prod.valorNeto, total: prod.valorNeto.toFixed(2)});
  }
  actualizarTablaProductos();
}

function actualizarTablaProductos() {
  const cont = document.getElementById('tablaProductosContenedor');
  if (productosEnCotizacion.length === 0) {
    cont.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS</div>';
    document.getElementById('resumenTotales').style.display = 'none';
    document.getElementById('utilidadResumen').style.display = 'none';
    return;
  }
  let html = '<table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th style="width:70px;">CANTIDAD</th><th style="width:80px;">VALOR NETO</th><th style="width:80px;">DESCUENTO (%)</th><th style="width:80px;">V. NETO DESC.</th><th style="width:100px;">TOTAL</th><th style="width:60px;">ACCIÓN</th></tr></thead><tbody>';
  productosEnCotizacion.forEach((p, i) => {
    html += `<tr><td>${p.codigo}</td><td>${p.descripcion}</td><td><input type="number" min="1" value="${p.cantidad}" onchange="actualizarCantidad(${i}, this.value)"></td><td class="valor-numerico">$${parseFloat(p.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td><input type="number" min="0" max="100" step="0.01" value="${p.descuento}" style="width:100%;padding:5px;" onchange="actualizarDescuento(${i}, this.value)"></td><td class="valor-numerico">$${parseFloat(p.valorNetaConDescuento).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(p.total).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="celda-acciones"><button class="btn-eliminar" onclick="eliminarProducto(${i})">ELIMINAR</button></td></tr>`;
  });
  html += '</tbody></table>';
  cont.innerHTML = html;
  actualizarResumenTotales();
}

function actualizarCantidad(i, cant) {
  cant = parseInt(cant);
  if (isNaN(cant) || cant < 1) {alert('CANTIDAD INVÁLIDA'); actualizarTablaProductos(); return;}
  productosEnCotizacion[i].cantidad = cant;
  productosEnCotizacion[i].total = +(productosEnCotizacion[i].valorNetaConDescuento * cant).toFixed(2);
  actualizarTablaProductos();
}

function actualizarDescuento(i, desc) {
  desc = parseFloat(desc) || 0;
  if (desc < 0 || desc > 100) {alert('DESCUENTO ENTRE 0-100'); actualizarTablaProductos(); return;}
  productosEnCotizacion[i].descuento = desc;
  const valorConDesc = productosEnCotizacion[i].valorNeto - (productosEnCotizacion[i].valorNeto * (desc / 100));
  productosEnCotizacion[i].valorNetaConDescuento = parseFloat(valorConDesc.toFixed(2));
  productosEnCotizacion[i].total = +(productosEnCotizacion[i].valorNetaConDescuento * productosEnCotizacion[i].cantidad).toFixed(2);
  actualizarTablaProductos();
}

function eliminarProducto(i) {
  productosEnCotizacion.splice(i, 1);
  actualizarTablaProductos();
}

function actualizarResumenTotales() {
  const net = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.total), 0);
  const iva = +(net * 0.19).toFixed(2);
  const tot = +(net + iva).toFixed(2);
  document.getElementById('totalNeto').textContent = '$' + net.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2});
  document.getElementById('totalIva').textContent = '$' + iva.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2});
  document.getElementById('totalGeneral').textContent = '$' + tot.toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2});
  document.getElementById('resumenTotales').style.display = 'block';
  
  let htmlUtilidad = '<div class="resumen-utilidad"><h4>📊 UTILIDAD (INTERNO)</h4>';
  let utilidadTotal = 0;
  productosEnCotizacion.forEach(p => {
    const utilidad = (parseFloat(p.total) - (parseFloat(p.costo) * p.cantidad));
    utilidadTotal += utilidad;
    htmlUtilidad += `<div class="utilidad-item"><strong>${p.codigo}:</strong> $${Math.round(utilidad).toLocaleString('es-CL')}</div>`;
  });
  htmlUtilidad += `<div class="utilidad-item" style="margin-top: 10px; padding-top: 10px; border-top: 1px solid #f39c12;"><strong>UTILIDAD TOTAL:</strong> $${Math.round(utilidadTotal).toLocaleString('es-CL')}</div>`;
  htmlUtilidad += '</div>';
  document.getElementById('utilidadResumen').innerHTML = htmlUtilidad;
  document.getElementById('utilidadResumen').style.display = 'block';
}

function abrirArticulos() {
  document.getElementById('modalArticulos').style.display = 'block';
  listarArticulos();
  document.getElementById('formularioEditarArticulo').style.display = 'none';
}

function cerrarArticulos() {
  document.getElementById('modalArticulos').style.display = 'none';
}

function listarArticulos() {
  const todos = gestorProductos.obtenerTodos();
  let html = '<table class="tabla-articulos"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO</th><th>PORCENTAJE (%)</th><th>VALOR NETO</th><th>UTILIDAD</th><th>PROVEEDOR</th><th>LINK</th><th>ACCIÓN</th></tr></thead><tbody>';
  const claves = Object.keys(todos);
  if (claves.length === 0) {
    html += '<tr><td colspan="9" style="text-align:center;">NO HAY ARTÍCULOS</td></tr>';
  } else {
    claves.forEach(codigo => {
      const art = todos[codigo];
      const util = (parseFloat(art.valorNeto) - parseFloat(art.costo)).toFixed(2);
      html += `<tr><td>${art.codigo}</td><td>${art.descripcion}</td><td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">${parseFloat(art.porcentaje).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(art.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(util).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td>${art.proveedor}</td><td style="text-transform:lowercase;">${art.link}</td><td><button class="btn btn-editar" onclick="editarArticulo('${art.codigo}')">EDITAR</button><button class="btn btn-eliminar" onclick="eliminarArticulo('${art.codigo}')">ELIMINAR</button></td></tr>`;
    });
  }
  html += '</tbody></table>';
  document.getElementById('tablaArticulosContenedor').innerHTML = html;
}

function editarArticulo(codigo) {
  const art = gestorProductos.buscarPorCodigo(codigo);
  if (!art) return;
  articuloEdicion = art;
  const f = document.getElementById('formularioEditarArticulo');
  f.style.display = 'block';
  f.innerHTML = `<h4>EDITAR ARTÍCULO</h4><div class="fila-campos-producto"><div class="campo-grupo"><label>CÓDIGO</label><input type="text" value="${art.codigo}" disabled></div><div class="campo-grupo"><label>DESCRIPCIÓN</label><input type="text" id="editarDescripcion" value="${art.descripcion}"></div></div><div class="fila-campos-producto-tres"><div class="campo-grupo"><label>COSTO</label><input type="number" id="editarCosto" value="${art.costo}"></div><div class="campo-grupo"><label>PORCENTAJE (%)</label><input type="number" id="editarPorcentaje" value="${art.porcentaje}"></div><div class="campo-grupo"><label>VALOR NETO</label><input type="number" id="editarValorNeto" value="${art.valorNeto}" disabled></div></div><div class="fila-campos-producto"><div class="campo-grupo"><label>PROVEEDOR</label><input type="text" id="editarProveedor" value="${art.proveedor}"></div><div class="campo-grupo"><label>LINK</label><input type="text" id="editarLink" value="${art.link}"></div></div><div class="botones-producto"><button class="btn btn-agregar" onclick="guardarEdicionArticulo()">GUARDAR</button><button class="btn btn-cancelar" onclick="cancelarEdicionArticulo()">CANCELAR</button></div>`;
  document.getElementById('editarCosto').addEventListener('change', calcularValorNetoEdicion);
  document.getElementById('editarPorcentaje').addEventListener('change', calcularValorNetoEdicion);
  document.getElementById('editarLink').addEventListener('input', e => {e.target.value = e.target.value.toLowerCase();});
}

function calcularValorNetoEdicion() {
  const c = parseFloat(document.getElementById('editarCosto').value) || 0;
  const p = parseFloat(document.getElementById('editarPorcentaje').value) || 0;
  const vn = c + (c * (p / 100));
  document.getElementById('editarValorNeto').value = vn.toFixed(2);
}

function guardarEdicionArticulo() {
  if (!articuloEdicion) return;
  const codigo = articuloEdicion.codigo;
  const desc = document.getElementById('editarDescripcion').value.trim().toUpperCase();
  const cos = parseFloat(document.getElementById('editarCosto').value) || 0;
  const por = parseFloat(document.getElementById('editarPorcentaje').value) || 0;
  const vn = parseFloat(document.getElementById('editarValorNeto').value) || 0;
  const prov = document.getElementById('editarProveedor').value.trim().toUpperCase();
  const link = document.getElementById('editarLink').value.trim().toLowerCase();
  if (!desc || !prov || !link) {alert('COMPLETE TODOS'); return;}
  const nuevo = {codigo, descripcion: desc, costo: cos, porcentaje: por, valorNeto: vn, proveedor: prov, link: link};
  gestorProductos.actualizarProducto(codigo, nuevo);
  articuloEdicion = null;
  document.getElementById('formularioEditarArticulo').style.display = 'none';
  listarArticulos();
}

function cancelarEdicionArticulo() {
  articuloEdicion = null;
  document.getElementById('formularioEditarArticulo').style.display = 'none';
}

function eliminarArticulo(codigo) {
  if (confirm('¿ELIMINAR?')) {
    gestorProductos.eliminarProducto(codigo);
    listarArticulos();
  }
}

function generarPDF() {
  if (!clienteActual) {alert('INGRESE CLIENTE'); return;}
  if (productosEnCotizacion.length === 0) {alert('AGREGUE PRODUCTOS'); return;}
  const numCot = gestorCotizaciones.siguienteCotizacion();
  const net = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.total), 0);
  const cotizacion = {numero: numCot, razonSocial: clienteActual.razonSocial, neto: net.toFixed(2), fecha: new Date().toISOString(), cliente: JSON.parse(JSON.stringify(clienteActual)), productos: JSON.parse(JSON.stringify(productosEnCotizacion))};
  cotizacionesEmitidas.push(cotizacion);
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  generarPDFDocumento(cotizacion);
  alert('PDF GENERADO Y COTIZACIÓN ACTUALIZADA');
}

function generarPDFDocumento(cotizacion) {
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();
  const net = parseFloat(cotizacion.neto);
  const iva = +(net * 0.19).toFixed(2);
  const tot = +(net + iva).toFixed(2);

  doc.setFillColor(44, 62, 80);
  doc.rect(0, 0, 210, 25, 'F');
  doc.setTextColor(255, 255, 255);
  doc.setFontSize(22);
  doc.setFont(undefined, 'bold');
  doc.text('COTIZACIÓN', 15, 12);
  doc.setFontSize(12);
  doc.setFont(undefined, 'bold');
  doc.setTextColor(52, 152, 219);
  doc.text(`N° ${cotizacion.numero}`, 180, 12, {align: 'right'});

  doc.setTextColor(0, 0, 0);
  doc.setFontSize(11);
  doc.setFont(undefined, 'bold');
  doc.text('DATOS DEL CLIENTE', 15, 40);
  doc.setDrawColor(52, 152, 219);
  doc.setLineWidth(0.3);
  doc.line(15, 42, 195, 42);
  doc.setFontSize(9);
  doc.setFont(undefined, 'normal');
  const clienteData = [
    ['RUT:', cotizacion.cliente.rut, 'CONTACTO:', cotizacion.cliente.nombreContacto],
    ['RAZÓN SOCIAL:', cotizacion.cliente.razonSocial, 'CELULAR:', cotizacion.cliente.celular],
    ['GIRO:', cotizacion.cliente.giro, 'MAIL:', cotizacion.cliente.mail],
    ['DIRECCIÓN:', cotizacion.cliente.direccion, '', '']
  ];
  let yPos = 48;
  clienteData.forEach(row => {
    doc.setFont(undefined, 'bold');
    doc.text(row[0], 15, yPos);
    doc.setFont(undefined, 'normal');
    doc.text(row[1], 40, yPos);
    if (row[2]) {
      doc.setFont(undefined, 'bold');
      doc.text(row[2], 110, yPos);
      doc.setFont(undefined, 'normal');
      doc.text(row[3], 135, yPos);
    }
    yPos += 6;
  });

  doc.setDrawColor(52, 152, 219);
  doc.setLineWidth(0.5);
  doc.line(15, yPos + 2, 195, yPos + 2);

  yPos += 8;
  doc.setFont(undefined, 'bold');
  doc.setFontSize(9);
  doc.text('PRODUCTOS Y SERVICIOS', 15, yPos);
  yPos += 5;
  doc.autoTable({
    startY: yPos,
    head: [['Código', 'Descripción', 'Cant.', 'Valor Neto', 'Total']],
    body: cotizacion.productos.map(p => [
      p.codigo,
      p.descripcion,
      p.cantidad.toString(),
      `$${Math.round(parseFloat(p.valorNetaConDescuento)).toLocaleString('es-CL')}`,
      `$${Math.round(parseFloat(p.total)).toLocaleString('es-CL')}`
    ]),
    theme: 'striped',
    styles: {fontSize: 8, cellPadding: 3},
    headStyles: {fillColor: [52, 73, 94], textColor: 255, fontStyle: 'bold'},
    columnStyles: {
      0: {cellWidth: 20},
      1: {cellWidth: 90},
      2: {cellWidth: 15, halign: 'center'},
      3: {cellWidth: 30, halign: 'right'},
      4: {cellWidth: 30, halign: 'right'}
    },
    margin: {left: 15, right: 15}
  });

  const resumenY = doc.lastAutoTable.finalY + 15;
  const resumenX = 120;
  doc.setFont(undefined, 'normal');
  doc.setTextColor(0, 0, 0);
  doc.setFontSize(10);
  doc.text("Neto:", resumenX, resumenY);
  doc.text(`$${Math.round(net).toLocaleString('es-CL')}`, 185, resumenY, {align: 'right'});
  doc.text("IVA (19%):", resumenX, resumenY + 7);
  doc.text(`$${Math.round(iva).toLocaleString('es-CL')}`, 185, resumenY + 7, {align: 'right'});
  doc.setFont(undefined, 'bold');
  doc.text("TOTAL:", resumenX, resumenY + 14);
  doc.text(`$${Math.round(tot).toLocaleString('es-CL')}`, 185, resumenY + 14, {align: 'right'});

  doc.setFontSize(7);
  doc.setFont(undefined, 'normal');
  doc.setTextColor(100, 100, 100);
  let empresaY = 268;
  doc.text('RUT: 78000256-0 | Razón social: Empresa Servicios SPA | Dirección: Los Pepinos 287, Las Condes.', 15, empresaY);
  doc.text('Mail: contacto@servicios.cl | Teléfono: 56 22 5510365', 15, empresaY + 4);

  doc.setTextColor(150, 150, 150);
  doc.setFontSize(8);
  doc.setFont(undefined, 'italic');
  doc.text('Gracias por su preferencia', 105, 280, {align: 'center'});
  doc.save(`Cotizacion_${cotizacion.numero}.pdf`);
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones');
  const cont = document.getElementById('listaCotizaciones');
  if (cotizacionesEmitidas.length === 0) {
    cont.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 20px;">NO HAY COTIZACIONES EMITIDAS</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    cotizacionesEmitidas.forEach((c, index) => {
      html += `<tr><td>${c.numero}</td><td>${c.razonSocial}</td><td style="text-align:right;">$${parseFloat(c.neto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td style="text-align:center;"><button class="btn-ver" onclick="verCotizacion(${index})">VER</button><button class="btn-editar-cot" onclick="editarCotizacionGuardada(${index})">EDITAR</button><button class="btn-eliminar" onclick="borrarCotizacion(${index})">BORRAR</button></td></tr>`;
    });
    html += '</tbody></table>';
    cont.innerHTML = html;
  }
  modal.style.display = 'block';
}

function verCotizacion(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  const cotizacion = cotizacionesEmitidas[index];
  if (!cotizacion) { alert('ERROR: COTIZACIÓN VACÍA'); return; }
  generarPDFDocumento(cotizacion);
}

function editarCotizacionGuardada(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  const cotizacion = cotizacionesEmitidas[index];
  if (!cotizacion) { alert('ERROR: COTIZACIÓN VACÍA'); return; }
  clienteActual = JSON.parse(JSON.stringify(cotizacion.cliente));
  productosEnCotizacion = JSON.parse(JSON.stringify(cotizacion.productos));
  mostrarResumenCliente(clienteActual);
  document.getElementById('formularioCliente').classList.remove('activo');
  actualizarTablaProductos();
  cerrarCotizaciones();
  mostrarMensaje(`COTIZACIÓN ${cotizacion.numero} CARGADA PARA EDICIÓN`, 'info');
  alert('COTIZACIÓN CARGADA. PUEDE MODIFICAR CLIENTE Y/O PRODUCTOS.');
}

function borrarCotizacion(index) {
  if (!confirm('¿Está seguro que desea borrar esta cotización?')) return;
  cotizacionesEmitidas.splice(index, 1);
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  mostrarCotizaciones();
}

function cerrarCotizaciones() {
  document.getElementById('modalCotizaciones').style.display = 'none';
}

</script>
</body>
</html>
