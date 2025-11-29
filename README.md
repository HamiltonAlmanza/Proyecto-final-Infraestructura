## I. Objetivo del Proyecto
El objetivo principal de este proyecto es crear un sistema que realice respaldos de manera automática, evitando que la información se pierda si ocurre algún problema en el servidor.  
La idea es que el sistema trabaje solo, sin depender de una persona para hacer copias manuales, y que la información esté siempre disponible cuando se necesite.  
En pocas palabras, lo que buscamos es tener un sistema más seguro, más confiable y capaz de recuperarse rápidamente ante una falla.

---

## II. Justificación e Importancia
Este proyecto es importante porque ayuda a que la información y los servicios no se pierdan ni se detengan ante una falla, un apagón o un error del sistema.  
En una infraestructura universitaria o empresarial, la pérdida de datos puede afectar clases, trámites, investigaciones o incluso procesos administrativos completos, por lo que contar con respaldos automáticos y monitoreo constante se vuelve indispensable.  
La implementación de este sistema reduce el riesgo de que todo dependa de un solo servidor (el famoso Single Point of Failure) y mejora la continuidad operacional, ya que aunque uno falle, la información sigue estando disponible gracias a los respaldos y al monitoreo.  
Grafana además permite ver el estado de los equipos en tiempo real, lo que hace más rápido detectar y solucionar fallos antes de que se vuelvan un problema mayor.

---

## III. Tecnologías y Conceptos Implementados

### 3.1 Tecnologías Clave Utilizadas

| Tecnología | Rol dentro del proyecto |
|---|---|
| **Nginx (Tecnología 1)** | Se usó como punto central para recibir solicitudes, servir como proxy y mejorar la forma en que se entregan los servicios. |
| **PostgreSQL (Tecnología 2)** | Base de datos principal con estructura Maestro–Esclavo para mantener una copia idéntica de la información y protegerla contra fallos. |
| **Bash/Scripting automático (Tecnología 4)** | Se utilizaron scripts para generar respaldos sin depender de una persona, lo que hace que el sistema trabaje solo. |
| **Grafana (Tecnología 5)** | Permite ver gráficas y métricas del sistema, como consumo de recursos, estado del servidor y actividad en tiempo real. |

> En conjunto, estas tecnologías permiten crear un entorno con respaldos automáticos y monitoreo constante, ideal para una universidad donde la información no puede perderse.

---

### 3.2 Temas de la Asignatura Aplicados (T1 - T6)

| Tema | Aplicación en el sistema de copias de seguridad para la universidad |
|---|---|
| 🟢 **Continuidad Operacional (T1)** | Los respaldos automáticos permiten recuperar información en caso de fallas, evitando detener trámites, clases o información institucional. |
| 🟢 **Alta Disponibilidad y Tolerancia a Fallos (T2)** | Con la réplica Maestro–Esclavo en PostgreSQL, los datos existen en dos servidores, reduciendo el riesgo de perderlos completamente. |
| 🟢 **Balanceo y Proxy (T3)** | Nginx ayuda a gestionar solicitudes, y el sistema puede escalar a más instancias si en un futuro la universidad lo necesita. |
| 🟢 **Optimización y Servicios Web (T3/T4)** | Los servicios fueron organizados para que respondan de manera más fluida, permitiendo que el sistema crezca con el tiempo. |
| 🟢 **Seguridad y Hardening (T5)** | Se configuraron reglas de firewall (UFW) para limitar el acceso solo a los puertos necesarios y proteger la información sensible. |
| 🟢 **Automatización (T6)** | Los respaldos se ejecutan solos con scripts Bash, reduciendo trabajo manual y evitando errores humanos. |
| 🟢 **Monitoreo de Infraestructura (T4/T1)** | Grafana muestra en gráficos el estado del sistema, permitiendo detectar problemas antes de que afecten a estudiantes o personal administrativo. |

---

### 4.1 Diseño Esquemático

| VM/Host | Rol | IP Física | IP Virtual | Red Lógica | SO |
|---|---|---|---|---|---|
| VM-WEB1 | Servidor Web 1 | 172.20.10.4 | N/A | Red 10 | Ubuntu 24.04 |
| VM-WEB2 | Servidor Web 2 | 172.20.10.10 | N/A | Red 10 | Ubuntu 24.04 |
| VM-BAL | Balanceador / Punto de Entrada | 172.20.10.5 | N/A | Red 15 | Ubuntu 24.04 |
| VM-GRAFANA | Monitoreo (Grafana) | (pendiente) | N/A | Red 15 | Ubuntu 24.04 |
| VM-DB-M | Base de Datos Maestro | 172.20.10.6 | N/A | Red 20 | Ubuntu 24.04 |
| VM-DB-S | Base de Datos Esclavo | 172.20.10.7 | N/A | Red 20 | Ubuntu 24.04 |
| VM-BACKUP | Servidor de Copias | (pendiente) | N/A | Red 25 | Ubuntu 24.04 |

---

### 4.2 Estrategia Adoptada

**Estrategia de Replicación:**  
Se optó por la replicación asíncrona de Postgres debido a la menor latencia, priorizando la separación de lectura/escritura para mantener mayor estabilidad entre conexión y peticiones a la base de datos evitando que se sobrecargue.

**Estrategia de Hardening:**  
Se aplicaron los estándares CIS de hardening para mantener una mayor seguridad en los sitios web evitando accesos no deseados mediante conexiones SSH, protegiendo de esa manera los servidores, también aplicando protección a los diferentes puertos mediante el firewall para una mayor seguridad.

---

## 5. Guía de Implementación y Puesta en Marcha

### 5.1 Pre-requisitos

- 7 VMs con Ubuntu 24.04 y acceso root/sudo con SSH activo.  
- Repositorio Git clonado en cada VM.

---

### 5.2 Despliegue

**Instalación:**
- Instalar en 3 VMs la base de datos Postgres.
- Instalar en 2 VMs servidores con Node.js.
- Instalar en 1 VM el balanceador de cargas con Nginx.
- Instalar en 1 VM Prometheus + Grafana.

**Configuración:**  
Ver los scripts (los que están con barras).

**Ejecución (opcional):**
```bash
sudo ./script.sh
```

---

### 5.3. Ficheros de Configuración Clave

Ver los scripts (son los que están con barras).

Incluir además los archivos de configuración y software a utilizar dentro del proyecto, organizados en carpetas.  
(Ver lo que está dentro de los comandos con barras).


