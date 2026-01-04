<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA PRO V10 - INGENIERIA CUNTEL SPA</title>
    
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
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; text-transform: uppercase; }
        body { background: var(--bg); padding: 15px; color: var(--text); font-size: 11px; padding-bottom: 80px; }
        
        .main-container { max-width: 1400px; margin: 0 auto; background: white; min-height: 100vh; box-shadow: 0 5px 30px rgba(0,0,0,0.15); border-radius: 8px; overflow: hidden; display: flex; flex-direction: column; }

        /* TOP BAR */
        .top-bar { background: var(--dark); color: white; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; }
        .top-menu { display: flex; gap: 8px; }
        .btn-nav { background: #34495E; border: 1px solid #566573; color: white; padding: 6px 12px; cursor: pointer; border-radius: 3px; font-size: 10px; font-weight: bold; transition: 0.2s; display: flex; align-items: center; gap: 5px; }
        .btn-nav:hover { background: var(--secondary); border-color: var(--secondary); }
        .btn-backup { background: #27AE60; border-color: #1E8449; }

        /* HEADER */
        .header { padding: 25px 30px; display: flex; justify-content: space-between; border-bottom: 3px solid var(--secondary); background: white; align-items: center; }
        
        .brand-area { display: flex; align-items: center; gap: 15px; }
        .logo-placeholder { width: 60px; height: 60px; background: #eee; border: 2px dashed #ccc; display: flex; align-items: center; justify-content: center; cursor: pointer; overflow: hidden; border-radius: 4px; }
        .logo-placeholder img { width: 100%; height: 100%; object-fit: contain; }
        
        .brand h1 { color: var(--primary); font-size: 24px; font-weight: 900; margin: 0; }
        .brand p { color: #7F8C8D; font-size: 10px; font-weight: 600; }

        .quote-data { text-align: right; }
        .quote-number { font-size: 22px; font-weight: bold; color: var(--secondary); }
        
        /* AVISO EDICIÓN */
        #bar-edicion { display: none; background: #FFF3CD; color: #856404; text-align: center; padding: 8px; font-weight: bold; border-bottom: 1px solid #FFEEBA; }

        /* CONTENIDO */
        .content { padding: 25px; }
        .section-box { margin-bottom: 25px; border: 1px solid #D5DBDB; border-radius: 5px; padding: 20px; background: #FBFCFC; position: relative; }
        .section-label { position: absolute; top: -10px; left: 15px; background: var(--primary); color: white; padding: 3px 10px; font-size: 10px; font-weight: bold; border-radius: 3px; }

        /* INPUTS */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 12px; }
        .input-group label { display: block; font-size: 9px; font-weight: bold; color: #7F8C8D; margin-bottom: 4px; }
        .input-group input, .input-group select { width: 100%; padding: 6px 10px; border: 1px solid #D5DBDB; border-radius: 3px; font-size: 11px; font-weight: 600; color: var(--dark); height: 30px; }
        .input-group input:focus, .input-group select:focus { border-color: var(--secondary); outline: none; background: #fff; }
        .input-group input[readonly] { background: #F2F4F4; color: #777; cursor: default; }

        /* BUSCADOR */
        .search-container { position: relative; margin-bottom: 15px; }
        .search-results { position: absolute; top: 100%; left: 0; width: 100%; background: white; border: 1px solid #3498DB; box-shadow: 0 8px 20px rgba(0,0,0,0.15); z-index: 500; display: none; max-height: 250px; overflow-y: auto; }
        .search-item { padding: 10px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 11px; }
        .search-item:hover { background: #EBF5FB; color: var(--primary); }
        .create-new { background: #FDEDEC; color: #C0392B; font-weight: bold; }

        /* TABLA */
        table { width: 100%; border-collapse: collapse; font-size: 10px; }
        th { background: var(--primary); color: white; padding: 10px; text-align: left; }
        td { border: 1px solid #D5DBDB; padding: 0; height: 32px; }
        input.cell-edit { width: 100%; height: 100%; border: none; padding: 0 8px; font-size: 11px; background: transparent; }
        input.cell-edit:focus { outline: 2px solid var(--secondary); background: white; z-index: 10; position: relative; }
        input.cell-locked { background: #F8F9F9; color: #777; text-align: right; cursor: not-allowed; }
        input.cell-qty { text-align: center; font-weight: bold; color: var(--primary); }

        /* TOTALES Y PAGO */
        .bottom-area { display: grid; grid-template-columns: 1fr 350px; gap: 20px; margin-top: 20px; }
        
        .totals-card { background: white; border: 1px solid #D5DBDB; padding: 15px; border-radius: 5px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 6px; font-size: 11px; font-weight: 600; color: #555; align-items: center; }
        
        .final-row { border-top: 2px solid var(--secondary); padding-top: 10px; margin-top: 10px; display: flex; justify-content: space-between; align-items: center; }
        .total-price { font-size: 20px; font-weight: 900; color: var(--primary); }
        .util-label { font-size: 10px; color: var(--green); font-weight: bold; display: block; text-align: right; }

        /* BOTONES */
        .action-area { text-align: center; margin-top: 30px; border-top: 1px dashed #BDC3C7; padding-top: 20px; }
        .btn-main { background: var(--secondary); color: white; padding: 14px 60px; font-size: 15px; font-weight: 800; border: none; border-radius: 4px; cursor: pointer; box-shadow: 0 4px 15px rgba(242, 92, 5, 0.3); }
        .btn-main:hover { background: #D35400; transform: translateY(-1px); }

        /* MODALES */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 2000; align-items: center; justify-content: center; backdrop-filter: blur(2px); }
        .modal-box { background: white; width: 95%; max-width: 1000px; max-height: 90vh; overflow-y: auto; padding: 25px; border-radius: 6px; }
        .manage-table { width: 100%; margin-top: 15px; border: 1px solid #ddd; }
        .manage-table th { padding: 8px; background: #2C3E50; color: white; font-size: 10px; }
        .manage-table td { padding: 6px; font-size: 10px; border-bottom: 1px solid #eee; }
        .btn-small { padding: 3px 6px; border: none; color: white; border-radius: 2px; cursor: pointer; font-size: 9px; margin-right: 3px; font-weight: bold; }

    </style>
</head>
<body>

<div class="main-container">

    <div id="bar-edicion">⚠️ MODO EDICIÓN ACTIVO</div>

    <div class="top-bar">
        <div style="font-weight: 800; font-size: 12px; opacity: 0.8;">SISTEMA DE GESTIÓN PRO</div>
        <div class="top-menu">
            <button class="btn-nav" onclick="abrirGestor('clientes')">👥 CLIENTES</button>
            <button class="btn-nav" onclick="abrirGestor('productos')">📦 PRODUCTOS</button>
            <button class="btn-nav" onclick="abrirGestor('historial')">📜 HISTORIAL</button>
            <button class="btn-nav btn-backup" onclick="descargarRespaldo()">⬇️ RESPALDAR</button>
            <label for="inputImport" class="btn-nav btn-backup" style="cursor:pointer;">⬆️ RESTAURAR</label>
            <input type="file" id="inputImport" style="display:none" onchange="importarRespaldo(this)">
            <button class="btn-nav" onclick="nuevaCotizacion()" style="background:#C0392B;">🔄 LIMPIAR</button>
        </div>
    </div>

    <div class="header">
        <div class="brand-area">
            <div class="logo-placeholder" onclick="document.getElementById('logoInput').click()">
                <img id="logoPreview" src="" alt="LOGO" style="display:none">
                <span id="logoText" style="font-size:9px; text-align:center; color:#999;">+ LOGO</span>
            </div>
            <input type="file" id="logoInput" style="display:none" accept="image/*" onchange="cargarLogo(this)">
            
            <div class="brand">
                <h1>CUNDO SPA</h1>
                <p>SOLUCIONES INTEGRALES</p>
            </div>
        </div>
        <div class="quote-data">
            <span style="font-size:9px; color:#999;">FOLIO COTIZACIÓN</span>
            <span class="quote-number" id="lblCorrelativo">...</span>
            <span style="font-size:11px; font-weight:bold; color:var(--primary); display:block;" id="lblFecha"></span>
        </div>
    </div>

    <div class="content">

        <div class="section-box">
            <span class="section-label">1. INFORMACIÓN DEL CLIENTE</span>
            
            <div class="input-group search-container">
                <input type="text" id="buscadorCliente" placeholder="🔍 BUSCAR (Escribe 2 letras, RUT o Nombre)..." onkeyup="buscarCliente(this)" autocomplete="off" style="border: 2px solid var(--primary);">
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
            <span class="section-label">2. ÍTEMS Y COSTOS</span>
            
            <table id="tablaItems">
                <thead>
                    <tr>
                        <th style="width: 30px; text-align:center;">X</th>
                        <th>CÓDIGO / DESCRIPCIÓN</th>
                        <th style="width: 60px; text-align:center;">CANT</th>
                        
                        <th style="width: 90px; background:#EBEDEF; color:#555;">COSTO U.</th>
                        <th style="width: 90px; background:#EBEDEF; color:#555;">T. COSTO</th>
                        
                        <th style="width: 90px;">PRECIO U.</th>
                        <th style="width: 90px;">TOTAL</th>
                        <th style="width: 70px;">UTIL.</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
            
            <div style="margin-top: 15px; display:flex; justify-content:space-between;">
                <button onclick="agregarFila()" class="btn-nav" style="background:#34495E;">+ AGREGAR LÍNEA</button>
            </div>
        </div>

        <div class="bottom-area">
            <div class="section-box" style="margin-bottom:0;">
                <span class="section-label">MODALIDAD DE PAGO</span>
                <select id="cmbPago">
                    <option value="TRANSFERENCIA">TRANSFERENCIA</option>
                    <option value="WEBPAY">WEBPAY</option>
                    <option value="EFECTIVO">EFECTIVO</option>
                    <option value="CHEQUE 30 DÍAS">CHEQUE 30 DÍAS</option>
                    <option value="CHEQUE AL DÍA">CHEQUE AL DÍA</option>
                    <option value="OC 30 DÍAS">OC 30 DÍAS</option>
                </select>
            </div>

            <div class="totals-card">
                <div class="t-row">
                    <span>SUBTOTAL NETO:</span>
                    <span id="txtNeto">$0</span>
                </div>
                <div class="t-row">
                    <span>DESCUENTO (%):</span>
                    <input type="number" id="txtDescPorc" value="0" min="0" max="100" style="width:50px; text-align:right;" onchange="calcular()">
                </div>
                <div class="t-row">
                    <span>MONTO DESC.:</span>
                    <span id="txtDescMonto">$0</span>
                </div>
                <div class="t-row" style="border-top:1px dashed #ddd; padding-top:5px;">
                    <span>NETO FINAL:</span>
                    <span id="txtNetoFinal">$0</span>
                </div>
                <div class="t-row">
                    <span>IVA (19%):</span>
                    <span id="txtIva">$0</span>
                </div>
                
                <div class="final-row">
                    <div style="text-align:right; width:100%;">
                        <span style="font-size:9px; color:#777;">TOTAL A PAGAR</span>
                        <div class="total-price" id="txtTotal">$0</div>
                        <span class="util-label">UTILIDAD: <span id="txtUtilidad">$0</span></span>
                    </div>
                </div>
            </div>
        </div>

        <div class="action-area">
            <input type="hidden" id="editIndex" value="-1">
            <button class="btn-main" onclick="guardarYGenerar()">💾 GUARDAR Y GENERAR PDF</button>
        </div>

    </div>
</div>

<div id="modalGestor" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="document.getElementById('modalGestor').style.display='none'" style="float:right; cursor:pointer; font-size:20px;">&times;</span>
        <h3 id="modalTitle">GESTIÓN</h3>
        <div style="margin-bottom:10px; text-align:right;">
             <button id="btnModalNew" class="btn-nav" style="background:var(--primary); float:right;">+ NUEVO</button>
             <div style="clear:both;"></div>
        </div>
        <table class="manage-table">
            <thead id="modalHead"></thead>
            <tbody id="modalBody"></tbody>
        </table>
    </div>
</div>

<div id="modalForm" class="modal" style="z-index:2100;">
    <div class="modal-box" style="max-width:500px;">
        <h3 id="formTitle">...</h3>
        <input type="hidden" id="formType">
        <input type="hidden" id="formIdx">
        <div id="formContent" class="grid-form" style="grid-template-columns:1fr; margin-top:15px;"></div>
        <button onclick="guardarCRUD()" class="btn-main" style="width:100%; margin-top:15px;">GUARDAR</button>
        <button onclick="document.getElementById('modalForm').style.display='none'" style="width:100%; margin-top:5px; background:#999; border:none; padding:10px; color:white; border-radius:4px; cursor:pointer;">CANCELAR</button>
    </div>
</div>

<script>
    // --- 1. CORE & CONFIG ---
    const DB_KEYS = { CLI: 'db_clients', PROD: 'db_products', HIST: 'db_history', SEQ: 'db_seq', LOGO: 'db_logo' };
    const formatMoney = n => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(n);
    const upper = e => e.value = e.value.toUpperCase();
    
    // Rut Formatter
    const formatRut = i => {
        let v = i.value.replace(/[^0-9kK]/g, '').toUpperCase();
        if(v.length > 1) i.value = v.slice(0,-1) + '-' + v.slice(-1);
    };

    const getDB = k => JSON.parse(localStorage.getItem(k) || "[]");
    const setDB = (k,v) => localStorage.setItem(k, JSON.stringify(v));

    window.onload = () => {
        document.getElementById('lblFecha').innerText = new Date().toLocaleDateString('es-CL');
        const savedLogo = localStorage.getItem(DB_KEYS.LOGO);
        if(savedLogo) {
            document.getElementById('logoPreview').src = savedLogo;
            document.getElementById('logoPreview').style.display = 'block';
        }
        if(document.getElementById('editIndex').value === "-1") {
             let s = localStorage.getItem(DB_KEYS.SEQ) || 50100;
             document.getElementById('lblCorrelativo').innerText = "CN" + s;
        }
        agregarFila();
    };

    // --- 2. LOGIC: SEARCH & TABLE ---
    function buscarCliente(input) {
        upper(input);
        const term = input.value.trim();
        const list = document.getElementById('listaClientes');
        list.innerHTML = '';
        if(term.length < 2) { list.style.display='none'; return; }

        const hits = getDB(DB_KEYS.CLI).filter(c => c.razon.includes(term) || c.rut.includes(term));
        
        if(hits.length > 0) {
            hits.forEach(c => {
                const d = document.createElement('div');
                d.className = 'search-item';
                d.innerHTML = `<strong>${c.rut}</strong> ${c.razon}`;
                d.onclick = () => { 
                    ['Razon','Rut','Giro','Dir','Comuna','Region','Email','Contacto'].forEach(f => {
                         if(document.getElementById('cli'+f)) document.getElementById('cli'+f).value = c[f.toLowerCase()]||'';
                    });
                    list.style.display='none'; 
                };
                list.appendChild(d);
            });
        }
        const create = document.createElement('div');
        create.className = 'search-item create-new';
        create.innerHTML = `+ CREAR NUEVO: "${term}"`;
        create.onclick = () => { list.style.display='none'; openForm('CLI', -1); };
        list.appendChild(create);
        list.style.display='block';
    }

    function agregarFila() {
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:red;border:none;background:none;font-weight:bold;cursor:pointer">X</button></td>
            <td style="position:relative">
                <input type="text" class="cell-edit" placeholder="BUSCAR..." onkeyup="buscarProd(this)" oninput="upper(this)">
                <div class="search-results"></div>
            </td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            <td><input type="text" class="cell-edit cell-locked u-cost" readonly></td>
            <td><input type="text" class="cell-edit cell-locked t-cost" readonly></td>
            <td><input type="text" class="cell-edit cell-locked u-price" readonly></td>
            <td><input type="text" class="cell-edit cell-locked t-price" readonly></td>
            <td><input type="text" class="cell-edit cell-locked util" readonly style="font-size:9px"></td>
            <input type="hidden" class="raw-cost" value="0">
            <input type="hidden" class="raw-price" value="0">
            <input type="hidden" class="raw-cod" value="">
        `;
        document.querySelector('#tablaItems tbody').appendChild(tr);
    }

    function buscarProd(input) {
        const term = input.value.trim().toUpperCase();
        const list = input.nextElementSibling;
        list.innerHTML='';
        if(term.length === 0) { list.style.display='none'; return; }

        const hits = getDB(DB_KEYS.PROD).filter(p => p.cod.includes(term) || p.desc.includes(term));
        
        if(hits.length>0) {
            hits.forEach(p => {
                const d = document.createElement('div');
                d.className='search-item';
                d.innerHTML=`<strong>${p.cod}</strong> ${p.desc}`;
                d.onclick=()=>{
                    input.value=`${p.cod} - ${p.desc}`;
                    const row = input.closest('tr');
                    row.querySelector('.raw-cod').value=p.cod;
                    row.querySelector('.raw-cost').value=p.costo;
                    row.querySelector('.raw-price').value=p.precio;
                    row.querySelector('.u-cost').value=formatMoney(p.costo);
                    row.querySelector('.u-price').value=formatMoney(p.precio);
                    list.style.display='none';
                    calcular();
                };
                list.appendChild(d);
            });
        }
        const create = document.createElement('div');
        create.className='search-item create-new';
        create.innerHTML=`+ CREAR PROD: "${term}"`;
        create.onclick=()=>{ list.style.display='none'; openForm('PROD', -1); };
        list.appendChild(create);
        list.style.display='block';
    }

    function calcular() {
        let neto = 0, util = 0;
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const q = parseFloat(tr.querySelector('.cell-qty').value)||0;
            const c = parseFloat(tr.querySelector('.raw-cost').value)||0;
            const p = parseFloat(tr.querySelector('.raw-price').value)||0;
            
            const tc = q*c; const tp = q*p;
            tr.querySelector('.t-cost').value=formatMoney(tc);
            tr.querySelector('.t-price').value=formatMoney(tp);
            tr.querySelector('.util').value=formatMoney(tp-tc);
            
            neto += tp; util += (tp-tc);
        });

        const descPorc = parseFloat(document.getElementById('txtDescPorc').value)||0;
        const descMonto = neto * (descPorc/100);
        const netoFinal = neto - descMonto;
        const iva = netoFinal * 0.19;
        
        document.getElementById('txtNeto').innerText = formatMoney(neto);
        document.getElementById('txtDescMonto').innerText = formatMoney(descMonto);
        document.getElementById('txtNetoFinal').innerText = formatMoney(netoFinal);
        document.getElementById('txtIva').innerText = formatMoney(iva);
        document.getElementById('txtTotal').innerText = formatMoney(netoFinal + iva);
        document.getElementById('txtUtilidad').innerText = formatMoney(util - descMonto);
    }

    // --- 3. MANAGERS (CRUD) ---
    function abrirGestor(type) {
        const modal = document.getElementById('modalGestor');
        const thead = document.getElementById('modalHead');
        const tbody = document.getElementById('modalBody');
        const btn = document.getElementById('btnModalNew');
        
        modal.style.display='flex';
        tbody.innerHTML='';
        
        if(type==='clientes') {
            document.getElementById('modalTitle').innerText="BASE DE CLIENTES";
            thead.innerHTML=`<tr><th>RUT</th><th>RAZÓN</th><th>CONTACTO</th><th>ACCIÓN</th></tr>`;
            getDB(DB_KEYS.CLI).forEach((c,i)=> {
                tbody.innerHTML+=`<tr><td>${c.rut}</td><td>${c.razon}</td><td>${c.contacto}</td><td>
                <button class="btn-small" style="background:#F39C12" onclick="openForm('CLI',${i})">EDIT</button>
                <button class="btn-small" style="background:#C0392B" onclick="delItem('${DB_KEYS.CLI}',${i},'clientes')">DEL</button></td></tr>`;
            });
            btn.onclick = () => openForm('CLI', -1);
        } else if(type==='productos') {
            document.getElementById('modalTitle').innerText="BASE DE PRODUCTOS (COMPLETO)";
            // CORRECCIÓN SOLICITADA: MOSTRAR TODOS LOS DATOS
            thead.innerHTML=`<tr><th>COD</th><th>DESC</th><th>COSTO</th><th>VENTA</th><th>ACCIÓN</th></tr>`;
            getDB(DB_KEYS.PROD).forEach((p,i)=> {
                tbody.innerHTML+=`<tr><td>${p.cod}</td><td>${p.desc}</td><td>${formatMoney(p.costo)}</td><td>${formatMoney(p.precio)}</td><td>
                <button class="btn-small" style="background:#F39C12" onclick="openForm('PROD',${i})">EDIT</button>
                <button class="btn-small" style="background:#C0392B" onclick="delItem('${DB_KEYS.PROD}',${i},'productos')">DEL</button></td></tr>`;
            });
            btn.onclick = () => openForm('PROD', -1);
        } else {
            document.getElementById('modalTitle').innerText="HISTORIAL";
            btn.style.display='none';
            thead.innerHTML=`<tr><th>FOLIO</th><th>FECHA</th><th>CLIENTE</th><th>TOTAL</th><th>ACCIÓN</th></tr>`;
            getDB(DB_KEYS.HIST).forEach((h,i)=> {
                tbody.innerHTML+=`<tr><td><b>${h.n}</b></td><td>${h.fecha}</td><td>${h.cli.razon}</td><td>${h.total}</td><td>
                <button class="btn-small" style="background:#2980B9" onclick="loadHistory(${i})">EDITAR</button>
                <button class="btn-small" style="background:#C0392B" onclick="delItem('${DB_KEYS.HIST}',${i},'historial')">DEL</button></td></tr>`;
            });
        }
    }

    function delItem(k,i,refresh) {
        if(confirm("¿Eliminar?")) {
            const d=getDB(k); d.splice(i,1); setDB(k,d);
            abrirGestor(refresh);
        }
    }

    function openForm(type, idx) {
        const modal = document.getElementById('modalForm');
        const content = document.getElementById('formContent');
        modal.style.display='flex';
        document.getElementById('formType').value=type;
        document.getElementById('formIdx').value=idx;
        content.innerHTML='';
        
        let data = {};
        if(idx!==-1) data = getDB(type==='CLI'?DB_KEYS.CLI:DB_KEYS.PROD)[idx];

        if(type==='CLI') {
            const fields = [
                {id:'rut',l:'RUT'}, {id:'razon',l:'RAZÓN SOCIAL'},
                {id:'giro',l:'GIRO'}, {id:'dir',l:'DIRECCIÓN'}, 
                {id:'comuna',l:'COMUNA'}, {id:'region',l:'REGIÓN'},
                {id:'email',l:'EMAIL'}, {id:'contacto',l:'CONTACTO'}
            ];
            fields.forEach(f => content.innerHTML+=`<div class="input-group"><label>${f.l}</label><input id="f_${f.id}" value="${data[f.id]||''}" oninput="upper(this)"></div>`);
        } else {
            content.innerHTML+=`
            <div class="input-group"><label>CÓDIGO</label><input id="f_cod" value="${data.cod||''}" oninput="upper(this)"></div>
            <div class="input-group"><label>DESCRIPCIÓN</label><input id="f_desc" value="${data.desc||''}" oninput="upper(this)"></div>
            <div class="input-group"><label>COSTO NETO</label><input type="number" id="f_costo" value="${data.costo||''}"></div>
            <div class="input-group"><label>PRECIO VENTA NETO</label><input type="number" id="f_precio" value="${data.precio||''}"></div>
            `;
        }
    }

    function guardarCRUD() {
        const type = document.getElementById('formType').value;
        const idx = parseInt(document.getElementById('formIdx').value);
        const key = type==='CLI'?DB_KEYS.CLI:DB_KEYS.PROD;
        const db = getDB(key);
        
        let obj = {};
        if(type==='CLI') {
            obj = { rut:document.getElementById('f_rut').value, razon:document.getElementById('f_razon').value, 
                    giro:document.getElementById('f_giro').value, dir:document.getElementById('f_dir').value,
                    comuna:document.getElementById('f_comuna').value, region:document.getElementById('f_region').value,
                    email:document.getElementById('f_email').value, contacto:document.getElementById('f_contacto').value };
            if(!obj.rut) return alert("RUT Obligatorio");
        } else {
            obj = { cod:document.getElementById('f_cod').value, desc:document.getElementById('f_desc').value,
                    costo:parseFloat(document.getElementById('f_costo').value)||0, precio:parseFloat(document.getElementById('f_precio').value)||0 };
            if(!obj.cod) return alert("Código Obligatorio");
        }

        if(idx===-1) db.push(obj); else db[idx]=obj;
        setDB(key, db);
        document.getElementById('modalForm').style.display='none';
        if(document.getElementById('modalGestor').style.display!=='none') abrirGestor(type==='CLI'?'clientes':'productos');
    }

    function cargarLogo(input) {
        if(input.files && input.files[0]) {
            const reader = new FileReader();
            reader.onload = e => {
                localStorage.setItem(DB_KEYS.LOGO, e.target.result);
                document.getElementById('logoPreview').src = e.target.result;
                document.getElementById('logoPreview').style.display = 'block';
            };
            reader.readAsDataURL(input.files[0]);
        }
    }

    function descargarRespaldo() {
        const data = JSON.stringify(localStorage);
        const blob = new Blob([data], {type: "application/json"});
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = `CUNTEL_BACKUP_${new Date().toISOString().slice(0,10)}.json`;
        a.click();
    }

    function importarRespaldo(input) {
        const file = input.files[0];
        if(!file) return;
        const reader = new FileReader();
        reader.onload = e => {
            try {
                const data = JSON.parse(e.target.result);
                Object.keys(data).forEach(k => localStorage.setItem(k, data[k]));
                alert("Restauración completada.");
                location.reload();
            } catch(err) { alert("Error archivo"); }
        };
        reader.readAsText(file);
    }

    function loadHistory(i) {
        const h = getDB(DB_KEYS.HIST)[i];
        document.getElementById('editIndex').value = i;
        document.getElementById('bar-edicion').style.display='block';
        document.getElementById('modalGestor').style.display='none';
        
        document.getElementById('lblCorrelativo').innerText = h.n;
        document.getElementById('cmbPago').value = h.pago || "TRANSFERENCIA";
        document.getElementById('txtDescPorc').value = h.descPorc || 0;
        
        ['Razon','Rut','Giro','Dir','Comuna','Region','Email','Contacto'].forEach(f => {
            if(document.getElementById('cli'+f)) document.getElementById('cli'+f).value = h.cli[f.toLowerCase()]||'';
        });

        const tbody = document.querySelector('#tablaItems tbody');
        tbody.innerHTML='';
        h.items.forEach(it => {
            const tr = document.createElement('tr');
            tr.innerHTML = `
            <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:red;border:none;background:none;font-weight:bold">X</button></td>
            <td><input type="text" class="cell-edit" value="${it.d}" onkeyup="buscarProd(this)" oninput="upper(this)"><div class="search-results"></div></td>
            <td><input type="number" class="cell-edit cell-qty" value="${it.q}" oninput="calcular()"></td>
            <td><input type="text" class="cell-edit cell-locked u-cost" readonly></td>
            <td><input type="text" class="cell-edit cell-locked t-cost" readonly></td>
            <td><input type="text" class="cell-edit cell-locked u-price" readonly></td>
            <td><input type="text" class="cell-edit cell-locked t-price" readonly></td>
            <td><input type="text" class="cell-edit cell-locked util" readonly style="font-size:9px"></td>
            <input type="hidden" class="raw-cost" value="${it.c}"><input type="hidden" class="raw-price" value="${it.p}"><input type="hidden" class="raw-cod" value="${it.cd||''}">`;
            tbody.appendChild(tr);
            tr.querySelector('.u-cost').value=formatMoney(it.c);
            tr.querySelector('.u-price').value=formatMoney(it.p);
        });
        calcular();
    }
    
    function nuevaCotizacion() { if(confirm("¿Limpiar?")) location.reload(); }

    function guardarYGenerar() {
        if(!document.getElementById('cliRazon').value) return alert("Falta Cliente");

        const cli = {
            razon: document.getElementById('cliRazon').value,
            rut: document.getElementById('cliRut').value,
            giro: document.getElementById('cliGiro').value,
            dir: document.getElementById('cliDir').value,
            comuna: document.getElementById('cliComuna').value,
            region: document.getElementById('cliRegion').value,
            email: document.getElementById('cliEmail').value,
            contacto: document.getElementById('cliContacto').value
        };

        const items = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.cell-edit').value;
            if(desc) items.push({
                d: desc,
                cd: tr.querySelector('.raw-cod').value || '',
                q: parseFloat(tr.querySelector('.cell-qty').value)||0,
                c: parseFloat(tr.querySelector('.raw-cost').value)||0,
                p: parseFloat(tr.querySelector('.raw-price').value)||0
            });
        });

        if(items.length===0) return alert("Agregue Items");

        const nCot = document.getElementById('lblCorrelativo').innerText;
        const total = document.getElementById('txtTotal').innerText;
        const pago = document.getElementById('cmbPago').value;
        const descPorc = document.getElementById('txtDescPorc').value;
        
        const registro = { n: nCot, fecha: document.getElementById('lblFecha').innerText, cli, items, total, pago, descPorc };
        
        const hist = getDB(DB_KEYS.HIST);
        const editIdx = parseInt(document.getElementById('editIndex').value);

        if(editIdx > -1) hist[editIdx] = registro;
        else {
            hist.push(registro);
            let s = parseInt(nCot.replace('CN',''));
            localStorage.setItem(DB_KEYS.SEQ, s+1);
        }
        
        setDB(DB_KEYS.HIST, hist);
        generarPDF(registro);
    }

    function generarPDF(data) {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();
        const azul = [31, 111, 139];
        
        const logo = localStorage.getItem(DB_KEYS.LOGO);
        if(logo) doc.addImage(logo, 'PNG', 15, 15, 25, 25);
        
        doc.setFontSize(22); doc.setTextColor(...azul); doc.setFont("helvetica","bold");
        doc.text("INGENIERIA CUNTEL SPA", 50, 25);
        doc.setFontSize(9); doc.setTextColor(100); doc.setFont("helvetica","normal");
        doc.text("SOLUCIONES INTEGRALES", 50, 30);
        doc.text("WEB: WWW.CUNTEL.CL | CONTACTO@CUNTEL.CL", 50, 35);
        
        doc.setFillColor(245,245,245); doc.rect(140, 15, 55, 20, 'F');
        doc.setFontSize(10); doc.setTextColor(242, 92, 5); doc.setFont("helvetica","bold");
        doc.text("COTIZACIÓN", 167, 22, {align:"center"});
        doc.setTextColor(0); doc.text(data.n, 167, 30, {align:"center"});

        doc.setDrawColor(31, 111, 139); doc.setLineWidth(0.5);
        doc.line(15, 45, 195, 45);
        
        doc.setFontSize(8); doc.setTextColor(...azul); doc.setFont("helvetica","bold");
        doc.text("INFORMACIÓN DEL CLIENTE", 15, 50);
        
        // CORRECCIÓN SOLICITADA: MOSTRAR TODOS LOS DATOS EN PDF
        doc.setTextColor(0); doc.setFontSize(8); doc.setFont("helvetica","normal");
        const c = data.cli;
        doc.text(`RAZÓN SOCIAL: ${c.razon}`, 15, 56); doc.text(`RUT: ${c.rut}`, 120, 56);
        doc.text(`GIRO: ${c.giro}`, 15, 61); doc.text(`DIRECCIÓN: ${c.dir}`, 120, 61);
        doc.text(`COMUNA: ${c.comuna}`, 15, 66); doc.text(`REGIÓN: ${c.region}`, 120, 66);
        doc.text(`CONTACTO: ${c.contacto}`, 15, 71); doc.text(`EMAIL: ${c.email}`, 120, 71);

        // CORRECCIÓN SOLICITADA: COLUMNAS PDF (ITEM, COD, DESC, P.UNIT, TOTAL)
        // Nota: Agrego la cantidad en la descripción para que tenga sentido matemático
        const rows = data.items.map((i, idx) => [
            idx + 1,
            i.cd,
            `(CANT: ${i.q}) ${i.d}`, 
            formatMoney(i.p), 
            formatMoney(i.q*i.p)
        ]);

        doc.autoTable({
            startY: 80,
            head: [['ITEM', 'CÓDIGO', 'DESCRIPCIÓN', 'P.UNITARIO', 'TOTAL']],
            body: rows,
            theme: 'plain',
            styles: { fontSize: 8, cellPadding: 3 },
            headStyles: { fillColor: azul, textColor: 255, fontStyle:'bold' },
            columnStyles: { 0:{halign:'center', cellWidth:10}, 1:{cellWidth:25}, 3:{halign:'right'}, 4:{halign:'right', fontStyle:'bold'} },
            didParseCell: d => { if(d.section==='body' && d.row.index%2===0) d.cell.styles.fillColor=[250,250,250]; }
        });

        const finalY = doc.lastAutoTable.finalY + 5;
        
        doc.setFontSize(8); doc.setTextColor(0); doc.setFont("helvetica","bold");
        doc.text("MODALIDAD DE PAGO:", 15, finalY + 5);
        doc.setFont("helvetica","normal");
        doc.text(data.pago, 50, finalY + 5);

        const boxX = 130;
        let y = finalY + 5;
        
        const neto = data.items.reduce((a,b)=>a+(b.q*b.p),0);
        const desc = neto * (data.descPorc/100);
        const netoF = neto - desc;
        const iva = netoF * 0.19;

        doc.text("SUBTOTAL:", boxX, y); doc.text(formatMoney(neto), 195, y, {align:'right'}); y+=5;
        if(desc > 0) {
            doc.text(`DESCUENTO (${data.descPorc}%):`, boxX, y); doc.text("- "+formatMoney(desc), 195, y, {align:'right'}); y+=5;
        }
        doc.text("NETO:", boxX, y); doc.text(formatMoney(netoF), 195, y, {align:'right'}); y+=5;
        doc.text("IVA (19%):", boxX, y); doc.text(formatMoney(iva), 195, y, {align:'right'}); y+=7;
        
        doc.setFillColor(...azul); doc.rect(boxX-5, y-4, 75, 12, 'F');
        doc.setTextColor(255); doc.setFontSize(10); doc.setFont("helvetica","bold");
        doc.text("TOTAL:", boxX, y+3); doc.text(formatMoney(netoF+iva), 195, y+3, {align:'right'});

        // CORRECCIÓN SOLICITADA: PIE DE FIRMA
        const pageHeight = doc.internal.pageSize.height;
        doc.setTextColor(150); doc.setFontSize(7); doc.setFont("helvetica","normal");
        doc.text("Generado por INGENIERIA CUNTEL SPA", 105, pageHeight - 10, {align:"center"});

        doc.save(`Cotizacion_${data.n}.pdf`);
        setTimeout(() => location.reload(), 2000);
    }
</script>
</body>
</html>
