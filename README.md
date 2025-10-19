<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Registro de Trabajo</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #f2f6fc;
    margin: 0;
    padding: 15px;
    color: #333;
  }
  h1 {
    text-align: center;
    color: #1a73e8;
  }
  textarea {
    width: 100%;
    height: 250px;
    resize: vertical;
    border-radius: 8px;
    border: 1px solid #ccc;
    padding: 10px;
    font-size: 15px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }
  input, select {
    width: 100%;
    padding: 10px;
    margin: 8px 0;
    border-radius: 8px;
    border: 1px solid #ccc;
    font-size: 15px;
  }
  button {
    width: 100%;
    background: #1a73e8;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 12px;
    font-size: 16px;
    margin-top: 10px;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  button:hover {
    background: #155ab6;
  }
  label {
    font-weight: 600;
  }
  .checkbox-container {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: 8px;
    margin-top: 5px;
  }
  .checkbox-container input[type="checkbox"] {
    width: 20px;
    height: 20px;
    accent-color: #1a73e8;
    flex-shrink: 0;
  }
  .checkbox-container label {
    font-weight: 500;
    font-size: 15px;
  }
</style>
</head>
<body>
<h1>Registro de Trabajo</h1>

<textarea id="output" placeholder="Aquí aparecerá la información..." ></textarea>

<label>Hora de Entrada:</label>
<input type="time" id="entrada" step="60">

<label>Hora de Salida:</label>
<input type="time" id="salida" step="60">

<div class="checkbox-container">
  <input type="checkbox" id="diaSiguiente">
  <label for="diaSiguiente">Marcar si sales al día siguiente</label>
</div>

<label>Tiempo de Lonche (minutos):</label>
<input type="number" id="lonche" placeholder="Ej. 30">

<button onclick="calcularHoras()">Calcular Horas</button>

<label>Nombre del trabajador:</label>
<input type="text" id="nombreTrabajador" placeholder="Escribe el nombre">
<button onclick="agregarTrabajador()">Agregar Trabajador</button>

<label>Nombre de la Colonia/Casa Particular:</label>
<input type="text" id="colonia" placeholder="Casa/Colonia">
<button onclick="agregarColonia()">Agregar Colonia</button>

<label>Número de Lote/Casa:</label>
<input type="text" id="lote" placeholder="Lote o numero de casa">

<label>Actividades Realizadas:</label>
<input type="text" id="actividad" placeholder="Ej. Pintura de interior">
<button onclick="agregarActividad()">Agregar</button>

<label>Pendientes:</label>
<input type="text" id="pendientes" placeholder="Ej. Retocar puerta principal">
<button onclick="agregarPendientes()">Agregar</button>

<button style="background:#25D366" onclick="enviarWhatsApp()">Enviar x WhatsApp</button>

<script>
function formatoFecha() {
  const dias = ['Domingo','Lunes','Martes','Miércoles','Jueves','Viernes','Sábado'];
  const meses = ['Enero','Febrero','Marzo','Abril','Mayo','Junio','Julio','Agosto','Septiembre','Octubre','Noviembre','Diciembre'];
  const hoy = new Date();
  return `*${dias[hoy.getDay()]}, ${hoy.getDate()} de ${meses[hoy.getMonth()]}*`;
}

document.getElementById('output').value = formatoFecha() + "\n\n";

function formato12Horas(timeStr) {
  let [h, m] = timeStr.split(':').map(Number);
  const ampm = h >= 12 ? 'p.m.' : 'a.m.';
  h = h % 12 || 12;
  return `${h}:${m.toString().padStart(2, '0')} ${ampm}`;
}

function calcularHoras() {
  const entrada = document.getElementById('entrada').value;
  const salida = document.getElementById('salida').value;
  const lonche = parseInt(document.getElementById('lonche').value) || 0;
  const diaSig = document.getElementById('diaSiguiente').checked;
  if (!entrada || !salida) return alert('Por favor selecciona las horas.');
  
  let t1 = new Date(`2025-01-01T${entrada}`);
  let t2 = new Date(`2025-01-01T${salida}`);
  if (diaSig) t2.setDate(t2.getDate() + 1);
  let diff = (t2 - t1) / 60000 - lonche;

  const horas = Math.floor(diff / 60);
  const minutos = diff % 60;

  const texto = `Hora de Entrada: ${formato12Horas(entrada)}\nHora de Salida: ${formato12Horas(salida)}\nTiempo de lonche: ${lonche} minutos\nTiempo Laborado: ${horas} Horas ${minutos} Minutos _El tiempo de lonche ya va descontado_\n\n`;
  document.getElementById('output').value += texto;
}

function agregarTrabajador() {
  const nombre = document.getElementById('nombreTrabajador').value.trim();
  if (!nombre) return;
  document.getElementById('output').value += `Nombre del Trabajador: ${nombre}\n`;
  document.getElementById('nombreTrabajador').value = '';
}

function agregarColonia() {
  const col = document.getElementById('colonia').value.trim();
  if (!col) return;
  const area = document.getElementById('output');
  area.value += `\n*${col}*\n`;
  document.getElementById('colonia').value = '';
}

function agregarActividad() {
  const lote = document.getElementById('lote').value.trim();
  const act = document.getElementById('actividad').value.trim();
  if (!lote || !act) return;
  document.getElementById('output').value += `* ${lote} ${act}\n`;
  document.getElementById('lote').value = '';
  document.getElementById('actividad').value = '';
}

function agregarPendientes() {
  const pend = document.getElementById('pendientes').value.trim();
  if (!pend) return;
  document.getElementById('output').value += `*Pendientes:* ${pend}\n`;
  document.getElementById('pendientes').value = '';
}

function enviarWhatsApp() {
  const texto = encodeURIComponent(document.getElementById('output').value);
  const url = `https://wa.me/12517521659?text=${texto}`;
  window.open(url, '_blank');
}
</script>
</body>
</html>
