# **README.md — Práctica de Streaming de Audio (Mixxx + Icecast)**

```markdown
# Práctica de Streaming de Audio – Mixxx + Icecast

## 1. Objetivo de la práctica
Configurar un sistema completo de streaming de audio utilizando:

- Icecast como servidor de streaming  
- Mixxx como software de emisión  
- Un mountpoint personalizado (`/josep`)  
- Validación desde un navegador externo  

---

## 2. Instalación de Icecast

```bash
sudo apt update
sudo apt install icecast2
```

Durante la instalación se configuran:

- Nombre del servidor  
- Contraseña de administración  
- Contraseña de emisión (source password)  
- Puerto por defecto: **8000**

Reinicio del servicio:

```bash
sudo systemctl restart icecast2
```

Acceso al panel web:

```
http://IP_DEL_SERVIDOR:8000
```

---

## ⚙️ 3. Configuración de Icecast

Archivo principal:

```
/etc/icecast2/icecast.xml
```

Parámetros importantes:

- `<hostname>` → IP del servidor  
- `<source-password>` → contraseña usada por Mixxx  
- `<admin-password>` → contraseña del panel web  
- `<port>` → 8000  

Tras modificarlo:

```bash
sudo systemctl restart icecast2
```

---

## 🎚️ 4. Instalación de Mixxx

```bash
sudo apt install mixxx
```

---

## 📡 5. Configuración de Mixxx para emitir

En **Preferences → Live Broadcasting**:

- **Type:** Icecast  
- **Host:** IP del servidor  
- **Port:** 8000  
- **Mountpoint:** `/josep`  
- **Username:** `source`  
- **Password:** (source password de Icecast)  
- **Format:** MP3 u Ogg Vorbis  

Para iniciar la emisión:  
**Enable Live Broadcasting**

---

## 6. Validación de la emisión

### Mixxx conectado correctamente  
El icono de emisión aparece en verde.

### Icecast detecta el mountpoint  
En la web de Icecast aparece:

```
Mountpoint: /josep
```

### Reproducción desde navegador  
Desde el anfitrión:

```
http://IP_DEL_SERVIDOR:8000/josep
```

Si se escucha, la emisión funciona correctamente.

---


