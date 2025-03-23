La gestión de **Slice Units** en `systemd` te permite agrupar y organizar procesos en **jerarquías** para gestionar y controlar el uso de recursos como la CPU y la memoria. Un **slice** (rebanada) es una unidad especial en `systemd` que agrupa servicios o procesos en un "contenedor" de recursos, aplicando límites como el uso máximo de CPU o memoria.

### ¿Cómo Funciona un Slice en systemd?

1. **Slices**: Los **slices** son como "contenedores" o "grupos de control" (`cgroups`) en los que se pueden organizar los procesos o servicios. Un slice permite imponer límites sobre los recursos (como CPU, memoria, etc.) que un grupo de procesos puede consumir.
    
2. **Cgroups**: `systemd` utiliza los **control groups (cgroups)** para gestionar los recursos de los procesos en Linux. Un slice es una manera de gestionar estos grupos jerárquicamente, de modo que puedes aplicar límites y restricciones a conjuntos de procesos que están agrupados bajo el mismo slice.
    
3. **Herencia**: Los slices son jerárquicos, lo que significa que un slice puede tener "sub-slices". Esto permite crear estructuras complejas de gestión de recursos, donde las restricciones se aplican tanto en los slices "padre" como en los slices "hijo".
    

### Proceso para Crear y Gestionar un Slice

#### 1. Crear un Archivo de Configuración para el Slice

Primero, creas un archivo para definir un **slice personalizado** que gestione los recursos que quieres limitar. El archivo se coloca en `/etc/systemd/system/`.

1. Crea un archivo para tu slice
```bash
sudo nano /etc/systemd/system/mi-slice.slice

```

2. Contenido del archivo `mi-slice.slice`:
```bash
[Slice]
CPUQuota=20%
MemoryLimit=500M
```

- **`CPUQuota=20%`**: Limita el uso de la CPU al 20% para todos los procesos y servicios asignados a este slice.
- **`MemoryLimit=500M`**: Limita el uso máximo de memoria a **500 MB** para los procesos asignados a este slice.
#### 2. Asignar un Servicio al Slice

Después de crear el slice, necesitas asignar los servicios o procesos específicos al slice. Esto se hace editando el archivo de unidad del servicio correspondiente.

1. Abres el archivo del servicio como root.
```bash
sudo nano /etc/systemd/system/nombre-del-servicio.service

```

2. Lo enlazas con el slice anterior
```bash
[Service]
Slice=mi-slice.slice
```

Esto indica que el servicio (o proceso) debe ser gestionado bajo el slice `mi-slice.slice`, y los límites de CPU y memoria definidos en ese slice se aplicarán al servicio.
Asignar un servicio al slice:  Editar el archivo de unidad del servicio y añade:    
[Service]
   Slice=mi-slice.slice
   Recargar y reiniciar el servicio:
   systemctl daemon-reload
   systemctl restart nombre-del-servicio


