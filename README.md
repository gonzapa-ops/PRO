<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SISTEMA PRO V13 - INGENIERIA CUNTEL SPA</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS GENERALES (V12 Base) --- */
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
        .top-bar { background: var(--dark); color: white; padding: 10px 20px; display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; gap: 10px; }
        .top-menu { display: flex; gap: 8px; flex-wrap: wrap; }
        .btn-nav { background: #34495E; border: 1px solid #566573; color: white; padding: 6px 12px; cursor: pointer; border-radius: 3px; font-size: 10px; font-weight: bold; transition: 0.2s; display: flex; align-items: center; gap: 5px; white-space: nowrap; }
        .btn-nav:hover { background: var(--secondary); border-color: var(--secondary); }
        .btn-backup { background: #27AE60; border-color: #1E8449; }

        /* HEADER */
        .header { padding: 20px; display: flex; flex-wrap: wrap; justify-content: space-between; border-bottom: 3px solid var(--secondary); background: white; align-items: center; gap: 20px; }
        
        .brand-area { display: flex; align-items: center; gap: 15px; }
        .logo-placeholder { width: 60px; height: 60px; background: #eee; border: 2px dashed #ccc; display: flex; align-items: center; justify-content: center; cursor: pointer; overflow: hidden; border-radius: 4px; flex-shrink: 0; }
        .logo-placeholder img { width: 100%; height: 100%; object-fit: contain; }
        
        .brand h1 { color: var(--primary); font-size: 20px; font-weight: 900; margin: 0; word-break: break-word; }
        .brand p { color: #7F8C8D; font-size: 10px; font-weight: 600; }

        .quote-data { text-align: right; }
        .quote-number { font-size: 18px; font-weight: bold; color: var(--secondary); white-space: nowrap; }
        
        #bar-edicion { display: none; background: #FFF3CD; color: #856404; text-align: center; padding: 8px; font-weight: bold; border-bottom: 1px solid #FFEEBA; }

        /* CONTENIDO */
        .content { padding: 20px; }
        .section-box { margin-bottom: 25px; border: 1px solid #D5DBDB; border-radius: 5px; padding: 15px; background: #FBFCFC; position: relative; }
        .section-label { position: absolute; top: -10px; left: 15px; background: var(--primary); color: white; padding: 3px 10px; font-size: 10px; font-weight: bold; border-radius: 3px; }

        /* INPUTS */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 12px; }
        .input-group label { display: block; font-size: 9px; font-weight: bold; color: #7F8C8D; margin-bottom: 4px; }
        .input-group input, .input-group select { width: 100%; padding: 6px 10px; border: 1px solid #D5DBDB; border-radius: 3px; font-size: 11px; font-weight: 600; color: var(--dark); height: 35px; }
        .input-group input:focus, .input-group select:focus { border-color: var(--secondary); outline: none; background: #fff; }
        .input-group input[readonly] { background: #F2F4F4; color: #777; cursor: default; }

        /* === RESTAURADO: SMART SEARCH BAR (V11) === */
        .smart-search-wrapper { position: relative; margin-bottom: 15px; display: flex; gap: 10px; flex-wrap: wrap; }
        .smart-search-input { 
            flex: 1; padding: 10px 15px; font-size: 12px; 
            border: 2px solid #3498DB; border-radius: 4px; 
            background: #fff; transition: 0.3s;
        }
        .smart-search-input:focus { border-color: var(--secondary); box-shadow: 0 0 0 3px rgba(31, 111, 139, 0.1); }
        
        .smart-results { 
            position: absolute; top: 100%; left: 0; width: 100%; 
            background: white; border-radius: 4px; box-shadow: 0 10px 30px rgba(0,0,0,0.2); 
            z-index: 100; max-height: 250px; overflow-y: auto; display: none; border: 1px solid #ddd;
        }
        .smart-item { padding: 10px; border-bottom: 1px solid #eee; cursor: pointer; display: flex; justify-content: space-between; align-items: center; }
        .smart-item:hover { background: #EBF5FB; }
        .smart-item .main-text { font-weight: bold; color: var(--dark); font-size: 11px; }
        .smart-item .sub-text { font-size: 10px; color: #7f8c8d; margin-left: 10px; }
        .smart-item .price-tag { background: #E8F8F5; color: var(--green); padding: 2px 6px; border-radius: 4px; font-weight: bold; font-size: 10px; }
        .create-new { background: #FDEDEC; color: #C0392B; font-weight: bold; }

        /* TABLA RESPONSIVE */
        .table-responsive { overflow-x: auto; -webkit-overflow-scrolling: touch; margin-bottom: 15px; border: 1px solid #D5DBDB; border-radius: 4px; }
        table { width: 100%; border-collapse: collapse; font-size: 10px; min-width: 800px; }
        th { background: var(--primary); color: white; padding: 10px; text-align: left; white-space: nowrap; }
        td { border: 1px solid #D5DBDB; padding: 0; height: 32px; }
        
        input.cell-edit { width: 100%; height: 100%; border: none; padding: 0 8px; font-size: 11px; background: transparent; }
        input.cell-edit:focus { outline: 2px solid var(--secondary); background: white; z-index: 10; position: relative; }
        input.cell-locked { background: #F8F9F9; color: #777; text-align: right; cursor: not-allowed; }
        input.cell-qty { text-align: center; font-weight: bold; color: var(--primary); }
        
        .col-cost { background: #FEF2F2; color: #922B21; }
        .col-price { background: #F4F6F7; color: #117A65; }
        .col-util { text-align: right; padding-right: 5px; font-weight: bold; color: blue; }

        /* BOTTOM AREA */
        .bottom-area { display: grid; grid-template-columns: 1fr 350px; gap: 20px; margin-top: 20px; }
        .notes-box textarea { width: 100%; height: 100px; padding: 10px; border: 1px solid #ddd; border-radius: 5px; font-family: inherit; resize: none; box-sizing: border-box; }

        .totals-card { background: white; border: 1px solid #D5DBDB; padding: 15px; border-radius: 5px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 6px; font-size: 11px; font-weight: 600; color: #555; align-items: center; }
        
        .final-row { border-top: 2px solid var(--secondary); padding-top: 10px; margin-top: 10px; display: flex; justify-content: space-between; align-items: center; }
        .total-price { font-size: 20px; font-weight: 900; color: var(--primary); }
        .util-label { font-size: 10px; color: var(--green); font-weight: bold; display: block; text-align: right; }

        /* BOTONES */
        .action-area { text-align: center; margin-top: 30px; border-top: 1px dashed #BDC3C7; padding-top: 20px; }
        .btn-main { background: var(--secondary); color: white; padding: 14px 40px; font-size: 14px; font-weight: 800; border: none; border-radius: 4px; cursor: pointer; box-shadow: 0 4px 15px rgba(242, 92, 5, 0.3); width: 100%; max-width: 400px; }
        .btn-main:hover { background: #D35400; transform: translateY(-1px); }

        /* MEDIA QUERIES */
        @media (max-width: 768px) {
            .header { flex-direction: column; text-align: center; }
            .brand-area { flex-direction: column; }
            .quote-data { text-align: center; width: 100%; margin-top: 10px; }
            .bottom-area { grid-template-columns: 1fr; }
            .totals-card { width: 100%; }
            .top-bar { justify-content: center; }
            .smart-search-wrapper { flex-direction: column; }
            .btn-nav { flex: 1 1 auto; justify-content: center; }
        }

        /* MODALES */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 2000; align-items: center; justify-content: center; backdrop-filter: blur(2px); }
        .modal-box { background: white; width: 95%; max-width: 1000px; max-height: 90vh; overflow-y: auto; padding: 20px; border-radius: 6px; }
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
        <div style="font-weight: 800; font-size: 12px; opacity: 0.8; margin-bottom: 5px;">PANEL DE CONTROL</div>
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
                <h1>INGENIERIA CUNTEL SPA</h1>
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
                <input type="text" id="buscadorCliente" placeholder="🔍 BUSCAR CLIENTE..." onkeyup="buscarCliente(this)" autocomplete="off" style="border: 2px solid var(--primary);">
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
            
            <div class="smart-search-wrapper">
                <input type="text" id="smartProductSearch" class="smart-search-input" placeholder="✨ BUSCAR CÓDIGO O PRODUCTO PARA AGREGAR..." onkeyup="smartSearchProduct(this)" autocomplete="off">
                <div id="smartProductResults" class="smart-results"></div>
                <button class="btn-nav" style="background:#334155;" onclick="agregarFilaManual()">+ MANUAL</button>
            </div>

            <div class="table-responsive">
                <table id="tablaItems">
                    <thead>
                        <tr>
                            <th style="width: 30px; text-align:center;">X</th>
                            <th>CÓDIGO / DESCRIPCIÓN</th>
                            <th style="width: 60px; text-align:center;">CANT</th>
                            
                            <th style="width: 90px;" class="col-cost">COSTO U.</th>
                            <th style="width: 90px;" class="col-cost">T. COSTO</th>
                            
                            <th style="width: 90px;" class="col-price">PRECIO U.</th>
                            <th style="width: 90px;" class="col-price">TOTAL</th>
                            <th style="width: 70px;">% UTIL</th>
                        </tr>
                    </thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>

        <div class="bottom-area">
            <div>
                <div class="section-box" style="margin-bottom:20px; padding: 15px;">
                    <span class="section-label">PAGO</span>
                    <label style="font-size:9px; font-weight:bold; color:#777;">MODALIDAD DE PAGO:</label>
                    <select id="cmbPago" style="width:100%; margin-top:5px; padding:5px;">
                        <option value="TRANSFERENCIA">TRANSFERENCIA</option>
                        <option value="WEBPAY">WEBPAY</option>
                        <option value="EFECTIVO">EFECTIVO</option>
                        <option value="CHEQUE 30 DÍAS">CHEQUE 30 DÍAS</option>
                        <option value="CHEQUE AL DÍA">CHEQUE AL DÍA</option>
                        <option value="OC 30 DÍAS">OC 30 DÍAS</option>
                    </select>
                </div>

                <div class="section-box notes-box" style="margin-bottom:0; padding:15px;">
                    <span class="section-label">OBSERVACIONES</span>
                    <textarea id="txtObservaciones" placeholder="Indique condiciones especiales..."></textarea>
                </div>
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
        <div class="table-responsive">
            <table class="manage-table">
                <thead id="modalHead"></thead>
                <tbody id="modalBody"></tbody>
            </table>
        </div>
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
    // --- 1. CORE ---
    const DB_KEYS = { CLI: 'db_clients', PROD: 'db_products', HIST: 'db_history', SEQ: 'db_seq', LOGO: 'db_logo' };
    const formatMoney = n => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(n);
    const upper = e => e.value = e.value.toUpperCase();
    
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
    };

    // --- 2. BUSCADORES Y TABLA ---
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
                d.innerHTML = `<div><strong>${c.rut}</strong> ${c.razon}</div>`;
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
        create.innerHTML = `+ CREAR CLIENTE NUEVO`;
        create.onclick = () => { list.style.display='none'; openForm('CLI', -1); };
        list.appendChild(create);
        list.style.display='block';
    }

    // SMART SEARCH (Restaurado V11)
    function smartSearchProduct(input) {
        upper(input);
        const term = input.value.trim();
        const list = document.getElementById('smartProductResults');
        list.innerHTML = '';
        
        if(term.length === 0) { list.style.display='none'; return; }

        const hits = getDB(DB_KEYS.PROD).filter(p => p.cod.includes(term) || p.desc.includes(term));
        
        if(hits.length > 0) {
            hits.forEach(p => {
                const d = document.createElement('div');
                d.className = 'search-item';
                d.innerHTML = `
                    <div><span style="font-weight:bold">${p.cod}</span> - ${p.desc}</div>
                    <div style="font-size:10px; font-weight:bold; color:green">${formatMoney(p.precio)}</div>
                `;
                d.onclick = () => {
                    agregarFilaDirecta(p);
                    input.value = "";
                    list.style.display = 'none';
                    input.focus();
                };
                list.appendChild(d);
            });
        }
        const create = document.createElement('div');
        create.className = 'search-item create-new';
        create.innerHTML = `+ CREAR PRODUCTO: "${term}"`;
        create.onclick = () => { list.style.display='none'; openForm('PROD', -1); };
        list.appendChild(create);
        list.style.display = 'block';
    }

    function agregarFilaDirecta(p) {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:red;border:none;background:none;font-weight:bold;cursor:pointer;font-size:14px;">&times;</button></td>
            <td><input type="text" class="cell-edit" value="${p.cod} - ${p.desc}" readonly></td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            <td class="col-cost"><input type="text" class="cell-edit cell-locked u-cost" value="${formatMoney(p.costo)}" readonly></td>
            <td class="col-cost"><input type="text" class="cell-edit cell-locked t-cost" readonly></td>
            <td class="col-price"><input type="text" class="cell-edit cell-locked u-price" value="${formatMoney(p.precio)}" readonly></td>
            <td class="col-price"><input type="text" class="cell-edit cell-locked t-price" readonly></td>
            <td><input type="text" class="cell-edit cell-locked util-porc" readonly style="color:blue; font-weight:bold; text-align:right;"></td>
            <input type="hidden" class="raw-cost" value="${p.costo}">
            <input type="hidden" class="raw-price" value="${p.precio}">
            <input type="hidden" class="raw-cod" value="${p.cod}">
            <input type="hidden" class="raw-desc" value="${p.desc}">
        `;
        tbody.appendChild(tr);
        calcular();
    }

    function agregarFilaManual() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:red;border:none;background:none;font-weight:bold;cursor:pointer;">&times;</button></td>
            <td><input type="text" class="cell-edit" placeholder="DESCRIPCIÓN MANUAL..." oninput="upper(this)"></td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            <td class="col-cost"><input type="number" class="cell-edit raw-cost-manual" value="0" oninput="calcularManual(this)"></td>
            <td class="col-cost"><input type="text" class="cell-edit cell-locked t-cost" readonly></td>
            <td class="col-price"><input type="number" class="cell-edit raw-price-manual" value="0" oninput="calcularManual(this)"></td>
            <td class="col-price"><input type="text" class="cell-edit cell-locked t-price" readonly></td>
            <td><input type="text" class="cell-edit cell-locked util-porc" readonly style="color:blue; font-weight:bold; text-align:right;"></td>
            <input type="hidden" class="raw-cost" value="0">
            <input type="hidden" class="raw-price" value="0">
            <input type="hidden" class="raw-cod" value="MANUAL">
        `;
        tbody.appendChild(tr);
    }

    function calcularManual(input) {
        const tr = input.closest('tr');
        tr.querySelector('.raw-cost').value = tr.querySelector('.raw-cost-manual').value;
        tr.querySelector('.raw-price').value = tr.querySelector('.raw-price-manual').value;
        calcular();
    }

    function calcular() {
        let neto = 0, util = 0;
        const descPorc = parseFloat(document.getElementById('txtDescPorc').value)||0;
        const factorDesc = (100 - descPorc) / 100;

        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const q = parseFloat(tr.querySelector('.cell-qty').value)||0;
            const c = parseFloat(tr.querySelector('.raw-cost').value)||0;
            const p = parseFloat(tr.querySelector('.raw-price').value)||0;
            
            const tc = q*c; 
            const tp = q*p;
            
            const pConDescuento = p * factorDesc;
            let margenPorc = 0;
            if(pConDescuento > 0) margenPorc = ((pConDescuento - c) / pConDescuento) * 100;

            tr.querySelector('.t-cost').value = formatMoney(tc);
            tr.querySelector('.t-price').value = formatMoney(tp);
            
            const celdaUtil = tr.querySelector('.util-porc');
            celdaUtil.value = margenPorc.toFixed(1) + '%';
            celdaUtil.style.color = margenPorc < 0 ? 'red' : 'blue';

            neto += tp; 
            util += ( (tp * factorDesc) - tc ); 
        });

        const descMonto = neto * (descPorc/100);
        const netoFinal = neto - descMonto;
        const iva = netoFinal * 0.19;
        
        document.getElementById('txtNeto').innerText = formatMoney(neto);
        document.getElementById('txtDescMonto').innerText = formatMoney(descMonto);
        document.getElementById('txtNetoFinal').innerText = formatMoney(netoFinal);
        document.getElementById('txtIva').innerText = formatMoney(iva);
        document.getElementById('txtTotal').innerText = formatMoney(netoFinal + iva);
        document.getElementById('txtUtilidad').innerText = formatMoney(util);
    }

    // --- 3. CRUD ---
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
            document.getElementById('modalTitle').innerText="BASE DE PRODUCTOS";
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
            document.getElementById('formTitle').innerText = idx===-1 ? "NUEVO CLIENTE" : "EDITAR CLIENTE";
            const fields = [
                {id:'rut',l:'RUT'}, {id:'razon',l:'RAZÓN SOCIAL'},
                {id:'giro',l:'GIRO'}, {id:'dir',l:'DIRECCIÓN'}, 
                {id:'comuna',l:'COMUNA'}, {id:'region',l:'REGIÓN'},
                {id:'email',l:'EMAIL'}, {id:'contacto',l:'CONTACTO'}
            ];
            fields.forEach(f => content.innerHTML+=`<div class="input-group"><label>${f.l}</label><input id="f_${f.id}" value="${data[f.id]||''}" oninput="upper(this)"></div>`);
        } else {
            document.getElementById('formTitle').innerText = idx===-1 ? "NUEVO PRODUCTO" : "EDITAR PRODUCTO";
            content.innerHTML+=`
            <div class="input-group"><label>CÓDIGO (SKU)</label><input id="f_cod" value="${data.cod||''}" oninput="upper(this)"></div>
            <div class="input-group"><label>DESCRIPCIÓN</label><input id="f_desc" value="${data.desc||''}" oninput="upper(this)"></div>
            <div class="input-group"><label>COSTO NETO ($)</label><input type="number" id="f_costo" value="${data.costo||''}"></div>
            <div class="input-group"><label>PRECIO VENTA NETO ($)</label><input type="number" id="f_precio" value="${data.precio||''}"></div>
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

    // --- 4. BACKUP & LOGO ---
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

    // --- 5. PDF & GUARDADO ---
    function loadHistory(i) {
        const h = getDB(DB_KEYS.HIST)[i];
        document.getElementById('editIndex').value = i;
        document.getElementById('bar-edicion').style.display='block';
        document.getElementById('modalGestor').style.display='none';
        
        document.getElementById('lblCorrelativo').innerText = h.n;
        document.getElementById('cmbPago').value = h.pago || "TRANSFERENCIA";
        document.getElementById('txtDescPorc').value = h.descPorc || 0;
        document.getElementById('txtObservaciones').value = h.obs || "";
        
        ['Razon','Rut','Giro','Dir','Comuna','Region','Email','Contacto'].forEach(f => {
            if(document.getElementById('cli'+f)) document.getElementById('cli'+f).value = h.cli[f.toLowerCase()]||'';
        });

        const tbody = document.querySelector('#tablaItems tbody');
        tbody.innerHTML='';
        h.items.forEach(it => {
            if(it.cd !== "MANUAL") {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:#EF4444;border:none;background:none;font-weight:bold;cursor:pointer;font-size:14px;">&times;</button></td>
                <td><input type="text" class="cell-edit" value="${it.cd} - ${it.d}" readonly></td>
                <td><input type="number" class="cell-edit cell-qty" value="${it.q}" min="1" oninput="calcular()"></td>
                <td class="col-cost"><input type="text" class="cell-edit cell-locked u-cost" value="${formatMoney(it.c)}" readonly></td>
                <td class="col-cost"><input type="text" class="cell-edit cell-locked t-cost" readonly></td>
                <td class="col-price"><input type="text" class="cell-edit cell-locked u-price" value="${formatMoney(it.p)}" readonly></td>
                <td class="col-price"><input type="text" class="cell-edit cell-locked t-price" readonly></td>
                <td><input type="text" class="cell-edit cell-locked util-porc" readonly style="color:blue; font-weight:bold; text-align:right;"></td>
                <input type="hidden" class="raw-cost" value="${it.c}"><input type="hidden" class="raw-price" value="${it.p}"><input type="hidden" class="raw-cod" value="${it.cd}">`;
                tbody.appendChild(tr);
            } else {
                 const tr = document.createElement('tr');
                 tr.innerHTML = `
                    <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:#EF4444;border:none;background:none;font-weight:bold;cursor:pointer;">&times;</button></td>
                    <td><input type="text" class="cell-edit" value="${it.d}" oninput="upper(this)"></td>
                    <td><input type="number" class="cell-edit cell-qty" value="${it.q}" min="1" oninput="calcular()"></td>
                    <td class="col-cost"><input type="number" class="cell-edit raw-cost-manual" value="${it.c}" oninput="calcularManual(this)"></td>
                    <td class="col-cost"><input type="text" class="cell-edit cell-locked t-cost" readonly></td>
                    <td class="col-price"><input type="number" class="cell-edit raw-price-manual" value="${it.p}" oninput="calcularManual(this)"></td>
                    <td class="col-price"><input type="text" class="cell-edit cell-locked t-price" readonly></td>
                    <td><input type="text" class="cell-edit cell-locked util-porc" readonly style="color:blue; font-weight:bold; text-align:right;"></td>
                    <input type="hidden" class="raw-cost" value="${it.c}"><input type="hidden" class="raw-price" value="${it.p}"><input type="hidden" class="raw-cod" value="MANUAL">`;
                 tbody.appendChild(tr);
            }
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
            let cod = tr.querySelector('.raw-cod').value;
            let desc = "";
            const inputDesc = tr.querySelector('td:nth-child(2) input');
            if(inputDesc) desc = inputDesc.value;
            
            if(cod !== "MANUAL" && desc.includes(" - ")) {
                 desc = desc.split(" - ")[1]; 
            }

            if(desc) items.push({
                d: desc,
                cd: cod,
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
        const obs = document.getElementById('txtObservaciones').value;
        
        const registro = { n: nCot, fecha: document.getElementById('lblFecha').innerText, cli, items, total, pago, descPorc, obs };
        
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
        
        doc.setFontSize(16); doc.setTextColor(...azul); doc.setFont("helvetica","bold");
        doc.text("INGENIERIA CUNTEL SPA", 50, 25);
        doc.setFontSize(9); doc.setTextColor(100); doc.setFont("helvetica","normal");
        doc.text("SOLUCIONES INTEGRALES", 50, 30);
        doc.text("WEB: WWW.CUNTEL.CL | CONTACTO@CUNTEL.CL", 50, 35);
        
        doc.setFillColor(245,
