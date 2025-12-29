<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Cotizador - EMPRESA CUNDO</title>
<style>
* {margin: 0; padding: 0; box-sizing: border-box;}
html, body {width: 100%; height: 100%;}
body {font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #F5F5F5; padding: 10px; color: #3B3B3B;}
.cotizador-container {max-width: 1200px; margin: 0 auto; background: #fff; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 15px; border-radius: 8px;}
.cotizador-header {background: #1F6F8B; color: white; padding: 8px 15px; display: flex; justify-content: space-between; align-items: center; border-bottom: 3px solid #F25C05; border-radius: 6px; margin-bottom: 15px; flex-wrap: wrap; gap: 10px;}
.empresa-nombre {font-weight: 700; font-size: 14px; letter-spacing: 1px;}
.numero-cotizacion {font-weight: 900; color: white; font-size: 12px;}
.btn-config {background: #D9822B; color: white; padding: 6px 10px; font-size: 9px; border: none; cursor: pointer; border-radius: 2px; font-weight: 700; transition: all 0.3s;}
.btn-config:hover {background: #b36e1e;}
.seccion-titulo {font-weight: 700; font-size: 16px; margin-bottom: 15px; padding-bottom: 8px; border-bottom: 2px solid #F25C05; text-transform: uppercase; color: #1F6F8B;}
button {cursor: pointer; border: none; border-radius: 2px; font-weight: 700; text-transform: uppercase; transition: all 0.3s ease; padding: 6px 10px; font-size: 10px;}
.btn-buscar {background: #F25C05; color: white;}
.btn-buscar:hover {background: #cb4a04;}
.btn-limpiar {background: #7A8C52; color: white;}
.btn-limpiar:hover {background: #5a6b37;}
.btn-eliminar {background: #9B2E00; color: white; font-size: 9px; padding: 4px 6px;}
.btn-articulos {background: #4B732E; color: white;}
.btn-articulos:hover {background: #385525;}
.btn-pdf {background: #F25C05; color: white; padding: 8px 12px; font-size: 11px;}
.btn-agregar {background: #385525; color: white;}
.btn-editar {background: #D9822B; color: white;}
.btn-cancelar {background: #9B2E00; color: white;}
.btn-reportes {background: #7b1fa2; color: white;}
.btn-enviar {background: #1976d2; color: white;}
.btn-exportar {background: #1976d2; color: white;}
.btn-importar {background: #388e3c; color: white;}
.formulario-cliente, .formulario-producto {display: none;}
.formulario-cliente.activo, .formulario-producto.activo {display: block;}
.campo-grupo {margin-bottom: 12px;}
.campo-grupo label {display: block; font-weight: 700; color: #3B3B3B; margin-bottom: 4px; font-size: 11px; text-transform: uppercase;}
.campo-grupo input, .campo-grupo select {width: 100%; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase; font-family: inherit;}
.campo-grupo input:focus, .campo-grupo select:focus {outline: none; border-color: #F25C05;}
.campo-grupo input:disabled {background-color: #F5F5F5; cursor: not-allowed; color: #777;}
.campo-grupo input.error {border-color: #9B2E00 !important; background-color: #ffebee !important;}
.campo-grupo input.valido {border-color: #4B732E !important; background-color: #e8f5e9 !important;}
.validacion-msg {font-size: 9px; margin-top: 3px; display: none; font-weight: 600;}
.validacion-msg.error {color: #9B2E00; display: block;}
.validacion-msg.exito {color: #4B732E; display: block;}
.fila-campos {display: grid; grid-template-columns: 1fr 1fr; gap: 12px;}
@media (max-width: 768px) {.fila-campos {grid-template-columns: 1fr;}}
.mensaje {padding: 10px 12px; border-radius: 2px; margin-bottom: 12px; font-size: 11px; text-transform: uppercase; font-weight: 700;}
.mensaje-exito {background-color: #d0e8d0; color: #385525; border: 1px solid #8bb76f;}
.mensaje-error {background-color: #fbd7d2; color: #9b2e00; border: 1px solid #e4827b;}
.mensaje-info {background-color: #e2f0ec; color: #4b732e; border: 1px solid #a7c3a1;}
.resumen-cliente {display: none; background-color: #e8f0e4; padding: 12px; border-radius: 2px; border-left: 5px solid #4B732E;}
.resumen-cliente.activo {display: block;}
.resumen-cliente h4 {color: #3B3B3B; margin-bottom: 8px; text-transform: uppercase; font-weight: 700; font-size: 12px;}
.resumen-cliente p {margin: 3px 0; font-size: 11px; color: #4B732E; text-transform: uppercase;}
.botones-resumen {margin-top: 10px; display: flex; gap: 6px; flex-wrap: wrap;}
.botones-formulario {display: flex; gap: 6px; margin-top: 8px; flex-wrap: wrap;}
.seccion-cliente {margin-bottom: 20px; border: 1px solid #ddd; border-radius: 2px; padding: 15px; background-color: #fafafa;}
.busqueda-rut {display: flex; gap: 6px; margin-bottom: 15px; align-items: center; flex-wrap: wrap;}
.contenedor-rut {flex: 1; min-width: 150px; display: flex; align-items: center; gap: 6px;}
.btn-lupa {background: transparent; border: none; color: #4B732E; padding: 4px; font-size: 20px; cursor: pointer;}
.seccion-productos {margin-bottom: 20px; border: 1px solid #ddd; border-radius: 2px; padding: 15px; background-color: #fafafa; overflow-x: auto;}
.busqueda-producto {display: flex; gap: 6px; margin-bottom: 15px; align-items: center; flex-wrap: wrap;}
.busqueda-producto input {flex: 1; min-width: 150px; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px;}
.formulario-producto {background: linear-gradient(135deg, #f9ead4 0%, #f7dba1 100%); padding: 12px; border-radius: 2px; border-left: 5px solid #F25C05; margin-bottom: 15px;}
.formulario-producto h4 {text-transform: uppercase; color: #b35304; margin-bottom: 12px; font-weight: 700; font-size: 12px;}
.alerta-sin-productos {background-color: #e9f6ec; padding: 12px; border-radius: 2px; border-left: 5px solid #4B732E; text-align: center; color: #4B732E; text-transform: uppercase; font-weight: 700; font-size: 11px;}
.descuento-global {background: #e3f2fd; padding: 12px; border-radius: 2px; border-left: 5px solid #1976d2; margin-bottom: 15px; display: none;}
.descuento-global.activo {display: block;}
.descuento-global h4 {color: #1976d2; margin-bottom: 8px; font-weight: 700; text-transform: uppercase; font-size: 12px;}
.error-desc {color: #9B2E00; font-weight: 700; margin-top: 8px; padding: 8px; background-color: #ffebee; border-radius: 2px; display: none;}
.error-desc.mostrar {display: block;}
.seccion-validez {background: #f3e5f5; padding: 12px; border-radius: 2px; border-left: 5px solid #7b1fa2; margin-bottom: 15px; display: none;}
.seccion-validez.activo {display: block;}
.seccion-validez h4 {color: #7b1fa2; margin-bottom: 8px; font-weight: 700; text-transform: uppercase; font-size: 12px;}
.resumen-totales {max-width: 350px; margin-left: auto; border-top: 3px solid #F25C05; padding-top: 12px; margin-bottom: 15px; font-size: 12px; display: none;}
.resumen-linea {display: flex; justify-content: space-between; margin: 4px 0; font-weight: 700; font-size: 11px; color: #3B3B3B;}
.resumen-linea.total {font-size: 14px; color: #000; font-weight: 900;}
table {width: 100%; border-collapse: collapse; margin-bottom: 15px; font-size: 11px;}
th, td {border: 1px solid #ddd; padding: 6px; text-transform: uppercase; color: #3B3B3B;}
th {background: #1F6F8B; color: white; font-weight: 700;}
tr:nth-child(even) {background: #E9F0EA;}
.valor-numerico {text-align: right; font-weight: 700; color: #3B3B3B;}
.margen-verde {color: #2E7D32; font-weight: 700;}
.margen-roja {color: #C62828; font-weight: 700;}
#modalConfig, #modalArticulos, #modalCotizaciones, #modalReportes, #modalBackup, #modalClientes, #modalPDF {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 9999; overflow: auto; padding: 10px;}
.modal-content {background: white; max-width: 95%; margin: 20px auto; padding: 15px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); border-radius: 2px;}
.btn-cerrar {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.tabla-cotizaciones {width: 100%; border-collapse: collapse; background: white;}
.tabla-cotizaciones tr.cot-borrador {background-color: #FFF9C4 !important;}
.tabla-cotizaciones tr.cot-enviada {background-color: #E1F5FE !important;}
.tabla-cotizaciones tr.cot-aceptado {background-color: #C8E6C9 !important;}
.tabla-cotizaciones tr.cot-rechazado {background-color: #FFCDD2 !important;}
.badge-estado {display: inline-block; padding: 3px 6px; border-radius: 2px; font-size: 9px; font-weight: 700; text-transform: uppercase;}
.badge-borrador {background: #FFA500; color: white;}
.badge-enviada {background: #1976d2; color: white;}
.badge-aceptado {background: #4B732E; color: white;}
.badge-rechazado {background: #9B2E00; color: white;}
.seccion-cierre {margin-top: 20px; border: 1px solid #ddd; border-radius: 2px; padding: 15px; background-color: #fafafa; display: none;}
.seccion-cierre.activo {display: block;}
.botones-cierre {display: flex; gap: 8px; margin-top: 12px; flex-wrap: wrap;}
.seccion-bloqueada {background-color: #fff3cd; padding: 12px; border-radius: 2px; border-left: 5px solid #FFA500; margin-bottom: 15px; display: none;}
.seccion-bloqueada.activa {display: block;}
.seccion-bloqueada h4 {color: #856404; text-transform: uppercase; font-weight: 700; font-size: 12px;}
.seccion-bloqueada p {color: #856404; font-size: 10px; text-transform: uppercase;}
</style>
</head>
<body>
<div class="cotizador-container">
<header class="cotizador-header">
<div class="empresa-nombre" id="empresaNombre">EMPRESA CUNDO</div>
<div class="numero-cotizacion">N° <span id="numeroCotizacion">CO100500</span></div>
<div><button class="btn-config" onclick="abrirConfiguracion()">⚙️ CONFIG</button></div>
</header>

<div style="display: flex; gap: 6px; margin-bottom: 15px; flex-wrap: wrap;">
<button class="btn btn-articulos" onclick="abrirArticulos()">ARTÍCULOS</button>
<button class="btn btn-reportes" onclick="abrirReportes()">📊 REPORTES</button>
<button class="btn btn-buscar" onclick="mostrarCotizaciones()">COTIZACIONES</button>
<button class="btn btn-exportar" onclick="abrirBackup()">💾 BACKUP</button>
</div>

<div id="seccionBloqueada" class="seccion-bloqueada">
<h4>🔒 COTIZACIÓN EN MODO LECTURA</h4>
<p>Puede revisar productos, costos y proveedores. No se pueden hacer modificaciones.</p>
</div>

<section class="seccion-cliente">
<h2 class="seccion-titulo">DATOS DEL CLIENTE</h2>
<div class="busqueda-rut">
<div class="contenedor-rut">
<input type="text" id="inputRut" placeholder="INGRESE RUT (EJ: 78070615-7)" maxlength="12" />
<button class="btn-lupa" onclick="abrirModalClientes()">🔍</button>
</div>
<button class="btn btn-buscar" onclick="buscarCliente()">BUSCAR</button>
<button class="btn btn-limpiar" onclick="limpiarCotizacion()">LIMPIAR</button>
</div>
<div id="mensaje"></div>
<div id="resumenCliente" class="resumen-cliente"></div>
<div id="formularioCliente" class="formulario-cliente">
<div class="campo-grupo">
<label>RUT *</label>
<input type="text" id="rut" disabled />
<div class="validacion-msg"></div>
</div>
<div class="campo-grupo"><label>RAZÓN SOCIAL *</label><input type="text" id="razonSocial" /></div>
<div class="campo-grupo"><label>GIRO *</label><input type="text" id="giro" /></div>
<div class="campo-grupo"><label>DIRECCIÓN *</label><input type="text" id="direccion" /></div>
<div class="fila-campos">
<div class="campo-grupo"><label>NOMBRE CONTACTO *</label><input type="text" id="nombreContacto" /></div>
<div class="campo-grupo"><label>CELULAR *</label><input type="tel" id="celular" placeholder="+56912345678" onchange="validarTelefono(this)" /><div class="validacion-msg"></div></div>
</div>
<div class="campo-grupo"><label>MAIL *</label><input type="email" id="mail" onchange="validarEmail(this)" /><div class="validacion-msg"></div></div>
<div class="campo-grupo"><label>MEDIO PAGO *</label><select id="medioPago"><option value="">SELECCIONE</option><option value="TRANSFERENCIA">TRANSFERENCIA</option><option value="WEBPAY">WEBPAY</option><option value="CHEQUE 30 DÍAS">CHEQUE 30 DÍAS</option><option value="OC 30 DÍAS">OC 30 DÍAS</option><option value="EFECTIVO">EFECTIVO</option></select></div>
<div class="botones-formulario">
<button class="btn btn-buscar" onclick="guardarCliente()">GUARDAR CLIENTE</button>
<button class="btn btn-cancelar" onclick="cancelarEdicion()" style="display:none;" id="btnCancelar">CANCELAR</button>
</div>
</div>
</section>

<section class="seccion-productos">
<h2 class="seccion-titulo">PRODUCTOS Y/O SERVICIOS</h2>
<div class="busqueda-producto">
<input type="text" id="inputCodigoProducto" placeholder="INGRESE CÓDIGO O DESCRIPCIÓN" maxlength="50" />
<button class="btn btn-buscar" onclick="buscarProducto()">BUSCAR</button>
</div>
<div id="formularioProducto" class="formulario-producto">
<h4>CREAR NUEVO PRODUCTO/SERVICIO</h4>
<div class="fila-campos">
<div class="campo-grupo"><label>CÓDIGO *</label><input type="text" id="codigoProducto" disabled /></div>
<div class="campo-grupo"><label>DESCRIPCIÓN *</label><input type="text" id="descripcionProducto" /></div>
</div>
<div class="fila-campos">
<div class="campo-grupo"><label>COSTO (SIN IVA) *</label><input type="number" id="costoProducto" min="0" step="0.01" /></div>
<div class="campo-grupo"><label>PRECIO NETO (SIN IVA) *</label><input type="number" id="valorTotalProducto" min="0" step="0.01" /></div>
</div>
<div class="campo-grupo"><label>% MARGEN</label><input type="number" id="porcentajeProducto" disabled /></div>
<div class="campo-grupo"><label>LINK *</label><input type="text" id="linkProducto" /></div>
<div class="botones-formulario">
<button class="btn btn-agregar" onclick="guardarProducto()">GUARDAR PRODUCTO</button>
<button class="btn btn-cancelar" onclick="cancelarProducto()">CANCELAR</button>
</div>
</div>

<div id="descuentoGlobal" class="descuento-global">
<h4>💰 DESCUENTO GLOBAL</h4>
<div class="fila-campos">
<div class="campo-grupo"><label>TIPO DESCUENTO</label><select id="tipoDescuento" onchange="actualizarTablaProductos()"><option value="porcentaje">PORCENTAJE (%)</option><option value="monto">MONTO ($)</option></select></div>
<div class="campo-grupo"><label>VALOR DESCUENTO</label><input type="number" id="valorDescuento" min="0" step="0.01" onchange="validarDescuentoGlobal()" /></div>
</div>
<div id="errorDescuento" class="error-desc"></div>
</div>

<div id="tablaProductosContenedor">
<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS</div>
</div>

<div id="resumenTotales" class="resumen-totales">
<div class="resumen-linea"><div>NETO</div><div id="totalNeto">$0.00</div></div>
<div class="resumen-linea"><div>DESCUENTO</div><div id="totalDescuento">$0.00</div></div>
<div class="resumen-linea"><div>NETO FINAL</div><div id="totalNetoFinal">$0.00</div></div>
<div class="resumen-linea"><div>IVA (19%)</div><div id="totalIva">$0.00</div></div>
<div class="resumen-linea total"><div>TOTAL</div><div id="totalGeneral">$0.00</div></div>
</div>

<div id="seccionValidez" class="seccion-validez">
<h4>📅 VALIDEZ DE OFERTA Y TIEMPO DE ENTREGA</h4>
<div class="fila-campos">
<div class="campo-grupo"><label>VÁLIDA HASTA *</label><input type="date" id="fechaValidez" /></div>
<div class="campo-grupo"><label>TIEMPO ENTREGA (DÍAS) *</label><input type="number" id="tiempoEntrega" min="0" step="1" placeholder="0" /></div>
</div>
</div>

<div style="text-align:center; margin-top: 15px;">
<button class="btn btn-pdf" onclick="generarPDF()">GENERAR PDF</button>
</div>
</section>

<section class="seccion-cierre" id="seccionCierre">
<h2 class="seccion-titulo">DATOS DE CIERRE</h2>
<div id="estadoCotizacion"></div>
<div class="botones-cierre">
<button class="btn-enviar" onclick="enviarCotizacion()">📤 ENVIAR COTIZACIÓN</button>
<button class="btn-buscar" onclick="marcarAceptado()" style="background:#4B732E;">✅ ACEPTADA</button>
<button class="btn-cancelar" onclick="marcarRechazado()">❌ RECHAZADA</button>
<button class="btn-limpiar" onclick="limpiarCotizacion()">LIMPIAR</button>
</div>
</section>
</div>

<!-- MODALES -->
<div id="modalConfig">
<div class="modal-content" style="max-width:600px;">
<button class="btn-cerrar" onclick="cerrarConfiguracion()">×</button>
<div style="font-size:16px; font-weight:700; margin-bottom:15px; text-transform:uppercase; border-bottom:3px solid #D9822B; padding-bottom:8px;">⚙️ CONFIGURACIÓN DE EMPRESA</div>
<div class="campo-grupo"><label>NOMBRE EMPRESA *</label><input type="text" id="configNombreEmpresa" /></div>
<div class="campo-grupo"><label>RUT EMPRESA *</label><input type="text" id="configRutEmpresa" /></div>
<div class="campo-grupo"><label>DIRECCIÓN *</label><input type="text" id="configDireccion" /></div>
<div class="campo-grupo"><label>TELÉFONO *</label><input type="text" id="configTelefono" /></div>
<div class="campo-grupo"><label>EMAIL *</label><input type="email" id="configEmail" /></div>
<div style="display: flex; gap: 6px; margin-top: 15px; justify-content: flex-end; flex-wrap: wrap;">
<button class="btn btn-cancelar" onclick="cerrarConfiguracion()">CANCELAR</button>
<button class="btn btn-agregar" onclick="guardarConfiguracion()">GUARDAR</button>
</div>
</div>
</div>

<div id="modalArticulos">
<div class="modal-content">
<button class="btn-cerrar" onclick="cerrarArticulos()">×</button>
<div style="font-size:16px; font-weight:700; margin-bottom:15px; text-transform:uppercase;">ARTÍCULOS</div>
<div style="margin-bottom:12px;">
<input type="text" id="inputBuscador" placeholder="BUSCAR" style="flex:1; padding:8px; border:2px solid #ddd; width:100%; margin-bottom:8px;" />
<button class="btn btn-buscar" onclick="filtrarArticulos()" style="margin-right:6px;">BUSCAR</button>
<button class="btn btn-limpiar" onclick="limpiarBuscador()">LIMPIAR</button>
</div>
<div id="tablaArticulosContenedor" style="overflow-x:auto;"></div>
</div>
</div>

<div id="modalCotizaciones">
<div class="modal-content">
<button class="btn-cerrar" onclick="cerrarCotizaciones()">×</button>
<h2 style="margin-bottom:12px; text-transform:uppercase; color:#3B3B3B;">COTIZACIONES EMITIDAS</h2>
<div id="listaCotizaciones" style="overflow-x:auto;"></div>
</div>
</div>

<div id="modalReportes">
<div class="modal-content">
<button class="btn-cerrar" onclick="cerrarReportes()">×</button>
<h2 style="margin-bottom:15px; text-transform:uppercase; color:#3B3B3B; border-bottom:3px solid #7b1fa2; padding-bottom:8px;">📊 REPORTES</h2>
<div id="contenidoReportes" style="overflow-x:auto;"></div>
</div>
</div>

<div id="modalBackup">
<div class="modal-content" style="max-width:500px;">
<button class="btn-cerrar" onclick="cerrarBackup()">×</button>
<div style="font-size:16px; font-weight:700; margin-bottom:15px; text-transform:uppercase; border-bottom:3px solid #388e3c; padding-bottom:8px;">💾 RESPALDO DE DATOS</div>
<div style="display: flex; gap: 6px; margin-top: 15px; flex-wrap: wrap;">
<button class="btn btn-exportar" onclick="exportarDatos()">⬇️ DESCARGAR</button>
<button class="btn btn-importar" onclick="document.getElementById('inputImportar').click()">⬆️ RESTAURAR</button>
<input type="file" id="inputImportar" accept=".json" onchange="importarDatos(event)" style="display:none;" />
</div>
<div id="mensajeBackup" style="margin-top:12px;"></div>
</div>
</div>

<div id="modalClientes">
<div class="modal-content">
<button class="btn-cerrar" onclick="cerrarModalClientes()">×</button>
<h2 style="margin-bottom:12px; text-transform:uppercase;">SELECCIONAR CLIENTE</h2>
<div id="listaClientesModal" style="overflow-x:auto;"></div>
</div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>

<script>
const configDefault = {nombre: 'EMPRESA CUNDO', rut: '78000256-0', direccion: 'Los Pepinos 287, Las Condes', telefono: '56 22 5510365', email: 'contacto@servicios.cl'};
let descuento = {tipo: 'porcentaje', valor: 0};
let cliente = null, modoEdicion = false, productos = [], cotizaciones = JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]');
let estado = 'borrador', fechaValidez = null, tiempoEntrega = 0, pdfGen = false, cotIndex = null, esLectura = false, esEdicion = false;

function cargarConfig() {
  const config = JSON.parse(localStorage.getItem('configEmpresa')) || configDefault;
  document.getElementById('empresaNombre').textContent = config.nombre;
  return config;
}

function abrirConfiguracion() {
  const config = JSON.parse(localStorage.getItem('configEmpresa')) || configDefault;
  document.getElementById('configNombreEmpresa').value = config.nombre;
  document.getElementById('configRutEmpresa').value = config.rut;
  document.getElementById('configDireccion').value = config.direccion;
  document.getElementById('configTelefono').value = config.telefono;
  document.getElementById('configEmail').value = config.email;
  document.getElementById('modalConfig').style.display = 'block';
}

function cerrarConfiguracion() {
  document.getElementById('modalConfig').style.display = 'none';
}

function guardarConfiguracion() {
  const config = {
    nombre: document.getElementById('configNombreEmpresa').value.toUpperCase() || 'EMPRESA CUNDO',
    rut: document.getElementById('configRutEmpresa').value,
    direccion: document.getElementById('configDireccion').value.toUpperCase(),
    telefono: document.getElementById('configTelefono').value,
    email: document.getElementById('configEmail').value
  };
  localStorage.setItem('configEmpresa', JSON.stringify(config));
  document.getElementById('empresaNombre').textContent = config.nombre;
  mostrarMensaje('CONFIGURACIÓN GUARDADA ✓', 'exito');
  cerrarConfiguracion();
}

function validarRut(rut) {
  if (!/^[0-9]{7,8}-[0-9kK]$/.test(rut)) return false;
  const [numeros, digito] = rut.split('-');
  const suma = [...numeros].reverse().reduce((acc, n, i) => acc + parseInt(n) * ((i % 6) + 2), 0);
  const dv = (11 - (suma % 11)) % 11;
  const dvEsperado = digito.toUpperCase() === 'K' ? 10 : parseInt(digito);
  return dv === dvEsperado;
}

function validarTelefono(input) {
  const valor = input.value.trim();
  const regex = /^(\+56|56)?[\s]?9?[\s]?\d{8}$/;
  const msg = input.nextElementSibling;
  if (!valor) {input.classList.remove('error', 'valido'); msg.textContent = ''; return;}
  if (regex.test(valor)) {
    input.classList.add('valido');
    input.classList.remove('error');
    msg.textContent = '✓ Válido';
    msg.className = 'validacion-msg exito';
  } else {
    input.classList.add('error');
    input.classList.remove('valido');
    msg.textContent = '❌ Formato: +56912345678';
    msg.className = 'validacion-msg error';
  }
}

function validarEmail(input) {
  const valor = input.value.trim();
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const msg = input.nextElementSibling;
  if (!valor) {input.classList.remove('error', 'valido'); msg.textContent = ''; return;}
  if (regex.test(valor)) {
    input.classList.add('valido');
    input.classList.remove('error');
    msg.textContent = '✓ Válido';
    msg.className = 'validacion-msg exito';
  } else {
    input.classList.add('error');
    input.classList.remove('valido');
    msg.textContent = '❌ Email inválido';
    msg.className = 'validacion-msg error';
  }
}

function validarRutInput(input) {
  const valor = input.value.trim();
  const msg = input.nextElementSibling;
  if (!valor) {input.classList.remove('error', 'valido'); msg.textContent = ''; return true;}
  if (validarRut(valor)) {
    input.classList.add('valido');
    input.classList.remove('error');
    msg.textContent = '✓ RUT Válido';
    msg.className = 'validacion-msg exito';
    return true;
  } else {
    input.classList.add('error');
    input.classList.remove('valido');
    msg.textContent = '❌ RUT inválido';
    msg.className = 'validacion-msg error';
    return false;
  }
}

function formatNum(valor) {
  return Math.round(parseFloat(valor) * 100) / 100;
}

function validarDescuentoGlobal() {
  const totalNeto = formatNum(productos.reduce((a, p) => a + parseFloat(p.totalNeto), 0));
  const tipo = document.getElementById('tipoDescuento').value;
  const valor = parseFloat(document.getElementById('valorDescuento').value) || 0;
  const errorDiv = document.getElementById('errorDescuento');
  
  if (valor === 0) {errorDiv.classList.remove('mostrar'); return true;}
  if (tipo === 'porcentaje' && (valor < 0 || valor > 100)) {
    errorDiv.textContent = '❌ Porcentaje debe estar entre 0% y 100%';
    errorDiv.classList.add('mostrar');
    return false;
  }
  
  const monto = tipo === 'porcentaje' ? totalNeto * (valor / 100) : valor;
  const netoFinal = totalNeto - monto;
  
  if (netoFinal <= 0) {
    errorDiv.textContent = '❌ ERROR: El descuento NO PUEDE ser igual o mayor al neto total. El neto final quedaría NEGATIVO o CERO.';
    errorDiv.classList.add('mostrar');
    return false;
  }
  
  errorDiv.classList.remove('mostrar');
  return true;
}

function obtenerMontoDesc() {
  if (descuento.valor === 0) return 0;
  const totalNeto = productos.reduce((a, p) => a + parseFloat(p.totalNeto), 0);
  return descuento.tipo === 'porcentaje' ? formatNum(totalNeto * (descuento.valor / 100)) : formatNum(descuento.valor);
}

function exportarDatos() {
  const datos = {
    clientes: JSON.parse(localStorage.getItem('clientes') || '{}'),
    productos: JSON.parse(localStorage.getItem('productos') || '{}'),
    cotizaciones: JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]'),
    config: JSON.parse(localStorage.getItem('configEmpresa') || JSON.stringify(configDefault)),
    fecha: new Date().toISOString()
  };
  const json = JSON.stringify(datos, null, 2);
  const blob = new Blob([json], {type: 'application/json'});
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `respaldo_${new Date().toISOString().split('T')[0]}.json`;
  link.click();
  URL.revokeObjectURL(url);
  mostrarMsjBackup('✓ DESCARGADO', 'exito');
}

function importarDatos(event) {
  const file = event.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const datos = JSON.parse(e.target.result);
      localStorage.setItem('clientes', JSON.stringify(datos.clientes || {}));
      localStorage.setItem('productos', JSON.stringify(datos.productos || {}));
      localStorage.setItem('cotizacionesEmitidas', JSON.stringify(datos.cotizaciones || []));
      if (datos.config) localStorage.setItem('configEmpresa', JSON.stringify(datos.config));
      mostrarMsjBackup('✓ RESTAURADO', 'exito');
      setTimeout(() => location.reload(), 2000);
    } catch (error) {
      mostrarMsjBackup('❌ ERROR: ' + error.message, 'error');
    }
  };
  reader.readAsText(file);
}

function mostrarMsjBackup(texto, tipo) {
  const m = document.getElementById('mensajeBackup');
  m.className = `mensaje mensaje-${tipo}`;
  m.textContent = texto;
}

function abrirBackup() {document.getElementById('modalBackup').style.display = 'block';}
function cerrarBackup() {document.getElementById('modalBackup').style.display = 'none';}

function abrirReportes() {
  generarReportes();
  document.getElementById('modalReportes').style.display = 'block';
}

function cerrarReportes() {document.getElementById('modalReportes').style.display = 'none';}

function generarReportes() {
  if (cotizaciones.length === 0) {
    document.getElementById('contenidoReportes').innerHTML = '<p style="text-align:center; padding:15px;">SIN COTIZACIONES</p>';
    return;
  }
  let totalIngresos = 0, totalCostos = 0, totalUtil = 0;
  let html = '<table class="tabla-reportes" style="background:white;"><thead><tr style="background:#7b1fa2; color:white;"><th style="padding:8px; text-align:left;">N° COT</th><th style="padding:8px;">CLIENTE</th><th style="padding:8px; text-align:right;">INGRESOS</th><th style="padding:8px; text-align:right;">COSTO</th><th style="padding:8px; text-align:right;">UTILIDAD</th><th style="padding:8px;">% MARGEN</th><th>ESTADO</th></tr></thead><tbody>';
  
  cotizaciones.forEach(cot => {
    let costoTotal = 0;
    const ingresoNeto = formatNum(parseFloat(cot.totalNeto));
    if (cot.productos) {
      costoTotal = formatNum(cot.productos.reduce((a, p) => a + (parseFloat(p.costo) * p.cantidad), 0));
    }
    const util = formatNum(ingresoNeto - costoTotal);
    const margen = ingresoNeto > 0 ? formatNum((util / ingresoNeto) * 100) : 0;
    totalIngresos += ingresoNeto;
    totalCostos += costoTotal;
    totalUtil += util;
    
    const badge = cot.estado === 'aceptado' ? 'badge-aceptado' : (cot.estado === 'rechazado' ? 'badge-rechazado' : (cot.estado === 'enviada' ? 'badge-enviada' : 'badge-borrador'));
    html += `<tr><td>${cot.numero}</td><td>${cot.razonSocial}</td><td style="text-align:right;">$${ingresoNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td style="text-align:right;">$${costoTotal.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td style="text-align:right;">$${util.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td style="text-align:center;">${margen.toFixed(2)}%</td><td><span class="badge-estado ${badge}">${cot.estado.toUpperCase()}</span></td></tr>`;
  });

  const margenTotal = totalIngresos > 0 ? formatNum((totalUtil / totalIngresos) * 100) : 0;
  html += `<tr style="background:#7b1fa2; color:white; font-weight:700;"><td colspan="2" style="padding:8px;"><strong>TOTALES</strong></td><td style="text-align:right; padding:8px;"><strong>$${formatNum(totalIngresos).toLocaleString('es-CL', {minimumFractionDigits: 2})}</strong></td><td style="text-align:right; padding:8px;"><strong>$${formatNum(totalCostos).toLocaleString('es-CL', {minimumFractionDigits: 2})}</strong></td><td style="text-align:right; padding:8px;"><strong>$${formatNum(totalUtil).toLocaleString('es-CL', {minimumFractionDigits: 2})}</strong></td><td style="text-align:center; padding:8px;"><strong>${margenTotal.toFixed(2)}%</strong></td><td></td></tr>`;
  html += '</tbody></table>';
  document.getElementById('contenidoReportes').innerHTML = html;
}

class GestorCotizaciones {
  constructor() {this.prefijo = 'CO'; this.numeroBase = 100500; this.cargar();}
  cargar() {const n = localStorage.getItem('ultimaCotizacion'); if (n) this.numeroBase = parseInt(n); this.actualizar();}
  obtener() {return `${this.prefijo}${this.numeroBase}`;}
  siguiente() {this.numeroBase++; localStorage.setItem('ultimaCotizacion', this.numeroBase); this.actualizar(); return this.obtener();}
  actualizar() {const el = document.getElementById('numeroCotizacion'); if (el) el.textContent = this.obtener();}
  establecer(numero) {this.numeroBase = parseInt(numero.replace(this.prefijo, '')); this.actualizar();}
}

class GestorClientes {
  constructor() {this.datos = this.cargar();}
  cargar() {const c = localStorage.getItem('clientes'); return c ? JSON.parse(c) : {};}
  guardar() {localStorage.setItem('clientes', JSON.stringify(this.datos));}
  obtener(rut) {return this.datos[rut] || null;}
  agregar(rut, datos) {this.datos[rut] = datos; this.guardar();}
  actualizar(rut, datos) {this.datos[rut] = datos; this.guardar();}
  obtenerTodos() {return this.datos;}
}

class GestorProductos {
  constructor() {this.datos = this.cargar();}
  cargar() {const p = localStorage.getItem('productos'); return p ? JSON.parse(p) : {};}
  guardar() {localStorage.setItem('productos', JSON.stringify(this.datos));}
  obtener(codigo) {return this.datos[codigo] || null;}
  agregar(codigo, datos) {this.datos[codigo] = datos; this.guardar();}
  obtenerTodos() {return this.datos;}
  buscar(termino) {termino = termino.toUpperCase().trim(); return Object.values(this.datos).filter(prod => prod.codigo.includes(termino) || prod.descripcion.includes(termino)).slice(0, 10);}
}

const gestorCot = new GestorCotizaciones();
const gestorCli = new GestorClientes();
const gestorProd = new GestorProductos();

document.addEventListener('DOMContentLoaded', function() {
  cargarConfig();
  document.getElementById('inputRut').addEventListener('input', function(e) {
    let v = e.target.value.replace(/[^0-9kK-]/g, '');
    if (v.length > 1 && v[v.length - 2] !== '-') v = v.slice(0, -1) + '-' + v.slice(-1);
    e.target.value = v.toUpperCase();
  });
  document.getElementById('inputRut').addEventListener('change', function(e) {validarRutInput(e.target);});
  document.getElementById('inputRut').addEventListener('keydown', function(e) {if (e.key === 'Enter') buscarCliente();});
  document.getElementById('costoProducto').addEventListener('change', calcularMargen);
  document.getElementById('valorTotalProducto').addEventListener('change', calcularMargen);
});

function calcularMargen() {
  const costo = parseFloat(document.getElementById('costoProducto').value) || 0;
  const precio = parseFloat(document.getElementById('valorTotalProducto').value) || 0;
  if (costo > 0 && precio > 0) {
    const margen = precio < costo ? 0 : +((precio - costo) / precio * 100).toFixed(2);
    document.getElementById('porcentajeProducto').value = margen;
  } else {
    document.getElementById('porcentajeProducto').value = '0.00';
  }
}

function buscarCliente() {
  if (esLectura) {mostrarMensaje('MODO LECTURA', 'info'); return;}
  const rut = document.getElementById('inputRut').value.trim();
  if (!rut) return mostrarMensaje('INGRESE RUT', 'error');
  if (!validarRut(rut)) return mostrarMensaje('RUT INVÁLIDO', 'error');
  const cli = gestorCli.obtener(rut);
  if (cli) {
    cliente = cli;
    mostrarResumenCliente(cli);
    document.getElementById('formularioCliente').classList.remove('activo');
    habilitarProductos();
    mostrarMensaje('CLIENTE ENCONTRADO ✓', 'exito');
  } else {
    mostrarFormulario(rut);
    mostrarMensaje('CLIENTE NO ENCONTRADO. CREE UNO NUEVO.', 'info');
  }
}

function mostrarFormulario(rut) {
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('formularioCliente').classList.add('activo');
  document.getElementById('rut').value = rut;
  document.getElementById('razonSocial').value = '';
  document.getElementById('giro').value = '';
  document.getElementById('direccion').value = '';
  document.getElementById('nombreContacto').value = '';
  document.getElementById('celular').value = '';
  document.getElementById('mail').value = '';
  document.getElementById('medioPago').value = '';
}

function mostrarResumenCliente(cli) {
  const r = document.getElementById('resumenCliente');
  r.className = 'resumen-cliente activo';
  r.innerHTML = `<h4>✓ CLIENTE REGISTRADO</h4>
    <p><strong>RUT:</strong> ${cli.rut}</p>
    <p><strong>RAZÓN SOCIAL:</strong> ${cli.razonSocial}</p>
    <p><strong>GIRO:</strong> ${cli.giro}</p>
    <p><strong>DIRECCIÓN:</strong> ${cli.direccion}</p>
    <p><strong>CONTACTO:</strong> ${cli.nombreContacto} | ${cli.celular}</p>
    <p><strong>MAIL:</strong> ${cli.mail}</p>
    <p><strong>MEDIO PAGO:</strong> ${cli.medioPago}</p>
    <div class="botones-resumen"><button class="btn btn-editar" onclick="editarCliente()">✏️ EDITAR</button></div>`;
}

function editarCliente() {
  if (!cliente) return;
  modoEdicion = true;
  document.getElementById('formularioCliente').classList.add('activo');
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('rut').value = cliente.rut;
  document.getElementById('razonSocial').value = cliente.razonSocial;
  document.getElementById('giro').value = cliente.giro;
  document.getElementById('direccion').value = cliente.direccion;
  document.getElementById('nombreContacto').value = cliente.nombreContacto;
  document.getElementById('celular').value = cliente.celular;
  document.getElementById('mail').value = cliente.mail;
  document.getElementById('medioPago').value = cliente.medioPago;
  document.getElementById('btnCancelar').style.display = 'inline-block';
}

function cancelarEdicion() {
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarResumenCliente(cliente);
  document.getElementById('btnCancelar').style.display = 'none';
}

function guardarCliente() {
  const rut = document.getElementById('rut').value;
  const rs = document.getElementById('razonSocial').value.trim();
  const gi = document.getElementById('giro').value.trim();
  const di = document.getElementById('direccion').value.trim();
  const nc = document.getElementById('nombreContacto').value.trim();
  const ce = document.getElementById('celular').value.trim();
  const ma = document.getElementById('mail').value.trim();
  const mp = document.getElementById('medioPago').value;
  
  if (!rs || !gi || !di || !nc || !ce || !ma || !mp) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  
  const msgEmail = document.getElementById('mail').nextElementSibling;
  const msgTel = document.getElementById('celular').nextElementSibling;
  if (msgEmail && msgEmail.classList.contains('error')) return mostrarMensaje('EMAIL INVÁLIDO', 'error');
  if (msgTel && msgTel.classList.contains('error')) return mostrarMensaje('TELÉFONO INVÁLIDO', 'error');
  
  const cli = {rut, razonSocial: rs.toUpperCase(), giro: gi.toUpperCase(), direccion: di.toUpperCase(), nombreContacto: nc.toUpperCase(), celular: ce, mail: ma.toUpperCase(), medioPago: mp};
  if (modoEdicion) gestorCli.actualizar(rut, cli);
  else gestorCli.agregar(rut, cli);
  
  cliente = cli;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarResumenCliente(cli);
  habilitarProductos();
  mostrarMensaje('CLIENTE GUARDADO ✓', 'exito');
}

function habilitarProductos() {
  if (esLectura) {document.getElementById('inputCodigoProducto').disabled = true; return;}
  document.getElementById('inputCodigoProducto').disabled = false;
}

function limpiarCotizacion() {
  document.getElementById('inputRut').value = '';
  document.getElementById('inputRut').disabled = false;
  document.getElementById('mensaje').innerHTML = '';
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('formularioCliente').classList.remove('activo');
  cliente = null;
  productos = [];
  estado = 'borrador';
  fechaValidez = null;
  tiempoEntrega = 0;
  pdfGen = false;
  cotIndex = null;
  esLectura = false;
  esEdicion = false;
  descuento = {tipo: 'porcentaje', valor: 0};
  document.getElementById('tipoDescuento').value = 'porcentaje';
  document.getElementById('valorDescuento').value = '';
  document.getElementById('fechaValidez').value = '';
  document.getElementById('tiempoEntrega').value = '';
  document.getElementById('seccionCierre').classList.remove('activo');
  document.getElementById('seccionValidez').classList.remove('activo');
  document.getElementById('seccionBloqueada').classList.remove('activa');
  document.getElementById('estadoCotizacion').innerHTML = '';
  actualizarTablaProductos();
  document.getElementById('inputRut').focus();
}

function buscarProducto() {
  if (esLectura) return mostrarMensaje('MODO LECTURA', 'info');
  const cod = document.getElementById('inputCodigoProducto').value.trim();
  if (!cod) return mostrarMensaje('INGRESE CÓDIGO', 'error');
  const prod = gestorProd.obtener(cod);
  if (prod) {
    agregarProducto(cod, prod);
    document.getElementById('inputCodigoProducto').value = '';
    mostrarMensaje('PRODUCTO ENCONTRADO ✓', 'exito');
    document.getElementById('formularioProducto').classList.remove('activo');
  } else {
    mostrarMensaje('NO ENCONTRADO. CREE UNO NUEVO.', 'info');
    document.getElementById('formularioProducto').classList.add('activo');
    document.getElementById('codigoProducto').value = cod;
    document.getElementById('descripcionProducto').value = '';
    document.getElementById('costoProducto').value = '';
    document.getElementById('valorTotalProducto').value = '';
    document.getElementById('porcentajeProducto').value = '';
    document.getElementById('linkProducto').value = '';
  }
}

function guardarProducto() {
  const cod = document.getElementById('codigoProducto').value.trim();
  const desc = document.getElementById('descripcionProducto').value.trim();
  const cos = formatNum(document.getElementById('costoProducto').value);
  const precio = formatNum(document.getElementById('valorTotalProducto').value);
  const link = document.getElementById('linkProducto').value.trim().toLowerCase();
  
  if (!cod || !desc || isNaN(cos) || cos <= 0 || isNaN(precio) || precio <= 0 || !link) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  if (precio < cos) return mostrarMensaje('PRECIO DEBE SER >= COSTO', 'error');
  
  const prod = {codigo: cod, descripcion: desc.toUpperCase(), costo: cos, precioNeto: precio, porcentaje: +((precio - cos) / precio * 100).toFixed(2), link: link};
  gestorProd.agregar(cod, prod);
  agregarProducto(cod, prod);
  cancelarProducto();
  mostrarMensaje('PRODUCTO GUARDADO ✓', 'exito');
}

function cancelarProducto() {
  document.getElementById('formularioProducto').classList.remove('activo');
  document.getElementById('inputCodigoProducto').value = '';
}

function agregarProducto(cod, prod) {
  if (esLectura) return mostrarMensaje('MODO LECTURA', 'info');
  const ex = productos.find(p => p.codigo === cod);
  if (ex) {
    ex.cantidad++;
    ex.totalNeto = formatNum(ex.cantidad * ex.precioNetoConDesc);
  } else {
    productos.push({codigo: cod, descripcion: prod.descripcion, cantidad: 1, precioNeto: prod.precioNeto, costo: prod.costo, descuento: 0, precioNetoConDesc: prod.precioNeto, totalNeto: prod.precioNeto, link: prod.link});
  }
  actualizarTablaProductos();
}

function actualizarTablaProductos() {
  const cont = document.getElementById('tablaProductosContenedor');
  if (productos.length === 0) {
    cont.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS</div>';
    document.getElementById('resumenTotales').style.display = 'none';
    document.getElementById('seccionValidez').classList.remove('activo');
    document.getElementById('descuentoGlobal').classList.remove('activo');
    return;
  }
  
  let html = '<div style="overflow-x:auto;"><table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANT</th><th>PRECIO UNIT</th><th>DESC%</th><th>PRECIO DESC</th><th>COSTO</th><th>COSTO TOTAL</th><th>TOTAL NETO</th><th>% MARGEN</th><th>UTILIDAD</th><th>CON IVA</th><th>ACCIÓN</th></tr></thead><tbody>';
  
  productos.forEach((p, i) => {
    const bloqueado = esLectura;
    const costoTotal = formatNum(parseFloat(p.costo) * p.cantidad);
    const precioUnitario = formatNum(parseFloat(p.precioNetoConDesc));
    const totalNeto = formatNum(p.totalNeto);
    const utilidad = formatNum(totalNeto - costoTotal);
    const margen = totalNeto > 0 ? formatNum((utilidad / totalNeto) * 100) : 0;
    const conIva = formatNum(totalNeto * 1.19);
    const claseMargen = margen >= 20 ? 'margen-verde' : 'margen-roja';
    
    html += `<tr>
      <td>${p.codigo}</td>
      <td>${p.descripcion}</td>
      <td><input type="number" min="1" value="${p.cantidad}" onchange="cambiarCant(${i}, this.value)" ${bloqueado ? 'disabled' : ''} style="width:60px; text-align:center; padding:4px;"></td>
      <td class="valor-numerico">$${precioUnitario.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td><input type="number" min="0" max="100" step="0.01" value="${p.descuento}" onchange="cambiarDesc(${i}, this.value)" ${bloqueado ? 'disabled' : ''} style="width:60px; text-align:center; padding:4px;"></td>
      <td class="valor-numerico">$${parseFloat(p.precioNetoConDesc).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${parseFloat(p.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${costoTotal.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${totalNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico ${claseMargen}">${margen.toFixed(2)}%</td>
      <td class="valor-numerico">$${utilidad.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${conIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td><button class="btn btn-eliminar" onclick="eliminarProd(${i})" ${bloqueado ? 'style="display:none;"' : ''}>DEL</button></td>
    </tr>`;
  });
  
  html += '</tbody></table></div>';
  cont.innerHTML = html;
  actualizarResumen();
  document.getElementById('descuentoGlobal').classList.add('activo');
  document.getElementById('seccionValidez').classList.add('activo');
}

function cambiarCant(i, cant) {
  cant = parseInt(cant);
  if (isNaN(cant) || cant < 1) {alert('CANTIDAD INVÁLIDA'); actualizarTablaProductos(); return;}
  productos[i].cantidad = cant;
  productos[i].totalNeto = formatNum(productos[i].precioNetoConDesc * cant);
  actualizarTablaProductos();
}

function cambiarDesc(i, desc) {
  desc = parseFloat(desc) || 0;
  if (desc < 0 || desc > 100) {alert('DESCUENTO ENTRE 0-100'); actualizarTablaProductos(); return;}
  productos[i].descuento = desc;
  const precioConDesc = productos[i].precioNeto * (1 - desc / 100);
  productos[i].precioNetoConDesc = formatNum(precioConDesc);
  productos[i].totalNeto = formatNum(productos[i].precioNetoConDesc * productos[i].cantidad);
  actualizarTablaProductos();
}

function eliminarProd(i) {
  if (esLectura) return;
  if (!confirm('¿Eliminar?')) return;
  productos.splice(i, 1);
  actualizarTablaProductos();
}

function actualizarResumen() {
  const totalNetoSinDesc = formatNum(productos.reduce((a, p) => a + parseFloat(p.totalNeto), 0));
  descuento.valor = parseFloat(document.getElementById('valorDescuento').value) || 0;
  descuento.tipo = document.getElementById('tipoDescuento').value;
  
  const montoDesc = obtenerMontoDesc();
  const totalNetoFinal = formatNum(totalNetoSinDesc - montoDesc);
  const iva = formatNum(totalNetoFinal * 0.19);
  const totalConIva = formatNum(totalNetoFinal + iva);
  
  document.getElementById('totalNeto').textContent = '$' + totalNetoSinDesc.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalDescuento').textContent = '$' + montoDesc.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalNetoFinal').textContent = '$' + totalNetoFinal.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalIva').textContent = '$' + iva.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalGeneral').textContent = '$' + totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('resumenTotales').style.display = 'block';
}

function abrirArticulos() {
  document.getElementById('modalArticulos').style.display = 'block';
  listarArticulos();
}

function cerrarArticulos() {
  document.getElementById('modalArticulos').style.display = 'none';
}

function listarArticulos() {
  const todos = gestorProd.obtenerTodos();
  const claves = Object.keys(todos);
  if (claves.length === 0) {
    document.getElementById('tablaArticulosContenedor').innerHTML = '<p style="text-align:center; padding:15px;">SIN ARTÍCULOS</p>';
    return;
  }
  let html = '<table style="font-size:10px; width:100%;"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO</th><th>PRECIO NETO</th><th>% MARGEN</th><th>UTILIDAD</th><th>CON IVA</th><th>LINK</th></tr></thead><tbody>';
  claves.forEach(cod => {
    const art = todos[cod];
    const util = formatNum(parseFloat(art.precioNeto) - parseFloat(art.costo));
    const margen = art.precioNeto > 0 ? formatNum((util / art.precioNeto) * 100) : 0;
    const conIva = formatNum(parseFloat(art.precioNeto) * 1.19);
    html += `<tr><td>${art.codigo}</td><td>${art.descripcion}</td><td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(art.precioNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">${margen.toFixed(2)}%</td><td class="valor-numerico">$${util.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${conIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td><a href="${art.link.startsWith('http') ? art.link : 'https://' + art.link}" target="_blank">VER</a></td></tr>`;
  });
  html += '</tbody></table>';
  document.getElementById('tablaArticulosContenedor').innerHTML = html;
}

function filtrarArticulos() {
  const termino = document.getElementById('inputBuscador').value.toUpperCase().trim();
  if (!termino) {listarArticulos(); return;}
  const todos = gestorProd.obtenerTodos();
  const filtrados = Object.entries(todos).filter(([cod, art]) => cod.includes(termino) || art.descripcion.includes(termino));
  if (filtrados.length === 0) {
    document.getElementById('tablaArticulosContenedor').innerHTML = '<p style="text-align:center;">SIN RESULTADOS</p>';
    return;
  }
  let html = '<table style="font-size:10px; width:100%;"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO</th><th>PRECIO NETO</th><th>% MARGEN</th><th>UTILIDAD</th><th>CON IVA</th><th>LINK</th></tr></thead><tbody>';
  filtrados.forEach(([cod, art]) => {
    const util = formatNum(parseFloat(art.precioNeto) - parseFloat(art.costo));
    const margen = art.precioNeto > 0 ? formatNum((util / art.precioNeto) * 100) : 0;
    const conIva = formatNum(parseFloat(art.precioNeto) * 1.19);
    html += `<tr><td>${art.codigo}</td><td>${art.descripcion}</td><td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(art.precioNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">${margen.toFixed(2)}%</td><td class="valor-numerico">$${util.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${conIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td><a href="${art.link.startsWith('http') ? art.link : 'https://' + art.link}" target="_blank">VER</a></td></tr>`;
  });
  html += '</tbody></table>';
  document.getElementById('tablaArticulosContenedor').innerHTML = html;
}

function limpiarBuscador() {
  document.getElementById('inputBuscador').value = '';
  listarArticulos();
}

function generarPDF() {
  if (!cliente) {alert('INGRESE CLIENTE'); return;}
  if (productos.length === 0) {alert('AGREGUE PRODUCTOS'); return;}
  
  fechaValidez = document.getElementById('fechaValidez').value;
  tiempoEntrega = parseInt(document.getElementById('tiempoEntrega').value) || 0;
  
  if (!fechaValidez) {alert('INGRESE FECHA VÁLIDA HASTA'); return;}
  if (tiempoEntrega <= 0) {alert('INGRESE TIEMPO DE ENTREGA'); return;}
  if (!validarDescuentoGlobal()) {alert('CORRIJA DESCUENTO'); return;}
  
  let numCot;
  if (esEdicion && cotIndex !== null) {
    numCot = cotizaciones[cotIndex].numero;
  } else {
    numCot = gestorCot.siguiente();
  }
  
  const totalNetoSinDesc = formatNum(productos.reduce((a, p) => a + parseFloat(p.totalNeto), 0));
  const montoDesc = obtenerMontoDesc();
  const totalNetoFinal = formatNum(totalNetoSinDesc - montoDesc);
  
  const cot = {
    numero: numCot,
    razonSocial: cliente.razonSocial,
    totalNeto: totalNetoFinal,
    fecha: new Date().toISOString(),
    cliente: JSON.parse(JSON.stringify(cliente)),
    productos: JSON.parse(JSON.stringify(productos)),
    estado: 'enviada',
    descuentoGlobal: descuento,
    fechaValidez: fechaValidez,
    tiempoEntrega: tiempoEntrega
  };
  
  if (esEdicion && cotIndex !== null) {
    cotizaciones[cotIndex] = cot;
  } else {
    cotizaciones.push(cot);
  }
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  
  pdfGen = true;
  estado = 'enviada';
  document.getElementById('seccionCierre').classList.add('activo');
  mostrarEstado();
  generarPDFDoc(cot);
}

function mostrarEstado() {
  const estados = {
    'borrador': {badge: 'badge-borrador', texto: '📝 BORRADOR'},
    'enviada': {badge: 'badge-enviada', texto: '📤 ENVIADA'},
    'aceptado': {badge: 'badge-aceptado', texto: '✅ ACEPTADA'},
    'rechazado': {badge: 'badge-rechazado', texto: '❌ RECHAZADA'}
  };
  const est = estados[estado] || estados['borrador'];
  document.getElementById('estadoCotizacion').innerHTML = `<div style="margin-bottom:12px;"><span class="badge-estado ${est.badge}">${est.texto}</span></div>`;
}

function enviarCotizacion() {
  if (!pdfGen) {alert('GENERE PDF PRIMERO'); return;}
  estado = 'enviada';
  mostrarEstado();
  if (cotIndex !== null) {
    cotizaciones[cotIndex].estado = 'enviada';
    localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  }
  alert('✓ ENVIADA');
}

function marcarAceptado() {
  if (!cliente) {alert('SELECCIONE CLIENTE'); return;}
  if (productos.length === 0) {alert('AGREGUE PRODUCTOS'); return;}
  if (!pdfGen) {alert('GENERE PDF'); return;}
  estado = 'aceptado';
  esLectura = true;
  mostrarEstado();
  document.getElementById('seccionBloqueada').classList.add('activa');
  habilitarProductos();
  if (cotIndex !== null) {
    cotizaciones[cotIndex].estado = 'aceptado';
    localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  }
  alert('✓ ACEPTADA');
}

function marcarRechazado() {
  if (!cliente) {alert('SELECCIONE CLIENTE'); return;}
  if (!pdfGen) {alert('GENERE PDF'); return;}
  estado = 'rechazado';
  mostrarEstado();
  document.getElementById('seccionBloqueada').classList.add('activa');
  if (cotIndex !== null) {
    cotizaciones[cotIndex].estado = 'rechazado';
    localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  }
  alert('✓ RECHAZADA');
}

function generarPDFDoc(cot) {
  try {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF('p', 'mm', 'a4');
    const config = JSON.parse(localStorage.getItem('configEmpresa')) || configDefault;
    
    const netoFinal = parseFloat(cot.totalNeto || 0);
    const iva = formatNum(netoFinal * 0.19);
    const total = formatNum(netoFinal + iva);
    
    const fechaEmision = new Date(cot.fecha);
    const fechaFormat = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
    
    const fechaValid = new Date(cot.fechaValidez);
    const fechaValidFormat = `${fechaValid.getDate().toString().padStart(2, '0')}/${(fechaValid.getMonth() + 1).toString().padStart(2, '0')}/${fechaValid.getFullYear()}`;
    
    doc.setFillColor(31, 111, 139);
    doc.rect(0, 0, 210, 20, 'F');
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(18);
    doc.setFont(undefined, 'bold');
    doc.text('COTIZACIÓN', 15, 12);
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    doc.text(`${cot.numero} | ${fechaFormat}`, 195, 12, {align: 'right'});
    
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(12);
    doc.setFont(undefined, 'bold');
    doc.text('DATOS DEL CLIENTE', 15, 32);
    doc.setDrawColor(242, 92, 5);
    doc.setLineWidth(0.5);
    doc.line(15, 35, 195, 35);
    
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    let yPos = 43;
    const datos = [
      ['RUT:', cot.cliente.rut, 'CONTACTO:', cot.cliente.nombreContacto],
      ['RAZÓN SOCIAL:', cot.cliente.razonSocial, 'CELULAR:', cot.cliente.celular],
      ['GIRO:', cot.cliente.giro, 'MAIL:', cot.cliente.mail],
      ['DIRECCIÓN:', cot.cliente.direccion, 'MEDIO PAGO:', cot.cliente.medioPago]
    ];
    
    datos.forEach(row => {
      doc.setFont(undefined, 'bold');
      doc.text(row[0], 15, yPos);
      doc.setFont(undefined, 'normal');
      doc.text(row[1], 45, yPos);
      doc.setFont(undefined, 'bold');
      doc.text(row[2], 110, yPos);
      doc.setFont(undefined, 'normal');
      doc.text(row[3], 145, yPos);
      yPos += 7;
    });
    
    yPos += 3;
    doc.setLineWidth(0.8);
    doc.line(15, yPos, 195, yPos);
    yPos += 8;
    doc.setFont(undefined, 'bold');
    doc.setFontSize(11);
    doc.setFillColor(243, 229, 245);
    doc.rect(15, yPos - 5, 180, 8, 'F');
    doc.text('📅 VALIDEZ DE OFERTA Y TIEMPO DE ENTREGA', 15, yPos);
    yPos += 10;
    
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    doc.text(`VÁLIDA HASTA: ${fechaValidFormat}`, 15, yPos);
    doc.text(`TIEMPO DE ENTREGA: ${cot.tiempoEntrega} DÍAS`, 110, yPos);
    yPos += 8;
    
    doc.setLineWidth(0.8);
    doc.line(15, yPos + 1, 195, yPos + 1);
    yPos += 8;
    
    doc.setFont(undefined, 'bold');
    doc.setFontSize(11);
    doc.text('PRODUCTOS Y SERVICIOS', 15, yPos);
    yPos += 5;
    
    doc.autoTable({
      startY: yPos,
      head: [['CÓDIGO', 'DESCRIPCIÓN', 'CANT.', 'PRECIO UNIT', 'TOTAL NETO', 'TOTAL CON IVA']],
      body: cot.productos.map(p => [
        p.codigo,
        p.descripcion,
        p.cantidad.toString(),
        `$${Math.round(parseFloat(p.precioNetoConDesc)).toLocaleString('es-CL')}`,
        `$${Math.round(parseFloat(p.totalNeto)).toLocaleString('es-CL')}`,
        `$${Math.round(parseFloat(p.totalNeto) * 1.19).toLocaleString('es-CL')}`
      ]),
      theme: 'striped',
      styles: {fontSize: 9, cellPadding: 4},
      headStyles: {fillColor: [31, 111, 139], textColor: 255, fontStyle: 'bold'},
      columnStyles: {0: {cellWidth: 25}, 1: {cellWidth: 70}, 2: {cellWidth: 15}, 3: {cellWidth: 30}, 4: {cellWidth: 30}, 5: {cellWidth: 30}},
      margin: {left: 15, right: 15}
    });
    
    const resumenY = doc.lastAutoTable.finalY + 12;
    doc.setDrawColor(150, 150, 150);
    doc.setLineWidth(0.5);
    doc.line(125, resumenY, 195, resumenY);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(11);
    
    let lineY = resumenY;
    const totalNetoSinDesc = formatNum(cot.productos.reduce((a, p) => a + parseFloat(p.totalNeto), 0));
    doc.text('NETO (SIN DESC):', 160, lineY + 8, {align: 'right'});
    doc.text(`$${Math.round(totalNetoSinDesc).toLocaleString('es-CL')}`, 195, lineY + 8, {align: 'right'});
    
    if (cot.descuentoGlobal && cot.descuentoGlobal.valor > 0) {
      const montoDesc = cot.descuentoGlobal.tipo === 'porcentaje' ? formatNum(totalNetoSinDesc * (cot.descuentoGlobal.valor / 100)) : cot.descuentoGlobal.valor;
      doc.text(`DESC (${cot.descuentoGlobal.tipo === 'porcentaje' ? cot.descuentoGlobal.valor + '%' : '$'})`, 160, lineY + 15, {align: 'right'});
      doc.text(`$${Math.round(montoDesc).toLocaleString('es-CL')}`, 195, lineY + 15, {align: 'right'});
      lineY += 7;
    }
    
    doc.text('NETO FINAL:', 160, lineY + 15, {align: 'right'});
    doc.text(`$${Math.round(netoFinal).toLocaleString('es-CL')}`, 195, lineY + 15, {align: 'right'});
    doc.text('IVA (19%):', 160, lineY + 22, {align: 'right'});
    doc.text(`$${Math.round(iva).toLocaleString('es-CL')}`, 195, lineY + 22, {align: 'right'});
    
    doc.setFont(undefined, 'bold');
    doc.setFontSize(13);
    doc.text('TOTAL (CON IVA):', 160, lineY + 31, {align: 'right'});
    doc.text(`$${Math.round(total).toLocaleString('es-CL')}`, 195, lineY + 31, {align: 'right'});
    
    doc.setFontSize(8);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(100, 100, 100);
    const pageHeight = doc.internal.pageSize.height;
    doc.text(`RUT: ${config.rut} | ${config.nombre} | ${config.direccion}`, 15, pageHeight - 18);
    doc.text(`Email: ${config.email} | Tel: ${config.telefono}`, 15, pageHeight - 14);
    
    const pdfData = doc.output('datauristring');
    const pdfFrame = document.createElement('div');
    pdfFrame.style.cssText = 'position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); z-index:10005; display:flex; flex-direction:column;';
    pdfFrame.innerHTML = `
      <div style="background:#1F6F8B; color:white; padding:15px; display:flex; justify-content:space-between; align-items:center;">
        <h3 style="margin:0;">📄 COTIZACIÓN EN PDF</h3>
        <button onclick="this.parentElement.parentElement.remove()" style="background:#9B2E00; color:white; border:none; padding:8px 12px; cursor:pointer; font-weight:700;">CERRAR</button>
      </div>
      <div style="flex:1; overflow:auto; background:#525252; display:flex; justify-content:center; padding:30px 20px;">
        <iframe src="${pdfData}" style="width:100%; max-width:850px; background:white; box-shadow:0 0 40px rgba(0,0,0,0.7); border:none;"></iframe>
      </div>
    `;
    document.body.appendChild(pdfFrame);
    doc.save(`Cotizacion-${cot.numero}.pdf`);
  } catch (error) {
    console.error('Error:', error);
    alert('Error al generar PDF');
  }
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones');
  const cont = document.getElementById('listaCotizaciones');
  
  if (cotizaciones.length === 0) {
    cont.innerHTML = '<p style="text-align:center; padding:15px;">SIN COTIZACIONES</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th>FECHA</th><th>ESTADO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    cotizaciones.forEach((c, index) => {
      const claseEstado = c.estado === 'aceptado' ? 'cot-aceptado' : (c.estado === 'rechazado' ? 'cot-rechazado' : (c.estado === 'enviada' ? 'cot-enviada' : 'cot-borrador'));
      const fechaEmision = new Date(c.fecha);
      const fechaFormat = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
      const badgeClase = c.estado === 'aceptado' ? 'badge-aceptado' : (c.estado === 'rechazado' ? 'badge-rechazado' : (c.estado === 'enviada' ? 'badge-enviada' : 'badge-borrador'));
      html += `<tr class="${claseEstado}"><td>${c.numero}</td><td>${c.razonSocial}</td><td style="text-align:right;">$${formatNum(parseFloat(c.totalNeto)).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${fechaFormat}</td><td><span class="badge-estado ${badgeClase}">${c.estado.toUpperCase()}</span></td><td style="text-align:center;"><button class="btn btn-buscar" style="padding:4px 8px; font-size:9px;" onclick="verCotizacion(${index})">VER</button><button class="btn btn-editar" style="padding:4px 8px; font-size:9px;" onclick="editarCot(${index})">EDITAR</button></td></tr>`;
    });
    html += '</tbody></table>';
    cont.innerHTML = html;
  }
  modal.style.display = 'block';
}

function verCotizacion(index) {
  if (index < 0 || index >= cotizaciones.length) return;
  generarPDFDoc(cotizaciones[index]);
}

function editarCot(index) {
  if (index < 0 || index >= cotizaciones.length) return;
  const cot = cotizaciones[index];
  if (cot.estado === 'rechazado') {
    mostrarMensaje('NO SE PUEDE EDITAR RECHAZADA', 'error');
    return;
  }
  
  cliente = JSON.parse(JSON.stringify(cot.cliente));
  productos = JSON.parse(JSON.stringify(cot.productos));
  descuento = cot.descuentoGlobal || {tipo: 'porcentaje', valor: 0};
  fechaValidez = cot.fechaValidez;
  tiempoEntrega = cot.tiempoEntrega;
  cotIndex = index;
  pdfGen = true;
  estado = cot.estado;
  
  if (cot.estado === 'aceptado') {
    esLectura = true;
    esEdicion = false;
  } else {
    esLectura = false;
    esEdicion = true;
  }
  
  gestorCot.establecer(cot.numero);
  document.getElementById('seccionCierre').classList.add('activo');
  document.getElementById('tipoDescuento').value = descuento.tipo;
  document.getElementById('valorDescuento').value = descuento.valor;
  document.getElementById('fechaValidez').value = fechaValidez;
  document.getElementById('tiempoEntrega').value = tiempoEntrega;
  document.getElementById('inputRut').value = cliente.rut;
  document.getElementById('inputRut').disabled = true;
  mostrarResumenCliente(cliente);
  actualizarTablaProductos();
  mostrarEstado();
  habilitarProductos();
  
  if (cot.estado === 'aceptado') {
    document.getElementById('seccionBloqueada').classList.add('activa');
  }
  
  cerrarCotizaciones();
  mostrarMensaje(`COTIZACIÓN ${cot.numero} CARGADA`, 'info');
}

function cerrarCotizaciones() {
  document.getElementById('modalCotizaciones').style.display = 'none';
}

function abrirModalClientes() {
  const modal = document.getElementById('modalClientes');
  const todos = gestorCli.obtenerTodos();
  const clientes = Object.entries(todos);
  let html = '';
  
  if (clientes.length === 0) {
    html = '<p style="text-align:center; padding:15px;">SIN CLIENTES</p>';
  } else {
    html = '<table style="width:100%; border-collapse:collapse;"><thead><tr style="background:#1F6F8B; color:white;"><th style="padding:8px; text-align:left;">RUT</th><th style="padding:8px; text-align:left;">RAZÓN SOCIAL</th><th style="padding:8px; text-align:left;">GIRO</th><th style="padding:8px; text-align:left;">CONTACTO</th><th style="padding:8px; text-align:left;">CELULAR</th><th style="padding:8px; text-align:left;">ACCIÓN</th></tr></thead><tbody>';
    clientes.forEach(([rut, cli]) => {
      html += `<tr style="border:1px solid #ddd;"><td style="padding:8px;">${rut}</td><td style="padding:8px;">${cli.razonSocial}</td><td style="padding:8px;">${cli.giro}</td><td style="padding:8px;">${cli.nombreContacto}</td><td style="padding:8px;">${cli.celular}</td><td style="padding:8px;"><button class="btn btn-buscar" style="padding:4px 8px; font-size:9px;" onclick="seleccionarCliente('${rut}')">SELECCIONAR</button></td></tr>`;
    });
    html += '</tbody></table>';
  }
  
  document.getElementById('listaClientesModal').innerHTML = html;
  modal.style.display = 'block';
}

function seleccionarCliente(rut) {
  document.getElementById('inputRut').value = rut;
  cerrarModalClientes();
  buscarCliente();
}

function cerrarModalClientes() {
  document.getElementById('modalClientes').style.display = 'none';
}

function mostrarMensaje(texto, tipo) {
  const m = document.getElementById('mensaje');
  m.className = `mensaje mensaje-${tipo}`;
  m.textContent = texto;
}

window.addEventListener('click', function(e) {
  if (e.target.id === 'modalConfig' && e.target === e.currentTarget) cerrarConfiguracion();
  if (e.target.id === 'modalArticulos' && e.target === e.currentTarget) cerrarArticulos();
  if (e.target.id === 'modalCotizaciones' && e.target === e.currentTarget) cerrarCotizaciones();
  if (e.target.id === 'modalReportes' && e.target === e.currentTarget) cerrarReportes();
  if (e.target.id === 'modalBackup' && e.target === e.currentTarget) cerrarBackup();
  if (e.target.id === 'modalClientes' && e.target === e.currentTarget) cerrarModalClientes();
});
</script>
</body>
</html>
