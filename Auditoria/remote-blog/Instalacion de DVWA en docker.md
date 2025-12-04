github de la pagina :[DVWA](https://github.com/digininja/DVWA)

Se instalara a través de docker para evitar configurar manualmente todo el servidor LAMP
imagen: https://hub.docker.com/r/vulnerables/web-dvwa/
para descargar la imagen 
```
docker pull vulnerables/web-dvwa
```

![Pasted image 20251204105337](../images/Pasted%20image%2020251202001251.png)

```
docker run --rm -it -p 80:80 vulnerables/web-dvwa
```

![Pasted image 20251202001618](../images/Pasted%20image%2020251202001618.png)
en el navegador entrar a  **localhost** 
user:admin
pass:password
![Pasted image 20251202001618](../images/Pasted%20image%2020251202001746.png)

DVWA está "diseñada intencionalmente con graves defectos" para ser una plataforma de aprendizaje.
Ese menú gris que ves a la izquierda es un índice de **lecciones aisladas**:

- **Pestaña SQL Injection:** El código de _esta página específica_ está programado mal a propósito en el parámetro `id` para que practiques la extracción de bases de datos.
    
- **Pestaña Brute Force:** Aquí encontrarás un formulario de **Login** (Usuario y Contraseña) diseñado para que practiques ataques de fuerza bruta, aunque a veces también es vulnerable a inyección SQL básica.
    
- **Pestaña Stored XSS:** Aquí hay un libro de visitas ("Guestbook") diseñado para que practiques guardar scripts maliciosos

![Pasted%20image%2020251202001458](../images/Pasted%20image%2020251202001458.png)
En una aplicación real (no de práctica), tú no tienes un menú que te diga dónde está el fallo. Tú tendrías que probar inyección SQL en:
- El formulario de Login (para entrar sin contraseña).
- La barra de búsqueda.
- Los comentarios.
- Las cookies.
