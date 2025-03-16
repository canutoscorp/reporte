<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Trabajo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 90%;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        }
        input, button, textarea {
            width: 100%;
            margin: 5px 0;
            padding: 10px;
            font-size: 1em;
        }
        textarea {
            resize: none;
        }
        button {
            background-color: #007BFF;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .output {
            white-space: pre-line;
            margin-top: 20px;
            padding: 10px;
            background: #e9ecef;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Registro de Trabajo</h2>
        <textarea id="output" rows="10" readonly></textarea>
        <label>Hora de Entrada:</label>
        <input type="time" id="horaEntrada" />
        <label>Hora de Salida:</label>
        <input type="time" id="horaSalida" />
        <label>Tiempo de Lonche (minutos):</label>
        <input type="number" id="tiempoLonche" />
        <button onclick="calcularHoras()">Calcular Horas</button>
        <label>Nombre de Trabajador:</label>
        <input type="text" id="nombreTrabajador" />
        <button onclick="agregarTrabajador()">Agregar</button>
        <label>Nombre de la Colonia/Casa Particular:</label>
        <input type="text" id="colonia" />
        <button onclick="agregarColonia()">Agregar</button>
        <label>Número de Lote/Casa:</label>
        <input type="text" id="lote" />
        <label>Actividades Realizadas:</label>
        <input type="text" id="actividad" />
        <button onclick="agregarLoteActividad()">Agregar</button>
        <label>Pendientes:</label>
        <input type="text" id="pendientes" />
        <button onclick="agregarPendientes()">Agregar</button>
        <button onclick="enviarWhatsApp()">Enviar por WhatsApp</button>
    </div>
    <script>
        document.getElementById("output").value = new Date().toLocaleDateString("es-ES", { weekday: 'long', day: 'numeric', month: 'long' });
        
        function calcularHoras() {
            let entrada = document.getElementById("horaEntrada").value;
            let salida = document.getElementById("horaSalida").value;
            let lonche = parseInt(document.getElementById("tiempoLonche").value) || 0;
            if (!entrada || !salida) return;
            
            let [hE, mE] = entrada.split(':').map(Number);
            let [hS, mS] = salida.split(':').map(Number);
            
            let minutosEntrada = hE * 60 + mE;
            let minutosSalida = hS * 60 + mS;
            let minutosTrabajados = minutosSalida - minutosEntrada - lonche;
            
            let horas = Math.floor(minutosTrabajados / 60);
            let minutos = minutosTrabajados % 60;
            
            document.getElementById("output").value += `\nHora de entrada: ${entrada}\nHora de salida: ${salida}\nTiempo de Lonche: ${lonche} minutos\nTiempo Laborado: ${horas} horas ${minutos} minutos\n`;
        }
        
        function agregarTrabajador() {
            let nombre = document.getElementById("nombreTrabajador").value;
            if (nombre) {
                document.getElementById("output").value += `\n* ${nombre}`;
                document.getElementById("nombreTrabajador").value = "";
            }
        }
        
        function agregarColonia() {
            let colonia = document.getElementById("colonia").value;
            if (colonia) {
                document.getElementById("output").value += `\n\n*${colonia}*`;
                document.getElementById("colonia").value = "";
            }
        }
        
        function agregarLoteActividad() {
            let lote = document.getElementById("lote").value;
            let actividad = document.getElementById("actividad").value;
            if (lote && actividad) {
                document.getElementById("output").value += `\n* Número de Lote/Casa ${lote} ${actividad}`;
                document.getElementById("lote").value = "";
                document.getElementById("actividad").value = "";
            }
        }
        
        function agregarPendientes() {
            let pendientes = document.getElementById("pendientes").value;
            if (pendientes) {
                document.getElementById("output").value += `\n*Pendientes:* ${pendientes}`;
                document.getElementById("pendientes").value = "";
            }
        }
        
        function enviarWhatsApp() {
            let telefono = "12517521659"; // Número de WhatsApp a enviar
            let mensaje = encodeURIComponent(document.getElementById("output").value);
            let url = `https://wa.me/${telefono}?text=${mensaje}`;
            window.open(url, "_blank");
        }
    </script>
</body>
</html>
