# Snippets Web

Colección de fragmentos HTML/CSS listos para pegar en el **Módulo de código** de Divi Builder, más guías sobre cómo apoyarme en IA (Claude Code, Claude en Chrome, MCP) para mantener la web del centro sin tener acceso al código fuente de WordPress.

## Por qué existe este repo

En la web del centro no tengo acceso al tema ni al servidor, solo a:

- El editor visual **Divi Builder**.
- Los módulos de **Código** dentro de Divi, donde puedo pegar HTML/CSS (y JS si hace falta).

Eso es suficiente para construir componentes visuales consistentes (tarjetas, acordeones, tablas, banners...) sin depender de un desarrollador. Este repo documenta cada snippet para no reinventar la rueda cada curso y para poder reutilizarlos en otras páginas.

## Contenido

- **[Snippets](snippets/tarjeta-noticia.md)** — cada snippet con código, vista previa y dónde pegarlo.
- **[Guías](guias/claude-chrome-divi.md)** — qué es un snippet, y cómo usar Claude en Chrome para tareas repetitivas de mantenimiento en `wp-admin` (revisar enlaces, imágenes destacadas, rellenar campos...).

## Flujo de trabajo recomendado

1. Prototipar el snippet en local con Claude Code (ver la guía) hasta que el HTML/CSS quede bien.
2. Documentarlo aquí: código + captura + notas de uso.
3. Pegarlo en el Módulo de código de Divi en la página real.
4. Si la tarea es repetitiva dentro de `wp-admin` (no un snippet nuevo, sino rellenar/editar/revisar contenido existente), usar Claude en Chrome en vez de hacerlo a mano.
