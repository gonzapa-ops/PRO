<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA PRO V6 - CUNDO SPA</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS COMPACTOS Y GENERALES --- */
        :root {
            --primary: #1F6F8B;
            --secondary: #F25C05;
            --bg: #EAEDED;
            --text: #2C3E50;
            --dark: #17202A;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; text-transform: uppercase; }
        body { background: var(--bg); padding: 15px; color: var(--text); font-size: 11px; } /* Letra base más pequeña */
        
        .main-container { max-width: 1400px; margin: 0 auto; background: white; min-height: 100vh; box-shadow: 0 0 15px rgba(0,0,0,0.1); display: flex; flex-direction: column; border-radius: 6px; }

        /* HEADER */
        .top-bar { background: var(--dark); color: white; padding: 8px 15px; display: flex; justify-content: space-between; align-items: center; }
        .top-menu { display: flex; gap: 8px; }
        .top-menu button { background: #34495E; border: 1px solid #5D6D7E; color: white; padding: 5px 12px; cursor: pointer; border-radius: 3px; font-size: 10px; font-weight: bold; transition: 0.3s; display: flex; align-items: center; gap: 4px; }
        .top-menu button:hover { background: var(--secondary); border-color: var(--secondary); }

        .header { padding: 20px 25px; display: flex; justify-content: space-between; border-bottom: 3px solid var(--secondary); background: white; }
        .brand h1 { color: var(--primary); font-size: 24px; font-weight: 900; letter-spacing: -0.5px; margin: 0; }
        .brand p { color: #7F8C8D; font-size: 10px; font-weight: 600; margin-top: 2px; }

        .quote-data { text-align: right; }
        .quote-number { font-size: 18px; font-weight: bold; color: var(--secondary); display: block; }

        /* CONTENIDO */
        .content { padding: 20px; }
        .section-box { margin-bottom: 20px; border: 1px solid #E5E7E9; border-radius: 5px; padding: 15px; background: #FBFCFC; position: relative; }
        .section-label { position: absolute; top: -10px; left: 15px; background: var(--primary); color: white; padding: 2px 8px; font-size: 9px; font-weight: bold; border-radius: 3px; }

        /* FORMULARIOS COMPACTOS */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px; }
        .input-group label { display: block; font-size: 9px; font-weight: bold; color: #7F8C8D; margin-bottom: 3px; }
        .input-group input { width: 100%; padding: 5px 8px; border: 1px solid #D5DBDB; border-radius: 3px; font-size: 11px; font-weight: 600; color: var(--dark); height: 28px; }
        .input-group input:focus { border-color: var(--secondary); outline: none; background: #fff; }
        .input-group input[readonly] { background: #F2F4F4; color: #777; cursor: default; }

        /* BUSCADOR FLOTANTE */
        .search-container { position: relative; margin-bottom: 15px; }
        .search-results { 
            position: absolute; top: 100%; left: 0; width: 100%; 
            background: white; border: 1px solid #3498DB; box-shadow: 0 5px 15px rgba(0,0,0,0.1); 
            z-index: 500; display: none; max-height: 200px; overflow-y: auto;
        }
        .search-item { padding: 8px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 10px; color: #333; }
        .search-item:hover { background: #EBF5FB; color: var(--primary); }
        .search-item strong { color: var(--secondary); }

        /* TABLA PRINCIPAL COMPACTA */
        table { width: 100%; border-collapse: collapse; font-size: 10px; }
        th { background: var(--primary); color: white; padding: 8px; text-align: left; font-weight: 700; }
        td { border: 1px solid #D5DBDB; padding: 0; vertical-align: middle; height: 30px; }
        
        input.cell-edit { width: 100%; height: 100%; border: none; padding: 0 8px; font-size: 11px; font-family: inherit; color: #333; background: transparent; }
        input.cell-edit:focus { outline: 2px solid var(--secondary); background: white; z-index: 2; position: relative; }
        
        input.cell-locked { background: #F8F9F9; color: #7F8C8D; text-align: right; cursor: not-allowed; }
        input.cell-qty { text-align: center; font-weight: bold; color: var(--primary); }

        /* TOTALES */
        .totals-wrapper { display: flex; justify-content: flex-end; margin-top: 15px; }
        .totals-card { width: 280px; background: white; border: 1px solid #D5DBDB; padding: 15px; border-radius: 5px; }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 5px; font-size: 11px; font-weight: 600; }
        .t-final { border-top: 2px solid var(--secondary); padding-top: 10px; margin-top: 8px; font-size: 14px; font-weight: 900; color: var(--primary); }

        /* BOTONES */
        .action-area { text-align: center; margin-top: 30px; border-top: 1px dashed #ccc; padding-top: 20px; }
        .btn-main { 
            background: var(--secondary); color: white; padding: 10px 40px; 
            font-size: 13px; font-weight: bold; border: none; border-radius: 4px; 
            cursor: pointer; transition: all 0.2s; 
        }
        .btn-main:hover { background: #D35400; transform: translateY(-1px); }
        
        .btn-add { padding: 6px 12px; background: #34495E; color: white; border: none; border-radius: 3px; cursor: pointer; font-size: 10px; font-weight: bold; }

        /* MODALES */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 2000; align-items: center; justify-content: center; }
        .modal-box { background: white; width: 90%; max-width: 1000px; max-height: 90vh; overflow-y: auto; padding: 20px; border-radius: 6px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); }
        .close-btn { float: right; font-size: 20px; cursor: pointer; color: #E74C3C; font-weight: bold; }
        
        /* Tablas Gestión */
        .manage-table th { padding: 6px; font-size: 9px; background: #2C3E50; color: white; }
        .manage-table td { padding: 5px; font-size: 10px; }
        .btn-small { padding: 3px 6px; border: none; color: white; border-radius: 2px; cursor: pointer; font-size: 9px; margin-right: 3px; }
        .btn-edit { background: #F1C40F; color: #333; }
        .btn-del { background: #C0392B; }
        .btn-new { background: var(--primary); padding: 5px 15px; color: white; border: none; border-radius: 3px; cursor: pointer; font-size: 10px; font-weight: bold; float: right; }

    </style>
</head>
<body>

<div class="main-container">
    
    <div class="top-bar">
        <div style="font-weight: 800; font-size: 12px; color: #BDC3C7;">PANEL ADMINISTRATIVO</div>
        <div class="top-menu">
            <button onclick="abrirGestor('clientes')">👥 CLIENTES</button>
            <button onclick="abrirGestor('productos')">📦 PRODUCTOS</button>
            <button onclick="abrirGestor('historial')">📜 HISTORIAL</button>
        </div>
    </div>

    <div class="header">
        <div class="brand">
            <h1>CUNDO SPA</h1>
            <p>INGENIERÍA Y SOLUCIONES</p>
        </div>
        <div class="quote-data">
            <span style="font-size:9px; color:#999;">COTIZACIÓN N°</span>
            <span class="quote-number" id="lblCorrelativo">...</span>
            <span style="font-size:10px; font-weight:bold; color:var(--primary);" id="lblFecha"></span>
        </div>
    </div>

    <div class="content">

        <div class="section-box">
            <span class="section-label">1. CLIENTE</span>
            
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
                        <th style="width: 30px; text-align:center;">X</th>
                        <th style="width: 40%;">CÓDIGO / DESCRIPCIÓN (BUSCADOR)</th>
                        <th style="width: 60px; text-align:center;">CANT.</th>
                        <th style="width: 100px; background:#EBEDEF; color:#555;">COSTO UNIT.</th>
                        <th style="width: 100px; background:#EBEDEF; color:#555;">TOTAL COSTO</th>
                        <th style="width: 100px;">VENTA UNIT.</th>
                        <th style="width: 100px;">TOTAL VENTA</th>
                        <th style="width: 80px;">UTILIDAD</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
            
            <div style="margin-top: 10px; display:flex; justify-content:space-between; align-items:center;">
                <button onclick="agregarFila()" class="btn-add">+ AGREGAR LÍNEA</button>
                <span style="font-size:9px; color:red;">* PRECIOS SE EDITAN EN BASE DE DATOS DE PRODUCTOS</span>
            </div>
        </div>

        <div class="totals-wrapper">
            <div class="totals-card">
                <div class="t-row"><span>NETO:</span><span id="txtNeto">$0</span></div>
                <div class="t-row"><span>IVA (19%):</span><span id="txtIva">$0</span></div>
                <div class="t-row t-final"><span>TOTAL:</span><span id="txtTotal">$0</span></div>
                <div style="font-size:9px; color:green; text-align:right; margin-top:5px;">UTILIDAD: <span id="txtUtilidad">$0</span></div>
            </div>
        </div>

        <div class="action-area">
            <button class="btn-main" onclick="finalizarCotizacion()">📄 GENERAR PDF (COMPACTO)</button>
        </div>
    </div>
</div>

<div id="modalGestorClientes" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="cerrarGestor('clientes')">&times;</span>
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
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
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
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
        <h3>HISTORIAL</h3>
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

<div id="modalFormulario" class="modal" style="z-index: 2100;">
    <div class="modal-box" style="max-width: 500px;">
        <span class="close-btn" onclick="document.getElementById('modalFormulario').style.display='none'">&times;</span>
        <h3 id="tituloFormulario" style="margin-bottom:15px;">...</h3>
        <input type="hidden" id="editIndex">
        <input type="hidden" id="editType">
        <div id="contenidoFormulario" class="grid-form" style="grid-template-columns: 1fr;"></div>
        <button onclick="guardarDatos()" class="btn-main" style="width:100%; margin-top:15px;">GUARDAR</button>
    </div>
</div>

<script>
    // --- UTILIDADES ---
    const formatMoney = num => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(num);
    const upper = e => e.value = e.value.toUpperCase();
    
    // FORMATO RUT: 12345678-9 (Sin puntos, con guion)
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
        agregarFila(); 
    };

    // --- BUSCADOR CLIENTES (COD + DESC) ---
    function buscarCliente(input) {
        upper(input);
        const term = input.value;
        const list = document.getElementById('listaClientes');
        list.innerHTML = '';
        
        if (term.length < 2) { list.style.display = 'none'; return; } // Buscar desde 2 letras

        const clientes = DB.get('cundo_clientes');
        // Busca en RUT o RAZÓN SOCIAL
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

    // --- TABLA PRODUCTOS (BUSCADOR COD + DESC) ---
    function agregarFila() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center;"><button onclick="this.closest('tr').remove(); calcular();" style="color:red; background:none; border:none; font-weight:bold; cursor:pointer;">X</button></td>
            <td style="position:relative;">
                <input type="text" class="cell-edit" placeholder="BUSCAR COD O NOMBRE..." onkeyup="buscarProductoFila(this)" oninput="upper(this)">
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
        const term = input.value;
        const list = input.nextElementSibling;
        list.innerHTML = '';
        
        if(term.length === 0) { limpiarFila(input.closest('tr')); return; }

        const prods = DB.get('cundo_productos');
        // Busca en CÓDIGO o DESCRIPCIÓN
        const results = prods.filter(p => p.cod.includes(term) || p.desc.includes(term));

        if(results.length > 0) {
            list.style.display = 'block';
            results.forEach(p => {
                const div = document.createElement('div');
                div.className = 'search-item';
                div.innerHTML = `<strong>${p.cod}</strong> - ${p.desc}`;
                div.onclick = () => {
                    input.value = `${p.cod} - ${p.desc}`; // Muestra ambos
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
        tr.querySelector('.val-costo').value = prod.costo;
        tr.querySelector('.val-venta').value = prod.precio;
        tr.querySelector('.costo-unit').value = formatMoney(prod.costo);
        tr.querySelector('.venta-unit').value = formatMoney(prod.precio);
        calcular();
    }

    function limpiarFila(tr) {
        tr.querySelector('.val-costo').value = 0;
        tr.querySelector('.val-venta').value = 0;
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

    // --- GESTIÓN CRUD ---
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

    function renderHistorial() {
        const tbody = document.getElementById('bodyHistorial');
        tbody.innerHTML = '';
        DB.get('cundo_historial').reverse().forEach(h => {
            tbody.innerHTML += `<tr><td>${h.n}</td><td>${h.fecha}</td><td>${h.cliente}</td><td>${h.total}</td></tr>`;
        });
    }

    function borrar(key, idx) {
        if(confirm("¿Eliminar?")) {
            const d = DB.get(key); d.splice(idx,1); DB.set(key,d);
            if(key.includes('clientes')) renderClientes(); else renderProductos();
        }
    }

    // --- FORMULARIO ---
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
        
        if(tipo === 'cliente') renderClientes(); else renderProductos();
        document.getElementById('modalFormulario').style.display = 'none';
    }

    // --- PDF COMPACTO (LETRAS PEQUEÑAS) ---
    function finalizarCotizacion() {
        if(!document.getElementById('cliRazon').value) return alert("Seleccione Cliente");
        const nCot = document.getElementById('lblCorrelativo').innerText;
        const tot = document.getElementById('txtTotal').innerText;
        
        const h = DB.get('cundo_historial');
        h.push({ n:nCot, fecha:new Date().toLocaleDateString('es-CL'), cliente:document.getElementById('cliRazon').value, total:tot });
        DB.set('cundo_historial', h);
        
        let seq = parseInt(nCot.replace('CN',''));
        localStorage.setItem('cundo_seq', seq + 1);
        
        generarPDF(nCot);
    }

    function generarPDF(numero) {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();
        const azul = [31, 111, 139];
        const naranja = [242, 92, 5];

        doc.setFillColor(...azul); doc.rect(0,0,10,297,'F');

        // HEADER COMPACTO
        doc.setFontSize(20); doc.setTextColor(...azul); doc.setFont("helvetica","bold");
        doc.text("CUNDO SPA", 18, 18);
        doc.setFontSize(7); doc.setTextColor(100); doc.setFont("helvetica","normal");
        doc.text("INGENIERÍA Y SOLUCIONES", 18, 22);
        doc.text("RUT: 77.XXX.XXX-X | CONTACTO@CUNDOSPA.CL", 18, 26);

        // CAJA INFO PEQUEÑA
        doc.setFillColor(245,245,245); doc.rect(150, 10, 50, 15, 'F');
        doc.setFontSize(10); doc.setTextColor(...naranja); doc.setFont("helvetica","bold");
        doc.text("COTIZACIÓN", 175, 15, {align:"center"});
        doc.setTextColor(0); doc.text(numero, 175, 21, {align:"center"});

        // INFO CLIENTE (LETRA 8)
        doc.setFontSize(7); doc.setTextColor(...azul); doc.text("CLIENTE:", 18, 38);
        doc.setTextColor(0); doc.setFont("helvetica","normal");
        const rz = document.getElementById('cliRazon').value;
        const rut = document.getElementById('cliRut').value;
        doc.text(`${rz} | RUT: ${rut}`, 18, 43);
        doc.text(`DIRECCIÓN: ${document.getElementById('cliDir').value}`, 18, 47);
        doc.text(`ATENCIÓN: ${document.getElementById('cliContacto').value}`, 18, 51);

        // TABLA (LETRA 7/8)
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
            startY: 58,
            head: [['CANT', 'DESCRIPCIÓN', 'UNITARIO', 'TOTAL']],
            body: rows,
            theme: 'plain',
            styles: { fontSize: 7, cellPadding: 2 }, // LETRA MUY PEQUEÑA
            headStyles: { fillColor: azul, textColor: 255, fontSize: 8, fontStyle: 'bold' },
            columnStyles: { 0: {halign:'center'}, 2:{halign:'right'}, 3:{halign:'right', fontStyle:'bold'} },
            didParseCell: d => { if(d.section==='body' && d.row.index%2===0) d.cell.styles.fillColor=[250,250,250]; }
        });

        const y = doc.lastAutoTable.finalY + 8;
        const tot = document.getElementById('txtTotal').innerText;
        doc.setFontSize(8); doc.text(`NETO: ${document.getElementById('txtNeto').innerText}`, 195, y, {align:'right'});
        doc.text(`IVA: ${document.getElementById('txtIva').innerText}`, 195, y+4, {align:'right'});
        doc.setFontSize(10); doc.setTextColor(...azul); doc.setFont("helvetica","bold");
        doc.text(`TOTAL: ${tot}`, 195, y+10, {align:'right'});

        doc.save(`Cotizacion_${numero}.pdf`);
        setTimeout(() => location.reload(), 1500);
    }
</script>
</body>
</html>
