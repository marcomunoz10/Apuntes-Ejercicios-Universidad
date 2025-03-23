
#### **2. Configurar una clave SSH (recomendado)**

Otra opción más segura y cómoda es usar una **clave SSH**.

##### **Pasos para configurar la autenticación SSH**

1. **Generar una clave SSH en tu máquina:**
    
    ```sh
    ssh-keygen -t rsa -b 4096 -C "tu-email@example.com"
    ```
    
    Presiona **Enter** en todas las opciones (se guardará en `~/.ssh/id_rsa`).
    
2. **Añadir la clave a GitHub**:
    
    - Copia la clave pública:
        
        ```sh
        cat ~/.ssh/id_rsa.pub
        ```
        
    - Ve a **GitHub → Settings → SSH and GPG keys**.
    - Haz clic en **New SSH key**, pega la clave y guarda.
3. **Probar la conexión**:
    
    ```sh
    ssh -T git@github.com
    ```
    
    Si todo está bien, verás un mensaje de bienvenida.
    
4. **Clonar usando SSH**:
    
    ```sh
    git clone git@github.com:UAB-CAP-GEI/openmp-torn2_grupX.git
    ```
    
