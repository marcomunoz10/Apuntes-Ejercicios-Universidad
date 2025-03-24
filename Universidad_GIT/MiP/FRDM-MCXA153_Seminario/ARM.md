ARM tiene diferentes famílias:
* **A**plications Processors
* **R**eal Time Processors
* **M**icrocontrollers

La família ARM Cortex-M está especialmente diseñada para **aplicaciones en tiempo real** y **sistemas empotrados**. La **eficiencia energética** es uno de sus puntos clave. Es **escalable**.

# **Cortex M33**

| Arquitectura de Computador | Harvard       |
| -------------------------- | ------------- |
| Instrucciones de Pipeline  | 3             |
| Latencia de interrupción   | 12 ciclos     |
| TUMB1 instructions[+]      | Completa [++] |
| TUMB2 instructions         | Completa      |
| Inst Multiplicación 32·32  | SÍ            |
[+] TUMB1 se refiere a la primer **Trice Unit** (Bloque de rastreo y depuración del procesador). (Conjunto hardware especializado que permite monitorear la ejecución del programa en tiempo real sin afectar significativamente el rendimiento del sistema.

[++] ENTIRE: se está hablando de toda la unidad en su conjunto. 