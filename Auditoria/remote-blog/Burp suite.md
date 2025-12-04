Esta herramienta viene preinstalada en Kali Linux siempre que utilices una imagen oficial completa del sistema. En caso de usar una versión empaquetada, como las disponibles en WSL, o instalarla en otro  SO aparte será necesario instalarla manualmente desde la página oficial de [BurpSuite](https://portswigger.net/burp/communitydownload).

primero 
Se utilizará un proxy configurado mediante la extensión _FoxyProxy_ en Firefox. Al activarla, el navegador enviará el tráfico al _Proxy Listener_ de Burp Suite, que por defecto escucha en **127.0.0.1:8080**.

![Imagen 1](../images/Pasted%20image%2020251202013010.png)


![Imagen 2](../images/Pasted%20image%2020251202012910.png)



## Interceptor
interceptamos la peticion

![Imagen 3](../images/Pasted%20image%2020251204113306.png)  
![Imagen 4](../images/Pasted%20image%2020251204113407.png)
y la mandamos al repiter

Como el servidor espera un número y **no** pone comillas alrededor de la variable `$id` en el código SQL, **nosotros tampoco necesitamos usarlas**. Esto es perfecto porque así evitamos el filtro de seguridad que bloquea las comillas.


```
id=1 UNION SELECT user, password FROM users
```

![Imagen 5](../images/Pasted%20image%2020251204112550.png)

