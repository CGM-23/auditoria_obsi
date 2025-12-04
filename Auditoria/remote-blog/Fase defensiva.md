Si bien poner el nivel en **"Impossible"** simula que la auditoría ocurrió y se aplicaron parches (remediación), el rol defensivo va mucho más allá de "arreglar el código". 

El Blue Team tiene **tres responsabilidades críticas** que aún podemos practicar en este laboratorio:

1. **Detección Forense:** Saber _qué pasó_. Si no revisas los logs, no sabes si te robaron datos antes de que parcharas .
	Los ataques con `sqlmap`, Burp Suite y XSS dejaron "huellas digitales" muy claras que un defensor debe saber encontrar.
	
	Primero, necesitamos entrar a la consola del contenedor donde se guardan los registros. 
```
	docker exec -it name bash
```
	Una vez dentro, se va al directorio de los logs
```
	cd /var/log/apache2/
	ls -lh
```
SQLi
El informe técnico señala que una inyección deja patrones muy claros como palabras clave de SQL y caracteres especiales en la URL .

```
grep -Ei "UNION|SELECT|ORDER BY|Waitfor|Sleep|OR.*=|INFORMATION_SCHEMA" access.log
```

**SQLMap**
Genera cientos de líneas de log en segundos. Es muy obvio .
Por defecto se anuncia a sí mismo: `User-Agent: sqlmap/1.2.4...`

```
grep "sqlmap" /var/log/apache2/access.log
```

![Pasted image 20251204105337](../images/Pasted%20image%2020251204123946.png)
Lo primero que delata a la herramienta es que, por defecto, se anuncia a sí misma. En cada línea del log, al final, verás algo como: `"User-Agent: sqlmap/1.8.11#stable (http://sqlmap.org)"`

- **Diagnóstico:** Esto es una confesión automática. Un navegador normal diría `Mozilla/5.0...`.

**Manual/Script**
Aparecerán pocas líneas y muy espaciadas en el tiempo. Es más difícil de ver a simple vista, pero si buscas las palabras clave, ahí estarán.
Mostrará `Mozilla/5.0...` (como un navegador normal) o `Python-urllib` si usas un script básico sin configurar.

![Pasted ima7](../images/Pasted%20image%2020251204123006.png)

```
grep "UNION" /var/log/apache2/access.log | grep -v "sqlmap"
```
grep -v "sqlmap": Oculta todo lo que venga de la herramienta automática.

XSS
Ahora busquemos los intentos de inyección de JavaScript. El informe indica que debemos buscar etiquetas HTML codificadas (como `%3Cscript%3E`) o eventos sospechosos .
```
grep -iE "%3Cscript%3E|%3Cimg|%3Csvg|onerror=|onload=|alert\(|1337" access.log
```

![Pasted ima7](../images/Pasted%20image%2020251204122511.png)
Aquí está lo que un analista de seguridad ve:

1. **El Vector de Ataque (`%3Cscript%3E`):**
    
    - Si decodificas `%3C` y `%3E`, obtienes `<` y `>`.
        
    - Esto confirma que alguien inyectó etiquetas HTML (`<script>`) en la URL. En tráfico normal, un usuario escribiría "Pepe", no un script.
        
2. **La Carga Maliciosa (Payload de Exfiltración):**
    
    - Vemos `window.location`. Esto confirma que el script intentaba redirigir al usuario.
        
    - Vemos `document.cookie`. Esto es la prueba de que el objetivo era robar la sesión.
        
3. **El Destino del Robo (`192.168.122.132:1337`):**
    
    - El log revela exactamente a dónde se enviaron los datos robados. Un equipo de respuesta a incidentes bloquearía inmediatamente esa IP (192.168.122.132) en el firewall para contener la fuga.


4. **Análisis de Causa Raíz:** Entender _por qué_ el código "Low" fallaba y por qué el "Impossible" funciona (Secure Coding) .

	 El Fallo (Nivel Low): En el nivel bajo, la vulnerabilidad crítica es la **concatenación directa** de la entrada del usuario (`$id`) en la consulta SQL .
	
	- El código confía ciegamente en lo que escribes.
	    
	
	La Solución (Nivel Impossible): ¿Por qué `sqlmap` falló en el nivel Impossible?
	
	- **El cambio técnico:** En lugar de pegar tu texto en la consulta, el código usa marcadores de posición (`:id`).
	    
	- **El efecto:** La base de datos trata tu input (`' OR 1=1`) estrictamente como **datos** (texto literal), nunca como **código ejecutable** . Por eso, aunque intentes inyectar comandos, la base de datos solo busca a un usuario que se llame literalmente "OR 1=1".






5. **Defensa en Profundidad:** Implementar capas extra como WAFs para proteger incluso código vulnerable .
    
	Aunque instalar ModSecurity en Docker es complejo, DVWA trae una simulación interna llamada **PHPIDS** (mencionada en la imagen de instrucciones que subiste) .
	
	Para ver un "WAF" en acción en tu laboratorio:
	
	1. Busca en la configuración de DVWA si hay una opción para activar **PHPIDS** (a veces está en el archivo de configuración o en versiones específicas de DVWA hay un botón para activarlo).
	    
	2. Si logras activarlo y lanzas el mismo ataque SQL que te funcionó antes, verás una página de error diciendo "Attack Detected" y tu petición será rechazada, protegiendo la aplicación aunque esté en nivel "Low".

![Pasted ima7](../images/Pasted%20image%2020251204120536.png)


![Paste7](../images/Pasted%20image%2020251204120550.png)

