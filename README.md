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
            min-height: 400px;
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

        <!-- Aquí irá el contenido del cotizador -->
        <main class="cotizador-contenido">
            <!-- Por implementar -->
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

            // Cargar el número de cotización desde localStorage
            cargarNumeroCotizacion() {
                const numeroGuardado = localStorage.getItem('ultimaCotizacion');
                if (numeroGuardado) {
                    this.numeroBase = parseInt(numeroGuardado);
                }
                this.actualizarDisplay();
            }

            // Obtener el número actual de cotización
            obtenerNumeroActual() {
                return `${this.prefijo}${this.numeroBase}`;
            }

            // Incrementar y guardar el número de cotización
            siguienteCotizacion() {
                this.numeroBase++;
                localStorage.setItem('ultimaCotizacion', this.numeroBase);
                this.actualizarDisplay();
                return this.obtenerNumeroActual();
            }

            // Actualizar el display en la página
            actualizarDisplay() {
                const elemento = document.getElementById('numeroCotizacion');
                if (elemento) {
                    elemento.textContent = this.obtenerNumeroActual();
                }
            }
        }

        // Inicializar el gestor de cotizaciones
        const gestorCotizaciones = new GestorCotizaciones();

        // Función para crear nueva cotización (se llamará cuando sea necesario)
        function nuevaCotizacion() {
            const nuevoNumero = gestorCotizaciones.siguienteCotizacion();
            console.log('Nueva cotización creada:', nuevoNumero);
            // Aquí puedes agregar lógica adicional para limpiar formularios, etc.
        }
    </script>
</body>
</html>
