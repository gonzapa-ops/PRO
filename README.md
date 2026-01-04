<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema CUNDO SPA - Gestión PRO</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS GENERALES --- */
        :root {
            --primary: #1F6F8B;
            --secondary: #F25C05;
            --bg: #F5F5F5;
            --text: #333;
            --success: #28a745;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
        body { background: var(--bg); padding: 20px; color: var(--text); }
        
        .container { max-width: 1300px; margin: 0 auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }

        /* HEADER */
        .header { display: flex; justify-content: space-between; align-items: center; border-bottom: 4px solid var(--secondary); padding-bottom: 15px; margin-bottom: 20px; }
        .brand h1 { color: var(--primary); margin: 0; font-size: 28px; }
        .brand p { color: #666; font-size: 14px; }
        
        .quote-info { text-align: right; }
        .quote-info input { text-align: right; font-weight: bold; border: 1px solid #ccc; padding: 5px; border-radius: 4px; background: #eee; cursor: not-allowed; }

        /* SECCIONES */
        .section-header { background: var(--primary); color: white; padding: 8px 15px; font-weight: bold; margin: 20px 0 10px 0; border-radius: 4px; display: flex; justify-content: space-between; align-items: center; }
        .btn-small { background: var(--secondary); border: none; color: white; padding: 4px 10px; border-radius: 3px; cursor: pointer; font-size: 12px; }
        
        /* FORMULARIOS */
        .grid-4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
        .form-group label { display: block; font-size: 11px; font-weight: bold; color: var(--primary); margin-bottom: 4px; }
        .form-group input, .form-group select { width: 100%; padding: 6px; border: 1px solid #ddd; border-radius: 4px; font-size: 12px; }
        .form-group input[readonly] { background-color: #f9f9f9; color: #555; }

        /* TABLA COMPLEJA */
        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; font-size: 11px; min-width: 1000px; }
        th { background: var(--primary); color: white; padding: 8px; text-align: center; white-space: nowrap; }
        td { border: 1px solid #ddd; padding: 5px; }
        
        /* Celdas especiales */
        .col-costo { background-color: #fff3cd; } /* Amarillo suave para costos internos */
        .col-venta { background-color: #d4edda; } /* Verde suave para ventas */
        
        input.cell-input { width: 100%; border: none; padding: 4px; text-align: right; font-family: inherit; font-size: 11px; }
        input.cell-input:focus { outline: 2px solid var(--secondary); background: white; }

        /* MODALES */
        .modal { display: none; position: fixed; z-index: 100; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); }
        .modal-content { background-color: white; margin: 10% auto; padding: 20px; border: 1px solid #888; width: 50%; border-radius: 8px; box-shadow: 0 4px 20px rgba(0,0,0,0.2); }
        .close { float: right; font-size: 28px; font-weight: bold; cursor: pointer; }
        
        .btn-action { background: var(--primary); color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer; margin-top: 10px; width: 100%; }
        .btn-action:hover { background: #154a5c; }

        /* TOTALES */
        .totals-area { display: flex; justify-content: flex-end; margin-top: 20px; }
        .totals-table td { padding: 5px 15px; text-align: right; font-weight: bold; font-size: 13px; }
        .big-total { font-size: 18px; color: var(--primary); border-top: 2px solid var(--secondary); }

        /* BOTONES FINALES */
        .actions { text-align: center; margin-top: 30px; border-top: 1px solid #ccc; padding-top: 20px; }
        .btn-pdf { background: var(--secondary); color: white; padding: 12px 30px; font-size: 16px; border: none; border-radius: 5px; cursor: pointer; display: inline-flex; align-items: center; gap: 10px; }
        .btn-pdf:hover { background: #d14e04; }

    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div class="brand">
            <h1>CUNDO SPA</h1>
            <p>Sistema de Gestión Profesional</p>
        </div>
        <div class="quote-info">
            <div class="form-group">
                <label>N° COTIZACIÓN (Auto)</label>
                <input type="text" id="correlativo" readonly>
            </div>
            <div class="form-group" style="margin-top: 5px;">
                <label>FECHA (Hoy)</label>
                <input type="date" id="fechaHoy" readonly>
            </div>
        </div>
    </div>

    <div class="section-header">
        <span>INFORMACIÓN DEL CLIENTE</span>
        <button class="btn-small" onclick="abrirModal('modalCliente')">+ Nuevo Cliente</button>
    </div>
    
    <div style="margin-bottom: 10px;">
        <select id="selectCliente" onchange="cargarCliente()" style="width: 100%; padding: 8px; border: 1px solid var(--primary); border-radius: 4px;">
            <option value="">-- Seleccione o busque un Cliente Guardado --</option>
        </select>
    </div>

    <div class="grid-4">
        <div class="form-group"><label>RUT</label><input type="text" id="cliRut" readonly></div>
        <div class="form-group"><label>Razón Social</label><input type="text" id="cliRazon" readonly></div>
        <div class="form-group"><label>Giro</label><input type="text" id="cliGiro" readonly></div>
        <div class="form-group"><label>Dirección</label><input type="text" id="cliDir" readonly></div>
        <div class="form-group"><label>Comuna</label><input type="text" id="cliComuna" readonly></div>
        <div class="form-group"><label>Región</label><input type="text" id="cliRegion" readonly></div>
        <div class="form-group"><label>Contacto</label><input type="text" id="cliContacto" readonly></div>
        <div class="form-group"><label>Email</label><input type="text" id="cliEmail" readonly></div>
    </div>

    <div class="section-header">
        <span>DETALLE DE SERVICIOS / PRODUCTOS</span>
        <button class="btn-small" onclick="abrirModal('modalProducto')">+ Crear Producto Base</button>
    </div>

    <div class="table-container">
        <table id="tablaItems">
            <thead>
                <tr>
                    <th rowspan="2" style="width: 30px;">Acción</th>
                    <th rowspan="2" style="width: 80px;">Cod. Prov.</th> <th rowspan="2" style="width: 200px;">Descripción</th>
                    <th rowspan="2" style="width: 50px;">Cant.</th>
                    
                    <th colspan="3" class="col-costo">COSTOS (Interno)</th>
                    
                    <th colspan="4" class="col-venta">VENTA (Cliente)</th>
                </tr>
                <tr>
                    <th class="col-costo">Neto Unit.</th>
                    <th class="col-costo">IVA Costo</th> <th class="col-costo">Total Costo</th> <th class="col-venta" style="background: #28a745; color: white;">TOTAL VENTA</th> <th class="col-venta">Neto Venta</th>
                    <th class="col-venta">Utilidad $</th>
                    <th class="col-venta">% Util.</th>
                </tr>
            </thead>
            <tbody>
                </tbody>
        </table>
    </div>
    
    <button onclick="agregarFila()" style="margin-top: 10px; background: #333; color: white; padding: 5px 10px; border:none; border-radius:3px;">+ Agregar Línea Manual</button>

    <div class="totals-area">
        <table class="totals-table">
            <tr>
                <td>Total Neto Venta:</td>
                <td id="totalNetoVenta">$0</td>
            </tr>
            <tr>
                <td>IVA (19%):</td>
                <td id="totalIvaVenta">$0</td>
            </tr>
            <tr>
                <td class="big-total">TOTAL A PAGAR:</td>
                <td class="big-total" id="totalFinalVenta">$0</td>
            </tr>
            <tr>
                <td style="color: green; font-size: 11px; padding-top: 10px;">Utilidad Total Proyecto:</td>
                <td style="color: green; font-size: 11px; padding-top: 10px;" id="totalUtilidadProyecto">$0</td>
            </tr>
        </table>
    </div>

    <div class="actions">
        <button class="btn-pdf" onclick="generarPDF()">
            <span>📄</span> DESCARGAR PDF CLIENTE
        </button>
        <p style="margin-top: 10px; font-size: 12px; color: #777;">Nota: El PDF solo mostrará los precios de venta, ocultando costos y márgenes.</p>
    </div>

</div>

<div id="modalCliente" class="modal">
    <div class="modal-content">
        <span class="close" onclick="cerrarModal('modalCliente')">&times;</span>
        <h2 style="color: var(--primary); margin-bottom: 15px;">Crear Nuevo Cliente</h2>
        <div class="grid-4" style="grid-template-columns: 1fr 1fr;">
            <div class="form-group"><label>RUT *</label><input type="text" id="newCliRut"></div>
            <div class="form-group"><label>Razón Social *</label><input type="text" id="newCliRazon"></div>
            <div class="form-group"><label>Giro</label><input type="text" id="newCliGiro"></div>
            <div class="form-group"><label>Dirección</label><input type="text" id="newCliDir"></div>
            <div class="form-group"><label>Comuna</label><input type="text" id="newCliComuna"></div>
            <div class="form-group"><label>Región</label><input type="text" id="newCliRegion"></div>
            <div class="form-group"><label>Nombre Contacto</label><input type="text" id="newCliContacto"></div>
            <div class="form-group"><label>Celular</label><input type="text" id="newCliCel"></div>
            <div class="form-group"><label>Email</label><input type="text" id="newCliEmail"></div>
        </div>
        <button class="btn-action" onclick="guardarCliente()">Guardar Cliente</button>
    </div>
</div>

<div id="modalProducto" class="modal">
    <div class="modal-content">
        <span class="close" onclick="cerrarModal('modalProducto')">&times;</span>
        <h2 style="color: var(--primary); margin-bottom: 15px;">Crear Producto en Base de Datos</h2>
        <div class="grid-4" style="grid-template-columns: 1fr 1fr;">
            <div class="form-group"><label>Código Interno (Ej: PRO23A)</label><input type="text" id="newProdCod"></div>
            <div class="form-group"><label>Código Proveedor</label><input type="text" id="newProdProv"></div>
            <div class="form-group" style="grid-column: span 2;"><label>Descripción</label><input type="text" id="newProdDesc"></div>
            <div class="form-group"><label>Costo Neto ($)</label><input type="number" id="newProdCosto" oninput="calcProdModal()"></div>
            <div class="form-group"><label>IVA Costo (Auto)</label><input type="text" id="newProdIva" readonly></div>
            <div class="form-group"><label>Total Costo (Auto)</label><input type="text" id="newProdTotal" readonly></div>
        </div>
        <button class="btn-action" onclick="guardarProducto()">Guardar Producto</button>
    </div>
</div>

<script>
    // --- LÓGICA DEL SISTEMA ---
    
    const formatter = new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP', minimumFractionDigits: 0 });
    const percentFormatter = new Intl.NumberFormat('es-CL', { style: 'percent', minimumFractionDigits: 1 });

    // 1. INICIALIZACIÓN
    window.onload = function() {
        // Fecha
        document.getElementById('fechaHoy').valueAsDate = new Date();
        
        // Correlativo
        let lastId = localStorage.getItem('cundo_last_id');
        if(!lastId) {
            lastId = 50100; // Inicio si no existe
        } else {
            lastId = parseInt(lastId) + 1; // Incrementa
        }
        localStorage.setItem('cundo_last_id', lastId); // Guardar nuevo
        document.getElementById('correlativo').value = "CN" + lastId;

        // Cargar Listas
        actualizarSelectClientes();
        agregarFila(); // Una fila vacía al inicio
    };

    // --- GESTIÓN DE CLIENTES ---
    function guardarCliente() {
        const cliente = {
            rut: document.getElementById('newCliRut').value,
            razon: document.getElementById('newCliRazon').value,
            giro: document.getElementById('newCliGiro').value,
            dir: document.getElementById('newCliDir').value,
            comuna: document.getElementById('newCliComuna').value,
            region: document.getElementById('newCliRegion').value,
            contacto: document.getElementById('newCliContacto').value,
            cel: document.getElementById('newCliCel').value,
            email: document.getElementById('newCliEmail').value
        };

        if(!cliente.rut || !cliente.razon) { alert("Rut y Razón Social son obligatorios"); return; }

        let db = JSON.parse(localStorage.getItem('cundo_clients') || "[]");
        db.push(cliente);
        localStorage.setItem('cundo_clients', JSON.stringify(db));
        
        cerrarModal('modalCliente');
        actualizarSelectClientes();
        alert("Cliente guardado con éxito");
    }

    function actualizarSelectClientes() {
        const db = JSON.parse(localStorage.getItem('cundo_clients') || "[]");
        const select = document.getElementById('selectCliente');
        select.innerHTML = '<option value="">-- Seleccione Cliente --</option>';
        db.forEach((c, index) => {
            const opt = document.createElement('option');
            opt.value = index;
            opt.text = `${c.rut} - ${c.razon}`;
            select.appendChild(opt);
        });
    }

    function cargarCliente() {
        const idx = document.getElementById('selectCliente').value;
        if(idx === "") return;
        
        const db = JSON.parse(localStorage.getItem('cundo_clients') || "[]");
        const c = db[idx];
        
        document.getElementById('cliRut').value = c.rut;
        document.getElementById('cliRazon').value = c.razon;
        document.getElementById('cliGiro').value = c.giro;
        document.getElementById('cliDir').value = c.dir;
        document.getElementById('cliComuna').value = c.comuna;
        document.getElementById('cliRegion').value = c.region;
        document.getElementById('cliContacto').value = c.contacto;
        document.getElementById('cliEmail').value = c.email;
    }

    // --- GESTIÓN DE PRODUCTOS ---
    function calcProdModal() {
        const neto = parseFloat(document.getElementById('newProdCosto').value) || 0;
        document.getElementById('newProdIva').value = formatter.format(neto * 0.19);
        document.getElementById('newProdTotal').value = formatter.format(neto * 1.19);
    }

    function guardarProducto() {
        const prod = {
            cod: document.getElementById('newProdCod').value,
            prov: document.getElementById('newProdProv').value,
            desc: document.getElementById('newProdDesc').value,
            costo: parseFloat(document.getElementById('newProdCosto').value) || 0
        };

        if(!prod.desc) { alert("Descripción es obligatoria"); return; }

        let db = JSON.parse(localStorage.getItem('cundo_products') || "[]");
        db.push(prod);
        localStorage.setItem('cundo_products', JSON.stringify(db));

        cerrarModal('modalProducto');
        alert("Producto guardado. Ahora puede buscarlo en la tabla.");
    }

    // --- TABLA Y CÁLCULOS ---
    function agregarFila() {
        const tbody = document.querySelector('#tablaItems tbody');
        const tr = document.createElement('tr');
        
        // Crear Select de productos para la fila
        let productosOpts = '<option value="">- Buscar Prod -</option>';
        const dbProds = JSON.parse(localStorage.getItem('cundo_products') || "[]");
        dbProds.forEach((p, idx) => {
            productosOpts += `<option value="${idx}">${p.cod} - ${p.desc}</option>`;
        });

        tr.innerHTML = `
            <td><button onclick="this.closest('tr').remove(); calcularTotales();" style="color:red;">X</button></td>
            <td><input type="text" class="cell-input cod-prov" readonly></td>
            <td style="position:relative">
                <select class="cell-input" onchange="cargarProductoEnFila(this)" style="margin-bottom:5px;">${productosOpts}</select>
                <input type="text" class="cell-input descripcion" placeholder="Edite descripción si desea">
            </td>
            <td><input type="number" class="cell-input cantidad" value="1" min="1" oninput="calcularFila(this)"></td>
            
            <td class="col-costo"><input type="number" class="cell-input costo-neto" value="0" oninput="calcularFila(this)"></td>
            <td class="col-costo"><span class="iva-costo">$0</span></td>
            <td class="col-costo"><span class="total-costo">$0</span></td>
            
            <td class="col-venta" style="border: 2px solid #28a745;">
                <input type="number" class="cell-input total-venta-bruta" value="0" oninput="calcularFila(this)" placeholder="Total Venta con IVA">
            </td>
            <td class="col-venta"><span class="neto-venta">$0</span></td>
            <td class="col-venta"><span class="utilidad-pesos" style="font-weight:bold;">$0</span></td>
            <td class="col-venta"><span class="utilidad-porc">$0</span></td>
        `;
        tbody.appendChild(tr);
    }

    function cargarProductoEnFila(select) {
        const idx = select.value;
        if(idx === "") return;
        
        const db = JSON.parse(localStorage.getItem('cundo_products') || "[]");
        const p = db[idx];
        const row = select.closest('tr');

        row.querySelector('.cod-prov').value = p.prov;
        row.querySelector('.descripcion').value = p.desc;
        row.querySelector('.costo-neto').value = p.costo;
        
        // Asignar un precio de venta sugerido (ej: margen 30%) para no dejar en 0
        const precioSugerido = Math.round((p.costo * 1.30) * 1.19);
        row.querySelector('.total-venta-bruta').value = precioSugerido;

        calcularFila(select);
    }

    function calcularFila(element) {
        const row = element.closest('tr');
        
        // Inputs
        const qty = parseFloat(row.querySelector('.cantidad').value) || 1;
        const costoNetoUnit = parseFloat(row.querySelector('.costo-neto').value) || 0;
        const ventaTotalBruta = parseFloat(row.querySelector('.total-venta-bruta').value) || 0; // Total ingresado por usuario

        // Cálculos COSTO
        const costoTotalNeto = costoNetoUnit * qty;
        const ivaCosto = costoTotalNeto * 0.19;
        const totalCosto = costoTotalNeto * 1.19;

        // Cálculos VENTA
        // Si el usuario pone el TOTAL VENTA (Bruto de la línea), desglosamos hacia atrás
        const ventaTotalNeto = ventaTotalBruta / 1.19; 
        
        // Utilidad
        const utilidad = ventaTotalNeto - costoTotalNeto;
        let margen = 0;
        if(ventaTotalNeto > 0) {
            margen = utilidad / ventaTotalNeto; // Margen sobre venta
        }

        // Renderizar en fila
        row.querySelector('.iva-costo').innerText = formatter.format(ivaCosto);
        row.querySelector('.total-costo').innerText = formatter.format(totalCosto);
        
        row.querySelector('.neto-venta').innerText = formatter.format(ventaTotalNeto);
        row.querySelector('.utilidad-pesos').innerText = formatter.format(utilidad);
        row.querySelector('.utilidad-porc').innerText = percentFormatter.format(margen);
        
        // Colorear utilidad
        row.querySelector('.utilidad-pesos').style.color = utilidad >= 0 ? 'green' : 'red';

        calcularTotales();
    }

    function calcularTotales() {
        let sumaNetoVenta = 0;
        let sumaUtilidad = 0;

        document.querySelectorAll('#tablaItems tbody tr').forEach(row => {
            const bruto = parseFloat(row.querySelector('.total-venta-bruta').value) || 0;
            const neto = bruto / 1.19;
            
            // Re-calcular utilidad individual para sumar
            const qty = parseFloat(row.querySelector('.cantidad').value) || 1;
            const costoU = parseFloat(row.querySelector('.costo-neto').value) || 0;
            const costoTotal = costoU * qty;
            
            sumaNetoVenta += neto;
            sumaUtilidad += (neto - costoTotal);
        });

        const ivaVenta = sumaNetoVenta * 0.19;
        const totalFinal = sumaNetoVenta + ivaVenta;

        document.getElementById('totalNetoVenta').innerText = formatter.format(sumaNetoVenta);
        document.getElementById('totalIvaVenta').innerText = formatter.format(ivaVenta);
        document.getElementById('totalFinalVenta').innerText = formatter.format(totalFinal);
        document.getElementById('totalUtilidadProyecto').innerText = formatter.format(sumaUtilidad);
    }

    // --- MODALES ---
    function abrirModal(id) { document.getElementById(id).style.display = "block"; }
    function cerrarModal(id) { document.getElementById(id).style.display = "none"; }
    window.onclick = function(event) { if (event.target.classList.contains('modal')) { event.target.style.display = "none"; } }

    // --- PDF ---
    async function generarPDF() {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();
        
        const azul = [31, 111, 139]; 
        const naranja = [242, 92, 5]; 

        // Header Azul
        doc.setFillColor(...azul);
        doc.rect(0, 0, 210, 35, 'F');
        doc.setTextColor(255, 255, 255);
        doc.setFontSize(22);
        doc.text("CUNDO SPA", 15, 20);
        doc.setFontSize(10);
        doc.text("Soluciones Integrales", 15, 27);

        // Info Cotización
        const nCot = document.getElementById('correlativo').value;
        doc.setTextColor(0,0,0);
        doc.setFontSize(14);
        doc.text("COTIZACIÓN N°: " + nCot, 140, 20);
        doc.setFontSize(10);
        doc.text("Fecha: " + document.getElementById('fechaHoy').value, 140, 27);

        // Cliente
        doc.text("CLIENTE:", 15, 45);
        doc.setFont("helvetica", "bold");
        doc.text(document.getElementById('cliRazon').value || "", 15, 50);
        doc.setFont("helvetica", "normal");
        doc.text("RUT: " + document.getElementById('cliRut').value, 15, 55);
        doc.text("Dirección: " + document.getElementById('cliDir').value, 15, 60);
        doc.text("Atención: " + document.getElementById('cliContacto').value, 15, 65);

        // Tabla (SOLO VENTA, SIN COSTOS NI UTILIDAD)
        const rows = [];
        document.querySelectorAll('#tablaItems tbody tr').forEach(tr => {
            const desc = tr.querySelector('.descripcion').value;
            const cant = tr.querySelector('.cantidad').value;
            
            // Calculamos Unitario Neto de Venta para mostrar al cliente
            const totalBruto = parseFloat(tr.querySelector('.total-venta-bruta').value) || 0;
            const totalNeto = totalBruto / 1.19;
            const unitarioNeto = totalNeto / (parseFloat(cant) || 1);
            
            if(desc) {
                rows.push([
                    cant,
                    desc,
                    formatter.format(unitarioNeto),
                    formatter.format(totalNeto)
                ]);
            }
        });

        doc.autoTable({
            startY: 75,
            head: [['Cant.', 'Descripción', 'Precio Unit. Neto', 'Total Neto']],
            body: rows,
            theme: 'grid',
            headStyles: { fillColor: azul, textColor: 255 },
            columnStyles: {
                0: { halign: 'center' },
                2: { halign: 'right' },
                3: { halign: 'right' }
            }
        });

        // Totales
        const finalY = doc.lastAutoTable.finalY + 10;
        const neto = document.getElementById('totalNetoVenta').innerText;
        const iva = document.getElementById('totalIvaVenta').innerText;
        const total = document.getElementById('totalFinalVenta').innerText;

        doc.text("Neto:", 150, finalY);
        doc.text(neto, 195, finalY, { align: 'right' });
        
        doc.text("IVA 19%:", 150, finalY + 7);
        doc.text(iva, 195, finalY + 7, { align: 'right' });
        
        doc.setFontSize(12);
        doc.setTextColor(...azul);
        doc.setFont("helvetica", "bold");
        doc.text("TOTAL:", 150, finalY + 15);
        doc.text(total, 195, finalY + 15, { align: 'right' });

        doc.save(`Cotizacion_${nCot}.pdf`);
    }

</script>

</body>
</html>