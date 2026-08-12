# Checador Corporativo (Prototipo)

Aplicacion web para registrar entrada y salida de empleados, validando que
esten dentro de un radio de 100 metros de las coordenadas del corporativo
(19.4925, -99.2564), calculando horas trabajadas, retardos (tolerancia 10 min),
faltas y tiempo faltante de jornada. Cada empleado puede tener un horario
distinto (soporta los 3 horarios que manejan).

## Como desplegarlo en Render (gratis)

1. Crea una cuenta en render.com (gratis, no pide tarjeta para el plan free).
2. Sube esta carpeta completa a un repositorio de GitHub (puede ser privado).
3. En Render: "New +" -> "Web Service" -> conecta tu repositorio.
4. Render detectara automaticamente el archivo render.yaml y el Procfile.
   Si te pide configurarlo manualmente:
   - Build Command: pip install -r requirements.txt
   - Start Command: gunicorn app:app
5. Da clic en "Create Web Service". En unos minutos te entrega una URL
   publica (ej. https://checador-corporativo.onrender.com) que ya puedes
   compartir con tu equipo desde celular o computadora.

IMPORTANTE antes de compartir la URL con tu equipo:
- Cambia app.secret_key y ADMIN_PASSWORD dentro de app.py.
- La base de datos SQLite (checador.db) en el plan gratuito de Render
  se reinicia si el servicio se "duerme" por inactividad; para 25 personas
  y uso diario esto normalmente no es un problema, pero si necesitas
  persistencia garantizada a largo plazo, se recomienda un plan pagado
  con disco persistente o migrar a una base de datos externa (Postgres).

## Como correrlo en tu propia computadora / servidor (para pruebas locales)

1. Instala Python 3.9 o superior.
2. Abre una terminal dentro de la carpeta "checador_app".
3. (Opcional) Crea un entorno virtual:
   python -m venv venv
   Windows: venv\\Scripts\\activate
   Mac/Linux: source venv/bin/activate
4. Instala dependencias:
   pip install -r requirements.txt
5. Ejecuta:
   python app.py
6. Abre en el navegador: http://localhost:5000

## Carga de empleados

Se incluyen por separado:
- empleados_pins.csv: los 23 empleados con su PIN asignado (1001-1023).
- plantilla_carga_empleados.csv: mismo listado con columnas vacias para
  llenar el horario de cada persona (Hora_Entrada, Hora_Salida, Jornada_Horas).
- cargar_empleados.py: script que lee el CSV lleno y da de alta/actualiza
  automaticamente a todos los empleados en la base de datos.

Uso del script (despues de llenar la plantilla con los 3 horarios reales):
   python cargar_empleados.py plantilla_carga_empleados.csv

## Notas de seguridad antes de usarlo con datos reales

- Cambia app.secret_key y ADMIN_PASSWORD dentro de app.py.
- Si se aloja en internet publico, usa siempre HTTPS (Render lo da por
  defecto en su URL .onrender.com).
- Considera respaldar periodicamente el archivo checador.db.
- La precision del GPS en celulares suele ser de 5 a 20 metros, por eso
  se configuro un radio de tolerancia de 100 metros alrededor del corporativo.

## Personalizacion pendiente

- Reemplazar los recuadros "ESPACIO PARA LOGO DE LA EMPRESA" en los archivos
  dentro de templates/ por el logo real de la empresa.
- Ajustar colores en static/style.css si se requiere otra paleta.
