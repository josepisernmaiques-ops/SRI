---

#  **CHULETA STREAMING – TEORÍA + FÓRMULAS + MINI‑PRÁCTICA (Markdown)**

---

# # 1. Descarga directa vs Streaming

### **Descarga directa**
- Se descarga el archivo completo (ej. 100 MB).
- Aunque el usuario vea solo 2 minutos, el servidor envía **todo**.
- Se almacena localmente.

### **Streaming**
- Flujo continuo, sin guardar el archivo completo.
- El servidor solo envía lo que el usuario consume.
- Menor uso de ancho de banda.

**Ejemplo práctico:**  
Canción de 100 MB → usuario escucha 2 min:  
- Descarga: servidor envía 100 MB  
- Streaming: servidor envía solo lo reproducido (ej. 8 MB)

---

# # 2. Topologías de red

## **Unicast**
- Conexión 1 a 1.
- Si hay 100 oyentes → 100 conexiones.
- **Fórmula:**  
  \[
  BW_{total} = BW_{stream} \times N
  \]

**Ejemplo:**  
128 kbps × 100 oyentes = **12.800 kbps = 12,8 Mbps**

---

## **Multicast**
- El servidor envía **una sola copia**.
- Los routers replican solo si hay suscriptores.
- Solo funciona bien en redes internas.

**Ejemplo:**  
128 kbps a 100 oyentes → **128 kbps salen del servidor**

---

# # 3. TCP vs UDP

## **TCP**
- Fiable (retransmite paquetes).
- Más latencia.
- Pasa bien por firewalls.
- Usado por: Icecast, HLS, Netflix, Spotify, Twitch receptor.

## **UDP**
- No retransmite → mínima latencia.
- Puede perder calidad.
- Usado por: WebRTC, videollamadas, juegos, RTSP.

---

# # 4. QoS: Jitter y Buffer

## **Jitter**
- Variación en el tiempo de llegada de paquetes.
- Si supera el buffer → cortes.

## **Buffer**
- Memoria temporal para absorber jitter.
- Más buffer = más estabilidad, más retraso.

## **Burst-on-Connect**
- El servidor envía una ráfaga inicial rápida para llenar el buffer.
- Reduce el tiempo hasta que empieza a sonar.

---

# # 5. Protocolos de Streaming

## **HTTP Legacy (Icecast – ICY)**
- TCP.
- Flujo continuo.
- Formatos: MP3, OGG, AAC.
- Puertos: 80, 443, 8000.

## **HTTP Adaptativo (HLS / DASH)**
- TCP.
- Divide el vídeo en **chunks** de 2–10 s.
- Calidad adaptativa.
- Ideal para CDN.

## **Real-Time**
- **RTMP:** ingestión (OBS → servidor).  
- **RTSP:** cámaras IP (UDP + TCP).  
- **WebRTC:** videollamadas, <0.5 s de latencia.

---

# # 6. Códecs

## **Con pérdida**
- Eliminan información irrelevante.
- Ejemplo: MP3, AAC, H.264, H.265.

## **Sin pérdida**
- No eliminan información.
- Ejemplo: WAV, FLAC.

---

# # 7. Audio digital

- **Frecuencia de muestreo:** 44.1 kHz / 48 kHz  
- **Profundidad:** 16–24 bits  
- **Canales:** mono (1), estéreo (2)

---

# # 8. Cálculo de peso en audio (WAV sin comprimir)

### **Fórmula**
\[
Peso_{bits} = Frecuencia \times Bits \times Canales \times Segundos
\]

\[
Bytes = \frac{bits}{8}
\]

\[
MB = \frac{Bytes}{1.000.000}
\]

### **Ejemplo práctico**
5 min, 44.1 kHz, 16 bits, estéreo:

\[
44100 \times 16 \times 2 \times 300 = 423.360.000\ bits
\]

\[
423.360.000 / 8 = 52.920.000\ Bytes
\]

\[
52.920.000 / 1.000.000 = 52,92\ MB
\]

---

# # 9. Cálculo de bitrate en audio

### **Fórmula**
\[
Bitrate = Frecuencia \times Bits \times Canales
\]

### **Ejemplo**
48 kHz, 24 bits, estéreo:

\[
48.000 \times 24 \times 2 = 2.304.000\ bps = 2,304\ Mbps
\]

---

# # 10. Audio comprimido (MP3/AAC)

### **Fórmula**
\[
Peso = Bitrate \times Tiempo
\]

### **Ejemplo**
Canción 4 min a 128 kbps:

\[
128 \times 240 = 30.720\ kb
\]

\[
30.720 / 8 = 3.840\ kB = 3,84\ MB
\]

---

# # 11. Vídeo sin comprimir

### **Fórmula**
\[
Peso = (Ancho \times Alto) \times Profundidad \times FPS \times Tiempo
\]

---

# # 12. Vídeo comprimido

### **Fórmula**
\[
Peso = Bitrate \times Tiempo
\]

---

# # 13. Bitrates recomendados (vídeo)

| Resolución | Recomendado |
|-----------|-------------|
| 4K        | 25–45 Mbps |
| 1080p     | 6–9 Mbps |
| 720p      | 3–4 Mbps |
| 480p      | 1 Mbps |
| 360p      | 700 kbps |

---

# # 14. Usuarios simultáneos

### **Unicast**
\[
N = \frac{BW_{total}}{BW_{stream}}
\]

### **Multicast**
\[
BW_{servidor} = BW_{stream}
\]

**Ejemplo:**  
1000 Mbps / 40 Mbps = **25 usuarios**

---

# # 15. Porcentaje de uso de la línea

### **Fórmula**
\[
\% = \left( \frac{bitrate}{capacidad} \right) \times 100
\]

**Ejemplo:**  
25 Mbps en línea de 300 Mbps:

\[
25/300 \times 100 = 8,33\%
\]

---

# # 16. Conversión de unidades

## **Bits ↔ Bytes**
- 1 Byte = 8 bits  
- bits → bytes = dividir entre 8  
- bytes → bits = multiplicar por 8  

## **k, M, G**
- k = \(10^3\)  
- M = \(10^6\)  
- G = \(10^9\)

### **Regla de oro**
- Para pasar a unidad **más grande** → dividir  
- Para pasar a unidad **más pequeña** → multiplicar  

---

# # 17. Mini‑resumen final (para memorizar)

- **Unicast:** BW × usuarios  
- **Multicast:** 1 copia  
- **Peso WAV:** Freq × Bits × Canales × Segundos  
- **Bitrate:** Freq × Bits × Canales  
- **Peso comprimido:** Bitrate × Tiempo  
- **Porcentaje:** bitrate / capacidad × 100  
- **bits ↔ bytes:** divide o multiplica por 8  
- **k/M/G:** divide o multiplica por 1000  

---
