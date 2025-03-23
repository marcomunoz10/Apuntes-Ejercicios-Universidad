**URL**: Es la dirección web que puedes escribir en el navegador para acceder a páginas o funcionalidades específicas, y puede incluir parámetros que ayudan a determinar la información que necesitas.

Si visitas una URL como `http://tusitio.com/index.php?accio=llistar-categories`, la aplicación lee el valor de `accio` y ve que es `llistar-categories`. Esto hace que el enrutador incluya el archivo `resource_llistar_categories.php`, mostrando la lista de categorías.

- **Ejemplo**:
    - **Sin parámetro**: `http://tusitio.com/index.php` → Muestra `resource_portada.php`
    - **Con parámetro `accio=llistar-categories`**: `http://tusitio.com/index.php?accio=llistar-categories` → Muestra `resource_llistar_categories.php`