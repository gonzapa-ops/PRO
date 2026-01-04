<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA PRO V11 - INGENIERIA CUNTEL SPA</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS GENERALES --- */
        :root {
            --primary: #1F6F8B;
            --secondary: #F25C05;
            --bg: #F0F2F5;
            --text: #2C3E50;
            --dark: #17202A;
            --green: #27AE60;
            --light-white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, sans-serif; text-transform: uppercase; }
        body { background: var(--bg); padding: 15px; color: var(--text); font-size: 11px; padding-bottom: 80px; }
        
        .main-container { max-width: 1400px; margin: 0 auto; background: white; min-height: 100vh; box-shadow: 0 10px 40px rgba(0,0,0,0.08); border-radius: 12px; overflow: hidden; display: flex; flex-direction: column; }

        /* TOP BAR */
        .top-bar { background: var(--dark); color: white; padding: 12px 25px; display: flex; justify-content: space-between; align-items: center; }
        .top-menu { display: flex; gap: 10px; }
        .btn-nav { background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2); color: white; padding: 8px 15px; cursor: pointer; border-radius: 6px; font-size: 10px; font-weight: bold; transition: 0.2s; display: flex; align-items: center; gap: 6px; }
        .btn-nav:hover { background: var(--secondary); border-color: var(--secondary); }

        /* HEADER */
        .header { padding: 30px 40px; display: flex; justify-content: space-between; border-bottom: 3px solid var(--secondary); background: white; align-items: center; }
        .brand-area { display: flex; align-items: center; gap: 20px; }
        .logo-placeholder { width: 70px; height: 70px; background: #f8f9fa; border: 2px dashed #cbd5e0; display: flex; align-items: center; justify-content: center; cursor: pointer; overflow: hidden; border-radius: 8px; transition: 0.3s; }
        .logo-placeholder:hover { border-color: var(--primary); background: #fff; }
        .logo-placeholder img { width: 100%; height: 100%; object-fit: contain; }
        
        .brand h1 { color: var(--primary); font-size: 26px; font-weight: 900; margin: 0; letter-spacing: -0.5px; }
        .brand p { color: #7F8C8D; font-size: 10px; font-weight: 700; margin-top: 4px; }
        .quote-number { font-size: 24px; font-weight: 800; color: var(--secondary); }

        /* CONTENIDO */
        .content { padding: 30px 40px; }
        .section-box { margin-bottom: 30px; border: 1px solid #E2E8F0; border-radius: 8px; padding: 25px; background: #fff; position: relative; box-shadow: 0 4px 6px rgba(0,0,0,0.02); }
        .section-label { position: absolute; top: -12px; left: 20px; background: var(--primary); color: white; padding: 4px 12px; font-size: 10px; font-weight: bold; border-radius: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.2); }

        /* INPUTS */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .input-group label { display: block; font-size: 9px; font-weight: 700; color: #64748B; margin-bottom: 5px; }
        .input-group input, .input-group select { width: 100%; padding: 8px 12px; border: 1px solid #CBD5E1; border-radius: 6px; font-size: 11px; font-weight: 600; color: var(--dark); height: 36px; transition: 0.2s; }
        .input-group input:focus, .input-group select:focus { border-color: var(--secondary); outline: none; box-shadow: 0 0 0 3px rgba(242, 92, 5, 0.1); }
        .input-group input[readonly] { background: #F1F5F9; color: #64748B; cursor: default; }

        /* === NUEVA SECCIÓN: SMART SEARCH === */
        .smart-search-wrapper { position: relative; margin-bottom: 20px; display: flex; gap: 10px; }
        .smart-search-input { 
            width: 100%; padding: 12px 15px; font-size: 12px; 
            border: 2px solid #E2E8F0; border-radius: 8px; 
            background: #F8FAFC url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="%23999" viewBox="0 0 16 16"><path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/></svg>') no-repeat 15px center;
            padding-left: 40px; transition: 0.3s;
        }
        .smart-search-input:focus { border-color: var(--primary); background-color: #fff; box-shadow: 0 5px 15px rgba(31, 111, 139, 0.1); }
        
        .smart-results { 
            position: absolute; top: 110%; left: 0; width: 100%; 
            background: white; border-radius: 8px; box-shadow: 0 10px 30px rgba(0,0,0,0.15); 
            z-index: 100; max-height: 300px; overflow-y: auto; display: none; border: 1px solid #E2E8F0;
        }
        .smart-item { padding: 12px 15px; border-bottom: 1px solid #F1F5F9; cursor: pointer; display: flex; justify-content: space-between; align-items: center; transition: 0.1s; }
        .smart-item:hover { background: #F0F9FF; }
        .smart-item .main-text { font-weight: bold; color: var(--dark); font-size: 11px; }
        .smart-item .sub-text { font-size: 10px; color: #64748B; margin-left: 10px; }
        .smart-item .price-tag { background: #E0F2FE; color: var(--primary); padding: 3px 8px; border-radius: 4px; font-weight: bold; font-size: 10px; }
        
        /* TABLE MODERN */
        .table-container { border: 1px solid #E2E8F0; border-radius: 8px; overflow: hidden; }
        table { width: 100%; border-collapse: collapse; font-size: 11px; }
        th { background: #F8FAFC; color: #475569; padding: 12px 10px; text-align: left; font-weight: 700; border-bottom: 2px solid #E2E8F0; }
        td { border-bottom: 1px solid #F1F5F9; padding: 0; height: 38px; vertical-align: middle; }
        tr:hover td { background: #FAFAFA; }

        input.cell-edit { width: 100%; height: 100%; border: none; padding: 0 10px; font-size: 11px; background: transparent; transition: 0.2s; }
        input.cell-edit:focus { background: white; outline: 2px solid var(--secondary); z-index: 2; position: relative; }
        
        /* Colores de columnas */
        .col-cost { background: #FEF2F2; color: #991B1B; } /* Rojo suave para costos */
        .col-price { background: #F0FDF4; color: #166534; } /* Verde suave para venta */
        .col-locked { pointer-events: none; }

        /* Margen Indicator */
        .margin-dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; margin-right: 5px; }
        .dot-green { background: #22C55E; }
        .dot-red { background: #EF4444; }

        /* TOTALS AREA */
        .bottom-area { display: grid; grid-template-columns: 1fr 380px; gap: 30px; margin-top: 30px; }
        .notes-box textarea { width: 100%; height: 120px; border: 1px solid #CBD5E1; border-radius: 6px; padding: 10px; resize: none; font-size: 11px; }
        
        .totals-card { background: #F8FAFC; border: 1px solid #E2E8F0; padding: 20px; border-radius: 8px; }
        .t-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 11px; font-weight: 600; color: #475569; align-items: center; }
        
        .final-total { background: var(--primary); color: white; padding: 15px; border-radius: 6px; margin-top: 15px; display: flex; justify-content: space-between; align-items: center; }
        .final-total .label { font-size: 12px; font-weight: bold; }
        .final-total .value { font-size: 20px; font-weight: 900; }

        /* BOTONES */
        .action-area { text-align: center; margin-top: 40px; padding-top: 20px; border-top: 1px dashed #CBD5E1; }
        .btn-main { background: linear-gradient(135deg, var(--secondary) 0%, #D35400 100%); color: white; padding: 15px 50px; font-size: 14px; font-weight: 800; border: none; border-radius: 50px; cursor: pointer; box-shadow: 0 10px 20px rgba(242, 92, 5, 0.3); transition: transform 0.2s; }
        .btn-main:hover { transform: translateY(-2px); }

        /* MODALES */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 2000; align-items: center; justify-content: center; backdrop-filter: blur(4px); }
        .modal-box { background: white; width: 95%; max-width: 1000px; max-height: 90vh; overflow-y: auto; padding: 30px; border-radius: 12px; box-shadow: 0 20px 60px rgba(0,0,0,0.2); }
        
        .manage-table { width: 100%; margin-top: 15px; border: 1px solid #E2E8F0; }
        .manage-table th { padding: 10px; background: #334155; color: white; font-size: 10px; }
        .manage-table td { padding: 8px; font-size: 10px; border-bottom: 1px solid #F1F5F9; }

    </style>
</head>
<body>

<div class="main-container">

    <div class="top-bar">
        <div style="font-weight: 800; font-size: 12px; letter-spacing: 1px;">SISTEMA PRO V10</div>
        <div class="top-menu">
            <button class="btn-nav" onclick="abrirGestor('clientes')">👥 CLIENTES</button>
            <button class="btn-nav" onclick="abrirGestor('productos')">📦 PRODUCTOS</button>
            <button class="btn-nav" onclick="abrirGestor('historial')">📜 HISTORIAL</button>
            <button class="btn-nav btn-backup" onclick="descargarRespaldo()">⬇️ BACKUP</button>
            <label for="inputImport" class="btn-nav btn-backup" style="cursor:pointer;">⬆️ RESTAURAR</label>
            <input type="file" id="inputImport" style="display:none" onchange="importarRespaldo(this)">
            <button class="btn-nav" onclick="nuevaCotizacion()" style="background:#EF4444; border-color:#DC2626;">🔄 NUEVA</button>
        </div>
    </div>

    <div class="header">
        <div class="brand-area">
            <div class="logo-placeholder" onclick="document.getElementById('logoInput').click()">
                <img id="logoPreview" src="" alt="LOGO" style="display:none">
                <span id="logoText" style="font-size:9px; text-align:center; color:#94A3B8;">+ LOGO</span>
            </div>
            <input type="file" id="logoInput" style="display:none" accept="image/*" onchange="cargarLogo(this)">
            
            <div class="brand">
                <h1>CUNDO SPA</h1>
                <p>SOLUCIONES INTEGRALES</p>
            </div>
        </div>
        <div class="quote-data">
            <span style="font-size:10px; color:#64748B; font-weight:bold;">FOLIO COTIZACIÓN</span>
            <span class="quote-number" id="lblCorrelativo">...</span>
            <span style="font-size:11px; font-weight:bold; color:var(--primary); display:block; margin-top:5px;" id="lblFecha"></span>
        </div>
    </div>

    <div class="content">

        <div class="section-box">
            <span class="section-label">1. INFORMACIÓN DEL CLIENTE</span>
            
            <div class="smart-search-wrapper" style="margin-bottom:15px;">
                <input type="text" id="buscadorCliente" class="smart-search-input" placeholder="Escribe RUT o Nombre del Cliente..." onkeyup="buscarCliente(this)" autocomplete="off">
                <div id="listaClientes" class="smart-results"></div>
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
                <input type="text" id="smartProductSearch" class="smart-search-input" placeholder="✨ Buscador Inteligente: Escribe Código o Nombre del producto para añadir..." onkeyup="smartSearchProduct(this)" autocomplete="off">
                <div id="smartProductResults" class="smart-results"></div>
                <button class="btn-nav" style="background:#334155;" onclick="agregarFilaManual()">+ MANUAL</button>
            </div>

            <div class="table-container">
                <table id="tablaItems">
                    <thead>
                        <tr>
                            <th style="width: 40px; text-align:center;"></th>
                            <th>DESCRIPCIÓN / CÓDIGO</th>
                            <th style="width: 70px; text-align:center;">CANT</th>
                            
                            <th style="width: 100px; background:#FEF2F2; color:#7F1D1D;">COSTO U.</th>
                            <th style="width: 100px; background:#FEF2F2; color:#7F1D1D;">TOTAL COSTO</th>
                            
                            <th style="width: 100px; background:#F0FDF4; color:#14532D;">PRECIO U.</th>
                            <th style="width: 100px; background:#F0FDF4; color:#14532D;">TOTAL VENTA</th>
                            <th style="width: 80px;">UTILIDAD</th>
                        </tr>
                    </thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>

        <div class="bottom-area">
            <div class="section-box" style="margin-bottom:0; border:none; padding:0; box-shadow:none;">
                <div style="margin-bottom:20px;">
                    <span style="font-size:10px; font-weight:bold; color:var(--primary); display:block; margin-bottom:5px;">MODALIDAD DE PAGO</span>
                    <select id="cmbPago" style="width:100%; padding:10px; border:1px solid #CBD5E1; border-radius:6px; font-weight:bold;">
                        <option value="TRANSFERENCIA">TRANSFERENCIA</option>
                        <option value="WEBPAY">WEBPAY</option>
                        <option value="EFECTIVO">EFECTIVO</option>
                        <option value="CHEQUE 30 DÍAS">CHEQUE 30 DÍAS</option>
                        <option value="CHEQUE AL DÍA">CHEQUE AL DÍA</option>
                        <option value="OC 30 DÍAS">OC 30 DÍAS</option>
                    </select>
                </div>
                <div>
                    <span style="font-size:10px; font-weight:bold; color:var(--primary); display:block; margin-bottom:5px;">OBSERVACIONES</span>
                    <textarea id="txtObservaciones" placeholder="Indique condiciones especiales..."></textarea>
                </div>
            </div>

            <div class="totals-card">
                <div class="t-row">
                    <span>SUBTOTAL NETO</span>
                    <span id="txtNeto">$0</span>
                </div>
                <div class="t-row">
                    <span>DESCUENTO (%)</span>
                    <input type="number" id="txtDescPorc" value="0" min="0" max="100" style="width:60px; text-align:right; border:1px solid #ddd; border-radius:4px;" onchange="calcular()">
                </div>
                <div class="t-row">
                    <span>MONTO DESC.</span>
                    <span id="txtDescMonto">$0</span>
                </div>
                <div class="t-row" style="border-top:1px dashed #E2E8F0; padding-top:8px; margin-top:8px;">
                    <span>NETO FINAL</span>
                    <span id="txtNetoFinal">$0</span>
                </div>
                <div class="t-row">
                    <span>IVA (19%)</span>
                    <span id="txtIva">$0</span>
                </div>
                
                <div class="final-total">
                    <span class="label">TOTAL A PAGAR</span>
                    <span class="value" id="txtTotal">$0</span>
                </div>
                <div style="text-align:right; margin-top:10px; font-size:10px; color:var(--green); font-weight:bold;">
                    MARGEN UTILIDAD: <span id="txtUtilidad">$0</span>
                </div>
            </div>
        </div>

        <div class="action-area">
            <input type="hidden" id="editIndex" value="-1">
            <button class="btn-main" onclick="guardarYGenerar()">📄 GENERAR COTIZACIÓN OFICIAL</button>
        </div>

    </div>
</div>

<div id="modalGestor" class="modal">
    <div class="modal-box">
        <span class="close-btn" onclick="document.getElementById('modalGestor').style.display='none'" style="float:right; cursor:pointer; font-size:24px; color:#94A3B8;">&times;</span>
        <h3 id="modalTitle" style="color:var(--primary); margin-bottom:20px;">GESTIÓN</h3>
        <div style="margin-bottom:15px; text-align:right;">
             <button id="btnModalNew" class="btn-nav" style="background:var(--primary); float:right;">+ NUEVO REGISTRO</button>
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
        <h3 id="formTitle" style="color:var(--primary); border-bottom:2px solid var(--secondary); padding-bottom:10px; margin-bottom:20px;">...</h3>
        <input type="hidden" id="formType">
        <input type="hidden" id="formIdx">
        <div id="formContent" class="grid-form" style="grid-template-columns:1fr;"></div>
        <div style="margin-top:25px; display:flex; gap:10px;">
            <button onclick="guardarCRUD()" class="btn-main" style="width:100%; padding:10px;">GUARDAR</button>
            <button onclick="document.getElementById('modalForm').style.display='none'" style="width:100%; background:#94A3B8; border:none; border-radius:50px; color:white; font-weight:bold; cursor:pointer;">CANCELAR</button>
        </div>
    </div>
</div>

<script>
    // --- CONFIGURACIÓN ---
    const DB_KEYS = { CLI: 'db_clients', PROD: 'db_products', HIST: 'db_history', SEQ: 'db_seq', LOGO: 'db_logo' };
    const formatMoney = n => new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(n);
    const upper = e => e.value = e.value.toUpperCase();
    
    // Auto-formato RUT
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

    // --- BUSCADOR CLIENTES ---
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
                d.className = 'smart-item';
                d.innerHTML = `<div><div class="main-text">${c.razon}</div><div class="sub-text">${c.rut}</div></div>`;
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
        create.className = 'smart-item create-new';
        create.innerHTML = `+ CREAR CLIENTE NUEVO`;
        create.onclick = () => { list.style.display='none'; openForm('CLI', -1); };
        list.appendChild(create);
        list.style.display='block';
    }

    // --- SMART PRODUCT SEARCH (TECH FEATURE) ---
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
                d.className = 'smart-item';
                d.innerHTML = `
                    <div>
                        <span class="main-text">${p.cod}</span>
                        <span class="sub-text">${p.desc}</span>
                    </div>
                    <div class="price-tag">${formatMoney(p.precio)}</div>
                `;
                d.onclick = () => {
                    agregarFilaDirecta(p);
                    input.value = ""; // Limpiar buscador
                    list.style.display = 'none';
                    input.focus(); // Mantener foco para seguir buscando
                };
                list.appendChild(d);
            });
        }
        
        const create = document.createElement('div');
        create.className = 'smart-item create-new';
        create.innerHTML = `+ CREAR PRODUCTO: "${term}"`;
        create.onclick = () => { list.style.display='none'; openForm('PROD', -1); };
        list.appendChild(create);
        
        list.style.display = 'block';
    }

    // Función para añadir fila directamente desde el buscador inteligente
    function agregarFilaDirecta(p) {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        
        tr.innerHTML = `
            <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:#EF4444;border:none;background:none;font-weight:bold;cursor:pointer;font-size:14px;">&times;</button></td>
            <td>
                <input type="text" class="cell-edit" value="${p.cod} - ${p.desc}" readonly>
            </td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            
            <td class="col-cost"><input type="text" class="cell-edit col-locked u-cost" value="${formatMoney(p.costo)}" readonly></td>
            <td class="col-cost"><input type="text" class="cell-edit col-locked t-cost" readonly></td>
            
            <td class="col-price"><input type="text" class="cell-edit col-locked u-price" value="${formatMoney(p.precio)}" readonly></td>
            <td class="col-price"><input type="text" class="cell-edit col-locked t-price" readonly></td>
            
            <td>
                <div style="display:flex; align-items:center;">
                    <span class="margin-dot"></span>
                    <input type="text" class="cell-edit col-locked util" readonly style="font-size:10px">
                </div>
            </td>
            
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
            <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:#EF4444;border:none;background:none;font-weight:bold;cursor:pointer;">&times;</button></td>
            <td><input type="text" class="cell-edit" placeholder="Descripción Manual..." oninput="upper(this)"></td>
            <td><input type="number" class="cell-edit cell-qty" value="1" min="1" oninput="calcular()"></td>
            <td class="col-cost"><input type="number" class="cell-edit raw-cost-manual" value="0" oninput="calcularManual(this)"></td>
            <td class="col-cost"><input type="text" class="cell-edit col-locked t-cost" readonly></td>
            <td class="col-price"><input type="number" class="cell-edit raw-price-manual" value="0" oninput="calcularManual(this)"></td>
            <td class="col-price"><input type="text" class="cell-edit col-locked t-price" readonly></td>
            <td><div style="display:flex;align-items:center;"><span class="margin-dot"></span><input type="text" class="cell-edit col-locked util" readonly></div></td>
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
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const q = parseFloat(tr.querySelector('.cell-qty').value)||0;
            const c = parseFloat(tr.querySelector('.raw-cost').value)||0;
            const p = parseFloat(tr.querySelector('.raw-price').value)||0;
            
            const tc = q*c; const tp = q*p;
            const margin = tp - tc;
            
            tr.querySelector('.t-cost').value = formatMoney(tc);
            tr.querySelector('.t-price').value = formatMoney(tp);
            tr.querySelector('.util').value = formatMoney(margin);
            
            // Visual Margin Indicator
            const dot = tr.querySelector('.margin-dot');
            if(dot) {
                dot.className = 'margin-dot ' + (margin >= 0 ? 'dot-green' : 'dot-red');
            }

            neto += tp; util += margin;
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

    // --- GESTIÓN DATOS ---
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

    // --- PDF GENERATION ---
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
            // Manejo de filas manuales o automáticas
            let cod = tr.querySelector('.raw-cod').value;
            let desc = "";
            
            // Si es manual, obtener descripción del input
            const inputDesc = tr.querySelector('td:nth-child(2) input');
            if(inputDesc) desc = inputDesc.value;
            
            // Si viene de BD, limpiar el "COD - DESC"
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
        const obs = document.getElementById('txtObservaciones').value;
        const descPorc = document.getElementById('txtDescPorc').value;
        
        const registro = { n: nCot, fecha: document.getElementById('lblFecha').innerText, cli, items, total, pago, obs, descPorc };
        
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
        
        doc.setTextColor(0); doc.setFontSize(8); doc.setFont("helvetica","normal");
        const c = data.cli;
        doc.text(`RAZÓN SOCIAL: ${c.razon}`, 15, 56); doc.text(`RUT: ${c.rut}`, 120, 56);
        doc.text(`GIRO: ${c.giro}`, 15, 61); doc.text(`DIRECCIÓN: ${c.dir}`, 120, 61);
        doc.text(`COMUNA: ${c.comuna}`, 15, 66); doc.text(`REGIÓN: ${c.region}`, 120, 66);
        doc.text(`CONTACTO: ${c.contacto}`, 15, 71); doc.text(`EMAIL: ${c.email}`, 120, 71);

        // CORRECCIÓN SOLICITADA: COLUMNAS ITEM, COD, DESC, P.UNIT, TOTAL
        const rows = data.items.map((i, idx) => [
            idx + 1,
            i.cd === "MANUAL" ? "-" : i.cd,
            i.d, 
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
        
        if(data.obs) {
             doc.setFont("helvetica","bold");
             doc.text("OBSERVACIONES:", 15, finalY + 10);
             doc.setFont("helvetica","normal");
             doc.text(data.obs, 50, finalY + 10);
        }

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

        const pageHeight = doc.internal.pageSize.height;
        doc.setTextColor(150); doc.setFontSize(7); doc.setFont("helvetica","normal");
        doc.text("Generado por INGENIERIA CUNTEL SPA", 105, pageHeight - 10, {align:"center"});

        doc.save(`Cotizacion_${data.n}.pdf`);
        setTimeout(() => location.reload(), 2000);
    }
    
    // Funciones Auxiliares
    function nuevaCotizacion() { if(confirm("¿Limpiar?")) location.reload(); }
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
                // Reconstruir como item de BD
                const tr = document.createElement('tr');
                tr.innerHTML = `
                <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:#EF4444;border:none;background:none;font-weight:bold;cursor:pointer;font-size:14px;">&times;</button></td>
                <td><input type="text" class="cell-edit" value="${it.cd} - ${it.d}" readonly></td>
                <td><input type="number" class="cell-edit cell-qty" value="${it.q}" min="1" oninput="calcular()"></td>
                <td class="col-cost"><input type="text" class="cell-edit col-locked u-cost" value="${formatMoney(it.c)}" readonly></td>
                <td class="col-cost"><input type="text" class="cell-edit col-locked t-cost" readonly></td>
                <td class="col-price"><input type="text" class="cell-edit col-locked u-price" value="${formatMoney(it.p)}" readonly></td>
                <td class="col-price"><input type="text" class="cell-edit col-locked t-price" readonly></td>
                <td><div style="display:flex; align-items:center;"><span class="margin-dot"></span><input type="text" class="cell-edit col-locked util" readonly style="font-size:10px"></div></td>
                <input type="hidden" class="raw-cost" value="${it.c}"><input type="hidden" class="raw-price" value="${it.p}"><input type="hidden" class="raw-cod" value="${it.cd}">`;
                tbody.appendChild(tr);
            } else {
                // Reconstruir como manual
                 const tr = document.createElement('tr');
                 tr.innerHTML = `
                    <td style="text-align:center"><button onclick="this.closest('tr').remove();calcular()" style="color:#EF4444;border:none;background:none;font-weight:bold;cursor:pointer;">&times;</button></td>
                    <td><input type="text" class="cell-edit" value="${it.d}" oninput="upper(this)"></td>
                    <td><input type="number" class="cell-edit cell-qty" value="${it.q}" min="1" oninput="calcular()"></td>
                    <td class="col-cost"><input type="number" class="cell-edit raw-cost-manual" value="${it.c}" oninput="calcularManual(this)"></td>
                    <td class="col-cost"><input type="text" class="cell-edit col-locked t-cost" readonly></td>
                    <td class="col-price"><input type="number" class="cell-edit raw-price-manual" value="${it.p}" oninput="calcularManual(this)"></td>
                    <td class="col-price"><input type="text" class="cell-edit col-locked t-price" readonly></td>
                    <td><div style="display:flex;align-items:center;"><span class="margin-dot"></span><input type="text" class="cell-edit col-locked util" readonly></div></td>
                    <input type="hidden" class="raw-cost" value="${it.c}"><input type="hidden" class="raw-price" value="${it.p}"><input type="hidden" class="raw-cod" value="MANUAL">`;
                 tbody.appendChild(tr);
            }
        });
        calcular();
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
</script>
</body>
</html>
