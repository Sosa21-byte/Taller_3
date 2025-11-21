

# 🦾 Simulación de Robots de SoftBank Robotics – Humanoid Gym

Despliegue en Docker | Pepper • NAO • Romeo (Dancer no disponible)

---

## 📌 **Descripción del proyecto**

Este repositorio documenta el **paso a paso** para clonar y desplegar el proyecto
**Humanoid Gym** (SoftBank Robotics) desde:

**[https://github.com/0aqz0/humanoid-gym](https://github.com/0aqz0/humanoid-gym)**

El objetivo es ejecutar simulaciones de robots humanoides dentro de un entorno **Docker**, asegurando reproducibilidad y fácil despliegue en cualquier máquina.

---

## 🤖 **Robots Simulados**

Los modelos que pueden visualizarse desde el repositorio clonado son:

* 🧠 **Pepper**
* 🦿 **NAO**
* 🛡️ **Romeo**

> ⚠️ **Nota importante:**
> El robot **Dancer no aparece ni puede simularse**, ya que **no existe ningún modelo, archivo URDF/SDF, ni referencia a él** dentro del repositorio original que clonamos.

---

# 🚀 PASO A PASO DEL DESPLIEGUE

A continuación se explica de forma clara y estética cómo realizar el proceso completo.

---

# 1️⃣ **Instalar requisitos previos**

### ✔ Git

```bash
sudo apt update && sudo apt install -y git
```

Permite clonar el repositorio.

### ✔ Docker

Instala Docker para desplegar el entorno en contenedores.

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

### ✔ (Opcional) Driver NVIDIA

Solo si vas a usar GPU:

```bash
sudo apt install nvidia-container-toolkit
```

---

# 2️⃣ **Clonar el repositorio**

```bash
git clone https://github.com/0aqz0/humanoid-gym.git
cd humanoid-gym
```

Esto descarga todo el código fuente y te ubica dentro del proyecto.

---

# 3️⃣ **Crear y activar entorno virtual (modo local)**

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

Aísla las dependencias del proyecto.

---

# 4️⃣ **Instalar dependencias del proyecto**

```bash
pip install -r requirements.txt   # si existe
pip install -e .                  # si el repo incluye setup.py
```

Aquí se instalan librerías como PyBullet, gym, etc.

---

# 5️⃣ **Ejecutar simulación (modo local)**

Cada repositorio suele incluir scripts o demos.
Generalmente se encuentran en:

```
examples/
scripts/
demo/
```

Ejemplo típico:

```bash
python examples/run_simulation.py
```

---

# 6️⃣ **Crear el Dockerfile**

Copia y pega este Dockerfile en la raíz del proyecto:

```dockerfile
FROM python:3.10-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libgl1-mesa-glx \
    xvfb \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . /app

RUN pip install --upgrade pip
RUN if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
RUN if [ -f setup.py ]; then pip install -e .; fi

RUN useradd -m appuser
USER appuser

ENTRYPOINT ["/bin/bash"]
```

---

# 7️⃣ **Construir la imagen Docker**

```bash
docker build -t humanoid-gym:latest .
```

Esto crea una imagen completamente funcional para simular los robots.

---

# 8️⃣ **Ejecutar el contenedor con soporte gráfico**

### 🔷 Para Linux con X11:

```bash
docker run -it --rm \
  -e DISPLAY=$DISPLAY \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  humanoid-gym:latest
```

Dentro del contenedor podrás ejecutar:

```bash
python examples/run_simulation.py
```

---

# 9️⃣ **Ejecución en modo Headless (sin pantalla)**

Ideal para servidores o WSL:

```bash
sudo apt install -y xvfb
Xvfb :99 -screen 0 1280x720x24 & export DISPLAY=:99
python examples/run_simulation.py --headless
```

---

# 🔟 **Docker Compose (opcional y recomendado)**

Archivo `docker-compose.yml`:

```yaml
version: "3.8"
services:
  humanoid:
    build: .
    image: humanoid-gym:latest
    environment:
      - DISPLAY=${DISPLAY}
    volumes:
      - ./:/app
      - /tmp/.X11-unix:/tmp/.X11-unix
    network_mode: host
    stdin_open: true
    tty: true
```

Ejecutar:

```bash
docker-compose up --build
```

---

# Sobre el robot “Dancer”

Durante la revisión del repositorio clonado se verificó que:

* No existe ningún archivo URDF/SDF llamado *Dancer*
* No aparece en las carpetas `/models`, `/assets`, `/robots`
* No existe ningún script que lo cargue
* No hay referencias en el código fuente a un robot llamado “Dancer”

---

# 📝 **Resultados**
![Imagen de WhatsApp 2025-11-21 a las 16 37 47_afa2d854](https://github.com/user-attachments/assets/b22aef6e-55a3-426f-8029-33e513a89672)
![Imagen de WhatsApp 2025-11-21 a las 16 37 47_4d077704](https://github.com/user-attachments/assets/2f40b272-f1ae-4084-867f-966055de3561)
![Imagen de WhatsApp 2025-11-21 a las 16 37 47_7dbe2d86](https://github.com/user-attachments/assets/62597f74-6bed-4bef-b447-1984d17fb856)
![Imagen de WhatsApp 2025-11-21 a las 16 37 47_1f5638f9](https://github.com/user-attachments/assets/fc6b095e-4df6-42b9-96f8-fcf75c93b76b)
![Imagen de WhatsApp 2025-11-21 a las 16 37 47_6ceb8af4](https://github.com/user-attachments/assets/a70cb42f-c240-44f0-afc0-0c67b640456f)



---


