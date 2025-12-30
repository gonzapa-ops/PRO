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
.seccion-titulo {font-weight: 700; font-size: 16px; margin-bottom: 15px; padding-bottom: 8px; border-bottom: 2px solid #F25C05; text-transform: uppercase; color: #1F6F8B;}
table {width: 100%; border-collapse: collapse; margin-bottom: 15px; font-size: 11px;}
th, td {border: 1px solid #ddd; padding: 6px; text-transform: uppercase; color: #3B3B3B;}
th {background: #1F6F8B; color: white; font-weight: 700;}
tr:nth-child(even) {background: #E9F0EA;}
.valor-numerico {text-align: right; font-weight: 700; color: #3B3B3B;}
.texto-centrado {text-align: center;}
button {cursor: pointer; border: none; border-radius: 2px; font-weight: 700; text-transform: uppercase; transition: all 0.3s ease; padding: 6px 10px; font-size: 10px;}
.btn-buscar {background: #F25C05; color: white;}
.btn-buscar:hover {background: #cb4a04;}
.btn-buscar:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-limpiar {background: #7A8C52; color: white;}
.btn-limpiar:hover {background: #5a6b37;}
.btn-limpiar:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-eliminar {background: #9B2E00; color: white; font-size: 9px; padding: 4px 6px;}
.btn-eliminar:hover {background: #7a2300;}
.btn-articulos {background: #4B732E; color: white;}
.btn-articulos:hover {background: #385525;}
.btn-articulos:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-pdf {background: #F25C05; color: white; padding: 8px 12px; font-size: 11px;}
.btn-pdf:hover {background: #cb4a04;}
.btn-pdf:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-agregar {background: #385525; color: white;}
.btn-agregar:hover {background: #274015;}
.btn-editar {background: #D9822B; color: white;}
.btn-editar:hover {background: #b36e1e;}
.btn-editar:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-cancelar {background: #9B2E00; color: white;}
.btn-cancelar:hover {background: #7a2300;}
.btn-ver {background: #556B2F; color: white; padding: 4px 5px; font-size: 8px; margin-right: 2px;}
.btn-ver:hover {background: #3f4f20;}
.btn-eliminar-cot {background: #9B2E00; color: white; padding: 4px 5px; font-size: 8px; margin-left: 2px;}
.btn-eliminar-cot:hover {background: #7a2300;}
.formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none;}
.formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
.campo-grupo {margin-bottom: 12px;}
.campo-grupo label {display: block; font-weight: 700; color: #3B3B3B; margin-bottom: 4px; font-size: 11px; text-transform: uppercase;}
.campo-grupo input, .campo-grupo select, .campo-grupo textarea {width: 100%; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase; font-family: inherit;}
.campo-grupo input:focus, .campo-grupo select:focus, .campo-grupo textarea:focus {outline: none; border-color: #F25C05;}
.campo-grupo input:disabled, .campo-grupo select:disabled, .campo-grupo textarea:disabled {background-color: #F5F5F5; cursor: not-allowed; color: #777;}
.fila-campos {display: grid; grid-template-columns: 1fr 1fr; gap: 12px;}
.fila-campos-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px;}
.fila-campos-dos {display: grid; grid-template-columns: 1fr 1fr; gap: 12px;}
@media (max-width: 768px) {
  .fila-campos, .fila-campos-tres {grid-template-columns: 1fr;}
  .cotizador-header {padding: 8px 12px;}
  .empresa-nombre {font-size: 12px;}
  .numero-cotizacion {font-size: 10px;}
  .seccion-titulo {font-size: 14px;}
  button {padding: 5px 8px; font-size: 9px;}
  th, td {padding: 4px; font-size: 9px;}
  table {font-size: 9px;}
}
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
.contenedor-rut {flex: 1; min-width: 150px; display: flex; align-items: center; gap: 6px; position: relative;}
.contenedor-rut input {flex: 1; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase;}
.btn-lupa {background: transparent; border: none; color: #4B732E; padding: 4px; font-size: 20px; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.3s ease;}
.btn-lupa:hover {color: #385525; transform: scale(1.2);}
.busqueda-rut-botones {display: flex; gap: 6px; align-items: center; flex-wrap: wrap;}
.seccion-productos {margin-bottom: 20px; border: 1px solid #ddd; border-radius: 2px; padding: 15px; background-color: #fafafa; overflow-x: auto;}
.busqueda-producto {display: flex; gap: 6px; margin-bottom: 15px; align-items: center; position: relative; flex-wrap: wrap;}
.busqueda-producto input {flex: 1; min-width: 150px; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase;}
.autocomplete-list {display: none; position: absolute; top: 100%; left: 0; right: 0; background: white; border: 1px solid #ddd; border-top: none; max-height: 200px; overflow-y: auto; z-index: 1000; border-radius: 0 0 2px 2px; box-shadow: 0 4px 8px rgba(0,0,0,0.07);}
.autocomplete-list.activo {display: block;}
.autocomplete-item {padding: 8px; cursor: pointer; background: white; font-size: 10px; text-transform: uppercase; border-bottom: 1px solid #eee; transition: background 0.2s;}
.autocomplete-item:hover {background: #f9e2cd; border-left: 3px solid #F25C05;}
.formulario-producto {background: linear-gradient(135deg, #f9ead4 0%, #f7dba1 100%); padding: 12px; border-radius: 2px; border-left: 5px solid #F25C05; margin-bottom: 15px;}
.formulario-producto h4 {text-transform: uppercase; color: #b35304; margin-bottom: 12px; font-weight: 700; font-size: 12px;}
.fila-campos-producto {display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;}
.fila-campos-producto-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 12px;}
.botones-producto {display: flex; gap: 6px; margin-top: 8px; flex-wrap: wrap;}
.alerta-sin-productos {background-color: #e9f6ec; padding: 12px; border-radius: 2px; border-left: 5px solid #4B732E; text-align: center; color: #4B732E; text-transform: uppercase; font-weight: 700; font-size: 11px;}
#modalArticulos {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 9999; overflow: auto; padding: 10px;}
.articulos-content {background: white; border-radius: 2px; margin: 20px auto; padding: 15px; max-width: 95%; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15);}
.modal-titulo {font-size: 16px; font-weight: 700; margin-bottom: 15px; text-transform: uppercase; color: #3B3B3B;}
.btn-cerrar-modal {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.botones-superiores {display: flex; gap: 6px; margin-bottom: 15px; flex-wrap: wrap; align-items: center;}
#modalCotizaciones {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10000; overflow: auto; padding: 10px;}
#modalCotizaciones > div {background: white; max-width: 95%; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); overflow-x: auto;}
#modalClientes {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10001; overflow: auto; padding: 10px;}
.modal-clientes-content {background: white; max-width: 90%; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); max-height: 90vh; overflow-y: auto;}
.btn-cerrar-clientes {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.tabla-clientes-modal {width: 100%; border-collapse: collapse; background: white;}
.tabla-clientes-modal th {background: #1F6F8B; color: white; padding: 8px; text-align: left; text-transform: uppercase; font-weight: 700; font-size: 10px;}
.tabla-clientes-modal td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B; font-size: 10px;}
.tabla-clientes-modal tr:nth-child(even) {background: #E9F0EA;}
.btn-seleccionar-cliente {background: #4B732E; color: white; padding: 4px 8px; font-size: 9px; cursor: pointer; border: none; border-radius: 2px;}
.btn-seleccionar-cliente:hover {background: #385525;}
.resumen-totales {max-width: 350px; margin-left: auto; border-top: 3px solid #F25C05; padding-top: 12px; text-transform: uppercase; margin-bottom: 15px; font-size: 12px;}
.resumen-linea {display: flex; justify-content: space-between; margin: 4px 0; font-weight: 700; font-size: 11px; color: #3B3B3B;}
.resumen-linea.total {font-size: 14px; color: #000; font-weight: 900;}
.tabla-cotizaciones {width: 100%; border-collapse: collapse; background: white;}
.tabla-cotizaciones th {background: #1F6F8B; color: white; padding: 8px; text-align: left; text-transform: uppercase; font-weight: 700; font-size: 10px;}
.tabla-cotizaciones td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B; font-size: 10px;}
.tabla-cotizaciones tr.cot-enviada {background-color: #E1F5FE !important;}
.tabla-cotizaciones tr.cot-aceptado {background-color: #C8E6C9 !important;}
.tabla-cotizaciones tr.cot-rechazado {background-color: #FFCDD2 !important;}
.tabla-clientes {width: 100%; border-collapse: collapse; background: white;}
.tabla-clientes th {background: #1F6F8B; color: white; padding: 8px; text-align: left; text-transform: uppercase; font-weight: 700; font-size: 10px;}
.tabla-clientes td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B; font-size: 10px;}
.tabla-clientes tr:nth-child(even) {background: #E9F0EA;}
.seccion-cierre {margin-top: 20px; border: 1px solid #ddd; border-radius: 2px; padding: 15px; background-color: #fafafa; display: none;}
.seccion-cierre.activo {display: block;}
.botones-cierre {display: flex; gap: 8px; margin-top: 12px; flex-wrap: wrap;}
.btn-aceptado {background: #4B732E; color: white; padding: 8px 12px; font-size: 10px;}
.btn-aceptado:hover {background: #385525;}
.btn-rechazado {background: #9B2E00; color: white; padding: 8px 12px; font-size: 10px;}
.btn-rechazado:hover {background: #7a2300;}
.btn-limpiar-cot {background: #556B2F; color: white; padding: 8px 12px; font-size: 10px;}
.btn-limpiar-cot:hover {background: #3f4f20;}
#modalAceptado {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10002; overflow: auto; padding: 10px;}
.modal-aceptado-content {background: white; max-width: 90%; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); max-height: 90vh; overflow-y: auto;}
.modal-aceptado-titulo {font-size: 16px; font-weight: 700; margin-bottom: 15px; text-transform: uppercase; color: #3B3B3B; border-bottom: 3px solid #4B732E; padding-bottom: 8px;}
.btn-cerrar-aceptado {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.botones-archivo {display: flex; gap: 6px; align-items: center; margin-bottom: 12px; flex-wrap: wrap;}
.btn-adjuntar {background: #F25C05; color: white; padding: 6px 10px; font-size: 10px;}
.btn-adjuntar:hover {background: #cb4a04;}
#inputArchivo {display: none;}
.archivo-info {font-size: 10px; color: #4B732E; font-weight: 600; text-transform: uppercase;}
.seccion-adjuntos {margin-top: 12px; padding: 12px; background-color: #e9f6ec; border-radius: 2px; border-left: 5px solid #4B732E;}
.seccion-adjuntos h4 {color: #4B732E; margin-bottom: 8px; font-weight: 700; text-transform: uppercase; font-size: 11px;}
.lista-adjuntos {list-style: none;}
.item-adjunto {display: flex; justify-content: space-between; align-items: center; padding: 6px; background: white; margin: 4px 0; border-radius: 2px; border-left: 3px solid #4B732E; flex-wrap: wrap; gap: 5px;}
.nombre-archivo {font-size: 10px; color: #3B3B3B; font-weight: 600; text-transform: uppercase; word-break: break-all;}
.btn-eliminar-archivo {background: #9B2E00; color: white; padding: 3px 5px; font-size: 8px;}
.btn-eliminar-archivo:hover {background: #7a2300;}
.botones-modal-aceptado {display: flex; gap: 6px; margin-top: 15px; justify-content: flex-end; flex-wrap: wrap;}
.btn-confirmar {background: #4B732E; color: white; padding: 6px 10px; font-size: 10px;}
.btn-confirmar:hover {background: #385525;}
.resumen-despacho {display: none; background-color: #e8f0e4; padding: 12px; border-radius: 2px; border-left: 5px solid #4B732E; margin-top: 15px;}
.resumen-despacho.activo {display: block;}
.resumen-despacho h4 {color: #3B3B3B; margin-bottom: 8px; text-transform: uppercase; font-weight: 700; font-size: 12px;}
.resumen-despacho p {margin: 3px 0; font-size: 10px; color: #4B732E; text-transform: uppercase;}
#modalArchivos {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10003; overflow: auto; padding: 10px;}
.modal-archivos-content {background: white; max-width: 90%; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15);}
.btn-cerrar-archivos {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.resumen-compra {display: none; background-color: #e3f2fd; padding: 12px; border-radius: 2px; border-left: 5px solid #1976d2; margin-top: 15px; overflow-x: auto;}
.resumen-compra.activo {display: block;}
.resumen-compra h4 {color: #1976d2; margin-bottom: 10px; text-transform: uppercase; font-weight: 700; font-size: 12px;}
.tabla-compra {width: 100%; border-collapse: collapse; font-size: 10px; background: white;}
.tabla-compra th {background: #1976d2; color: white; padding: 6px; text-transform: uppercase; font-weight: 700; text-align: left; font-size: 9px;}
.tabla-compra td {border: 1px solid #ccc; padding: 6px; text-transform: uppercase; color: #333; font-size: 9px;}
.tabla-compra tr:nth-child(even) {background: #f0f8ff;}
.tabla-compra a {color: #1976d2; text-decoration: none; font-weight: 600; font-size: 9px;}
.tabla-compra a:hover {text-decoration: underline;}
#modalVisualizarArchivo {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.5); z-index: 10004; overflow: auto; padding: 10px;}
.modal-archivo-content {background: white; max-width: 95%; max-height: 90vh; margin: 20px auto; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); overflow: auto;}
.btn-cerrar-archivo {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700; z-index: 1;}
.contenido-archivo {padding: 15px; text-align: center;}
.contenido-archivo img {max-width: 100%; max-height: 70vh; object-fit: contain;}
.contenido-archivo iframe {width: 100%; height: 70vh; border: none;}
.contenido-archivo embed {width: 100%; height: 70vh; border: none;}
.contenido-archivo pre {background: #f5f5f5; padding: 12px; border-radius: 2px; overflow-x: auto; text-align: left; font-size: 10px;}
.seccion-botones-pdf {margin-bottom: 15px; text-align: center;}
.badge-estado {display: inline-block; padding: 3px 6px; border-radius: 2px; font-size: 9px; font-weight: 700; text-transform: uppercase;}
.badge-aceptado {background: #4B732E; color: white;}
.badge-rechazado {background: #9B2E00; color: white;}
.badge-pendiente {background: #F25C05; color: white;}
#modalPDF {display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.95); z-index: 10005; overflow: hidden;}
#modalPDF.mostrar {display: flex; flex-direction: column;}
.pdf-header {background: #1F6F8B; color: white; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 4px 12px rgba(0,0,0,0.5); flex-shrink: 0; z-index: 100;}
.pdf-header h3 {margin: 0; font-size: 18px; font-weight: 700; letter-spacing: 1px;}
.btn-cerrar-pdf {background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 3px; cursor: pointer; padding: 10px 18px; font-weight: 700; transition: all 0.2s ease;}
.btn-cerrar-pdf:hover {background: #7a2300; transform: scale(1.05);}
.pdf-viewer {flex: 1; width: 100%; background: #525252; overflow: auto; display: flex; justify-content: center; align-items: flex-start; padding: 30px 20px;}
.pdf-container {width: 100%; max-width: 850px; background: white; box-shadow: 0 0 40px rgba(0,0,0,0.7);}
.pdf-container iframe {width: 100%; height: 1000px; border: none; display: block; background: white;}
.seccion-bloqueada {background-color: #fff3cd; padding: 12px; border-radius: 2px; border-left: 5px solid #FFA500; margin-bottom: 15px; display: none;}
.seccion-bloqueada.activa {display: block;}
.seccion-bloqueada h4 {color: #856404; text-transform: uppercase; font-weight: 700; margin-bottom: 4px; font-size: 12px;}
.seccion-bloqueada p {color: #856404; font-size: 10px; text-transform: uppercase;}
input[type="number"] {text-align: center;}
.margen-verde {color: #2E7D32; font-weight: 700;}
.margen-roja {color: #C62828; font-weight: 700;}
.tabla-articulos {width: 100%; border-collapse: collapse;}
.tabla-articulos th {background: #1F6F8B; color: white; padding: 8px; text-align: left; font-weight: 700; font-size: 10px; text-transform: uppercase;}
.tabla-articulos td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B; font-size: 10px;}
.tabla-articulos tr:nth-child(even) {background: #E9F0EA;}
.tabla-articulos a {color: #1976d2; text-decoration: none; font-weight: 600; font-size: 9px; text-transform: lowercase;}
.tabla-articulos a:hover {text-decoration: underline;}
.input-solo-lectura {background-color: #f0f0f0 !important; color: #666 !important;}
input#linkProducto {text-transform: lowercase;}
</style>
</head>
<body>
  <div class="cotizador-container">
    <header class="cotizador-header">
      <div class="empresa-nombre">EMPRESA CUNDO</div>
      <div class="numero-cotizacion">N° <span id="numeroCotizacion" style="color: white;">CO100500</span></div>
    </header>

    <div class="botones-superiores">
      <button class="btn btn-articulos" onclick="abrirArticulos()" id="btnArticulos">ARTÍCULOS</button>
      <button class="btn btn-buscar" onclick="mostrarCotizaciones()" id="btnCotizaciones">COTIZACIONES</button>
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
          <button class="btn-lupa" onclick="abrirModalClientes()" title="Ver todos los clientes">🔍</button>
        </div>
        <div class="busqueda-rut-botones">
          <button class="btn btn-buscar" onclick="buscarCliente()" id="btnBuscarCliente">BUSCAR</button>
          <button class="btn btn-limpiar" onclick="limpiarCotizacion()" id="btnLimpiarCliente">LIMPIAR</button>
        </div>
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
          <div class="campo-grupo"><label>COSTO (SIN IVA) *</label><input type="number" id="costoProducto" min="0" step="0.01" /></div>
          <div class="campo-grupo"><label>PRECIO CON IVA *</label><input type="number" id="precioConIvaProducto" min="0" step="0.01" /></div>
          <div class="campo-grupo"><label>PRECIO NETO (SIN IVA)</label><input type="number" id="precioNetoProducto" disabled class="input-solo-lectura" /></div>
        </div>
        <div class="fila-campos-producto-tres">
          <div class="campo-grupo"><label>% MARGEN NETO</label><input type="number" id="porcentajeProducto" disabled class="input-solo-lectura" /></div>
          <div class="campo-grupo"><label>UTILIDAD (SIN IVA)</label><input type="number" id="utilidadProducto" disabled class="input-solo-lectura" /></div>
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
      </div>
    </section>
  </div>

  <div id="modalArticulos">
    <div class="articulos-content">
      <button class="btn-cerrar-modal" onclick="cerrarArticulos()">×</button>
      <div class="modal-titulo">ARTÍCULOS (PRODUCTOS Y SERVICIOS REGISTRADOS)</div>
      <div id="tablaArticulosContenedor"></div>
    </div>
  </div>

  <div id="modalCotizaciones">
    <div>
      <button onclick="cerrarCotizaciones()" style="position:absolute;top:10px;right:15px;background:#9B2E00;color:white;border:none;border-radius:2px;padding:4px 6px;cursor:pointer;font-size:14px;font-weight:bold;">×</button>
      <h2 style="margin-bottom:12px;text-transform:uppercase;color:#3B3B3B;font-size:16px;">COTIZACIONES EMITIDAS</h2>
      <div id="listaCotizaciones" style="overflow-x:auto;"></div>
    </div>
  </div>

  <div id="modalClientes">
    <div class="modal-clientes-content">
      <button class="btn-cerrar-clientes" onclick="cerrarModalClientes()">×</button>
      <h2 style="margin-bottom:12px;text-transform:uppercase;color:#3B3B3B;font-size:16px;border-bottom:3px solid #4B732E;padding-bottom:8px;">SELECCIONAR CLIENTE</h2>
      <div id="listaClientesModal" style="overflow-x:auto;"></div>
    </div>
  </div>

  <div id="modalAceptado">
    <div class="modal-aceptado-content">
      <button class="btn-cerrar-aceptado" onclick="cerrarModalAceptado()">×</button>
      <h2 class="modal-aceptado-titulo">ACEPTACIÓN DE COTIZACIÓN</h2>
      <div class="campo-grupo"><label>TIPO DE ENTREGA *</label><select id="tipoEntrega"><option value="">SELECCIONE TIPO DE ENTREGA</option><option value="RETIRO">RETIRO</option><option value="DESPACHO RM">DESPACHO RM</option><option value="STARKEN">STARKEN</option><option value="CHILEXPRESS">CHILEXPRESS</option><option value="PDQ">PDQ</option><option value="BLUEXPRESS">BLUEXPRESS</option><option value="OTRO">OTRO</option></select></div>
      <div class="campo-grupo"><label>DIRECCIÓN *</label><input type="text" id="direccionDespacho" placeholder="INGRESE DIRECCIÓN" /></div>
      <div class="campo-grupo"><label>REGIÓN *</label><input type="text" id="regionSelect" placeholder="INGRESE REGIÓN" /></div>
      <div class="campo-grupo"><label>COMUNA *</label><input type="text" id="comunaInput" placeholder="INGRESE COMUNA" /></div>
      <div class="fila-campos">
        <div class="campo-grupo"><label>CONTACTO DE DESPACHO *</label><input type="text" id="contactoDespacho" placeholder="NOMBRE DE CONTACTO" /></div>
        <div class="campo-grupo"><label>CELULAR CONTACTO DESPACHO *</label><input type="tel" id="celularDespacho" placeholder="+56912345678" /></div>
      </div>
      <div class="campo-grupo"><label>ADJUNTO</label><div class="botones-archivo"><button class="btn-adjuntar" onclick="abrirSelectorArchivos()">📎 SELECCIONAR ARCHIVO</button><input type="file" id="inputArchivo" onchange="agregarArchivo(event)" /><span class="archivo-info" id="infoArchivo"></span></div><div id="adjuntosContainer" class="seccion-adjuntos" style="display: none;"><h4>ARCHIVOS ADJUNTOS</h4><ul class="lista-adjuntos" id="listaAdjuntos"></ul></div></div>
      <div class="botones-modal-aceptado">
        <button class="btn btn-cancelar" onclick="cerrarModalAceptado()">CANCELAR</button>
        <button class="btn-confirmar" onclick="confirmarAceptacion()">GUARDAR</button>
      </div>
    </div>
  </div>

  <div id="modalArchivos">
    <div class="modal-archivos-content">
      <button class="btn-cerrar-archivos" onclick="cerrarModalArchivos()">×</button>
      <h2 style="margin-bottom:12px;text-transform:uppercase;color:#3B3B3B;font-size:16px;border-bottom:3px solid #4B732E;padding-bottom:8px;">ARCHIVOS ADJUNTOS</h2>
      <div id="listaArchivosModal" style="overflow-x:auto;"></div>
    </div>
  </div>

  <div id="modalVisualizarArchivo">
    <div class="modal-archivo-content">
      <button class="btn-cerrar-archivo" onclick="cerrarModalVisualizarArchivo()">×</button>
      <div class="contenido-archivo" id="contenidoArchivo"></div>
    </div>
  </div>

  <div id="modalPDF">
    <div class="pdf-header">
      <h3>📄 COTIZACIÓN EN PDF</h3>
      <button class="btn-cerrar-pdf" onclick="cerrarModalPDF()">✕ CERRAR</button>
    </div>
    <div class="pdf-viewer">
      <div class="pdf-container" id="pdfContainer"></div>
    </div>
  </div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>

<script>
function calcularPrecioDesdeConIva() {
  const costoCompra = parseFloat(document.getElementById('costoProducto').value) || 0;
  const precioConIva = parseFloat(document.getElementById('precioConIvaProducto').value) || 0;
  
  if (costoCompra > 0 && precioConIva > 0) {
    const precioNeto = precioConIva / 1.19;
    const precioNetoRedondeado = Math.round(precioNeto * 100) / 100;
    const utilidad = Math.round((precioNetoRedondeado - costoCompra) * 100) / 100;
    const margen = precioNetoRedondeado > 0 ? Math.round((utilidad / precioNetoRedondeado) * 10000) / 100 : 0;
    
    document.getElementById('precioNetoProducto').value = precioNetoRedondeado.toFixed(2);
    document.getElementById('utilidadProducto').value = utilidad.toFixed(2);
    document.getElementById('porcentajeProducto').value = margen.toFixed(2);
  } else {
    document.getElementById('precioNetoProducto').value = '';
    document.getElementById('utilidadProducto').value = '';
    document.getElementById('porcentajeProducto').value = '';
  }
}

class GestorCotizaciones {
  constructor() { this.prefijo = 'CO'; this.numeroBase = 100500; this.cargarNumeroCotizacion(); }
  cargarNumeroCotizacion() { const n = localStorage.getItem('ultimaCotizacion'); if (n) this.numeroBase = parseInt(n); this.actualizarDisplay(); }
  obtenerNumeroActual() { return `${this.prefijo}${this.numeroBase}`; }
  siguienteCotizacion() { this.numeroBase++; localStorage.setItem('ultimaCotizacion', this.numeroBase); this.actualizarDisplay(); return this.obtenerNumeroActual(); }
  actualizarDisplay() { const el = document.getElementById('numeroCotizacion'); if (el) el.textContent = this.obtenerNumeroActual(); }
  establecerNumero(numero) { const numPuro = parseInt(numero.replace(this.prefijo, '')); this.numeroBase = numPuro; this.actualizarDisplay(); }
}

class GestorClientes {
  constructor() { this.clientes = this.cargarClientes(); }
  cargarClientes() { const c = localStorage.getItem('clientes'); return c ? JSON.parse(c) : {}; }
  guardarClientes() { localStorage.setItem('clientes', JSON.stringify(this.clientes)); }
  buscarPorRut(rut) { return this.clientes[rut] || null; }
  agregarCliente(rut, datos) { this.clientes[rut] = datos; this.guardarClientes(); }
  actualizarCliente(rut, datos) { this.clientes[rut] = datos; this.guardarClientes(); }
  obtenerTodos() { return this.clientes; }
}

class GestorProductos {
  constructor() { this.productos = this.cargarProductos(); }
  cargarProductos() { const p = localStorage.getItem('productos'); return p ? JSON.parse(p) : {}; }
  guardarProductos() { localStorage.setItem('productos', JSON.stringify(this.productos)); }
  buscarPorCodigo(codigo) { return this.productos[codigo] || null; }
  agregarProducto(codigo, datos) { this.productos[codigo] = datos; this.guardarProductos(); }
  actualizarProducto(codigo, datos) { this.productos[codigo] = datos; this.guardarProductos(); }
  obtenerTodos() { return this.productos; }
  buscar(termino) { termino = termino.toUpperCase().trim(); return Object.values(this.productos).filter(prod => prod.codigo.includes(termino) || prod.descripcion.includes(termino)).slice(0, 10); }
  eliminar(codigo) { delete this.productos[codigo]; this.guardarProductos(); }
}

const gestorCot = new GestorCotizaciones();
const gestorCli = new GestorClientes();
const gestorProd = new GestorProductos();

let cliente = null, modoEdicionCliente = false, productos = [], cotizaciones = JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]');
let estado = 'borrador', cotIndex = null, esLectura = false, archivosAdjuntos = [];

document.addEventListener('DOMContentLoaded', function() {
  document.getElementById('inputRut').addEventListener('input', function(e) {
    let v = e.target.value.replace(/[^0-9kK-]/g, '');
    if (v.length > 1 && v[v.length - 2] !== '-') v = v.slice(0, -1) + '-' + v.slice(-1);
    e.target.value = v.toUpperCase();
  });
  document.getElementById('inputRut').addEventListener('keydown', function(e) {if (e.key === 'Enter') buscarCliente();});
  document.getElementById('precioConIvaProducto').addEventListener('change', calcularPrecioDesdeConIva);
  document.getElementById('linkProducto').addEventListener('input', function(e) { e.target.value = e.target.value.toLowerCase(); });
});

function buscarCliente() {
  const rut = document.getElementById('inputRut').value.trim();
  if (!rut) return mostrarMensaje('INGRESE RUT', 'error');
  
  const cli = gestorCli.buscarPorRut(rut);
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
  modoEdicionCliente = true;
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
  document.getElementById('btnCancelarEdicion').style.display = 'inline-block';
}

function cancelarEdicion() {
  modoEdicionCliente = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarResumenCliente(cliente);
  document.getElementById('btnCancelarEdicion').style.display = 'none';
}

function guardarCliente() {
  const rut = document.getElementById('rut').value;
  const rs = document.getElementById('razonSocial').value.trim().toUpperCase();
  const gi = document.getElementById('giro').value.trim().toUpperCase();
  const di = document.getElementById('direccion').value.trim().toUpperCase();
  const nc = document.getElementById('nombreContacto').value.trim().toUpperCase();
  const ce = document.getElementById('celular').value.trim();
  const ma = document.getElementById('mail').value.trim().toUpperCase();
  const mp = document.getElementById('medioPago').value;
  
  if (!rs || !gi || !di || !nc || !ce || !ma || !mp) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  
  const cli = {rut, razonSocial: rs, giro: gi, direccion: di, nombreContacto: nc, celular: ce, mail: ma, medioPago: mp};
  if (modoEdicionCliente) gestorCli.actualizarCliente(rut, cli);
  else gestorCli.agregarCliente(rut, cli);
  
  cliente = cli;
  modoEdicionCliente = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarResumenCliente(cli);
  habilitarProductos();
  mostrarMensaje('CLIENTE GUARDADO ✓', 'exito');
}

function habilitarProductos() {
  if (esLectura) { document.getElementById('inputCodigoProducto').disabled = true; return; }
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
  cotIndex = null;
  esLectura = false;
  archivosAdjuntos = [];
  document.getElementById('seccionCierre').classList.remove('activo');
  document.getElementById('seccionBloqueada').classList.remove('activa');
  actualizarTablaProductos();
  document.getElementById('inputRut').focus();
}

function buscarProducto() {
  if (esLectura) return mostrarMensaje('MODO LECTURA', 'info');
  const cod = document.getElementById('inputCodigoProducto').value.trim().toUpperCase();
  if (!cod) return mostrarMensaje('INGRESE CÓDIGO', 'error');
  
  const prod = gestorProd.buscarPorCodigo(cod);
  if (prod) {
    agregarProducto(cod, prod);
    document.getElementById('inputCodigoProducto').value = '';
    mostrarMensaje('PRODUCTO ENCONTRADO ✓', 'exito');
    document.getElementById('formularioProducto').style.display = 'none';
  } else {
    mostrarMensaje('NO ENCONTRADO. CREE UNO NUEVO.', 'info');
    document.getElementById('formularioProducto').style.display = 'block';
    document.getElementById('codigoProducto').value = cod;
    document.getElementById('descripcionProducto').value = '';
    document.getElementById('costoProducto').value = '';
    document.getElementById('precioConIvaProducto').value = '';
    document.getElementById('precioNetoProducto').value = '';
    document.getElementById('porcentajeProducto').value = '';
    document.getElementById('utilidadProducto').value = '';
    document.getElementById('linkProducto').value = '';
    document.getElementById('costoProducto').focus();
  }
}

function guardarProducto() {
  const cod = document.getElementById('codigoProducto').value.trim().toUpperCase();
  const desc = document.getElementById('descripcionProducto').value.trim().toUpperCase();
  const cos = parseFloat(document.getElementById('costoProducto').value) || 0;
  const precioConIva = parseFloat(document.getElementById('precioConIvaProducto').value) || 0;
  const link = document.getElementById('linkProducto').value.trim().toLowerCase();
  
  if (!cod || !desc || cos <= 0 || precioConIva <= 0 || !link) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  
  const precioNeto = precioConIva / 1.19;
  const utilidad = precioNeto - cos;
  const margen = precioNeto > 0 ? (utilidad / precioNeto) * 100 : 0;
  
  if (precioNeto < cos) return mostrarMensaje('PRECIO NETO DEBE SER >= COSTO', 'error');
  
  const prod = {
    codigo: cod,
    descripcion: desc,
    costo: Math.round(cos * 100) / 100,
    precioConIva: Math.round(precioConIva * 100) / 100,
    precioNeto: Math.round(precioNeto * 100) / 100,
    utilidad: Math.round(utilidad * 100) / 100,
    margen: Math.round(margen * 100) / 100,
    link: link
  };
  
  gestorProd.agregarProducto(cod, prod);
  agregarProducto(cod, prod);
  cancelarProducto();
  mostrarMensaje('PRODUCTO GUARDADO ✓', 'exito');
}

function cancelarProducto() {
  document.getElementById('formularioProducto').style.display = 'none';
  document.getElementById('inputCodigoProducto').value = '';
}

function agregarProducto(cod, prod) {
  if (esLectura) return mostrarMensaje('MODO LECTURA', 'info');
  const ex = productos.find(p => p.codigo === cod);
  if (ex) {
    ex.cantidad++;
  } else {
    productos.push({
      codigo: cod,
      descripcion: prod.descripcion,
      cantidad: 1,
      costo: prod.costo,
      precioNeto: prod.precioNeto,
      precioConIva: prod.precioConIva,
      utilidad: prod.utilidad,
      margen: prod.margen,
      link: prod.link
    });
  }
  actualizarTablaProductos();
}

function actualizarTablaProductos() {
  const cont = document.getElementById('tablaProductosContenedor');
  if (productos.length === 0) {
    cont.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>';
    document.getElementById('resumenTotales').style.display = 'none';
    return;
  }
  
  let html = '<div style="overflow-x:auto;"><table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANT</th><th>COSTO UN</th><th>PRECIO NETO</th><th>PRECIO C/IVA</th><th>UTILIDAD</th><th>% MARGEN</th><th>TOTAL NETO</th><th>TOTAL C/IVA</th><th>ACCIÓN</th></tr></thead><tbody>';
  
  let totalNetoSinIva = 0, totalConIva = 0;
  
  productos.forEach((p, i) => {
    const bloqueado = esLectura;
    const totalNeto = Math.round(p.precioNeto * p.cantidad * 100) / 100;
    const totalConIva = Math.round(p.precioConIva * p.cantidad * 100) / 100;
    
    totalNetoSinIva += totalNeto;
    totalConIva += totalConIva;
    
    const claseMargen = p.margen >= 20 ? 'margen-verde' : 'margen-roja';
    
    html += `<tr>
      <td>${p.codigo}</td>
      <td>${p.descripcion}</td>
      <td class="texto-centrado"><input type="number" min="1" value="${p.cantidad}" onchange="cambiarCant(${i}, this.value)" ${bloqueado ? 'disabled' : ''} style="width:50px; text-align:center; padding:4px;"></td>
      <td class="valor-numerico">$${p.costo.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${p.precioNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${p.precioConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${p.utilidad.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico ${claseMargen}">${p.margen.toFixed(2)}%</td>
      <td class="valor-numerico">$${totalNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="texto-centrado"><button class="btn btn-eliminar" onclick="eliminarProd(${i})" ${bloqueado ? 'style="display:none;"' : ''}>DEL</button></td>
    </tr>`;
  });
  
  html += '</tbody></table></div>';
  cont.innerHTML = html;
  actualizarResumen();
}

function cambiarCant(i, cant) {
  cant = parseInt(cant);
  if (isNaN(cant) || cant < 1) { alert('CANTIDAD INVÁLIDA'); actualizarTablaProductos(); return; }
  productos[i].cantidad = cant;
  actualizarTablaProductos();
}

function eliminarProd(i) {
  if (esLectura) return;
  if (!confirm('¿Eliminar?')) return;
  productos.splice(i, 1);
  actualizarTablaProductos();
}

function actualizarResumen() {
  let totalNetoSinIva = 0, totalConIva = 0;
  
  productos.forEach(p => {
    const totalNeto = Math.round(p.precioNeto * p.cantidad * 100) / 100;
    const totalConIva = Math.round(p.precioConIva * p.cantidad * 100) / 100;
    totalNetoSinIva += totalNeto;
    totalConIva += totalConIva;
  });
  
  const iva = Math.round(totalNetoSinIva * 0.19 * 100) / 100;
  
  document.getElementById('totalNeto').textContent = '$' + totalNetoSinIva.toLocaleString('es-CL', {minimumFractionDigits: 2});
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
  
  let html = '<table class="tabla-articulos"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO</th><th>PRECIO NETO</th><th>PRECIO C/IVA</th><th>UTILIDAD</th><th>% MARGEN</th><th>LINK</th><th>ACCIONES</th></tr></thead><tbody>';
  
  claves.forEach(cod => {
    const art = todos[cod];
    html += `<tr>
      <td>${art.codigo}</td>
      <td>${art.descripcion}</td>
      <td class="valor-numerico">$${art.costo.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${art.precioNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${art.precioConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${art.utilidad.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">${art.margen.toFixed(2)}%</td>
      <td><a href="${art.link.startsWith('http') ? art.link : 'https://' + art.link}" target="_blank">VER</a></td>
      <td class="texto-centrado"><button class="btn btn-eliminar" onclick="eliminarArticulo('${art.codigo}')">DEL</button></td>
    </tr>`;
  });
  
  html += '</tbody></table>';
  document.getElementById('tablaArticulosContenedor').innerHTML = html;
}

function eliminarArticulo(codigo) {
  if (!confirm('¿Eliminar artículo?')) return;
  gestorProd.eliminar(codigo);
  listarArticulos();
  mostrarMensaje('ARTÍCULO ELIMINADO ✓', 'exito');
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones');
  const cont = document.getElementById('listaCotizaciones');
  
  if (cotizaciones.length === 0) {
    cont.innerHTML = '<p style="text-align:center; padding:15px;">SIN COTIZACIONES</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th>FECHA</th><th>ESTADO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    
    cotizaciones.forEach((c, index) => {
      const claseEstado = c.estado === 'aceptado' ? 'cot-aceptado' : (c.estado === 'rechazado' ? 'cot-rechazado' : (c.estado === 'enviada' ? 'cot-enviada' : 'cot-pendiente'));
      const fechaEmision = new Date(c.fecha);
      const fechaFormat = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
      const badgeClase = c.estado === 'aceptado' ? 'badge-aceptado' : (c.estado === 'rechazado' ? 'badge-rechazado' : 'badge-pendiente');
      
      const totalNetoSinIva = c.productos ? c.productos.reduce((a, p) => a + (p.precioNeto * p.cantidad), 0) : 0;
      
      html += `<tr class="${claseEstado}">
        <td>${c.numero}</td>
        <td>${c.razonSocial}</td>
        <td style="text-align:right;">$${Math.round(totalNetoSinIva).toLocaleString('es-CL')}</td>
        <td>${fechaFormat}</td>
        <td><span class="badge-estado ${badgeClase}">${c.estado.toUpperCase()}</span></td>
        <td style="text-align:center;">
          <button class="btn btn-ver" onclick="verCotizacion(${index})">VER</button>
          <button class="btn btn-eliminar-cot" onclick="eliminarCotizacion(${index})">DEL</button>
        </td>
      </tr>`;
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

function eliminarCotizacion(index) {
  if (index < 0 || index >= cotizaciones.length) return;
  if (!confirm('¿Eliminar cotización? Esta acción no se puede deshacer.')) return;
  cotizaciones.splice(index, 1);
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  mostrarMensaje('COTIZACIÓN ELIMINADA ✓', 'exito');
  mostrarCotizaciones();
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
    html = '<table class="tabla-clientes-modal"><thead><tr><th>RUT</th><th>RAZÓN SOCIAL</th><th>GIRO</th><th>CONTACTO</th><th>CELULAR</th><th>ACCIÓN</th></tr></thead><tbody>';
    clientes.forEach(([rut, cli]) => {
      html += `<tr><td>${rut}</td><td>${cli.razonSocial}</td><td>${cli.giro}</td><td>${cli.nombreContacto}</td><td>${cli.celular}</td><td><button class="btn-seleccionar-cliente" onclick="seleccionarCliente('${rut}')">SELECCIONAR</button></td></tr>`;
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

function generarPDF() {
  if (!cliente) { alert('INGRESE CLIENTE'); return; }
  if (productos.length === 0) { alert('AGREGUE PRODUCTOS'); return; }
  
  let numCot;
  if (cotIndex !== null) {
    numCot = cotizaciones[cotIndex].numero;
  } else {
    numCot = gestorCot.siguienteCotizacion();
  }
  
  const cot = {
    numero: numCot,
    razonSocial: cliente.razonSocial,
    rut: cliente.rut,
    giro: cliente.giro,
    direccion: cliente.direccion,
    nombreContacto: cliente.nombreContacto,
    celular: cliente.celular,
    mail: cliente.mail,
    medioPago: cliente.medioPago,
    productos: JSON.parse(JSON.stringify(productos)),
    fecha: new Date().toISOString(),
    estado: 'enviada'
  };
  
  if (cotIndex !== null) {
    cotizaciones[cotIndex] = cot;
  } else {
    cotizaciones.push(cot);
  }
  
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  estado = 'enviada';
  document.getElementById('seccionCierre').classList.add('activo');
  generarPDFDoc(cot);
  mostrarMensaje('PDF GENERADO ✓', 'exito');
}

function generarPDFDoc(cot) {
  try {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF('p', 'mm', 'a4');
    
    const totalNetoSinIva = cot.productos ? cot.productos.reduce((a, p) => a + (p.precioNeto * p.cantidad), 0) : 0;
    const iva = Math.round(totalNetoSinIva * 0.19 * 100) / 100;
    const totalConIva = Math.round((totalNetoSinIva + iva) * 100) / 100;
    
    const fechaEmision = new Date(cot.fecha);
    const fechaFormat = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
    
    // HEADER
    doc.setFillColor(31, 111, 139);
    doc.rect(0, 0, 210, 16, 'F');
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(13);
    doc.setFont(undefined, 'bold');
    doc.text('COTIZACIÓN', 15, 9);
    doc.setFontSize(8);
    doc.setFont(undefined, 'normal');
    doc.text(`${cot.numero} | ${fechaFormat}`, 195, 9, {align: 'right'});
    
    // DATOS CLIENTE
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(9);
    doc.setFont(undefined, 'bold');
    doc.text('DATOS DEL CLIENTE', 15, 25);
    doc.setDrawColor(242, 92, 5);
    doc.setLineWidth(0.2);
    doc.line(15, 27, 195, 27);
    
    doc.setFontSize(7);
    doc.setFont(undefined, 'normal');
    let yPos = 32;
    const datos = [
      ['RUT:', cot.rut, 'CONTACTO:', cot.nombreContacto],
      ['RAZÓN SOCIAL:', cot.razonSocial, 'CELULAR:', cot.celular],
      ['GIRO:', cot.giro, 'MAIL:', cot.mail],
      ['DIRECCIÓN:', cot.direccion, 'MEDIO PAGO:', cot.medioPago]
    ];
    
    datos.forEach(row => {
      doc.setFont(undefined, 'bold');
      doc.text(row[0], 15, yPos);
      doc.setFont(undefined, 'normal');
      doc.text(row[1], 33, yPos);
      doc.setFont(undefined, 'bold');
      doc.text(row[2], 110, yPos);
      doc.setFont(undefined, 'normal');
      doc.text(row[3], 135, yPos);
      yPos += 4;
    });
    
    yPos += 2;
    doc.setLineWidth(0.3);
    doc.line(15, yPos, 195, yPos);
    yPos += 5;
    
    // TABLA PRODUCTOS
    doc.setFontSize(7);
    doc.autoTable({
      startY: yPos,
      head: [['CÓDIGO', 'DESCRIPCIÓN', 'CANT', 'P. NETO', 'P. C/IVA', 'TOTAL NETO', 'TOTAL C/IVA']],
      body: cot.productos.map(p => [
        p.codigo,
        p.descripcion,
        p.cantidad.toString(),
        `$${p.precioNeto.toLocaleString('es-CL')}`,
        `$${p.precioConIva.toLocaleString('es-CL')}`,
        `$${Math.round(p.precioNeto * p.cantidad).toLocaleString('es-CL')}`,
        `$${Math.round(p.precioConIva * p.cantidad).toLocaleString('es-CL')}`
      ]),
      theme: 'striped',
      styles: {fontSize: 6.5, cellPadding: 1.5},
      headStyles: {fillColor: [31, 111, 139], textColor: 255, fontStyle: 'bold', fontSize: 7},
      columnStyles: {0: {cellWidth: 18}, 1: {cellWidth: 55}, 2: {cellWidth: 10}, 3: {cellWidth: 16}, 4: {cellWidth: 16}, 5: {cellWidth: 22}, 6: {cellWidth: 22}},
      margin: {left: 15, right: 15}
    });
    
    const resumenY = doc.lastAutoTable.finalY + 5;
    doc.setDrawColor(150, 150, 150);
    doc.setLineWidth(0.2);
    doc.line(118, resumenY, 195, resumenY);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(7);
    
    let lineY = resumenY;
    doc.text('NETO (SIN IVA):', 155, lineY + 3, {align: 'right'});
    doc.text(`$${Math.round(totalNetoSinIva).toLocaleString('es-CL')}`, 195, lineY + 3, {align: 'right'});
    doc.text('IVA 19%:', 155, lineY + 7, {align: 'right'});
    doc.text(`$${Math.round(iva).toLocaleString('es-CL')}`, 195, lineY + 7, {align: 'right'});
    
    doc.setFont(undefined, 'bold');
    doc.setFontSize(8);
    doc.text('TOTAL (CON IVA):', 155, lineY + 12, {align: 'right'});
    doc.text(`$${Math.round(totalConIva).toLocaleString('es-CL')}`, 195, lineY + 12, {align: 'right'});
    
    doc.setFontSize(6);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(100, 100, 100);
    const pageHeight = doc.internal.pageSize.height;
    doc.text('EMPRESA CUNDO | Los Pepinos 287, Las Condes | contacto@servicios.cl | Tel: 56 22 5510365', 15, pageHeight - 8);
    
    const pdfData = doc.output('datauristring');
    document.getElementById('pdfContainer').innerHTML = `<iframe src="${pdfData}" style="width:100%; height:100%; border:none;"></iframe>`;
    document.getElementById('modalPDF').classList.add('mostrar');
  } catch (error) {
    console.error('Error:', error);
    alert('Error al generar PDF');
  }
}

function cerrarModalPDF() {
  document.getElementById('modalPDF').classList.remove('mostrar');
}

function marcarAceptado() {
  if (!cliente || productos.length === 0) { alert('COMPLETE LA COTIZACIÓN'); return; }
  document.getElementById('modalAceptado').style.display = 'block';
}

function confirmarAceptacion() {
  const tipoEntrega = document.getElementById('tipoEntrega').value;
  const direccionDespacho = document.getElementById('direccionDespacho').value.trim().toUpperCase();
  const region = document.getElementById('regionSelect').value.trim().toUpperCase();
  const comuna = document.getElementById('comunaInput').value.trim().toUpperCase();
  const contactoDespacho = document.getElementById('contactoDespacho').value.trim().toUpperCase();
  const celularDespacho = document.getElementById('celularDespacho').value.trim();
  
  if (!tipoEntrega || !direccionDespacho || !region || !comuna || !contactoDespacho || !celularDespacho) {
    alert('COMPLETE TODOS LOS CAMPOS');
    return;
  }
  
  const cot = cotizaciones[cotIndex];
  cot.estado = 'aceptado';
  cot.despacho = {tipoEntrega, direccionDespacho, region, comuna, contactoDespacho, celularDespacho, archivos: archivosAdjuntos};
  
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  estado = 'aceptado';
  esLectura = true;
  document.getElementById('seccionBloqueada').classList.add('activa');
  habilitarProductos();
  cerrarModalAceptado();
  mostrarMensaje('✓ COTIZACIÓN ACEPTADA', 'exito');
}

function cerrarModalAceptado() {
  document.getElementById('modalAceptado').style.display = 'none';
  document.getElementById('tipoEntrega').value = '';
  document.getElementById('direccionDespacho').value = '';
  document.getElementById('regionSelect').value = '';
  document.getElementById('comunaInput').value = '';
  document.getElementById('contactoDespacho').value = '';
  document.getElementById('celularDespacho').value = '';
  archivosAdjuntos = [];
  document.getElementById('adjuntosContainer').style.display = 'none';
}

function marcarRechazado() {
  if (!cliente || productos.length === 0) { alert('COMPLETE LA COTIZACIÓN'); return; }
  if (!confirm('¿Marcar como rechazada?')) return;
  
  const cot = cotizaciones[cotIndex];
  cot.estado = 'rechazado';
  
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizaciones));
  estado = 'rechazado';
  esLectura = true;
  document.getElementById('seccionBloqueada').classList.add('activa');
  habilitarProductos();
  mostrarMensaje('✓ COTIZACIÓN RECHAZADA', 'info');
}

function abrirSelectorArchivos() {
  document.getElementById('inputArchivo').click();
}

function agregarArchivo(event) {
  const file = event.target.files[0];
  if (!file) return;
  
  const reader = new FileReader();
  reader.onload = function(e) {
    archivosAdjuntos.push({nombre: file.name, contenido: e.target.result, tipo: file.type});
    document.getElementById('adjuntosContainer').style.display = 'block';
    listarAdjuntos();
    document.getElementById('inputArchivo').value = '';
  };
  reader.readAsDataURL(file);
}

function listarAdjuntos() {
  const lista = document.getElementById('listaAdjuntos');
  lista.innerHTML = '';
  archivosAdjuntos.forEach((arch, i) => {
    const item = document.createElement('li');
    item.className = 'item-adjunto';
    item.innerHTML = `<span class="nombre-archivo">${arch.nombre}</span><button class="btn-eliminar-archivo" onclick="eliminarAdjunto(${i})">X</button>`;
    lista.appendChild(item);
  });
}

function eliminarAdjunto(index) {
  archivosAdjuntos.splice(index, 1);
  listarAdjuntos();
  if (archivosAdjuntos.length === 0) {
    document.getElementById('adjuntosContainer').style.display = 'none';
  }
}

function verArchivos() {
  if (cotIndex === null) return alert('SIN COTIZACIÓN CARGADA');
  const cot = cotizaciones[cotIndex];
  if (!cot.despacho || !cot.despacho.archivos || cot.despacho.archivos.length === 0) {
    alert('SIN ARCHIVOS ADJUNTOS');
    return;
  }
  
  document.getElementById('modalArchivos').style.display = 'block';
  const modal = document.getElementById('listaArchivosModal');
  let html = '';
  cot.despacho.archivos.forEach((arch, i) => {
    html += `<div style="padding:10px; border:1px solid #ddd; margin:5px; border-radius:2px; display:flex; justify-content:space-between; align-items:center;">
      <span style="font-weight:700; font-size:11px; text-transform:uppercase;">${arch.nombre}</span>
      <button class="btn btn-buscar" style="padding:4px 8px; font-size:9px;" onclick="verArchivo(${i})">VER</button>
    </div>`;
  });
  modal.innerHTML = html;
}

function verArchivo(index) {
  if (cotIndex === null) return;
  const arch = cotizaciones[cotIndex].despacho.archivos[index];
  const modal = document.getElementById('modalVisualizarArchivo');
  const contenido = document.getElementById('contenidoArchivo');
  
  if (arch.tipo.includes('image')) {
    contenido.innerHTML = `<img src="${arch.contenido}" />`;
  } else if (arch.tipo.includes('pdf')) {
    contenido.innerHTML = `<iframe src="${arch.contenido}"></iframe>`;
  } else {
    contenido.innerHTML = `<pre>${arch.contenido}</pre>`;
  }
  
  modal.style.display = 'block';
}

function cerrarModalArchivos() {
  document.getElementById('modalArchivos').style.display = 'none';
}

function cerrarModalVisualizarArchivo() {
  document.getElementById('modalVisualizarArchivo').style.display = 'none';
}

function mostrarMensaje(texto, tipo) {
  const m = document.getElementById('mensaje');
  m.className = `mensaje mensaje-${tipo}`;
  m.textContent = texto;
  setTimeout(() => m.textContent = '', 4000);
}

window.addEventListener('click', function(e) {
  if (e.target.id === 'modalArticulos' && e.target === e.currentTarget) cerrarArticulos();
  if (e.target.id === 'modalCotizaciones' && e.target === e.currentTarget) cerrarCotizaciones();
  if (e.target.id === 'modalClientes' && e.target === e.currentTarget) cerrarModalClientes();
  if (e.target.id === 'modalAceptado' && e.target === e.currentTarget) cerrarModalAceptado();
  if (e.target.id === 'modalArchivos' && e.target === e.currentTarget) cerrarModalArchivos();
  if (e.target.id === 'modalVisualizarArchivo' && e.target === e.currentTarget) cerrarModalVisualizarArchivo();
});
</script>
</body>
</html>
