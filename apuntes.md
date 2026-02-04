---

# # Apuntes de Streaming – 2º ASIX / ASIR**  

---

# ## **1. Descarga directa vs Streaming**

### **Descarga directa**
- El usuario solicita un fichero completo (ej. 100 MB, 10 minutos).  
- Se descarga entero antes o durante la reproducción.  
- Aunque el usuario solo vea 2 minutos, **el servidor entrega los 100 MB completos**.  
- Se almacena localmente.

### **Streaming**
- Datos enviados en flujo constante.  
- No hay almacenamiento permanente.  
- Solo se consume el ancho de banda equivalente al tiempo reproducido.  
- Si el usuario ve 2 minutos, el servidor solo envía esos 2 minutos.

---

# ## **2. Topología de red**

### **Unicast**
- Conexión **1 a 1** (modelo estándar de Internet).  
- Si hay 100 oyentes, el servidor abre **100 sockets TCP** y envía el flujo 100 veces.  
- **Cálculo de ancho de banda:**

\[
BW_{total} = BW_{stream} \times N_{usuarios}
\]

- Desventaja: **poco escalable**.

### **Multicast**
- El servidor envía a una **dirección multicast** (224.0.0.0 – 239.255.255.255).  
- Los routers replican el paquete solo si hay suscriptores.  
- Desventaja: muchos routers **bloquean multicast** → solo viable en redes internas.

### **Broadcast**
- Envío a todos los dispositivos de la red local.  
- No se usa para streaming en Internet.

---

# ## **3. Capa de transporte: TCP vs UDP**

### **TCP**
- Si un paquete se pierde, el servidor lo reenvía (ACK/NACK).  
- Ventajas:
  - Calidad garantizada.  
  - Pasa firewalls, NAT y proxies sin problemas.  
- Desventajas:
  - **Alta latencia** por retransmisiones.

### **UDP**
- No hay retransmisión.  
- Ventajas:
  - **Latencia mínima**.  
- Desventajas:
  - Pérdida de calidad.  
  - Problemas con NAT/firewalls.

---

# ## **4. QoS: Jitter y Buffer**

### **Jitter**
Variación en el tiempo de llegada de los paquetes.

Ejemplo:
- Paquete 1 → 20 ms  
- Paquete 2 → 150 ms  
- Paquete 3 → 20 ms  

Si el jitter supera el tamaño del buffer → **cortes (buffer underrun)**.

### **Buffer**
Memoria temporal en cliente/servidor.

- Función: absorber jitter.  
- A mayor buffer → más estabilidad pero **más latencia**.

### **Burst-on-Connect (Icecast)**
- Problema: llenar el buffer inicial tardaría varios segundos.  
- Solución: el servidor envía los primeros KB a **máxima velocidad** (ej. 10×).  
- Resultado: reproducción casi instantánea.

---

# ## **5. Protocolos de Streaming**

## **5.1 Capa de transporte**
- TCP → calidad, latencia alta.  
- UDP → latencia baja, pérdida de calidad.

---

## **5.2 Capa de aplicación (3 modelos)**

### **1. HTTP Legacy (Icecast2)**
- Protocolo: **ICY**  
- Conexión TCP continua.  
- Puertos: 80, 443, 8000  
- Formatos: MP3, OGG, AAC  
- Flujo continuo de bytes.

### **2. HTTP Adaptativo**
- Protocolos: **HLS**, **MPEG-DASH**  
- El servidor trocea el vídeo en **chunks** de 2–10 s.  
- Formatos: .ts, .m4s  
- Ventaja: **calidad adaptativa** mediante manifest.

### **3. Real-Time**
- **RTMP**: TCP, usado para enviar vídeo desde OBS a YouTube/Twitch.  
- **RTSP**: cámaras IP, usa UDP para datos y TCP para control.  
- **WebRTC**: videoconferencia, P2P, UDP, <0.5 s de latencia.

---

# ## **6. Cuadro resumen de protocolos**

| Protocolo | Base | Latencia | Uso | Firewall | Caché CDN |
|----------|------|----------|------|----------|-----------|
| Icecast (ICY) | TCP/HTTP | 10–30 s | Radio online | Muy fácil | Difícil |
| HLS / DASH | TCP/HTTP | 15–45 s | Netflix, YouTube | Muy fácil | Excelente |
| RTMP | TCP | 2–5 s | Ingesta (OBS → servidor) | Medio | No |
| WebRTC | UDP/TCP | <0.5 s | Videoconferencia | Complejo | No |
| RTSP | UDP+TCP | <1 s | Cámaras IP | Problemas NAT | No |

---

# ## **7. Icecast2**

- Servidor de streaming de código abierto.  
- Recibe audio de una fuente (Mixxx, Butt) y lo distribuye a oyentes.  
- No genera contenido, solo lo retransmite.  
- Formatos: MP3, OGG.  
- Usa **mountpoints** (ej. /radio).

### Instalación
```
apt update
apt install icecast2
```

### Configuración
- Puerto: 8000  
- Contraseñas: source-password, admin-password  
- Archivo: `icecast.xml`

---

# ## **8. Mixxx (emisor)**

Instalación:
```
add-apt-repository ppa:mixxx/mixxx
apt update
apt install mixxx
```

Configuración de emisión:
- Tipo: Icecast2  
- Montaje: /manu  
- Servidor: 127.0.0.1  
- Puerto: 8000  
- Usuario: source  
- Contraseña: (la configurada)

---

# ## **9. Códecs**

### ¿Qué es un códec?
Algoritmo que **comprime y descomprime** audio/vídeo.

### Ejemplos
- Audio: MP3, AAC, Vorbis, WAV  
- Vídeo: H.264, H.265, AV1  

### ¿Por qué?
- Reducir tamaño sin perder calidad perceptible.

---

# ## **10. Audio digital**

### **Frecuencia de muestreo**
- “Fotos” por segundo de la onda.  
- Estándar: **44.1 kHz**.

### **Profundidad de bits**
- Calidad de cada muestra.  
- Estándar: **16 bits** (CD).

### **Canales**
- Mono, estéreo, 5.1, etc.

---

# ## **11. Códecs con pérdida / sin pérdida**

### Con pérdida
- Eliminan información irrelevante.  
- Ejemplo: MP3.

### Sin pérdida
- Igual que un ZIP: no se pierde información.  
- Ejemplo: FLAC, WAV.

---

# ## **12. Cálculo de peso (audio)**

### Fórmula:
\[
Peso = Frecuencia \times Bits \times Canales \times Segundos
\]

Ejemplo del PDF:
\[
44100 \times 16 \times 2 \times 180 = 254016000\ \text{bits}
\]

---

# ## **13. Cálculo de peso (vídeo)**

### Sin comprimir:
\[
Peso = (Ancho \times Alto) \times Profundidad \times FPS \times Tiempo
\]

### Con códec:
\[
Peso = Bitrate \times Tiempo
\]

---

# ## **14. Bitrates recomendados (vídeo)**

| Resolución | Calidad | Mínimo | Recomendado |
|------------|---------|--------|-------------|
| 4K | Ultra HD | 15 Mbps | 25–45 Mbps |
| 1080p | Alta | 4 Mbps | 6–9 Mbps |
| 720p | Media | 1.5 Mbps | 3–4 Mbps |
| 480p | SD | 500 kbps | 1 Mbps |
| 360p | Baja | 400 kbps | 700 kbps |

---

# ## **15. Ejercicios del PDF**

Incluidos:

- Cálculo de bitrate RAW  
- Cálculo de almacenamiento  
- Porcentaje de uso de línea  
- Saturación de red  
- Número de oyentes  
- Comparación de códecs  
- Simulación de perfiles de streaming

---

# ## **16. Contenedores de vídeo**

Incluyen:
- Pistas de vídeo  
- Pistas de audio  
- Subtítulos  
- Metadatos  

Ejemplos: MP4, MKV, MOV, OGG.

---

# ## **17. FFmpeg (práctica)**

### Remuxing (cambiar contenedor sin recodificar)
```
ffmpeg -i original.mp4 -c:v copy -c:a copy salida.mkv
```

### Re-encode H.264 y H.265
```
ffmpeg -i original.mp4 -c:v libx264 -b:v 2M -c:a copy h264.mp4
ffmpeg -i original.mp4 -c:v libx265 -b:v 2M -c:a copy h265.mp4
```

---

# ## **18. Preguntas finales del PDF**

- ¿Cuántas horas de vídeo HD (2 Mbps) caben en 500 GB?  
- ¿Cuántos usuarios pueden ver 400 kbps antes de saturar el 80% de 100 Mbps?

---

