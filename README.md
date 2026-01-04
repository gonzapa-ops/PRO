<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA PRO V5 - CUNDO SPA</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS GENERALES --- */
        :root {
            --primary: #1F6F8B;
            --secondary: #F25C05;
            --bg: #EAEDED;
            --text: #2C3E50;
            --dark: #17202A;
            --gray-input: #EBEDEF;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; text-transform: uppercase; }
        body { background: var(--bg); padding: 20px; color: var(--text); padding-bottom: 100px; }
        
        .main-container { max-width: 1600px; margin: 0 auto; background: white; min-height: 100vh; box-shadow: 0 0 20px rgba(0,0,0,0.1); display: flex; flex-direction: column; border-radius: 8px; overflow: hidden; }

        /* HEADER */
        .top-bar { background: var(--dark); color: white; padding: 12px 25px; display: flex; justify-content: space-between; align-items: center; }
        .top-menu { display: flex; gap: 10px; }
        .top-menu button { background: #34495E; border: 1px solid #5D6D7E; color: white; padding: 6px 15px; cursor: pointer; border-radius: 4px; font-size: 11px; font-weight: bold; transition: 0.3s; display: flex; align-items: center; gap: 5px; }
        .top-menu button:hover { background: var(--secondary); border-color: var(--secondary); }

        .header { padding: 30px; display: flex; justify-content: space-between; border-bottom: 4px solid var(--secondary); background: white; }
        .brand h1 { color: var(--primary); font-size: 32px; font-weight: 900; letter-spacing: -1px; margin: 0; }
        .brand p { color: #7F8C8D; font-size: 11px; font-weight: 600; margin-top: 5px; }

        .quote-data { text-align: right; }
        .quote-number { font-size: 24px; font-weight: bold; color: var(--secondary); display: block; }

        /* CONTENIDO */
        .content { padding: 30px; }
        .section-box { margin-bottom: 30px; border: 1px solid #E5E7E9; border-radius: 8px; padding: 25px; background: #FBFCFC; position: relative; }
        .section-label { position: absolute; top: -12px; left: 20px; background: var(--primary); color: white; padding: 4px 12px; font-size: 11px; font-weight: bold; border-radius: 4px; letter-spacing: 1px; }

        /* FORMULARIOS */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .input-group label { display: block; font-size: 10px; font-weight: bold; color: #7F8C8D; margin-bottom: 5px; }
        .input-group input { width: 100%; padding: 8px 10px; border: 1px solid #D5DBDB; border-radius: 4px; font-size: 12px; font-weight: 600; color: var(--dark); }
        .input-group input:focus { border-color: var(--secondary); outline: none; background: #fff; }
        .input-group input[readonly] { background: var(--gray-input); color: #555; cursor: not-allowed; border-color: #E5E7E9; }

        /* BUSCADOR FLOTANTE */
        .search-container { position: relative; }
        .search-results { 
            position: absolute; top: 100%; left: 0; width: 100%; 
            background: white; border: 1px solid #3498DB; box-shadow: 0 10px 20px rgba(0,0,0,0.15); 
            z-index: 500; display: none; max-height: 250px; overflow-y: auto; border-radius: 0 0 6px 6px;
        }
        .search-item { padding: 12px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 11px; color: #333; }
        .search-item:hover { background: #EBF5FB; color: var(--primary); font-weight: bold; }
        .search-item strong { color: var(--secondary); }

        /* TABLA PRINCIPAL */
        table { width: 100%; border-collapse: collapse; font-size: 11px; }
        th { background: var(--primary); color: white; padding: 12px 10px; text-align: left; font-weight: 700; border-right: 1px solid rgba(255,255,255,0.1); }
        td { border: 1px solid #D5DBDB; padding: 0; vertical-align: middle; height: 35px; }
        
        /* Inputs de tabla */
        input.cell-edit { width: 100%; height: 100%; border: none; padding: 0 10px; font-size: 12px; font-family: inherit; color: #333; }
        input.cell-edit:focus { outline: 2px solid var(--secondary); z-index: 10; position: relative; background: white; }
        
        /* Celdas Bloqueadas (Readonly) */
        input.cell-locked { background: #F2F4F4; color: #7F8C8D; text-align: right; cursor: not-allowed; font-weight: bold; }
        input.cell-qty { text-align: center; font-weight: bold; color: var(--primary); }

        /* TOTALES */
        .totals-wrapper { display: flex; justify-content: flex-end; margin-top: 20px; }
        .totals-card { width: 350px; background: white; border: 1px solid #D5DBDB; padding: 20px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 12px; font-weight: 600; color: #555; }
        .t-final { border-top: 2px solid var(--secondary); padding-top: 15px; margin-top: 15px; font-size: 18px; font-weight: 900; color: var(--primary); }

        /* BOTÓN GENERAR */
        .action-area { text-align: center; margin-top: 40px; border-top: 1px dashed #BDC3C7; padding-top: 30px; }
        .btn-main { 
            background: var(--secondary); color: white; padding: 16px 60px; 
            font-size: 16px; font-weight: 800; border: none; border-radius: 4px; 
            cursor: pointer; box-shadow: 0 5px 15px rgba(242, 92, 5, 0.3); 
            transition: all 0.2s; 
        }
        .btn-main:hover { transform: translateY(-2px); background: #D35400; }

        /* MODALES */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 2000; align-items: center; justify-content: center; backdrop-filter: blur(3px); }
        .modal-box { background: white; width: 95%; max-width: 1200px; max-height: 90vh; overflow-y: auto; padding: 30px; border-radius: 6px; box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        .close-btn { float: right; font-size: 28px; cursor: pointer; color: #E74C3C; font-weight: bold; line-height: 1; }
        .close-btn:hover { color: #C0392B; }

        /* TABLAS DE GESTIÓN */
        .manage-table { width: 100%; margin-top: 20px; border: 1px solid #ddd; }
        .manage-table th { background: #2C3E50; color: white; padding: 10px; font-size: 10px; }
        .manage-table td { padding: 8px; border-bottom: 1px solid #eee; font-size: 11px; color: #333; }
        .manage-table tr:hover { background: #F4F6F6; }

        .btn-action-group { display: flex; gap: 5px; }
        .btn-small { padding: 4px 8px; border: none; color: white; border-radius: 3px; cursor: pointer; font-size: 10px; font-weight: bold; }
        .btn-edit { background: #F1C40F; color: black; }
        .btn-del { background: #E74C3C; }
        .btn-new { background: var(--primary); padding: 10px 20px; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; float: right; }

    </style>
</head>
<body>

<div class="main-container">
    
    <div class="top-bar">
        <div style="font-weight: 800; font-size: 14px; color: #BDC3C7;">PANEL DE CONTROL ADMINISTRATIVO</div>
        <div class="top-menu">
            <button onclick="abrirGestor('clientes')">👥 BASE CLIENTES</button>
            <button onclick="abrirGestor('productos')">📦 BASE PRODUCTOS</button>
            <button onclick="abrirGestor('historial')">📜 HISTORIAL</button>
        </div>
    </div>

    <div class="header">
        <div class="brand">
            <h1>CUNDO SPA</h1>
            <p>INGENIERÍA, SERVICIOS Y SOLUCIONES INTEGRALES</p>
        </div>
        <div class="quote-data">
            <span style="font-size:10px; color:#999;">NÚMERO DE COTIZACIÓN</span>
            <span class="quote-number" id="lblCorrelativo">CN-Cargando...</span>
            <span style="font-size:11px; font-weight:bold; color:var(--primary); margin-top:5px; display:block;" id="lblFecha"></span>
        </div>
    </div>

    <div class="content">

        <div class="section-box">
            <span class="section-label">1. INFORMACIÓN DEL CLIENTE</span>
            
            <div class="input-group search-container" style="margin-bottom: 20px;">
                <label style="color:var(--secondary);">🔍 BUSCAR CLIENTE (INGRESE 3 LETRAS O RUT):</label>
                <input type="text" id="buscadorCliente" placeholder="Ej: MINERA... o 76.XXX..." onkeyup="buscarCliente(this)" autocomplete="off" style="border: 2px solid var(--secondary); background:#fff;">
                <div id="listaClientes" class="search-results"></div>
            </div>

            <div class="grid-form">
                <div class="input-group"><label>RAZÓN SOCIAL</label><input type="text" id="cliRazon" readonly></div>
                <div class="input-group"><label>RUT</label><input type="text" id="cliRut" readonly></div>
                <div class="input-group"><label>GIRO</label><input type="text" id="cliGiro" readonly></div>
                <div class="input-group"><label>DIRECCIÓN</label><input type="text" id="cliDir" readonly></div>
                <div class="input-group"><label>COMUNA</label><input type="text" id="cliComuna" readonly></div>
                <div class="input-group"><label>REGIÓN</label><input type="text" id="cliRegion" readonly></div>
                <div class="input-group"><label>CONTACTO</label><input type="text" id="cliContacto" readonly></div>
                <div class="input-group"><label>EMAIL</label><input type="text" id="cliEmail" readonly></div>
            </div>
        </div>

        <div class="section-box">
            <span class="section-label">2. DETALLE ECONÓMICO</span>
            
            <table id="tablaItems">
                <thead>
                    <tr>
                        <th style="width: 40px; text-align:center;">Del</th>
                        <th style="width: 35%;">CÓDIGO / DESCRIPCIÓN (BUSCADOR)</th>
                        <th style="width: 80px; text-align:center;">CANTIDAD</th>
                        
                        <th style="width: 120px; background:#D6DBDF; color:#2C3E50;">COSTO UNIT.</th>
                        <th style="width: 120px; background:#D6DBDF; color:#2C3E50;">TOTAL COSTO</th>
                        
                        <th style="width: 120px;">VENTA UNIT.</th>
                        <th style="width: 120px;">TOTAL VENTA</th>
                        <th style="width: 100px;">UTILIDAD</th>
                    </tr>
                </thead>
                <tbody>
                    </tbody>
            </table>
            
            <div style="margin-top: 15px;">
                <button onclick="agregarFila()" style="padding: 10px 20px; background: #34495E; color: white; border: none; cursor: pointer; border-radius:4px; font-weight:bold;">+ AGREGAR LÍNEA</button>
                <p style="font-size:10px; color:red; margin-top:5px;">* NOTA: LOS PRECIOS NO SON EDITABLES AQUÍ. DEBE MODIFICAR EL PRODUCTO EN LA BASE DE DATOS SI DESEA CAMBIAR PRECIOS.</p>
            </div>
        </div>

        <div class="totals-wrapper">
            <div class="totals-card">
                <div class="t-row"><span>NETO:</span><span id="txtNeto">$0</span></div>
                <div class="t-row"><span>IVA (19%):</span><span id="txtIva">$0</span></div>
                <div class="t-row t-final"><span>TOTAL A PAGAR:</span><span id="txtTotal">$0</span></div>
                
                <div style="margin-top:20px; padding-top:10px; border-top:1px dashed #ddd; font-size:10px; color:#999; text-align:right;">
                    UTILIDAD PROYECTO: <span id="txtUtilidad" style="color:green; font-weight:bold;">$0</span>
                </div>
            </div>
        </div>

        <div class="action-area">
            <button class="btn-main" onclick="finalizarCotizacion()">📄 GENERAR COTIZACIÓN PDF (PROFESIONAL)</button>
        </div>

    </div>
</div>

<div id="modalGestorClientes" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('clientes')">&times;</span>
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h2>ADMINISTRACIÓN DE CLIENTES</h2>
            <button class="btn-new" onclick="prepararFormulario('cliente')">+ NUEVO CLIENTE</button>
        </div>
        <div style="overflow-x:auto;">
            <table class="manage-table">
                <thead>
                    <tr>
                        <th>RUT</th>
                        <th>RAZÓN SOCIAL</th>
                        <th>GIRO</th>
                        <th>DIRECCIÓN</th>
                        <th>COMUNA</th>
                        <th>REGIÓN</th>
                        <th>CONTACTO</th>
                        <th>EMAIL</th>
                        <th>ACCIONES</th>
                    </tr>
                </thead>
                <tbody id="bodyGestorClientes"></tbody>
            </table>
        </div>
    </div>
</div>

<div id="modalGestorProductos" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('productos')">&times;</span>
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h2>ADMINISTRACIÓN DE PRODUCTOS</h2>
            <button class="btn-new" onclick="prepararFormulario('producto')">+ NUEVO PRODUCTO</button>
        </div>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>CÓDIGO</th>
                    <th>DESCRIPCIÓN</th>
                    <th>COSTO NETO</th>
                    <th>PRECIO VENTA SUGERIDO (NETO)</th>
                    <th>ACCIONES</th>
                </tr>
            </thead>
            <tbody id="bodyGestorProductos"></tbody>
        </table>
    </div>
</div>

<div id="modalHistorial" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('historial')">&times;</span>
        <h2>HISTORIAL DE COTIZACIONES</h2>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>N° COTIZACIÓN</th>
                    <th>FECHA EMISIÓN</th>
                    <th>CLIENTE</th>
                    <th>TOTAL</th>
                </tr>
            </thead>
            <tbody id="bodyHistorial"></tbody>
        </table>
    </div>
</div>

<div id="modalFormulario" class="modal" style="z-index: 2100;">
    <div class="modal-box" style="max-width: 600px;">
        <span class="close-btn" onclick="document.getElementById('modalFormulario').style.display='none'">&times;</span>
        <h2 id="tituloFormulario">...</h2>
        <input type="hidden" id="editIndex"> <input type="hidden" id="editType">  <div id="contenidoFormulario" class="grid-form" style="grid-template-columns: 1fr; margin-top:20px;">
            </div>
        
        <button onclick="guardarDatos()" style="width:100%; margin-top:20px; padding:15px; background:var(--secondary); color:white; border:none; font-weight:bold; cursor:pointer;">GUARDAR DATOS</button>
    </div>
</div>

<script>
    // --- UTILIDADES ---
    const formatMoney = num => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(num);
    const upper = e => e.value = e.value.toUpperCase();

    // --- BASE DE DATOS ---
    const DB = {
        get: k => JSON.parse(localStorage.getItem(k) || "[]"),
        set: (k, v) => localStorage.setItem(k, JSON.stringify(v)),
        init: () => {
            document.getElementById('lblFecha').innerText = new Date().toLocaleDateString('es-CL');
            let seq = localStorage.getItem('cundo_seq') || 50100;
            document.getElementById('lblCorrelativo').innerText = "CN" + seq;
        }
    };

    window.onload = () => {
        DB.init();
        agregarFila(); // Inicia con una fila
    };

    // ==========================================
    // LÓGICA DE BUSCADORES Y TABLA PRINCIPAL
    // ==========================================

    // 1. Buscador Cliente
    function buscarCliente(input) {
        upper(input);
        const term = input.value;
        const list = document.getElementById('listaClientes');
        list.innerHTML = '';
        
        if (term.length < 3) { list.style.display = 'none'; return; }

        const clientes = DB.get('cundo_clientes');
        const results = clientes.filter(c => c.razon.includes(term) || c.rut.includes(term));

        if (results.length > 0) {
            list.style.display = 'block';
            results.forEach(c => {
                const div = document.createElement('div');
                div.className = 'search-item';
                div.innerHTML = `<strong>${c.rut}</strong> - ${c.razon}`;
                div.onclick = () => {
                    document.getElementById('buscadorCliente').value = c.razon;
                    ['cliRazon','cliRut','cliGiro','cliDir','cliComuna','cliRegion','cliContacto','cliEmail'].forEach(id => {
                        document.getElementById(id).value = c[id.replace('cli','').toLowerCase()] || '';
                    });
                    list.style.display = 'none';
                };
                list.appendChild(div);
            });
        } else {
            list.style.display = 'none';
        }
    }

    // 2. Tabla Items (Productos)
    function agregarFila() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center;"><button onclick="this.closest('tr').remove(); calcular();" style="color:red; background:none; border:none; font-weight:bold; cursor:pointer;">X</button></td>
            <td style="position:relative;">
                <input type="text" class="cell-edit" placeholder="ESCRIBA PARA BUSCAR..." onkeyup="buscarProductoFila(this)" oninput="upper(this)">
                <div class="search-results"></div>
            </td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            
            <td><input type="text" class="cell-edit cell-locked costo-unit" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked costo-total" readonly value="$0"></td>
            
            <td><input type="text" class="cell-edit cell-locked venta-unit" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked venta-total" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked utilidad" readonly value="$0" style="font-size:10px;"></td>
            
            <input type="hidden" class="val-costo" value="0">
            <input type="hidden" class="val-venta" value="0">
        `;
        tbody.appendChild(tr);
    }

    function buscarProductoFila(input) {
        const term = input.value;
        const list = input.nextElementSibling;
        list.innerHTML = '';
        
        // Si borra, limpiamos valores
        if(term.length === 0) {
            limpiarFila(input.closest('tr'));
            return;
        }

        const prods = DB.get('cundo_productos');
        const results = prods.filter(p => p.cod.includes(term) || p.desc.includes(term));

        if(results.length > 0) {
            list.style.display = 'block';
            results.forEach(p => {
                const div = document.createElement('div');
                div.className = 'search-item';
                div.innerHTML = `<strong>${p.cod}</strong> - ${p.desc}`;
                div.onclick = () => {
                    input.value = `${p.cod} - ${p.desc}`;
                    llenarFila(input.closest('tr'), p);
                    list.style.display = 'none';
                };
                list.appendChild(div);
            });
        } else {
            list.style.display = 'none';
        }
    }

    function llenarFila(tr, prod) {
        // Guardamos valores reales numéricos en hidden inputs para calcular
        tr.querySelector('.val-costo').value = prod.costo;
        tr.querySelector('.val-venta').value = prod.precio; // Precio de venta guardado

        // Mostramos formateado
        tr.querySelector('.costo-unit').value = formatMoney(prod.costo);
        tr.querySelector('.venta-unit').value = formatMoney(prod.precio);
        
        calcular();
    }

    function limpiarFila(tr) {
        tr.querySelector('.val-costo').value = 0;
        tr.querySelector('.val-venta').value = 0;
        tr.querySelector('.costo-unit').value = "$0";
        tr.querySelector('.venta-unit').value = "$0";
        calcular();
    }

    function calcular() {
        let neto = 0;
        let utilidadTotal = 0;

        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const cant = parseFloat(tr.querySelector('.cell-qty').value) || 0;
            const costo = parseFloat(tr.querySelector('.val-costo').value) || 0;
            const venta = parseFloat(tr.querySelector('.val-venta').value) || 0;

            const tCosto = cant * costo;
            const tVenta = cant * venta;
            const util = tVenta - tCosto;

            tr.querySelector('.costo-total').value = formatMoney(tCosto);
            tr.querySelector('.venta-total').value = formatMoney(tVenta);
            tr.querySelector('.utilidad').value = formatMoney(util);

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

    // ==========================================
    // GESTIÓN DE MODALES (CRUD COMPLETO)
    // ==========================================

    function abrirGestor(tipo) {
        document.querySelectorAll('.modal').forEach(m => m.style.display = 'none');
        if(tipo === 'clientes') {
            document.getElementById('modalGestorClientes').style.display = 'flex';
            renderClientes();
        }
        if(tipo === 'productos') {
            document.getElementById('modalGestorProductos').style.display = 'flex';
            renderProductos();
        }
        if(tipo === 'historial') {
            document.getElementById('modalHistorial').style.display = 'flex';
            renderHistorial();
        }
    }

    function cerrarGestor(id) {
        if(id === 'clientes') document.getElementById('modalGestorClientes').style.display = 'none';
        if(id === 'productos') document.getElementById('modalGestorProductos').style.display = 'none';
        if(id === 'historial') document.getElementById('modalHistorial').style.display = 'none';
    }

    // --- Render Tablas ---
    function renderClientes() {
        const tbody = document.getElementById('bodyGestorClientes');
        tbody.innerHTML = '';
        DB.get('cundo_clientes').forEach((c, i) => {
            tbody.innerHTML += `
                <tr>
                    <td>${c.rut}</td><td>${c.razon}</td><td>${c.giro}</td><td>${c.dir}</td>
                    <td>${c.comuna}</td><td>${c.region}</td><td>${c.contacto}</td><td>${c.email}</td>
                    <td>
                        <div class="btn-action-group">
                            <button class="btn-small btn-edit" onclick="prepararFormulario('cliente', ${i})">✏️</button>
                            <button class="btn-small btn-del" onclick="borrar('cundo_clientes', ${i})">🗑️</button>
                        </div>
                    </td>
                </tr>`;
        });
    }

    function renderProductos() {
        const tbody = document.getElementById('bodyGestorProductos');
        tbody.innerHTML = '';
        DB.get('cundo_productos').forEach((p, i) => {
            tbody.innerHTML += `
                <tr>
                    <td>${p.cod}</td><td>${p.desc}</td><td>${formatMoney(p.costo)}</td><td>${formatMoney(p.precio)}</td>
                    <td>
                        <div class="btn-action-group">
                            <button class="btn-small btn-edit" onclick="prepararFormulario('producto', ${i})">✏️</button>
                            <button class="btn-small btn-del" onclick="borrar('cundo_productos', ${i})">🗑️</button>
                        </div>
                    </td>
                </tr>`;
        });
    }

    function renderHistorial() {
        const tbody = document.getElementById('bodyHistorial');
        tbody.innerHTML = '';
        DB.get('cundo_historial').reverse().forEach(h => {
            tbody.innerHTML += `<tr><td>${h.n}</td><td>${h.fecha}</td><td>${h.cliente}</td><td>${h.total}</td></tr>`;
        });
    }

    function borrar(key, idx) {
        if(confirm("¿ELIMINAR REGISTRO?")) {
            const d = DB.get(key);
            d.splice(idx, 1);
            DB.set(key, d);
            if(key.includes('clientes')) renderClientes();
            if(key.includes('productos')) renderProductos();
        }
    }

    // ==========================================
    // FORMULARIO DE CREACIÓN / EDICIÓN
    // ==========================================

    function prepararFormulario(tipo, idx = null) {
        const modal = document.getElementById('modalFormulario');
        const container = document.getElementById('contenidoFormulario');
        const titulo = document.getElementById('tituloFormulario');
        const hiddenIdx = document.getElementById('editIndex');
        const hiddenType = document.getElementById('editType');

        modal.style.display = 'flex';
        container.innerHTML = '';
        hiddenType.value = tipo;
        hiddenIdx.value = idx !== null ? idx : -1;

        if(tipo === 'cliente') {
            titulo.innerText = idx !== null ? "EDITAR CLIENTE" : "NUEVO CLIENTE";
            // Obtener datos si es edición
            let data = {};
            if(idx !== null) data = DB.get('cundo_clientes')[idx];

            const fields = [
                { id: 'rut', label: 'RUT', val: data.rut || '' },
                { id: 'razon', label: 'RAZÓN SOCIAL', val: data.razon || '' },
                { id: 'giro', label: 'GIRO', val: data.giro || '' },
                { id: 'dir', label: 'DIRECCIÓN', val: data.dir || '' },
                { id: 'comuna', label: 'COMUNA', val: data.comuna || '' },
                { id: 'region', label: 'REGIÓN', val: data.region || '' },
                { id: 'contacto', label: 'NOMBRE CONTACTO', val: data.contacto || '' },
                { id: 'email', label: 'EMAIL', val: data.email || '' }
            ];

            fields.forEach(f => {
                container.innerHTML += `
                    <div class="input-group">
                        <label>${f.label}</label>
                        <input type="text" id="f_${f.id}" value="${f.val}" oninput="upper(this)">
                    </div>`;
            });

        } else {
            titulo.innerText = idx !== null ? "EDITAR PRODUCTO" : "NUEVO PRODUCTO";
            let data = {};
            if(idx !== null) data = DB.get('cundo_productos')[idx];

            container.innerHTML += `
                <div class="input-group"><label>CÓDIGO INTERNO</label><input type="text" id="f_cod" value="${data.cod||''}" oninput="upper(this)"></div>
                <div class="input-group"><label>DESCRIPCIÓN</label><input type="text" id="f_desc" value="${data.desc||''}" oninput="upper(this)"></div>
                <div class="input-group"><label>COSTO NETO ($)</label><input type="number" id="f_costo" value="${data.costo||''}"></div>
                <div class="input-group"><label>PRECIO VENTA NETO ($)</label><input type="number" id="f_precio" value="${data.precio||''}"></div>
            `;
        }
    }

    function guardarDatos() {
        const tipo = document.getElementById('editType').value;
        const idx = parseInt(document.getElementById('editIndex').value);
        
        if(tipo === 'cliente') {
            const nuevo = {
                rut: document.getElementById('f_rut').value,
                razon: document.getElementById('f_razon').value,
                giro: document.getElementById('f_giro').value,
                dir: document.getElementById('f_dir').value,
                comuna: document.getElementById('f_comuna').value,
                region: document.getElementById('f_region').value,
                contacto: document.getElementById('f_contacto').value,
                email: document.getElementById('f_email').value
            };
            if(!nuevo.rut) return alert("RUT OBLIGATORIO");

            const db = DB.get('cundo_clientes');
            if(idx === -1) db.push(nuevo);
            else db[idx] = nuevo;
            
            DB.set('cundo_clientes', db);
            renderClientes();

        } else {
            const nuevo = {
                cod: document.getElementById('f_cod').value,
                desc: document.getElementById('f_desc').value,
                costo: parseFloat(document.getElementById('f_costo').value) || 0,
                precio: parseFloat(document.getElementById('f_precio').value) || 0
            };
            if(!nuevo.cod) return alert("CÓDIGO OBLIGATORIO");

            const db = DB.get('cundo_productos');
            if(idx === -1) db.push(nuevo);
            else db[idx] = nuevo;

            DB.set('cundo_productos', db);
            renderProductos();
        }

        document.getElementById('modalFormulario').style.display = 'none';
    }

    // ==========================================
    // PDF GENERATION (PROFESIONAL Y PEQUEÑO)
    // ==========================================
    function finalizarCotizacion() {
        const cliente = document.getElementById('cliRazon').value;
        if(!cliente) return alert("SELECCIONE UN CLIENTE PRIMERO");

        const nCot = document.getElementById('lblCorrelativo').innerText;
        const total = document.getElementById('txtTotal').innerText;

        // Guardar Historial
        const h = DB.get('cundo_historial');
        h.push({ n: nCot, fecha: new Date().toLocaleDateString('es-CL'), cliente: cliente, total: total });
        DB.set('cundo_historial', h);

        // Aumentar Correlativo
        let seq = parseInt(nCot.replace('CN',''));
        localStorage.setItem('cundo_seq', seq + 1);

        generarPDF(nCot);
    }

    function generarPDF(numero) {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();

        const azul = [31, 111, 139];
        const naranja = [242, 92, 5];

        // 1. LATERAL
        doc.setFillColor(...azul);
        doc.rect(0, 0, 12, 297, 'F');

        // 2. HEADER
        doc.setFontSize(24);
        doc.setFont("helvetica", "bold");
        doc.setTextColor(...azul);
        doc.text("CUNDO SPA", 20, 20);
        
        doc.setFontSize(8);
        doc.setTextColor(100);
        doc.setFont("helvetica", "normal");
        doc.text("SOLUCIONES INTEGRALES DE INGENIERÍA", 20, 25);
        doc.text("RUT: 77.XXX.XXX-X | CONTACTO@CUNDOSPA.CL", 20, 29);

        // CAJA COTIZACIÓN
        doc.setFillColor(245, 245, 245);
        doc.rect(140, 10, 60, 20, 'F');
        doc.setTextColor(...naranja);
        doc.setFontSize(12);
        doc.setFont("helvetica", "bold");
        doc.text("COTIZACIÓN", 170, 16, { align: "center" });
        doc.setTextColor(0);
        doc.text(numero, 170, 24, { align: "center" });
        doc.setFontSize(8);
        doc.setFont("helvetica", "normal");
        doc.text(document.getElementById('lblFecha').innerText, 170, 28, { align: "center" });

        // CLIENTE
        doc.setDrawColor(200);
        doc.line(20, 35, 200, 35);
        
        doc.setFontSize(8);
        doc.setFont("helvetica", "bold");
        doc.setTextColor(...azul);
        doc.text("INFORMACIÓN DEL CLIENTE:", 20, 42);
        
        doc.setTextColor(0);
        doc.setFont("helvetica", "normal");
        const rz = document.getElementById('cliRazon').value;
        const rut = document.getElementById('cliRut').value;
        const dir = document.getElementById('cliDir').value;
        const com = document.getElementById('cliComuna').value;
        const con = document.getElementById('cliContacto').value;

        doc.text(`RAZÓN SOCIAL: ${rz}`, 20, 48);
        doc.text(`RUT: ${rut}`, 120, 48);
        doc.text(`DIRECCIÓN: ${dir}, ${com}`, 20, 53);
        doc.text(`ATENCIÓN: ${con}`, 120, 53);

        // TABLA
        const rows = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.cell-edit').value; // Input búsqueda tiene descripción
            if(!desc) return;
            const cant = tr.querySelector('.cell-qty').value;
            const unit = tr.querySelector('.val-venta').value; // Usar valor crudo
            const tot = parseFloat(cant) * parseFloat(unit);
            
            rows.push([cant, desc, formatMoney(unit), formatMoney(tot)]);
        });

        doc.autoTable({
            startY: 60,
            head: [['CANT', 'DESCRIPCIÓN', 'P.UNITARIO', 'TOTAL']],
            body: rows,
            theme: 'plain',
            styles: { fontSize: 8, cellPadding: 3 }, // LETRA PEQUEÑA SOLICITADA
            headStyles: { fillColor: azul, textColor: 255, fontStyle: 'bold', fontSize: 8 },
            columnStyles: {
                0: { halign: 'center', cellWidth: 15 },
                2: { halign: 'right', cellWidth: 30 },
                3: { halign: 'right', cellWidth: 30, fontStyle: 'bold' }
            },
            didParseCell: function(data) {
                if(data.section === 'body' && data.row.index % 2 === 0) data.cell.styles.fillColor = [250, 250, 250];
            }
        });

        // TOTALES
        const finalY = doc.lastAutoTable.finalY + 10;
        const neto = document.getElementById('txtNeto').innerText;
        const iva = document.getElementById('txtIva').innerText;
        const tot = document.getElementById('txtTotal').innerText;

        doc.setFontSize(9);
        doc.text("NETO:", 150, finalY);
        doc.text(neto, 195, finalY, { align: 'right' });
        doc.text("IVA (19%):", 150, finalY + 5);
        doc.text(iva, 195, finalY + 5, { align: 'right' });

        doc.setFontSize(11);
        doc.setFont("helvetica", "bold");
        doc.setTextColor(...azul);
        doc.text("TOTAL:", 150, finalY + 12);
        doc.text(tot, 195, finalY + 12, { align: 'right' });

        // FOOTER
        doc.setTextColor(150);
        doc.setFontSize(7);
        doc.setFont("helvetica", "normal");
        doc.text("DOCUMENTO GENERADO POR SISTEMA INTERNO CUNDO SPA - VÁLIDO POR 15 DÍAS", 105, 290, { align: "center" });

        doc.save(`Cotizacion_${numero}.pdf`);
        setTimeout(() => location.reload(), 1500); // Recargar para actualizar ID
    }

</script>

</body>
</html>
