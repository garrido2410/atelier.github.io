<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generador de Boleta</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        .container {
            width: 100%;
            max-width: 600px; /* Ajusta el ancho máximo para tabletas */
            margin: 0 auto;
            padding: 20px;
            background: #fff;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
            font-size: 14px; /* Ajusta el tamaño de la fuente para mejor legibilidad en tabletas */
        }
        .container h1 {
            font-size: 18px; /* Aumenta el tamaño del título */
            margin: 0 0 20px;
            text-align: center;
        }
        .container label {
            display: block;
            margin-bottom: 8px; /* Aumenta el espacio entre etiquetas y campos */
            font-size: 14px; /* Ajusta el tamaño de la fuente */
        }
        .container input[type="text"],
        .container select {
            width: 100%;
            padding: 10px; /* Aumenta el padding para mayor comodidad en tablets */
            margin-bottom: 15px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 14px; /* Ajusta el tamaño de la fuente */
        }
        .container button {
            width: 100%;
            padding: 10px;
            background-color: #28a745;
            color: #fff;
            border: none;
            border-radius: 4px;
            font-size: 14px; /* Ajusta el tamaño de la fuente del botón */
            cursor: pointer;
        }
        .container button:hover {
            background-color: #218838;
        }
        @media print {
            @page {
                margin: 0; /* Eliminar márgenes de la página al imprimir */
            }
            body, html {
                margin: 0;
                padding: 0;
                height: 100%;
            }
            .receipt {
                width: 80mm;
                padding: 0;
                box-shadow: none;
                font-size: 14px;
                margin: 0;
                height: auto;
                padding-top: 0; /* Eliminar cualquier espacio en la parte superior */
            }
            .logo img {
                margin-top: 0; /* Asegurarse de que el logo esté al inicio sin espacio superior */
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Generador de Boleta</h1>
        <label for="cliente">Cliente:</label>
        <input type="text" id="cliente" placeholder="Nombre del Cliente">
        <label for="fecha">Fecha:</label>
        <input type="text" id="fecha" readonly>
        <label for="hora">Hora:</label>
        <input type="text" id="hora" readonly>
        <label for="tiempo">Tiempo de juego:</label>
        <select id="tiempo" onchange="calcularPrecio()">
            <option value="">Seleccione una opción</option>
            <option value="Media hora" data-precio="6">Media hora</option>
            <option value="Una hora" data-precio="10">Una hora</option>
            <option value="Dos horas" data-precio="18">Dos horas</option>
            <option value="Full dayS" data-precio="18">Full dayS</option>
            <option value="Full dayFS" data-precio="25">Full dayFS</option>
            <option value="Membresia Semanal" data-precio="44">Membresía Semanal</option>
            <option value="Membresía Mensual" data-precio="89">Membresía Mensual</option>
        </select>
        <label for="precio">Precio:</label>
        <input type="text" id="precio" readonly>
        <label for="metodoPago">Método de pago:</label>
        <select id="metodoPago">
            <option value="Efectivo">Efectivo</option>
            <option value="Transferencia">Transferencia</option>
            <option value="Yape">Yape</option>
        </select>
        <button onclick="generarBoleta()">Generar Boleta</button>
    </div>
    <script>
        function calcularPrecio() {
            const tiempoSelect = document.getElementById('tiempo');
            const precioInput = document.getElementById('precio');
            const selectedOption = tiempoSelect.options[tiempoSelect.selectedIndex];
            const precio = selectedOption ? selectedOption.getAttribute('data-precio') : '';
            precioInput.value = precio ? `S/${precio}` : '';
        }

        function generarBoleta() {
            const cliente = document.getElementById('cliente').value;
            const fecha = document.getElementById('fecha').value;
            const hora = document.getElementById('hora').value;
            const tiempo = document.getElementById('tiempo').value;
            const precio = document.getElementById('precio').value;
            const metodoPago = document.getElementById('metodoPago').value;

            const boletaHTML = `
                <!DOCTYPE html>
                <html lang="es">
                <head>
                    <meta charset="UTF-8">
                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                    <title>Boleta</title>
                    <style>
                        @page {
                            margin: 0;
                        }
                        body {
                            margin: 0;
                            padding: 0;
                            background-color: #fff;
                        }
                        .receipt {
                            width: 80mm;
                            padding: 0;
                            box-shadow: none;
                            font-size: 14px;
                            margin: 0;
                            height: auto;
                            padding-top: 0; /* Ajusta para que inicie justo debajo del logo */
                        }
                        .receipt .address {
                            margin-bottom: 10px;
                        }
                        .receipt .address p {
                            margin: 0;
                            font-size: 16px;
                            font-weight: bold;
                            text-align: left;
                        }
                        .receipt p {
                            margin: 2px 0;
                            text-align: left;
                        }
                        .bold {
                            font-weight: bold;
                        }
                        .logo {
                            text-align: center;
                            margin-bottom: 10px;
                        }
                        img {
                            width: 120px; /* Tamaño del logo */
                            height: auto;
                            margin-top: 0; /* Evita espacio adicional arriba del logo */
                        }
                    </style>
                </head>
                <body>
                    <div class="receipt">
                        <div class="logo">
                            <img src="zoe.jpg" alt="Logo" onload="window.print()">
                        </div>
                        <div class="address">
                            <p class="bold">Atelier de Zoe - Salon kids</p>
                            <p class="bold">Luzuriaga 306 - Los Libertadores SMP</p>
                        </div>
                        <p><strong>Cliente:</strong> ${cliente}</p>
                        <p><strong>Tiempo de juego:</strong> ${tiempo}</p>
                        <p><strong>Precio:</strong> ${precio}</p>
                        <p><strong>Método de pago:</strong> ${metodoPago}</p>
                        <p><strong>Fecha:</strong> ${fecha}</p>
                        <p><strong>Hora:</strong> ${hora}</p>
                        <p class="bold">¡Gracias por su visita!</p>
                        <p class="bold">Recuerda: ¡Celebra tu cumpleaños con Nosotros, escríbenos al 982604285!</p>
                    </div>
                </body>
                </html>
            `;

            const printWindow = window.open('', '', 'height=600,width=800');
            printWindow.document.open();
            printWindow.document.write(boletaHTML);
            printWindow.document.close();
            printWindow.focus();

            // Espera a que la imagen se cargue antes de imprimir
            printWindow.onload = function() {
                const img = printWindow.document.querySelector('img');
                if (img.complete) {
                    printWindow.print();
                    printWindow.close();
                } else {
                    img.onload = function() {
                        printWindow.print();
                        printWindow.close();
                    };
                }
            };
        }

        function establecerFechaHoraActual() {
            const fechaInput = document.getElementById('fecha');
            const horaInput = document.getElementById('hora');
            const ahora = new Date();
            const dia = ahora.getDate().toString().padStart(2, '0');
            const mes = (ahora.getMonth() + 1).toString().padStart(2, '0');
            const año = ahora.getFullYear();
            const horas = ahora.getHours().toString().padStart(2, '0');
            const minutos = ahora.getMinutes().toString().padStart(2, '0');
            fechaInput.value = `${año}-${mes}-${dia}`;
            horaInput.value = `${horas}:${minutos}`;
        }

        window.onload = establecerFechaHoraActual;
    </script>
</body>
</html>
