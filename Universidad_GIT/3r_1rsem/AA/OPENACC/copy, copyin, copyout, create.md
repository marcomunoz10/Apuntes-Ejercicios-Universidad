- **`copy(var)`**: Copia la variable `var` **del host al device al inicio de la región** y **de vuelta al host al final**. Es útil para variables que necesitan sincronizarse entre el host y el device antes y después de la ejecución.
    
- **`copyin(var)`**: Copia la variable `var` **del host al device al inicio de la región**, pero **no la copia de vuelta** al host. Se usa para datos que solo necesitan leerse en el device y no requieren ser actualizados en el host.
    
- **`copyout(var)`**: Copia la variable `var` **del device al host al final de la región**. Es útil para datos que se calculan o modifican en el device y que necesitan estar disponibles en el host al final.
- **`create(var)`**: Crea `var` en el device sin transferir datos entre host y device.