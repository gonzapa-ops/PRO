<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Cotizador - EMPRESA CUNDO</title>
<style>
* {margin: 0; padding: 0; box-sizing: border-box;}
body {font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #F5F5F5; padding: 20px; color: #3B3B3B;}
.cotizador-container {max-width: 1200px; margin: auto; background: #fff; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 30px; border-radius: 8px;}
.cotizador-header {background: #1F6F8B; color: white; padding: 8px 30px; display: flex; justify-content: space-between; align-items: center; border-bottom: 3px solid #F25C05; border-radius: 6px; margin-bottom: 20px;}
.empresa-nombre {font-weight: 700; font-size: 16px; letter-spacing: 1px;}
.numero-cotizacion {font-weight: 900; color: white; font-size: 15px;}
.seccion-titulo {font-weight: 700; font-size: 18px; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 2px solid #F25C05; text-transform: uppercase; color: #1F6F8B;}
table {width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 13px;}
th, td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B;}
th {background: #1F6F8B; color: white; font-weight: 700;}
tr:nth-child(even) {background: #E9F0EA;}
.valor-numerico {text-align: right; font-weight: 700; color: #3B3B3B;}
.texto-centrado {text-align: center;}
button {cursor: pointer; border: none; border-radius: 5px; font-weight: 700; text-transform: uppercase; transition: all 0.3s ease;}
.btn-buscar {background: #F25C05; color: white; padding: 8px 14px; font-size: 12px;}
.btn-buscar:hover {background: #cb4a04;}
.btn-buscar:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-limpiar {background: #7A8C52; color: white; padding: 8px 14px; font-size: 12px;}
.btn-limpiar:hover {background: #5a6b37;}
.btn-limpiar:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-eliminar {background: #9B2E00; color: white; font-size: 11px; padding: 5px 10px;}
.btn-eliminar:hover {background: #7a2300;}
.btn-articulos {background: #4B732E; color: white; padding: 8px 14px; font-size: 12px;}
.btn-articulos:hover {background: #385525;}
.btn-articulos:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-clientes {background: #1F6F8B; color: white; padding: 8px 14px; font-size: 12px;}
.btn-clientes:hover {background: #174d63;}
.btn-clientes:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-pdf {background: #F25C05; color: white; padding: 10px 20px; font-size: 13px;}
.btn-pdf:hover {background: #cb4a04;}
.btn-pdf:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-agregar {background: #385525; color: white; padding: 8px 14px; font-size: 12px;}
.btn-agregar:hover {background: #274015;}
.btn-editar {background: #D9822B; color: white; padding: 8px 14px; font-size: 12px;}
.btn-editar:hover {background: #b36e1e;}
.btn-editar:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-cancelar {background: #9B2E00; color: white; padding: 8px 14px; font-size: 12px;}
.btn-cancelar:hover {background: #7a2300;}
.btn-ver {background: #556B2F; color: white; padding: 5px 8px; font-size: 10px; margin-right: 3px;}
.btn-ver:hover {background: #3f4f20;}
.btn-editar-cot {background: #D9822B; color: white; padding: 5px 8px; font-size: 10px; margin-right: 3px;}
.btn-editar-cot:hover {background: #b36e1e;}
.btn-ver-archivo {background: #4B732E; color: white; padding: 5px 8px; font-size: 10px; margin-right: 3px;}
.btn-ver-archivo:hover {background: #385525;}
.btn-toggle-utilidad {background: #556B2F; color: white; padding: 8px 16px; font-size: 12px; margin-top: 10px;}
.btn-toggle-utilidad:hover {background: #3f4f20;}
.formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none;}
.formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
.campo-grupo {margin-bottom: 15px;}
.campo-grupo label {display: block; font-weight: 700; color: #3B3B3B; margin-bottom: 5px; font-size: 13px; text-transform: uppercase;}
.campo-grupo input, .campo-grupo select, .campo-grupo textarea {width: 100%; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 5px; text-transform: uppercase; font-family: inherit;}
.campo-grupo input:focus, .campo-grupo select:focus, .campo-grupo textarea:focus {outline: none; border-color: #F25C05;}
.campo-grupo input:disabled, .campo-grupo select:disabled, .campo-grupo textarea:disabled {background-color: #F5F5F5; cursor: not-allowed; color: #777;}
.fila-campos {display: grid; grid-template-columns: 1fr 1fr; gap: 15px;}
.fila-campos-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px;}
.mensaje {padding: 12px 15px; border-radius: 5px; margin-bottom: 15px; font-size: 12px; text-transform: uppercase; font-weight: 700;}
.mensaje-exito {background-color: #d0e8d0; color: #385525; border: 1px solid #8bb76f;}
.mensaje-error {background-color: #fbd7d2; color: #9b2e00; border: 1px solid #e4827b;}
.mensaje-info {background-color: #e2f0ec; color: #4b732e; border: 1px solid #a7c3a1;}
.mensaje-bloqueado {background-color: #fff3cd; color: #856404; border: 1px solid #ffeaa7;}
.resumen-cliente {display: none; background-color: #e8f0e4; padding: 15px; border-radius: 5px; border-left: 5px solid #4B732E;}
.resumen-cliente.activo {display: block;}
.resumen-cliente h4 {color: #3B3B3B; margin-bottom: 10px; text-transform: uppercase; font-weight: 700;}
.resumen-cliente p {margin: 5px 0; font-size: 13px; color: #4B732E; text-transform: uppercase;}
.botones-resumen {margin-top: 15px; display: flex; gap: 10px;}
.botones-formulario {display: flex; gap: 10px; margin-top: 10px;}
.seccion-cliente {margin-bottom: 30px; border: 1px solid #ddd; border-radius: 6px; padding: 20px; background-color: #fafafa;}
.busqueda-rut {display: flex; gap: 10px; margin-bottom: 20px; align-items: center;}
.busqueda-rut input {flex: 1; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 6px; text-transform: uppercase;}
.seccion-productos {margin-bottom: 30px; border: 1px solid #ddd; border-radius: 6px; padding: 20px; background-color: #fafafa;}
.busqueda-producto {display: flex; gap: 10px; margin-bottom: 20px; align-items: center; position: relative;}
.busqueda-producto input {flex: 1; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 6px; text-transform: uppercase;}
.autocomplete-list {display: none; position: absolute; top: 100%; left: 0; right: 70px; background: white; border: 1px solid #ddd; border-top: none; max-height: 200px; overflow-y: auto; z-index: 1000; border-radius: 0 0 6px 6px; box-shadow: 0 4px 8px rgba(0,0,0,0.07);}
.autocomplete-list.activo {display: block;}
.autocomplete-item {padding: 10px; cursor: pointer; background: white; font-size: 12px; text-transform: uppercase; border-bottom: 1px solid #eee; transition: background 0.2s;}
.autocomplete-item:hover {background: #f9e2cd; border-left: 3px solid #F25C05;}
.formulario-producto {background: linear-gradient(135deg, #f9ead4 0%, #f7dba1 100%); padding: 15px; border-radius: 6px; border-left: 5px solid #F25C05; margin-bottom: 20px;}
.formulario-producto h4 {text-transform: uppercase; color: #b35304; margin-bottom: 15px; font-weight: 700;}
.fila-campos-producto {display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;}
.fila-campos-producto-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 15px;}
.fila-campos-producto .campo-grupo, .fila-campos-producto-tres .campo-grupo {margin-bottom: 0;}
.botones-producto {display: flex; gap: 10px; margin-top: 10px;}
.alerta-sin-productos {background-color: #e9f6ec; padding: 15px; border-radius: 6px; border-left: 5px solid #4B732E; text-align: center; color: #4B732E; text-transform: uppercase; font-weight: 700;}
#modalArticulos {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 9999; overflow: auto;}
.articulos-content {background: white; border-radius: 8px; margin: 40px auto; padding: 25px; max-width: 1000px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15);}
.modal-titulo {font-size: 22px; font-weight: 700; margin-bottom: 20px; text-transform: uppercase; color: #3B3B3B;}
.btn-cerrar-modal {position: absolute; top: 14px; right: 25px; background: #9B2E00; color: white; font-size: 18px; border: none; border-radius: 6px; cursor: pointer; padding: 4px 10px; font-weight: 700;}
.botones-superiores {display: flex; gap: 8px; margin-bottom: 20px; flex-wrap: wrap; align-items: center;}
#modalCotizaciones {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10000; overflow: auto;}
#modalCotizaciones > div {background: white; max-width: 1100px; margin: 40px auto; padding: 20px; border-radius: 8px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); overflow-x: auto;}
#modalClientes {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 9998; overflow: auto;}
#modalClientes > div {background: white; max-width: 1100px; margin: 40px auto; padding: 20px; border-radius: 8px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); overflow-x: auto;}
.resumen-totales {max-width: 400px; margin-left: auto; border-top: 3px solid #F25C05; padding-top: 15px; text-transform: uppercase; margin-bottom: 20px;}
.resumen-linea {display: flex; justify-content: space-between; margin: 5px 0; font-weight: 700; font-size: 13px; color: #3B3B3B;}
.resumen-linea.total {font-size: 18px; color: #000; font-weight: 900;}
.resumen-utilidad {background-color: #E8E8E8; padding: 20px; border-radius: 8px; border: 2px solid #999999; border-top: 4px solid #1F6F8B; border-bottom: 4px solid #1F6F8B; margin-top: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.08); display: none;}
.resumen-utilidad.visible {display: block;}
.resumen-utilidad h4 {color: #1F6F8B; text-transform: uppercase; margin-bottom: 12px; font-size: 14px; font-weight: 700;}
.utilidad-item {display: flex; justify-content: space-between; align-items: center; margin: 10px 0; font-size: 13px; color: #3B3B3B; flex-wrap: wrap; gap: 10px;}
.utilidad-item strong {color: #1F6F8B; font-weight: 700;}
.utilidad-porcentaje {color: #F25C05; font-weight: 700; font-size: 12px; background-color: #fff; padding: 2px 8px; border-radius: 3px; border: 1px solid #F25C05;}
.utilidad-costo {color: #555; font-weight: 600; font-size: 12px; background-color: #fff; padding: 2px 8px; border-radius: 3px; border: 1px solid #999;}
.tabla-cotizaciones {width: 100%; border-collapse: collapse; background: white; min-width: 900px;}
.tabla-cotizaciones th {background: #1F6F8B; color: white; padding: 12px; text-align: left; text-transform: uppercase; font-weight: 700;}
.tabla-cotizaciones td {border: 1px solid #ddd; padding: 12px; text-transform: uppercase; color: #3B3B3B;}
.tabla-cotizaciones tr.cot-aceptado {background-color: #C8E6C9 !important;}
.tabla-cotizaciones tr.cot-aceptado td {background-color: #C8E6C9 !important; color: #2E7D32;}
.tabla-cotizaciones tr.cot-rechazado {background-color: #FFCDD2 !important;}
.tabla-cotizaciones tr.cot-rechazado td {background-color: #FFCDD2 !important; color: #C62828;}
.tabla-cotizaciones tr.cot-pendiente {background-color: #FFF9C4 !important;}
.tabla-cotizaciones tr.cot-pendiente td {background-color: #FFF9C4 !important; color: #F57F17;}
.tabla-clientes {width: 100%; border-collapse: collapse; background: white; min-width: 900px;}
.tabla-clientes th {background: #1F6F8B; color: white; padding: 12px; text-align: left; text-transform: uppercase; font-weight: 700;}
.tabla-clientes td {border: 1px solid #ddd; padding: 12px; text-transform: uppercase; color: #3B3B3B;}
.tabla-clientes tr:nth-child(even) {background: #E9F0EA;}
.seccion-cierre {margin-top: 30px; border: 1px solid #ddd; border-radius: 6px; padding: 20px; background-color: #fafafa; display: none;}
.seccion-cierre.activo {display: block;}
.botones-cierre {display: flex; gap: 15px; margin-top: 15px; flex-wrap: wrap;}
.btn-aceptado {background: #4B732E; color: white; padding: 12px 30px; font-size: 14px;}
.btn-aceptado:hover {background: #385525;}
.btn-aceptado:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-rechazado {background: #9B2E00; color: white; padding: 12px 30px; font-size: 14px;}
.btn-rechazado:hover {background: #7a2300;}
.btn-rechazado:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-limpiar-cot {background: #556B2F; color: white; padding: 12px 30px; font-size: 14px;}
.btn-limpiar-cot:hover {background: #3f4f20;}
.btn-archivo {background: #D9822B; color: white; padding: 12px 30px; font-size: 14px; display: none;}
.btn-archivo:hover {background: #b36e1e;}
#modalAceptado {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10001; overflow: auto;}
.modal-aceptado-content {background: white; max-width: 700px; margin: 40px auto; padding: 25px; border-radius: 8px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); max-height: 90vh; overflow-y: auto;}
.modal-aceptado-titulo {font-size: 22px; font-weight: 700; margin-bottom: 20px; text-transform: uppercase; color: #3B3B3B; border-bottom: 3px solid #4B732E; padding-bottom: 10px;}
.btn-cerrar-aceptado {position: absolute; top: 15px; right: 20px; background: #9B2E00; color: white; font-size: 18px; border: none; border-radius: 6px; cursor: pointer; padding: 4px 10px; font-weight: 700;}
.botones-archivo {display: flex; gap: 10px; align-items: center; margin-bottom: 15px;}
.btn-adjuntar {background: #F25C05; color: white; padding: 10px 20px; font-size: 12px;}
.btn-adjuntar:hover {background: #cb4a04;}
#inputArchivo {display: none;}
.archivo-info {font-size: 12px; color: #4B732E; font-weight: 600; text-transform: uppercase;}
.seccion-adjuntos {margin-top: 15px; padding: 15px; background-color: #e9f6ec; border-radius: 6px; border-left: 5px solid #4B732E;}
.seccion-adjuntos h4 {color: #4B732E; margin-bottom: 10px; font-weight: 700; text-transform: uppercase;}
.lista-adjuntos {list-style: none;}
.item-adjunto {display: flex; justify-content: space-between; align-items: center; padding: 8px; background: white; margin: 5px 0; border-radius: 4px; border-left: 3px solid #4B732E;}
.nombre-archivo {font-size: 12px; color: #3B3B3B; font-weight: 600; text-transform: uppercase; word-break: break-all;}
.btn-eliminar-archivo {background: #9B2E00; color: white; padding: 4px 8px; font-size: 10px;}
.btn-eliminar-archivo:hover {background: #7a2300;}
.botones-modal-aceptado {display: flex; gap: 10px; margin-top: 20px; justify-content: flex-end;}
.btn-confirmar {background: #4B732E; color: white; padding: 10px 20px; font-size: 12px;}
.btn-confirmar:hover {background: #385525;}
.resumen-despacho {display: none; background-color: #e8f0e4; padding: 15px; border-radius: 5px; border-left: 5px solid #4B732E; margin-top: 20px;}
.resumen-despacho.activo {display: block;}
.resumen-despacho h4 {color: #3B3B3B; margin-bottom: 10px; text-transform: uppercase; font-weight: 700;}
.resumen-despacho p {margin: 5px 0; font-size: 13px; color: #4B732E; text-transform: uppercase;}
#modalArchivos {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10002; overflow: auto;}
.modal-archivos-content {background: white; max-width: 600px; margin: 40px auto; padding: 25px; border-radius: 8px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15);}
.modal-archivos-titulo {font-size: 22px; font-weight: 700; margin-bottom: 20px; text-transform: uppercase; color: #3B3B3B; border-bottom: 3px solid #4B732E; padding-bottom: 10px;}
.btn-cerrar-archivos {position: absolute; top: 15px; right: 20px; background: #9B2E00; color: white; font-size: 18px; border: none; border-radius: 6px; cursor: pointer; padding: 4px 10px; font-weight: 700;}
.resumen-compra {display: none; background-color: #e3f2fd; padding: 15px; border-radius: 6px; border-left: 5px solid #1976d2; margin-top: 20px; overflow-x: auto;}
.resumen-compra.activo {display: block;}
.resumen-compra h4 {color: #1976d2; margin-bottom: 12px; text-transform: uppercase; font-weight: 700; font-size: 14px;}
.tabla-compra {width: 100%; border-collapse: collapse; font-size: 11px; background: white; min-width: 700px;}
.tabla-compra th {background: #1976d2; color: white; padding: 8px; text-transform: uppercase; font-weight: 700; text-align: left;}
.tabla-compra td {border: 1px solid #ccc; padding: 8px; text-transform: uppercase; color: #333;}
.tabla-compra tr:nth-child(even) {background: #f0f8ff;}
.tabla-compra .valor-numerico {text-align: right; font-weight: 700;}
.tabla-compra a {color: #1976d2; text-decoration: none; font-weight: 600;}
.tabla-compra a:hover {text-decoration: underline;}
#modalVisualizarArchivo {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.5); z-index: 10003; overflow: auto;}
.modal-archivo-content {background: white; max-width: 90%; max-height: 90vh; margin: 40px auto; border-radius: 8px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); overflow: auto;}
.btn-cerrar-archivo {position: absolute; top: 15px; right: 20px; background: #9B2E00; color: white; font-size: 18px; border: none; border-radius: 6px; cursor: pointer; padding: 4px 10px; font-weight: 700; z-index: 1;}
.contenido-archivo {padding: 20px; text-align: center;}
.contenido-archivo img {max-width: 100%; max-height: 70vh; object-fit: contain;}
.contenido-archivo iframe {width: 100%; height: 70vh; border: none;}
.contenido-archivo pre {background: #f5f5f5; padding: 15px; border-radius: 5px; overflow-x: auto; text-align: left; font-size: 12px;}
.seccion-botones-pdf {margin-bottom: 20px; text-align: center;}
.badge-estado {display: inline-block; padding: 4px 8px; border-radius: 3px; font-size: 11px; font-weight: 700; text-transform: uppercase;}
.badge-aceptado {background: #4B732E; color: white;}
.badge-rechazado {background: #9B2E00; color: white;}
.badge-pendiente {background: #F25C05; color: white;}
#modalPDF {display: none !important; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.8); z-index: 10004; overflow: auto; padding: 10px;}
#modalPDF.mostrar {display: block !important;}
.modal-pdf-wrapper {width: 100%; height: 100%; display: flex; flex-direction: column;}
.pdf-header {background: #1F6F8B; color: white; padding: 12px 20px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 2px 8px rgba(0,0,0,0.3);}
.pdf-header h3 {margin: 0; font-size: 16px; font-weight: 700;}
.btn-cerrar-pdf {background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 4px; cursor: pointer; padding: 8px 15px; font-weight: 700;}
.btn-cerrar-pdf:hover {background: #7a2300;}
.pdf-viewer {flex: 1; width: 100%; background: white; overflow: auto; display: flex; justify-content: center; align-items: flex-start; padding: 20px;}
.pdf-viewer iframe {width: 100%; height: 100%; border: none; min-height: 100vh;}
.seccion-bloqueada {background-color: #fff3cd; padding: 15px; border-radius: 6px; border-left: 5px solid #FFA500; margin-bottom: 20px; display: none;}
.seccion-bloqueada.activa {display: block;}
.seccion-bloqueada h4 {color: #856404; text-transform: uppercase; font-weight: 700; margin-bottom: 5px;}
.seccion-bloqueada p {color: #856404; font-size: 12px; text-transform: uppercase;}
</style>
</head>
<body>
  <div class="cotizador-container">
    <header class="cotizador-header">
      <div class="empresa-nombre">EMPRESA CUNDO</div>
      <div class="numero-cotizacion">COTIZACIÓN N° <span id="numeroCotizacion" style="color: white;">CO100500</span></div>
    </header>

    <div class="botones-superiores">
      <button class="btn btn-articulos" onclick="abrirArticulos()" id="btnArticulos">ARTÍCULOS</button>
      <button class="btn btn-buscar" onclick="mostrarCotizaciones()" id="btnCotizaciones">COTIZACIONES</button>
      <button class="btn btn-clientes" onclick="abrirClientes()" id="btnClientes">CLIENTES</button>
    </div>

    <div id="seccionBloqueada" class="seccion-bloqueada">
      <h4>🔒 COTIZACIÓN EN MODO LECTURA</h4>
      <p>Puede revisar productos, costos y proveedores. No se pueden hacer modificaciones.</p>
    </div>

    <section class="seccion-cliente">
      <h2 class="seccion-titulo">DATOS DEL CLIENTE</h2>
      <div class="busqueda-rut">
        <input type="text" id="inputRut" placeholder="INGRESE RUT (EJ: 78070615-7)" maxlength="12" />
        <button class="btn btn-buscar" onclick="buscarCliente()" id="btnBuscarCliente">BUSCAR</button>
        <button class="btn btn-limpiar" onclick="limpiarCotizacion()" id="btnLimpiarCliente">LIMPIAR</button>
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
        <div class="campo-grupo"><label>MEDIO DE PAGO *</label><select id="medioPago"><option value="">SELECCIONE MEDIO DE PAGO</option><option value="TRANSFERENCIA">TRANSFERENCIA</option><option value="WEBPAY">WEBPAY</option><option value="CHEQUE 30 DÍAS">CHEQUE 30 DÍAS</option><option value="OC 30 DÍAS">OC 30 DÍAS</option><option value="EFECTIVO">EFECTIVO</option></select></div>
        <div class="botones-formulario">
          <button class="btn btn-buscar" onclick="guardarCliente()" id="btnGuardarCliente">GUARDAR CLIENTE</button>
          <button class="btn btn-cancelar" id="btnCancelarEdicion" onclick="cancelarEdicion()" style="display: none;">CANCELAR</button>
        </div>
      </div>
    </section>

    <section class="seccion-productos">
      <h2 class="seccion-titulo">PRODUCTOS Y/O SERVICIOS</h2>
      <div class="busqueda-producto">
        <input type="text" id="inputCodigoProducto" placeholder="INGRESE CÓDIGO O DESCRIPCIÓN DEL PRODUCTO" maxlength="50" />
        <div id="autocompleteLista" class="autocomplete-list"></div>
        <button class="btn btn-buscar" onclick="buscarProducto()" id="btnBuscarProducto">BUSCAR</button>
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
          <div class="campo-grupo"><label>LINK *</label><input type="text" id="linkProducto" /></div>
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

      <button class="btn-toggle-utilidad" id="btnToggleUtilidad" onclick="toggleUtilidad()">📊 VER UTILIDAD</button>
      <div id="utilidadResumen" class="resumen-utilidad"></div>
      
      <div class="seccion-botones-pdf">
        <button class="btn btn-pdf" onclick="generarPDF()" id="btnPDF">GENERAR PDF</button>
      </div>
    </section>

    <section class="seccion-cierre" id="seccionCierre">
      <h2 class="seccion-titulo">DATOS DE CIERRE</h2>
      
      <div id="resumenDespacho" class="resumen-despacho"></div>
      <div id="resumenCompra" class="resumen-compra"></div>
      <div class="botones-cierre">
        <button class="btn-aceptado" onclick="marcarAceptado()" id="btnAceptado">ACEPTADO</button>
        <button class="btn-rechazado" onclick="marcarRechazado()" id="btnRechazado">RECHAZADO</button>
        <button class="btn-limpiar-cot" onclick="limpiarCotizacion()" id="btnLimpiarCotizacion">LIMPIAR</button>
        <button class="btn-archivo" onclick="verArchivos()" id="btnVerArchivos">ARCHIVOS</button>
      </div>
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

  <div id="modalClientes">
    <div>
      <button onclick="cerrarClientes()" style="position:absolute;top:15px;right:20px;background:#9B2E00;color:white;border:none;border-radius:4px;padding:5px 10px;cursor:pointer;font-size:18px;font-weight:bold;">×</button>
      <h2 style="margin-bottom:15px;text-transform:uppercase;color:#3B3B3B;">CLIENTES REGISTRADOS</h2>
      <div id="listaClientes"></div>
    </div>
  </div>

  <div id="modalCotizaciones">
    <div>
      <button onclick="cerrarCotizaciones()" style="position:absolute;top:15px;right:20px;background:#9B2E00;color:white;border:none;border-radius:4px;padding:5px 10px;cursor:pointer;font-size:18px;font-weight:bold;">×</button>
      <h2 style="margin-bottom:15px;text-transform:uppercase;color:#3B3B3B;">COTIZACIONES EMITIDAS</h2>
      <div id="listaCotizaciones"></div>
    </div>
  </div>

  <div id="modalAceptado">
    <div class="modal-aceptado-content">
      <button class="btn-cerrar-aceptado" onclick="cerrarModalAceptado()">×</button>
      <h2 class="modal-aceptado-titulo">ACEPTACIÓN DE COTIZACIÓN</h2>
      
      <div class="campo-grupo">
        <label>TIPO DE ENTREGA *</label>
        <select id="tipoEntrega">
          <option value="">SELECCIONE TIPO DE ENTREGA</option>
          <option value="RETIRO">RETIRO</option>
          <option value="DESPACHO RM">DESPACHO RM</option>
          <option value="STARKEN">STARKEN</option>
          <option value="CHILEXPRESS">CHILEXPRESS</option>
          <option value="PDQ">PDQ</option>
          <option value="BLUEXPRESS">BLUEXPRESS</option>
          <option value="OTRO">OTRO</option>
        </select>
      </div>

      <div class="fila-campos">
        <div class="campo-grupo">
          <label>REGIÓN *</label>
          <select id="regionSelect">
            <option value="">SELECCIONE REGIÓN</option>
            <option value="XV REGIÓN (ARICA Y PARINACOTA)">XV REGIÓN (ARICA Y PARINACOTA)</option>
            <option value="I REGIÓN (TARAPACÁ)">I REGIÓN (TARAPACÁ)</option>
            <option value="II REGIÓN (ANTOFAGASTA)">II REGIÓN (ANTOFAGASTA)</option>
            <option value="III REGIÓN (ATACAMA)">III REGIÓN (ATACAMA)</option>
            <option value="IV REGIÓN (COQUIMBO)">IV REGIÓN (COQUIMBO)</option>
            <option value="V REGIÓN (VALPARAÍSO)">V REGIÓN (VALPARAÍSO)</option>
            <option value="VI REGIÓN (LIB. B. O'HIGGINS)">VI REGIÓN (LIB. B. O'HIGGINS)</option>
            <option value="VII REGIÓN (MAULE)">VII REGIÓN (MAULE)</option>
            <option value="VIII REGIÓN (BÍO-BÍO)">VIII REGIÓN (BÍO-BÍO)</option>
            <option value="IX REGIÓN (LA ARAUCANÍA)">IX REGIÓN (LA ARAUCANÍA)</option>
            <option value="X REGIÓN (LOS LAGOS)">X REGIÓN (LOS LAGOS)</option>
            <option value="XI REGIÓN (AYSÉN)">XI REGIÓN (AYSÉN)</option>
            <option value="XII REGIÓN (MAGALLANES)">XII REGIÓN (MAGALLANES)</option>
            <option value="RM (METROPOLITANA)">RM (METROPOLITANA)</option>
          </select>
        </div>
        <div class="campo-grupo">
          <label>COMUNA *</label>
          <input type="text" id="comunaInput" placeholder="INGRESE COMUNA" />
        </div>
      </div>

      <div class="campo-grupo">
        <label>DIRECCIÓN *</label>
        <input type="text" id="direccionDespacho" placeholder="INGRESE DIRECCIÓN" />
      </div>

      <div class="fila-campos">
        <div class="campo-grupo">
          <label>CONTACTO DE DESPACHO *</label>
          <input type="text" id="contactoDespacho" placeholder="NOMBRE DE CONTACTO" />
        </div>
        <div class="campo-grupo">
          <label>CELULAR CONTACTO DESPACHO *</label>
          <input type="tel" id="celularDespacho" placeholder="+56912345678" />
        </div>
      </div>

      <div class="campo-grupo">
        <label>ADJUNTO</label>
        <div class="botones-archivo">
          <button class="btn-adjuntar" onclick="abrirSelectorArchivos()">📎 SELECCIONAR ARCHIVO</button>
          <input type="file" id="inputArchivo" onchange="agregarArchivo(event)" />
          <span class="archivo-info" id="infoArchivo"></span>
        </div>
        <div id="adjuntosContainer" class="seccion-adjuntos" style="display: none;">
          <h4>ARCHIVOS ADJUNTOS</h4>
          <ul class="lista-adjuntos" id="listaAdjuntos"></ul>
        </div>
      </div>

      <div class="botones-modal-aceptado">
        <button class="btn btn-cancelar" onclick="cerrarModalAceptado()">CANCELAR</button>
        <button class="btn-confirmar" onclick="confirmarAceptacion()">GUARDAR</button>
      </div>
    </div>
  </div>

  <div id="modalArchivos">
    <div class="modal-archivos-content">
      <button class="btn-cerrar-archivos" onclick="cerrarModalArchivos()">×</button>
      <h2 class="modal-archivos-titulo">ARCHIVOS ADJUNTOS</h2>
      <div id="listaArchivosModal"></div>
    </div>
  </div>

  <div id="modalVisualizarArchivo">
    <div class="modal-archivo-content">
      <button class="btn-cerrar-archivo" onclick="cerrarModalVisualizarArchivo()">×</button>
      <div class="contenido-archivo" id="contenidoArchivo"></div>
    </div>
  </div>

  <div id="modalPDF">
    <div class="modal-pdf-wrapper">
      <div class="pdf-header">
        <h3>COTIZACIÓN - PRESUPUESTO</h3>
        <button class="btn-cerrar-pdf" onclick="cerrarModalPDF()">✕ CERRAR</button>
      </div>
      <div class="pdf-viewer">
        <iframe id="pdfIframe" src="" title="Visualizador PDF"></iframe>
      </div>
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
  establecerNumero(numero) {const numPuro = parseInt(numero.replace(this.prefijo, '')); this.numeroBase = numPuro; this.actualizarDisplay();}
}

class GestorClientes {
  constructor() {this.clientes = this.cargarClientes();}
  cargarClientes() {const c = localStorage.getItem('clientes'); return c ? JSON.parse(c) : {};}
  guardarClientes() {localStorage.setItem('clientes', JSON.stringify(this.clientes));}
  buscarPorRut(rut) {return this.clientes[rut] || null;}
  agregarCliente(rut, datos) {this.clientes[rut] = datos; this.guardarClientes();}
  actualizarCliente(rut, datos) {this.clientes[rut] = datos; this.guardarClientes();}
  obtenerTodos() {return this.clientes;}
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
    return Object.values(todos).filter(prod => prod.codigo.includes(termino) || prod.descripcion.includes(termino)).slice(0, 10);
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
let archivosAdjuntos = [];
let cotizacionGuardada = false;
let datosDespacho = null;
let cotizacionActualIndex = null;
let pdfEmitido = false;
let esEdicionCotizacion = false;
let esLecturaCotizacion = false;
let pdfActualDoc = null;
let numeroCotizacionActual = null;
let estadoCotizacionActual = 'pendiente';
let utilidadVisible = false;

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
    html += `<div class="autocomplete-item" onclick="seleccionarProductoAutocomplete('${prod.codigo}')"><strong>${prod.codigo}</strong> - ${prod.descripcion}</div>`;
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
  if (esLecturaCotizacion) {
    mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado');
    return;
  }
  if (cotizacionGuardada && (estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado')) {
    mostrarMensaje('COTIZACIÓN BLOQUEADA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado');
    return;
  }
  const rut = document.getElementById('inputRut').value.trim();
  if (!rut) return mostrarMensaje('POR FAVOR INGRESE UN RUT', 'error');
  if (!validarRut(rut)) return mostrarMensaje('EL FORMATO DEL RUT NO ES VÁLIDO', 'error');
  const cliente = gestorClientes.buscarPorRut(rut);
  if (cliente) {
    clienteActual = cliente;
    mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
    mostrarResumenCliente(cliente);
    document.getElementById('formularioCliente').classList.remove('activo');
    habilitarProductos();
  } else {
    mostrarMensaje('CLIENTE NO ENCONTRADO. COMPLETE LOS DATOS PARA CREAR.', 'info');
    mostrarFormularioNuevo(rut);
  }
}

function mostrarFormularioNuevo(rut) {
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
  document.getElementById('btnGuardarCliente').style.display = 'inline-block';
  document.getElementById('btnCancelarEdicion').style.display = 'none';
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
  const btnEditarDisabled = (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') ? 'disabled' : '';
  r.className = 'resumen-cliente activo';
  r.innerHTML = `<h4>✓ CLIENTE REGISTRADO</h4><p><strong>RUT:</strong> ${cliente.rut}</p><p><strong>RAZÓN SOCIAL:</strong> ${cliente.razonSocial}</p><p><strong>GIRO:</strong> ${cliente.giro}</p><p><strong>DIRECCIÓN:</strong> ${cliente.direccion}</p><p><strong>CONTACTO:</strong> ${cliente.nombreContacto}</p><p><strong>CELULAR:</strong> ${cliente.celular}</p><p><strong>MAIL:</strong> ${cliente.mail}</p><p><strong>MEDIO DE PAGO:</strong> ${cliente.medioPago}</p><div class="botones-resumen"><button class="btn btn-editar" onclick="editarCliente()" ${btnEditarDisabled}>✏️ EDITAR</button></div>`;
}

function editarCliente() {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') {
    mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado');
    return;
  }
  if (!clienteActual) return;
  modoEdicion = true;
  document.getElementById('formularioCliente').classList.add('activo');
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('rut').value = clienteActual.rut;
  document.getElementById('razonSocial').value = clienteActual.razonSocial;
  document.getElementById('giro').value = clienteActual.giro;
  document.getElementById('direccion').value = clienteActual.direccion;
  document.getElementById('nombreContacto').value = clienteActual.nombreContacto;
  document.getElementById('celular').value = clienteActual.celular;
  document.getElementById('mail').value = clienteActual.mail;
  document.getElementById('medioPago').value = clienteActual.medioPago || '';
  document.getElementById('btnGuardarCliente').style.display = 'inline-block';
  document.getElementById('btnCancelarEdicion').style.display = 'inline-block';
  mostrarMensaje('MODO EDICIÓN. MODIFIQUE LOS CAMPOS.', 'info');
}

function cancelarEdicion() {
  if (!clienteActual) return;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  document.getElementById('btnGuardarCliente').style.display = 'none';
  document.getElementById('btnCancelarEdicion').style.display = 'none';
  mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
  mostrarResumenCliente(clienteActual);
}

function guardarCliente() {
  if (esLecturaCotizacion) {
    mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado');
    return;
  }
  const rut = document.getElementById('rut').value;
  const rs = document.getElementById('razonSocial').value.trim();
  const gi = document.getElementById('giro').value.trim();
  const di = document.getElementById('direccion').value.trim();
  const nc = document.getElementById('nombreContacto').value.trim();
  const ce = document.getElementById('celular').value.trim();
  const ma = document.getElementById('mail').value.trim();
  const mp = document.getElementById('medioPago').value;
  if (!rs || !gi || !di || !nc || !ce || !ma || !mp) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  if (!validarEmail(ma)) return mostrarMensaje('EMAIL INVÁLIDO', 'error');
  const cliente = {rut, razonSocial: rs.toUpperCase(), giro: gi.toUpperCase(), direccion: di.toUpperCase(), nombreContacto: nc.toUpperCase(), celular: ce, mail: ma.toUpperCase(), medioPago: mp};
  if (modoEdicion) gestorClientes.actualizarCliente(rut, cliente);
  else gestorClientes.agregarCliente(rut, cliente);
  mostrarMensaje(modoEdicion ? 'CLIENTE ACTUALIZADO' : 'CLIENTE GUARDADO', 'exito');
  clienteActual = cliente;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  document.getElementById('btnGuardarCliente').style.display = 'none';
  document.getElementById('btnCancelarEdicion').style.display = 'none';
  mostrarResumenCliente(cliente);
  habilitarProductos();
}

function validarEmail(e) {return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(e);}

function habilitarProductos() {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') {
    document.getElementById('inputCodigoProducto').disabled = true;
    document.getElementById('btnBuscarProducto').disabled = true;
    return;
  }
  document.getElementById('inputCodigoProducto').disabled = false;
  document.getElementById('btnBuscarProducto').disabled = false;
}

function deshabilitarProductos() {
  document.getElementById('inputCodigoProducto').disabled = true;
  document.getElementById('inputCodigoProducto').value = '';
  document.getElementById('btnBuscarProducto').disabled = true;
}

function limpiarCotizacion() {
  document.getElementById('inputRut').value = '';
  document.getElementById('mensaje').style.display = 'none';
  document.getElementById('seccionBloqueada').classList.remove('activa');
  document.getElementById('resumenCliente').classList.remove('activo');
  document.getElementById('formularioCliente').classList.remove('activo');
  document.getElementById('btnGuardarCliente').style.display = 'none';
  document.getElementById('btnCancelarEdicion').style.display = 'none';
  document.getElementById('rut').value = '';
  document.getElementById('razonSocial').value = '';
  document.getElementById('giro').value = '';
  document.getElementById('direccion').value = '';
  document.getElementById('nombreContacto').value = '';
  document.getElementById('celular').value = '';
  document.getElementById('mail').value = '';
  document.getElementById('medioPago').value = '';
  document.getElementById('inputRut').disabled = false;
  
  clienteActual = null;
  modoEdicion = false;
  productosEnCotizacion = [];
  cotizacionGuardada = false;
  datosDespacho = null;
  cotizacionActualIndex = null;
  archivosAdjuntos = [];
  pdfEmitido = false;
  esEdicionCotizacion = false;
  esLecturaCotizacion = false;
  pdfActualDoc = null;
  numeroCotizacionActual = null;
  estadoCotizacionActual = 'pendiente';
  utilidadVisible = false;
  document.getElementById('resumenDespacho').classList.remove('activo');
  document.getElementById('resumenCompra').classList.remove('activo');
  document.getElementById('seccionCierre').classList.remove('activo');
  document.getElementById('btnAceptado').disabled = false;
  document.getElementById('btnRechazado').disabled = false;
  document.getElementById('btnArticulos').disabled = false;
  document.getElementById('btnPDF').disabled = false;
  document.getElementById('btnCotizaciones').disabled = false;
  document.getElementById('btnClientes').disabled = false;
  document.getElementById('btnBuscarCliente').disabled = false;
  document.getElementById('btnLimpiarCliente').disabled = false;
  document.getElementById('btnVerArchivos').style.display = 'none';
  document.getElementById('btnToggleUtilidad').textContent = '📊 VER UTILIDAD';
  document.getElementById('utilidadResumen').classList.remove('visible');
  
  deshabilitarProductos();
  actualizarTablaProductos();
  document.getElementById('inputRut').focus();
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
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') {
    mostrarMensaje('MODO LECTURA. NO SE PUEDEN AGREGAR PRODUCTOS.', 'bloqueado');
    return;
  }
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
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') {
    mostrarMensaje('MODO LECTURA. NO SE PUEDEN AGREGAR PRODUCTOS.', 'bloqueado');
    return;
  }
  const ex = productosEnCotizacion.find(p => p.codigo === cod);
  if (ex) {
    ex.cantidad++;
    ex.total = +(ex.cantidad * ex.valorNetaConDescuento).toFixed(2);
  } else {
    productosEnCotizacion.push({codigo: prod.codigo, descripcion: prod.descripcion, cantidad: 1, valorNeto: prod.valorNeto, costo: prod.costo, descuento: 0, valorNetaConDescuento: prod.valorNeto, total: prod.valorNeto.toFixed(2), proveedor: prod.proveedor, link: prod.link});
  }
  actualizarTablaProductos();
}

function actualizarTablaProductos() {
  const cont = document.getElementById('tablaProductosContenedor');
  if (productosEnCotizacion.length === 0) {
    cont.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS</div>';
    document.getElementById('resumenTotales').style.display = 'none';
    document.getElementById('utilidadResumen').classList.remove('visible');
    return;
  }
  let html = '<table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th style="width:70px;">CANTIDAD</th><th style="width:80px;">VALOR NETO</th><th style="width:80px;">DESCUENTO (%)</th><th style="width:80px;">V. NETO DESC.</th><th style="width:80px;">COSTO</th><th style="width:90px;">COSTO TOTAL</th><th style="width:80px;">TOTAL</th><th style="width:100px;">PROVEEDOR</th><th style="width:60px;">ACCIÓN</th></tr></thead><tbody>';
  productosEnCotizacion.forEach((p, i) => {
    const bloqueado = esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado';
    const inputQuantityDisabled = bloqueado ? 'disabled' : '';
    const inputDescuentoDisabled = bloqueado ? 'disabled' : '';
    const btnEliminarDisplay = bloqueado ? 'none' : 'block';
    const costoTotal = parseFloat(p.costo) * p.cantidad;
    html += `<tr><td>${p.codigo}</td><td>${p.descripcion}</td><td class="texto-centrado"><input type="number" min="1" value="${p.cantidad}" onchange="actualizarCantidad(${i}, this.value)" ${inputQuantityDisabled} style="width:100%;text-align:center;"></td><td class="valor-numerico">$${parseFloat(p.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td><input type="number" min="0" max="100" step="0.01" value="${p.descuento}" style="width:100%;padding:5px;text-align:center;" onchange="actualizarDescuento(${i}, this.value)" ${inputDescuentoDisabled}></td><td class="valor-numerico">$${parseFloat(p.valorNetaConDescuento).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(p.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(costoTotal).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(p.total).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${p.proveedor}</td><td><button class="btn-eliminar" onclick="eliminarProducto(${i})" style="display:${btnEliminarDisplay}">ELIMINAR</button></td></tr>`;
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
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') {
    mostrarMensaje('MODO LECTURA. NO SE PUEDEN ELIMINAR PRODUCTOS.', 'bloqueado');
    return;
  }
  productosEnCotizacion.splice(i, 1);
  actualizarTablaProductos();
}

function actualizarResumenTotales() {
  const net = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.total), 0);
  const iva = +(net * 0.19).toFixed(2);
  const tot = +(net + iva).toFixed(2);
  document.getElementById('totalNeto').textContent = '$' + net.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalIva').textContent = '$' + iva.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalGeneral').textContent = '$' + tot.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('resumenTotales').style.display = 'block';
  
  let htmlUtilidad = '<div class="resumen-utilidad" id="resumenUtilidadInterno"><h4>📊 UTILIDAD (INTERNO)</h4>';
  let utilidadTotal = 0, ventaTotal = 0, costoTotal = 0;
  productosEnCotizacion.forEach(p => {
    const costoProducto = parseFloat(p.costo) * p.cantidad;
    const utilidad = parseFloat(p.total) - costoProducto;
    const venta = parseFloat(p.total);
    const porcentajeUtilidad = venta > 0 ? ((utilidad / venta) * 100).toFixed(2) : 0;
    utilidadTotal += utilidad;
    ventaTotal += venta;
    costoTotal += costoProducto;
    htmlUtilidad += `<div class="utilidad-item"><strong>${p.codigo}:</strong> <span>$${Math.round(utilidad).toLocaleString('es-CL')}</span><span class="utilidad-porcentaje">${porcentajeUtilidad}%</span><span class="utilidad-costo">Costo: $${Math.round(costoProducto).toLocaleString('es-CL')}</span></div>`;
  });
  const porcentajeUtilidadTotal = ventaTotal > 0 ? ((utilidadTotal / ventaTotal) * 100).toFixed(2) : 0;
  htmlUtilidad += `<div class="utilidad-item" style="border-top: 1px solid #999; padding-top: 10px; margin-top: 10px;"><strong>UTILIDAD TOTAL:</strong> <span>$${Math.round(utilidadTotal).toLocaleString('es-CL')}</span><span class="utilidad-porcentaje">${porcentajeUtilidadTotal}%</span><span class="utilidad-costo">Costo Total: $${Math.round(costoTotal).toLocaleString('es-CL')}</span></div></div>`;
  document.getElementById('utilidadResumen').innerHTML = htmlUtilidad;
}

function toggleUtilidad() {
  utilidadVisible = !utilidadVisible;
  const btn = document.getElementById('btnToggleUtilidad');
  const utilidadEl = document.getElementById('utilidadResumen');
  
  if (utilidadVisible) {
    utilidadEl.classList.add('visible');
    btn.textContent = '📊 OCULTAR UTILIDAD';
  } else {
    utilidadEl.classList.remove('visible');
    btn.textContent = '📊 VER UTILIDAD';
  }
}

function abrirArticulos() {
  document.getElementById('modalArticulos').style.display = 'block';
  listarArticulos();
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
      html += `<tr><td>${art.codigo}</td><td>${art.descripcion}</td><td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">${parseFloat(art.porcentaje).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(art.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${parseFloat(util).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${art.proveedor}</td><td style="text-transform:lowercase;"><a href="${art.link}" target="_blank">${art.link}</a></td><td><button class="btn btn-editar" onclick="editarArticulo('${art.codigo}')">EDITAR</button><button class="btn btn-eliminar" onclick="eliminarArticulo('${art.codigo}')">ELIMINAR</button></td></tr>`;
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

function abrirClientes() {
  document.getElementById('modalClientes').style.display = 'block';
  listarClientes();
}

function cerrarClientes() {
  document.getElementById('modalClientes').style.display = 'none';
}

function listarClientes() {
  const todos = gestorClientes.obtenerTodos();
  let html = '<table class="tabla-clientes"><thead><tr><th>RUT</th><th>RAZÓN SOCIAL</th><th>GIRO</th><th>CONTACTO</th><th>CELULAR</th><th>MAIL</th><th>ACCIÓN</th></tr></thead><tbody>';
  const claves = Object.keys(todos);
  if (claves.length === 0) {
    html += '<tr><td colspan="7" style="text-align:center;">NO HAY CLIENTES REGISTRADOS</td></tr>';
  } else {
    claves.forEach(rut => {
      const cli = todos[rut];
      html += `<tr><td>${cli.rut}</td><td>${cli.razonSocial}</td><td>${cli.giro}</td><td>${cli.nombreContacto}</td><td>${cli.celular}</td><td>${cli.mail}</td><td><button class="btn btn-editar" onclick="editarClienteDesdeModal('${cli.rut}')">EDITAR</button></td></tr>`;
    });
  }
  html += '</tbody></table>';
  document.getElementById('listaClientes').innerHTML = html;
}

function editarClienteDesdeModal(rut) {
  const cliente = gestorClientes.buscarPorRut(rut);
  if (!cliente) return;
  clienteActual = cliente;
  mostrarResumenCliente(cliente);
  document.getElementById('inputRut').value = rut;
  document.getElementById('formularioCliente').classList.remove('activo');
  cerrarClientes();
  mostrarMensaje('CLIENTE CARGADO PARA EDITAR', 'exito');
  habilitarProductos();
}

function generarPDF() {
  if (!clienteActual) {alert('INGRESE CLIENTE'); return;}
  if (productosEnCotizacion.length === 0) {alert('AGREGUE PRODUCTOS'); return;}
  if (estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') {
    mostrarMensaje('COTIZACIÓN BLOQUEADA. NO SE PUEDE REGENERAR PDF.', 'bloqueado');
    return;
  }
  
  let numCot;
  if (esEdicionCotizacion && cotizacionActualIndex !== null) {
    numCot = cotizacionesEmitidas[cotizacionActualIndex].numero;
  } else {
    numCot = gestorCotizaciones.siguienteCotizacion();
  }
  
  numeroCotizacionActual = numCot;
  const net = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.total), 0);
  const estado = datosDespacho ? 'aceptado' : 'pendiente';
  const cotizacion = {numero: numCot, razonSocial: clienteActual.razonSocial, neto: net.toFixed(2), fecha: new Date().toISOString(), cliente: JSON.parse(JSON.stringify(clienteActual)), productos: JSON.parse(JSON.stringify(productosEnCotizacion)), estado: estado, despacho: datosDespacho || null};
  
  if (esEdicionCotizacion && cotizacionActualIndex !== null) {
    cotizacionesEmitidas[cotizacionActualIndex] = cotizacion;
    mostrarMensaje('COTIZACIÓN ACTUALIZADA CORRECTAMENTE', 'exito');
  } else {
    cotizacionesEmitidas.push(cotizacion);
    mostrarMensaje('COTIZACIÓN GUARDADA CORRECTAMENTE', 'exito');
  }
  
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  pdfEmitido = true;
  document.getElementById('seccionCierre').classList.add('activo');
  generarPDFDocumento(cotizacion);
}

function generarPDFDocumento(cotizacion) {
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();
  const net = parseFloat(cotizacion.neto);
  const iva = +(net * 0.19).toFixed(2);
  const tot = +(net + iva).toFixed(2);
  const fechaEmision = new Date(cotizacion.fecha);
  const fechaFormateada = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
  const horaFormateada = `${fechaEmision.getHours().toString().padStart(2, '0')}:${fechaEmision.getMinutes().toString().padStart(2, '0')}:${fechaEmision.getSeconds().toString().padStart(2, '0')}`;

  doc.setFillColor(31, 111, 139);
  doc.rect(0, 0, 210, 25, 'F');
  doc.setTextColor(255, 255, 255);
  doc.setFontSize(22);
  doc.setFont(undefined, 'bold');
  doc.text('COTIZACIÓN', 15, 12);
  doc.setFontSize(12);
  doc.setFont(undefined, 'bold');
  doc.setTextColor(255, 255, 255);
  doc.text(`N° ${cotizacion.numero}`, 180, 12, {align: 'right'});
  doc.setFontSize(10);
  doc.setTextColor(255, 255, 255);
  doc.text(`Fecha: ${fechaFormateada}`, 180, 18, {align: 'right'});

  doc.setTextColor(0, 0, 0);
  doc.setFontSize(11);
  doc.setFont(undefined, 'bold');
  doc.text('DATOS DEL CLIENTE', 15, 40);
  doc.setDrawColor(242, 92, 5);
  doc.setLineWidth(0.3);
  doc.line(15, 42, 195, 42);
  doc.setFontSize(9);
  doc.setFont(undefined, 'normal');
  const clienteData = [
    ['RUT:', cotizacion.cliente.rut, 'CONTACTO:', cotizacion.cliente.nombreContacto],
    ['RAZÓN SOCIAL:', cotizacion.cliente.razonSocial, 'CELULAR:', cotizacion.cliente.celular],
    ['GIRO:', cotizacion.cliente.giro, 'MAIL:', cotizacion.cliente.mail],
    ['DIRECCIÓN:', cotizacion.cliente.direccion, 'MEDIO PAGO:', cotizacion.cliente.medioPago]
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

  doc.setDrawColor(242, 92, 5);
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
    body: cotizacion.productos.map(p => [p.codigo, p.descripcion, p.cantidad.toString(), `$${Math.round(parseFloat(p.valorNetaConDescuento)).toLocaleString('es-CL')}`, `$${Math.round(parseFloat(p.total)).toLocaleString('es-CL')}`]),
    theme: 'striped',
    styles: {fontSize: 8, cellPadding: 3},
    headStyles: {fillColor: [31, 111, 139], textColor: 255, fontStyle: 'bold'},
    columnStyles: {0: {cellWidth: 20}, 1: {cellWidth: 90}, 2: {cellWidth: 15, halign: 'center'}, 3: {cellWidth: 30, halign: 'right'}, 4: {cellWidth: 30, halign: 'right'}},
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

  doc.setFontSize(8);
  doc.setFont(undefined, 'bold');
  doc.setTextColor(50, 50, 50);
  doc.text('Validez de la cotización: 5 días hábiles desde la emisión de este documento.', 15, 260);

  doc.setFontSize(7);
  doc.setFont(undefined, 'normal');
  doc.setTextColor(100, 100, 100);
  doc.text('RUT: 78000256-0 | Razón social: Empresa Servicios SPA | Dirección: Los Pepinos 287, Las Condes.', 15, 268);
  doc.text('Mail: contacto@servicios.cl | Teléfono: 56 22 5510365', 15, 272);
  doc.text(`Hora de emisión: ${horaFormateada}`, 15, 276);
  doc.setTextColor(150, 150, 150);
  doc.setFontSize(8);
  doc.setFont(undefined, 'italic');
  doc.text('Gracias por su preferencia', 105, 283, {align: 'center'});
  
  pdfActualDoc = doc;
  mostrarPDFEnMismaVentana();
}

function mostrarPDFEnMismaVentana() {
  const pdfDataUri = pdfActualDoc.output('datauristring');
  const modal = document.getElementById('modalPDF');
  const iframe = document.getElementById('pdfIframe');
  iframe.src = pdfDataUri;
  modal.classList.add('mostrar');
}

function cerrarModalPDF() {
  const modal = document.getElementById('modalPDF');
  modal.classList.remove('mostrar');
  document.getElementById('pdfIframe').src = '';
  limpiarCotizacion();
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones');
  const cont = document.getElementById('listaCotizaciones');
  if (cotizacionesEmitidas.length === 0) {
    cont.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 20px;">NO HAY COTIZACIONES EMITIDAS</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th>FECHA EMISIÓN</th><th>ESTADO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    cotizacionesEmitidas.forEach((c, index) => {
      const claseEstado = c.estado === 'aceptado' ? 'cot-aceptado' : (c.estado === 'rechazado' ? 'cot-rechazado' : 'cot-pendiente');
      const fechaEmision = new Date(c.fecha);
      const fechaFormateada = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
      const badgeClase = c.estado === 'aceptado' ? 'badge-aceptado' : (c.estado === 'rechazado' ? 'badge-rechazado' : 'badge-pendiente');
      const btnEditarDisabled = (c.estado === 'rechazado') ? 'disabled' : '';
      html += `<tr class="${claseEstado}"><td>${c.numero}</td><td>${c.razonSocial}</td><td style="text-align:right;">$${parseFloat(c.neto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${fechaFormateada}</td><td><span class="badge-estado ${badgeClase}">${c.estado}</span></td><td style="text-align:center;"><button class="btn-ver" onclick="verCotizacion(${index})">VER</button><button class="btn-editar-cot" onclick="editarCotizacionGuardada(${index})" ${btnEditarDisabled}>EDITAR</button></td></tr>`;
    });
    html += '</tbody></table>';
    cont.innerHTML = html;
  }
  modal.style.display = 'block';
}

function verCotizacion(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  generarPDFDocumento(cotizacionesEmitidas[index]);
}

function editarCotizacionGuardada(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  const cotizacion = cotizacionesEmitidas[index];
  
  if (cotizacion.estado === 'rechazado') {
    mostrarMensaje('COTIZACIÓN RECHAZADA. NO SE PUEDE EDITAR.', 'bloqueado');
    return;
  }
  
  clienteActual = JSON.parse(JSON.stringify(cotizacion.cliente));
  productosEnCotizacion = JSON.parse(JSON.stringify(cotizacion.productos));
  datosDespacho = cotizacion.despacho;
  cotizacionActualIndex = index;
  pdfEmitido = true;
  estadoCotizacionActual = cotizacion.estado;
  
  if (cotizacion.estado === 'aceptado') {
    esLecturaCotizacion = true;
    esEdicionCotizacion = false;
  } else {
    esLecturaCotizacion = false;
    esEdicionCotizacion = true;
  }
  
  gestorCotizaciones.establecerNumero(cotizacion.numero);
  
  document.getElementById('seccionCierre').classList.add('activo');
  
  cotizacionGuardada = true;
  
  if (cotizacion.estado === 'aceptado') {
    document.getElementById('seccionBloqueada').classList.add('activa');
    document.getElementById('btnAceptado').disabled = true;
    document.getElementById('btnRechazado').disabled = false;
    document.getElementById('btnArticulos').disabled = false;
    document.getElementById('btnPDF').disabled = false;
    document.getElementById('btnCotizaciones').disabled = false;
    document.getElementById('btnClientes').disabled = false;
    document.getElementById('btnLimpiarCotizacion').disabled = false;
  } else {
    document.getElementById('seccionBloqueada').classList.remove('activa');
    document.getElementById('btnAceptado').disabled = false;
    document.getElementById('btnRechazado').disabled = false;
  }
  
  mostrarResumenCliente(clienteActual);
  actualizarTablaProductos();
  habilitarProductos();
  
  if (datosDespacho) {
    mostrarResumenDespacho();
    mostrarResumenCompra();
    document.getElementById('btnVerArchivos').style.display = 'inline-block';
  }
  
  document.getElementById('inputRut').disabled = true;
  
  cerrarCotizaciones();
  
  if (cotizacion.estado === 'aceptado') {
    mostrarMensaje(`COTIZACIÓN ${cotizacion.numero} EN MODO LECTURA - PARA REVISAR PRODUCTOS Y COSTOS`, 'info');
  } else {
    mostrarMensaje(`COTIZACIÓN ${cotizacion.numero} CARGADA PARA EDITAR`, 'info');
  }
}

function cerrarCotizaciones() {
  document.getElementById('modalCotizaciones').style.display = 'none';
}

function mostrarResumenCompra() {
  const totalCosto = productosEnCotizacion.reduce((acc, p) => acc + (parseFloat(p.costo) * p.cantidad), 0);
  let html = '<h4>📋 RESUMEN DE COMPRA</h4><table class="tabla-compra"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANTIDAD</th><th>COSTO UNITARIO</th><th>PROVEEDOR</th><th>LINK PROVEEDOR</th></tr></thead><tbody>';
  productosEnCotizacion.forEach(p => {
    const linkSeguro = p.link.startsWith('http') ? p.link : 'https://' + p.link;
    html += `<tr><td>${p.codigo}</td><td>${p.descripcion}</td><td class="valor-numerico">${p.cantidad}</td><td class="valor-numerico">$${parseFloat(p.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${p.proveedor}</td><td><a href="${linkSeguro}" target="_blank">${p.link}</a></td></tr>`;
  });
  html += `<tr style="background: #fff3cd; font-weight: bold;"><td colspan="5" style="text-align: right;">TOTAL COSTO DE COMPRA:</td><td class="valor-numerico">$${totalCosto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td></tr>`;
  html += '</tbody></table>';
  document.getElementById('resumenCompra').innerHTML = html;
  document.getElementById('resumenCompra').classList.add('activo');
}

function marcarAceptado() {
  if (!clienteActual) { alert('DEBE SELECCIONAR UN CLIENTE PRIMERO'); return; }
  if (productosEnCotizacion.length === 0) { alert('DEBE AGREGAR PRODUCTOS A LA COTIZACIÓN'); return; }
  if (!pdfEmitido) { alert('DEBE GENERAR PDF PRIMERO'); return; }
  archivosAdjuntos = [];
  document.getElementById('tipoEntrega').value = '';
  document.getElementById('regionSelect').value = '';
  document.getElementById('comunaInput').value = datosDespacho ? datosDespacho.comuna : '';
  document.getElementById('direccionDespacho').value = datosDespacho ? datosDespacho.direccion : '';
  document.getElementById('contactoDespacho').value = datosDespacho ? datosDespacho.contacto : '';
  document.getElementById('celularDespacho').value = datosDespacho ? datosDespacho.celular : '';
  document.getElementById('infoArchivo').textContent = '';
  document.getElementById('adjuntosContainer').style.display = 'none';
  document.getElementById('listaAdjuntos').innerHTML = '';
  if (datosDespacho && datosDespacho.archivos) {
    archivosAdjuntos = JSON.parse(JSON.stringify(datosDespacho.archivos));
    document.getElementById('infoArchivo').textContent = `${archivosAdjuntos.length} archivo(s) adjunto(s)`;
    actualizarListaAdjuntos();
  }
  document.getElementById('modalAceptado').style.display = 'block';
}

function cerrarModalAceptado() {
  document.getElementById('modalAceptado').style.display = 'none';
}

function abrirSelectorArchivos() {
  document.getElementById('inputArchivo').click();
}

function agregarArchivo(event) {
  const file = event.target.files[0];
  if (!file) return;
  const nuevoArchivo = { nombre: file.name, tamaño: (file.size / 1024).toFixed(2), tipo: file.type, contenido: '' };
  
  const reader = new FileReader();
  reader.onload = function(e) {
    nuevoArchivo.contenido = e.target.result;
    archivosAdjuntos.push(nuevoArchivo);
    actualizarListaAdjuntos();
    document.getElementById('inputArchivo').value = '';
    document.getElementById('infoArchivo').textContent = `${archivosAdjuntos.length} archivo(s) adjunto(s)`;
  };
  reader.readAsDataURL(file);
}

function actualizarListaAdjuntos() {
  const container = document.getElementById('adjuntosContainer');
  const lista = document.getElementById('listaAdjuntos');
  if (archivosAdjuntos.length === 0) {
    container.style.display = 'none';
    return;
  }
  container.style.display = 'block';
  lista.innerHTML = '';
  archivosAdjuntos.forEach((archivo, index) => {
    const li = document.createElement('li');
    li.className = 'item-adjunto';
    li.innerHTML = `<span class="nombre-archivo">${archivo.nombre} (${archivo.tamaño} KB)</span><button class="btn-eliminar-archivo" onclick="eliminarArchivo(${index})">ELIMINAR</button>`;
    lista.appendChild(li);
  });
}

function eliminarArchivo(index) {
  archivosAdjuntos.splice(index, 1);
  actualizarListaAdjuntos();
  document.getElementById('infoArchivo').textContent = archivosAdjuntos.length > 0 ? `${archivosAdjuntos.length} archivo(s) adjunto(s)` : '';
}

function confirmarAceptacion() {
  const tipoEntrega = document.getElementById('tipoEntrega').value;
  const region = document.getElementById('regionSelect').value;
  const comuna = document.getElementById('comunaInput').value.trim();
  const direccion = document.getElementById('direccionDespacho').value.trim();
  const contacto = document.getElementById('contactoDespacho').value.trim();
  const celular = document.getElementById('celularDespacho').value.trim();
  
  if (!tipoEntrega) { alert('DEBE SELECCIONAR UN TIPO DE ENTREGA'); return; }
  if (!region) { alert('DEBE SELECCIONAR UNA REGIÓN'); return; }
  if (!comuna) { alert('DEBE INGRESAR UNA COMUNA'); return; }
  if (!direccion) { alert('DEBE INGRESAR DIRECCIÓN'); return; }
  if (!contacto) { alert('DEBE INGRESAR CONTACTO DE DESPACHO'); return; }
  if (!celular) { alert('DEBE INGRESAR CELULAR DE CONTACTO'); return; }
  
  datosDespacho = {
    tipoEntrega: tipoEntrega,
    region: region.toUpperCase(),
    comuna: comuna.toUpperCase(),
    direccion: direccion.toUpperCase(),
    contacto: contacto.toUpperCase(),
    celular: celular,
    archivos: JSON.parse(JSON.stringify(archivosAdjuntos))
  };

  cotizacionGuardada = true;
  estadoCotizacionActual = 'aceptado';
  esLecturaCotizacion = true;
  mostrarResumenDespacho();
  mostrarResumenCompra();
  cerrarModalAceptado();
  bloquearEdicionConBotonesActivos();
  document.getElementById('seccionBloqueada').classList.add('activa');
  
  if (cotizacionActualIndex !== null) {
    cotizacionesEmitidas[cotizacionActualIndex].estado = 'aceptado';
    cotizacionesEmitidas[cotizacionActualIndex].despacho = datosDespacho;
    localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  }
  
  document.getElementById('btnVerArchivos').style.display = 'inline-block';
  alert('COTIZACIÓN ACEPTADA Y GUARDADA CORRECTAMENTE');
}

function mostrarResumenDespacho() {
  if (!datosDespacho) return;
  const resumen = document.getElementById('resumenDespacho');
  resumen.className = 'resumen-despacho activo';
  resumen.innerHTML = `<h4>✓ COTIZACIÓN ACEPTADA</h4><p><strong>TIPO ENTREGA:</strong> ${datosDespacho.tipoEntrega}</p><p><strong>REGIÓN:</strong> ${datosDespacho.region}</p><p><strong>COMUNA:</strong> ${datosDespacho.comuna}</p><p><strong>DIRECCIÓN:</strong> ${datosDespacho.direccion}</p><p><strong>CONTACTO DESPACHO:</strong> ${datosDespacho.contacto}</p><p><strong>CELULAR CONTACTO:</strong> ${datosDespacho.celular}</p><p><strong>ARCHIVOS ADJUNTOS:</strong> ${datosDespacho.archivos.length}</p>`;
}

function bloquearEdicionConBotonesActivos() {
  document.getElementById('btnAceptado').disabled = true;
  document.getElementById('btnRechazado').disabled = false;
  document.getElementById('btnArticulos').disabled = false;
  document.getElementById('btnPDF').disabled = false;
  document.getElementById('btnCotizaciones').disabled = false;
  document.getElementById('btnClientes').disabled = false;
  document.getElementById('btnBuscarCliente').disabled = true;
  document.getElementById('btnLimpiarCliente').disabled = false;
  document.getElementById('btnBuscarProducto').disabled = true;
  document.getElementById('btnLimpiarCotizacion').disabled = false;
  document.getElementById('inputCodigoProducto').disabled = true;
  document.getElementById('inputRut').disabled = true;
  actualizarTablaProductos();
}

function marcarRechazado() {
  if (!clienteActual) { alert('DEBE SELECCIONAR UN CLIENTE PRIMERO'); return; }
  if (!pdfEmitido) { alert('DEBE GENERAR PDF PRIMERO'); return; }
  
  cotizacionGuardada = true;
  estadoCotizacionActual = 'rechazado';
  bloquearEdicion();
  document.getElementById('seccionBloqueada').classList.add('activa');
  
  if (cotizacionActualIndex !== null) {
    cotizacionesEmitidas[cotizacionActualIndex].estado = 'rechazado';
    localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  }
  
  alert('COTIZACIÓN RECHAZADA Y GUARDADA');
}

function bloquearEdicion() {
  document.getElementById('btnArticulos').disabled = true;
  document.getElementById('btnBuscarProducto').disabled = true;
  document.getElementById('btnAceptado').disabled = true;
  document.getElementById('btnRechazado').disabled = true;
  document.getElementById('btnBuscarCliente').disabled = true;
  document.getElementById('btnLimpiarCliente').disabled = false;
  document.getElementById('btnPDF').disabled = true;
  document.getElementById('btnCotizaciones').disabled = false;
  document.getElementById('btnClientes').disabled = false;
  document.getElementById('inputCodigoProducto').disabled = true;
  document.getElementById('inputRut').disabled = true;
  actualizarTablaProductos();
}

function verArchivos() {
  if (!datosDespacho || !datosDespacho.archivos || datosDespacho.archivos.length === 0) {
    alert('NO HAY ARCHIVOS ADJUNTOS');
    return;
  }
  
  const modal = document.getElementById('modalArchivos');
  const lista = document.getElementById('listaArchivosModal');
  let html = '<div class="seccion-adjuntos" style="display:block;"><ul class="lista-adjuntos">';
  datosDespacho.archivos.forEach((archivo, index) => {
    html += `<li class="item-adjunto"><span class="nombre-archivo">${archivo.nombre} (${archivo.tamaño} KB)</span><button class="btn-ver-archivo" onclick="verArchivo(${index})">VER</button></li>`;
  });
  html += '</ul></div>';
  lista.innerHTML = html;
  modal.style.display = 'block';
}

function verArchivo(index) {
  if (!datosDespacho || !datosDespacho.archivos || index < 0 || index >= datosDespacho.archivos.length) {
    alert('ERROR: ARCHIVO NO ENCONTRADO');
    return;
  }
  
  const archivo = datosDespacho.archivos[index];
  const modalVisualizar = document.getElementById('modalVisualizarArchivo');
  const contenido = document.getElementById('contenidoArchivo');
  
  if (archivo.tipo.startsWith('image/')) {
    contenido.innerHTML = `<img src="${archivo.contenido}" alt="${archivo.nombre}" />`;
  } else if (archivo.tipo === 'application/pdf') {
    contenido.innerHTML = `<iframe src="${archivo.contenido}"></iframe>`;
  } else if (archivo.tipo.startsWith('text/')) {
    const xhr = new XMLHttpRequest();
    xhr.responseType = 'arraybuffer';
    xhr.onload = function() {
      const texto = new TextDecoder().decode(xhr.response);
      contenido.innerHTML = `<pre>${texto}</pre>`;
    };
    xhr.onerror = function() {
      contenido.innerHTML = `<pre>No se pudo visualizar el contenido del archivo</pre>`;
    };
    xhr.open('GET', archivo.contenido);
    xhr.send();
  } else {
    contenido.innerHTML = `<p>Tipo de archivo no soportado para vista previa: ${archivo.tipo}</p>`;
  }
  
  modalVisualizar.style.display = 'block';
}

function cerrarModalArchivos() {
  document.getElementById('modalArchivos').style.display = 'none';
}

function cerrarModalVisualizarArchivo() {
  document.getElementById('modalVisualizarArchivo').style.display = 'none';
  document.getElementById('contenidoArchivo').innerHTML = '';
}
</script>
</body>
</html>
