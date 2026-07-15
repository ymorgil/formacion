# Tarjeta de noticia

Tarjeta responsive para destacar una noticia o evento: imagen, fecha, título, extracto y botón "Leer más". Pensada para pegarse en un **Módulo de código** de Divi dentro de una fila (puedes repetir el módulo varias veces en una fila de 3-4 columnas para una cuadrícula de noticias).

!!! info "Dónde pegarlo"
    Divi Builder → añade una **Fila** → dentro, un **Módulo de código** → pega el bloque completo (HTML + `<style>`) tal cual. El CSS va con `scope` propio (clase `.snw-card`) para no chocar con otros estilos del tema.

## Vista previa

[captura pendiente: tarjeta de noticia renderizada en la web del centro]

## Código

```html
<div class="snw-card">
  <a href="https://tu-centro.es/noticia-ejemplo/" class="snw-card__link">
    <div class="snw-card__img">
      <img src="https://tu-centro.es/wp-content/uploads/2026/07/ejemplo.jpg" alt="Descripción de la imagen de la noticia" loading="lazy">
      <span class="snw-card__date">08 Jul 2026</span>
    </div>
    <div class="snw-card__body">
      <h3 class="snw-card__title">Título de la noticia o evento</h3>
      <p class="snw-card__excerpt">
        Resumen breve de una o dos líneas que enganche a hacer clic y explique de qué trata la noticia.
      </p>
      <span class="snw-card__cta">Leer más →</span>
    </div>
  </a>
</div>

<style>
.snw-card {
  font-family: inherit;
  max-width: 360px;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0,0,0,0.08);
  transition: transform .2s ease, box-shadow .2s ease;
  background: #fff;
}
.snw-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.12);
}
.snw-card__link {
  text-decoration: none;
  color: inherit;
  display: block;
}
.snw-card__img {
  position: relative;
  aspect-ratio: 16 / 9;
  overflow: hidden;
}
.snw-card__img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.snw-card__date {
  position: absolute;
  top: 10px;
  left: 10px;
  background: rgba(20, 20, 20, 0.75);
  color: #fff;
  font-size: 0.75rem;
  padding: 4px 10px;
  border-radius: 20px;
}
.snw-card__body {
  padding: 16px 18px 20px;
}
.snw-card__title {
  margin: 0 0 8px;
  font-size: 1.05rem;
  line-height: 1.3;
  color: #1a1a1a;
}
.snw-card__excerpt {
  margin: 0 0 12px;
  font-size: 0.9rem;
  line-height: 1.5;
  color: #555;
}
.snw-card__cta {
  font-size: 0.85rem;
  font-weight: 600;
  color: #4b2e83; /* ajusta al color corporativo del centro */
}

@media (max-width: 480px) {
  .snw-card {
    max-width: 100%;
  }
}
</style>
```

## Notas de uso

- Cambia el color de `.snw-card__cta` (`#4b2e83`) por el color corporativo del centro.
- La imagen usa `object-fit: cover`, así que no hace falta recortarla manualmente antes de subirla: rellena el hueco 16:9 sin deformarse.
- Si Divi ya carga Font Awesome, puedes sustituir la flecha `→` por un icono.
- Para una cuadrícula de 3 noticias, crea una fila de 3 columnas y repite este mismo módulo de código en cada una, cambiando solo el contenido (imagen, fecha, título, extracto, enlace).
- Si el tema del centro tiene un ancho de columna distinto, ajusta `max-width` de `.snw-card`.

## Variables que debes editar cada vez

| Campo | Dónde |
|---|---|
| Enlace a la noticia | `href` del `<a>` |
| Imagen | `src` del `<img>` |
| Texto alternativo | `alt` del `<img>` (accesibilidad, no lo dejes vacío) |
| Fecha | contenido de `.snw-card__date` |
| Título | contenido de `.snw-card__title` |
| Extracto | contenido de `.snw-card__excerpt` |
