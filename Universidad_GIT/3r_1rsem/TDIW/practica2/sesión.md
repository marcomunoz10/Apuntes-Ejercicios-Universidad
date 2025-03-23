#### Para establecer una sesión

```php
<?php 
session_start(); 
if (!isset($_SESSION['count'])) 
{ 
	$_SESSION['count'] = 0; 
} 
else { 
$_SESSION['count']++; 
} 
?>
```

Se verifica si la variable de sesión `$_SESSION['count']` está definida. Si no lo está (es decir, es la primera vez que el usuario accede a la página en esta sesión), se inicializa y se le asigna el valor 0.
Si la variable de sesión `$_SESSION['count']` ya está definida (lo que significa que el usuario ha recargado la página anteriormente), se incrementa su valor en uno.

#### Para eliminar una sesión
```php
<?php  
session_start();  
unset($_SESSION['count']);  
?>
```

`unset($_SESSION['count']);` Elimina la variable de sesión `$_SESSION['count']`, lo que significa que la próxima vez que se intente acceder a `$_SESSION['count']`, no existirá a menos que se vuelva a definir.