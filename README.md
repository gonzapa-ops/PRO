<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotizador - EMPRESA CUNDO</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }

        .cotizador-container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .cotizador-header {
            background-color: #2c3e50;
            color: white;
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 3px solid #3498db;
        }

        .empresa-nombre {
            font-size: 20px;
            font-weight: bold;
            letter-spacing: 1px;
        }

        .numero-cotizacion {
            font-size: 16px;
        }

        .numero-cotizacion span:first-child {
            font-weight: normal;
            opacity: 0.9;
        }

        .numero-cotizacion span:last-child {
            font-weight: bold;
            color: #3498db;
            margin-left: 5px;
        }

        .cotizador-contenido {
            padding: 30px;
        }

        .seccion-cliente {
            margin-bottom: 30px;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 20px;
            background-color: #fafafa;
        }

        .seccion-titulo {
            font-size: 18px;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #3498db;
            text-transform: uppercase;
        }

        .busqueda-rut {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            align-items: center;
        }

        .busqueda-rut input {
            flex: 1;
            padding: 10px;
            font-size: 16px;
            border: 2px solid #ddd;
            border-radius: 4px;
            transition: border-color 0.3s;
            text-transform: uppercase;
        }

        .busqueda-rut input:focus {
            outline: none;
            border-color: #3498db;
        }

        .btn {
            padding: 10px 20px;
            font-size: 14px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
            font-weight: bold;
            text-transform: uppercase;
        }

        .btn-buscar {
            background-color: #3498db;
            color: white;
        }

        .btn-buscar:hover {
            background-color: #2980b9;
        }

        .btn-limpiar {
            background-color: #95a5a6;
            color: white;
        }

        .btn-limpiar:hover {
            background-color: #7f8c8d;
        }

        .btn-editar {
            background-color: #f39c12;
            color: white;
            padding: 8px 15px;
            font-size: 13px;
        }

        .btn-editar:hover {
            background-color: #e67e22;
        }

        .btn-cancelar {
            background-color: #e74c3c;
            color: white;
            padding: 8px 15px;
            font-size: 13px;
            margin-left: 10px;
        }

        .btn-cancelar:hover {
            background-color: #c0392b;
        }

        .btn-agregar {
            background-color: #27ae60;
            color: white;
            padding: 8px 15px;
            font-size: 13px;
        }

        .btn-agregar:hover {
            background-color: #229954;
        }

        .btn-eliminar {
            background-color: #e74c3c;
            color: white;
            padding: 5px 10px;
            font-size: 12px;
        }

        .btn-eliminar:hover {
            background-color: #c0392b;
        }

        .formulario-cliente {
            display: none;
            animation: fadeIn 0.3s;
        }

        .formulario-cliente.activo {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .campo-grupo {
            margin-bottom: 15px;
        }

        .campo-grupo label {
            display: block;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 5px;
            font-size: 14px;
            text-transform: uppercase;
        }

        .campo-grupo input {
            width: 100%;
            padding: 10px;
            font-size: 14px;
            border: 2px solid #ddd;
            border-radius: 4px;
            transition: border-color 0.3s;
            text-transform: uppercase;
        }

        .campo-grupo input:focus {
            outline: none;
            border-color: #3498db;
        }

        .campo-grupo input:disabled {
            background-color: #ecf0f1;
            cursor: not-allowed;
        }

        .fila-campos {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }

        .mensaje {
            padding: 10px 15px;
            border-radius: 4px;
            margin-bottom: 15px;
            font-size: 14px;
            text-transform: uppercase;
        }

        .mensaje-exito {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .mensaje-error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .mensaje-info {
            background-color: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .resumen-cliente {
            display: none;
            background-color: #e8f5e9;
            padding: 15px;
            border-radius: 4px;
            border-left: 4px solid #4caf50;
            position: relative;
        }

        .resumen-cliente.activo {
            display: block;
        }

        .resumen-cliente h4 {
            color: #2c3e50;
            margin-bottom: 10px;
            text-transform: uppercase;
        }

        .resumen-cliente p {
            margin: 5px 0;
            font-size: 14px;
            color: #555;
            text-transform: uppercase;
        }

        .resumen-cliente strong {
            color: #2c3e50;
            text-transform: uppercase;
        }

        .botones-resumen {
            margin-top: 15px;
            display: flex;
            gap: 10px;
        }

        .botones-formulario {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        /* ESTILOS PARA PRODUCTOS Y SERVICIOS */
        .seccion-productos {
            margin-bottom: 30px;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 20px;
            background-color: #fafafa;
        }

        .busqueda-producto {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            align-items: center;
        }

        .busqueda-producto input {
            flex: 1;
            padding: 10px;
            font-size: 16px;
            border: 2px solid #ddd;
            border-radius: 4px;
            transition: border-color 0.3s;
            text-transform: uppercase;
        }

        .busqueda-producto input:focus {
            outline: none;
            border-color: #3498db;
        }

        .tabla-productos {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        .tabla-productos thead {
            background-color: #34495e;
            color: white;
        }

        .tabla-productos th {
            padding: 12px;
            text-align: left;
            font-weight: bold;
            text-transform: uppercase;
            border: 1px solid #ddd;
        }

        .tabla-productos td {
            padding: 12px;
            border: 1px solid #ddd;
            text-transform: uppercase;
        }

        .tabla-productos tbody tr:nth-child(even) {
            background-color: #f9f9f9;
        }

        .tabla-productos tbody tr:hover {
            background-color: #f0f0f0;
        }

        .tabla-productos input {
            width: 100%;
            padding: 6px;
            border: 1px solid #ddd;
            border-radius: 3px;
            text-transform: uppercase;
            font-size: 13px;
        }

        .tabla-productos input:focus {
            outline: none;
            border-color: #3498db;
            box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
        }

        .tabla-productos input[type="number"] {
            text-align: right;
        }

        .valor-numerico {
            text-align: right;
            font-weight: bold;
        }

        .celda-acciones {
            text-align: center;
        }

        .btn-pequeno {
            padding: 5px 10px;
            font-size: 12px;
        }

        .formulario-producto {
            display: none;
            background-color: #fff3cd;
            padding: 15px;
            border-radius: 4px;
            border-left: 4px solid #f39c12;
            margin-bottom: 20px;
            animation: fadeIn 0.3s;
        }

        .formulario-producto.activo {
            display: block;
        }

        .formulario-producto h4 {
            text-transform: uppercase;
            color: #856404;
            margin-bottom: 15px;
        }

        .fila-campos-producto {
            display: grid;
            grid-template-columns: 1fr 2fr 1fr;
            gap: 15px;
            margin-bottom: 10px;
        }

        .fila-campos-producto .campo-grupo {
            margin-bottom: 0;
        }

        .botones-producto {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        .alerta-sin-productos {
            background-color: #e8f4f8;
            padding: 15px;
            border-radius: 4px;
            border-left: 4px solid #3498db;
            text-align: center;
            color: #2c3e50;
            text-transform: uppercase;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="cotizador-container">
        <header class="cotizador-header">
            <div class="empresa-nombre">EMPRESA CUNDO</div>
            <div class="numero-cotizacion">
                <span>COTIZACIÓN N°: </span>
                <span id="numeroCotizacion">CO100500</span>
            </div>
        </header>

        <main class="cotizador-contenido">
            <!-- SECCIÓN DATOS DEL CLIENTE -->
            <section class="seccion-cliente">
                <h2 class="seccion-titulo">DATOS DEL CLIENTE</h2>
                
                <div class="busqueda-rut">
                    <input type="text" 
                           id="inputRut" 
                           placeholder="INGRESE RUT (EJ: 78070615-7)" 
                           maxlength="12">
                    <button class="btn btn-buscar" onclick="buscarCliente()">BUSCAR</button>
                    <button class="btn btn-limpiar" onclick="limpiarFormulario()">LIMPIAR</button>
                </div>

                <div id="mensaje"></div>

                <div id="resumenCliente" class="resumen-cliente"></div>

                <div id="formularioCliente" class="formulario-cliente">
                    <div class="campo-grupo">
                        <label for="rut">RUT *</label>
                        <input type="text" id="rut" disabled>
                    </div>

                    <div class="campo-grupo">
                        <label for="razonSocial">RAZÓN SOCIAL *</label>
                        <input type="text" id="razonSocial" required>
                    </div>

                    <div class="campo-grupo">
                        <label for="giro">GIRO *</label>
                        <input type="text" id="giro" required>
                    </div>

                    <div class="campo-grupo">
                        <label for="direccion">DIRECCIÓN DE FACTURACIÓN *</label>
                        <input type="text" id="direccion" required>
                    </div>

                    <div class="fila-campos">
                        <div class="campo-grupo">
                            <label for="nombreContacto">NOMBRE DE CONTACTO *</label>
                            <input type="text" id="nombreContacto" required>
                        </div>

                        <div class="campo-grupo">
                            <label for="celular">CELULAR *</label>
                            <input type="tel" id="celular" placeholder="+56912345678" required>
                        </div>
                    </div>

                    <div class="campo-grupo">
                        <label for="mail">MAIL *</label>
                        <input type="email" id="mail" required>
                    </div>

                    <div class="botones-formulario">
                        <button class="btn btn-buscar" onclick="guardarCliente()">
                            <span id="textoBotonGuardar">GUARDAR CLIENTE</span>
                        </button>
                        <button class="btn btn-cancelar" id="btnCancelarEdicion" onclick="cancelarEdicion()" style="display: none;">
                            CANCELAR
                        </button>
                    </div>
                </div>

            </section>

            <!-- SECCIÓN PRODUCTOS Y SERVICIOS -->
            <section class="seccion-productos">
                <h2 class="seccion-titulo">PRODUCTOS Y/O SERVICIOS</h2>

                <div class="busqueda-producto">
                    <input type="text" 
                           id="inputCodigoProducto" 
                           placeholder="INGRESE CÓDIGO DEL PRODUCTO O SERVICIO" 
                           maxlength="20">
                    <button class="btn btn-buscar" onclick="buscarProducto()">BUSCAR</button>
                </div>

                <div id="mensajeProducto"></div>

                <div id="formularioProducto" class="formulario-producto">
                    <h4>CREAR NUEVO PRODUCTO/SERVICIO</h4>
                    <div class="fila-campos-producto">
                        <div class="campo-grupo">
                            <label for="codigoProducto">CÓDIGO *</label>
                            <input type="text" id="codigoProducto" disabled>
                        </div>
                        <div class="campo-grupo">
                            <label for="descripcionProducto">DESCRIPCIÓN *</label>
                            <input type="text" id="descripcionProducto" required>
                        </div>
                        <div class="campo-grupo">
                            <label for="valorNetoProducto">VALOR NETO *</label>
                            <input type="number" id="valorNetoProducto" min="0" step="0.01" required>
                        </div>
                    </div>
                    <div class="botones-producto">
                        <button class="btn btn-agregar" onclick="guardarProducto()">GUARDAR PRODUCTO</button>
                        <button class="btn btn-cancelar btn-pequeno" onclick="cancelarProducto()">CANCELAR</button>
                    </div>
                </div>

                <div id="tablaProductosContenedor">
                    <div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>
                </div>

            </section>

        </main>
    </div>

    <script>
        class GestorCotizaciones {
            constructor() {
                this.prefijo = 'CO';
                this.numeroBase = 100500;
                this.cargarNumeroCotizacion();
            }

            cargarNumeroCotizacion() {
                const numeroGuardado = localStorage.getItem('ultimaCotizacion');
                if (numeroGuardado) {
                    this.numeroBase = parseInt(numeroGuardado);
                }
                this.actualizarDisplay();
            }

            obtenerNumeroActual() {
                return `${this.prefijo}${this.numeroBase}`;
            }

            siguienteCotizacion() {
                this.numeroBase++;
                localStorage.setItem('ultimaCotizacion', this.numeroBase);
                this.actualizarDisplay();
                return this.obtenerNumeroActual();
            }

            actualizarDisplay() {
                const elemento = document.getElementById('numeroCotizacion');
                if (elemento) {
                    elemento.textContent = this.obtenerNumeroActual();
                }
            }
        }

        class GestorClientes {
            constructor() {
                this.clientes = this.cargarClientes();
            }

            cargarClientes() {
                const clientesGuardados = localStorage.getItem('clientes');
                return clientesGuardados ? JSON.parse(clientesGuardados) : {};
            }

            guardarClientes() {
                localStorage.setItem('clientes', JSON.stringify(this.clientes));
            }

            buscarPorRut(rut) {
                return this.clientes[rut] || null;
            }

            agregarCliente(rut, datos) {
                this.clientes[rut] = datos;
                this.guardarClientes();
            }

            actualizarCliente(rut, datos) {
                this.clientes[rut] = datos;
                this.guardarClientes();
            }
        }

        class GestorProductos {
            constructor() {
                this.productos = this.cargarProductos();
            }

            cargarProductos() {
                const productosGuardados = localStorage.getItem('productos');
                return productosGuardados ? JSON.parse(productosGuardados) : {};
            }

            guardarProductos() {
                localStorage.setItem('productos', JSON.stringify(this.productos));
            }

            buscarPorCodigo(codigo) {
                return this.productos[codigo] || null;
            }

            agregarProducto(codigo, datos) {
                this.productos[codigo] = datos;
                this.guardarProductos();
            }

            obtenerTodos() {
                return this.productos;
            }
        }

        const gestorCotizaciones = new GestorCotizaciones();
        const gestorClientes = new GestorClientes();
        const gestorProductos = new GestorProductos();
        let clienteActual = null;
        let modoEdicion = false;
        let productosEnCotizacion = [];
        let codigoProductoActual = null;

        // FUNCIONES CLIENTE
        document.getElementById('inputRut').addEventListener('input', function(e) {
            let valor = e.target.value.replace(/[^0-9kK]/g, '');
            if (valor.length > 1) {
                valor = valor.slice(0, -1) + '-' + valor.slice(-1);
            }
            e.target.value = valor.toUpperCase();
        });

        function buscarCliente() {
            const rut = document.getElementById('inputRut').value.trim();
            const mensaje = document.getElementById('mensaje');
            const formulario = document.getElementById('formularioCliente');
            const resumen = document.getElementById('resumenCliente');

            if (!rut) {
                mostrarMensaje('POR FAVOR INGRESE UN RUT', 'error');
                return;
            }

            if (!validarRut(rut)) {
                mostrarMensaje('EL FORMATO DEL RUT NO ES VÁLIDO', 'error');
                return;
            }

            const cliente = gestorClientes.buscarPorRut(rut);

            if (cliente) {
                clienteActual = cliente;
                modoEdicion = false;
                mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
                mostrarResumenCliente(cliente);
                formulario.classList.remove('activo');
            } else {
                mostrarMensaje('CLIENTE NO ENCONTRADO. POR FAVOR COMPLETE LOS DATOS PARA CREAR EL CLIENTE.', 'info');
                resumen.classList.remove('activo');
                formulario.classList.add('activo');
                document.getElementById('rut').value = rut;
                document.getElementById('textoBotonGuardar').textContent = 'GUARDAR CLIENTE';
                document.getElementById('btnCancelarEdicion').style.display = 'none';
                limpiarCamposFormulario();
                modoEdicion = false;
            }
        }

        function validarRut(rut) {
            const rutRegex = /^[0-9]{7,8}-[0-9kK]$/;
            return rutRegex.test(rut);
        }

        function mostrarMensaje(texto, tipo) {
            const mensaje = document.getElementById('mensaje');
            mensaje.className = `mensaje mensaje-${tipo}`;
            mensaje.textContent = texto;
            mensaje.style.display = 'block';
        }

        function mostrarResumenCliente(cliente) {
            const resumen = document.getElementById('resumenCliente');
            resumen.className = 'resumen-cliente activo';
            resumen.innerHTML = `
                <h4>✓ CLIENTE REGISTRADO</h4>
                <p><strong>RUT:</strong> ${cliente.rut}</p>
                <p><strong>RAZÓN SOCIAL:</strong> ${cliente.razonSocial}</p>
                <p><strong>GIRO:</strong> ${cliente.giro}</p>
                <p><strong>DIRECCIÓN:</strong> ${cliente.direccion}</p>
                <p><strong>CONTACTO:</strong> ${cliente.nombreContacto}</p>
                <p><strong>CELULAR:</strong> ${cliente.celular}</p>
                <p><strong>MAIL:</strong> ${cliente.mail}</p>
                <div class="botones-resumen">
                    <button class="btn btn-editar" onclick="editarCliente()">✏️ EDITAR DATOS</button>
                </div>
            `;
        }

        function editarCliente() {
            if (!clienteActual) return;

            modoEdicion = true;
            const formulario = document.getElementById('formularioCliente');
            const resumen = document.getElementById('resumenCliente');

            document.getElementById('rut').value = clienteActual.rut;
            document.getElementById('razonSocial').value = clienteActual.razonSocial;
            document.getElementById('giro').value = clienteActual.giro;
            document.getElementById('direccion').value = clienteActual.direccion;
            document.getElementById('nombreContacto').value = clienteActual.nombreContacto;
            document.getElementById('celular').value = clienteActual.celular;
            document.getElementById('mail').value = clienteActual.mail;

            resumen.classList.remove('activo');
            formulario.classList.add('activo');
            document.getElementById('textoBotonGuardar').textContent = 'GUARDAR CAMBIOS';
            document.getElementById('btnCancelarEdicion').style.display = 'inline-block';

            mostrarMensaje('MODO DE EDICIÓN ACTIVADO. MODIFIQUE LOS CAMPOS NECESARIOS.', 'info');
        }

        function cancelarEdicion() {
            if (!clienteActual) return;

            modoEdicion = false;
            document.getElementById('formularioCliente').classList.remove('activo');
            mostrarMensaje('CLIENTE ENCONTRADO EN LA BASE DE DATOS', 'exito');
            mostrarResumenCliente(clienteActual);
        }

        function guardarCliente() {
            const rut = document.getElementById('rut').value;
            const razonSocial = document.getElementById('razonSocial').value.trim();
            const giro = document.getElementById('giro').value.trim();
            const direccion = document.getElementById('direccion').value.trim();
            const nombreContacto = document.getElementById('nombreContacto').value.trim();
            const celular = document.getElementById('celular').value.trim();
            const mail = document.getElementById('mail').value.trim();

            if (!razonSocial || !giro || !direccion || !nombreContacto || !celular || !mail) {
                mostrarMensaje('POR FAVOR COMPLETE TODOS LOS CAMPOS OBLIGATORIOS (*)', 'error');
                return;
            }

            if (!validarEmail(mail)) {
                mostrarMensaje('EL FORMATO DEL CORREO ELECTRÓNICO NO ES VÁLIDO', 'error');
                return;
            }

            const cliente = {
                rut,
                razonSocial: razonSocial.toUpperCase(),
                giro: giro.toUpperCase(),
                direccion: direccion.toUpperCase(),
                nombreContacto: nombreContacto.toUpperCase(),
                celular,
                mail: mail.toUpperCase()
            };

            if (modoEdicion) {
                gestorClientes.actualizarCliente(rut, cliente);
                mostrarMensaje('DATOS DEL CLIENTE ACTUALIZADOS EXITOSAMENTE', 'exito');
            } else {
                gestorClientes.agregarCliente(rut, cliente);
                mostrarMensaje('CLIENTE GUARDADO EXITOSAMENTE', 'exito');
            }

            clienteActual = cliente;
            modoEdicion = false;
            mostrarResumenCliente(cliente);
            document.getElementById('formularioCliente').classList.remove('activo');
        }

        function validarEmail(email) {
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(email);
        }

        function limpiarFormulario() {
            document.getElementById('inputRut').value = '';
            document.getElementById('mensaje').style.display = 'none';
            document.getElementById('resumenCliente').classList.remove('activo');
            document.getElementById('formularioCliente').classList.remove('activo');
            document.getElementById('btnCancelarEdicion').style.display = 'none';
            limpiarCamposFormulario();
            clienteActual = null;
            modoEdicion = false;
        }

        function limpiarCamposFormulario() {
            document.getElementById('razonSocial').value = '';
            document.getElementById('giro').value = '';
            document.getElementById('direccion').value = '';
            document.getElementById('nombreContacto').value = '';
            document.getElementById('celular').value = '';
            document.getElementById('mail').value = '';
        }

        // FUNCIONES PRODUCTOS
        document.getElementById('inputCodigoProducto').addEventListener('input', function(e) {
            e.target.value = e.target.value.toUpperCase();
        });

        function buscarProducto() {
            const codigo = document.getElementById('inputCodigoProducto').value.trim();
            const mensajeProducto = document.getElementById('mensajeProducto');
            const formularioProducto = document.getElementById('formularioProducto');

            if (!codigo) {
                mostrarMensajeProducto('POR FAVOR INGRESE UN CÓDIGO', 'error');
                return;
            }

            const producto = gestorProductos.buscarPorCodigo(codigo);

            if (producto) {
                codigoProductoActual = codigo;
                mostrarMensajeProducto('PRODUCTO ENCONTRADO. AGREGUE CANTIDAD.', 'exito');
                formularioProducto.classList.remove('activo');
                agregarProductoACotizacion(codigo, producto);
                document.getElementById('inputCodigoProducto').value = '';
            } else {
                mostrarMensajeProducto('PRODUCTO NO ENCONTRADO. POR FAVOR COMPLETE LOS DATOS PARA CREAR EL PRODUCTO.', 'info');
                codigoProductoActual = codigo;
                formularioProducto.classList.add('activo');
                document.getElementById('codigoProducto').value = codigo;
                document.getElementById('descripcionProducto').value = '';
                document.getElementById('valorNetoProducto').value = '';
            }
        }

        function mostrarMensajeProducto(texto, tipo) {
            const mensaje = document.getElementById('mensajeProducto');
            mensaje.className = `mensaje mensaje-${tipo}`;
            mensaje.textContent = texto;
            mensaje.style.display = 'block';
        }

        function guardarProducto() {
            const codigo = document.getElementById('codigoProducto').value.trim();
            const descripcion = document.getElementById('descripcionProducto').value.trim();
            const valorNeto = parseFloat(document.getElementById('valorNetoProducto').value);

            if (!codigo || !descripcion || isNaN(valorNeto) || valorNeto <= 0) {
                mostrarMensajeProducto('POR FAVOR COMPLETE TODOS LOS CAMPOS CORRECTAMENTE', 'error');
                return;
            }

            const producto = {
                codigo,
                descripcion: descripcion.toUpperCase(),
                valorNeto: parseFloat(valorNeto.toFixed(2))
            };

            gestorProductos.agregarProducto(codigo, producto);
            mostrarMensajeProducto('PRODUCTO GUARDADO EXITOSAMENTE', 'exito');
            cancelarProducto();
            agregarProductoACotizacion(codigo, producto);
        }

        function cancelarProducto() {
            document.getElementById('formularioProducto').classList.remove('activo');
            document.getElementById('codigoProducto').value = '';
            document.getElementById('descripcionProducto').value = '';
            document.getElementById('valorNetoProducto').value = '';
            document.getElementById('inputCodigoProducto').value = '';
            document.getElementById('mensajeProducto').style.display = 'none';
        }

        function agregarProductoACotizacion(codigo, producto) {
            const productoExistente = productosEnCotizacion.find(p => p.codigo === codigo);

            if (productoExistente) {
                productoExistente.cantidad += 1;
                productoExistente.total = (productoExistente.cantidad * productoExistente.valorNeto).toFixed(2);
            } else {
                productosEnCotizacion.push({
                    codigo: producto.codigo,
                    descripcion: producto.descripcion,
                    cantidad: 1,
                    valorNeto: producto.valorNeto,
                    total: producto.valorNeto.toFixed(2)
                });
            }

            actualizarTablaProductos();
        }

        function actualizarTablaProductos() {
            const contenedor = document.getElementById('tablaProductosContenedor');

            if (productosEnCotizacion.length === 0) {
                contenedor.innerHTML = '<div class="alerta-sin-productos">NO HAY PRODUCTOS AÑADIDOS A LA COTIZACIÓN</div>';
                return;
            }

            let html = `
                <table class="tabla-productos">
                    <thead>
                        <tr>
                            <th>CÓDIGO</th>
                            <th>DESCRIPCIÓN</th>
                            <th style="width: 80px;">CANTIDAD</th>
                            <th style="width: 100px;">VALOR NETO</th>
                            <th style="width: 100px;">TOTAL</th>
                            <th style="width: 60px;">ACCIÓN</th>
                        </tr>
                    </thead>
                    <tbody>
            `;

            productosEnCotizacion.forEach((producto, index) => {
                html += `
                    <tr>
                        <td>${producto.codigo}</td>
                        <td>${producto.descripcion}</td>
                        <td>
                            <input type="number" 
                                   value="${producto.cantidad}" 
                                   min="1" 
                                   onchange="actualizarCantidad(${index}, this.value)">
                        </td>
                        <td class="valor-numerico">$${parseFloat(producto.valorNeto).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                        <td class="valor-numerico">$${parseFloat(producto.total).toLocaleString('es-CL', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                        <td class="celda-acciones">
                            <button class="btn btn-eliminar btn-pequeno" onclick="eliminarProducto(${index})">ELIMINAR</button>
                        </td>
                    </tr>
                `;
            });

            html += `
                    </tbody>
                </table>
            `;

            contenedor.innerHTML = html;
        }

        function actualizarCantidad(index, cantidad) {
            const cantidadNum = parseInt(cantidad);

            if (cantidadNum <= 0) {
                mostrarMensajeProducto('LA CANTIDAD DEBE SER MAYOR A 0', 'error');
                actualizarTablaProductos();
                return;
            }

            productosEnCotizacion[index].cantidad = cantidadNum;
            productosEnCotizacion[index].total = (cantidadNum * productosEnCotizacion[index].valorNeto).toFixed(2);
            actualizarTablaProductos();
        }

        function eliminarProducto(index) {
            productosEnCotizacion.splice(index, 1);
            actualizarTablaProductos();
            mostrarMensajeProducto('PRODUCTO ELIMINADO CORRECTAMENTE', 'exito');
            setTimeout(() => {
                document.getElementById('mensajeProducto').style.display = 'none';
            }, 3000);
        }

        function nuevaCotizacion() {
            const nuevoNumero = gestorCotizaciones.siguienteCotizacion();
            console.log('Nueva cotización creada:', nuevoNumero);
            limpiarFormulario();
            productosEnCotizacion = [];
            actualizarTablaProductos();
        }
    </script>
</body>
</html>
