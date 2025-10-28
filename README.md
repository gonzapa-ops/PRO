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

        const gestorCotizaciones = new GestorCotizaciones();
        const gestorClientes = new GestorClientes();
        let clienteActual = null;
        let modoEdicion = false;

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

        function nuevaCotizacion() {
            const nuevoNumero = gestorCotizaciones.siguienteCotizacion();
            console.log('Nueva cotización creada:', nuevoNumero);
            limpiarFormulario();
        }
    </script>
</body>
</html>
