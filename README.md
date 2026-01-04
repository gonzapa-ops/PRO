<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA PRO V8 - CUNDO SPA</title>
    
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
            --green: #27AE60;
            --blue-util: #2980B9;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; text-transform: uppercase; }
        body { background: var(--bg); padding: 15px; color: var(--text); font-size: 11px; padding-bottom: 80px; }
        
        .main-container { max-width: 1400px; margin: 0 auto; background: white; min-height: 100vh; box-shadow: 0 5px 25px rgba(0,0,0,0.1); border-radius: 6px; overflow: hidden; display: flex; flex-direction: column; }

        /* HEADER */
        .top-bar { background: var(--dark); color: white; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; }
        .top-menu { display: flex; gap: 10px; }
        .top-menu button { background: #34495E; border: 1px solid #566573; color: white; padding: 6px 15px; cursor: pointer; border-radius: 3px; font-size: 10px; font-weight: bold; transition: 0.2s; display: flex; align-items: center; gap: 5px; }
        .top-menu button:hover { background: var(--secondary); border-color: var(--secondary); }

        .header { padding: 25px 30px; display: flex; justify-content: space-between; border-bottom: 3px solid var(--secondary); background: white; }
        .brand h1 { color: var(--primary); font-size: 26px; font-weight: 900; letter-spacing: -0.5px; margin: 0; }
        .brand p { color: #7F8C8D; font-size: 10px; font-weight: 600; margin-top: 4px; }

        .quote-data { text-align: right; }
        .quote-number { font-size: 20px; font-weight: bold; color: var(--secondary); display: block; }
        .quote-date { font-size: 11px; font-weight: bold; color: var(--primary); margin-top: 5px; display: block; }

        /* AVISO DE EDICIÓN */
        #avisoEdicion { display: none; background: #FFF3CD; color: #856404; padding: 10px; text-align: center; border-bottom: 1px solid #FFEEBA; font-weight: bold; font-size: 12px; }

        /* CONTENIDO */
        .content { padding: 25px; }
        .section-box { margin-bottom: 25px; border: 1px solid #D5DBDB; border-radius: 5px; padding: 20px; background: #FBFCFC; position: relative; }
        .section-label { position: absolute; top: -10px; left: 15px; background: var(--primary); color: white; padding: 3px 10px; font-size: 10px; font-weight: bold; border-radius: 3px; letter-spacing: 0.5px; }

        /* INPUTS Y FORMULARIOS */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 12px; }
        .input-group label { display: block; font-size: 9px; font-weight: bold; color: #7F8C8D; margin-bottom: 4px; }
        .input-group input { width: 100%; padding: 6px 10px; border: 1px solid #D5DBDB; border-radius: 3px; font-size: 11px; font-weight: 600; color: var(--dark); height: 30px; }
        .input-group input:focus { border-color: var(--secondary); outline: none; background: #fff; }
        .input-group input[readonly] { background: #F2F4F4; color: #777; cursor: default; border-color: #E5E7E9; }

        /* BUSCADOR */
        .search-container { position: relative; margin-bottom: 15px; }
        .search-results { 
            position: absolute; top: 100%; left: 0; width: 100%; 
            background: white; border: 1px solid #3498DB; box-shadow: 0 8px 20px rgba(0,0,0,0.15); 
            z-index: 500; display: none; max-height: 250px; overflow-y: auto; border-radius: 0 0 5px 5px;
        }
        .search-item { padding: 10px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 11px; color: #333; }
        .search-item:hover { background: #EBF5FB; color: var(--primary); font-weight: bold; }
        .create-new-option { background: #FDEDEC; color: #C0392B; font-weight: bold; border-top: 1px solid #F5B7B1; }

        /* TABLA DETALLE */
        table { width: 100%; border-collapse: collapse; font-size: 10px; }
        th { background: var(--primary); color: white; padding: 10px; text-align: left; font-weight: 700; border-right: 1px solid rgba(255,255,255,0.1); }
        td { border: 1px solid #D5DBDB; padding: 0; vertical-align: middle; height: 32px; }
        
        input.cell-edit { width: 100%; height: 100%; border: none; padding: 0 8px; font-size: 11px; font-family: inherit; color: #333; background: transparent; }
        input.cell-edit:focus { outline: 2px solid var(--secondary); background: white; z-index: 10; position: relative; }
        input.cell-locked { background: #F8F9F9; color: #7F8C8D; text-align: right; cursor: not-allowed; }
        input.cell-qty { text-align: center; font-weight: bold; color: var(--primary); }

        /* TOTALES REVISADOS */
        .totals-wrapper { display: flex; justify-content: flex-end; margin-top: 20px; }
        .totals-card { width: 450px; background: white; border: 1px solid #D5DBDB; padding: 15px; border-radius: 5px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 6px; font-size: 11px; font-weight: 600; color: #555; }
        
        /* Fila final especial con Utilidad a la izquierda */
        .final-row-container { 
            display: grid; 
            grid-template-columns: 1fr 1fr; 
            gap: 15px; 
            margin-top: 15px; 
            padding-top: 15px; 
            border-top: 2px solid var(--secondary); 
        }
        
        .util-box { text-align: right; color: var(--blue-util); }
        .util-box span { display: block; font-size: 9px; color: #7f8c8d; }
        .util-box strong { font-size: 16px; font-weight: 900; }

        .total-box { text-align: right; color: var(--primary); }
        .total-box span { display: block; font-size: 9px; color: #7f8c8d; }
        .total-box strong { font-size: 18px; font-weight: 900; }

        /* BOTONES */
        .action-area { text-align: center; margin-top: 30px; border-top: 1px dashed #BDC3C7; padding-top: 20px; }
        .btn-main { background: var(--secondary); color: white; padding: 12px 50px; font-size: 14px; font-weight: 800; border: none; border-radius: 4px; cursor: pointer; transition: all 0.2s; box-shadow: 0 4px 10px rgba(242, 92, 5, 0.3); }
        .btn-main:hover { background: #D35400; transform: translateY(-1px); }
        .btn-add { padding: 8px 15px; background: #34495E; color: white; border: none; border-radius: 3px; cursor: pointer; font-size: 10px; font-weight: bold; }

        /* MODALES */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 2000; align-items: center; justify-content: center; backdrop-filter: blur(2px); }
        .modal-box { background: white; width: 95%; max-width: 900px; max-height: 90vh; overflow-y: auto; padding: 25px; border-radius: 6px; box-shadow: 0 10px 40px rgba(0,0,0,0.4); }
        .close-btn { float: right; font-size: 24px; cursor: pointer; color: #E74C3C; font-weight: bold; }
        
        .manage-table { width: 100%; margin-top: 15px; border: 1px solid #ddd; }
        .manage-table th { padding: 8px; font-size: 10px; background: #2C3E50; color: white; }
        .manage-table td { padding: 6px; font-size: 10px; border-bottom: 1px solid #eee; }
        
        .btn-small { padding: 4px 8px; border: none; color: white; border-radius: 2px; cursor: pointer; font-size: 9px; margin-right: 4px; font-weight: bold; }
        .btn-edit { background: #F39C12; color: white; }
        .btn-del { background: #C0392B; }
        .btn-new { background: var(--primary); padding: 6px 15px; color: white; border: none; border-radius: 3px; cursor: pointer; font-size: 10px; font-weight: bold; float: right; }

    </style>
</head>
<body>

<div class="main-container">
    
    <div id="avisoEdicion">⚠️ ESTÁS EDITANDO UNA COTIZACIÓN ANTIGUA. AL GUARDAR SE ACTUALIZARÁ EL REGISTRO EXISTENTE.</div>

    <div class="top-bar">
        <div style="font-weight: 800; font-size: 12px; color: #BDC3C7;">PANEL ADMINISTRATIVO</div>
        <div class="top-menu">
            <button onclick="abrirGestor('clientes')">👥 CLIENTES</button>
            <button onclick="abrirGestor('productos')">📦 PRODUCTOS</button>
            <button onclick="abrirGestor('historial')">📜 HISTORIAL</button>
            <button onclick="limpiarYRecargar()" style="background:#C0392B;">🔄 LIMPIAR / NUEVA</button>
        </div>
    </div>

    <div class="header">
        <div class="brand">
            <h1>CUNDO SPA</h1>
            <p>INGENIERÍA, SERVICIOS Y SOLUCIONES INTEGRALES</p>
        </div>
        <div class="quote-data">
            <span style="font-size:9px; color:#999;">N° COTIZACIÓN</span>
            <span class="quote-number" id="lblCorrelativo">...</span>
            <span class="quote-date" id="lblFecha"></span>
        </div>
    </div>

    <div class="content">

        <div class="section-box">
            <span class="section-label">1. INFORMACIÓN DEL CLIENTE</span>
            
            <div class="input-group search-container">
                <input type="text" id="buscadorCliente" placeholder="🔍 BUSCAR POR RUT O NOMBRE..." onkeyup="buscarCliente(this)" autocomplete="off" style="border: 2px solid var(--primary); background:#fff;">
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
                        <th style="width: 35px; text-align:center;">X</th>
                        <th style="width: 40%;">CÓDIGO / DESCRIPCIÓN (BUSCADOR)</th>
                        <th style="width: 60px; text-align:center;">CANT.</th>
                        
                        <th style="width: 90px; background:#EBEDEF; color:#555;">COSTO U.</th>
                        <th style="width: 90px; background:#EBEDEF; color:#555;">TOTAL COSTO</th>
                        
                        <th style="width: 90px;">VENTA U.</th>
                        <th style="width: 90px;">TOTAL VENTA</th>
                        <th style="width: 70px;">UTILIDAD</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
            
            <div style="margin-top: 15px; display:flex; justify-content:space-between; align-items:center;">
                <button onclick="agregarFila()" class="btn-add">+ AGREGAR LÍNEA</button>
                <span style="font-size:10px; color:#C0392B; font-weight:bold;">* PRECIOS SE EDITAN EN BASE DE PRODUCTOS</span>
            </div>
        </div>

        <div class="totals-wrapper">
            <div class="totals-card">
                <div class="t-row"><span>SUBTOTAL NETO:</span><span id="txtNeto">$0</span></div>
                <div class="t-row"><span>IVA (19%):</span><span id="txtIva">$0</span></div>
                
                <div class="final-row-container">
                    <div class="util-box">
                        <span>UTILIDAD PROYECTO</span>
                        <strong id="txtUtilidad">$0</strong>
                    </div>
                    <div class="total-box">
                        <span>TOTAL A PAGAR</span>
                        <strong id="txtTotal">$0</strong>
                    </div>
                </div>
            </div>
        </div>

        <div class="action-area">
            <input type="hidden" id="indiceEdicionHistorial" value="-1">
            <button class="btn-main" onclick="finalizarCotizacion()">📄 GUARDAR Y GENERAR PDF</button>
        </div>
    </div>
</div>

<div id="modalGestorClientes" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('clientes')">&times;</span>
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
            <h3>BASE DE CLIENTES</h3>
            <button class="btn-new" onclick="prepararFormulario('cliente')">+ NUEVO</button>
        </div>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>RUT</th>
                    <th>RAZÓN SOCIAL</th>
                    <th>GIRO</th>
                    <th>CONTACTO</th>
                    <th>ACCIONES</th>
                </tr>
            </thead>
            <tbody id="bodyGestorClientes"></tbody>
        </table>
    </div>
</div>

<div id="modalGestorProductos" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('productos')">&times;</span>
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
            <h3>BASE DE PRODUCTOS</h3>
            <button class="btn-new" onclick="prepararFormulario('producto')">+ NUEVO</button>
        </div>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>CÓDIGO</th>
                    <th>DESCRIPCIÓN</th>
                    <th>COSTO</th>
                    <th>VENTA</th>
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
        <h3>HISTORIAL DE COTIZACIONES</h3>
        <p style="font-size:10px; margin-bottom:10px;">AQUÍ PUEDES VOLVER A CARGAR COTIZACIONES ANTIGUAS PARA EDITARLAS O BORRARLAS.</p>
        <table class="manage-table">
            <thead>
                <tr>
                    <th>N°</th>
                    <th>FECHA</th>
                    <th>CLIENTE</th>
                    <th>TOTAL</th>
                    <th>ACCIONES</th>
                </tr>
            </thead>
            <tbody id="bodyHistorial"></tbody>
        </table>
    </div>
</div>

<div id="modalFormulario" class="modal" style="z-index: 2100;">
    <div class="modal-box" style="max-width: 500px;">
        <span class="close-btn" onclick="document.getElementById('modalFormulario').style.display='none'">&times;</span>
        <h3 id="tituloFormulario" style="margin-bottom:20px; color:var(--primary);">...</h3>
        <input type="hidden" id="editIndex">
        <input type="hidden" id="editType">
        <div id="contenidoFormulario" class="grid-form" style="grid-template-columns: 1fr;"></div>
        <button onclick="guardarDatos()" class="btn-main" style="width:100%; margin-top:20px;">GUARDAR</button>
    </div>
</div>

<script>
    // --- 1. CONFIGURACIÓN ---
    const formatMoney = num => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(num);
    const upper = e => e.value = e.value.toUpperCase();
    
    const formatearRut = (input) => {
        let val = input.value.replace(/[^0-9kK]/g, '').toUpperCase();
        if (val.length > 1) {
            const dv = val.slice(-1);
            const cuerpo = val.slice(0, -1);
            input.value = `${cuerpo}-${dv}`;
        } else {
            input.value = val;
        }
    };

    // --- 2. BASE DE DATOS ---
    const DB = {
        get: k => JSON.parse(localStorage.getItem(k) || "[]"),
        set: (k, v) => localStorage.setItem(k, JSON.stringify(v)),
        init: () => {
            document.getElementById('lblFecha').innerText = new Date().toLocaleDateString('es-CL');
            // Solo carga el siguiente si NO estamos editando
            if(document.getElementById('indiceEdicionHistorial').value == "-1") {
                let seq = localStorage.getItem('cundo_seq') || 50100;
                document.getElementById('lblCorrelativo').innerText = "CN" + seq;
            }
        }
    };

    window.onload = () => {
        DB.init();
        agregarFila(); 
    };

    function limpiarYRecargar() {
        if(confirm("¿Limpiar todo y comenzar una nueva cotización?")) {
            location.reload();
        }
    }

    // --- 3. BUSCADOR CLIENTES ---
    function buscarCliente(input) {
        upper(input);
        const term = input.value.trim();
        const list = document.getElementById('listaClientes');
        list.innerHTML = '';
        
        if (term.length < 2) { list.style.display = 'none'; return; }

        const clientes = DB.get('cundo_clientes');
        const results = clientes.filter(c => c.razon.toUpperCase().includes(term) || c.rut.toUpperCase().includes(term));

        list.style.display = 'block';
        if (results.length > 0) {
            results.forEach(c => {
                const div = document.createElement('div');
                div.className = 'search-item';
                div.innerHTML = `<strong>${c.rut}</strong> - ${c.razon}`;
                div.onclick = () => {
                    cargarDatosCliente(c);
                    list.style.display = 'none';
                };
                list.appendChild(div);
            });
        } 
        
        const btnCrear = document.createElement('div');
        btnCrear.className = 'search-item create-new-option';
        btnCrear.innerHTML = `⛔ NO EXISTE. <strong>+ CREAR CLIENTE</strong>`;
        btnCrear.onclick = () => { list.style.display = 'none'; prepararFormulario('cliente'); };
        list.appendChild(btnCrear);
    }

    function cargarDatosCliente(c) {
        document.getElementById('buscadorCliente').value = c.razon;
        ['cliRazon','cliRut','cliGiro','cliDir','cliComuna','cliRegion','cliContacto','cliEmail'].forEach(id => {
            document.getElementById(id).value = c[id.replace('cli','').toLowerCase()] || '';
        });
    }

    // --- 4. TABLA PRODUCTOS ---
    function agregarFila() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center;"><button onclick="this.closest('tr').remove(); calcular();" style="color:red; background:none; border:none; font-weight:bold; cursor:pointer;">X</button></td>
            <td style="position:relative;">
                <input type="text" class="cell-edit" placeholder="BUSCAR..." onkeyup="buscarProductoFila(this)" oninput="upper(this)">
                <div class="search-results"></div>
            </td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            <td><input type="text" class="cell-edit cell-locked costo-unit" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked costo-total" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked venta-unit" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked venta-total" readonly value="$0"></td>
            <td><input type="text" class="cell-edit cell-locked utilidad" readonly value="$0" style="font-size:9px;"></td>
            <input type="hidden" class="val-costo" value="0">
            <input type="hidden" class="val-venta" value="0">
        `;
        tbody.appendChild(tr);
    }

    function buscarProductoFila(input) {
        const term = input.value.trim().toUpperCase();
        const list = input.nextElementSibling;
        list.innerHTML = '';
        if(term.length === 0) { limpiarFila(input.closest('tr')); list.style.display = 'none'; return; }

        const prods = DB.get('cundo_productos');
        const results = prods.filter(p => p.cod.toUpperCase().includes(term) || p.desc.toUpperCase().includes(term));

        list.style.display = 'block';
        if (results.length > 0) {
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
        }
        const btnCrear = document.createElement('div');
        btnCrear.className = 'search-item create-new-option';
        btnCrear.innerHTML = `📦 NO EXISTE. <strong>+ CREAR PRODUCTO</strong>`;
        btnCrear.onclick = () => { list.style.display = 'none'; prepararFormulario('producto'); };
        list.appendChild(btnCrear);
    }

    function llenarFila(tr, prod) {
        tr.querySelector('.val-costo').value = prod.costo;
        tr.querySelector('.val-venta').value = prod.precio;
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
        let utilTotal = 0;

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
            utilTotal += util;
        });

        const iva = neto * 0.19;
        document.getElementById('txtNeto').innerText = formatMoney(neto);
        document.getElementById('txtIva').innerText = formatMoney(iva);
        document.getElementById('txtTotal').innerText = formatMoney(neto + iva);
        document.getElementById('txtUtilidad').innerText = formatMoney(utilTotal);
    }

    // --- 5. GESTIÓN MODALES CRUD ---
    function abrirGestor(id) {
        document.querySelectorAll('.modal').forEach(m => m.style.display = 'none');
        if(id === 'clientes') { document.getElementById('modalGestorClientes').style.display = 'flex'; renderClientes(); }
        if(id === 'productos') { document.getElementById('modalGestorProductos').style.display = 'flex'; renderProductos(); }
        if(id === 'historial') { document.getElementById('modalHistorial').style.display = 'flex'; renderHistorial(); }
    }
    
    function cerrarGestor(id) { 
        if(id === 'clientes') document.getElementById('modalGestorClientes').style.display = 'none';
        if(id === 'productos') document.getElementById('modalGestorProductos').style.display = 'none';
        if(id === 'historial') document.getElementById('modalHistorial').style.display = 'none';
    }

    function renderClientes() {
        const tbody = document.getElementById('bodyGestorClientes');
        tbody.innerHTML = '';
        DB.get('cundo_clientes').forEach((c, i) => {
            tbody.innerHTML += `<tr><td>${c.rut}</td><td>${c.razon}</td><td>${c.giro}</td><td>${c.contacto}</td>
            <td><button class="btn-small btn-edit" onclick="prepararFormulario('cliente',${i})">✏️</button><button class="btn-small btn-del" onclick="borrar('cundo_clientes',${i})">🗑️</button></td></tr>`;
        });
    }

    function renderProductos() {
        const tbody = document.getElementById('bodyGestorProductos');
        tbody.innerHTML = '';
        DB.get('cundo_productos').forEach((p, i) => {
            tbody.innerHTML += `<tr><td>${p.cod}</td><td>${p.desc}</td><td>${formatMoney(p.costo)}</td><td>${formatMoney(p.precio)}</td>
            <td><button class="btn-small btn-edit" onclick="prepararFormulario('producto',${i})">✏️</button><button class="btn-small btn-del" onclick="borrar('cundo_productos',${i})">🗑️</button></td></tr>`;
        });
    }

    // --- LÓGICA DE HISTORIAL AVANZADO (EDITAR/BORRAR) ---
    function renderHistorial() {
        const tbody = document.getElementById('bodyHistorial');
        tbody.innerHTML = '';
        const data = DB.get('cundo_historial');
        
        // Iteramos para mostrar (usamos índice real para poder editar/borrar correctamente)
        data.forEach((h, i) => {
            tbody.innerHTML += `
            <tr>
                <td><strong>${h.n}</strong></td>
                <td>${h.fecha}</td>
                <td>${h.cliente}</td>
                <td>${h.total}</td>
                <td>
                    <button class="btn-small btn-edit" onclick="cargarCotizacionDesdeHistorial(${i})">✏️ EDITAR</button>
                    <button class="btn-small btn-del" onclick="borrarHistorial(${i})">🗑️ BORRAR</button>
                </td>
            </tr>`;
        });
    }

    function borrarHistorial(index) {
        if(confirm("¿Estás seguro de ELIMINAR permanentemente esta cotización del historial?")) {
            const h = DB.get('cundo_historial');
            h.splice(index, 1);
            DB.set('cundo_historial', h);
            renderHistorial();
        }
    }

    function cargarCotizacionDesdeHistorial(index) {
        if(!confirm("Esto cargará la cotización en la pantalla principal para editarla. ¿Continuar?")) return;
        
        const h = DB.get('cundo_historial');
        const cot = h[index];

        // 1. Marcar modo edición
        document.getElementById('indiceEdicionHistorial').value = index;
        document.getElementById('avisoEdicion').style.display = 'block';
        document.getElementById('lblCorrelativo').innerText = cot.n; // Mantener ID original
        document.getElementById('lblFecha').innerText = cot.fecha;

        // 2. Cargar Cliente
        if(cot.datosCliente) {
            cargarDatosCliente(cot.datosCliente);
        }

        // 3. Cargar Productos
        const tbody = document.querySelector('#tablaItems tbody');
        tbody.innerHTML = ''; // Limpiar tabla actual
        
        if(cot.items && cot.items.length > 0) {
            cot.items.forEach(item => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td style="text-align:center;"><button onclick="this.closest('tr').remove(); calcular();" style="color:red; background:none; border:none; font-weight:bold; cursor:pointer;">X</button></td>
                    <td style="position:relative;">
                        <input type="text" class="cell-edit" value="${item.desc}" onkeyup="buscarProductoFila(this)" oninput="upper(this)">
                        <div class="search-results"></div>
                    </td>
                    <td><input type="number" class="cell-edit cell-qty" value="${item.cant}" min="1" oninput="calcular()"></td>
                    <td><input type="text" class="cell-edit cell-locked costo-unit" readonly></td>
                    <td><input type="text" class="cell-edit cell-locked costo-total" readonly></td>
                    <td><input type="text" class="cell-edit cell-locked venta-unit" readonly></td>
                    <td><input type="text" class="cell-edit cell-locked venta-total" readonly></td>
                    <td><input type="text" class="cell-edit cell-locked utilidad" readonly style="font-size:9px;"></td>
                    <input type="hidden" class="val-costo" value="${item.costo}">
                    <input type="hidden" class="val-venta" value="${item.precio}">
                `;
                tbody.appendChild(tr);
                
                // Formatear visualmente
                tr.querySelector('.costo-unit').value = formatMoney(item.costo);
                tr.querySelector('.venta-unit').value = formatMoney(item.precio);
            });
            calcular();
        }

        cerrarGestor('historial');
    }

    function borrar(key, idx) {
        if(confirm("¿Eliminar?")) {
            const d = DB.get(key); d.splice(idx,1); DB.set(key,d);
            if(key.includes('clientes')) renderClientes(); else renderProductos();
        }
    }

    // --- 6. GUARDAR DATOS MODALES ---
    function prepararFormulario(tipo, idx = null) {
        const modal = document.getElementById('modalFormulario');
        const cont = document.getElementById('contenidoFormulario');
        const tit = document.getElementById('tituloFormulario');
        document.getElementById('editIndex').value = idx !== null ? idx : -1;
        document.getElementById('editType').value = tipo;
        
        modal.style.display = 'flex';
        cont.innerHTML = '';
        
        if(tipo === 'cliente') {
            tit.innerText = idx !== null ? "EDITAR CLIENTE" : "NUEVO CLIENTE";
            const d = idx !== null ? DB.get('cundo_clientes')[idx] : {};
            const fields = [
                { id:'rut', lbl:'RUT', val:d.rut||'', fn:'formatearRut(this)' },
                { id:'razon', lbl:'RAZÓN SOCIAL', val:d.razon||'' },
                { id:'giro', lbl:'GIRO', val:d.giro||'' },
                { id:'dir', lbl:'DIRECCIÓN', val:d.dir||'' },
                { id:'comuna', lbl:'COMUNA', val:d.comuna||'' },
                { id:'region', lbl:'REGIÓN', val:d.region||'' },
                { id:'contacto', lbl:'CONTACTO', val:d.contacto||'' },
                { id:'email', lbl:'EMAIL', val:d.email||'' }
            ];
            fields.forEach(f => cont.innerHTML += `<div class="input-group"><label>${f.lbl}</label><input type="text" id="f_${f.id}" value="${f.val}" oninput="upper(this);${f.fn||''}"></div>`);
        } else {
            tit.innerText = idx !== null ? "EDITAR PRODUCTO" : "NUEVO PRODUCTO";
            const d = idx !== null ? DB.get('cundo_productos')[idx] : {};
            cont.innerHTML += `
                <div class="input-group"><label>CÓDIGO</label><input type="text" id="f_cod" value="${d.cod||''}" oninput="upper(this)"></div>
                <div class="input-group"><label>DESCRIPCIÓN</label><input type="text" id="f_desc" value="${d.desc||''}" oninput="upper(this)"></div>
                <div class="input-group"><label>COSTO NETO</label><input type="number" id="f_costo" value="${d.costo||''}"></div>
                <div class="input-group"><label>VENTA NETO</label><input type="number" id="f_precio" value="${d.precio||''}"></div>
            `;
        }
    }

    function guardarDatos() {
        const tipo = document.getElementById('editType').value;
        const idx = parseInt(document.getElementById('editIndex').value);
        const dbName = tipo === 'cliente' ? 'cundo_clientes' : 'cundo_productos';
        const db = DB.get(dbName);
        
        let nuevo = {};
        if(tipo === 'cliente') {
            nuevo = {
                rut: document.getElementById('f_rut').value,
                razon: document.getElementById('f_razon').value,
                giro: document.getElementById('f_giro').value,
                dir: document.getElementById('f_dir').value,
                comuna: document.getElementById('f_comuna').value,
                region: document.getElementById('f_region').value,
                contacto: document.getElementById('f_contacto').value,
                email: document.getElementById('f_email').value
            };
            if(!nuevo.rut) return alert("RUT Obligatorio");
        } else {
            nuevo = {
                cod: document.getElementById('f_cod').value,
                desc: document.getElementById('f_desc').value,
                costo: parseFloat(document.getElementById('f_costo').value)||0,
                precio: parseFloat(document.getElementById('f_precio').value)||0
            };
            if(!nuevo.cod) return alert("Código Obligatorio");
        }

        if(idx === -1) db.push(nuevo); else db[idx] = nuevo;
        DB.set(dbName, db);
        
        if(document.getElementById('modalGestorClientes').style.display === 'flex') renderClientes();
        if(document.getElementById('modalGestorProductos').style.display === 'flex') renderProductos();
        document.getElementById('modalFormulario').style.display = 'none';
    }

    // --- 7. FINALIZAR Y PDF ---
    function finalizarCotizacion() {
        if(!document.getElementById('cliRazon').value) return alert("Falta Cliente");
        
        const nCot = document.getElementById('lblCorrelativo').innerText;
        const tot = document.getElementById('txtTotal').innerText;
        const editIdx = parseInt(document.getElementById('indiceEdicionHistorial').value);
        
        // Recopilar datos completos para guardar en historial
        const clienteData = {
            razon: document.getElementById('cliRazon').value,
            rut: document.getElementById('cliRut').value,
            giro: document.getElementById('cliGiro').value,
            dir: document.getElementById('cliDir').value,
            comuna: document.getElementById('cliComuna').value,
            region: document.getElementById('cliRegion').value,
            contacto: document.getElementById('cliContacto').value,
            email: document.getElementById('cliEmail').value
        };

        const itemsData = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.cell-edit').value;
            if(desc) {
                itemsData.push({
                    desc: desc,
                    cant: parseFloat(tr.querySelector('.cell-qty').value) || 0,
                    costo: parseFloat(tr.querySelector('.val-costo').value) || 0,
                    precio: parseFloat(tr.querySelector('.val-venta').value) || 0
                });
            }
        });

        const registro = { 
            n: nCot, 
            fecha: document.getElementById('lblFecha').innerText, 
            cliente: clienteData.razon, 
            total: tot,
            datosCliente: clienteData,
            items: itemsData
        };
        
        const h = DB.get('cundo_historial');

        if(editIdx > -1) {
            // ACTUALIZAR EXISTENTE
            h[editIdx] = registro;
            alert("Cotización actualizada correctamente.");
        } else {
            // CREAR NUEVA
            h.push(registro);
            // Solo incrementar secuencia si es nueva
            let seq = parseInt(nCot.replace('CN',''));
            localStorage.setItem('cundo_seq', seq + 1);
        }
        
        DB.set('cundo_historial', h);
        generarPDF(nCot);
    }

    function generarPDF(numero) {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();
        const azul = [31, 111, 139];
        const naranja = [242, 92, 5];
        const grisClaro = [245, 245, 245];

        doc.setFillColor(...azul); doc.rect(0,0,10,297,'F'); 
        doc.setFontSize(22); doc.setTextColor(...azul); doc.setFont("helvetica","bold");
        doc.text("CUNDO SPA", 18, 20);
        doc.setFontSize(8); doc.setTextColor(100); doc.setFont("helvetica","normal");
        doc.text("INGENIERÍA, SERVICIOS Y SOLUCIONES INTEGRALES", 18, 25);
        doc.text("WEB: WWW.CUNDOSPA.CL | CONTACTO@CUNDOSPA.CL", 18, 29);

        doc.setFillColor(...grisClaro); doc.rect(145, 10, 55, 18, 'F');
        doc.setFontSize(10); doc.setTextColor(...naranja); doc.setFont("helvetica","bold");
        doc.text("COTIZACIÓN", 172, 16, {align:"center"});
        doc.setTextColor(0); doc.text(numero, 172, 23, {align:"center"});

        const startY = 40;
        doc.setDrawColor(200); doc.setFillColor(252, 252, 252);
        doc.roundedRect(18, startY, 182, 35, 2, 2, 'FD');
        doc.setFillColor(...azul); doc.rect(18, startY, 182, 6, 'F');
        doc.setTextColor(255); doc.setFontSize(8); doc.setFont("helvetica","bold");
        doc.text("DATOS DEL CLIENTE", 20, startY + 4);

        doc.setTextColor(0); doc.setFontSize(8);
        const r1 = startY + 12; const r2 = startY + 18; const r3 = startY + 24; const r4 = startY + 30;
        
        doc.setFont("helvetica", "bold");
        doc.text("RAZÓN SOCIAL:", 22, r1); doc.text("RUT:", 110, r1);
        doc.text("DIRECCIÓN:", 22, r2); doc.text("COMUNA:", 110, r2);
        doc.text("CONTACTO:", 22, r3); doc.text("EMAIL:", 110, r3);
        doc.text("GIRO:", 22, r4);

        doc.setFont("helvetica", "normal");
        const cR = document.getElementById('cliRazon').value;
        const cRu = document.getElementById('cliRut').value;
        doc.text(cR.substring(0,35), 50, r1); doc.text(cRu, 130, r1);
        doc.text(document.getElementById('cliDir').value.substring(0,35), 50, r2); doc.text(document.getElementById('cliComuna').value, 130, r2);
        doc.text(document.getElementById('cliContacto').value.substring(0,25), 50, r3); doc.text(document.getElementById('cliEmail').value.substring(0,30), 130, r3);
        doc.text(document.getElementById('cliGiro').value.substring(0,35), 50, r4);

        const rows = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.cell-edit').value;
            if(desc) {
                const cant = tr.querySelector('.cell-qty').value;
                const unit = tr.querySelector('.val-venta').value;
                rows.push([cant, desc, formatMoney(unit), formatMoney(cant*unit)]);
            }
        });

        doc.autoTable({
            startY: 85,
            head: [['CANT', 'DESCRIPCIÓN', 'P. UNITARIO', 'TOTAL']],
            body: rows,
            theme: 'plain',
            styles: { fontSize: 8, cellPadding: 3, lineColor: 220, lineWidth: 0.1 },
            headStyles: { fillColor: azul, textColor: 255, fontSize: 8, fontStyle: 'bold', halign:'center' },
            columnStyles: { 0: {halign:'center', cellWidth: 15}, 2: {halign:'right', cellWidth: 35}, 3: {halign:'right', cellWidth: 35, fontStyle:'bold'} },
            margin: { left: 18, right: 10 },
            didParseCell: d => { if(d.section==='body' && d.row.index%2===0) d.cell.styles.fillColor=[250,250,250]; }
        });

        const finalY = doc.lastAutoTable.finalY + 5;
        const boxX = 140;
        doc.setFontSize(8); doc.setTextColor(0);
        doc.text("SUBTOTAL NETO:", boxX, finalY + 5);
        doc.text(document.getElementById('txtNeto').innerText, 195, finalY + 5, {align:'right'});
        doc.text("IVA (19%):", boxX, finalY + 10);
        doc.text(document.getElementById('txtIva').innerText, 195, finalY + 10, {align:'right'});

        doc.setFillColor(...azul); doc.rect(boxX - 5, finalY + 14, 65, 12, 'F');
        doc.setTextColor(255); doc.setFontSize(10); doc.setFont("helvetica","bold");
        doc.text("TOTAL A PAGAR:", boxX, finalY + 21);
        doc.text(document.getElementById('txtTotal').innerText, 195, finalY + 21, {align:'right'});

        const pageHeight = doc.internal.pageSize.height;
        doc.setTextColor(150); doc.setFontSize(7); doc.setFont("helvetica","normal");
        doc.text("DOCUMENTO VÁLIDO POR 15 DÍAS HÁBILES DESDE SU EMISIÓN", 105, pageHeight - 15, {align:"center"});
        doc.text("GENERADO POR SISTEMA INTERNO CUNDO SPA", 105, pageHeight - 10, {align:"center"});

        doc.save(`Cotizacion_${numero}.pdf`);
        // Recargar para limpiar
        setTimeout(() => location.reload(), 2000);
    }
</script>
</body>
</html>
