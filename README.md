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
.header-botones {display: flex; gap: 6px; flex-wrap: wrap; align-items: center;}
.btn-config {background: #D9822B; color: white; padding: 6px 10px; font-size: 9px; border: none; cursor: pointer; border-radius: 2px; font-weight: 700; transition: all 0.3s;}
.btn-config:hover {background: #b36e1e;}
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
.btn-editar-cot {background: #D9822B; color: white; padding: 4px 5px; font-size: 8px;}
.btn-editar-cot:hover {background: #b36e1e;}
.btn-ver-archivo {background: #4B732E; color: white; padding: 4px 5px; font-size: 8px;}
.btn-ver-archivo:hover {background: #385525;}
.btn-exportar {background: #1976d2; color: white; padding: 8px 12px; font-size: 10px;}
.btn-exportar:hover {background: #1565c0;}
.btn-importar {background: #388e3c; color: white; padding: 8px 12px; font-size: 10px;}
.btn-importar:hover {background: #2e7d32;}
.btn-reportes {background: #7b1fa2; color: white; padding: 8px 12px; font-size: 10px;}
.btn-reportes:hover {background: #6a1b9a;}
.formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none;}
.formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
.campo-grupo {margin-bottom: 12px;}
.campo-grupo label {display: block; font-weight: 700; color: #3B3B3B; margin-bottom: 4px; font-size: 11px; text-transform: uppercase;}
.campo-grupo input, .campo-grupo select, .campo-grupo textarea {width: 100%; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase; font-family: inherit;}
.campo-grupo input:focus, .campo-grupo select:focus, .campo-grupo textarea:focus {outline: none; border-color: #F25C05;}
.campo-grupo input:disabled, .campo-grupo select:disabled, .campo-grupo textarea:disabled {background-color: #F5F5F5; cursor: not-allowed; color: #777;}
.campo-grupo input.error {border-color: #9B2E00; background-color: #ffebee;}
.campo-grupo input.valido {border-color: #4B732E; background-color: #e8f5e9;}
.campo-grupo .validacion-msg {font-size: 9px; margin-top: 3px; display: none; font-weight: 600;}
.campo-grupo .validacion-msg.error {color: #9B2E00; display: block;}
.campo-grupo .validacion-msg.exito {color: #4B732E; display: block;}
.fila-campos {display: grid; grid-template-columns: 1fr 1fr; gap: 12px;}
.fila-campos-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px;}
.fila-campos-dos {display: grid; grid-template-columns: 1fr 1fr; gap: 12px;}
@media (max-width: 768px) {
  .fila-campos, .fila-campos-tres {grid-template-columns: 1fr;}
  .cotizador-header {padding: 8px 12px; flex-direction: column;}
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
.mensaje-bloqueado {background-color: #fff3cd; color: #856404; border: 1px solid #ffeaa7;}
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
.desplegable-clientes {display: none; position: absolute; top: 100%; left: 0; right: 70px; background: white; border: 1px solid #ddd; border-top: none; max-height: 200px; overflow-y: auto; z-index: 1001; border-radius: 0 0 2px 2px; box-shadow: 0 4px 8px rgba(0,0,0,0.07);}
.desplegable-clientes.activo {display: block;}
.item-cliente {padding: 8px; cursor: pointer; background: white; font-size: 10px; text-transform: uppercase; border-bottom: 1px solid #eee; transition: background 0.2s;}
.item-cliente:hover {background: #e8f5e9; border-left: 3px solid #4B732E;}
.formulario-producto {background: linear-gradient(135deg, #f9ead4 0%, #f7dba1 100%); padding: 12px; border-radius: 2px; border-left: 5px solid #F25C05; margin-bottom: 15px;}
.formulario-producto h4 {text-transform: uppercase; color: #b35304; margin-bottom: 12px; font-weight: 700; font-size: 12px;}
.fila-campos-producto {display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;}
.fila-campos-producto-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 12px;}
.botones-producto {display: flex; gap: 6px; margin-top: 8px; flex-wrap: wrap;}
.alerta-sin-productos {background-color: #e9f6ec; padding: 12px; border-radius: 2px; border-left: 5px solid #4B732E; text-align: center; color: #4B732E; text-transform: uppercase; font-weight: 700; font-size: 11px;}
.descuento-global {background: #e3f2fd; padding: 12px; border-radius: 2px; border-left: 5px solid #1976d2; margin-bottom: 15px;}
.descuento-global h4 {color: #1976d2; margin-bottom: 8px; font-weight: 700; text-transform: uppercase; font-size: 12px;}
.descuento-global p {color: #1976d2; font-size: 10px; margin: 3px 0;}
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
.tabla-cotizaciones tr.cot-aceptado {background-color: #C8E6C9 !important;}
.tabla-cotizaciones tr.cot-aceptado td {background-color: #C8E6C9 !important; color: #2E7D32;}
.tabla-cotizaciones tr.cot-rechazado {background-color: #FFCDD2 !important;}
.tabla-cotizaciones tr.cot-rechazado td {background-color: #FFCDD2 !important; color: #C62828;}
.tabla-cotizaciones tr.cot-pendiente {background-color: #FFF9C4 !important;}
.tabla-cotizaciones tr.cot-pendiente td {background-color: #FFF9C4 !important; color: #F57F17;}
.tabla-clientes {width: 100%; border-collapse: collapse; background: white;}
.tabla-clientes th {background: #1F6F8B; color: white; padding: 8px; text-align: left; text-transform: uppercase; font-weight: 700; font-size: 10px;}
.tabla-clientes td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B; font-size: 10px;}
.tabla-clientes tr:nth-child(even) {background: #E9F0EA;}
.seccion-cierre {margin-top: 20px; border: 1px solid #ddd; border-radius: 2px; padding: 15px; background-color: #fafafa; display: none;}
.seccion-cierre.activo {display: block;}
.botones-cierre {display: flex; gap: 8px; margin-top: 12px; flex-wrap: wrap;}
.btn-aceptado {background: #4B732E; color: white; padding: 8px 12px; font-size: 10px;}
.btn-aceptado:hover {background: #385525;}
.btn-aceptado:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-rechazado {background: #9B2E00; color: white; padding: 8px 12px; font-size: 10px;}
.btn-rechazado:hover {background: #7a2300;}
.btn-rechazado:disabled {background: #a0a0a0; cursor: not-allowed;}
.btn-limpiar-cot {background: #556B2F; color: white; padding: 8px 12px; font-size: 10px;}
.btn-limpiar-cot:hover {background: #3f4f20;}
.btn-archivo {background: #D9822B; color: white; padding: 8px 12px; font-size: 10px; display: none;}
.btn-archivo:hover {background: #b36e1e;}
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
.buscador-articulos {display: flex; gap: 6px; margin-bottom: 12px; align-items: center;}
.buscador-articulos input {flex: 1; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase;}
.buscador-articulos button {background: #4B732E; color: white;}
.buscador-articulos button:hover {background: #385525;}
#modalConfig {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 9998; overflow: auto; padding: 10px;}
.modal-config-content {background: white; max-width: 600px; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15);}
.modal-config-titulo {font-size: 16px; font-weight: 700; margin-bottom: 15px; text-transform: uppercase; color: #3B3B3B; border-bottom: 3px solid #D9822B; padding-bottom: 8px;}
.btn-cerrar-config {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.botones-config {display: flex; gap: 6px; margin-top: 15px; justify-content: flex-end; flex-wrap: wrap;}
#modalReportes {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 10006; overflow: auto; padding: 10px;}
.modal-reportes-content {background: white; max-width: 95%; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15); max-height: 90vh; overflow-y: auto;}
.btn-cerrar-reportes {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.tabla-reportes {width: 100%; border-collapse: collapse; background: white;}
.tabla-reportes th {background: #7b1fa2; color: white; padding: 8px; text-align: left; text-transform: uppercase; font-weight: 700; font-size: 10px;}
.tabla-reportes td {border: 1px solid #ddd; padding: 8px; text-transform: uppercase; color: #3B3B3B; font-size: 10px;}
.tabla-reportes tr:nth-child(even) {background: #f3e5f5;}
.fila-total-reportes {background: #7b1fa2 !important; color: white !important; font-weight: 700;}
.fila-total-reportes td {background: #7b1fa2; color: white; font-weight: 700;}
#modalBackup {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.4); z-index: 9997; overflow: auto; padding: 10px;}
.modal-backup-content {background: white; max-width: 500px; margin: 20px auto; padding: 15px; border-radius: 2px; position: relative; box-shadow: 0 8px 16px rgba(0,0,0,0.15);}
.modal-backup-titulo {font-size: 16px; font-weight: 700; margin-bottom: 15px; text-transform: uppercase; color: #3B3B3B; border-bottom: 3px solid #388e3c; padding-bottom: 8px;}
.btn-cerrar-backup {position: absolute; top: 10px; right: 15px; background: #9B2E00; color: white; font-size: 14px; border: none; border-radius: 2px; cursor: pointer; padding: 4px 6px; font-weight: 700;}
.botones-backup {display: flex; gap: 6px; margin-top: 15px; flex-wrap: wrap;}
#inputImportar {display: none;}
</style>
</head>
<body>
  <div class="cotizador-container">
    <header class="cotizador-header">
      <div class="empresa-nombre" id="empresaNombre">EMPRESA CUNDO</div>
      <div class="numero-cotizacion">N° <span id="numeroCotizacion" style="color: white;">CO100500</span></div>
      <div class="header-botones">
        <button class="btn-config" onclick="abrirConfiguracion()" title="Configurar empresa">⚙️ CONFIG</button>
      </div>
    </header>

    <div class="botones-superiores">
      <button class="btn btn-articulos" onclick="abrirArticulos()" id="btnArticulos">ARTÍCULOS</button>
      <button class="btn btn-reportes" onclick="abrirReportes()" id="btnReportes">📊 REPORTES</button>
      <button class="btn btn-buscar" onclick="mostrarCotizaciones()" id="btnCotizaciones">COTIZACIONES</button>
      <button class="btn btn-exportar" onclick="abrirBackup()" id="btnBackup">💾 BACKUP</button>
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
          <div id="desplegableClientes" class="desplegable-clientes"></div>
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
          <div class="campo-grupo">
            <label>NOMBRE DE CONTACTO *</label>
            <input type="text" id="nombreContacto" />
          </div>
          <div class="campo-grupo">
            <label>CELULAR *</label>
            <input type="tel" id="celular" placeholder="+56912345678" onchange="validarTelefono(this)" />
            <div class="validacion-msg"></div>
          </div>
        </div>
        <div class="campo-grupo">
          <label>MAIL *</label>
          <input type="email" id="mail" onchange="validarEmail(this)" />
          <div class="validacion-msg"></div>
        </div>
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
          <div class="campo-grupo"><label>COSTO (SIN IVA) *</label><input type="number" id="costoProducto" min="0" step="0.01" /></div>
          <div class="campo-grupo"><label>PRECIO NETO (SIN IVA) *</label><input type="number" id="valorTotalProducto" min="0" step="0.01" /></div>
          <div class="campo-grupo"><label>% MARGEN SOBRE NETO</label><input type="number" id="porcentajeProducto" disabled class="input-solo-lectura" /></div>
        </div>
        <div class="campo-grupo"><label>LINK *</label><input type="text" id="linkProducto" /></div>
        <div class="botones-producto">
          <button class="btn btn-agregar" onclick="guardarProducto()">GUARDAR PRODUCTO</button>
          <button class="btn btn-cancelar" onclick="cancelarProducto()">CANCELAR</button>
        </div>
      </div>

      <div id="descuentoGlobal" class="descuento-global" style="display:none;">
        <h4>💰 DESCUENTO GLOBAL</h4>
        <div class="fila-campos-dos">
          <div class="campo-grupo">
            <label>TIPO DESCUENTO</label>
            <select id="tipoDescuento" onchange="actualizarTablaProductos()">
              <option value="porcentaje">PORCENTAJE (%)</option>
              <option value="monto">MONTO ($)</option>
            </select>
          </div>
          <div class="campo-grupo">
            <label>VALOR DESCUENTO</label>
            <input type="number" id="valorDescuento" min="0" step="0.01" placeholder="0" onchange="actualizarTablaProductos()" />
          </div>
        </div>
        <p id="textoDescuento"></p>
      </div>

      <div id="tablaProductosContenedor">
        <div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>
      </div>
      <div class="resumen-totales" id="resumenTotales" style="display:none;">
        <div class="resumen-linea"><div>NETO</div><div id="totalNeto">$0.00</div></div>
        <div class="resumen-linea"><div>DESCUENTO</div><div id="totalDescuento">$0.00</div></div>
        <div class="resumen-linea"><div>NETO FINAL</div><div id="totalNetoFinal">$0.00</div></div>
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
        <button class="btn-archivo" onclick="verArchivos()" id="btnVerArchivos">ARCHIVOS</button>
      </div>
    </section>
  </div>

  <!-- MODALES -->
  <div id="modalConfig">
    <div class="modal-config-content">
      <button class="btn-cerrar-config" onclick="cerrarConfiguracion()">×</button>
      <div class="modal-config-titulo">⚙️ CONFIGURACIÓN DE EMPRESA</div>
      <div class="campo-grupo"><label>NOMBRE DE EMPRESA *</label><input type="text" id="configNombreEmpresa" placeholder="Ej: Empresa Servicios SPA" /></div>
      <div class="campo-grupo"><label>RUT EMPRESA *</label><input type="text" id="configRutEmpresa" placeholder="Ej: 78000256-0" /></div>
      <div class="campo-grupo"><label>DIRECCIÓN *</label><input type="text" id="configDireccion" placeholder="Ej: Los Pepinos 287, Las Condes" /></div>
      <div class="campo-grupo"><label>TELÉFONO *</label><input type="text" id="configTelefono" placeholder="Ej: 56 22 5510365" /></div>
      <div class="campo-grupo"><label>EMAIL *</label><input type="email" id="configEmail" placeholder="contacto@empresa.cl" /></div>
      <div class="botones-config">
        <button class="btn btn-cancelar" onclick="cerrarConfiguracion()">CANCELAR</button>
        <button class="btn btn-agregar" onclick="guardarConfiguracion()">GUARDAR</button>
      </div>
    </div>
  </div>

  <div id="modalArticulos">
    <div class="articulos-content">
      <button class="btn-cerrar-modal" onclick="cerrarArticulos()">×</button>
      <div class="modal-titulo">ARTÍCULOS (PRODUCTOS Y SERVICIOS REGISTRADOS)</div>
      
      <div class="buscador-articulos">
        <input type="text" id="inputBuscador" placeholder="BUSCAR CÓDIGO O DESCRIPCIÓN" />
        <button class="btn" onclick="filtrarArticulos()">🔍 BUSCAR</button>
        <button class="btn btn-limpiar" onclick="limpiarBuscadorArticulos()">🗑️ LIMPIAR</button>
      </div>

      <div id="tablaArticulosContenedor"></div>
      <div id="formularioEditarArticulo" class="formulario-editar-articulo" style="display: none;"></div>
    </div>
  </div>

  <div id="modalCotizaciones">
    <div>
      <button onclick="cerrarCotizaciones()" style="position:absolute;top:10px;right:15px;background:#9B2E00;color:white;border:none;border-radius:2px;padding:4px 6px;cursor:pointer;font-size:14px;font-weight:bold;">×</button>
      <h2 style="margin-bottom:12px;text-transform:uppercase;color:#3B3B3B;font-size:16px;">COTIZACIONES EMITIDAS</h2>
      <div id="listaCotizaciones" style="overflow-x:auto;"></div>
    </div>
  </div>

  <div id="modalReportes">
    <div class="modal-reportes-content">
      <button class="btn-cerrar-reportes" onclick="cerrarReportes()">×</button>
      <h2 style="margin-bottom:15px;text-transform:uppercase;color:#3B3B3B;font-size:16px;border-bottom:3px solid #7b1fa2;padding-bottom:8px;">📊 REPORTES - UTILIDAD VS INGRESOS</h2>
      <div id="contenidoReportes" style="overflow-x:auto;"></div>
    </div>
  </div>

  <div id="modalBackup">
    <div class="modal-backup-content">
      <button class="btn-cerrar-backup" onclick="cerrarBackup()">×</button>
      <div class="modal-backup-titulo">💾 RESPALDO Y RESTAURACIÓN DE DATOS</div>
      <p style="font-size:11px; color:#666; margin-bottom:12px; text-transform:uppercase;">Descargue un respaldo de todos sus datos (clientes, productos, cotizaciones) o restaure desde un backup anterior.</p>
      <div class="botones-backup">
        <button class="btn btn-exportar" onclick="exportarDatos()">⬇️ DESCARGAR RESPALDO</button>
        <button class="btn btn-importar" onclick="abrirSelectorArchivoBak()">⬆️ RESTAURAR RESPALDO</button>
        <input type="file" id="inputImportar" accept=".json" onchange="importarDatos(event)" style="display:none;" />
      </div>
      <div id="mensajeBackup" style="margin-top:12px;"></div>
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
// ================== CONFIGURACIÓN DE EMPRESA (MEJORA 1) ==================
const configEmpresaDefault = {
  nombre: 'EMPRESA CUNDO',
  rut: '78000256-0',
  direccion: 'Los Pepinos 287, Las Condes',
  telefono: '56 22 5510365',
  email: 'contacto@servicios.cl'
};

function cargarConfiguracion() {
  const config = JSON.parse(localStorage.getItem('configEmpresa')) || configEmpresaDefault;
  document.getElementById('empresaNombre').textContent = config.nombre;
  return config;
}

function abrirConfiguracion() {
  const config = JSON.parse(localStorage.getItem('configEmpresa')) || configEmpresaDefault;
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
    rut: document.getElementById('configRutEmpresa').value || '78000256-0',
    direccion: document.getElementById('configDireccion').value.toUpperCase() || 'Los Pepinos 287, Las Condes',
    telefono: document.getElementById('configTelefono').value || '56 22 5510365',
    email: document.getElementById('configEmail').value || 'contacto@servicios.cl'
  };
  localStorage.setItem('configEmpresa', JSON.stringify(config));
  document.getElementById('empresaNombre').textContent = config.nombre;
  mostrarMensaje('CONFIGURACIÓN GUARDADA ✓', 'exito');
  cerrarConfiguracion();
}

// ================== VALIDACIÓN AVANZADA (MEJORA 5) ==================
function validarTelefono(input) {
  const valor = input.value.trim();
  const regexTelefono = /^(\+56|56)?[\s]?9?[\s]?\d{8}$/;
  const msgDiv = input.nextElementSibling;
  
  if (!valor) {
    input.classList.remove('error', 'valido');
    msgDiv.textContent = '';
    msgDiv.className = 'validacion-msg';
    return;
  }
  
  if (regexTelefono.test(valor)) {
    input.classList.remove('error');
    input.classList.add('valido');
    msgDiv.textContent = '✓ Teléfono válido';
    msgDiv.className = 'validacion-msg exito';
  } else {
    input.classList.remove('valido');
    input.classList.add('error');
    msgDiv.textContent = '❌ Formato: +56912345678 o 912345678';
    msgDiv.className = 'validacion-msg error';
  }
}

function validarEmail(input) {
  const valor = input.value.trim();
  const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const msgDiv = input.nextElementSibling;
  
  if (!valor) {
    input.classList.remove('error', 'valido');
    msgDiv.textContent = '';
    msgDiv.className = 'validacion-msg';
    return;
  }
  
  if (regexEmail.test(valor)) {
    input.classList.remove('error');
    input.classList.add('valido');
    msgDiv.textContent = '✓ Email válido';
    msgDiv.className = 'validacion-msg exito';
  } else {
    input.classList.remove('valido');
    input.classList.add('error');
    msgDiv.textContent = '❌ Email inválido';
    msgDiv.className = 'validacion-msg error';
  }
}

function validarRut(rut) {
  if (!/^[0-9]{7,8}-[0-9kK]$/.test(rut)) return false;
  const [numeros, digito] = rut.split('-');
  const suma = [...numeros].reverse().reduce((acc, n, i) => acc + parseInt(n) * ((i % 6) + 2), 0);
  const dvCalculado = (11 - (suma % 11)) % 11;
  const dvEsperado = digito.toUpperCase() === 'K' ? 10 : parseInt(digito);
  return dvCalculado === dvEsperado;
}

function formatearNumero(valor) {
  return Math.round(parseFloat(valor) * 100) / 100;
}

// ================== DESCUENTOS GLOBALES (MEJORA 2) ==================
let descuentoGlobal = {tipo: 'porcentaje', valor: 0};

function mostrarSeccionDescuento() {
  document.getElementById('descuentoGlobal').style.display = productosEnCotizacion.length > 0 ? 'block' : 'none';
}

function obtenerMontoDescuento() {
  if (descuentoGlobal.valor === 0) return 0;
  const totalNeto = productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0);
  if (descuentoGlobal.tipo === 'porcentaje') {
    return formatearNumero(totalNeto * (descuentoGlobal.valor / 100));
  } else {
    return formatearNumero(descuentoGlobal.valor);
  }
}

// ================== EXPORTAR/IMPORTAR DATOS (MEJORA 4) ==================
function exportarDatos() {
  const datos = {
    clientes: JSON.parse(localStorage.getItem('clientes') || '{}'),
    productos: JSON.parse(localStorage.getItem('productos') || '{}'),
    cotizaciones: JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]'),
    configuracion: JSON.parse(localStorage.getItem('configEmpresa') || JSON.stringify(configEmpresaDefault)),
    fecha_respaldo: new Date().toISOString()
  };
  const json = JSON.stringify(datos, null, 2);
  const blob = new Blob([json], {type: 'application/json'});
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `respaldo_cotizador_${new Date().toISOString().split('T')[0]}.json`;
  link.click();
  URL.revokeObjectURL(url);
  mostrarMensajeBackup('✓ RESPALDO DESCARGADO CORRECTAMENTE', 'exito');
}

function abrirSelectorArchivoBak() {
  document.getElementById('inputImportar').click();
}

function importarDatos(event) {
  const file = event.target.files[0];
  if (!file) return;
  
  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const datos = JSON.parse(e.target.result);
      
      if (!datos.clientes || !datos.productos || !datos.cotizaciones) {
        mostrarMensajeBackup('❌ ARCHIVO INVÁLIDO - Estructura incorrecta', 'error');
        return;
      }
      
      localStorage.setItem('clientes', JSON.stringify(datos.clientes));
      localStorage.setItem('productos', JSON.stringify(datos.productos));
      localStorage.setItem('cotizacionesEmitidas', JSON.stringify(datos.cotizaciones));
      if (datos.configuracion) {
        localStorage.setItem('configEmpresa', JSON.stringify(datos.configuracion));
        cargarConfiguracion();
      }
      
      mostrarMensajeBackup(`✓ RESPALDO RESTAURADO - ${Object.keys(datos.clientes).length} clientes, ${Object.keys(datos.productos).length} productos, ${datos.cotizaciones.length} cotizaciones`, 'exito');
      setTimeout(() => { location.reload(); }, 2000);
    } catch (error) {
      mostrarMensajeBackup('❌ ERROR AL LEER EL ARCHIVO: ' + error.message, 'error');
    }
  };
  reader.readAsText(file);
}

function mostrarMensajeBackup(texto, tipo) {
  const m = document.getElementById('mensajeBackup');
  m.className = `mensaje mensaje-${tipo}`;
  m.textContent = texto;
}

function abrirBackup() {
  document.getElementById('modalBackup').style.display = 'block';
}

function cerrarBackup() {
  document.getElementById('modalBackup').style.display = 'none';
}

// ================== REPORTES (MEJORA 3) ==================
function abrirReportes() {
  generarReportes();
  document.getElementById('modalReportes').style.display = 'block';
}

function cerrarReportes() {
  document.getElementById('modalReportes').style.display = 'none';
}

function generarReportes() {
  if (cotizacionesEmitidas.length === 0) {
    document.getElementById('contenidoReportes').innerHTML = '<p style="text-align:center; padding:15px; font-size:11px; text-transform:uppercase;">NO HAY COTIZACIONES PARA REPORTAR</p>';
    return;
  }

  let totalIngresos = 0, totalCostos = 0, totalUtilidad = 0, html = '<div style="overflow-x:auto;"><table class="tabla-reportes"><thead><tr><th>N° COTIZACIÓN</th><th>CLIENTE</th><th>INGRESOS NETO</th><th>COSTO TOTAL</th><th>UTILIDAD NETA</th><th>% MARGEN</th><th>ESTADO</th></tr></thead><tbody>';
  
  cotizacionesEmitidas.forEach(cot => {
    let costoTotal = 0, ingresoNeto = formatearNumero(parseFloat(cot.totalNeto));
    if (cot.productos) {
      costoTotal = formatearNumero(cot.productos.reduce((acc, p) => acc + (parseFloat(p.costo) * p.cantidad), 0));
    }
    const utilidad = formatearNumero(ingresoNeto - costoTotal);
    const margen = ingresoNeto > 0 ? formatearNumero((utilidad / ingresoNeto) * 100) : 0;
    
    totalIngresos += ingresoNeto;
    totalCostos += costoTotal;
    totalUtilidad += utilidad;
    
    const badgeClase = cot.estado === 'aceptado' ? 'badge-aceptado' : (cot.estado === 'rechazado' ? 'badge-rechazado' : 'badge-pendiente');
    html += `<tr><td>${cot.numero}</td><td>${cot.razonSocial}</td><td class="valor-numerico">$${ingresoNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${costoTotal.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">$${utilidad.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td class="valor-numerico">${margen.toFixed(2)}%</td><td><span class="badge-estado ${badgeClase}">${cot.estado}</span></td></tr>`;
  });

  const margenTotal = totalIngresos > 0 ? formatearNumero((totalUtilidad / totalIngresos) * 100) : 0;
  html += `<tr class="fila-total-reportes"><td colspan="2"><strong>TOTALES</strong></td><td class="valor-numerico"><strong>$${formatearNumero(totalIngresos).toLocaleString('es-CL', {minimumFractionDigits: 2})}</strong></td><td class="valor-numerico"><strong>$${formatearNumero(totalCostos).toLocaleString('es-CL', {minimumFractionDigits: 2})}</strong></td><td class="valor-numerico"><strong>$${formatearNumero(totalUtilidad).toLocaleString('es-CL', {minimumFractionDigits: 2})}</strong></td><td class="valor-numerico"><strong>${margenTotal.toFixed(2)}%</strong></td><td></td></tr>`;
  html += '</tbody></table></div>';
  
  document.getElementById('contenidoReportes').innerHTML = html;
}

// ================== RESTO DEL CÓDIGO (IGUAL AL ANTERIOR) ==================
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
  buscarPorCoincidencia(termino) { termino = termino.toUpperCase().trim(); const todos = this.obtenerTodos(); return Object.entries(todos).filter(([rut, cliente]) => rut.includes(termino) || cliente.razonSocial.includes(termino)).map(([rut, cliente]) => ({ rut, ...cliente })).slice(0, 10); }
}

class GestorProductos {
  constructor() { this.productos = this.cargarProductos(); }
  cargarProductos() { const p = localStorage.getItem('productos'); return p ? JSON.parse(p) : {}; }
  guardarProductos() { localStorage.setItem('productos', JSON.stringify(this.productos)); }
  buscarPorCodigo(codigo) { return this.productos[codigo] || null; }
  agregarProducto(codigo, datos) { this.productos[codigo] = datos; this.guardarProductos(); }
  actualizarProducto(codigo, datos) { this.productos[codigo] = datos; this.guardarProductos(); }
  obtenerTodos() { return this.productos; }
  eliminarProducto(codigo) { delete this.productos[codigo]; this.guardarProductos(); }
  buscarPorCodigoODescripcion(termino) { termino = termino.toUpperCase().trim(); const todos = this.obtenerTodos(); return Object.values(todos).filter(prod => prod.codigo.includes(termino) || prod.descripcion.includes(termino)).slice(0, 10); }
}

const gestorCotizaciones = new GestorCotizaciones();
const gestorClientes = new GestorClientes();
const gestorProductos = new GestorProductos();
let clienteActual = null, modoEdicion = false, productosEnCotizacion = [], cotizacionesEmitidas = JSON.parse(localStorage.getItem('cotizacionesEmitidas') || '[]'), articuloEdicion = null, archivosAdjuntos = [], cotizacionGuardada = false, datosDespacho = null, cotizacionActualIndex = null, pdfEmitido = false, esEdicionCotizacion = false, esLecturaCotizacion = false, pdfActualDoc = null, numeroCotizacionActual = null, estadoCotizacionActual = 'pendiente';

document.addEventListener('DOMContentLoaded', function() {
  cargarConfiguracion();
  
  document.getElementById('inputRut').addEventListener('input', function(e) {
    let v = e.target.value.replace(/[^0-9kK-]/g, '');
    if (v.length > 1 && v[v.length - 2] !== '-') v = v.slice(0, -1) + '-' + v.slice(-1);
    e.target.value = v.toUpperCase();
    mostrarDesplegableClientes();
  });
  
  document.getElementById('inputRut').addEventListener('keydown', function(e) {
    if (e.key === 'Enter') buscarCliente();
  });
  
  document.getElementById('inputCodigoProducto').addEventListener('input', function(e) {
    e.target.value = e.target.value.toUpperCase();
    mostrarAutocomplete();
  });
  
  document.getElementById('inputCodigoProducto').addEventListener('keydown', function(e) {
    if (e.key === 'Enter') buscarProducto();
  });
  
  document.getElementById('costoProducto').addEventListener('change', calcularMargenUtilidad);
  document.getElementById('valorTotalProducto').addEventListener('change', calcularMargenUtilidad);
  
  document.addEventListener('click', function(e) {
    if (!e.target.closest('.seccion-cliente')) document.getElementById('desplegableClientes').classList.remove('activo');
    if (!e.target.closest('.busqueda-producto')) document.getElementById('autocompleteLista').classList.remove('activo');
  });
});

function calcularMargenUtilidad() {
  const costo = parseFloat(document.getElementById('costoProducto').value) || 0;
  const precioNeto = parseFloat(document.getElementById('valorTotalProducto').value) || 0;
  if (costo > 0 && precioNeto > 0) {
    if (precioNeto < costo) { document.getElementById('porcentajeProducto').value = '0.00'; return; }
    const margen = +((precioNeto - costo) / precioNeto * 100).toFixed(2);
    document.getElementById('porcentajeProducto').value = margen;
  } else {
    document.getElementById('porcentajeProducto').value = '0.00';
  }
}

function mostrarDesplegableClientes() {
  const input = document.getElementById('inputRut').value.trim(), desplegable = document.getElementById('desplegableClientes');
  if (!input || input.length < 2) { desplegable.classList.remove('activo'); return; }
  const resultados = gestorClientes.buscarPorCoincidencia(input);
  if (resultados.length === 0) { desplegable.classList.remove('activo'); return; }
  let html = '';
  resultados.forEach(cliente => { html += `<div class="item-cliente" onclick="seleccionarClienteDesplegable('${cliente.rut}', '${cliente.razonSocial}')"><strong>${cliente.rut}</strong> - ${cliente.razonSocial}</div>`; });
  desplegable.innerHTML = html;
  desplegable.classList.add('activo');
}

function seleccionarClienteDesplegable(rut, razonSocial) {
  document.getElementById('inputRut').value = rut;
  document.getElementById('desplegableClientes').classList.remove('activo');
  buscarCliente();
}

function abrirModalClientes() {
  const modal = document.getElementById('modalClientes'), listado = document.getElementById('listaClientesModal'), todos = gestorClientes.obtenerTodos(), clientes = Object.entries(todos);
  if (clientes.length === 0) {
    listado.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 15px; font-size:11px;">NO HAY CLIENTES REGISTRADOS</p>';
  } else {
    let html = '<div style="overflow-x:auto;"><table class="tabla-clientes-modal"><thead><tr><th>RUT</th><th>RAZÓN SOCIAL</th><th>GIRO</th><th>CONTACTO</th><th>CELULAR</th><th>ACCIÓN</th></tr></thead><tbody>';
    clientes.forEach(([rut, cliente]) => { html += `<tr><td>${rut}</td><td>${cliente.razonSocial}</td><td>${cliente.giro}</td><td>${cliente.nombreContacto}</td><td>${cliente.celular}</td><td><button class="btn-seleccionar-cliente" onclick="seleccionarClienteDelModal('${rut}')">SELECCIONAR</button></td></tr>`; });
    html += '</tbody></table></div>';
    listado.innerHTML = html;
  }
  modal.style.display = 'block';
}

function seleccionarClienteDelModal(rut) {
  document.getElementById('inputRut').value = rut;
  cerrarModalClientes();
  buscarCliente();
}

function cerrarModalClientes() { document.getElementById('modalClientes').style.display = 'none'; }

function buscarCliente() {
  if (esLecturaCotizacion) { mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado'); return; }
  if (cotizacionGuardada && (estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado')) { mostrarMensaje('COTIZACIÓN BLOQUEADA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado'); return; }
  const rut = document.getElementById('inputRut').value.trim();
  if (!rut) return mostrarMensaje('POR FAVOR INGRESE UN RUT', 'error');
  if (!validarRut(rut)) return mostrarMensaje('RUT INVÁLIDO - VERIFIQUE FORMATO Y DÍGITO VERIFICADOR', 'error');
  const cliente = gestorClientes.buscarPorRut(rut);
  if (cliente) {
    clienteActual = cliente;
    mostrarMensaje('CLIENTE ENCONTRADO ✓', 'exito');
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
}

function mostrarMensaje(texto, tipo) {
  const m = document.getElementById('mensaje');
  m.className = `mensaje mensaje-${tipo}`;
  m.textContent = texto;
  m.style.display = 'block';
}

function mostrarResumenCliente(cliente) {
  const r = document.getElementById('resumenCliente'), btnEditarDisabled = (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') ? 'disabled' : '';
  r.className = 'resumen-cliente activo';
  r.innerHTML = `<h4>✓ CLIENTE REGISTRADO</h4><p><strong>RUT:</strong> ${cliente.rut}</p><p><strong>RAZÓN SOCIAL:</strong> ${cliente.razonSocial}</p><p><strong>GIRO:</strong> ${cliente.giro}</p><p><strong>DIRECCIÓN:</strong> ${cliente.direccion}</p><p><strong>CONTACTO:</strong> ${cliente.nombreContacto}</p><p><strong>CELULAR:</strong> ${cliente.celular}</p><p><strong>MAIL:</strong> ${cliente.mail}</p><p><strong>MEDIO DE PAGO:</strong> ${cliente.medioPago}</p><div class="botones-resumen"><button class="btn btn-editar" onclick="editarCliente()" ${btnEditarDisabled}>✏️ EDITAR</button></div>`;
}

function editarCliente() {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado'); return; }
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
  mostrarMensaje('MODO EDICIÓN - MODIFIQUE LOS CAMPOS', 'info');
}

function cancelarEdicion() {
  if (!clienteActual) return;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarMensaje('CLIENTE ENCONTRADO ✓', 'exito');
  mostrarResumenCliente(clienteActual);
}

function guardarCliente() {
  if (esLecturaCotizacion) { mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado'); return; }
  const rut = document.getElementById('rut').value, rs = document.getElementById('razonSocial').value.trim(), gi = document.getElementById('giro').value.trim(), di = document.getElementById('direccion').value.trim(), nc = document.getElementById('nombreContacto').value.trim(), ce = document.getElementById('celular').value.trim(), ma = document.getElementById('mail').value.trim(), mp = document.getElementById('medioPago').value;
  if (!rs || !gi || !di || !nc || !ce || !ma || !mp) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS REQUERIDOS', 'error');
  
  const msgEmail = document.getElementById('mail').nextElementSibling;
  const msgTel = document.getElementById('celular').nextElementSibling;
  
  if (msgEmail && msgEmail.classList.contains('error')) return mostrarMensaje('EMAIL INVÁLIDO', 'error');
  if (msgTel && msgTel.classList.contains('error')) return mostrarMensaje('TELÉFONO INVÁLIDO', 'error');
  
  const cliente = { rut, razonSocial: rs.toUpperCase(), giro: gi.toUpperCase(), direccion: di.toUpperCase(), nombreContacto: nc.toUpperCase(), celular: ce, mail: ma.toUpperCase(), medioPago: mp };
  if (modoEdicion) gestorClientes.actualizarCliente(rut, cliente);
  else gestorClientes.agregarCliente(rut, cliente);
  mostrarMensaje(modoEdicion ? 'CLIENTE ACTUALIZADO ✓' : 'CLIENTE GUARDADO ✓', 'exito');
  clienteActual = cliente;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarResumenCliente(cliente);
  habilitarProductos();
}

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
  descuentoGlobal = {tipo: 'porcentaje', valor: 0};
  document.getElementById('tipoDescuento').value = 'porcentaje';
  document.getElementById('valorDescuento').value = '';
  document.getElementById('resumenDespacho').classList.remove('activo');
  document.getElementById('resumenCompra').classList.remove('activo');
  document.getElementById('seccionCierre').classList.remove('activo');
  deshabilitarProductos();
  actualizarTablaProductos();
  document.getElementById('inputRut').focus();
}

function mostrarAutocomplete() {
  const input = document.getElementById('inputCodigoProducto').value.trim(), lista = document.getElementById('autocompleteLista');
  if (!input || input.length < 1) { lista.classList.remove('activo'); return; }
  const resultados = gestorProductos.buscarPorCodigoODescripcion(input);
  if (resultados.length === 0) { lista.classList.remove('activo'); return; }
  let html = '';
  resultados.forEach(prod => { html += `<div class="autocomplete-item" onclick="seleccionarProductoAutocomplete('${prod.codigo}')"><strong>${prod.codigo}</strong> - ${prod.descripcion}</div>`; });
  lista.innerHTML = html;
  lista.classList.add('activo');
}

function seleccionarProductoAutocomplete(codigo) {
  document.getElementById('inputCodigoProducto').value = codigo;
  document.getElementById('autocompleteLista').classList.remove('activo');
  buscarProducto();
}

function buscarProducto() {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensajeProducto('MODO LECTURA. NO SE PUEDEN AGREGAR PRODUCTOS.', 'bloqueado'); return; }
  const cod = document.getElementById('inputCodigoProducto').value.trim();
  if (!cod) return mostrarMensajeProducto('INGRESE UN CÓDIGO O DESCRIPCIÓN', 'error');
  const prod = gestorProductos.buscarPorCodigo(cod);
  if (prod) {
    mostrarMensajeProducto('PRODUCTO ENCONTRADO ✓', 'exito');
    document.getElementById('formularioProducto').style.display = 'none';
    agregarProductoACotizacion(cod, prod);
    document.getElementById('inputCodigoProducto').value = '';
  } else {
    mostrarMensajeProducto('NO ENCONTRADO. CREE UNO NUEVO.', 'info');
    document.getElementById('formularioProducto').style.display = 'block';
    document.getElementById('codigoProducto').value = cod;
    document.getElementById('descripcionProducto').value = '';
    document.getElementById('costoProducto').value = '';
    document.getElementById('valorTotalProducto').value = '';
    document.getElementById('porcentajeProducto').value = '';
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
  const cod = document.getElementById('codigoProducto').value.trim(), desc = document.getElementById('descripcionProducto').value.trim(), cos = formatearNumero(document.getElementById('costoProducto').value), precioNeto = formatearNumero(document.getElementById('valorTotalProducto').value), por = parseFloat(document.getElementById('porcentajeProducto').value) || 0, link = document.getElementById('linkProducto').value.trim().toLowerCase();
  if (!cod || !desc || isNaN(cos) || cos <= 0 || isNaN(precioNeto) || precioNeto <= 0 || !link) return mostrarMensajeProducto('COMPLETE TODOS LOS CAMPOS REQUERIDOS', 'error');
  if (precioNeto < cos) return mostrarMensajeProducto('❌ ERROR: PRECIO NETO DEBE SER ≥ COSTO', 'error');
  const prod = { codigo: cod, descripcion: desc.toUpperCase(), costo: cos, precioNeto: precioNeto, porcentaje: +(por.toFixed(2)), link: link };
  gestorProductos.agregarProducto(cod, prod);
  mostrarMensajeProducto('PRODUCTO GUARDADO ✓', 'exito');
  cancelarProducto();
  agregarProductoACotizacion(cod, prod);
}

function cancelarProducto() {
  document.getElementById('formularioProducto').style.display = 'none';
  document.getElementById('codigoProducto').value = '';
  document.getElementById('descripcionProducto').value = '';
  document.getElementById('costoProducto').value = '';
  document.getElementById('valorTotalProducto').value = '';
  document.getElementById('porcentajeProducto').value = '';
  document.getElementById('linkProducto').value = '';
  document.getElementById('inputCodigoProducto').value = '';
  document.getElementById('mensajeProducto').style.display = 'none';
}

function agregarProductoACotizacion(cod, prod) {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensaje('MODO LECTURA. NO SE PUEDEN AGREGAR PRODUCTOS.', 'bloqueado'); return; }
  const ex = productosEnCotizacion.find(p => p.codigo === cod);
  if (ex) {
    ex.cantidad++;
    ex.totalNeto = formatearNumero(ex.cantidad * ex.precioNetoConDescuento);
  } else {
    productosEnCotizacion.push({
      codigo: prod.codigo,
      descripcion: prod.descripcion,
      cantidad: 1,
      precioNeto: prod.precioNeto,
      costo: prod.costo,
      descuento: 0,
      precioNetoConDescuento: prod.precioNeto,
      totalNeto: prod.precioNeto,
      link: prod.link
    });
  }
  actualizarTablaProductos();
}

function actualizarTablaProductos() {
  const cont = document.getElementById('tablaProductosContenedor');
  if (productosEnCotizacion.length === 0) {
    cont.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS</div>';
    document.getElementById('resumenTotales').style.display = 'none';
    mostrarSeccionDescuento();
    return;
  }
  let html = '<div style="overflow-x:auto;"><table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANT</th><th>PRECIO UNITARIO (NETO)</th><th>DESC(%)</th><th>PRECIO DESC (NETO)</th><th>COSTO UNIT</th><th>COSTO TOTAL</th><th>TOTAL NETO</th><th>% MARGEN NETO</th><th>UTILIDAD NETA</th><th>TOTAL CON IVA</th><th>ACCIÓN</th></tr></thead><tbody>';
  productosEnCotizacion.forEach((p, i) => {
    const bloqueado = esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado';
    const inputQuantityDisabled = bloqueado ? 'disabled' : '';
    const inputDescuentoDisabled = bloqueado ? 'disabled' : '';
    const btnEliminarDisplay = bloqueado ? 'none' : 'block';

    const costoTotal = formatearNumero(parseFloat(p.costo) * p.cantidad);
    const precioUnitarioNeto = formatearNumero(parseFloat(p.precioNetoConDescuento));
    const totalNeto = formatearNumero(p.totalNeto);
    const utilidadNeta = formatearNumero(totalNeto - costoTotal);
    const margenPorcentaje = totalNeto > 0 ? formatearNumero((utilidadNeta / totalNeto) * 100) : 0;
    const totalConIva = formatearNumero(totalNeto * 1.19);
    const claseMargen = margenPorcentaje >= 20 ? 'margen-verde' : 'margen-roja';

    html += `<tr>
      <td>${p.codigo}</td>
      <td>${p.descripcion}</td>
      <td class="texto-centrado">
        <input type="number" min="1" value="${p.cantidad}" onchange="actualizarCantidad(${i}, this.value)" ${inputQuantityDisabled} style="width:100%;text-align:center;border:1px solid #ddd;padding:4px;">
      </td>
      <td class="valor-numerico">$${precioUnitarioNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="texto-centrado">
        <input type="number" min="0" max="100" step="0.01" value="${p.descuento}" style="width:100%;padding:4px;text-align:center;border:1px solid #ddd;" onchange="actualizarDescuento(${i}, this.value)" ${inputDescuentoDisabled}>
      </td>
      <td class="valor-numerico">$${parseFloat(p.precioNetoConDescuento).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${parseFloat(p.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${costoTotal.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${totalNeto.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico ${claseMargen}">${margenPorcentaje.toFixed(2)}%</td>
      <td class="valor-numerico">$${utilidadNeta.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td><button class="btn-eliminar" onclick="eliminarProducto(${i})" style="display:${btnEliminarDisplay};width:100%;">DEL</button></td>
    </tr>`;
  });
  html += '</tbody></table></div>';
  cont.innerHTML = html;
  actualizarResumenTotales();
  mostrarSeccionDescuento();
}

function actualizarCantidad(i, cant) {
  cant = parseInt(cant);
  if (isNaN(cant) || cant < 1) { alert('❌ CANTIDAD INVÁLIDA - DEBE SER MAYOR A 0'); actualizarTablaProductos(); return; }
  productosEnCotizacion[i].cantidad = cant;
  productosEnCotizacion[i].totalNeto = formatearNumero(productosEnCotizacion[i].precioNetoConDescuento * cant);
  actualizarTablaProductos();
}

function actualizarDescuento(i, desc) {
  desc = parseFloat(desc) || 0;
  if (desc < 0 || desc > 100) { alert('❌ ERROR: EL DESCUENTO DEBE ESTAR ENTRE 0% Y 100%'); actualizarTablaProductos(); return; }
  productosEnCotizacion[i].descuento = desc;
  const precioConDesc = productosEnCotizacion[i].precioNeto * (1 - desc / 100);
  productosEnCotizacion[i].precioNetoConDescuento = formatearNumero(precioConDesc);
  productosEnCotizacion[i].totalNeto = formatearNumero(productosEnCotizacion[i].precioNetoConDescuento * productosEnCotizacion[i].cantidad);
  actualizarTablaProductos();
}

function eliminarProducto(i) {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensaje('MODO LECTURA. NO SE PUEDEN ELIMINAR PRODUCTOS.', 'bloqueado'); return; }
  if (!confirm('¿Está seguro de eliminar este producto?')) return;
  productosEnCotizacion.splice(i, 1);
  actualizarTablaProductos();
}

function actualizarResumenTotales() {
  const totalNetoSinDesc = formatearNumero(productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0));
  descuentoGlobal.valor = parseFloat(document.getElementById('valorDescuento').value) || 0;
  descuentoGlobal.tipo = document.getElementById('tipoDescuento').value;
  
  const montoDescuento = obtenerMontoDescuento();
  const totalNetoFinal = formatearNumero(totalNetoSinDesc - montoDescuento);
  const iva = formatearNumero(totalNetoFinal * 0.19);
  const totalConIva = formatearNumero(totalNetoFinal + iva);

  document.getElementById('totalNeto').textContent = '$' + totalNetoSinDesc.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalDescuento').textContent = '$' + montoDescuento.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalNetoFinal').textContent = '$' + totalNetoFinal.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalIva').textContent = '$' + iva.toLocaleString('es-CL', {minimumFractionDigits: 2});
  document.getElementById('totalGeneral').textContent = '$' + totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2});
  
  const textoDesc = descuentoGlobal.valor > 0 ? `${descuentoGlobal.tipo === 'porcentaje' ? descuentoGlobal.valor + '%' : '$' + descuentoGlobal.valor.toLocaleString('es-CL', {minimumFractionDigits: 2})} descuento aplicado` : '';
  document.getElementById('textoDescuento').textContent = textoDesc;
  
  document.getElementById('resumenTotales').style.display = 'block';
}

function abrirArticulos() {
  document.getElementById('modalArticulos').style.display = 'block';
  document.getElementById('inputBuscador').value = '';
  listarArticulos();
}

function cerrarArticulos() {
  document.getElementById('modalArticulos').style.display = 'none';
}

function listarArticulos() {
  const todos = gestorProductos.obtenerTodos();
  let html = '<div style="overflow-x:auto;"><table class="tabla-articulos"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO (SIN IVA)</th><th>PRECIO NETO (SIN IVA)</th><th>% MARGEN NETO</th><th>UTILIDAD NETA</th><th>TOTAL CON IVA</th><th>LINK</th><th>ACCIÓN</th></tr></thead><tbody>';
  const claves = Object.keys(todos);
  if (claves.length === 0) {
    html += '<tr><td colspan="9" style="text-align:center;">NO HAY ARTÍCULOS</td></tr>';
  } else {
    claves.forEach(codigo => {
      const art = todos[codigo], 
            utilidadNeta = formatearNumero(parseFloat(art.precioNeto) - parseFloat(art.costo)),
            margenPorcentaje = art.precioNeto > 0 ? formatearNumero((utilidadNeta / art.precioNeto) * 100) : 0,
            totalConIva = formatearNumero(parseFloat(art.precioNeto) * 1.19);
      html += `<tr>
        <td>${art.codigo}</td>
        <td>${art.descripcion}</td>
        <td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">$${parseFloat(art.precioNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">${margenPorcentaje.toFixed(2)}%</td>
        <td class="valor-numerico">$${utilidadNeta.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td style="text-transform:lowercase;"><a href="${art.link.startsWith('http') ? art.link : 'https://' + art.link}" target="_blank">${art.link}</a></td>
        <td>
          <button class="btn btn-editar" onclick="editarArticulo('${art.codigo}')">EDITAR</button>
          <button class="btn btn-eliminar" onclick="eliminarArticulo('${art.codigo}')">ELIMINAR</button>
        </td>
      </tr>`;
    });
  }
  html += '</tbody></table></div>';
  document.getElementById('tablaArticulosContenedor').innerHTML = html;
}

function filtrarArticulos() {
  const termino = document.getElementById('inputBuscador').value.toUpperCase().trim();
  if (!termino) { listarArticulos(); return; }
  
  const todos = gestorProductos.obtenerTodos();
  const filtrados = Object.entries(todos).filter(([codigo, art]) => 
    codigo.includes(termino) || art.descripcion.includes(termino)
  ).reduce((obj, [codigo, art]) => { obj[codigo] = art; return obj; }, {});

  let html = '<div style="overflow-x:auto;"><table class="tabla-articulos"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO (SIN IVA)</th><th>PRECIO NETO (SIN IVA)</th><th>% MARGEN NETO</th><th>UTILIDAD NETA</th><th>TOTAL CON IVA</th><th>LINK</th><th>ACCIÓN</th></tr></thead><tbody>';
  
  if (Object.keys(filtrados).length === 0) {
    html += '<tr><td colspan="9" style="text-align:center;">NO HAY RESULTADOS PARA: "' + termino + '"</td></tr>';
  } else {
    Object.keys(filtrados).forEach(codigo => {
      const art = filtrados[codigo],
            utilidadNeta = formatearNumero(parseFloat(art.precioNeto) - parseFloat(art.costo)),
            margenPorcentaje = art.precioNeto > 0 ? formatearNumero((utilidadNeta / art.precioNeto) * 100) : 0,
            totalConIva = formatearNumero(parseFloat(art.precioNeto) * 1.19);
      html += `<tr>
        <td>${art.codigo}</td>
        <td>${art.descripcion}</td>
        <td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">$${parseFloat(art.precioNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">${margenPorcentaje.toFixed(2)}%</td>
        <td class="valor-numerico">$${utilidadNeta.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td style="text-transform:lowercase;"><a href="${art.link.startsWith('http') ? art.link : 'https://' + art.link}" target="_blank">${art.link}</a></td>
        <td>
          <button class="btn btn-editar" onclick="editarArticulo('${art.codigo}')">EDITAR</button>
          <button class="btn btn-eliminar" onclick="eliminarArticulo('${art.codigo}')">ELIMINAR</button>
        </td>
      </tr>`;
    });
  }
  html += '</tbody></table></div>';
  document.getElementById('tablaArticulosContenedor').innerHTML = html;
}

function limpiarBuscadorArticulos() {
  document.getElementById('inputBuscador').value = '';
  listarArticulos();
}

function editarArticulo(codigo) {
  const art = gestorProductos.buscarPorCodigo(codigo);
  if (!art) return;
  articuloEdicion = art;
  const f = document.getElementById('formularioEditarArticulo');
  f.style.display = 'block';
  f.innerHTML = `<h4>EDITAR ARTÍCULO</h4>
    <div class="fila-campos-producto">
      <div class="campo-grupo"><label>CÓDIGO</label><input type="text" value="${art.codigo}" disabled></div>
      <div class="campo-grupo"><label>DESCRIPCIÓN</label><input type="text" id="editarDescripcion" value="${art.descripcion}"></div>
    </div>
    <div class="fila-campos-producto-tres">
      <div class="campo-grupo"><label>COSTO (SIN IVA)</label><input type="number" id="editarCosto" value="${art.costo}"></div>
      <div class="campo-grupo"><label>PRECIO NETO (SIN IVA)</label><input type="number" id="editarPrecioNeto" value="${art.precioNeto}"></div>
      <div class="campo-grupo"><label>% MARGEN NETO</label><input type="number" id="editarPorcentaje" value="${art.porcentaje}" disabled class="input-solo-lectura"></div>
    </div>
    <div class="campo-grupo"><label>LINK</label><input type="text" id="editarLink" value="${art.link}"></div>
    <div class="botones-producto">
      <button class="btn btn-agregar" onclick="guardarEdicionArticulo()">GUARDAR</button>
      <button class="btn btn-cancelar" onclick="cancelarEdicionArticulo()">CANCELAR</button>
    </div>`;
  document.getElementById('editarCosto').addEventListener('change', calcularMargenEdicion);
  document.getElementById('editarPrecioNeto').addEventListener('change', calcularMargenEdicion);
}

function calcularMargenEdicion() {
  const c = parseFloat(document.getElementById('editarCosto').value) || 0;
  const pn = parseFloat(document.getElementById('editarPrecioNeto').value) || 0;
  if (c > 0 && pn > 0) {
    if (pn < c) { document.getElementById('editarPorcentaje').value = '0.00'; return; }
    const margen = +((pn - c) / pn * 100).toFixed(2);
    document.getElementById('editarPorcentaje').value = margen;
  } else {
    document.getElementById('editarPorcentaje').value = '0.00';
  }
}

function guardarEdicionArticulo() {
  if (!articuloEdicion) return;
  const codigo = articuloEdicion.codigo, desc = document.getElementById('editarDescripcion').value.trim().toUpperCase(), cos = formatearNumero(document.getElementById('editarCosto').value), pn = formatearNumero(document.getElementById('editarPrecioNeto').value), por = parseFloat(document.getElementById('editarPorcentaje').value) || 0, link = document.getElementById('editarLink').value.trim().toLowerCase();
  if (!desc || !link) { alert('COMPLETE TODOS LOS CAMPOS'); return; }
  if (pn < cos) { alert('❌ ERROR: PRECIO NETO (SIN IVA) DEBE SER ≥ COSTO'); return; }
  const nuevo = { codigo, descripcion: desc, costo: cos, precioNeto: pn, porcentaje: +(por.toFixed(2)), link: link };
  gestorProductos.actualizarProducto(codigo, nuevo);
  articuloEdicion = null;
  document.getElementById('formularioEditarArticulo').style.display = 'none';
  listarArticulos();
}

function cancelarEdicionArticulo() { articuloEdicion = null; document.getElementById('formularioEditarArticulo').style.display = 'none'; }

function eliminarArticulo(codigo) { if (confirm('¿ESTÁ SEGURO DE ELIMINAR ESTE ARTÍCULO?')) { gestorProductos.eliminarProducto(codigo); listarArticulos(); } }

function generarPDF() {
  if (!clienteActual) { alert('INGRESE CLIENTE'); return; }
  if (productosEnCotizacion.length === 0) { alert('AGREGUE PRODUCTOS'); return; }
  if (estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensaje('COTIZACIÓN BLOQUEADA. NO SE PUEDE REGENERAR PDF.', 'bloqueado'); return; }
  let numCot;
  if (esEdicionCotizacion && cotizacionActualIndex !== null) {
    numCot = cotizacionesEmitidas[cotizacionActualIndex].numero;
  } else {
    numCot = gestorCotizaciones.siguienteCotizacion();
  }
  numeroCotizacionActual = numCot;
  const totalNetoSinDesc = formatearNumero(productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0));
  const montoDescuento = obtenerMontoDescuento();
  const totalNetoFinal = formatearNumero(totalNetoSinDesc - montoDescuento);
  const estado = datosDespacho ? 'aceptado' : 'pendiente';
  const cotizacion = {
    numero: numCot,
    razonSocial: clienteActual.razonSocial,
    totalNeto: totalNetoFinal,
    fecha: new Date().toISOString(),
    cliente: JSON.parse(JSON.stringify(clienteActual)),
    productos: JSON.parse(JSON.stringify(productosEnCotizacion)),
    estado: estado,
    despacho: datosDespacho || null,
    descuentoGlobal: descuentoGlobal
  };
  if (esEdicionCotizacion && cotizacionActualIndex !== null) {
    cotizacionesEmitidas[cotizacionActualIndex] = cotizacion;
    mostrarMensaje('COTIZACIÓN ACTUALIZADA ✓', 'exito');
  } else {
    cotizacionesEmitidas.push(cotizacion);
    mostrarMensaje('COTIZACIÓN GUARDADA ✓', 'exito');
  }
  localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
  pdfEmitido = true;
  document.getElementById('seccionCierre').classList.add('activo');
  generarPDFDocumento(cotizacion);
}

function generarPDFDocumento(cotizacion) {
  try {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF('p', 'mm', 'a4');
    const config = JSON.parse(localStorage.getItem('configEmpresa')) || configEmpresaDefault;

    const netoFinal = parseFloat(cotizacion.totalNeto || 0);
    const iva = formatearNumero(netoFinal * 0.19);
    const total = formatearNumero(netoFinal + iva);

    const fechaEmision = new Date(cotizacion.fecha);
    const fechaFormateada = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
    const horaFormateada = `${fechaEmision.getHours().toString().padStart(2, '0')}:${fechaEmision.getMinutes().toString().padStart(2, '0')}:${fechaEmision.getSeconds().toString().padStart(2, '0')}`;

    doc.setFillColor(31, 111, 139);
    doc.rect(0, 0, 210, 20, 'F');
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(18);
    doc.setFont(undefined, 'bold');
    doc.text('COTIZACIÓN', 15, 12);
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    doc.text(`${cotizacion.numero} | ${fechaFormateada}`, 195, 12, {align: 'right'});

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
    const clienteData = [
      ['RUT:', cotizacion.cliente.rut, 'CONTACTO:', cotizacion.cliente.nombreContacto],
      ['RAZÓN SOCIAL:', cotizacion.cliente.razonSocial, 'CELULAR:', cotizacion.cliente.celular],
      ['GIRO:', cotizacion.cliente.giro, 'MAIL:', cotizacion.cliente.mail],
      ['DIRECCIÓN:', cotizacion.cliente.direccion, 'MEDIO PAGO:', cotizacion.cliente.medioPago]
    ];
    clienteData.forEach(row => {
      doc.setFont(undefined, 'bold');
      doc.text(row[0], 15, yPos);
      doc.setFont(undefined, 'normal');
      doc.text(row[1], 45, yPos);
      if (row[2]) {
        doc.setFont(undefined, 'bold');
        doc.text(row[2], 110, yPos);
        doc.setFont(undefined, 'normal');
        doc.text(row[3], 145, yPos);
      }
      yPos += 7;
    });

    doc.setDrawColor(242, 92, 5);
    doc.setLineWidth(0.8);
    doc.line(15, yPos + 3, 195, yPos + 3);
    yPos += 10;
    doc.setFont(undefined, 'bold');
    doc.setFontSize(11);
    doc.text('PRODUCTOS Y SERVICIOS', 15, yPos);
    yPos += 5;

    doc.autoTable({
      startY: yPos,
      head: [['CÓDIGO', 'DESCRIPCIÓN', 'CANT.', 'PRECIO UNITARIO (NETO)', 'TOTAL NETO', 'TOTAL CON IVA']],
      body: cotizacion.productos.map(p => [
        p.codigo,
        p.descripcion,
        p.cantidad.toString(),
        `$${Math.round(parseFloat(p.precioNetoConDescuento)).toLocaleString('es-CL')}`,
        `$${Math.round(parseFloat(p.totalNeto)).toLocaleString('es-CL')}`,
        `$${Math.round(parseFloat(p.totalNeto) * 1.19).toLocaleString('es-CL')}`
      ]),
      theme: 'striped',
      styles: {fontSize: 9, cellPadding: 4, halign: 'center', lineColor: [220, 220, 220], lineWidth: 0.1},
      headStyles: {fillColor: [31, 111, 139], textColor: 255, fontStyle: 'bold', halign: 'center', fontSize: 10},
      columnStyles: {
        0: {cellWidth: 25, halign: 'center'},
        1: {cellWidth: 70, halign: 'left'},
        2: {cellWidth: 15, halign: 'center'},
        3: {cellWidth: 30, halign: 'right'},
        4: {cellWidth: 30, halign: 'right'},
        5: {cellWidth: 30, halign: 'right'}
      },
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
    const totalNetoSinDesc = formatearNumero(cotizacion.productos.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0));
    doc.text('NETO (SIN DESC):', 160, lineY + 8, {align: 'right'});
    doc.text(`$${Math.round(totalNetoSinDesc).toLocaleString('es-CL')}`, 195, lineY + 8, {align: 'right'});
    
    if (cotizacion.descuentoGlobal && cotizacion.descuentoGlobal.valor > 0) {
      const montoDesc = cotizacion.descuentoGlobal.tipo === 'porcentaje' 
        ? formatearNumero(totalNetoSinDesc * (cotizacion.descuentoGlobal.valor / 100))
        : cotizacion.descuentoGlobal.valor;
      doc.text(`DESC (${cotizacion.descuentoGlobal.tipo === 'porcentaje' ? cotizacion.descuentoGlobal.valor + '%' : '$'})`, 160, lineY + 15, {align: 'right'});
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
    doc.text(`RUT: ${config.rut} | ${config.nombre} | Dirección: ${config.direccion}`, 15, pageHeight - 18);
    doc.text(`Mail: ${config.email} | Teléfono: ${config.telefono}`, 15, pageHeight - 14);
    doc.text(`Fecha de emisión: ${fechaFormateada} | Hora: ${horaFormateada}`, 15, pageHeight - 10);
    doc.setTextColor(180, 180, 180);
    doc.setFontSize(9);
    doc.setFont(undefined, 'italic');
    doc.text('ERP DESARROLLADO POR ING. AGONPA', 105, pageHeight - 5, {align: 'center'});

    pdfActualDoc = doc;
    const pdfData = doc.output('datauristring');
    const modal = document.getElementById('modalPDF'), container = document.getElementById('pdfContainer');
    container.innerHTML = `<iframe src="${pdfData}" style="width:100%;height:1000px;border:none;"></iframe>`;
    modal.classList.add('mostrar');
    doc.save(`Cotizacion-${numeroCotizacionActual}.pdf`);
  } catch (error) {
    console.error('Error al generar PDF:', error);
    alert('Error al generar PDF: ' + error.message);
  }
}

function cerrarModalPDF() {
  const modal = document.getElementById('modalPDF');
  modal.classList.remove('mostrar');
  document.getElementById('pdfContainer').innerHTML = '';
  pdfActualDoc = null;
  limpiarCotizacion();
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones'), cont = document.getElementById('listaCotizaciones');
  if (cotizacionesEmitidas.length === 0) {
    cont.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 15px; font-size:11px;">NO HAY COTIZACIONES EMITIDAS</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th>FECHA EMISIÓN</th><th>ESTADO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    cotizacionesEmitidas.forEach((c, index) => {
      const claseEstado = c.estado === 'aceptado' ? 'cot-aceptado' : (c.estado === 'rechazado' ? 'cot-rechazado' : 'cot-pendiente'), fechaEmision = new Date(c.fecha), fechaFormateada = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`, badgeClase = c.estado === 'aceptado' ? 'badge-aceptado' : (c.estado === 'rechazado' ? 'badge-rechazado' : 'badge-pendiente'), btnEditarDisabled = (c.estado === 'rechazado') ? 'disabled' : '';
      html += `<tr class="${claseEstado}"><td>${c.numero}</td><td>${c.razonSocial}</td><td style="text-align:right;">$${formatearNumero(parseFloat(c.totalNeto)).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${fechaFormateada}</td><td><span class="badge-estado ${badgeClase}">${c.estado}</span></td><td style="text-align:center;"><button class="btn-ver" onclick="verCotizacion(${index})">VER</button><button class="btn-editar-cot" onclick="editarCotizacionGuardada(${index})" ${btnEditarDisabled}>EDITAR</button></td></tr>`;
    });
    html += '</tbody></table>';
    cont.innerHTML = html;
  }
  modal.style.display = 'block';
}

function verCotizacion(index) { if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; } generarPDFDocumento(cotizacionesEmitidas[index]); }

function editarCotizacionGuardada(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  const cotizacion = cotizacionesEmitidas[index];
  if (cotizacion.estado === 'rechazado') { mostrarMensaje('COTIZACIÓN RECHAZADA. NO SE PUEDE EDITAR.', 'bloqueado'); return; }
  clienteActual = JSON.parse(JSON.stringify(cotizacion.cliente));
  productosEnCotizacion = JSON.parse(JSON.stringify(cotizacion.productos));
  datosDespacho = cotizacion.despacho;
  descuentoGlobal = cotizacion.descuentoGlobal || {tipo: 'porcentaje', valor: 0};
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
    mostrarMensaje(`COTIZACIÓN ${cotizacion.numero} EN MODO LECTURA`, 'info');
  } else {
    mostrarMensaje(`COTIZACIÓN ${cotizacion.numero} CARGADA PARA EDITAR`, 'info');
  }
}

function cerrarCotizaciones() { document.getElementById('modalCotizaciones').style.display = 'none'; }

function mostrarResumenCompra() {
  const totalCosto = productosEnCotizacion.reduce((acc, p) => acc + (parseFloat(p.costo) * p.cantidad), 0);
  let html = '<h4>📋 RESUMEN DE COMPRA</h4><div style="overflow-x:auto;"><table class="tabla-compra"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANTIDAD</th><th>COSTO UNITARIO</th><th>LINK</th></tr></thead><tbody>';
  productosEnCotizacion.forEach(p => {
    const linkSeguro = p.link.startsWith('http') ? p.link : 'https://' + p.link;
    html += `<tr><td>${p.codigo}</td><td>${p.descripcion}</td><td class="valor-numerico">${p.cantidad}</td><td class="valor-numerico">$${parseFloat(p.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td><a href="${linkSeguro}" target="_blank">${p.link}</a></td></tr>`;
  });
  html += `<tr style="background: #fff3cd; font-weight: bold;"><td colspan="4" style="text-align: right;">TOTAL COSTO DE COMPRA:</td><td class="valor-numerico">$${formatearNumero(totalCosto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td></tr>`;
  html += '</tbody></table></div>';
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

function cerrarModalAceptado() { document.getElementById('modalAceptado').style.display = 'none'; }

function abrirSelectorArchivos() { document.getElementById('inputArchivo').click(); }

function agregarArchivo(event) {
  const file = event.target.files[0];
  if (!file) return;
  const nuevoArchivo = {nombre: file.name, tamaño: (file.size / 1024).toFixed(2), tipo: file.type, contenido: ''};
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
  const container = document.getElementById('adjuntosContainer'), lista = document.getElementById('listaAdjuntos');
  if (archivosAdjuntos.length === 0) { container.style.display = 'none'; return; }
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
  const tipoEntrega = document.getElementById('tipoEntrega').value, region = document.getElementById('regionSelect').value, comuna = document.getElementById('comunaInput').value.trim(), direccion = document.getElementById('direccionDespacho').value.trim(), contacto = document.getElementById('contactoDespacho').value.trim(), celular = document.getElementById('celularDespacho').value.trim();
  if (!tipoEntrega) { alert('DEBE SELECCIONAR UN TIPO DE ENTREGA'); return; }
  if (!region) { alert('DEBE INGRESAR REGIÓN'); return; }
  if (!comuna) { alert('DEBE INGRESAR UNA COMUNA'); return; }
  if (!direccion) { alert('DEBE INGRESAR DIRECCIÓN'); return; }
  if (!contacto) { alert('DEBE INGRESAR CONTACTO DE DESPACHO'); return; }
  if (!celular) { alert('DEBE INGRESAR CELULAR DE CONTACTO'); return; }
  datosDespacho = { tipoEntrega: tipoEntrega, region: region.toUpperCase(), comuna: comuna.toUpperCase(), direccion: direccion.toUpperCase(), contacto: contacto.toUpperCase(), celular: celular, archivos: JSON.parse(JSON.stringify(archivosAdjuntos)) };
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
  alert('COTIZACIÓN ACEPTADA Y GUARDADA ✓');
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
  document.getElementById('btnBuscarCliente').disabled = true;
  document.getElementById('btnLimpiarCliente').disabled = false;
  document.getElementById('btnBuscarProducto').disabled = true;
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
  alert('COTIZACIÓN RECHAZADA Y GUARDADA ✓');
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
  document.getElementById('inputCodigoProducto').disabled = true;
  document.getElementById('inputRut').disabled = true;
  actualizarTablaProductos();
}

function verArchivos() {
  if (!datosDespacho || !datosDespacho.archivos || datosDespacho.archivos.length === 0) { alert('NO HAY ARCHIVOS ADJUNTOS'); return; }
  const modal = document.getElementById('modalArchivos'), lista = document.getElementById('listaArchivosModal');
  let html = '<div class="seccion-adjuntos" style="display:block;"><ul class="lista-adjuntos">';
  datosDespacho.archivos.forEach((archivo, index) => { html += `<li class="item-adjunto"><span class="nombre-archivo">${archivo.nombre} (${archivo.tamaño} KB)</span><button class="btn-ver-archivo" onclick="verArchivo(${index})">VER</button></li>`; });
  html += '</ul></div>';
  lista.innerHTML = html;
  modal.style.display = 'block';
}

function verArchivo(index) {
  if (!datosDespacho || !datosDespacho.archivos || index < 0 || index >= datosDespacho.archivos.length) { alert('ERROR: ARCHIVO NO ENCONTRADO'); return; }
  const archivo = datosDespacho.archivos[index], modalVisualizar = document.getElementById('modalVisualizarArchivo'), contenido = document.getElementById('contenidoArchivo');
  if (archivo.tipo.startsWith('image/')) {
    contenido.innerHTML = `<img src="${archivo.contenido}" alt="${archivo.nombre}" />`;
  } else if (archivo.tipo === 'application/pdf') {
    contenido.innerHTML = `<embed src="${archivo.contenido}" type="application/pdf" width="100%" height="100%" />`;
  } else if (archivo.tipo.startsWith('text/')) {
    const xhr = new XMLHttpRequest();
    xhr.responseType = 'arraybuffer';
    xhr.onload = function() { const texto = new TextDecoder().decode(xhr.response); contenido.innerHTML = `<pre>${texto}</pre>`; };
    xhr.onerror = function() { contenido.innerHTML = `<pre>No se pudo visualizar el contenido del archivo</pre>`; };
    xhr.open('GET', archivo.contenido);
    xhr.send();
  } else {
    contenido.innerHTML = `<p>Tipo de archivo no soportado para vista previa: ${archivo.tipo}</p>`;
  }
  modalVisualizar.style.display = 'block';
}

function cerrarModalArchivos() { document.getElementById('modalArchivos').style.display = 'none'; }

function cerrarModalVisualizarArchivo() { document.getElementById('modalVisualizarArchivo').style.display = 'none'; document.getElementById('contenidoArchivo').innerHTML = ''; }
</script>
</body>
</html>
