<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema PRO - CUNDO SPA</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS VISUALES DEL SISTEMA --- */
        :root {
            --primary: #1F6F8B; /* Azul Corporativo */
            --secondary: #F25C05; /* Naranja Corporativo */
            --bg: #f4f7f6;
            --text: #2c3e50;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Roboto, Helvetica, sans-serif; }
        body { background: var(--bg); padding: 30px; color: var(--text); }
        
        .container { 
            max-width: 1400px; 
            margin: 0 auto; 
            background: white; 
            padding: 40px; 
            border-radius: 12px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.08); 
        }

        /* HEADER */
        .header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 40px; border-bottom: 2px solid #eee; padding-bottom: 20px; }
        .brand h1 { color: var(--primary); font-size: 32px; letter-spacing: -1px; margin-bottom: 5px; }
        .brand p { color: #7f8c8d; font-size: 14px; }
        
        .quote-meta { text-align: right; }
        .tag-cotizacion { background: var(--secondary); color: white; padding: 5px 15px; border-radius: 20px; font-size: 12px; font-weight: bold; text-transform: uppercase; margin-bottom: 10px; display: inline-block; }
        .input-meta { text-align: right; border: none; font-size: 18px; font-weight: bold; color: var(--primary); width: 150px; background: transparent; }

        /* SECCIONES */
        .section-title { 
            font-size: 14px; 
            color: var(--primary); 
            font-weight: 800; 
            text-transform: uppercase; 
            letter-spacing: 1px; 
            margin: 30px 0 15px 0; 
            border-left: 4px solid var(--secondary); 
            padding-left: 10px;
            display: flex;
            justify-content: space-between;
        }

        .btn-ghost { background: transparent; color: var(--primary); border: 1px solid var(--primary); padding: 4px 12px; border-radius: 4px; cursor: pointer; font-size: 11px; transition: 0.2s; }
        .btn-ghost:hover { background: var(--primary); color: white; }

        /* FORMULARIOS & AUTOCOMPLETE */
        .grid-form { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .form-group { position: relative; } /* Para el autocomplete */
        .form-group label { display: block; font-size: 11px; color: #7f8c8d; margin-bottom: 5px; font-weight: 600; }
        .form-group input { width: 100%; padding: 10px; border: 1px solid #e0e0e0; border-radius: 6px; font-size: 13px; transition: 0.3s; }
        .form-group input:focus { border-color: var(--primary); outline: none; box-shadow: 0 0 0 3px rgba(31, 111, 139, 0.1); }
        .form-group input[readonly] { background: #f9f9f9; color: #777; border-color: #eee; }

        /* SUGGESTION BOX (Buscador) */
        .suggestions-list {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: white;
            border: 1px solid #ddd;
            border-radius: 0 0 6px 6px;
            max-height: 200px;
            overflow-y: auto;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            z-index: 1000;
            display: none;
        }
        .suggestions-list div { padding: 10px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 12px; }
        .suggestions-list div:hover { background: #f0f8ff; color: var(--primary); }
        .suggestions-list div strong { color: var(--secondary); }

        /* TABLA */
        .table-wrapper { border: 1px solid #eee; border-radius: 8px; overflow: hidden; margin-bottom: 10px; }
        table { width: 100%; border-collapse: collapse; font-size: 12px; }
        th { background: #f8f9fa; color: var(--primary); padding: 12px; text-align: left; font-weight: 700; border-bottom: 2px solid #eee; }
        td { padding: 10px; border-bottom: 1px solid #eee; vertical-align: middle; }
        
        /* Columnas específicas */
        .th-costo { background: #fff8e1; color: #d35400; }
        .td-costo { background: #fffcf5; }
        .th-venta { background: #e8f5e9; color: #27ae60; }
        .td-venta { background: #f1f8e9; }

        input.row-input { width: 100%; border: none; padding: 5px; background: transparent; font-family: inherit; font-size: 12px; }
        input.row-input:focus { background: white; outline: 2px solid var(--secondary); border-radius: 3px; }

        /* TOTALES */
        .summary-section { display: flex; justify-content: flex-end; margin-top: 20px; }
        .summary-box { background: #f8f9fa; padding: 20px; border-radius: 8px; width: 350px; }
        .summary-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 13px; }
        .summary-row.final { border-top: 2px solid #ddd; margin-top: 10px; padding-top: 10px; font-size: 18px; font-weight: 800; color: var(--primary); }
        .utilidad-tag { font-size: 11px; text-align: right; color: #27ae60; margin-top: 5px; font-weight: bold; }

        /* ACTIONS */
        .actions-bar { margin-top: 40px; padding-top: 20px; border-top: 1px dashed #ddd; text-align: center; }
        .btn-pdf { 
            background: linear-gradient(135deg, var(--secondary) 0%, #d35400 100%);
            color: white; padding: 15px 40px; border: none; border-radius: 50px; 
            font-size: 16px; font-weight: bold; cursor: pointer; 
            box-shadow: 0 4px 15px rgba(242, 92, 5, 0.4); 
            transition: transform 0.2s;
        }
        .btn-pdf:hover { transform: translateY(-2px); }

        /* MODAL */
        .modal { display: none; position: fixed; z-index: 2000; left: 0; top: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); backdrop-filter: blur(2px); }
        .modal-content { background: white; margin: 5% auto; padding: 30px; width: 600px; border-radius: 12px; box-shadow: 0 20px 50px rgba(0,0,0,0.2); }
        .close-modal { float: right; font-size: 24px; cursor: pointer; color: #999; }
        .btn-save { width: 100%; background: var(--primary); color: white; border: none; padding: 12px; border-radius: 6px; margin-top: 15px; cursor: pointer; font-weight: bold; }

    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div class="brand">
            <h1>CUNDO SPA</h1>
            <p>Ingeniería, Servicios y Soluciones Integrales</p>
        </div>
        <div class="quote-meta">
            <span class="tag-cotizacion">Borrador</span>
            <div>
                N° <input type="text" id="correlativo" class="input-meta" readonly>
            </div>
            <div style="font-size: 12px; color: #999; margin-top: 5px;">
                Fecha: <span id="fechaHoyDisplay"></span>
            </div>
        </div>
    </div>

    <div class="section-title">
        <span>1. Información del Cliente</span>
        <button class="btn-ghost" onclick="abrirModal('modalCliente')">+ Nuevo Cliente</button>
    </div>

    <div class="grid-form" style="margin-bottom: 20px;">
        <div class="form-group" style="grid-column: span 2;">
            <label>BUSCAR CLIENTE (Escribe 3 letras...)</label>
            <input type="text" id="buscadorCliente" placeholder="Ej: Minera..." onkeyup="buscarCliente(this)" autocomplete="off" style="border: 2px solid var(--secondary);">
            <div id="listaResultadosCliente" class="suggestions-list"></div>
        </div>
    </div>

    <div class="grid-form">
        <div class="form-group"><label>Razón Social</label><input type="text" id="cliRazon" readonly></div>
        <div class="form-group"><label>RUT</label><input type="text" id="cliRut" readonly></div>
        <div class="form-group"><label>Giro</label><input type="text" id="cliGiro" readonly></div>
        <div class="form-group"><label>Dirección</label><input type="text" id="cliDir" readonly></div>
        <div class="form-group"><label>Comuna</label><input type="text" id="cliComuna" readonly></div>
        <div class="form-group"><label>Contacto</label><input type="text" id="cliContacto" readonly></div>
        <div class="form-group"><label>Email</label><input type="text" id="cliEmail" readonly></div>
    </div>

    <div class="section-title">
        <span>2. Detalle Económico</span>
        <button class="btn-ghost" onclick="abrirModal('modalProducto')">+ Nuevo Producto Base</button>
    </div>

    <div class="table-wrapper">
        <table id="tablaItems">
            <thead>
                <tr>
                    <th style="width: 30px;"></th>
                    <th style="width: 25%;">Descripción / Servicio</th> <th style="width: 50px; text-align: center;">Cant.</th>
                    
                    <th class="th-costo" style="width: 100px;">Costo Unit.</th>
                    <th class="th-costo" style="width: 100px;">Total Costo</th>

                    <th class="th-venta" style="width: 120px; border-left: 2px solid #ddd;">Venta Unit.</th>
                    <th class="th-venta" style="width: 120px;">Total Venta</th>
                    <th class="th-venta" style="width: 80px;">Utilidad</th>
                </tr>
            </thead>
            <tbody>
                </tbody>
        </table>
    </div>
    
    <button class="btn-ghost" onclick="agregarFila()">+ Añadir Línea Vacía</button>

    <div class="summary-section">
        <div class="summary-box">
            <div class="summary-row">
                <span>Subtotal Neto:</span>
                <span id="txtNeto">$0</span>
            </div>
            <div class="summary-row">
                <span>IVA (19%):</span>
                <span id="txtIva">$0</span>
            </div>
            <div class="summary-row final">
                <span>TOTAL A PAGAR:</span>
                <span id="txtTotal">$0</span>
            </div>
            <div class="utilidad-tag">
                Utilidad del Proyecto: <span id="txtUtilidad">$0</span> (<span id="txtMargen">0%</span>)
            </div>
        </div>
    </div>

    <div class="actions-bar">
        <button class="btn-pdf" onclick="generarPDFProfesional()">
            GENERAR PDF PROFESIONAL
        </button>
    </div>

</div>

<div id="modalCliente" class="modal">
    <div class="modal-content">
        <span class="close-modal" onclick="cerrarModal('modalCliente')">&times;</span>
        <h3>Registrar Nuevo Cliente</h3>
        <div class="grid-form" style="margin-top: 15px;">
            <input type="text" id="newCliRut" placeholder="RUT (Ej: 76.123.456-K)">
            <input type="text" id="newCliRazon" placeholder="Razón Social">
            <input type="text" id="newCliGiro" placeholder="Giro Comercial">
            <input type="text" id="newCliDir" placeholder="Dirección">
            <input type="text" id="newCliComuna" placeholder="Comuna">
            <input type="text" id="newCliContacto" placeholder="Nombre Contacto">
            <input type="email" id="newCliEmail" placeholder="Email">
        </div>
        <button class="btn-save" onclick="guardarCliente()">Guardar en Base de Datos</button>
    </div>
</div>

<div id="modalProducto" class="modal">
    <div class="modal-content">
        <span class="close-modal" onclick="cerrarModal('modalProducto')">&times;</span>
        <h3>Registrar Nuevo Producto/Servicio</h3>
        <div class="grid-form" style="margin-top: 15px;">
            <input type="text" id="newProdCod" placeholder="Código (Ej: PRO-001)">
            <input type="text" id="newProdDesc" placeholder="Descripción Detallada">
            <input type="number" id="newProdCosto" placeholder="Costo Neto Unitario ($)">
        </div>
        <button class="btn-save" onclick="guardarProducto()">Guardar en Base de Datos</button>
    </div>
</div>

<script>
    // --- CONFIGURACIÓN Y UTILIDADES ---
    const formatter = new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP', minimumFractionDigits: 0 });
    const fechaHoy = new Date().toLocaleDateString('es-CL');
    document.getElementById('fechaHoyDisplay').textContent = fechaHoy;

    // --- INICIO ---
    window.onload = function() {
        // Gestión Correlativo
        let lastId = localStorage.getItem('cundo_id_seq') || 50100;
        if(localStorage.getItem('cundo_last_session_date') !== fechaHoy) {
            // Lógica opcional: si cambia el día, o si se finaliza una cotización
            // Por ahora solo incrementa al cargar para simular
            // En producción deberías incrementar SOLO al guardar.
        }
        document.getElementById('correlativo').value = "CN" + lastId;
        
        agregarFila(); // Iniciar con una fila
    };

    // --- BASE DE DATOS LOCAL (SIMULADA) ---
    function getDB(key) { return JSON.parse(localStorage.getItem(key) || "[]"); }
    function setDB(key, data) { localStorage.setItem(key, JSON.stringify(data)); }

    // --- LÓGICA BUSCADOR CLIENTES ---
    function guardarCliente() {
        const c = {
            rut: document.getElementById('newCliRut').value,
            razon: document.getElementById('newCliRazon').value,
            giro: document.getElementById('newCliGiro').value,
            dir: document.getElementById('newCliDir').value,
            comuna: document.getElementById('newCliComuna').value,
            contacto: document.getElementById('newCliContacto').value,
            email: document.getElementById('newCliEmail').value
        };
        if(!c.razon) return alert("Falta Razón Social");
        
        const db = getDB('cundo_clientes');
        db.push(c);
        setDB('cundo_clientes', db);
        cerrarModal('modalCliente');
        alert("Cliente Guardado");
    }

    function buscarCliente(input) {
        const term = input.value.toLowerCase();
        const lista = document.getElementById('listaResultadosCliente');
        lista.innerHTML = '';
        
        if(term.length < 3) { lista.style.display = 'none'; return; }

        const db = getDB('cundo_clientes');
        const resultados = db.filter(c => c.razon.toLowerCase().includes(term) || c.rut.toLowerCase().includes(term));

        if(resultados.length > 0) {
            lista.style.display = 'block';
            resultados.forEach(c => {
                const item = document.createElement('div');
                item.innerHTML = `<strong>${c.razon}</strong> <small>(${c.rut})</small>`;
                item.onclick = () => seleccionarCliente(c);
                lista.appendChild(item);
            });
        } else {
            lista.style.display = 'none';
        }
    }

    function seleccionarCliente(c) {
        document.getElementById('buscadorCliente').value = c.razon; // Mostrar nombre en buscador
        document.getElementById('cliRazon').value = c.razon;
        document.getElementById('cliRut').value = c.rut;
        document.getElementById('cliGiro').value = c.giro;
        document.getElementById('cliDir').value = c.dir;
        document.getElementById('cliComuna').value = c.comuna;
        document.getElementById('cliContacto').value = c.contacto;
        document.getElementById('cliEmail').value = c.email;
        document.getElementById('listaResultadosCliente').style.display = 'none';
    }

    // --- LÓGICA BUSCADOR PRODUCTOS ---
    function guardarProducto() {
        const p = {
            cod: document.getElementById('newProdCod').value,
            desc: document.getElementById('newProdDesc').value,
            costo: parseFloat(document.getElementById('newProdCosto').value) || 0
        };
        if(!p.desc) return alert("Falta descripción");

        const db = getDB('cundo_productos');
        db.push(p);
        setDB('cundo_productos', db);
        cerrarModal('modalProducto');
        alert("Producto Guardado");
    }

    function buscarProducto(input) {
        const term = input.value.toLowerCase();
        const row = input.closest('tr');
        const lista = row.querySelector('.suggestions-list'); // Lista específica de esta fila
        lista.innerHTML = '';

        if(term.length < 3) { lista.style.display = 'none'; return; }

        const db = getDB('cundo_productos');
        const resultados = db.filter(p => p.desc.toLowerCase().includes(term) || p.cod.toLowerCase().includes(term));

        if(resultados.length > 0) {
            lista.style.display = 'block';
            resultados.forEach(p => {
                const item = document.createElement('div');
                item.innerHTML = `<strong>${p.cod}</strong>: ${p.desc}`;
                item.onclick = () => seleccionarProducto(p, row, lista);
                lista.appendChild(item);
            });
        } else {
            lista.style.display = 'none';
        }
    }

    function seleccionarProducto(p, row, lista) {
        lista.style.display = 'none';
        row.querySelector('.desc-input').value = p.desc;
        row.querySelector('.costo-input').value = p.costo;
        // Sugerir venta (ej: +40%)
        row.querySelector('.venta-input').value = Math.round(p.costo * 1.4); 
        calcularFila(row.querySelector('.costo-input'));
    }

    // --- TABLA Y CÁLCULOS ---
    function agregarFila() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style="text-align:center"><button onclick="eliminarFila(this)" style="color:red;border:none;background:none;cursor:pointer;">&times;</button></td>
            <td style="position:relative">
                <input type="text" class="row-input desc-input" placeholder="Buscar producto (3 letras)..." onkeyup="buscarProducto(this)">
                <div class="suggestions-list"></div>
            </td>
            <td><input type="number" class="row-input cant-input" value="1" min="1" oninput="calcularFila(this)" style="text-align:center"></td>
            
            <td class="td-costo"><input type="number" class="row-input costo-input" value="0" oninput="calcularFila(this)"></td>
            <td class="td-costo"><span class="total-costo-display">$0</span></td>
            
            <td class="td-venta" style="border-left: 2px solid #ddd;">
                <input type="number" class="row-input venta-input" value="0" oninput="calcularFila(this)" style="font-weight:bold; color:#27ae60;">
            </td>
            <td class="td-venta"><span class="total-venta-display">$0</span></td>
            <td class="td-venta"><span class="utilidad-row-display" style="font-size:10px;">$0</span></td>
        `;
        tbody.appendChild(tr);
    }

    function eliminarFila(btn) {
        if(document.querySelectorAll('#tablaItems tbody tr').length > 1) {
            btn.closest('tr').remove();
            calcularTotales();
        }
    }

    function calcularFila(input) {
        const row = input.closest('tr');
        const cant = parseFloat(row.querySelector('.cant-input').value) || 0;
        const costoUnit = parseFloat(row.querySelector('.costo-input').value) || 0;
        const ventaUnit = parseFloat(row.querySelector('.venta-input').value) || 0;

        const totalCosto = cant * costoUnit;
        const totalVenta = cant * ventaUnit;
        const utilidad = totalVenta - totalCosto;

        row.querySelector('.total-costo-display').textContent = formatter.format(totalCosto);
        row.querySelector('.total-venta-display').textContent = formatter.format(totalVenta);
        
        const utilSpan = row.querySelector('.utilidad-row-display');
        utilSpan.textContent = formatter.format(utilidad);
        utilSpan.style.color = utilidad >= 0 ? 'green' : 'red';

        calcularTotales();
    }

    function calcularTotales() {
        let neto = 0;
        let costoTotal = 0;

        document.querySelectorAll('#tablaItems tbody tr').forEach(row => {
            const cant = parseFloat(row.querySelector('.cant-input').value) || 0;
            const ventaUnit = parseFloat(row.querySelector('.venta-input').value) || 0;
            const costoUnit = parseFloat(row.querySelector('.costo-input').value) || 0;
            
            neto += (cant * ventaUnit);
            costoTotal += (cant * costoUnit);
        });

        const iva = neto * 0.19;
        const total = neto + iva;
        const utilidad = neto - costoTotal;
        const margen = neto > 0 ? (utilidad / neto) : 0;

        document.getElementById('txtNeto').textContent = formatter.format(neto);
        document.getElementById('txtIva').textContent = formatter.format(iva);
        document.getElementById('txtTotal').textContent = formatter.format(total);
        document.getElementById('txtUtilidad').textContent = formatter.format(utilidad);
        document.getElementById('txtMargen').textContent = (margen * 100).toFixed(1) + "%";
    }

    // --- MODALES UI ---
    function abrirModal(id) { document.getElementById(id).style.display = 'block'; }
    function cerrarModal(id) { document.getElementById(id).style.display = 'none'; }
    // Cerrar al hacer click fuera del modal
    window.onclick = function(e) { if(e.target.classList.contains('modal')) e.target.style.display = 'none'; }


    // ==========================================
    // === GENERACIÓN DE PDF PROFESIONAL (V3) ===
    // ==========================================
    async function generarPDFProfesional() {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();

        // COLORES
        const primary = [31, 111, 139];   // #1F6F8B
        const secondary = [242, 92, 5];   // #F25C05
        const lightGray = [245, 245, 245];

        // 1. BARRA LATERAL IZQUIERDA (Estilo moderno)
        doc.setFillColor(...primary);
        doc.rect(0, 0, 15, 297, 'F'); // Franja azul vertical completa

        // 2. HEADER
        doc.setFont("helvetica", "bold");
        doc.setFontSize(26);
        doc.setTextColor(...primary);
        doc.text("CUNDO SPA", 25, 25);
        
        doc.setFontSize(10);
        doc.setTextColor(100);
        doc.setFont("helvetica", "normal");
        doc.text("Servicios de Ingeniería y Soluciones", 25, 32);
        doc.text("RUT: 77.XXX.XXX-X", 25, 37);
        doc.text("Santiago, Chile", 25, 42);
        doc.text("Web: www.cundospa.cl", 25, 47);

        // CAJA DE DATOS DE COTIZACIÓN (Derecha)
        doc.setFillColor(250, 250, 250);
        doc.rect(130, 15, 70, 35, 'F');
        doc.setDrawColor(220);
        doc.rect(130, 15, 70, 35, 'S');

        doc.setTextColor(...primary);
        doc.setFontSize(14);
        doc.setFont("helvetica", "bold");
        doc.text("COTIZACIÓN", 165, 25, { align: "center" });
        
        doc.setTextColor(0);
        doc.setFontSize(12);
        doc.text(document.getElementById('correlativo').value, 165, 35, { align: "center" });
        
        doc.setFontSize(9);
        doc.setFont("helvetica", "normal");
        doc.text("Fecha: " + document.getElementById('fechaHoyDisplay').textContent, 165, 43, { align: "center" });

        // 3. INFORMACIÓN DEL CLIENTE (Recuadro elegante)
        const startYInfo = 60;
        doc.setFillColor(...lightGray);
        doc.rect(25, startYInfo, 175, 35, 'F'); // Fondo gris suave
        doc.setDrawColor(...primary);
        doc.setLineWidth(0.5);
        doc.line(25, startYInfo, 25, startYInfo + 35); // Línea vertical azul decorativa

        doc.setFontSize(9);
        doc.setTextColor(...primary);
        doc.setFont("helvetica", "bold");
        doc.text("CLIENTE:", 30, startYInfo + 8);
        
        doc.setTextColor(0);
        doc.setFont("helvetica", "bold");
        doc.setFontSize(11);
        doc.text(document.getElementById('cliRazon').value || "Cliente Mostrador", 30, startYInfo + 15);
        
        doc.setFont("helvetica", "normal");
        doc.setFontSize(9);
        doc.text("RUT: " + document.getElementById('cliRut').value, 30, startYInfo + 22);
        doc.text("Dirección: " + document.getElementById('cliDir').value + ", " + document.getElementById('cliComuna').value, 30, startYInfo + 27);
        
        doc.text("Atención: " + document.getElementById('cliContacto').value, 120, startYInfo + 15);
        doc.text("Email: " + document.getElementById('cliEmail').value, 120, startYInfo + 22);

        // 4. TABLA DE ITEMS
        const items = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.desc-input').value;
            const cant = tr.querySelector('.cant-input').value;
            const precioUnit = tr.querySelector('.venta-input').value;
            const total = parseFloat(cant) * parseFloat(precioUnit);

            if(desc.trim()) {
                items.push([
                    cant, 
                    desc, 
                    formatter.format(precioUnit), 
                    formatter.format(total)
                ]);
            }
        });

        doc.autoTable({
            startY: 105,
            head: [['CANT.', 'DESCRIPCIÓN', 'P. UNITARIO', 'TOTAL']],
            body: items,
            theme: 'plain', // Tema limpio para personalizar
            margin: { left: 25, right: 10 },
            headStyles: { 
                fillColor: primary, 
                textColor: 255, 
                fontStyle: 'bold', 
                halign: 'center',
                minCellHeight: 12
            },
            bodyStyles: { 
                cellPadding: 4, 
                fontSize: 9,
                lineColor: 230,
                lineWidth: { bottom: 0.1 } // Líneas sutiles entre filas
            },
            columnStyles: {
                0: { halign: 'center', cellWidth: 20 },
                2: { halign: 'right', cellWidth: 35 },
                3: { halign: 'right', cellWidth: 35, fontStyle: 'bold' }
            },
            // Color alternado sutil
            didParseCell: function (data) {
                if (data.section === 'body' && data.row.index % 2 === 0) {
                    data.cell.styles.fillColor = [252, 252, 252];
                }
            }
        });

        // 5. SECCIÓN DE TOTALES
        const finalY = doc.lastAutoTable.finalY + 10;
        
        // Cuadro de totales a la derecha
        const boxWidth = 70;
        const boxX = 130;
        
        // Fondo Totales
        doc.setFillColor(245, 245, 245);
        doc.rect(boxX, finalY, boxWidth, 30, 'F');

        doc.setFontSize(10);
        doc.setTextColor(0);
        
        // Neto
        doc.text("Neto:", boxX + 5, finalY + 8);
        doc.text(document.getElementById('txtNeto').textContent, boxX + 65, finalY + 8, { align: 'right' });
        
        // IVA
        doc.text("IVA (19%):", boxX + 5, finalY + 16);
        doc.text(document.getElementById('txtIva').textContent, boxX + 65, finalY + 16, { align: 'right' });

        // Total Final
        doc.setFillColor(...primary); // Franja azul para total
        doc.rect(boxX, finalY + 22, boxWidth, 12, 'F');
        doc.setTextColor(255);
        doc.setFont("helvetica", "bold");
        doc.setFontSize(12);
        doc.text("TOTAL:", boxX + 5, finalY + 30);
        doc.text(document.getElementById('txtTotal').textContent, boxX + 65, finalY + 30, { align: 'right' });


        // 6. PIE DE PÁGINA Y LEGAL (Al final de la página)
        const pageHeight = doc.internal.pageSize.height;
        const footerY = pageHeight - 50;

        doc.setTextColor(0);
        doc.setFontSize(9);
        doc.setFont("helvetica", "bold");
        doc.text("CONDICIONES COMERCIALES:", 25, footerY);
        
        doc.setFont("helvetica", "normal");
        doc.setFontSize(8);
        doc.text("1. Validez de la oferta: 15 días hábiles.", 25, footerY + 5);
        doc.text("2. Forma de pago: 50% anticipo, 50% contra entrega.", 25, footerY + 9);
        doc.text("3. Tiempos de entrega a convenir según disponibilidad.", 25, footerY + 13);

        // Datos Bancarios (Placeholder)
        doc.setFont("helvetica", "bold");
        doc.text("DATOS PARA TRANSFERENCIA:", 110, footerY);
        doc.setFont("helvetica", "normal");
        doc.text("Banco: Banco de Chile", 110, footerY + 5);
        doc.text("Cuenta Cte: 00-123-45678-00", 110, footerY + 9);
        doc.text("Titular: CUNDO SPA", 110, footerY + 13);
        doc.text("Email: finanzas@cundospa.cl", 110, footerY + 17);

        // Línea naranja final
        doc.setDrawColor(...secondary);
        doc.setLineWidth(2);
        doc.line(25, pageHeight - 15, 195, pageHeight - 15);
        
        doc.setFontSize(7);
        doc.setTextColor(150);
        doc.text("Documento generado electrónicamente por Sistema PRO CUNDO SPA", 110, pageHeight - 10, { align: "center" });

        // Incrementar correlativo después de generar (opcional)
        // localStorage.setItem('cundo_id_seq', parseInt(document.getElementById('correlativo').value.replace('CN','')) + 1);

        doc.save(`Cotizacion_${document.getElementById('correlativo').value}.pdf`);
    }
</script>

</body>
</html>
