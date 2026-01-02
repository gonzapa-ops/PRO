<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Cotizador - CUNDO SPA</title>
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
.btn-editar-cot {background: #D9822B; color: white; padding: 4px 5px; font-size: 8px;}
.btn-editar-cot:hover {background: #b36e1e;}
.btn-eliminar-cot {background: #9B2E00; color: white; padding: 4px 5px; font-size: 8px; margin-left: 2px;}
.btn-eliminar-cot:hover {background: #7a2300;}
.btn-descargar-pdf {background: #2E7D32; color: white; padding: 6px 10px; font-size: 9px; margin-left: 5px;}
.btn-descargar-pdf:hover {background: #1a5e2c;}
.btn-ver-archivo {background: #4B732E; color: white; padding: 4px 5px; font-size: 8px;}
.btn-ver-archivo:hover {background: #385525;}
.formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none;}
.formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
.campo-grupo {margin-bottom: 12px;}
.campo-grupo label {display: block; font-weight: 700; color: #3B3B3B; margin-bottom: 4px; font-size: 11px; text-transform: uppercase;}
.campo-grupo input, .campo-grupo select, .campo-grupo textarea {width: 100%; padding: 8px; font-size: 11px; border: 2px solid #ddd; border-radius: 2px; text-transform: uppercase; font-family: inherit;}
.campo-grupo input:focus, .campo-grupo select:focus, .campo-grupo textarea:focus {outline: none; border-color: #F25C05;}
.campo-grupo input:disabled, .campo-grupo select:disabled, .campo-grupo textarea:disabled {background-color: #f0f0f0; cursor: not-allowed; color: #777;}
.campo-grupo input.validacion-correcta {border: 2px solid #4CAF50; background-color: #f1f8f6;}
.campo-grupo input.validacion-incorrecta {border: 2px solid #f44336; background-color: #fef5f5;}
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
.tabla-cotizaciones tr:nth-child(even) {background: #E9F0EA;}
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
.pdf-preview {border: 2px solid #ddd; border-radius: 8px; padding: 15px; margin: 15px 0; background: #f9f9f9; text-align: center;}
.pdf-preview iframe {width: 100%; height: 600px; border: 1px solid #ccc;}
</style>
</head>
<body>
  <div class="cotizador-container">
    <header class="cotizador-header">
      <div class="empresa-nombre">CUNDO SPA - COTIZADOR</div>
      <div class="numero-cotizacion">N° <span id="numeroCotizacion" style="color: white;">CO100500</span></div>
    </header>

    <div class="botones-superiores">
      <button class="btn btn-articulos" onclick="abrirArticulos()" id="btnArticulos">ARTICULOS</button>
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
          <button class="btn-lupa" onclick="abrirModalClientes()" title="Ver todos los clientes">👤</button>
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
          <div class="campo-grupo"><label>COSTO (SIN IVA) *</label><input type="number" id="costoProducto" min="0" step="0.01" /></div>
          <div class="campo-grupo"><label>PRECIO CON IVA *</label><input type="number" id="precioConIvaProducto" min="0" step="0.01" onchange="calcularPrecioNeto()" /></div>
          <div class="campo-grupo"><label>PRECIO NETO (SIN IVA)</label><input type="number" id="valorTotalProducto" disabled class="input-solo-lectura" /></div>
        </div>
        <div class="fila-campos-producto">
          <div class="campo-grupo"><label>% MARGEN NETO</label><input type="number" id="porcentajeProducto" disabled class="input-solo-lectura" /></div>
          <div class="campo-grupo"><label>UTILIDAD NETA (SIN IVA)</label><input type="number" id="utilidadNetaProducto" disabled class="input-solo-lectura" /></div>
        </div>
        <div class="campo-grupo"><label>LINK *</label><input type="text" id="linkProducto" style="text-transform: lowercase;" /></div>
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
        <button class="btn btn-pdf" onclick="generarPDF()" id="btnPDF">📄 GENERAR PDF</button>
        <button class="btn btn-descargar-pdf" onclick="descargarPDF()" id="btnDescargarPDF" style="display:none;">⬇️ DESCARGAR PDF</button>
      </div>
      <div id="previewPDF"></div>
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
      <div class="modal-titulo">ARTICULOS (PRODUCTOS Y SERVICIOS REGISTRADOS)</div>
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
      <div class="campo-grupo"><label>DIRECCIÓN *</label><input type="text" id="direccionDespacho" placeholder="Quilicura, Región Metropolitana, CL" value="Quilicura, Región Metropolitana, CL" /></div>
      <div class="campo-grupo"><label>REGIÓN *</label><input type="text" id="regionSelect" placeholder="Región Metropolitana" value="Región Metropolitana" /></div>
      <div class="campo-grupo"><label>COMUNA *</label><input type="text" id="comunaInput" placeholder="Quilicura" value="Quilicura" /></div>
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

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>

<script>
function calcularPrecioNeto() {
  const costo = parseFloat(document.getElementById('costoProducto').value) || 0;
  const precioConIva = parseFloat(document.getElementById('precioConIvaProducto').value) || 0;
  
  if (precioConIva <= 0) {
    document.getElementById('valorTotalProducto').value = '';
    document.getElementById('porcentajeProducto').value = '';
    document.getElementById('utilidadNetaProducto').value = '';
    return;
  }

  const precioNeto = +(precioConIva / 1.19).toFixed(2);
  document.getElementById('valorTotalProducto').value = precioNeto;

  if (costo > 0) {
    const utilidadNeta = +(precioNeto - costo).toFixed(2);
    const margenPorcentaje = precioNeto > 0 ? +((utilidadNeta / precioNeto) * 100).toFixed(2) : 0;
    
    document.getElementById('utilidadNetaProducto').value = utilidadNeta;
    document.getElementById('porcentajeProducto').value = margenPorcentaje;
    
    if (margenPorcentaje < 0) {
      alert('⚠️ MARGEN NEGATIVO: El costo es mayor que el precio neto');
    }
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
  document.getElementById('inputRut').addEventListener('input', function(e) {
    let v = e.target.value.replace(/[^0-9kK]/g, '');
    if (v.length > 1) v = v.slice(0, -1) + '-' + v.slice(-1);
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
  
  document.getElementById('costoProducto').addEventListener('change', calcularPrecioNeto);
  document.getElementById('precioConIvaProducto').addEventListener('change', calcularPrecioNeto);
  
  document.addEventListener('click', function(e) {
    if (!e.target.closest('.seccion-cliente')) document.getElementById('desplegableClientes').classList.remove('activo');
    if (!e.target.closest('.busqueda-producto')) document.getElementById('autocompleteLista').classList.remove('activo');
  });
});

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
  if (!validarRut(rut)) return mostrarMensaje('EL FORMATO DEL RUT NO ES VÁLIDO', 'error');
  const cliente = gestorClientes.buscarPorRut(rut);
  if (cliente) {
    clienteActual = cliente;
    document.getElementById('inputRut').classList.add('validacion-correcta');
    document.getElementById('inputRut').classList.remove('validacion-incorrecta');
    mostrarMensaje('✓ CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
    mostrarResumenCliente(cliente);
    document.getElementById('formularioCliente').classList.remove('activo');
    habilitarProductos();
  } else {
    document.getElementById('inputRut').classList.add('validacion-incorrecta');
    document.getElementById('inputRut').classList.remove('validacion-correcta');
    mostrarMensaje('✗ CLIENTE NO ENCONTRADO. COMPLETE LOS DATOS PARA CREAR.', 'info');
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

function validarRut(rut) { return /^[0-9]{7,8}-[0-9kK]$/.test(rut); }

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
  if (esLecturaCotizacion) { mostrarMensaje('MODO LECTURA. NO SE PUEDEN HACER MODIFICACIONES.', 'bloqueado'); return; }
  const rut = document.getElementById('rut').value, rs = document.getElementById('razonSocial').value.trim(), gi = document.getElementById('giro').value.trim(), di = document.getElementById('direccion').value.trim(), nc = document.getElementById('nombreContacto').value.trim(), ce = document.getElementById('celular').value.trim(), ma = document.getElementById('mail').value.trim(), mp = document.getElementById('medioPago').value;
  if (!rs || !gi || !di || !nc || !ce || !ma || !mp) return mostrarMensaje('COMPLETE TODOS LOS CAMPOS', 'error');
  if (!validarEmail(ma)) return mostrarMensaje('EMAIL INVÁLIDO', 'error');
  const cliente = { rut, razonSocial: rs.toUpperCase(), giro: gi.toUpperCase(), direccion: di.toUpperCase(), nombreContacto: nc.toUpperCase(), celular: ce, mail: ma.toUpperCase(), medioPago: mp };
  if (modoEdicion) gestorClientes.actualizarCliente(rut, cliente);
  else gestorClientes.agregarCliente(rut, cliente);
  mostrarMensaje(modoEdicion ? 'CLIENTE ACTUALIZADO' : 'CLIENTE GUARDADO', 'exito');
  clienteActual = cliente;
  modoEdicion = false;
  document.getElementById('formularioCliente').classList.remove('activo');
  mostrarResumenCliente(cliente);
  habilitarProductos();
}

function validarEmail(e) { return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(e); }

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

function bloquearEdicion() {
  document.getElementById('inputRut').disabled = true;
  document.getElementById('btnBuscarCliente').disabled = true;
  document.getElementById('btnLimpiarCliente').disabled = true;
  document.getElementById('inputCodigoProducto').disabled = true;
  document.getElementById('btnBuscarProducto').disabled = true;
  document.getElementById('btnPDF').disabled = true;
  const tabla = document.querySelector('table');
  if (tabla) {
    const inputs = tabla.querySelectorAll('input[type="number"]');
    inputs.forEach(input => input.disabled = true);
  }
  const botonesEliminar = document.querySelectorAll('.btn-eliminar');
  botonesEliminar.forEach(btn => btn.style.display = 'none');
  document.getElementById('seccionBloqueada').classList.add('activa');
}

function limpiarCotizacion() {
  document.getElementById('inputRut').value = '';
  document.getElementById('inputRut').classList.remove('validacion-correcta', 'validacion-incorrecta');
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
  document.getElementById('btnBuscarCliente').disabled = false;
  document.getElementById('btnLimpiarCliente').disabled = false;
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
  document.getElementById('resumenDespacho').classList.remove('activo');
  document.getElementById('resumenCompra').classList.remove('activo');
  document.getElementById('seccionCierre').classList.remove('activo');
  document.getElementById('previewPDF').innerHTML = '';
  document.getElementById('btnDescargarPDF').style.display = 'none';
  document.getElementById('btnPDF').disabled = false;
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
  const inputProducto = document.getElementById('inputCodigoProducto');
  if (!cod) return mostrarMensajeProducto('INGRESE UN CÓDIGO O DESCRIPCIÓN', 'error');
  const prod = gestorProductos.buscarPorCodigo(cod);
  if (prod) {
    inputProducto.classList.add('validacion-correcta');
    inputProducto.classList.remove('validacion-incorrecta');
    mostrarMensajeProducto('✓ PRODUCTO ENCONTRADO', 'exito');
    document.getElementById('formularioProducto').style.display = 'none';
    agregarProductoACotizacion(cod, prod);
    document.getElementById('inputCodigoProducto').value = '';
  } else {
    inputProducto.classList.add('validacion-incorrecta');
    inputProducto.classList.remove('validacion-correcta');
    mostrarMensajeProducto('✗ NO ENCONTRADO. CREE UNO NUEVO.', 'info');
    document.getElementById('formularioProducto').style.display = 'block';
    document.getElementById('codigoProducto').value = cod;
    document.getElementById('descripcionProducto').value = '';
    document.getElementById('costoProducto').value = '';
    document.getElementById('precioConIvaProducto').value = '';
    document.getElementById('valorTotalProducto').value = '';
    document.getElementById('porcentajeProducto').value = '';
    document.getElementById('utilidadNetaProducto').value = '';
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
  const cod = document.getElementById('codigoProducto').value.trim(), desc = document.getElementById('descripcionProducto').value.trim(), cos = parseFloat(document.getElementById('costoProducto').value), precioNeto = parseFloat(document.getElementById('valorTotalProducto').value), precioConIva = parseFloat(document.getElementById('precioConIvaProducto').value), por = parseFloat(document.getElementById('porcentajeProducto').value) || 0, util = parseFloat(document.getElementById('utilidadNetaProducto').value) || 0, link = document.getElementById('linkProducto').value.trim().toLowerCase();
  if (!cod || !desc || isNaN(cos) || cos <= 0 || isNaN(precioConIva) || precioConIva <= 0 || !link) return mostrarMensajeProducto('COMPLETE TODOS LOS CAMPOS REQUERIDOS', 'error');
  if (precioNeto < cos) return mostrarMensajeProducto('⚠️ ERROR: PRECIO NETO DEBE SER ≥ COSTO', 'error');
  const prod = { codigo: cod, descripcion: desc.toUpperCase(), costo: +(cos.toFixed(2)), precioNeto: +(precioNeto.toFixed(2)), precioConIva: +(precioConIva.toFixed(2)), porcentaje: +(por.toFixed(2)), utilidad: +(util.toFixed(2)), link: link };
  gestorProductos.agregarProducto(cod, prod);
  mostrarMensajeProducto('PRODUCTO GUARDADO CORRECTAMENTE', 'exito');
  cancelarProducto();
  agregarProductoACotizacion(cod, prod);
}

function cancelarProducto() {
  document.getElementById('formularioProducto').style.display = 'none';
  document.getElementById('codigoProducto').value = '';
  document.getElementById('descripcionProducto').value = '';
  document.getElementById('costoProducto').value = '';
  document.getElementById('precioConIvaProducto').value = '';
  document.getElementById('valorTotalProducto').value = '';
  document.getElementById('porcentajeProducto').value = '';
  document.getElementById('utilidadNetaProducto').value = '';
  document.getElementById('linkProducto').value = '';
  document.getElementById('inputCodigoProducto').value = '';
  document.getElementById('inputCodigoProducto').classList.remove('validacion-correcta', 'validacion-incorrecta');
  document.getElementById('mensajeProducto').style.display = 'none';
}

function agregarProductoACotizacion(cod, prod) {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensaje('MODO LECTURA. NO SE PUEDEN AGREGAR PRODUCTOS.', 'bloqueado'); return; }
  const ex = productosEnCotizacion.find(p => p.codigo === cod);
  if (ex) {
    ex.cantidad++;
    ex.totalNeto = +(ex.cantidad * ex.precioNetoConDescuento).toFixed(2);
  } else {
    productosEnCotizacion.push({
      codigo: prod.codigo,
      descripcion: prod.descripcion,
      cantidad: 1,
      precioNeto: prod.precioNeto,
      costo: prod.costo,
      descuento: 0,
      precioNetoConDescuento: prod.precioNeto,
      totalNeto: +(prod.precioNeto.toFixed(2)),
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
    return;
  }
  let html = '<div style="overflow-x:auto;"><table><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANT</th><th>PRECIO UNITARIO (NETO)</th><th>DESC(%)</th><th>PRECIO DESC (NETO)</th><th>COSTO UNIT</th><th>COSTO TOTAL</th><th>TOTAL NETO</th><th>% MARGEN NETO</th><th>UTILIDAD NETA</th><th>TOTAL CON IVA</th><th>ACCIÓN</th></tr></thead><tbody>';
  productosEnCotizacion.forEach((p, i) => {
    const bloqueado = esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado';
    const inputQuantityDisabled = bloqueado ? 'disabled' : '';
    const inputDescuentoDisabled = bloqueado ? 'disabled' : '';
    const btnEliminarDisplay = bloqueado ? 'none' : 'block';

    const costoTotal = +(parseFloat(p.costo) * p.cantidad).toFixed(2);
    const precioUnitarioNeto = +(parseFloat(p.precioNetoConDescuento)).toFixed(2);
    const totalNeto = +(p.totalNeto).toFixed(2);
    const utilidadNeta = +(totalNeto - costoTotal).toFixed(2);
    const margenPorcentaje = totalNeto > 0 ? +((utilidadNeta / totalNeto) * 100).toFixed(2) : 0;
    const totalConIva = +(totalNeto * 1.19).toFixed(2);
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
      <td class="valor-numerico ${claseMargen}">${margenPorcentaje}%</td>
      <td class="valor-numerico">$${utilidadNeta.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td class="valor-numerico">$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
      <td><button class="btn-eliminar" onclick="eliminarProducto(${i})" style="display:${btnEliminarDisplay};width:100%;">DEL</button></td>
    </tr>`;
  });
  html += '</tbody></table></div>';
  cont.innerHTML = html;
  actualizarResumenTotales();
}

function actualizarCantidad(i, cant) {
  cant = parseInt(cant);
  if (isNaN(cant) || cant < 1) { alert('⚠️ CANTIDAD INVÁLIDA - DEBE SER MAYOR A 0'); actualizarTablaProductos(); return; }
  productosEnCotizacion[i].cantidad = cant;
  productosEnCotizacion[i].totalNeto = +(productosEnCotizacion[i].precioNetoConDescuento * cant).toFixed(2);
  actualizarTablaProductos();
}

function actualizarDescuento(i, desc) {
  desc = parseFloat(desc) || 0;
  if (desc < 0 || desc > 100) { alert('⚠️ ERROR: EL DESCUENTO DEBE ESTAR ENTRE 0% Y 100%'); actualizarTablaProductos(); return; }
  productosEnCotizacion[i].descuento = desc;
  const precioConDesc = productosEnCotizacion[i].precioNeto * (1 - desc / 100);
  productosEnCotizacion[i].precioNetoConDescuento = +(precioConDesc.toFixed(2));
  productosEnCotizacion[i].totalNeto = +(productosEnCotizacion[i].precioNetoConDescuento * productosEnCotizacion[i].cantidad).toFixed(2);
  actualizarTablaProductos();
}

function eliminarProducto(i) {
  if (esLecturaCotizacion || estadoCotizacionActual === 'aceptado' || estadoCotizacionActual === 'rechazado') { mostrarMensaje('MODO LECTURA. NO SE PUEDEN ELIMINAR PRODUCTOS.', 'bloqueado'); return; }
  productosEnCotizacion.splice(i, 1);
  actualizarTablaProductos();
}

function actualizarResumenTotales() {
  const totalNeto = +(productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0)).toFixed(2);
  const iva = +(totalNeto * 0.19).toFixed(2);
  const totalConIva = +(totalNeto + iva).toFixed(2);

  document.getElementById('totalNeto').textContent = '$' + totalNeto.toLocaleString('es-CL', {minimumFractionDigits: 2});
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
  const todos = gestorProductos.obtenerTodos();
  let html = '<div style="overflow-x:auto;"><table class="tabla-articulos"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>COSTO (SIN IVA)</th><th>PRECIO NETO (SIN IVA)</th><th>% MARGEN NETO</th><th>UTILIDAD NETA</th><th>TOTAL CON IVA</th><th>LINK</th><th>ACCIÓN</th></tr></thead><tbody>';
  const claves = Object.keys(todos);
  if (claves.length === 0) {
    html += '<tr><td colspan="9" style="text-align:center;">NO HAY ARTÍCULOS</td></tr>';
  } else {
    claves.forEach(codigo => {
      const art = todos[codigo], 
            utilidadNeta = +(parseFloat(art.precioNeto) - parseFloat(art.costo)).toFixed(2),
            margenPorcentaje = art.precioNeto > 0 ? +((utilidadNeta / art.precioNeto) * 100).toFixed(2) : 0,
            totalConIva = +(parseFloat(art.precioNeto) * 1.19).toFixed(2);
      html += `<tr>
        <td>${art.codigo}</td>
        <td>${art.descripcion}</td>
        <td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">$${parseFloat(art.precioNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">${margenPorcentaje.toLocaleString('es-CL', {minimumFractionDigits: 2})}%</td>
        <td class="valor-numerico">$${utilidadNeta.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td class="valor-numerico">$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 2})}</td>
        <td style="text-transform:lowercase;"><a href="${art.link}" target="_blank">${art.link}</a></td>
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
      <div class="campo-grupo"><label>COSTO (SIN IVA)</label><input type="number" id="editarCosto" value="${art.costo}" onchange="calcularMargenEdicion()"></div>
      <div class="campo-grupo"><label>PRECIO CON IVA</label><input type="number" id="editarPrecioConIva" value="${(art.precioNeto * 1.19).toFixed(2)}" onchange="calcularDesdeIvaEdicion()"></div>
      <div class="campo-grupo"><label>PRECIO NETO (SIN IVA)</label><input type="number" id="editarPrecioNeto" value="${art.precioNeto}" disabled class="input-solo-lectura" onchange="calcularMargenEdicion()"></div>
    </div>
    <div class="fila-campos-producto">
      <div class="campo-grupo"><label>% MARGEN NETO</label><input type="number" id="editarPorcentaje" value="${art.porcentaje}" disabled class="input-solo-lectura"></div>
      <div class="campo-grupo"><label>UTILIDAD NETA (SIN IVA)</label><input type="number" id="editarUtilidad" value="${art.utilidad}" disabled class="input-solo-lectura"></div>
    </div>
    <div class="campo-grupo"><label>LINK</label><input type="text" id="editarLink" value="${art.link}" style="text-transform: lowercase;"></div>
    <div class="botones-producto">
      <button class="btn btn-agregar" onclick="guardarEdicionArticulo()">GUARDAR</button>
      <button class="btn btn-cancelar" onclick="cancelarEdicionArticulo()">CANCELAR</button>
    </div>`;
}

function calcularDesdeIvaEdicion() {
  const precioConIva = parseFloat(document.getElementById('editarPrecioConIva').value) || 0;
  const precioNeto = +(precioConIva / 1.19).toFixed(2);
  document.getElementById('editarPrecioNeto').value = precioNeto;
  calcularMargenEdicion();
}

function calcularMargenEdicion() {
  const c = parseFloat(document.getElementById('editarCosto').value) || 0;
  const pn = parseFloat(document.getElementById('editarPrecioNeto').value) || 0;
  if (c > 0 && pn > 0) {
    if (pn < c) { document.getElementById('editarPorcentaje').value = '0.00'; document.getElementById('editarUtilidad').value = '0.00'; return; }
    const utilidad = +(pn - c).toFixed(2);
    const margen = +((utilidad / pn) * 100).toFixed(2);
    document.getElementById('editarPorcentaje').value = margen;
    document.getElementById('editarUtilidad').value = utilidad;
  } else {
    document.getElementById('editarPorcentaje').value = '0.00';
    document.getElementById('editarUtilidad').value = '0.00';
  }
}

function guardarEdicionArticulo() {
  if (!articuloEdicion) return;
  const codigo = articuloEdicion.codigo, desc = document.getElementById('editarDescripcion').value.trim().toUpperCase(), cos = parseFloat(document.getElementById('editarCosto').value) || 0, pn = parseFloat(document.getElementById('editarPrecioNeto').value) || 0, por = parseFloat(document.getElementById('editarPorcentaje').value) || 0, util = parseFloat(document.getElementById('editarUtilidad').value) || 0, precioConIva = parseFloat(document.getElementById('editarPrecioConIva').value) || 0, link = document.getElementById('editarLink').value.trim().toLowerCase();
  if (!desc || !link) { alert('COMPLETE TODOS LOS CAMPOS'); return; }
  if (pn < cos) { alert('⚠️ ERROR: PRECIO NETO (SIN IVA) DEBE SER ≥ COSTO'); return; }
  const nuevo = { codigo, descripcion: desc, costo: +(cos.toFixed(2)), precioNeto: +(pn.toFixed(2)), precioConIva: +(precioConIva.toFixed(2)), porcentaje: +(por.toFixed(2)), utilidad: +(util.toFixed(2)), link: link };
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
  const totalNeto = +(productosEnCotizacion.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0)).toFixed(2);
  const estado = datosDespacho ? 'aceptado' : 'pendiente';
  const cotizacion = {
    numero: numCot,
    razonSocial: clienteActual.razonSocial,
    totalNeto: totalNeto,
    fecha: new Date().toISOString(),
    cliente: JSON.parse(JSON.stringify(clienteActual)),
    productos: JSON.parse(JSON.stringify(productosEnCotizacion)),
    estado: estado,
    despacho: datosDespacho || null
  };
  
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
  document.getElementById('btnDescargarPDF').style.display = 'inline-block';
  generarPDFDocumento(cotizacion);
}

function generarPDFDocumento(cotizacion) {
  try {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
    
    const colorPrincipal = [31, 111, 139];
    const colorSecundario = [242, 92, 5];
    const colorGris = [100, 100, 100];
    
    let yPos = 10;
    
    // ===== HEADER PROFESIONAL =====
    doc.setFillColor(31, 111, 139);
    doc.rect(0, 0, 210, 40, 'F');
    
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(24);
    doc.setFont(undefined, 'bold');
    doc.text('CUNDO SPA', 15, 18);
    
    doc.setFontSize(10);
    doc.setFont(undefined, 'normal');
    doc.text('Soluciones Comerciales Integrales', 15, 26);
    doc.text('www.cundospa.cl | info@cundospa.cl', 15, 32);
    
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(12);
    doc.setFont(undefined, 'bold');
    doc.text('COTIZACIÓN', 140, 18);
    
    doc.setFontSize(11);
    doc.setFont(undefined, 'bold');
    doc.text(`#${cotizacion.numero}`, 140, 26);
    
    doc.setFontSize(9);
    doc.setFont(undefined, 'normal');
    const fechaEmision = new Date(cotizacion.fecha);
    const fechaFormato = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
    doc.text(`Fecha: ${fechaFormato}`, 140, 33);
    
    yPos = 45;
    
    // ===== SECCIÓN CLIENTE =====
    doc.setFillColor(242, 92, 5);
    doc.rect(15, yPos - 5, 180, 7, 'F');
    
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(11);
    doc.setFont(undefined, 'bold');
    doc.text('INFORMACIÓN DEL CLIENTE', 17, yPos);
    
    yPos += 10;
    
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(9);
    
    const cliente = cotizacion.cliente;
    
    // Información cliente en dos columnas
    doc.setFont(undefined, 'bold');
    doc.text('RUT:', 15, yPos);
    doc.setFont(undefined, 'normal');
    doc.text(cliente.rut, 35, yPos);
    
    doc.setFont(undefined, 'bold');
    doc.text('CONTACTO:', 15, yPos + 6);
    doc.setFont(undefined, 'normal');
    doc.text(cliente.nombreContacto, 35, yPos + 6);
    
    doc.setFont(undefined, 'bold');
    doc.text('CELULAR:', 15, yPos + 12);
    doc.setFont(undefined, 'normal');
    doc.text(cliente.celular, 35, yPos + 12);
    
    doc.setFont(undefined, 'bold');
    doc.text('RAZÓN SOCIAL:', 105, yPos);
    doc.setFont(undefined, 'normal');
    doc.text(cliente.razonSocial.substring(0, 50), 135, yPos);
    
    doc.setFont(undefined, 'bold');
    doc.text('EMAIL:', 105, yPos + 6);
    doc.setFont(undefined, 'normal');
    doc.text(cliente.mail.substring(0, 40), 135, yPos + 6);
    
    doc.setFont(undefined, 'bold');
    doc.text('PAGO:', 105, yPos + 12);
    doc.setFont(undefined, 'normal');
    doc.text(cliente.medioPago, 135, yPos + 12);
    
    yPos += 22;
    
    // Dirección
    doc.setFont(undefined, 'bold');
    doc.setFontSize(8);
    doc.text('DIRECCIÓN:', 15, yPos);
    doc.setFont(undefined, 'normal');
    const direccion = cliente.direccion.substring(0, 70);
    doc.text(direccion, 15, yPos + 4);
    
    yPos += 12;
    
    // ===== TABLA PRODUCTOS MEJORADA =====
    const columnasProductos = [
      { header: 'COD', dataKey: 'codigo', halign: 'center', width: 12 },
      { header: 'DESCRIPCIÓN', dataKey: 'descripcion', halign: 'left', width: 40 },
      { header: 'CANT', dataKey: 'cantidad', halign: 'center', width: 10 },
      { header: 'V.UNI', dataKey: 'precioNeto', halign: 'right', width: 18 },
      { header: 'DESC%', dataKey: 'descuento', halign: 'center', width: 10 },
      { header: 'SUBTOTAL', dataKey: 'totalNeto', halign: 'right', width: 22 },
      { header: '%MARGEN', dataKey: 'margen', halign: 'center', width: 14 }
    ];
    
    const filasProductos = cotizacion.productos.map(p => {
      const desc = p.descripcion.length > 35 ? p.descripcion.substring(0, 35) + '..' : p.descripcion;
      const costoTotal = +(parseFloat(p.costo) * p.cantidad).toFixed(2);
      const utilidadNeta = +(p.totalNeto - costoTotal).toFixed(2);
      const margen = p.totalNeto > 0 ? +((utilidadNeta / p.totalNeto) * 100).toFixed(1) : 0;
      
      return {
        codigo: p.codigo,
        descripcion: desc,
        cantidad: p.cantidad.toString(),
        precioNeto: `$${parseFloat(p.precioNetoConDescuento).toLocaleString('es-CL', {minimumFractionDigits: 0})}`,
        descuento: p.descuento > 0 ? `${p.descuento}%` : '-',
        totalNeto: `$${parseFloat(p.totalNeto).toLocaleString('es-CL', {minimumFractionDigits: 0})}`,
        margen: `${margen}%`
      };
    });
    
    doc.autoTable({
      columns: columnasProductos,
      body: filasProductos,
      startY: yPos,
      margin: { left: 15, right: 15 },
      styles: { 
        fontSize: 8, 
        cellPadding: 3.5, 
        overflow: 'linebreak',
        valign: 'middle'
      },
      headStyles: { 
        fillColor: colorPrincipal, 
        textColor: [255, 255, 255], 
        fontStyle: 'bold',
        fontSize: 8
      },
      bodyStyles: { textColor: [0, 0, 0] },
      alternateRowStyles: { fillColor: [245, 248, 250] },
      didDrawPage: (data) => { 
        if (data && data.lastAutoTable) {
          yPos = data.lastAutoTable.finalY + 5;
        }
      }
    });
    
    // ===== TOTALES PROFESIONALES =====
    const totalNeto = cotizacion.productos.reduce((acc, p) => acc + parseFloat(p.totalNeto), 0);
    const costoTotalGeneral = cotizacion.productos.reduce((acc, p) => {
      const costoTotal = parseFloat(p.costo) * p.cantidad;
      return acc + costoTotal;
    }, 0);
    const utilidadTotalGeneral = +(totalNeto - costoTotalGeneral).toFixed(2);
    const totalIva = +(totalNeto * 0.19).toFixed(2);
    const totalConIva = +(totalNeto + totalIva).toFixed(2);
    
    yPos += 3;
    
    // Fondo para sección de totales
    doc.setDrawColor(240, 240, 240);
    doc.setLineWidth(0.3);
    doc.rect(15, yPos - 2, 180, 28, 'S');
    
    doc.setFontSize(9);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(80, 80, 80);
    
    // Fila 1: Subtotal
    doc.text('SUBTOTAL (SIN IVA):', 20, yPos + 2);
    doc.setFont(undefined, 'bold');
    doc.text(`$${totalNeto.toLocaleString('es-CL', {minimumFractionDigits: 0})}`, 190, yPos + 2, { align: 'right' });
    
    // Fila 2: Costo Total
    doc.setFont(undefined, 'normal');
    doc.setTextColor(80, 80, 80);
    doc.text('COSTO TOTAL:', 20, yPos + 8);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(180, 0, 0);
    doc.text(`$${costoTotalGeneral.toLocaleString('es-CL', {minimumFractionDigits: 0})}`, 190, yPos + 8, { align: 'right' });
    
    // Fila 3: Utilidad Neta
    doc.setFont(undefined, 'normal');
    doc.setTextColor(80, 80, 80);
    doc.text('UTILIDAD NETA:', 20, yPos + 14);
    doc.setFont(undefined, 'bold');
    doc.setTextColor(0, 120, 0);
    doc.text(`$${utilidadTotalGeneral.toLocaleString('es-CL', {minimumFractionDigits: 0})}`, 190, yPos + 14, { align: 'right' });
    
    // Fila 4: IVA
    doc.setFont(undefined, 'normal');
    doc.setTextColor(80, 80, 80);
    doc.text('IVA (19%):', 20, yPos + 20);
    doc.setFont(undefined, 'bold');
    doc.text(`$${totalIva.toLocaleString('es-CL', {minimumFractionDigits: 0})}`, 190, yPos + 20, { align: 'right' });
    
    yPos += 28;
    
    // TOTAL DESTACADO
    doc.setFillColor(242, 92, 5);
    doc.rect(15, yPos - 2, 180, 10, 'F');
    
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(10);
    doc.setFont(undefined, 'bold');
    doc.text('TOTAL CON IVA:', 20, yPos + 3);
    doc.setFontSize(13);
    doc.text(`$${totalConIva.toLocaleString('es-CL', {minimumFractionDigits: 0})}`, 190, yPos + 3, { align: 'right' });
    
    yPos += 15;
    
    // ===== TÉRMINOS Y CONDICIONES =====
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(9);
    doc.setFont(undefined, 'bold');
    doc.text('TÉRMINOS Y CONDICIONES:', 15, yPos);
    
    yPos += 5;
    doc.setFontSize(7.5);
    doc.setFont(undefined, 'normal');
    doc.setTextColor(60, 60, 60);
    
    const terminos = [
      '• Cotización válida por 30 días desde su emisión.',
      '• Precios sujetos a cambios sin previo aviso.',
      '• Requiere confirmación para proceder con despacho.',
      '• Condiciones de pago según lo pactado.'
    ];
    
    terminos.forEach(termino => {
      if (yPos > 270) {
        doc.addPage();
        yPos = 15;
      }
      doc.text(termino, 17, yPos);
      yPos += 4;
    });
    
    yPos += 3;
    
    // LÍNEA FINAL
    doc.setDrawColor(200, 200, 200);
    doc.setLineWidth(0.5);
    doc.line(15, yPos, 195, yPos);
    
    yPos += 5;
    
    // PIE DE PÁGINA
    doc.setFontSize(7);
    doc.setTextColor(150, 150, 150);
    doc.setFont(undefined, 'italic');
    doc.text('Documento generado automáticamente por CUNDO SPA - Cotizador', 105, yPos, { align: 'center' });
    
    pdfActualDoc = doc;
    mostrarPreviewPDF(doc);
    mostrarMensaje('✓ PDF GENERADO CORRECTAMENTE', 'exito');
    
  } catch (error) {
    console.error('Error en generación PDF:', error);
    mostrarMensaje('ERROR AL GENERAR PDF: ' + error.message, 'error');
  }
}

function mostrarPreviewPDF(doc) {
  const pdfString = doc.output('datauristring');
  const previewDiv = document.getElementById('previewPDF');
  previewDiv.className = 'pdf-preview';
  previewDiv.innerHTML = `<iframe src="${pdfString}" type="application/pdf"></iframe>`;
}

function descargarPDF() {
  if (!pdfActualDoc) { alert('PRIMERO GENERA EL PDF'); return; }
  pdfActualDoc.save(`CUNDO_COTIZACION_${numeroCotizacionActual}.pdf`);
  mostrarMensaje('✓ PDF DESCARGADO CORRECTAMENTE', 'exito');
}

function mostrarCotizaciones() {
  const modal = document.getElementById('modalCotizaciones'), cont = document.getElementById('listaCotizaciones');
  if (cotizacionesEmitidas.length === 0) {
    cont.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 15px; font-size:11px;">NO HAY COTIZACIONES EMITIDAS</p>';
  } else {
    let html = '<table class="tabla-cotizaciones"><thead><tr><th>N° COTIZACIÓN</th><th>RAZÓN SOCIAL</th><th style="text-align:right;">MONTO NETO</th><th>FECHA EMISIÓN</th><th>ESTADO</th><th style="text-align:center;">ACCIONES</th></tr></thead><tbody>';
    cotizacionesEmitidas.forEach((c, index) => {
      const claseEstado = c.estado === 'aceptado' ? 'cot-aceptado' : (c.estado === 'rechazado' ? 'cot-rechazado' : 'cot-pendiente');
      const fechaEmision = new Date(c.fecha);
      const fechaFormateada = `${fechaEmision.getDate().toString().padStart(2, '0')}/${(fechaEmision.getMonth() + 1).toString().padStart(2, '0')}/${fechaEmision.getFullYear()}`;
      const badgeClase = c.estado === 'aceptado' ? 'badge-aceptado' : (c.estado === 'rechazado' ? 'badge-rechazado' : 'badge-pendiente');
      const btnEditarDisabled = (c.estado === 'rechazado') ? 'disabled' : '';
      html += `<tr class="${claseEstado}"><td>${c.numero}</td><td>${c.razonSocial}</td><td style="text-align:right;">$${parseFloat(c.totalNeto).toLocaleString('es-CL', {minimumFractionDigits: 2})}</td><td>${fechaFormateada}</td><td><span class="badge-estado ${badgeClase}">${c.estado}</span></td><td style="text-align:center;"><button class="btn-ver" onclick="verCotizacion(${index})">VER</button><button class="btn-editar-cot" onclick="editarCotizacionGuardada(${index})" ${btnEditarDisabled}>EDITAR</button><button class="btn-eliminar-cot" onclick="eliminarCotizacion(${index})">ELIMINAR</button></td></tr>`;
    });
    html += '</tbody></table>';
    cont.innerHTML = html;
  }
  modal.style.display = 'block';
}

function verCotizacion(index) { if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; } generarPDFDocumento(cotizacionesEmitidas[index]); }

function eliminarCotizacion(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  if (confirm(`¿ESTÁ SEGURO DE ELIMINAR LA COTIZACIÓN ${cotizacionesEmitidas[index].numero}?`)) {
    cotizacionesEmitidas.splice(index, 1);
    localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
    alert('COTIZACIÓN ELIMINADA CORRECTAMENTE');
    mostrarCotizaciones();
  }
}

function editarCotizacionGuardada(index) {
  if (index < 0 || index >= cotizacionesEmitidas.length) { alert('ERROR: COTIZACIÓN NO ENCONTRADA'); return; }
  const cotizacion = cotizacionesEmitidas[index];
  if (cotizacion.estado === 'rechazado') { mostrarMensaje('COTIZACIÓN RECHAZADA. NO SE PUEDE EDITAR.', 'bloqueado'); return; }
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
    document.getElementById('btnRechazado').disabled = true;
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

function marcarAceptado() { document.getElementById('modalAceptado').style.display = 'block'; }

function cerrarModalAceptado() { document.getElementById('modalAceptado').style.display = 'none'; }

function abrirSelectorArchivos() { document.getElementById('inputArchivo').click(); }

function agregarArchivo(event) {
  const archivo = event.target.files[0];
  if (!archivo) return;
  const reader = new FileReader();
  reader.onload = function(e) {
    const nombreArchivo = archivo.name;
    const contenidoBase64 = e.target.result;
    const objetoArchivo = { nombre: nombreArchivo, contenido: contenidoBase64, tipo: archivo.type };
    archivosAdjuntos.push(objetoArchivo);
    document.getElementById('infoArchivo').textContent = `✓ ${nombreArchivo} cargado`;
    mostrarListaAdjuntos();
    document.getElementById('adjuntosContainer').style.display = 'block';
  };
  reader.readAsDataURL(archivo);
}

function mostrarListaAdjuntos() {
  const lista = document.getElementById('listaAdjuntos');
  lista.innerHTML = '';
  archivosAdjuntos.forEach((archivo, index) => {
    const item = document.createElement('li');
    item.className = 'item-adjunto';
    item.innerHTML = `<span class="nombre-archivo">${archivo.nombre}</span><button class="btn-eliminar-archivo" onclick="eliminarArchivo(${index})">ELIMINAR</button>`;
    lista.appendChild(item);
  });
}

function eliminarArchivo(index) {
  archivosAdjuntos.splice(index, 1);
  mostrarListaAdjuntos();
  if (archivosAdjuntos.length === 0) {
    document.getElementById('adjuntosContainer').style.display = 'none';
    document.getElementById('infoArchivo').textContent = '';
  }
}

function confirmarAceptacion() {
  const tipoEntrega = document.getElementById('tipoEntrega').value, direccion = document.getElementById('direccionDespacho').value.trim(), region = document.getElementById('regionSelect').value.trim(), comuna = document.getElementById('comunaInput').value.trim(), contacto = document.getElementById('contactoDespacho').value.trim(), celular = document.getElementById('celularDespacho').value.trim();
  if (!tipoEntrega || !direccion || !region || !comuna || !contacto || !celular) return alert('COMPLETE TODOS LOS CAMPOS REQUERIDOS');
  datosDespacho = { tipoEntrega, direccion, region, comuna, contacto, celular, archivos: JSON.parse(JSON.stringify(archivosAdjuntos)) };
  generarPDF();
  mostrarResumenDespacho();
  mostrarResumenCompra();
  document.getElementById('btnVerArchivos').style.display = 'inline-block';
  cerrarModalAceptado();
}

function mostrarResumenDespacho() {
  if (!datosDespacho) return;
  const resumen = document.getElementById('resumenDespacho');
  resumen.className = 'resumen-despacho activo';
  resumen.innerHTML = `<h4>✓ DATOS DE DESPACHO CONFIRMADOS</h4><p><strong>TIPO DE ENTREGA:</strong> ${datosDespacho.tipoEntrega}</p><p><strong>DIRECCIÓN:</strong> ${datosDespacho.direccion}</p><p><strong>REGIÓN:</strong> ${datosDespacho.region}</p><p><strong>COMUNA:</strong> ${datosDespacho.comuna}</p><p><strong>CONTACTO:</strong> ${datosDespacho.contacto}</p><p><strong>CELULAR:</strong> ${datosDespacho.celular}</p>`;
}

function mostrarResumenCompra() {
  if (!datosDespacho) return;
  const resumen = document.getElementById('resumenCompra');
  resumen.className = 'resumen-compra activo';
  let html = `<h4>DETALLE DE COMPRA CONFIRMADA</h4><table class="tabla-compra"><thead><tr><th>CÓDIGO</th><th>DESCRIPCIÓN</th><th>CANTIDAD</th><th>PRECIO UNI</th><th>TOTAL</th><th>PROVEEDOR</th></tr></thead><tbody>`;
  productosEnCotizacion.forEach(p => {
    html += `<tr><td>${p.codigo}</td><td>${p.descripcion.substring(0, 20)}</td><td>${p.cantidad}</td><td>$${parseFloat(p.precioNetoConDescuento).toLocaleString('es-CL', {minimumFractionDigits: 0})}</td><td>$${parseFloat(p.totalNeto).toLocaleString('es-CL', {minimumFractionDigits: 0})}</td><td><a href="${p.link}" target="_blank">Ver proveedor</a></td></tr>`;
  });
  html += '</tbody></table>';
  resumen.innerHTML = html;
}

function marcarRechazado() {
  if (confirm('¿ESTÁ SEGURO DE RECHAZAR ESTA COTIZACIÓN?')) {
    estadoCotizacionActual = 'rechazado';
    if (esEdicionCotizacion && cotizacionActualIndex !== null) {
      cotizacionesEmitidas[cotizacionActualIndex].estado = 'rechazado';
      localStorage.setItem('cotizacionesEmitidas', JSON.stringify(cotizacionesEmitidas));
    }
    mostrarMensaje('COTIZACIÓN RECHAZADA CORRECTAMENTE', 'exito');
    document.getElementById('btnAceptado').disabled = true;
    document.getElementById('btnRechazado').disabled = true;
    document.getElementById('seccionBloqueada').classList.add('activa');
    limpiarCotizacion();
  }
}

function verArchivos() {
  const modal = document.getElementById('modalArchivos'), listado = document.getElementById('listaArchivosModal');
  if (!datosDespacho || datosDespacho.archivos.length === 0) {
    listado.innerHTML = '<p style="text-transform:uppercase; text-align:center; padding: 15px; font-size:11px;">NO HAY ARCHIVOS ADJUNTOS</p>';
  } else {
    let html = '<div style="overflow-x:auto;"><table class="tabla-clientes-modal"><thead><tr><th>NOMBRE ARCHIVO</th><th>ACCIÓN</th></tr></thead><tbody>';
    datosDespacho.archivos.forEach((archivo, index) => {
      html += `<tr><td>${archivo.nombre}</td><td><button class="btn-ver-archivo" onclick="visualizarArchivo(${index})">VER</button></td></tr>`;
    });
    html += '</tbody></table></div>';
    listado.innerHTML = html;
  }
  modal.style.display = 'block';
}

function visualizarArchivo(index) {
  if (!datosDespacho || index < 0 || index >= datosDespacho.archivos.length) { alert('ERROR: ARCHIVO NO ENCONTRADO'); return; }
  const archivo = datosDespacho.archivos[index], contenidoDiv = document.getElementById('contenidoArchivo'), modal = document.getElementById('modalVisualizarArchivo');
  const tipoArchivo = archivo.tipo.toLowerCase();
  if (tipoArchivo.includes('image')) {
    contenidoDiv.innerHTML = `<img src="${archivo.contenido}" style="max-width:100%; max-height:70vh; object-fit:contain;">`;
  } else if (tipoArchivo.includes('pdf')) {
    contenidoDiv.innerHTML = `<embed src="${archivo.contenido}" type="application/pdf" style="width:100%; height:70vh;">`;
  } else if (tipoArchivo.includes('text')) {
    fetch(archivo.contenido).then(r => r.text()).then(text => { contenidoDiv.innerHTML = `<pre>${text.substring(0, 2000)}</pre>`; });
  } else {
    contenidoDiv.innerHTML = `<p>Archivo: ${archivo.nombre}<br>Tipo: ${archivo.tipo}<br>No se puede visualizar este tipo de archivo</p>`;
  }
  modal.style.display = 'block';
}

function cerrarModalArchivos() { document.getElementById('modalArchivos').style.display = 'none'; }

function cerrarModalVisualizarArchivo() { document.getElementById('modalVisualizarArchivo').style.display = 'none'; }

document.addEventListener('keydown', function(e) {
  if (e.key === 'Escape') {
    document.getElementById('modalArticulos').style.display = 'none';
    document.getElementById('modalCotizaciones').style.display = 'none';
    document.getElementById('modalClientes').style.display = 'none';
    document.getElementById('modalAceptado').style.display = 'none';
    document.getElementById('modalArchivos').style.display = 'none';
    document.getElementById('modalVisualizarArchivo').style.display = 'none';
  }
});
</script>
</body>
</html>
