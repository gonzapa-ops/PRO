<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotizador - EMPRESA CUNDO</title>
    <style>
        * {margin: 0; padding: 0; box-sizing: border-box;}
        body {font-family: 'Arial', sans-serif; background-color: #f5f5f5; padding: 20px;}
        .cotizador-container {max-width: 1200px; margin: 0 auto; background-color: white; box-shadow: 0 0 10px rgba(0,0,0,0.1);}
        .cotizador-header {background-color: #2c3e50; color: white; padding: 15px 30px; display: flex; justify-content: space-between; align-items: center; border-bottom: 3px solid #3498db;}
        .empresa-nombre {font-size: 20px; font-weight: bold; letter-spacing: 1px;}
        .numero-cotizacion {font-size: 16px;}
        .numero-cotizacion span:first-child {font-weight: normal; opacity: 0.9;}
        .numero-cotizacion span:last-child {font-weight: bold; color: #3498db; margin-left: 5px;}
        .cotizador-contenido {padding: 30px;}
        .seccion-titulo {font-size: 18px; font-weight: bold; color: #2c3e50; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 2px solid #3498db; text-transform: uppercase;}
        .busqueda-rut {display: flex; gap: 10px; margin-bottom: 20px; align-items: center;}
        .busqueda-rut input {flex: 1; padding: 10px; font-size: 16px; border: 2px solid #ddd; border-radius: 4px; transition: border-color 0.3s; text-transform: uppercase;}
        .busqueda-rut input:focus {outline: none; border-color: #3498db;}
        .btn {padding: 10px 20px; font-size: 14px; border: none; border-radius: 4px; cursor: pointer; transition: background-color 0.3s; font-weight: bold; text-transform: uppercase;}
        .btn-buscar {background-color: #3498db; color: white;}
        .btn-buscar:hover {background-color: #2980b9;}
        .btn-limpiar {background-color: #95a5a6; color: white;}
        .btn-limpiar:hover {background-color: #7f8c8d;}
        .btn-editar {background-color: #f39c12; color: white; padding: 8px 15px; font-size: 13px;}
        .btn-editar:hover {background-color: #e67e22;}
        .btn-cancelar {background-color: #e74c3c; color: white; padding: 8px 15px; font-size: 13px; margin-left: 10px;}
        .btn-cancelar:hover {background-color: #c0392b;}
        .btn-articulos {background-color: #16a085; color: white; margin-bottom: 16px;}
        .btn-articulos:hover {background-color: #138d75;}
        .btn-agregar {background-color: #27ae60; color: white; padding: 8px 15px; font-size: 13px;}
        .btn-agregar:hover {background-color: #229954;}
        .btn-eliminar {background-color: #e74c3c; color: white; padding: 5px 10px; font-size: 12px;}
        .btn-eliminar:hover {background-color: #c0392b;}
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
        .valor-numerico {text-align: right; font-weight: bold;}
        .celda-acciones {text-align: center;}
        .btn-pequeno {padding: 5px 10px; font-size: 12px;}
        .formulario-producto h4, .formulario-editar-articulo h4 {text-transform: uppercase; color: #856404; margin-bottom: 15px;}
        .fila-campos-producto {display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;}
        .fila-campos-producto-tres {display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-bottom: 15px;}
        .fila-campos-producto .campo-grupo, .fila-campos-producto-tres .campo-grupo {margin-bottom: 0;}
        .botones-producto {display: flex; gap: 10px; margin-top: 10px;}
        .alerta-sin-productos {background-color: #e8f4f8; padding: 15px; border-radius: 4px; border-left: 4px solid #3498db; text-align: center; color: #2c3e50; text-transform: uppercase; font-weight: bold;}
        #modalArticulos {display:none; position:fixed; top:0; left:0; width:100vw; height:100vh; background:rgba(0,0,0,0.5); z-index:9999;}
        .articulos-content {background:white; border-radius:8px; margin:40px auto; padding:25px; max-width:1000px; position:relative;}
        .modal-titulo {font-size:22px; font-weight:bold; margin-bottom:20px; text-transform:uppercase;}
        .btn-cerrar-modal {position:absolute; top:14px; right:25px; background:#e74c3c; color:white; font-size:18px; border:none; border-radius:4px; cursor:pointer; padding:4px 10px;}
    </style>
</head>
<body>
    <div class="cotizador-container">
        <header class="cotizador-header">
            <div class="empresa-nombre">EMPRESA CUNDO</div>
            <div class="numero-cotizacion">
                <span>COTIZACIÓN N°: </span>
                <span id="numeroCotizacion">CO100500</span>
            </div>
        </header>

        <main class="cotizador-contenido">
            <button class="btn btn-articulos" onclick="abrirArticulos()">ARTÍCULOS</button>
            <!-- Sección cliente y productos igual que antes -->
            <!-- ... -->
            <!-- Sección para productos y servicios, formulario para crear productos y la tabla para cotización -->
            <!-- ... -->
        </main>
    </div>
    <!-- MODAL ARTÍCULOS -->
    <div id="modalArticulos">
        <div class="articulos-content">
            <button class="btn-cerrar-modal" onclick="cerrarArticulos()">×</button>
            <div class="modal-titulo">ARTÍCULOS (PRODUCTOS Y SERVICIOS REGISTRADOS)</div>
            <div id="tablaArticulosContenedor"></div>
            <div id="formularioEditarArticulo" class="formulario-editar-articulo"></div>
        </div>
    </div>
    <script>
    // ... Todas tus clases GestorCotizaciones, GestorClientes, GestorProductos igual que antes ...
    // ... Código principal de creación, edición y borrado de artículos igual que antes ...
    // ... Solo modifica la función listarArticulos para agregar el campo utilidad:
    function listarArticulos() {
        const todos = gestorProductos.obtenerTodos();
        let html = `<table class="tabla-articulos"><thead>
            <tr>
            <th>CÓDIGO</th>
            <th>DESCRIPCIÓN</th>
            <th>COSTO</th>
            <th>PORCENTAJE (%)</th>
            <th>VALOR NETO</th>
            <th>UTILIDAD</th>
            <th>PROVEEDOR</th>
            <th>LINK</th>
            <th>ACCIÓN</th>
            </tr></thead><tbody>`;
        const claves = Object.keys(todos);
        if(claves.length===0){
            html += `<tr><td colspan="9" style="text-align:center; color:#888;">NO HAY ARTÍCULOS</td></tr>`;
        }
        claves.forEach(codigo=>{
            const art = todos[codigo];
            html += `<tr>
            <td>${art.codigo}</td>
            <td>${art.descripcion}</td>
            <td class="valor-numerico">$${parseFloat(art.costo).toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}</td>
            <td class="valor-numerico">${parseFloat(art.porcentaje).toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}</td>
            <td class="valor-numerico">$${parseFloat(art.valorNeto).toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}</td>
            <td class="valor-numerico">$${(parseFloat(art.valorNeto)-parseFloat(art.costo)).toLocaleString('es-CL', {minimumFractionDigits:2, maximumFractionDigits:2})}</td>
            <td>${art.proveedor}</td>
            <td style="text-transform:lowercase;">${art.link}</td>
            <td><button class="btn btn-editar btn-pequeno" onclick="editarArticulo('${art.codigo.replace(/'/g,"")}')">EDITAR</button>
                <button class="btn btn-eliminar btn-pequeno" onclick="eliminarArticulo('${art.codigo.replace(/'/g,"")}')">ELIMINAR</button></td>
            </tr>`;
        });
        html += `</tbody></table>`;
        document.getElementById('tablaArticulosContenedor').innerHTML = html;
    }
    // Resto del JS igual que antes
    // ...
    </script>
</body>
</html>
