

Obtener la "Llave" de Sesión
Necesitas decirle a `sqlmap`: "Oye, soy el usuario 'admin' y esta es mi identificación".

1. En la pantalla de DVWA donde estás (tu navegador), haz clic en la opción **"SQL Injection"** en el menú gris de la izquierda.
2. Abrir las Herramientas de Desarrollador
3. Busca la pestaña **Almacenamiento** (o _Storage_).
4. En la columna izquierda, despliega **Cookies** y haz clic en tu IP
5. Verás algo llamado `PHPSESSID`. **Copia el valor** (será una cadena larga de letras y números, ej: `abcdef123456...`).

![Pasted ima7](../images/Pasted%20image%2020251203165249.png)

```
sqlmap -u "http://192.168.122.1/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
--cookie="PHPSESSID=ID; security=low" \
--batch --dbs
```
se esta preguntando que base de datos existen

![Pasted ima7](../images/Pasted%20image%2020251203172729.png)
**¿Por qué usar "solo" `--dbs` ahora?** Es una cuestión de orden y metodología. En un ataque real (Red Team), no intentas descargarlo todo de golpe porque hace mucho "ruido" en la red y alerta al Blue Team. Sigues una jerarquía:

1. **Enumeración (`--dbs`):** Primero preguntas "¿Qué bases de datos existen?"..
    
    - _Resultado:_ Verás nombres como `dvwa` y `information_schema`.


```
sqlmap -u "url" \
--cookie="PHPSESSID=ID; security=low" \
--batch -D dvwa --tables
```
**extraer los nombres de las tablas** para encontrar dónde están los usuarios de la tabal **dvwa**


![Pasted ima7](../images/Pasted%20image%2020251203173158.png)

![Pasted ima7](../images/Pasted%20image%2020251203174127.png)

![Pasted ima7](../images/Pasted%20image%2020251203174141.png)
