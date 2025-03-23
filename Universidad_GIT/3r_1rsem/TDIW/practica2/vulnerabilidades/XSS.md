Problema: si no está bien configurado el código el atacante podría insertar código malicioso donde pueda introducir texto.

Solución: convertir todas las entradas por parte del usuario a texto plano.

```php
<?php 
/** 
* Escape all HTML, JavaScript, and CSS 
* 
* @param string $input The input string 
* @param string $encoding Which character encoding are we using? 
* @return string 
**/ 
function noHTML($input, $encoding = 'UTF-8') 
{ 
	return htmlentities($input, ENT_QUOTES | ENT_HTML5, $encoding); 
} 
echo '<h2 title="', noHTML($title), '">', $articleTitle, '</h2>', "\n"; 
echo noHTML($some_data), "\n";
?>
```

