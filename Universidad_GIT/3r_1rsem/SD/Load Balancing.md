### Escalabilidad horizontal
`Scaling Out`: añadir recursos
`Scaling In`: Terminar recursos
Útil para aplicaciones de gran escala.

## Escalabilidad Vertical
Puede tener un límite, no siempre es rentable. Útil en corto plazo pq es fácil de implementar.

# **Amazon EC2 auto scaling**
**Es gratis**. Escala los servicios eficientemente.
Acorde a políticas definidas, horarios programados (schedules), o verificaciones de estado (health checks).

Múltiples zonas de disponibilidad en una región. Cuando una AZ cae, lanza nuevas instancias en otra AZ. Cuando se recupere redistribuye automáticamente las instancias.

Tiene integrado el `Load Balancer` registra **automáticamente** nuevas instancias para **distribuir el tráfico entre instancias.**

Hay 3 maneras de ajustar el escalado:
1. Scheduled
2. Dynamic
3. Predictive

## ¿Cómo funciona?

1. Crea un auto-scaling group. (Define el grupo de instancias que se gestionará automáticamente).
2. Configura una plantilla para las instancias EC2
3. Crea las instancias .
4. Define las políticas para escalar recursos hacia arriba o hacia abajo.

**Para usar Auto-Scaling de manera eficiente debe combinarse con Load-Balancer para distribuir el tráfico.**

### **Auto-Scaling Groups**

Una misma instancia no puede estar en dos ACG
Contiene EC2 con características similares.
Sobre el nº de instancias puedes especificar el mínimo, el máximo y el deseado.

![[Pasted image 20250115082801.png]]

- Intenta mantener el deseado con verificaciones de estado periódicas.
- Si una instancia falla, ASG la termina y crea otra.
- Un ASG puede contener instancias en una o más zonas de disponibilidad, pero en una sola región.
- Puede lanzar tipo de instancias bajo demanda, reservadas, de spot o combinación de ellas. Se puede especificar el porcentaje deseado. ASG hace la previsión de instancias con el precio más bajo posible.

**SPOT**
Utiliza la capacidad sobrante de una instancia a un precio de hasta el 90% rebajado. El usuario hace una oferta de el precio dispuesto a pagar. Se pueden terminar cuando uno quiera o cuando el trabajo del usuario finalice.

## Políticas de escalabilidad
Amazon puede añadir o quitar instancias. 

### **1. Scheduled**
Las acciones planificadas se hacen automáticamente.

### **2. Dynamic**
Permite definir parámetros para controlar el escalado. Útil para escalar en respuesta a condiciones cambiantes cuando no sabes cuándo ocurrirán los cambios.

### **3. Predictive**
Datos a partir de un uso real y complementados con miles de millones puntos de datos.
Necesita al menos 1 día de datos históricos. Se reevalúa cada 24 horas para crear una predicción de las próximas 48 horas. 
A partir de estos patrones, AWS crea un plan de escalado (conjunto de reglas q indican instancias a agregar/quitar, bajo qué horarios/demandas). Este plan se puede aplicar a más de un ASG


-------pagina 16-----

Diferencias clave entre un **Auto Scaling Group (ASG)** y un **Target Group**:

| **Aspecto**                     | **Auto Scaling Group (ASG)**                                           | **Target Group**                                                                                             |
| ------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Propósito principal**         | Administrar y escalar dinámicamente instancias EC2 según la demanda.   | Agrupar destinos (instancias, IPs, contenedores, etc.) para distribuir tráfico a través de un Load Balancer. |
| **Relación con instancias**     | Contiene y gestiona las instancias EC2 que escala automáticamente.     | Apunta a destinos (instancias, IPs, etc.) para enrutar tráfico según las solicitudes.                        |
| **Escalado**                    | Permite el escalado automático de instancias según políticas.          | No escala instancias, solo las organiza como objetivos de un Load Balancer.                                  |
| **Integración**                 | Funciona de forma independiente o con Load Balancers.                  | Necesita un Load Balancer para enrutar el tráfico a los destinos.                                            |
| **Configuración**               | Incluye un número mínimo, máximo y deseado de instancias.              | Configura un protocolo y un puerto para los destinos.                                                        |
| **Supervisión de salud**        | Monitorea la salud de las instancias y las reemplaza si fallan.        | Realiza verificaciones de estado (health checks) en los destinos configurados.                               |
| **Compatibilidad con recursos** | Solo gestiona instancias EC2.                                          | Compatible con instancias EC2, contenedores ECS y direcciones IP.                                            |
| **Relación con Load Balancer**  | Opcional, pero se usa frecuentemente con Elastic Load Balancers (ELB). | Necesita estar asociado a un Load Balancer para funcionar.                                                   |
| **Ejemplo de uso**              | Escalar instancias EC2 para manejar tráfico dinámico.                  | Distribuir tráfico web entre instancias EC2 asociadas.                                                       |

### **Resumen**

- Un **ASG** es responsable de la **gestión y escalado** de las instancias EC2.
- Un **Target Group** organiza los destinos que recibirán el tráfico de un Load Balancer, pero no escala ni administra recursos directamente.

Estas dos herramientas son complementarias y se suelen usar juntas en arquitecturas de AWS.

---

# Amazon route 53

**PUEDE FUNCIONAR EN MÁS DE UNA REGIÓN**

Amazon Route 53 es un servicio de Sistema de Nombres de Dominio (DNS) altamente disponible y escalable que permite administrar y enrutar el tráfico de usuarios finales a aplicaciones de manera eficiente. Una de las características principales de Route 53 son las **políticas de enrutamiento** que determinan cómo se dirige el tráfico hacia los recursos específicos. A continuación, se describen las diferentes políticas de enrutamiento disponibles:

---

### **1. Simple Routing (Round Robbin) (Política de Enrutamiento Simple)**

- **Descripción:** Es la política más básica. Responde con una sola dirección IP o recurso asociado a un nombre de dominio.
- **Ejemplo:**
    - Si `example.com` apunta a una única instancia EC2, el tráfico se dirige exclusivamente a esa instancia.

---

### **2. Weighted Routing Policy (Política de Enrutamiento Ponderado)**

- **Descripción:** Permite distribuir el tráfico entre varios recursos asignando pesos. Los pesos determinan la proporción del tráfico dirigido a cada recurso.
- **Casos de uso:** Útil para realizar pruebas A/B, distribuir cargas entre diferentes regiones o equilibrar tráfico entre versiones de aplicaciones.
- **Ejemplo:**
    - Si tienes dos instancias EC2, una con peso 70 y otra con peso 30, el 70% del tráfico se dirigirá a la primera instancia y el 30% a la segunda.

---

### **3. Latency Routing Policy (Política de Enrutamiento por Latencia)**

- **Descripción:** Redirige el tráfico al recurso que ofrezca la menor latencia al usuario, basado en mediciones de latencia entre las ubicaciones del usuario y las regiones de AWS.
- **Casos de uso:** Ideal para aplicaciones donde el tiempo de respuesta es crítico, como plataformas de streaming o videojuegos.
- **Ejemplo:**
    - Un usuario en Asia será dirigido al servidor en Tokio si este tiene menor latencia que un servidor en Estados Unidos.

---

### **4. Geolocation Routing Policy (Política de Enrutamiento por Geolocalización)**

- **Descripción:** Redirige el tráfico según la ubicación geográfica del usuario (país o continente).
- **Ejemplo:**
    - Los usuarios en Europa son dirigidos a servidores en Frankfurt, mientras que los usuarios en América del Norte son dirigidos a servidores en Virginia.

---

### **5. Geoproximity Routing Policy (Política de Enrutamiento por Proximidad Geográfica)**

- **Descripción:** Similar a la geolocalización, pero con más granularidad. Permite ajustar la proximidad de cada recurso mediante un sesgo que aumenta o disminuye la prioridad de ciertas ubicaciones.
- **Casos de uso:** Ideal para optimizar costos o priorizar ciertas regiones específicas.
- **Ejemplo:**
    - Un servidor en Londres puede configurarse para aceptar más tráfico de Europa que uno en Frankfurt, ajustando su "bias" (sesgo).

---

### **6. Failover Routing Policy (Política de Enrutamiento por Tolerancia a Fallos)**

- **Descripción:** Redirige el tráfico a un recurso de respaldo si el recurso principal falla, utilizando verificaciones de salud (health checks) de Route 53.
- **Casos de uso:** Ideal para escenarios de alta disponibilidad, donde es crítico redirigir tráfico automáticamente en caso de fallos.
- **Ejemplo:**
    - Si tu servidor principal en Virginia falla, el tráfico se redirige automáticamente a un servidor de respaldo en California.

---

### **Comparación de Uso:**

| **Política** | **Usos Clave**                             | **Complejidad** |
| ------------ | ------------------------------------------ | --------------- |
| Simple       | Configuración básica                       | Baja            |
| Weighted     | Pruebas A/B, equilibrio de carga           | Media           |
| Latency      | Mejorar experiencia con menor latencia     | Media           |
| Geolocation  | Cumplir regulaciones, contenido localizado | Media           |
| Geoproximity | Optimizar costos y controlar prioridades   | Alta            |
| Failover     | Alta disponibilidad                        | Media           |



