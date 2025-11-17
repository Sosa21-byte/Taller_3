
# 📘 ** Kubernetes y despliegue de ejercicio sencillo**

---

# 🔶 **Ítem 1: Definición de Kubernetes, características, aplicaciones y relación con contenedores**

## 📌 **¿Qué es Kubernetes?**

**Kubernetes (K8s)** es una plataforma open-source diseñada para **automatizar el despliegue, la gestión y la escalabilidad de aplicaciones en contenedores**.
Fue creada inicialmente por Google y ahora es mantenida por la **Cloud Native Computing Foundation (CNCF)**.

Es hoy el estándar más utilizado para administrar aplicaciones basadas en microservicios y contenedores.

---

## ⭐ **Características principales**

* **Orquestación de contenedores:** administra cientos o miles de contenedores automáticamente.
* **Alta disponibilidad:** mantiene la aplicación funcionando incluso si fallan contenedores o nodos.
* **Escalabilidad automática (autoscaling):** Kubernetes puede aumentar o reducir el número de réplicas según la carga.
* **Balanceo de carga:** distribuye tráfico entre múltiples pods de forma automática.
* **Actualizaciones sin downtime (rolling updates):**
  actualiza contenedores sin detener el sistema.
* **Autorreparación (self-healing):**
  si un contenedor falla, Kubernetes lo reinicia o lo reemplaza automáticamente.
* **Estandarización:** funciona igual en la nube, on-premise o entornos híbridos.

---

## 🚢 **Aplicaciones típicas**

* Aplicaciones web escalables
* Microservicios
* APIs distribuidas
* Sistemas de juegos multijugador
* Procesos de inteligencia artificial
* Sistemas con alta concurrencia

---

## 🐳 **Relación entre Kubernetes y Docker**

Docker permite **crear y ejecutar contenedores**, pero **no** puede:

* administrar múltiples contenedores,
* repartir carga entre ellos,
* detectar fallos,
* escalar automáticamente,
* o distribuir contenedores en múltiples máquinas.

**Kubernetes llena ese vacío.**

👉 **Docker = crea contenedores**
👉 **Kubernetes = orquesta y administra esos contenedores**

> Docker y Kubernetes se complementan; no son competencia.

---

---

# 🔶 **Ítem 2: ¿Cómo crear contenedores con Docker? (Explicación paso a paso)**

A continuación se explica cómo construir un contenedor desde cero usando Docker.

---

## 1️⃣ **Crear un archivo `Dockerfile`**

Ejemplo básico de un servidor en Node.js:

```dockerfile
FROM node:18

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .
EXPOSE 3000

CMD ["node", "server.js"]
```

---

## 2️⃣ **Construir la imagen**

Desde la carpeta donde está el Dockerfile:

```bash
docker build -t mi-app .
```

---

## 3️⃣ **Verificar que la imagen fue creada**

```bash
docker images
```

---

## 4️⃣ **Ejecutar el contenedor**

```bash
docker run -p 3000:3000 mi-app
```

Esto ejecuta la aplicación dentro de un contenedor completamente aislado.

---

## ⭐ Conceptos clave involucrados

* **Imagen:** plantilla estática a partir de la cual se crean contenedores.
* **Contenedor:** instancia en ejecución de una imagen.
* **Dockerfile:** instrucciones para crear una imagen.
* **tag:** nombre/versión de la imagen.
* **registry:** lugar donde se guardan imágenes (Docker Hub, GitHub Packages, etc.).

---

---

