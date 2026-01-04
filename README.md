<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA PRO - CUNDO SPA</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS GENERALES (TODO MAYÚSCULAS) --- */
        :root {
            --primary: #1F6F8B;
            --secondary: #F25C05;
            --bg: #EAEDED;
            --text: #2C3E50;
            --dark: #17202A;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; text-transform: uppercase; }
        body { background: var(--bg); padding: 20px; color: var(--text); }
        
        /* Contenedor Principal */
        .main-container { max-width: 1400px; margin: 0 auto; background: white; min-height: 100vh; box-shadow: 0 0 20px rgba(0,0,0,0.1); display: flex; flex-direction: column; }

        /* HEADER */
        .top-bar { background: var(--dark); color: white; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; }
        .top-menu button { background: #34495E; border: 1px solid #5D6D7E; color: white; padding: 5px 15px; cursor: pointer; border-radius: 4px; font-size: 11px; margin-left: 5px; transition: 0.3s; }
        .top-menu button:hover { background: var(--secondary); border-color: var(--secondary); }

        .header { padding: 30px; display: flex; justify-content: space-between; border-bottom: 4px solid var(--secondary); background: white; }
        .brand h1 { color: var(--primary); font-size: 36px; font-weight: 900; letter-spacing: -1px; margin: 0; }
        .brand p { color: #7F8C8D; font-size: 12px; font-weight: 600; margin-top: 5px; }

        .quote-data { text-align: right; }
        .quote-number { font-size: 24px; font-weight: bold; color: var(--secondary); display: block; }
        .quote-date { font-size: 12px; color: #7F8C8D; }

        /* BODY CONTENT */
        .content { padding: 30px; }

        /* FORMS */
        .section-box { margin-bottom: 30px; border: 1px solid #E5E7E9; border-radius: 8px; padding: 20px; background: #FBFCFC; position: relative; }
        .section-label { position: absolute; top: -12px; left: 20px; background: var(--primary); color: white; padding: 2px 10px; font-size: 11px; font-weight: bold; border-radius: 4px; }
        
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .input-group label { display: block; font-size: 10px; font-weight: bold; color: #7F8C8D; margin-bottom: 5px; }
        .input-group input { width: 100%; padding: 8px; border: 1px solid #D5DBDB; border-radius: 4px; font-size: 12px; font-weight: 600; color: var(--dark); }
        .input-group input:focus { border-color: var(--secondary); outline: none; background: #fff; }
        .input-group input[readonly] { background: #EBEDEF; color: #5D6D7E; cursor: default; }

        /* BUSCADOR AUTOCOMPLETE */
        .search-container { position: relative; }
        .search-results { 
            position: absolute; top: 100%; left: 0; width: 100%; 
            background: white; border: 1px solid #ddd; box-shadow: 0 5px 15px rgba(0,0,0,0.2); 
            z-index: 100; display: none; max-height: 200px; overflow-y: auto; 
        }
        .search-item { padding: 10px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 11px; }
        .search-item:hover { background: #EBF5FB; color: var(--primary); }
        .search-item strong { color: var(--secondary); }

        /* TABLA DE ITEMS */
        table { width: 100%; border-collapse: collapse; font-size: 11px; }
        th { background: var(--primary); color: white; padding: 10px; text-align: left; }
        td { border: 1px solid #E5E7E9; padding: 5px; vertical-align: middle; }
        tr:nth-child(even) { background: #F8F9F9; }

        .col-costo { background: #FEF9E7; }
        .col-venta { background: #EAFAF1; }
        
        input.cell-edit { width: 100%; border: none; background: transparent; padding: 5px; font-weight: bold; text-align: right; }
        input.cell-edit:focus { background: white; outline: 2px solid var(--secondary); }

        /* TOTALES */
        .totals-wrapper { display: flex; justify-content: flex-end; margin-top: 20px; }
        .totals-card { width: 300px; background: white; border: 1px solid #ddd; padding: 15px; border-radius: 8px; }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 12px; }
        .t-final { border-top: 2px solid var(--secondary); padding-top: 10px; margin-top: 10px; font-size: 16px; font-weight: 900; color: var(--primary); }

        /* BOTON PDF */
        .action-area { text-align: center; margin-top: 40px; border-top: 1px dashed #ccc; padding-top: 30px; }
        .btn-main { 
            background: var(--secondary); color: white; padding: 15px 50px; 
            font-size: 18px; font-weight: 900; border: none; border-radius: 50px; 
            cursor: pointer; box-shadow: 0 5px 15px rgba(242, 92, 5, 0.4); 
            transition: transform 0.2s; 
        }
        .btn-main:hover { transform: scale(1.05); background: #D35400; }

        /* MODALES DE GESTIÓN */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 2000; align-items: center; justify-content: center; }
        .modal-box { background: white; width: 80%; max-width: 900px; max-height: 90vh; overflow-y: auto; padding: 25px; border-radius: 8px; position: relative; }
        .close-btn { float: right; font-size: 24px; cursor: pointer; color: red; font-weight: bold; }
        
        .manage-table { width: 100%; margin-top: 15px; }
        .manage-table th { background: #34495E; }
        .btn-action-small { padding: 2px 8px; font-size: 10px; cursor: pointer; margin-right: 5px; }
        .btn-edit { background: #F39C12; border:none; color:white; }
        .btn-del { background: #C0392B; border:none; color:white; }

    </style>
</head>
<body>

<div class="main-container">
    
    <div class="top-bar">
        <div style="font-weight: bold; font-size: 12px;">PANEL DE CONTROL</div>
        <div class="top-menu">
            <button onclick="abrirGestor('clientes')">📂 VER CLIENTES</button>
            <button onclick="abrirGestor('productos')">📦 VER PRODUCTOS</button>
            <button onclick="abrirGestor('historial')">📜 HISTORIAL</button>
        </div>
    </div>

    <div class="header">
        <div class="brand">
            <h1>CUNDO SPA</h1>
            <p>INGENIERÍA, SERVICIOS Y SOLUCIONES INTEGRALES</p>
        </div>
        <div class="quote-data">
            <span class="quote-number" id="lblCorrelativo">CN-000</span>
            <span class="quote-date" id="lblFecha">HOY</span>
        </div>
    </div>

    <div class="content">

        <div class="section-box">
            <span class="section-label">1. INFORMACIÓN DEL CLIENTE</span>
            
            <div class="input-group search-container" style="margin-bottom: 15px;">
                <label>BUSCAR CLIENTE (ESCRIBE 3 LETRAS O RUT):</label>
                <input type="text" id="buscadorCliente" placeholder="EJ: MINERA... O 77.XXX..." onkeyup="buscarCliente(this)" autocomplete="off" style="border: 2px solid var(--primary);">
                <div id="listaClientes" class="search-results"></div>
            </div>

            <div class="grid-form">
                <div class="input-group"><label>RAZÓN SOCIAL</label><input type="text" id="cliRazon" readonly></div>
                <div class="input-group"><label>RUT</label><input type="text" id="cliRut" readonly></div>
                <div class="input-group"><label>GIRO</label><input type="text" id="cliGiro" readonly></div>
                <div class="input-group"><label>DIRECCIÓN</label><input type="text" id="cliDir" readonly></div>
                <div class="input-group"><label>COMUNA/REGIÓN</label><input type="text" id="cliComuna" readonly></div>
                <div class="input-group"><label>CONTACTO</label><input type="text" id="cliContacto" readonly></div>
                <div class="input-group"><label>EMAIL</label><input type="text" id="cliEmail" readonly></div>
                <div class="input-group" style="display:flex; align-items:flex-end;">
                    <button onclick="abrirModalCrear('cliente')" style="width:100%; padding:8px; background:var(--primary); color:white; border:none; cursor:pointer;">+ CREAR NUEVO CLIENTE</button>
                </div>
            </div>
        </div>

        <div class="section-box">
            <span class="section-label">2. DETALLE ECONÓMICO</span>
            
            <table id="tablaItems">
                <thead>
                    <tr>
                        <th style="width: 30px;">X</th>
                        <th style="width: 35%;">DESCRIPCIÓN / SERVICIO</th>
                        <th style="width: 60px;">CANT.</th>
                        
                        <th class="col-costo" style="width: 100px;">COSTO UNIT.</th>
                        <th class="col-costo" style="width: 100px;">TOTAL COSTO</th>
                        
                        <th class="col-venta" style="width: 120px;">VENTA UNIT.</th>
                        <th class="col-venta" style="width: 120px;">TOTAL VENTA</th>
                        <th class="col-venta" style="width: 80px;">UTILIDAD</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
            
            <div style="margin-top: 10px; display: flex; gap: 10px;">
                <button onclick="agregarFila()" style="padding: 8px 15px; background: #5D6D7E; color: white; border: none; cursor: pointer;">+ AÑADIR FILA</button>
                <button onclick="abrirModalCrear('producto')" style="padding: 8px 15px; background: var(--primary); color: white; border: none; cursor: pointer;">+ CREAR PRODUCTO NUEVO</button>
            </div>
        </div>

        <div class="totals-wrapper">
            <div class="totals-card">
                <div class="t-row"><span>NETO:</span><span id="txtNeto">$0</span></div>
                <div class="t-row"><span>IVA (19%):</span><span id="txtIva">$0</span></div>
                <div class="t-row t-final"><span>TOTAL:</span><span id="txtTotal">$0</span></div>
                <hr>
                <div class="t-row" style="color: green; margin-top:5px;"><span>UTILIDAD PROYECTO:</span><span id="txtUtilidad">$0</span></div>
            </div>
        </div>

        <div class="action-area">
            <button class="btn-main" onclick="finalizarCotizacion()">GENERAR COTIZACIÓN PDF</button>
        </div>

    </div>
</div>

<div id="modalGestorClientes" class="modal" style="display:none;">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('clientes')">&times;</span>
        <h2>BASE DE DATOS DE CLIENTES</h2>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>RUT</th>
                    <th>RAZÓN SOCIAL</th>
                    <th>CONTACTO</th>
                    <th>ACCIONES</th>
                </tr>
            </thead>
            <tbody id="bodyGestorClientes"></tbody>
        </table>
    </div>
</div>

<div id="modalGestorProductos" class="modal" style="display:none;">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('productos')">&times;</span>
        <h2>BASE DE DATOS DE PRODUCTOS</h2>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>CÓDIGO</th>
                    <th>DESCRIPCIÓN</th>
                    <th>COSTO</th>
                    <th>ACCIONES</th>
                </tr>
            </thead>
            <tbody id="bodyGestorProductos"></tbody>
        </table>
    </div>
</div>

<div id="modalHistorial" class="modal" style="display:none;">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('historial')">&times;</span>
        <h2>HISTORIAL DE COTIZACIONES EMITIDAS</h2>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>N°</th>
                    <th>FECHA</th>
                    <th>CLIENTE</th>
                    <th>TOTAL</th>
                </tr>
            </thead>
            <tbody id="bodyHistorial"></tbody>
        </table>
    </div>
</div>

<div id="modalCrear" class="modal" style="display:none;">
    <div class="modal-box" style="max-width: 500px;">
        <span class="close-btn" onclick="document.getElementById('modalCrear').style.display='none'">&times;</span>
        <h2 id="tituloModalCrear">NUEVO</h2>
        <div id="formCrearContenido" class="grid-form" style="grid-template-columns: 1fr;"></div>
        <button onclick="guardarNuevoItem()" style="width:100%; margin-top:15px; padding:10px; background:var(--secondary); color:white; border:none; font-weight:bold;">GUARDAR</button>
    </div>
</div>

<script>
    // --- UTILIDADES ---
    const formatMoney = num => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(num);
    const inputUpper = input => input.value = input.value.toUpperCase(); // Forzar mayúsculas en input

    // --- BASE DE DATOS LOCAL ---
    const DB = {
        get: (key) => JSON.parse(localStorage.getItem(key) || "[]"),
        set: (key, data) => localStorage.setItem(key, JSON.stringify(data)),
        init: () => {
            // Inicializar fecha
            document.getElementById('lblFecha').innerText = new Date().toLocaleDateString('es-CL');
            // Inicializar correlativo
            let last = localStorage.getItem('cundo_seq') || 50100;
            document.getElementById('lblCorrelativo').innerText = "CN" + last;
        }
    };

    window.onload = () => {
        DB.init();
        agregarFila(); // Una fila inicial
    };

    // --- LÓGICA DE CLIENTES ---
    function buscarCliente(input) {
        inputUpper(input); // Forzar mayúscula
        const term = input.value;
        const div = document.getElementById('listaClientes');
        div.innerHTML = '';
        
        if (term.length < 3) { div.style.display = 'none'; return; }

        const clientes = DB.get('cundo_db_clientes');
        const hits = clientes.filter(c => c.razon.includes(term) || c.rut.includes(term));

        if (hits.length > 0) {
            div.style.display = 'block';
            hits.forEach(c => {
                const item = document.createElement('div');
                item.className = 'search-item';
                item.innerHTML = `<strong>${c.razon}</strong> - ${c.rut}`;
                item.onclick = () => cargarCliente(c);
                div.appendChild(item);
            });
        } else {
            div.style.display = 'none';
        }
    }

    function cargarCliente(c) {
        document.getElementById('buscadorCliente').value = c.razon;
        document.getElementById('cliRazon').value = c.razon;
        document.getElementById('cliRut').value = c.rut;
        document.getElementById('cliGiro').value = c.giro;
        document.getElementById('cliDir').value = c.dir;
        document.getElementById('cliComuna').value = c.comuna;
        document.getElementById('cliContacto').value = c.contacto;
        document.getElementById('cliEmail').value = c.email;
        document.getElementById('listaClientes').style.display = 'none';
    }

    // --- LÓGICA DE PRODUCTOS (EN TABLA) ---
    function agregarFila() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center;"><button onclick="this.closest('tr').remove(); calcular();" style="color:red; font-weight:bold; border:none; background:none; cursor:pointer;">X</button></td>
            <td style="position:relative;">
                <input type="text" class="cell-edit desc-search" placeholder="BUSCAR PRODUCTO..." onkeyup="buscarProductoFila(this)" oninput="inputUpper(this)">
                <div class="search-results"></div>
            </td>
            <td><input type="number" class="cell-edit cant" value="1" oninput="calcular()"></td>
            
            <td class="col-costo"><input type="number" class="cell-edit costo" value="0" oninput="calcular()"></td>
            <td class="col-costo"><span class="total-costo">$0</span></td>
            
            <td class="col-venta"><input type="number" class="cell-edit venta" value="0" oninput="calcular()"></td>
            <td class="col-venta"><span class="total-venta">$0</span></td>
            <td class="col-venta"><span class="utilidad" style="font-size:10px;">$0</span></td>
        `;
        tbody.appendChild(tr);
    }

    function buscarProductoFila(input) {
        const term = input.value;
        const wrapper = input.nextElementSibling;
        wrapper.innerHTML = '';

        if(term.length < 3) { wrapper.style.display = 'none'; return; }

        const prods = DB.get('cundo_db_productos');
        const hits = prods.filter(p => p.desc.includes(term) || p.cod.includes(term));

        if(hits.length > 0) {
            wrapper.style.display = 'block';
            hits.forEach(p => {
                const item = document.createElement('div');
                item.className = 'search-item';
                item.innerHTML = `<strong>${p.cod}</strong> ${p.desc}`;
                item.onclick = () => {
                    const row = input.closest('tr');
                    input.value = p.desc; // Pone nombre
                    row.querySelector('.costo').value = p.costo; // Pone costo
                    row.querySelector('.venta').value = Math.round(p.costo * 1.4); // Sugiere venta +40%
                    wrapper.style.display = 'none';
                    calcular();
                };
                wrapper.appendChild(item);
            });
        } else {
            wrapper.style.display = 'none';
        }
    }

    function calcular() {
        let neto = 0;
        let utilidadTotal = 0;

        document.querySelectorAll('#tablaItems tbody tr').forEach(row => {
            const cant = parseFloat(row.querySelector('.cant').value) || 0;
            const costo = parseFloat(row.querySelector('.costo').value) || 0;
            const venta = parseFloat(row.querySelector('.venta').value) || 0;

            const tCosto = cant * costo;
            const tVenta = cant * venta;
            const util = tVenta - tCosto;

            row.querySelector('.total-costo').innerText = formatMoney(tCosto);
            row.querySelector('.total-venta').innerText = formatMoney(tVenta);
            row.querySelector('.utilidad').innerText = formatMoney(util);

            neto += tVenta;
            utilidadTotal += util;
        });

        const iva = neto * 0.19;
        const total = neto + iva;

        document.getElementById('txtNeto').innerText = formatMoney(neto);
        document.getElementById('txtIva').innerText = formatMoney(iva);
        document.getElementById('txtTotal').innerText = formatMoney(total);
        document.getElementById('txtUtilidad').innerText = formatMoney(utilidadTotal);
    }

    // --- GESTIÓN DE MODALES (CLIENTES/PRODUCTOS/HISTORIAL) ---
    function abrirGestor(tipo) {
        document.querySelectorAll('.modal').forEach(m => m.style.display = 'none'); // Cerrar otros
        
        if(tipo === 'clientes') {
            document.getElementById('modalGestorClientes').style.display = 'flex';
            renderTablaClientes();
        } else if(tipo === 'productos') {
            document.getElementById('modalGestorProductos').style.display = 'flex';
            renderTablaProductos();
        } else if(tipo === 'historial') {
            document.getElementById('modalHistorial').style.display = 'flex';
            renderHistorial();
        }
    }

    function cerrarGestor(tipo) {
        if(tipo === 'clientes') document.getElementById('modalGestorClientes').style.display = 'none';
        if(tipo === 'productos') document.getElementById('modalGestorProductos').style.display = 'none';
        if(tipo === 'historial') document.getElementById('modalHistorial').style.display = 'none';
    }

    // --- RENDERIZADO DE TABLAS DE GESTIÓN ---
    function renderTablaClientes() {
        const tbody = document.getElementById('bodyGestorClientes');
        tbody.innerHTML = '';
        const data = DB.get('cundo_db_clientes');
        data.forEach((c, idx) => {
            tbody.innerHTML += `<tr>
                <td>${c.rut}</td><td>${c.razon}</td><td>${c.contacto}</td>
                <td><button class="btn-action-small btn-del" onclick="borrarItem('cundo_db_clientes', ${idx})">BORRAR</button></td>
            </tr>`;
        });
    }

    function renderTablaProductos() {
        const tbody = document.getElementById('bodyGestorProductos');
        tbody.innerHTML = '';
        const data = DB.get('cundo_db_productos');
        data.forEach((p, idx) => {
            tbody.innerHTML += `<tr>
                <td>${p.cod}</td><td>${p.desc}</td><td>${formatMoney(p.costo)}</td>
                <td><button class="btn-action-small btn-del" onclick="borrarItem('cundo_db_productos', ${idx})">BORRAR</button></td>
            </tr>`;
        });
    }

    function renderHistorial() {
        const tbody = document.getElementById('bodyHistorial');
        tbody.innerHTML = '';
        const data = DB.get('cundo_historial');
        // Mostrar los últimos primero
        data.reverse().forEach(h => {
            tbody.innerHTML += `<tr>
                <td>${h.numero}</td><td>${h.fecha}</td><td>${h.cliente}</td><td>${h.total}</td>
            </tr>`;
        });
    }

    function borrarItem(dbKey, idx) {
        if(confirm('¿ESTÁ SEGURO DE ELIMINAR ESTE REGISTRO?')) {
            const data = DB.get(dbKey);
            data.splice(idx, 1);
            DB.set(dbKey, data);
            // Recargar tabla correspondiente
            if(dbKey.includes('clientes')) renderTablaClientes();
            if(dbKey.includes('productos')) renderTablaProductos();
        }
    }

    // --- CREAR NUEVO (CLIENTE/PRODUCTO) ---
    let tipoCreacion = '';
    function abrirModalCrear(tipo) {
        tipoCreacion = tipo;
        const modal = document.getElementById('modalCrear');
        const titulo = document.getElementById('tituloModalCrear');
        const form = document.getElementById('formCrearContenido');
        modal.style.display = 'flex';
        form.innerHTML = '';

        if(tipo === 'cliente') {
            titulo.innerText = "CREAR NUEVO CLIENTE";
            form.innerHTML = `
                <input type="text" id="newRut" placeholder="RUT" oninput="inputUpper(this)">
                <input type="text" id="newRazon" placeholder="RAZÓN SOCIAL" oninput="inputUpper(this)">
                <input type="text" id="newGiro" placeholder="GIRO" oninput="inputUpper(this)">
                <input type="text" id="newDir" placeholder="DIRECCIÓN" oninput="inputUpper(this)">
                <input type="text" id="newComuna" placeholder="COMUNA" oninput="inputUpper(this)">
                <input type="text" id="newContacto" placeholder="NOMBRE CONTACTO" oninput="inputUpper(this)">
                <input type="text" id="newEmail" placeholder="EMAIL" oninput="inputUpper(this)">
            `;
        } else {
            titulo.innerText = "CREAR NUEVO PRODUCTO";
            form.innerHTML = `
                <input type="text" id="newCod" placeholder="CÓDIGO (EJ: PRO-01)" oninput="inputUpper(this)">
                <input type="text" id="newDesc" placeholder="DESCRIPCIÓN" oninput="inputUpper(this)">
                <input type="number" id="newCosto" placeholder="COSTO NETO ($)">
            `;
        }
    }

    function guardarNuevoItem() {
        if(tipoCreacion === 'cliente') {
            const nuevo = {
                rut: document.getElementById('newRut').value,
                razon: document.getElementById('newRazon').value,
                giro: document.getElementById('newGiro').value,
                dir: document.getElementById('newDir').value,
                comuna: document.getElementById('newComuna').value,
                contacto: document.getElementById('newContacto').value,
                email: document.getElementById('newEmail').value
            };
            if(!nuevo.razon) return alert("RAZÓN SOCIAL OBLIGATORIA");
            const db = DB.get('cundo_db_clientes');
            db.push(nuevo);
            DB.set('cundo_db_clientes', db);
        } else {
            const nuevo = {
                cod: document.getElementById('newCod').value,
                desc: document.getElementById('newDesc').value,
                costo: parseFloat(document.getElementById('newCosto').value) || 0
            };
            if(!nuevo.desc) return alert("DESCRIPCIÓN OBLIGATORIA");
            const db = DB.get('cundo_db_productos');
            db.push(nuevo);
            DB.set('cundo_db_productos', db);
        }
        document.getElementById('modalCrear').style.display = 'none';
        alert("GUARDADO EXITOSAMENTE");
        
        // Si estamos en un gestor abierto, refrescar
        if(document.getElementById('modalGestorClientes').style.display === 'flex') renderTablaClientes();
        if(document.getElementById('modalGestorProductos').style.display === 'flex') renderTablaProductos();
    }

    // --- GENERAR COTIZACIÓN (PDF + HISTORIAL) ---
    async function finalizarCotizacion() {
        const cliente = document.getElementById('cliRazon').value;
        if(!cliente) return alert("DEBE SELECCIONAR UN CLIENTE");

        // 1. Guardar en Historial
        const nCot = document.getElementById('lblCorrelativo').innerText;
        const total = document.getElementById('txtTotal').innerText;
        const historial = DB.get('cundo_historial');
        
        historial.push({
            numero: nCot,
            fecha: new Date().toLocaleDateString('es-CL'),
            cliente: cliente,
            total: total
        });
        DB.set('cundo_historial', historial);

        // 2. Incrementar Correlativo
        let numActual = parseInt(nCot.replace('CN',''));
        localStorage.setItem('cundo_seq', numActual + 1);

        // 3. Generar PDF
        generarPDF(nCot);
    }

    function generarPDF(numeroCotizacion) {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();
        
        // Configuración Colores
        const azul = [31, 111, 139];
        const naranja = [242, 92, 5];

        // Header
        doc.setFillColor(...azul);
        doc.rect(0, 0, 15, 297, 'F'); // Banda lateral
        
        doc.setFontSize(28);
        doc.setTextColor(...azul);
        doc.setFont("helvetica", "bold");
        doc.text("CUNDO SPA", 25, 25);
        doc.setFontSize(10);
        doc.setTextColor(100);
        doc.setFont("helvetica", "normal");
        doc.text("INGENIERÍA Y SERVICIOS INTEGRALES", 25, 32);

        // Caja Info
        doc.setFillColor(245, 245, 245);
        doc.rect(140, 15, 60, 25, 'F');
        doc.setTextColor(...naranja);
        doc.setFontSize(14);
        doc.setFont("helvetica", "bold");
        doc.text("COTIZACIÓN", 170, 22, { align: "center" });
        doc.setTextColor(0);
        doc.text(numeroCotizacion, 170, 32, { align: "center" });

        // Cliente
        doc.setFontSize(10);
        doc.setTextColor(...azul);
        doc.text("DATOS DEL CLIENTE:", 25, 50);
        doc.setTextColor(0);
        doc.setFont("helvetica", "normal");
        
        const razon = document.getElementById('cliRazon').value;
        const rut = document.getElementById('cliRut').value;
        const dir = document.getElementById('cliDir').value;
        const att = document.getElementById('cliContacto').value;

        doc.text(`SEÑOR(ES): ${razon}`, 25, 57);
        doc.text(`RUT: ${rut}`, 25, 62);
        doc.text(`DIRECCIÓN: ${dir}`, 25, 67);
        doc.text(`ATENCIÓN: ${att}`, 25, 72);

        // Tabla Items
        const filas = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.desc-search').value;
            const cant = tr.querySelector('.cant').value;
            const unit = tr.querySelector('.venta').value;
            const total = parseFloat(cant) * parseFloat(unit);
            
            if(desc) {
                filas.push([cant, desc, formatMoney(unit), formatMoney(total)]);
            }
        });

        doc.autoTable({
            startY: 85,
            head: [['CANT.', 'DESCRIPCIÓN', 'P. UNIT.', 'TOTAL']],
            body: filas,
            theme: 'striped',
            headStyles: { fillColor: azul, textColor: 255, fontStyle:'bold' },
            columnStyles: {
                0: { halign: 'center', cellWidth: 20 },
                2: { halign: 'right' },
                3: { halign: 'right', fontStyle: 'bold' }
            },
            margin: { left: 25 }
        });

        // Totales
        const finalY = doc.lastAutoTable.finalY + 10;
        const neto = document.getElementById('txtNeto').innerText;
        const iva = document.getElementById('txtIva').innerText;
        const total = document.getElementById('txtTotal').innerText;

        doc.setFontSize(11);
        doc.text("NETO:", 140, finalY);
        doc.text(neto, 195, finalY, { align: 'right' });
        doc.text("IVA (19%):", 140, finalY + 6);
        doc.text(iva, 195, finalY + 6, { align: 'right' });
        
        doc.setFontSize(14);
        doc.setTextColor(...azul);
        doc.setFont("helvetica", "bold");
        doc.text("TOTAL:", 140, finalY + 15);
        doc.text(total, 195, finalY + 15, { align: 'right' });

        // Footer
        doc.setFontSize(8);
        doc.setTextColor(150);
        doc.text("DOCUMENTO GENERADO POR SISTEMA CUNDO SPA", 110, 280, { align: "center" });

        doc.save(`Cotizacion_${numeroCotizacion}.pdf`);
        
        // Recargar página para actualizar correlativo visualmente
        setTimeout(() => location.reload(), 1000);
    }
</script>

</body>
</html>
