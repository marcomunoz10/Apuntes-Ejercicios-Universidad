- **`schedule(static, chunk_size)`**:
    
    - Uso recomendado cuando las iteraciones tienen tiempos de ejecución similares.
    - Cada thread obtiene **`chunk_size`** iteraciones consecutivas.
- **`schedule(dynamic, chunk_size)`**:
    
    - Las iteraciones se distribuyen de manera **dinámica**.
    - Cada thread **toma un bloque de `chunk_size` iteraciones** a medida que va terminando su trabajo.
    - **Uso recomendado cuando no puedes predecir o cuando las iteraciones tienen tiempos de ejecución desiguales**.