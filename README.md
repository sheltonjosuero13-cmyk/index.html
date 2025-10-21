import zipfile
import os

# Crear contenido HTML
html_content = """
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Seleccionar Batido</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f5f5f5; }
    h1 { text-align: center; color: #ff6600; }
    .container { max-width: 800px; margin: auto; background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
    .option-group { display: flex; flex-wrap: wrap; gap: 20px; margin-bottom: 20px; }
    .option { text-align: center; cursor: pointer; }
    .option img { width: 100px; height: 100px; object-fit: cover; border-radius: 8px; border: 2px solid transparent; transition: border 0.3s; }
    .option.selected img { border: 2px solid #ff6600; }
    label { display: block; margin-top: 10px; font-weight: bold; }
    select, input { width: 100%; padding: 5px; margin-top: 5px; border-radius: 6px; border: 1px solid #ccc; }
    button { background: #ff6600; color: white; border: none; padding: 10px 20px; border-radius: 8px; cursor: pointer; font-size: 16px; }
    .summary { margin-top: 20px; padding: 10px; background: #ffe6cc; border-radius: 8px; }
  </style>
</head>
<body>
  <h1>Personaliza tu Batido</h1>
  <div class="container">
    <label>Elige el vaso:</label>
    <div class="option-group" id="vasos">
      <div class="option" onclick="selectOption('vaso', 'Pequeño', 'https://via.placeholder.com/100?text=Pequeño')">
        <img src="https://via.placeholder.com/100?text=Pequeño" alt="Vaso pequeño">
        <div>Pequeño</div>
      </div>
      <div class="option" onclick="selectOption('vaso', 'Mediano', 'https://via.placeholder.com/100?text=Mediano')">
        <img src="https://via.placeholder.com/100?text=Mediano" alt="Vaso mediano">
        <div>Mediano</div>
      </div>
      <div class="option" onclick="selectOption('vaso', 'Grande', 'https://via.placeholder.com/100?text=Grande')">
        <img src="https://via.placeholder.com/100?text=Grande" alt="Vaso grande">
        <div>Grande</div>
      </div>
    </div>

    <label>Elige las frutas:</label>
    <div class="option-group" id="frutas">
      <div class="option" onclick="toggleOption('fruta', 'Fresa', 'https://via.placeholder.com/100?text=Fresa')">
        <img src="https://via.placeholder.com/100?text=Fresa" alt="Fresa">
        <div>Fresa</div>
      </div>
      <div class="option" onclick="toggleOption('fruta', 'Banana', 'https://via.placeholder.com/100?text=Banana')">
        <img src="https://via.placeholder.com/100?text=Banana" alt="Banana">
        <div>Banana</div>
      </div>
      <div class="option" onclick="toggleOption('fruta', 'Mango', 'https://via.placeholder.com/100?text=Mango')">
        <img src="https://via.placeholder.com/100?text=Mango" alt="Mango">
        <div>Mango</div>
      </div>
    </div>

    <label>Cantidad:</label>
    <input type="number" id="cantidad" value="1" min="1">
    <button onclick="mostrarResumen()">Ver Resumen</button>
    <div class="summary" id="resumen"></div>
  </div>

  <script>
    let seleccion = { vaso: '', frutas: [], cantidad: 1 };
    function selectOption(tipo, nombre, imagen) {
      seleccion[tipo] = {nombre, imagen};
      document.querySelectorAll(`#${tipo === 'vaso' ? 'vasos' : 'frutas'} .option`).forEach(opt => opt.classList.remove('selected'));
      event.currentTarget.classList.add('selected');
    }
    function toggleOption(tipo, nombre, imagen) {
      let index = seleccion.frutas.findIndex(f => f.nombre === nombre);
      if (index > -1) { seleccion.frutas.splice(index, 1); event.currentTarget.classList.remove('selected'); }
      else { seleccion.frutas.push({nombre, imagen}); event.currentTarget.classList.add('selected'); }
    }
    function mostrarResumen() {
      seleccion.cantidad = document.getElementById('cantidad').value;
      let html = `<h3>Tu selección:</h3>`;
      if (seleccion.vaso) { html += `<p><strong>Vaso:</strong> ${seleccion.vaso.nombre} <img src="${seleccion.vaso.imagen}" width="50"></p>`; }
      if (seleccion.frutas.length > 0) {
        html += `<p><strong>Frutas:</strong> `;
        seleccion.frutas.forEach(f => { html += `${f.nombre} <img src="${f.imagen}" width="50"> `; });
        html += `</p>`;
      }
      html += `<p><strong>Cantidad:</strong> ${seleccion.cantidad}</p>`;
      document.getElementById('resumen').innerHTML = html;
    }
  </script>
</body>
</html>
"""

# Crear archivo ZIP
zip_filename = "/mnt/data/batidos_web.zip"
with zipfile.ZipFile(zip_filename, "w") as zipf:
    zipf.writestr("index.html", html_content)

zip_filename
