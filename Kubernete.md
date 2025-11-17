
# 📘 Kubernetes y despliegue de ejercicio sencillo

---

# 🔶 Ítem 1: Definición de Kubernetes, características, aplicaciones y relación con contenedores

## 📌 ¿Qué es Kubernetes?

**Kubernetes (K8s)** es una plataforma open-source diseñada para **automatizar el despliegue, la gestión y la escalabilidad de aplicaciones en contenedores**.
Fue creada inicialmente por Google y ahora es mantenida por la **Cloud Native Computing Foundation (CNCF)**.

Es hoy el estándar más utilizado para administrar aplicaciones basadas en microservicios y contenedores.

---

## ⭐ Características principales

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

## 🚢 Aplicaciones típicas

* Aplicaciones web escalables
* Microservicios
* APIs distribuidas
* Sistemas de juegos multijugador
* Procesos de inteligencia artificial
* Sistemas con alta concurrencia

---

## 🐳 Relación entre Kubernetes y Docker

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

# 🕹️ Juego K8s – Despliegue en Kubernetes con Minikube

Este proyecto despliega un juego web dentro de un clúster de **Kubernetes** usando un **Deployment** y un **Service** tipo `LoadBalancer`.
Fue diseñado para ejecutarse en **Minikube**, pero puede trasladarse fácilmente a cualquier entorno K8s.

---

## 📦 **Tecnologías usadas**

* Kubernetes (Deployment, Service)
* Docker (imagen personalizada)
* Minikube
* Node.js / Aplicación web (puerto 80)
* PowerShell / CLI

---

## 🏗️ **Estructura del proyecto**

```
juego-k8s/
├── deployment.yaml
├── service.yaml
└── README.md
```

---

## 🚀 **Cómo desplegar el proyecto**

### 1️⃣ Clona el repositorio

```bash
git clone https://github.com/tuusuario/juego-k8s.git
cd juego-k8s
```

---

### 2️⃣ Inicia Minikube

```bash
minikube start
```

---

### 3️⃣ Aplica los manifiestos de Kubernetes

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

### 4️⃣ Verifica los recursos

**Pods:**

```bash
kubectl get pods
```

**Service:**

```bash
kubectl get svc
```

---

## 🌐 **Acceder al juego en Minikube**

Como Minikube no asigna un `EXTERNAL-IP` real a los servicios `LoadBalancer`, debes usar:

```bash
minikube service juego-service
```

Esto abrirá automáticamente la URL donde está corriendo el juego.
Si no se abre, la terminal mostrará una URL como:

```
http://192.168.49.2:XXXXX
```

---

## 🧩 **Archivos importantes**

### **deployment.yaml**

Define:

* 2 réplicas del juego
* Imagen Docker: `sosabyte21/juego-k8s:latest`
* Puerto interno del contenedor: `80`

---

### **service.yaml**

Expone la aplicación con:

* Tipo: `LoadBalancer`
* Puerto del servicio: `80`
* TargetPort del contenedor: `80`

---

## 🔧 **Comandos útiles**

Reiniciar los pods:

```bash
kubectl rollout restart deployment juego-deployment
```

Ver logs:

```bash
kubectl logs -f <nombre-del-pod>
```

Entrar al pod en shell:

```bash
kubectl exec -it <pod> -- sh
```

---

## 📷 **Captura del juego (opcional)**
![Imagen de WhatsApp 2025-11-17 a las 15 58 59_f7360b91](https://github.com/user-attachments/assets/31a8c3a8-72cf-4c30-a6c1-2cb242eede5f)
![Imagen de WhatsApp 2025-11-17 a las 16 00 37_377f51ed](https://github.com/user-attachments/assets/ca6ecb53-17a9-4d45-aa55-c1ebe81500d0)
![Imagen de WhatsApp 2025-11-17 a las 16 02 25_19b08079](https://github.com/user-attachments/assets/6ecf9387-c744-4eb5-b542-010b22da2307)

---

# ⭐ **Posibles mejoras futuras del proyecto**

A pesar de que el proyecto cumple con los objetivos principales —desplegar y ejecutar el juego en Kubernetes—, existen diversas mejoras que no se pudieron implementar por falta de tiempo, pero que aportarían robustez, escalabilidad y mejores prácticas tanto a nivel de código como de infraestructura.

---

## 🔧 **1. Mejoras en el código de la aplicación**

### ✔️ **Separación por capas**

Implementar una arquitectura más modular dividiendo:

* Controladores
* Servicios
* Rutas
* Modelos (si usa base de datos)

Esto facilita mantenimiento y escalabilidad.

### ✔️ **Manejo de errores centralizado**

Agregar un middleware global para:

* Validar datos
* Capturar errores de lógica
* Enviar respuestas consistentes al frontend

### ✔️ **Variables de entorno**

Actualmente los valores están hardcodeados.
Se podría mejorar utilizando `.env` para:

* Puertos
* Configuraciones del juego
* Parámetros de conexión

### ✔️ **Testing automatizado**

Incluir pruebas:

* Unitarias (Jest / Mocha)
* Integración
* End-to-end

Esto mejora la calidad y estabilidad del proyecto.

### ✔️ **Optimización de assets y frontend**

* Minimizar imágenes
* Hacer bundle de scripts
* Mejorar tiempos de carga

---

## 🐳 **2. Mejoras en la imagen Docker**

### ✔️ **Reducir el tamaño de la imagen**

Pasar a un multistage build o usar imágenes base más ligeras como:

```
node:18-alpine
```

Ventajas:

* Menos peso
* Despliegues más rápidos
* Menor consumo de red

### ✔️ **Agregar HEALTHCHECK**

Permite que Kubernetes detecte si el juego sigue funcionando:

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:80 || exit 1
```

---

## ☸️ **3. Mejoras en Kubernetes**

### ✔️ **Liveness & Readiness Probes**

Ejemplo:

```yaml
livenessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 5

readinessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 5
  periodSeconds: 5
```

Ayuda a que Kubernetes reinicie contenedores que no respondan.

---

### ✔️ **Agregar un Horizontal Pod Autoscaler (HPA)**

Para escalar automáticamente según el tráfico:

```bash
kubectl autoscale deployment juego-deployment --cpu-percent=50 --min=2 --max=10
```

---

### ✔️ **Agregar un Ingress Controller**

Para exponer el servicio con dominio propio y HTTPS.

Ejemplo simple:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: juego-ingress
spec:
  rules:
    - host: juego.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: juego-service
                port:
                  number: 80
```

---

### ✔️ **Agregar ConfigMaps y Secrets**

Para manejar configuraciones externas sin modificar la imagen.

---

### ✔️ **Agregar monitoreo**

Con herramientas como:

* Prometheus
* Grafana
* Metrics Server

---

## 📦 **4. Mejoras en almacenamiento y persistencia**

Si el juego genera datos (puntuaciones, usuarios, historial), se podría añadir:

* Base de datos externa (MongoDB, MySQL, Redis)
* Persistent Volume Claims
* Backups automáticos

---

## 🎮 **5. Mejoras funcionales en el juego**

### ✔️ **Sistema de puntuación más completo**

Tablas de clasificación, récords históricos, estadísticas por jugador.

### ✔️ **Modo multijugador**

Crear lobbies o partidas compartidas.

### ✔️ **Mejor feedback visual**

Animaciones, sonidos, UI más clara.

### ✔️ **Pantallas adicionales**

* Pantalla de inicio
* Tutorial
* Créditos del proyecto

### ✔️ **Internacionalización**

Soporte para varios idiomas.

---

## 🚀 **6. Integración y despliegue continuo (CI/CD)**

Con GitHub Actions:

* Validar código con linters
* Construir imagen Docker automáticamente
* Subir la imagen a Docker Hub
* Desplegar al clúster con cada push

Pipeline ejemplo:

```
push → correr tests → build Docker → push imagen → aplicar manifests → deploy
```

---


