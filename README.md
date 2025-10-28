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
            max-width: 1000px;
            margin: 0 auto;
            background-color: white;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        /* Header - Franja delgada */
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

        /* Sección de Datos del Cliente */
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

        /* Formulario de datos del cliente */
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
        }

        .campo-grupo input {
            width: 100%;
            padding: 10px;
            font-size: 14px;
            border: 2px solid #ddd;
            border-radius: 4px;
            transition: border-color 0.3s;
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
        }

        .resumen-cliente.activo {
            display: block;
        }

        .resumen-cliente h4 {
            color: #2c3e50;
            margin-bottom: 10px;
        }

        .resumen-cliente p {
            margin: 5px 0;
            font-size: 14px;
            color: #555;
        }

        .resumen-cliente strong {
            color: #2c3e50;
        }
    </style>
</head>
<body>
    <div class="cotizador-container">
        <!-- Header con franja delgada -->
        <header class="cotizador-header">
            <div class="empresa-nombre">EMPRESA CUNDO</div>
            <div class="numero-cotizacion">
                <span>Cotización N°: </span>
                <span id="numeroCotizacion">CO100500</span>
            </div>
        </header>

        <!-- Contenido del cotizador -->
        <main class="cotizador-contenido">
            
            <!-- Sección de Datos del Cliente -->
            <section class="seccion-cliente">
                <h2 class="seccion-titulo">Datos del Cliente</h2>
                
                <!-- Búsqueda por RUT -->
                <div class="busqueda-rut">
                    <input type="text" 
                           id="inputRut" 
                           placeholder="Ingrese RUT (ej: 78070615-7)" 
                           maxlength="12">
                    <button class="btn btn-buscar" onclick="buscarCliente()">Buscar</button>
                    <button class="btn btn-limpiar" onclick="limpiarFormulario()">Limpiar</button>
                </div>

                <!-- Mensaje de estado -->
                <div id="mensaje"></div>

                <!-- Resumen del cliente encontrado -->
                <div id="resumenCliente" class="resumen-cliente"></div>

                <!-- Formulario para crear/editar cliente -->
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
                            <input type="tel" id="celular" placeholder="ej: +56912345678" required>
                        </div>
                    </div>

                    <div class="campo-grupo">
                        <label for="mail">MAIL *</label>
                        <input type="email" id="mail" required>
                    </div>

                    <button class="btn btn-buscar" onclick="guardarCliente()" style="margin-top: 10px;">
                        Guardar Cliente
                    </button>
                </div>

            </section>

        </main>
    </div>

    <script>
        // Sistema de numeración de cotizaciones
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

        // Base de datos de clientes (simulada con localStorage)
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
        }

        // Inicializar gestores
        const gestorCotizaciones = new GestorCotizaciones();
        const gestorClientes = new GestorClientes();
        let clienteActual = null;

        // Formatear RUT mientras se escribe
        document.getElementById('inputRut').addEventListener('input', function(e) {
            let valor = e.target.value.replace(/[^0-9kK]/g, '');
            if (valor.length > 1) {
                valor = valor.slice(0, -1) + '-' + valor.slice(-1);
            }
            e.target.value = valor;
        });

        // Buscar cliente por RUT
        function buscarCliente() {
            const rut = document.getElementById('inputRut').value.trim();
            const mensaje = document.getElementById('mensaje');
            const formulario = document.getElementById('formularioCliente');
            const resumen = document.getElementById('resumenCliente');

            if (!rut) {
                mostrarMensaje('Por favor ingrese un RUT', 'error');
                return;
            }

            if (!validarRut(rut)) {
                mostrarMensaje('El formato del RUT no es válido', 'error');
                return;
            }

            const cliente = gestorClientes.buscarPorRut(rut);

            if (cliente) {
                // Cliente encontrado
                clienteActual = cliente;
                mostrarMensaje('Cliente encontrado en la base de datos', 'exito');
                mostrarResumenCliente(cliente);
                formulario.classList.remove('activo');
            } else {
                // Cliente no encontrado, mostrar formulario
                mostrarMensaje('Cliente no encontrado. Por favor complete los datos para crear el cliente.', 'info');
                resumen.classList.remove('activo');
                formulario.classList.add('activo');
                document.getElementById('rut').value = rut;
                limpiarCamposFormulario();
            }
        }

        // Validar formato de RUT
        function validarRut(rut) {
            const rutRegex = /^[0-9]{7,8}-[0-9kK]$/;
            return rutRegex.test(rut);
        }

        // Mostrar mensaje
        function mostrarMensaje(texto, tipo) {
            const mensaje = document.getElementById('mensaje');
            mensaje.className = `mensaje mensaje-${tipo}`;
            mensaje.textContent = texto;
            mensaje.style.display = 'block';
        }

        // Mostrar resumen del cliente encontrado
        function mostrarResumenCliente(cliente) {
            const resumen = document.getElementById('resumenCliente');
            resumen.innerHTML = `
                <h4>✓ Cliente Registrado</h4>
                <p><strong>RUT:</strong> ${cliente.rut}</p>
                <p><strong>Razón Social:</strong> ${cliente.razonSocial}</p>
                <p><strong>Giro:</strong> ${cliente.giro}</p>
                <p><strong>Dirección:</strong> ${cliente.direccion}</p>
                <p><strong>Contacto:</strong> ${cliente.nombreContacto}</p>
                <p><strong>Celular:</strong> ${cliente.celular}</p>
                <p><strong>Mail:</strong> ${cliente.mail}</p>
            `;
            resumen.classList.add('activo');
        }

        // Guardar nuevo cliente
        function guardarCliente() {
            const rut = document.getElementById('rut').value;
            const razonSocial = document.getElementById('razonSocial').value.trim();
            const giro = document.getElementById('giro').value.trim();
            const direccion = document.getElementById('direccion').value.trim();
            const nombreContacto = document.getElementById('nombreContacto').value.trim();
            const celular = document.getElementById('celular').value.trim();
            const mail = document.getElementById('mail').value.trim();

            if (!razonSocial || !giro || !direccion || !nombreContacto || !celular || !mail) {
                mostrarMensaje('Por favor complete todos los campos obligatorios (*)', 'error');
                return;
            }

            if (!validarEmail(mail)) {
                mostrarMensaje('El formato del correo electrónico no es válido', 'error');
                return;
            }

            const cliente = {
                rut,
                razonSocial,
                giro,
                direccion,
                nombreContacto,
                celular,
                mail
            };

            gestorClientes.agregarCliente(rut, cliente);
            clienteActual = cliente;

            mostrarMensaje('Cliente guardado exitosamente', 'exito');
            mostrarResumenCliente(cliente);
            document.getElementById('formularioCliente').classList.remove('activo');
        }

        // Validar email
        function validarEmail(email) {
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(email);
        }

        // Limpiar formulario
        function limpiarFormulario() {
            document.getElementById('inputRut').value = '';
            document.getElementById('mensaje').style.display = 'none';
            document.getElementById('resumenCliente').classList.remove('activo');
            document.getElementById('formularioCliente').classList.remove('activo');
            limpiarCamposFormulario();
            clienteActual = null;
        }

        // Limpiar campos del formulario
        function limpiarCamposFormulario() {
            document.getElementById('razonSocial').value = '';
            document.getElementById('giro').value = '';
            document.getElementById('direccion').value = '';
            document.getElementById('nombreContacto').value = '';
            document.getElementById('celular').value = '';
            document.getElementById('mail').value = '';
        }

        // Función para crear nueva cotización
        function nuevaCotizacion() {
            const nuevoNumero = gestorCotizaciones.siguienteCotizacion();
            console.log('Nueva cotización creada:', nuevoNumero);
            limpiarFormulario();
        }
    </script>
</body>
</html>
