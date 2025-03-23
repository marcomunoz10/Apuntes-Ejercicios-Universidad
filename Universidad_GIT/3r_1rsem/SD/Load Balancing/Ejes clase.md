### Respuestas basadas en el PDF:

---

**1. Diferencia entre un balanceador de carga y un AutoScaling**

- **Balanceador de carga**: Distribuye automáticamente el tráfico entrante entre múltiples instancias o recursos (como servidores) en diferentes Zonas de Disponibilidad. Garantiza alta disponibilidad y tolerancia a fallos, redirigiendo el tráfico solo a instancias saludables.
- **AutoScaling**: Permite agregar o eliminar instancias EC2 automáticamente según políticas definidas. Ayuda a manejar cambios en la demanda del sistema, escalando verticalmente (más recursos en una instancia) u horizontalmente (más instancias).

---

**2. Parámetros necesarios de configuración del AutoScaling**

- **Grupo de AutoScaling (ASG)**: Definición del grupo que agrupa instancias EC2.
- **Plantilla EC2**: Configuración de las instancias.
- **Políticas de escalado**: Reglas para escalar hacia arriba o abajo (dinámico, programado o predictivo).
- **Capacidad deseada**: Número inicial de instancias.
- **Número mínimo/máximo de instancias**: Límite inferior y superior de recursos.

---

**3. Funcionamiento del Load Balancer (ELB)**

- El ELB distribuye el tráfico entrante entre múltiples instancias en diferentes Zonas de Disponibilidad.
- Realiza **verificaciones de salud** (pings o solicitudes) a las instancias para asegurarse de que estén operativas.
- Tipos de ELB:
    - **Application Load Balancer (ALB)**: Opera a nivel de aplicación, útil para tráfico HTTP/HTTPS.
    - **Network Load Balancer (NLB)**: Maneja tráfico de red, optimizado para conexiones TCP.
    - **Gateway Load Balancer**: Diseñado para gestionar y escalar dispositivos virtuales.

---

**4. Tipos de instancias Spot y cuándo son útiles**

- **Instancias Spot**: Utilizan capacidad EC2 sobrante a un costo menor (hasta un 90% de descuento).
    - **Útiles** para cargas de trabajo tolerantes a interrupciones, como procesamiento por lotes, análisis de big data o tareas que pueden reiniciarse sin problema.
    - **Funcionamiento**: Los usuarios ofrecen un precio máximo; si el precio Spot excede esta oferta o la capacidad no está disponible, la instancia puede finalizarse.

---

**5. Función de CloudWatch al escalar una base de datos**

- **CloudWatch** monitorea el rendimiento y la capacidad de la base de datos (por ejemplo, en DynamoDB).
- **Acciones**:
    - Detecta cuando la capacidad consumida supera el umbral establecido.
    - Dispara alarmas que invocan políticas de escalado.
    - Trabaja en conjunto con AutoScaling para ajustar la capacidad de lectura/escritura según la demanda.