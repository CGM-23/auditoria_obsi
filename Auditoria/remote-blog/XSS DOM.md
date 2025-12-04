Vulnerabilidad donde el ataque es ejecutado completamente en el lado del cliente mediante la manipulación del Document Object Model (DOM). El payload nunca es enviado al servidor, sino que se procesa a través de APIs del navegador que evalúan dinámicamente datos no confiables.

**Analogía Estructurada**:  
_Modificación de Planos en Sitio de Construcción_: Los planos arquitectónicos originales (respuesta del servidor) son correctos, pero en el sitio de construcción (navegador), los trabajadores interpretan instrucciones modificadas (fragmentos URL, almacenamiento local) que alteran la ejecución, resultando en una estructura comprometida sin que los planos centrales reflejen los cambios.


prueba básica

En este nivel, el script de la página toma el parámetro de la URL y lo escribe directamente en el HTML sin revisar nada.

	Selecciona un idioma (ej. English) y dale a "Select".
        
    Mira la URL: `.../vulnerabilities/xss_d/?default=English`
        
    Si inspeccionas el código (Click derecho -> Ver código fuente), verás que un script toma el valor de "default" y lo escribe: `document.write(...)`.
        
3. **El Ataque:**
    
    - Modifica la URL directamente en tu navegador. Borra `English` y pon el script básico.
        
    - **Payload:**
        
        Plaintext
        
        ```
        ?default=<script>alert(document.cookie)</script>
        ```
        
    - Presiona **Enter**.
        
![[Pasted image 20251204025611.png]]
**Resultado:** Debería saltar la alerta con tu cookie. El navegador ejecutó el script porque la página lo escribió "en vivo" en el documento.

 Nivel Medium     
 Si intentas el payload anterior (`<script>...`), no pasará nada.
        
    - **¿Por qué?** En este caso, el servidor tiene un filtro que busca la palabra `<script>` en la URL y, si la encuentra, te redirige a "English" por defecto. Además, el código se inserta dentro de una etiqueta `<select>` (un menú desplegable).
        
 **La Estrategia de Evasión:**
    
    - **Paso 1: Salir de la cárcel.** Como tu texto se inserta dentro de `<select><option value='TU_TEXTO'>`, poner un script ahí dentro no funciona porque el HTML dentro de un `option` no se ejecuta. Necesitamos "cerrar" esas etiquetas primero.
        
    - **Paso 2: Usar otra etiqueta.** Como `<script>` está prohibido, usamos la técnica de la imagen `<img>` que vimos antes.
        
**El Payload de Evasión ("Breakout"):** pega esto en la URL después de
    
```
?default=English</option></select><img src=x onerror=alert(document.cookie)>
```

```
?default=English</select><img src/onerror=alert(document.cookie)>
```

        
        - `</option></select>`: Cierra el menú desplegable a la fuerza.
            
        - `<img...>`: Inserta la imagen maliciosa fuera del menú, donde sí se puede ejecutar.
            
        - `onerror=...`: Ejecuta el JavaScript.
            

**Inténtalo con ese payload en la URL.** Deberías ver un menú desplegable "roto" en la página y luego la alerta emergente.
![[Pasted image 20251204025841.png]]


Como bien dedujiste, "solo modificar la URL" no sirve de nada si nadie la abre. El ataque no es contra el servidor, es contra la persona que hace clic.
Ejecutar scripts en el contexto del navegador de la víctima para **robar sesiones**, realizar acciones no autorizadas o redirigir tráfico .

1. **El Arma:** La URL modificada.
    
2. **El Método:** Phishing (hacer que alguien haga clic) .
    
3. **La Vulnerabilidad:** Un programador usó funciones inseguras de JavaScript (como `document.write` o `innerHTML`) que confían ciegamente en lo que viene en la URL.