# 📍 QR GeoTracker - Localizador Geográfico de Códigos QR

Un sistema completo para **generar códigos QR y rastrear la ubicación geográfica exacta** en el momento en que son escaneados desde cualquier teléfono móvil o dispositivo.

---

## 🚀 ¿Cómo funciona?

1. **Generas el código QR** desde el panel de control con un identificador único (ej: `Mesa-01`, `Vehículo-Norte`, `Paquete-902`).
2. **La persona escanea el QR** con la cámara de su teléfono móvil (iPhone o Android).
3. **El navegador del teléfono solicita permiso de ubicación**:
   - **GPS de Alta Precisión**: Captura latitud, longitud y margen de error en metros.
   - **Respaldo por Red/IP**: Si el usuario no concede GPS o se accede por HTTP, detecta automáticamente la ciudad, región e IP aproximada.
   - **Geocodificación Inversa (Nominatim OSM)**: Traduce las coordenadas GPS a una dirección real en la calle (Calle, Número, Barrio, Ciudad, País).
4. **Visualización y Alertas**:
   - El usuario ve su ubicación en un mapa interactivo con botones directos para abrir en **Google Maps** o **Waze**.
   - En tu **Panel de Control** aparece el nuevo pin en el mapa global, se reproduce un timbre sonoro y se registra en la tabla de historial.
   - (Opcional) Si configuraste un **Webhook de Discord o Telegram**, recibes una alerta instantánea en tu teléfono con la dirección y el mapa.

---

## 📂 Archivos del Proyecto

| Archivo | Descripción |
| :--- | :--- |
| **`index.html`** | Aplicación web completa (Panel de Control, Generador de QR, Mapa Leaflet, Modo Escáner). |
| **`iniciar.bat`** | Lanzador de 1 solo clic para Windows. Inicia el servidor nativo. |
| **`server.ps1`** | Servidor web nativo en PowerShell (no requiere instalar nada en Windows). |
| **`server.js`** | Servidor alternativo en Node.js (cero dependencias externas). |
| **`server.py`** | Servidor alternativo en Python (cero pip install). |
| **`scans.json`** | Base de datos local en formato JSON donde se guardan los escaneos. |

---

## ⚡ Inicio Rápido

### Método 1: Con 1 solo clic (Recomendado en Windows)
Haz doble clic sobre el archivo **`iniciar.bat`**.
Se abrirá la consola indicando que el servidor está listo en:
- **Local:** `http://localhost:8080`
- **Red Wi-Fi:** `http://192.168.x.x:8080`

### Método 2: Con Python o Node.js (Opcional)
Si prefieres usar Python o Node:
```powershell
# Con Python:
python server.py

# Con Node.js:
node server.js
```

---

## 📱 Cómo Escanear desde un Celular (Requisito de HTTPS y GPS)

> ⚠️ **Importante sobre los navegadores móviles:**
> Por motivos de seguridad y privacidad, los navegadores de celulares (**Safari en iPhone** y **Chrome en Android**) **bloquean el GPS de alta precisión en enlaces `http://`** (solo lo permiten en `localhost` o bajo **`https://`**).
> Si alguien escanea el código mediante `http://192.168.x.x:8080`, el sistema funcionará pero usará **Geolocalización por Red IP**.

### ¿Cómo tener HTTPS gratuito para que el GPS funcione al 100%?

Tienes varias opciones muy sencillas y gratuitas:

#### Opción A: Con Cloudflare Tunnel (Gratuito, sin registro)
Descarga Cloudflare Tunnel o ejecútalo con un comando:
```powershell
cloudflared tunnel --url http://localhost:8080
```
Te dará una URL segura como `https://xyz-random.trycloudflare.com`.

#### Opción B: Con ngrok (Gratuito)
```powershell
ngrok http 8080
```
Copia la URL `https://xxxx.ngrok-free.app` que te entrega en la consola.

#### Opción C: Con localtunnel (vía npx)
```powershell
npx localtunnel --port 8080
```

#### Opción D: Subir `index.html` gratis a GitHub Pages o Vercel
Dado que `index.html` funciona de forma completamente autónoma (almacena en el navegador y permite sincronizar con webhooks y exportar datos), puedes subir este archivo a **GitHub Pages**, **Vercel** o **Netlify**. Te dará una URL HTTPS permanente y gratuita donde podrás generar tus QR y recibir escaneos desde cualquier parte del mundo.

---

## 🛠️ Funcionalidades del Panel de Control

### 1. Mapa en Vivo
- Mapa interactivo mundial (OpenStreetMap) con pines diferenciados por precisión (GPS de alta precisión en azul, precisión media en amarillo, red IP en gris).
- Popups con dirección exacta, fecha y hora, coordenadas y enlace directo a Google Maps.
- Filtro por identificador de QR para ver únicamente los escaneos de un QR específico.

### 2. Generador de Códigos QR
- Personaliza el nombre/ID del QR (ej: `Mesa-Exterior`, `Caja-Fuerte`, `Vehiculo-Reparto`).
- Personaliza los colores del código QR y del fondo.
- Vista previa en tiempo real.
- **Descargar imagen PNG en alta resolución**: Lista para imprimir o insertar en documentos.
- **Imprimir Cartel**: Ficha diseñada y formateada para imprimir directamente con la impresora.

### 3. Registro e Historial Detallado
- Tabla con fecha y hora, ID de QR, dirección real formateada, coordenadas, precisión y dispositivo del usuario.
- Buscador en tiempo real por texto y filtros por QR.
- **Exportar a CSV (Excel)**: Descarga todos los escaneos en formato de hoja de cálculo.
- **Exportar a JSON**: Copia de seguridad completa de todos los datos.
- Vista individual de cualquier escaneo en modal con mapa ampliado.

### 4. Alertas Instantáneas & Webhooks
- **Discord Webhook**: Configura la URL de un canal de Discord para recibir una notificación push en tu teléfono con la foto del mapa, dirección y coordenadas cada vez que alguien escanee el código.
- **Alarma sonora**: Timbre musical con Web Audio API que suena en el panel cuando ingresa un nuevo escaneo.
- **Auto-refresco**: Sincronización automática cada 5 segundos.

### 5. Simulador de Pruebas
- Botón **"Probar Escaneo Aquí"** en la barra superior para verificar el funcionamiento del GPS y geocodificación inversa directamente desde tu equipo sin necesidad de imprimir el código.
