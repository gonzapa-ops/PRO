<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotizador - CUNDO SPA</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        /* --- ESTILOS ORIGINALES MEJORADOS --- */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #F5F5F5; padding: 20px; color: #3B3B3B; }
        
        .cotizador-container { max-width: 1200px; margin: 0 auto; background: #fff; box-shadow: 0 4px 12px rgba(0,0,0,0.1); padding: 20px; border-radius: 8px; }
        
        /* Header */
        .cotizador-header { background: #1F6F8B; color: white; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 4px solid #F25C05; border-radius: 6px 6px 0 0; margin-bottom: 20px; }
        .empresa-nombre { font-weight: 700; font-size: 24px; letter-spacing: 1px; text-transform: uppercase; }
        .datos-cotizacion { text-align: right; }
        .datos-cotizacion input { background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.3); color: white; padding: 4px; border-radius: 4px; text-align: right; font-weight: bold; }
        
        /* Secciones */
        .seccion-titulo { font-weight: 700; font-size: 16px; margin-bottom: 15px; padding-bottom: 5px; border-bottom: 2px solid #F25C05; text-transform: uppercase; color: #1F6F8B; margin-top: 10px; }
        
        /* Formularios */
        .fila-campos { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px; }
        .fila-campos-cuatro { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 15px; }
        
        .campo-grupo label { display: block; font-weight: 700; color: #3B3B3B; margin-bottom: 5px; font-size: 11px; text-transform: uppercase; }
        .campo-grupo input { width: 100%; padding: 8px; font-size: 13px; border: 1px solid #ddd; border-radius: 4px; font-family: inherit; }
        .campo-grupo input:focus { border-color: #F25C05; outline: none; }

        /* Tabla */
        table { width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 13px; }
        th { background: #1F6F8B; color: white; padding: 10px; text-transform: uppercase; font-weight: 700; text-align: left; }
        td { border: 1px solid #ddd; padding: 8px; }
        tr:nth-child(even) { background: #E9F0EA; }
        
        .input-tabla { width: 100%; border: none; background: transparent; padding: 4px; font-family: inherit; }
        .input-tabla:focus { outline: 2px solid #F25C05; background: #fff; }

        /* Totales */
        .totales-container { display: flex; justify-content: flex-end; margin-top: 20px; }
        .tabla-totales { width: 300px; border: none; }
        .tabla-totales td { border: none; padding: 5px; text-align: right; font-size: 14px; }
        .total-final { font-size: 18px; font-weight: 900; color: #1F6F8B; border-top: 2px solid #F25C05 !important; padding-top: 10px !important; }

        /* Botones */
        .botones-accion { display: flex; gap: 10px; margin-top: 20px; justify-content: center; border-top: 1px solid #eee; padding-top: 20px; }
        button { cursor: pointer; border: none; border-radius: 4px; font-weight: 700; text-transform: uppercase; padding: 10px 20px; font-size: 12px; transition: all 0.3s; }
        
        .btn-agregar { background: #385525; color: white; }
        .btn-agregar:hover { background: #274015; }
        
        .btn-pdf { background: #F25C05; color: white; font-size: 14px; padding: 12px 25px; display: flex; align-items: center; gap: 8px; }
        .btn-pdf:hover { background: #cb4a04; }
        
        .btn-eliminar { background: #9B2E00; color: white; padding: 5px 10px; font-size: 10px; }

        /* Responsive */
        @media (max-width: 768px) {
            .fila-campos, .fila-campos-cuatro { grid-template-columns: 1fr; }
            .cotizador-header { flex-direction: column; text-align: center; gap: 10px; }
            .datos-cotizacion { text-align: center; }
        }
    </style>
</head>
<body>

<div class="cotizador-container">
    <div class="cotizador-header">
        <div class="empresa-info">
            <div class="empresa-nombre">CUNDO SPA</div>
            <div style="font-size: 12px; opacity: 0.9;">Soluciones Integrales y Servicios</div>
            <div style="font-size: 11px; opacity: 0.8;">contacto@cundospa.cl</div>
        </div>
        <div class="datos-cotizacion">
            <div style="margin-bottom: 5px;">
                <label style="font-size: 10px; font-weight: bold;">N° COTIZACIÓN</label>
                <input type="text" id="numCotizacion" value="2024-001" style="width: 100px;">
            </div>
            <div>
                <label style="font-size: 10px; font-weight: bold;">FECHA</label>
                <input type="date" id="fechaEmision" style="width: 120px; color:black; background:white;">
            </div>
        </div>
    </div>

    <div class="seccion-titulo">Información del Cliente</div>
    <div class="fila-campos-cuatro">
        <div class="campo-grupo">
            <label>Razón Social / Nombre</label>
            <input type="text" id="clienteNombre" placeholder="Nombre del cliente">
        </div>
        <div class="campo-grupo">
            <label>RUT</label>
            <input type="text" id="clienteRut" placeholder="77.123.456-K">
        </div>
        <div class="campo-grupo">
            <label>Teléfono</label>
            <input type="text" id="clienteFono" placeholder="+56 9...">
        </div>
        <div class="campo-grupo">
            <label>Dirección</label>
            <input type="text" id="clienteDireccion" placeholder="Dirección comercial">
        </div>
    </div>

    <div class="seccion-titulo">Detalle de Servicios / Productos</div>
    <table id="tablaProductos">
        <thead>
            <tr>
                <th style="width: 50%;">Descripción</th>
                <th style="width: 10%; text-align: center;">Cant.</th>
                <th style="width: 15%; text-align: right;">Precio Unit.</th>
                <th style="width: 15%; text-align: right;">Total</th>
                <th style="width: 10%; text-align: center;">Acción</th>
            </tr>
        </thead>
        <tbody>
            </tbody>
    </table>

    <div style="margin-bottom: 20px;">
        <button class="btn-agregar" onclick="agregarFila()">+ Agregar Item</button>
    </div>

    <div class="totales-container">
        <table class="tabla-totales">
            <tr>
                <td><strong>NETO:</strong></td>
                <td id="lblNeto">$0</td>
            </tr>
            <tr>
                <td><strong>IVA (19%):</strong></td>
                <td id="lblIva">$0</td>
            </tr>
            <tr>
                <td class="total-final">TOTAL:</td>
                <td class="total-final" id="lblTotal">$0</td>
            </tr>
        </table>
    </div>

    <div class="botones-accion">
        <button class="btn-pdf" onclick="generarPDF()">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                <polyline points="14 2 14 8 20 8"></polyline>
                <line x1="16" y1="13" x2="8" y2="13"></line>
                <line x1="16" y1="17" x2="8" y2="17"></line>
                <polyline points="10 9 9 9 8 9"></polyline>
            </svg>
            Descargar PDF Oficial
        </button>
    </div>
</div>

<script>
    // --- LÓGICA DEL SISTEMA ---
    
    // 1. Inicialización
    document.addEventListener('DOMContentLoaded', () => {
        document.getElementById('fechaEmision').valueAsDate = new Date();
        agregarFila(); // Agregar primera fila vacía
    });

    // 2. Formateador de Moneda (Chile)
    const formatter = new Intl.NumberFormat('es-CL', {
        style: 'currency',
        currency: 'CLP',
        minimumFractionDigits: 0
    });

    // 3. Funciones de Tabla
    function agregarFila() {
        const tbody = document.querySelector('#tablaProductos tbody');
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td><input type="text" class="input-tabla descripcion" placeholder="Descripción del servicio"></td>
            <td><input type="number" class="input-tabla cantidad" value="1" min="1" oninput="calcular()" style="text-align: center;"></td>
            <td><input type="number" class="input-tabla precio" value="0" min="0" oninput="calcular()" style="text-align: right;"></td>
            <td class="total-fila" style="text-align: right; padding-right: 8px;">$0</td>
            <td style="text-align: center;"><button class="btn-eliminar" onclick="borrarFila(this)">X</button></td>
        `;
        tbody.appendChild(tr);
        calcular();
    }

    function borrarFila(btn) {
        const filas = document.querySelectorAll('#tablaProductos tbody tr');
        if (filas.length > 1) {
            btn.closest('tr').remove();
            calcular();
        } else {
            alert('Debe mantener al menos una línea en la cotización.');
        }
    }

    // 4. Cálculos Matemáticos
    function calcular() {
        let neto = 0;
        document.querySelectorAll('#tablaProductos tbody tr').forEach(fila => {
            const cant = parseFloat(fila.querySelector('.cantidad').value) || 0;
            const precio = parseFloat(fila.querySelector('.precio').value) || 0;
            const totalLinea = cant * precio;
            
            fila.querySelector('.total-fila').innerText = formatter.format(totalLinea);
            neto += totalLinea;
        });

        const iva = neto * 0.19;
        const total = neto + iva;

        document.getElementById('lblNeto').innerText = formatter.format(neto);
        document.getElementById('lblIva').innerText = formatter.format(iva);
        document.getElementById('lblTotal').innerText = formatter.format(total);
        
        return { neto, iva, total }; // Retornamos valores para el PDF
    }

    // --- LÓGICA DE GENERACIÓN PDF (NUEVA Y PROFESIONAL) ---
    async function generarPDF() {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF();
        
        // Variables de Colores Corporativos
        const azul = [31, 111, 139]; // #1F6F8B
        const naranja = [242, 92, 5]; // #F25C05

        // A. Encabezado con Fondo Azul
        doc.setFillColor(...azul);
        doc.rect(0, 0, 210, 40, 'F');
        
        doc.setTextColor(255, 255, 255);
        doc.setFontSize(24);
        doc.setFont("helvetica", "bold");
        doc.text("CUNDO SPA", 14, 20);
        
        doc.setFontSize(10);
        doc.setFont("helvetica", "normal");
        doc.text("Giro: Servicios Profesionales", 14, 26);
        doc.text("Email: contacto@cundospa.cl", 14, 31);

        // B. Datos Cotización (Esquina superior derecha)
        const nCot = document.getElementById('numCotizacion').value;
        const fecha = document.getElementById('fechaEmision').value;
        
        doc.setFontSize(14);
        doc.text("COTIZACIÓN", 150, 18);
        doc.setFontSize(16);
        doc.setFont("helvetica", "bold");
        doc.text(`N° ${nCot}`, 150, 25);
        doc.setFontSize(10);
        doc.setFont("helvetica", "normal");
        doc.text(`Fecha: ${fecha}`, 150, 32);

        // C. Datos del Cliente
        doc.setTextColor(0, 0, 0);
        doc.setFontSize(11);
        doc.setFont("helvetica", "bold");
        doc.text("DATOS DEL CLIENTE:", 14, 55);
        
        const clienteNombre = document.getElementById('clienteNombre').value || "----";
        const clienteRut = document.getElementById('clienteRut').value || "----";
        const clienteDir = document.getElementById('clienteDireccion').value || "----";
        const clienteFono = document.getElementById('clienteFono').value || "----";

        doc.setFont("helvetica", "normal");
        doc.setFontSize(10);
        doc.text(`Señor(es): ${clienteNombre}`, 14, 62);
        doc.text(`RUT: ${clienteRut}`, 14, 68);
        doc.text(`Dirección: ${clienteDir}`, 14, 74);
        doc.text(`Teléfono: ${clienteFono}`, 120, 68);

        // D. Tabla de Productos (Usando AutoTable)
        const filasPDF = [];
        document.querySelectorAll('#tablaProductos tbody tr').forEach(fila => {
            const desc = fila.querySelector('.descripcion').value;
            const cant = fila.querySelector('.cantidad').value;
            const precio = fila.querySelector('.precio').value;
            const total = fila.querySelector('.total-fila').innerText;
            
            // Formatear precio unitario visualmente
            const precioFmt = formatter.format(precio);

            if(desc.trim() !== "") {
                filasPDF.push([desc, cant, precioFmt, total]);
            }
        });

        doc.autoTable({
            startY: 85,
            head: [['Descripción', 'Cant.', 'Precio Unit.', 'Total']],
            body: filasPDF,
            theme: 'grid',
            headStyles: { fillColor: azul, textColor: 255, fontStyle: 'bold' },
            styles: { fontSize: 10, cellPadding: 3 },
            columnStyles: {
                0: { cellWidth: 90 },
                1: { halign: 'center' },
                2: { halign: 'right' },
                3: { halign: 'right', fontStyle: 'bold' }
            }
        });

        // E. Totales
        const finalY = doc.lastAutoTable.finalY + 10;
        const valores = calcular();

        doc.setFontSize(10);
        doc.text("Subtotal Neto:", 140, finalY);
        doc.text(formatter.format(valores.neto), 196, finalY, { align: 'right' });

        doc.text("IVA (19%):", 140, finalY + 6);
        doc.text(formatter.format(valores.iva), 196, finalY + 6, { align: 'right' });

        // Total Final Destacado
        doc.setFontSize(12);
        doc.setFont("helvetica", "bold");
        doc.setTextColor(...azul);
        doc.text("TOTAL:", 140, finalY + 14);
        doc.text(formatter.format(valores.total), 196, finalY + 14, { align: 'right' });

        // F. Footer / Condiciones
        doc.setTextColor(100);
        doc.setFontSize(8);
        doc.setFont("helvetica", "normal");
        
        const piePaginaY = 270;
        doc.setDrawColor(...naranja);
        doc.setLineWidth(1);
        doc.line(14, piePaginaY, 196, piePaginaY);
        
        doc.text("Condiciones de Pago: A convenir.", 14, piePaginaY + 5);
        doc.text("Validez de la oferta: 10 días hábiles.", 14, piePaginaY + 9);
        doc.text("Generado por Sistema CUNDO SPA", 196, piePaginaY + 9, { align: 'right' });

        // Guardar archivo
        doc.save(`Cotizacion_${nCot}_${clienteNombre}.pdf`);
    }
</script>

</body>
</html>
