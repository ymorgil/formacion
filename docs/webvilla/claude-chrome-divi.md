# MCP, Claude Code y Claude en Chrome aplicados a WordPress/Divi

Esta guía responde a tres preguntas prácticas: qué es un "snippet" en este repo, qué es MCP y para qué sirve, y cómo usar **Claude en Chrome** para las tareas repetitivas de mantenimiento de la web del centro en `wp-admin`/Divi.

## 1. Qué es un snippet (en este repo)

Un *snippet* es un fragmento de código autocontenido — normalmente HTML + CSS, a veces JS — pensado para pegarse tal cual en el **Módulo de código** de Divi Builder, sin tocar el tema ni el servidor. No es un plugin: no se instala, no se actualiza solo, y si Divi cambia de tema puede dejar de verse bien y habrá que revisarlo.

Cada snippet de este repo se documenta con:

- El código completo, listo para copiar y pegar.
- Una vista previa (captura) de cómo queda.
- Notas de uso: qué cambiar cada vez, límites conocidos, dependencias (por ejemplo, si asume que Font Awesome ya está cargado).

La ventaja de tenerlos aquí en vez de sueltos en un Word: control de versiones, historial de cambios, y que cualquier compañero con acceso al repo puede reutilizarlos.

## 2. MCP, en dos frases

**MCP (Model Context Protocol)** es el estándar que permite a Claude conectarse a herramientas externas (Google Drive, GitHub, Slack...) mediante "conectores": cada conector expone acciones concretas que Claude puede ejecutar durante la conversación, siempre dentro de los permisos que tú ya tienes en esa herramienta.

Para WordPress no existe (a día de hoy) un conector MCP estándar en el directorio de Anthropic. Como no tienes acceso al código ni a una API propia del sitio, la pieza que sí te sirve para automatizar tareas en `wp-admin`/Divi no es un conector MCP, sino el **agente de navegador Claude en Chrome**, que se explica en el punto siguiente.

Dónde sí encaja MCP en tu flujo real: en tus repos de documentación (`systems`, `snippets-web`, etc.), donde Claude Code ya trabaja de forma directa sobre tus archivos y tu Git local sin necesidad de ningún conector adicional.

## 3. Claude en Chrome para tareas en wp-admin/Divi

### Qué es

Claude en Chrome es una extensión que convierte a Claude en un agente capaz de ver y manejar tu navegador: navega, hace clic, rellena formularios y lee el contenido de la página, igual que lo harías tú manualmente. No accede a la web del centro por una API oculta — usa tu sesión de navegador ya autenticada en `wp-admin`, con tus mismos permisos.

Es la herramienta adecuada cuando la tarea es **repetitiva pero no requiere código nuevo**: revisar, rellenar, comprobar o corregir contenido ya existente en el panel de WordPress.

### Instalación (resumen)

1. Instala la extensión "Claude en Chrome" desde la Chrome Web Store con tu cuenta de Claude vinculada.
2. Ábrela desde el icono de la barra de extensiones mientras tienes `wp-admin` abierto y con sesión iniciada.
3. La extensión pide permiso pestaña a pestaña o sitio a sitio — concédelo solo para el dominio de la web del centro cuando vayas a trabajar ahí.

### Qué tareas tienen sentido aquí

- **Revisar enlaces rotos**: recorrer una sección (por ejemplo, "Oferta formativa") y listar qué enlaces devuelven error.
- **Comprobar imágenes destacadas**: entrar en Entradas → Todas las entradas y detectar cuáles no tienen imagen destacada asignada.
- **Rellenar campos repetitivos**: por ejemplo, meta-descripciones SEO de varias páginas siguiendo un patrón que tú defines.
- **Verificar consistencia**: que todas las noticias de un mismo tipo tengan la misma estructura (fecha, categoría, etiqueta).
- **Publicar contenido ya redactado**: pegar un texto que preparaste antes (aquí, en Claude Code, o en Word) en el editor de WordPress, en el bloque o módulo correcto.

### Qué NO conviene pedirle

- Crear componentes visuales nuevos (tarjetas, acordeones...) — eso se prototipa en local con Claude Code y el snippet se documenta aquí; luego se pega ya terminado.
- Cambios masivos irreversibles sin revisión: pídele siempre que te muestre qué va a cambiar antes de guardar/publicar.
- Nada que dependa del código fuente del tema (Claude en Chrome ve y usa la interfaz, no el código PHP/CSS del tema).

### Ejemplos de prompts

```text
Entra en wp-admin → Entradas. Revisa las últimas 20 entradas de la
categoría "Noticias" y dime cuáles no tienen imagen destacada.
No cambies nada, solo dame la lista.
```

```text
Abre la página "Oferta formativa" en el editor de Divi. Comprueba
uno a uno los enlaces de la tabla de ciclos formativos y dime cuáles
devuelven 404. No los edites todavía, solo repórtalos.
```

```text
Voy a darte el texto de una noticia ya redactada. Ábreme una entrada
nueva en wp-admin, pega el título y el cuerpo en el editor, asígnale
la categoría "Noticias" y dime cuándo está lista para que yo revise
antes de publicar.
```

### Permisos y límites a tener en cuenta

- Claude en Chrome actúa **con tu sesión**: todo lo que haga queda como si lo hubieras hecho tú (incluido en los registros de WordPress).
- Antes de cualquier acción que **publique o guarde cambios**, debe confirmarte el paso — si no lo hace, pídeselo explícitamente en el prompt ("no publiques sin que yo lo confirme").
- No sigas enlaces desconocidos que aparezcan en contenido de terceros (comentarios, formularios de contacto) sin comprobarlos tú antes.
- Para tareas grandes (revisar decenas de entradas), es más fiable dividir en lotes pequeños y revisar los resultados parciales, en vez de pedir todo de una vez.

## 4. Resumen del flujo completo

| Necesidad | Herramienta |
|---|---|
| Crear un componente visual nuevo (HTML/CSS) | Claude Code en local → documentar en `snippets/` → pegar en Divi a mano |
| Redactar o mejorar un texto | Claude Code o chat, sin necesidad de tocar el navegador |
| Rellenar, revisar o corregir contenido ya existente en `wp-admin` | Claude en Chrome |
| Automatizar contra un repo Git (apuntes, documentación) | Claude Code, con acceso directo a tu carpeta `repositorio/` |
