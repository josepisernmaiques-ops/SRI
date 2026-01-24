

```markdown
# Práctica de Vídeo – FFmpeg

## 1. Objetivo de la práctica
El objetivo de esta práctica es analizar, transformar y preparar un vídeo para diferentes escenarios de distribución utilizando FFmpeg y ffprobe.  
Se realizan tareas de análisis, remuxing, recodificación y creación de perfiles de streaming.



## 2. Análisis del vídeo original

### Comando utilizado
```bash
ffprobe -v error -show_streams original.mp4
```

### Explicación
El vídeo original está codificado en:
- Vídeo: H.264, 1920×1080, 24 fps  
- Audio: AAC, 5.1 canales  
- Bitrate total aproximado: 6 Mbps  


## 3. Remuxing (MP4 → MKV)

### Comando
```bash
ffmpeg -i original.mp4 -c:v copy -c:a copy salida.mkv
```

### Explicación
- No se recodifica nada.  
- El tamaño del archivo apenas cambia.  
- La CPU apenas trabaja.  
- Solo cambia el contenedor (MP4 → MKV).  



## 4. Recodificación a H.264 (2 Mbps)

### Comando
```bash
ffmpeg -i original.mp4 -c:v libx264 -b:v 2M -c:a copy h264_2mbps.mp4
```

### Explicación
- Se reduce el bitrate a 2 Mbps.  
- La calidad baja ligeramente respecto al original.  
- El archivo pesa menos.  


## 5. Recodificación a H.265 (2 Mbps)

### Comando
```bash
ffmpeg -i original.mp4 -c:v libx265 -b:v 2M -c:a copy h265_2mbps.mp4
```

### Explicación
- H.265 ofrece mejor compresión que H.264.  
- A igual bitrate, la calidad es superior.  
- El archivo suele ser más pequeño.  


## 6. Perfil LOW (240p – 400 kbps)

### Comando
```bash
ffmpeg -i original.mp4 -vf scale=426:240 -b:v 400k -c:v libx264 -c:a aac low_240p.mp4
```

### Explicación
- Resolución reducida a 240p.  
- Bitrate muy bajo (400 kbps).  
- Ideal para móviles o conexiones lentas.  


## 7. Perfil HIGH (1080p – 2 Mbps)

### Comando
```bash
ffmpeg -i original.mp4 -vf scale=1920:1080 -b:v 2M -c:v libx264 -c:a aac high_1080p.mp4
```

### Explicación
- Mantiene resolución Full HD.  
- Bitrate moderado (2 Mbps).  
- Buena calidad para streaming estándar.  


## 8. Comprobación de los archivos generados

### Comandos
```bash
ffprobe -v error -show_streams salida.mkv
ffprobe -v error -show_streams h264_2mbps.mp4
ffprobe -v error -show_streams h265_2mbps.mp4
ffprobe -v error -show_streams low_240p.mp4
ffprobe -v error -show_streams high_1080p.mp4
```

### Explicación general
- El remux mantiene exactamente los mismos parámetros que el original.  
- H.264 a 2 Mbps reduce tamaño y calidad.  
- H.265 a 2 Mbps mantiene mejor calidad con menor tamaño.  
- El perfil LOW reduce drásticamente resolución y bitrate.  
- El perfil HIGH mantiene 1080p pero con un bitrate más bajo que el original.  

---

## 9. Preguntas finales

### ¿Cuál pesa más?
El original y el remux (salida.mkv), porque mantienen el bitrate de ~6 Mbps.

### ¿Cuál tiene más artefactos?
El H.264 a 2 Mbps.  
H.265 mantiene mejor calidad al mismo bitrate.

### ¿Cuál comprime mejor?
H.265, porque ofrece mejor calidad con menor tamaño.

### ¿Cuántas horas caben en 500 GB a 2 Mbps?
2 Mbps = 0.25 MB/s  
0.25 MB/s × 3600 = 900 MB/h  
500 000 MB / 900 MB/h ≈ **555 horas**

### ¿Cuántos usuarios simultáneos soporta una línea de 100 Mbps con perfil LOW (400 kbps)?
100 Mbps × 0.8 = 80 Mbps disponibles  
80 Mbps / 0.4 Mbps = **200 usuarios simultáneos**



