# 🚀 Proyecto Final SIS313: Sistema distribuido de respaldos automáticos

> **Asignatura:** SIS313: Infraestructura, Plataformas Tecnológicas y Redes  
> **Semestre:** 2/2025  
> **Docente:** Ing. Marcelo Quispe Ortega  

---

## 👥 Miembros del Equipo (Grupo)

| Nombre Completo | Rol en el Proyecto | Contacto (GitHub / Email) |
|-----------------|--------------------|----------------------------|
| **David Flores Padilla** | Base de Datos Maestro y Esclavo | [DavidFloresPadilla](https://github.com/DavidFloresPadilla) · elflores43@gmail.com |
| **Hamilton Randall Almanza Castro** | Servidor de Backups | [HamiltonAlmanza](https://github.com/HamiltonAlmanza) · hac.0000009@gmail.com |
| **Cristhian Daniel Mamani Morales** | Servidor Web 1 y Servidor Web 2 | [danielecrac736](https://github.com/danielecrac736) · danielelcrac736@gmail.com |
| **Mateo Gabriel Villegas Arancibia** | Balanceador  | [MatVan23](https://github.com/MatVan23) · mateorito1@gmail.com |

## I. Objetivo del Proyecto
El objetivo principal de este proyecto es crear un sistema que realice respaldos de manera automática, evitando que la información se pierda si ocurre algún problema en el servidor.  
La idea es que el sistema trabaje solo, sin depender de una persona para hacer copias manuales, y que la información esté siempre disponible cuando se necesite.  
En pocas palabras, lo que buscamos es tener un sistema más seguro, más confiable y capaz de recuperarse rápidamente ante una falla.

---

## II. Justificación e Importancia
Este proyecto es importante porque ayuda a que la información y los servicios no se pierdan ni se detengan ante una falla, un apagón o un error del sistema.  
En una infraestructura universitaria o empresarial, la pérdida de datos puede afectar clases, trámites, investigaciones o incluso procesos administrativos completos, por lo que contar con respaldos automáticos y monitoreo constante se vuelve indispensable.  
La implementación de este sistema reduce el riesgo de que todo dependa de un solo servidor (el famoso Single Point of Failure) y mejora la continuidad operacional, ya que aunque uno falle, la información sigue estando disponible gracias a los respaldos y al monitoreo.  

---

## III. Tecnologías y Conceptos Implementados

### 3.1 Tecnologías Clave Utilizadas

| Tecnología | Rol dentro del proyecto |
|---|---|
| **Nginx (Tecnología 1)** | Se usó como punto central para recibir solicitudes, servir como proxy y mejorar la forma en que se entregan los servicios. |
| **PostgreSQL (Tecnología 2)** | Base de datos principal con estructura Maestro–Esclavo para mantener una copia idéntica de la información y protegerla contra fallos. |
| **Bash/Scripting automático (Tecnología 4)** | Se utilizaron scripts para generar respaldos sin depender de una persona, lo que hace que el sistema trabaje solo. |

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

---

## III. IV. Diseño de la Infraestructura y Topología

### 4.1 Diseño Esquemático

| VM/Host | Rol | IP Física | IP Virtual | Red Lógica | SO |
|---|---|---|---|---|---|
| VM-WEB1 | Servidor Web 1 | 172.20.10.4 | N/A | Red 10 | Ubuntu 24.04 |
| VM-WEB2 | Servidor Web 2 | 172.20.10.10 | N/A | Red 10 | Ubuntu 24.04 |
| VM-BAL | Balanceador / Punto de Entrada | 172.20.10.5 | N/A | Red 15 | Ubuntu 24.04 |
| VM-DB-M | Base de Datos Maestro | 172.20.10.6 | N/A | Red 20 | Ubuntu 24.04 |
| VM-DB-S | Base de Datos Esclavo | 172.20.10.7 | N/A | Red 20 | Ubuntu 24.04 |
| VM-BACKUP | Servidor de Copias | 172.20.10.14 | N/A | Red 25 | Ubuntu 24.04 |

---

### 4.2 Estrategia Adoptada

**Estrategia de Replicación:**  
Se optó por la replicación asíncrona de Postgres debido a la menor latencia, priorizando la separación de lectura/escritura para mantener mayor estabilidad entre conexión y peticiones a la base de datos evitando que se sobrecargue.

**Estrategia de Hardening:**  
Se aplicaron los estándares CIS de hardening para mantener una mayor seguridad en los sitios web evitando accesos no deseados mediante conexiones SSH, protegiendo de esa manera los servidores, también aplicando protección a los diferentes puertos mediante el firewall para una mayor seguridad.

---

## V. Guía de Implementación y Puesta en Marcha

A continuación se describen los pasos esenciales para replicar todo el sistema, incluyendo los requisitos previos, el proceso de despliegue y los archivos clave utilizados durante la configuración.

---

### 5.1. Pre-requisitos

- 7 VMs con Ubuntu 24.04 y acceso root/sudo con el servicio de SSH activo.  
- Repositorio Git clonado en cada VM.  
- Conexión entre todas las máquinas virtuales dentro de la misma red o redes internas configuradas.

---

### 5.2. Despliegue (Ejecución de la Automatización)

**Instalación:**
- Instalar en 3 VMs la base de datos Postgres.
- Instalar en 2 VMs servidores con Node.js.
- Instalar en 1 VM el balanceador de cargas con Nginx.

**Configuración:**  
- Entrar a cd /var/www/app/ 
- Ingresar al directorio de la aplicación web.

**Ejecución (opcional):**
```bash
sudo ./script.sh
```

---

### 5.3. Ficheros de Configuración Clave

A continuación se muestran los archivos y comandos utilizados durante la configuración del sistema.  
Estos ficheros representan la estructura real utilizada dentro de las VMs:

Last login: Tue Dec 2 02:34:11 2025 from 172.20.10.3
admin1@admin1:~$ nano web1.sh
admin1@admin1:~$ cd /var/www/app/
admin1@admin1:/var/www/app$ ls
database.db
node_modules
package.json
package-lock.json
admin1@admin1:/var/www/app$ nano public/index/.html
admin1@admin1:/var/www/app$ sudo nano public/index.html
[sudo] password for admin1:

#### Configuración del Balanceador (HAProxy)

log /dev/log local0
maxconn 4096
user haproxy
group haproxy

defaults
    log     global
    mode    http
    timeout connect 5000
    timeout client 50000
    timeout server 50000

frontend http_front
    bind *:80
    default_backend web_servers

backend web_servers
    balance roundrobin
    option httpchk GET /
    server vm1 172.20.10.4:80 check
    server vm2 172.20.10.10:80 check

Estos archivos forman parte esencial del sistema e incluyen:  
- Scripts de despliegue (web1.sh, web2.sh, balanceador.sh)  
- Archivos de la aplicación alojados en /var/www/app/  
- Configuración del balanceador HAProxy  
- Ediciones realizadas mediante nano en cada servidor

## VI. Pruebas y Validación

A continuación se presentan las pruebas realizadas al sistema, junto con lo que se esperaba obtener y el resultado obtenido durante las pruebas.

| Prueba Realizada | Resultado Esperado | Resultado Obtenido |
|------------------|--------------------|---------------------|
| **Test de Failover de la Base de Datos (Apagar Maestro)** | El esclavo debe tomar las escrituras o el servicio debe seguir activo. | OK |
| **Prueba de Carga/Estrés (Balanceo)** | El tráfico debe distribuirse entre los servidores web. | OK |
| **Test de Seguridad (SSL/Firewall)** | El acceso HTTP debe redirigir a HTTPS y el firewall debe bloquear todos los puertos excepto 443. | FALLIDO |
| **Ejecución del Script del Servidor Web** | El script debe crear la aplicación, instalar dependencias y generar los archivos necesarios. | OK |
| **Renderizado del Formulario HTML** | El formulario debe visualizarse correctamente desde el navegador. | OK |

## VII. Conclusiones y Lecciones Aprendidas

Este proyecto nos ayudó a entender mejor cómo funciona un sistema real donde varias máquinas trabajan juntas. Pudimos ver de primera mano lo importante que es tener respaldos automáticos, ya que cualquier falla o caída puede dejar sin servicio a un montón de personas si no hay una copia funcionando.

También aprendimos que no basta con “instalar cosas”. Cada servicio necesita configuraciones específicas, permisos, puertos abiertos, rutas correctas y bastante orden para que todo funcione como debería. Uno de los mayores retos fue darnos cuenta de que si una VM está mal configurada, todo lo demás puede fallar, así que la coordinación entre los servicios es clave.

En lo técnico, muchas cosas salieron bien: los scripts del servidor web funcionaron, la aplicación se levantó sin problemas y varias configuraciones se aplicaron como se esperaba. Sin embargo, también hubo cosas que no logramos al 100%, como el failover y el balanceo de carga, que requieren una configuración más avanzada y más tiempo de pruebas.

Si tuviéramos que repetir este proyecto, probablemente empezaríamos armando mejor las redes desde cero y organizando los roles de cada máquina con más detalle desde el inicio. También planificaríamos mejor la parte de seguridad para no tener que hacer ajustes a último momento.

En general, el proyecto nos sirvió para aprender cómo se arma un sistema que sea estable, seguro y fácil de mantener, y nos mostró la realidad de trabajar con infraestructura distribuida, donde cada cambio cuenta y todo debe estar bien conectado.





