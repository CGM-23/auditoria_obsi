Esta herramienta viene preinstalada en Kali Linux siempre que utilices una imagen oficial completa del sistema. En caso de usar una versión empaquetada, como las disponibles en WSL, o instalarla en otro  SO aparte será necesario instalarla manualmente desde la página oficial de [BurpSuite](https://portswigger.net/burp/communitydownload).

primero 
Se utilizará un proxy configurado mediante la extensión _FoxyProxy_ en Firefox. Al activarla, el navegador enviará el tráfico al _Proxy Listener_ de Burp Suite, que por defecto escucha en **127.0.0.1:8080**.

![Imagen 1](auditoria_obsi/Auditoria/images/Pasted+Image+20251202013010.png)

![[Pasted image 20251202012910.png]]


## Interceptor
interceptamos la peticion

![[Pasted image 20251204113306.png]]
![[Pasted image 20251204113407.png]]
y la mandamos al repiter

Como el servidor espera un número y **no** pone comillas alrededor de la variable `$id` en el código SQL, **nosotros tampoco necesitamos usarlas**. Esto es perfecto porque así evitamos el filtro de seguridad que bloquea las comillas.


```
id=1 UNION SELECT user, password FROM users
```

![[Pasted image 20251204112550.png]]

