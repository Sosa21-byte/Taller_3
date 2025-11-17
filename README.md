
# 📘 **Segmentador de Equipos Electrónicos – Streamlit + MediaPipe + Docker**

Este proyecto implementa un sistema de **segmentación en vivo** usando:

* **MediaPipe Selfie Segmentation**
* **Streamlit**
* **Python 3.10**
* **Docker**

El sistema abre la **cámara del navegador** y segmenta en azul la imagen capturada.
Adicionalmente, etiqueta el objeto mostrado frente a la cámara como:

* Multímetro
* Osciloscopio
* Raspberry Pi
* Usuario

(la clasificación es simulada según requerimientos del laboratorio).

---

# 📂 **Estructura del Proyecto**

```
segmentador_equipos/
│
├── app.py
├── requirements.txt
└── Dockerfile
```

---

# 🧠 **1. Archivo `app.py`**

```python
import streamlit as st
import cv2
import mediapipe as mp
import numpy as np
from PIL import Image
import io

st.title("Segmentador en Vivo de Equipos Electrónicos")

st.write("Activa la cámara y coloca un multímetro, osciloscopio o Raspberry Pi frente a ella.")

# Inicializar MediaPipe
mp_seg = mp.solutions.selfie_segmentation
segmentador = mp_seg.SelfieSegmentation(model_selection=1)

# Simulación de clasificación
CLASES = {
    "multimetro": "Multímetro Detectado",
    "osciloscopio": "Osciloscopio Detectado",
    "raspberry": "Raspberry Pi Detectada"
}

# Entrada por cámara del navegador
frame = st.camera_input("Activar cámara")

if frame:
    # Convertir imagen recibida
    img_bytes = frame.getvalue()
    image = Image.open(io.BytesIO(img_bytes)).convert("RGB")
    image_np = np.array(image)

    # Procesar segmentación
    resultado = segmentador.process(image_np)
    mask = resultado.segmentation_mask
    mask_uint8 = (mask * 255).astype(np.uint8)

    # Crear tinte azul
    blue = np.zeros_like(image_np)
    blue[:, :, 2] = 255

    mask_3c = cv2.cvtColor(mask_uint8, cv2.COLOR_GRAY2BGR)
    mask_norm = mask_3c.astype(float) / 255

    # Mezclar manualmente
    segmented = image_np * (1 - 0.4 * mask_norm) + blue * (0.4 * mask_norm)
    segmented = segmented.astype(np.uint8)

    # Simular clasificación
    etiqueta = np.random.choice(list(CLASES.keys()))

    st.image(segmented, caption="Imagen Segmentada", use_column_width=True)

    st.subheader(f"Etiqueta Detectada: {CLASES[etiqueta]}")
```

---

# 🧾 **2. Archivo `requirements.txt`**

```
streamlit==1.36.0
mediapipe==0.10.14
opencv-python-headless==4.8.1.78
numpy==1.26.4
Pillow==10.2.0
```

---

# 🐳 **3. Archivo `Dockerfile`**

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

# 🚀 **Cómo Ejecutarlo**

## ✅ 1. Construir la imagen

En la carpeta del proyecto:

```
docker build -t segmentador:v1 .
```

## ✅ 2. Ejecutar el contenedor

```
docker run -p 8501:8501 segmentador:v1
```

## ✅ 3. Abrir la aplicación

Ir al navegador y abrir:

```
http://localhost:8501
```

Permitir acceso a la cámara del navegador cuando Streamlit lo solicite.

---

# 🎥 **Cómo usarlo**

1. Marca la casilla **Activar cámara**
2. Permite acceso a la cámara del navegador
3. Coloca frente a la cámara:

   * un **multímetro**,
   * una **raspberry**,
   * o un **osciloscopio** (puede ser la foto en tu celular)
   * incluso el **Usuario**
4. El sistema:

   * Segmenta en azul
   * Muestra la etiqueta detectada

---


# 🎉 Resultados

![Imagen de WhatsApp 2025-11-16 a las 23 14 53_7906c83d](https://github.com/user-attachments/assets/4bd3f743-0d8b-4a33-b509-76ddf4cf75f5)
![Imagen de WhatsApp 2025-11-16 a las 23 15 54_2e743dfe](https://github.com/user-attachments/assets/40feefc4-4146-43be-9ab8-9fe82a489d1d)
![Imagen de WhatsApp 2025-11-16 a las 23 17 14_e6aec084](https://github.com/user-attachments/assets/77a9c15c-9d0a-4a50-9cc1-32fd00911232)

