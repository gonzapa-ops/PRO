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
button {cursor: pointer; border: none; border-radius: 4px; padding: 10px 15px; font-weight: bold; text-transform: uppercase; font-size: 13px; transition: background-color 0.3s;}
.btn-buscar {background: #3498db; color: white;}
.btn-buscar:hover {background: #2980b9;}
.btn-limpiar {background: #95a5a6; color: white;}
.btn-limpiar:hover {background: #7f8c8d;}
.btn-eliminar {background: #e74c3c; color: white; font-size: 11px; padding: 5px 10px;}
.btn-eliminar:hover {background: #c0392b;}
.btn-articulos {background: #16a085; color: white; margin-bottom: 10px;}
.btn-articulos:hover {background: #138d75;}
.btn-pdf {background: #d35400; color: white; margin-bottom: 10px;}
.btn-pdf:hover {background: #e67e22;}
.btn-agregar {background: #27ae60; color: white;}
.btn-agregar:hover {background: #229954;}
.btn-editar {background: #f39c12; color: white; padding: 8px 12px; font-size: 12px;}
.btn-editar:hover {background: #e67e22;}
.btn-cancelar {background: #e74c3c; color: white; padding: 8px 12px; font-size: 12px;}
.btn-cancelar:hover {background: #c0392b;}
.btn-ver {background: #9b59b6; color: white; padding: 6px 10px; font-size: 11px; margin-right: 5px;}
.btn-ver:hover {background: #8e44ad;}
.btn-editar-cot {background: #f39c12; color: white; padding: 6px 10px; font-size: 11px;}
.btn-editar-cot:hover {background: #e67e22;}
.formulario-cliente, .formulario-producto, .formulario-editar-articulo {display: none; animation: fadeIn 0.3s;}
.formulario-cliente.activo, .formulario-producto.activo, .formulario-editar-articulo.activo {display: block;}
@keyframes fadeIn {from {opacity: 0;} to {opacity: 1;}}
.campo-grupo {margin-bottom: 15px;}
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
.busqueda-producto {display: flex; gap: 10px; margin-bottom: 20px; align-items: center;}
.busqueda-producto input {flex: 1; padding: 10px; font-size: 13px; border: 2px solid #ddd; border-radius: 4px; text-transform: uppercase;}
.busqueda-producto input:focus {outline: none; border-color: #3498db;}
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
.botones-superiores {display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap;}
#modalCotizaciones {display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0,0,0,0.5); z-index: 10000; overflow: auto;}
#modalCotizaciones > div {background: white; max-width: 900px; margin: 40px auto; padding: 20px; border-radius: 8px; position: relative;}
.resumen-totales {max-width: 400px; margin-left: auto; border-top: 3px solid #3498db; padding-top: 15px; text-transform: uppercase;}
.resumen-linea {display: flex; justify-content: space-between; margin: 5px 0; font-weight: bold; font-size: 13px;}
.resumen-linea.total {font-size: 16px; color: #2c3e50;}
.formulario-editar-articulo {background: #fff3cd; padding: 15px; border-radius: 4px; border-left: 4px solid #f39c12; margin-bottom: 20px;}
.formulario-editar-articulo h4 {text-transform: uppercase; color: #856404; margin-bottom: 15px;}
.tabla-cotizaciones {width: 100%; border-collapse: collapse; background: white;}
.tabla-cotizaciones th {background: #34495e; color: white; padding: 12px; text-align: left; text-transform: uppercase; font-weight: bold;}
.tabla-cotizaciones td {border: 1px solid #ddd; padding: 12px; text-transform: uppercase;}
.tabla-cotizaciones tr:nth-child(even) {background: #f9f9f9;}
.tabla-cotizaciones tr:hover {background: #f0f0f0;}
.sincronizacion {position: fixed; bottom: 20px; right: 20px; background: #27ae60; color: white; padding: 10px 15px; border-radius: 4px; font-size: 12px; z-index: 1000;}
.sincronizacion.error {background: #e74c3c;}
</style>
</head>
<body>
  <div id="estadoSincronizacion" class="sincronizacion">Conectando...</div>
  
  <div class="cotizador-container">
    <!-- TODO EL HTML ANTERIOR IGUAL... -->
    <!-- (Pega todo el HTML anterior aquí sin cambios) -->
  </div>

<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>

<script>
// CONFIGURACIÓN FIREBASE - REEMPLAZA CON TUS DATOS
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "tu-proyecto.firebaseapp.com",
  databaseURL: "https://tu-proyecto-default-rtdb.firebaseio.com",
  projectId: "tu-proyecto",
  storageBucket: "tu-proyecto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};

// Inicializar Firebase
firebase.initializeApp(firebaseConfig);
const db = firebase.database();

let clienteActual = null;
let modoEdicion = false;
let productosEnCotizacion = [];
let cotizacionesEmitidas = [];
let articuloEdicion = null;

// SINCRONIZAR DATOS DESDE FIREBASE
function sincronizarDatos() {
  mostrarEstadoSincronizacion('Sincronizando...', false);
  
  db.ref('cotizador').once('value').then(snapshot => {
    const datos = snapshot.val();
    if (datos) {
      if (datos.clientes) Object.assign(gestorClientes.clientes, datos.clientes);
      if (datos.productos) Object.assign(gestorProductos.productos, datos.productos);
      if (datos.cotizaciones) cotizacionesEmitidas = datos.cotizaciones;
      if (datos.numeroCotizacion) gestorCotizaciones.numeroBase = datos.numeroCotizacion;
    }
    mostrarEstadoSincronizacion('Sincronizado', false);
  }).catch(error => {
    mostrarEstadoSincronizacion('Error: ' + error.message, true);
  });
}

function mostrarEstadoSincronizacion(texto, esError) {
  const el = document.getElementById('estadoSincronizacion');
  el.textContent = texto;
  el.className = esError ? 'sincronizacion error' : 'sincronizacion';
}

// GUARDAR CLIENTES EN FIREBASE
function guardarClientesEnFirebase() {
  db.ref('cotizador/clientes').set(gestorClientes.clientes).catch(err => {
    console.error('Error guardando clientes:', err);
    mostrarEstadoSincronizacion('Error al guardar', true);
  });
}

// GUARDAR PRODUCTOS EN FIREBASE
function guardarProductosEnFirebase() {
  db.ref('cotizador/productos').set(gestorProductos.productos).catch(err => {
    console.error('Error guardando productos:', err);
    mostrarEstadoSincronizacion('Error al guardar', true);
  });
}

// GUARDAR COTIZACIONES EN FIREBASE
function guardarCotizacionesEnFirebase() {
  db.ref('cotizador/cotizaciones').set(cotizacionesEmitidas).catch(err => {
    console.error('Error guardando cotizaciones:', err);
    mostrarEstadoSincronizacion('Error al guardar', true);
  });
  
  db.ref('cotizador/numeroCotizacion').set(gestorCotizaciones.numeroBase).catch(err => {
    console.error('Error guardando número:', err);
  });
}

// ESCUCHAR CAMBIOS EN TIEMPO REAL
function escucharCambiosEnTiempoReal() {
  db.ref('cotizador/clientes').on('value', snapshot => {
    const datos = snapshot.val();
    if (datos) gestorClientes.clientes = datos;
  });
  
  db.ref('cotizador/productos').on('value', snapshot => {
    const datos = snapshot.val();
    if (datos) gestorProductos.productos = datos;
  });
  
  db.ref('cotizador/cotizaciones').on('value', snapshot => {
    const datos = snapshot.val();
    if (datos) cotizacionesEmitidas = datos;
  });
}

// Clases iguales que antes
class GestorCotizaciones {
  constructor() {this.prefijo = 'CO'; this.numeroBase = 100500;}
  obtenerNumeroActual() {return `${this.prefijo}${this.numeroBase}`;}
  siguienteCotizacion() {
    this.numeroBase++;
    guardarCotizacionesEnFirebase();
    const el = document.getElementById('numeroCotizacion');
    if (el) el.textContent = this.obtenerNumeroActual();
    return this.obtenerNumeroActual();
  }
}

class GestorClientes {
  constructor() {this.clientes = {};}
  buscarPorRut(rut) {return this.clientes[rut] || null;}
  agregarCliente(rut, datos) {this.clientes[rut] = datos; guardarClientesEnFirebase();}
  actualizarCliente(rut, datos) {this.clientes[rut] = datos; guardarClientesEnFirebase();}
}

class GestorProductos {
  constructor() {this.productos = {};}
  buscarPorCodigo(codigo) {return this.productos[codigo] || null;}
  agregarProducto(codigo, datos) {this.productos[codigo] = datos; guardarProductosEnFirebase();}
  actualizarProducto(codigo, datos) {this.productos[codigo] = datos; guardarProductosEnFirebase();}
  obtenerTodos() {return this.productos;}
  eliminarProducto(codigo) {delete this.productos[codigo]; guardarProductosEnFirebase();}
}

const gestorCotizaciones = new GestorCotizaciones();
const gestorClientes = new GestorClientes();
const gestorProductos = new GestorProductos();

// INICIALIZAR AL CARGAR
window.addEventListener('load', () => {
  sincronizarDatos();
  escucharCambiosEnTiempoReal();
});

// TODO EL CÓDIGO JAVASCRIPT ANTERIOR IGUAL...
// (Pega todo el JavaScript anterior aquí, ya que el resto del código funciona igual)

</script>
</body>
</html>
