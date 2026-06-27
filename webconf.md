# Configuraciones Web

## Configuración entradas

1. Imagen destacada de **w320px por h180px** de altura con una proporción 16:9
2. TEXTO - Contenido - Seleccionar texto y justificar.
3. TEXTO - Diseño - Texto - {tamaño 17px}
4. TEXTO - Diseño - Texto de subtítulo - H1:{nEGRITA tamaño 22px Altura línea 1.5}
4. Cajón verde - Configuración - Diseño - Separación - Superior {-66px} 
5. Separación - margen superior {66px}

## Documentos Pdf

**BARRA DESCARGA DEL DOCUMENTO**
> Bloque de código
```html
<div style="background-color: #CCD; padding: 8px 15px; border-radius: 6px; border-left: 4px solid #4169E1; margin: 25px; font-family: Arial, sans-serif; justify-content: space-between; align-items: center; max-width: 90%;"><span style="color: #333; font-weight: bold; font-size: 0.95rem;">📄 nombre </span> <a style="background-color: #4169E1; color: #fff; padding: 5px 12px; text-decoration: none; border-radius: 4px; font-size: 0.85rem; font-weight: bold;margin-left:24px" href="URL.........." target="_blank" rel="noopener">DESCARGAR</a></div>
```
<div style="background-color: #CCD; padding: 8px 15px; border-radius: 6px; border-left: 4px solid #4169E1; margin: 25px; font-family: Arial, sans-serif; justify-content: space-between; align-items: center; max-width: 90%;"><span style="color: #333; font-weight: bold; font-size: 0.95rem;">  📄 Baremación provisional del alumnado</span> <a style="background-color: #4169E1; color: #fff; padding: 5px 12px; text-decoration: none; border-radius: 4px; font-size: 0.85rem; font-weight: bold;margin-left:24px" href="https://www3.gobiernodecanarias.org/medusa/edublog/cifpvilladeaguimes/wp-content/uploads/sites/706/2026/06/BAREMACION-PROVISIONAL-ALUMNADO-26-27.pdf" target="_blank" rel="noopener">DESCARGAR</a></div>

**DOCUMENTO PDF VISIBLE**

```
<div style="text-align:center;">
[pdf-embedder url="URL......."]
</div>
```
```
<!-- /wp:file -->
<div class="wp-block-file"><object class="wp-block-file__embed" style="width: 100%; height: 600px;" data="https://www3.gobiernodecanarias.org/medusa/edublog/cifpvilladeaguimes/wp-content/uploads/sites/706/2026/06/Vacantes-2o-cursos-26-27.pdf" type="application/pdf" aria-label="LISTADO VACANTES GRUPOS 2º CURSO 2026/2027"></object><a id="wp-block-file--media-f1daa344-ed71-43e1-9c34-713a34d48cd6" href="https://www3.gobiernodecanarias.org/medusa/edublog/cifpvilladeaguimes/wp-content/uploads/sites/706/2026/06/Vacantes-2o-cursos-26-27.pdf"></a></div>
<div class="wp-block-file"></div>
<!-- /wp:file -->
```


