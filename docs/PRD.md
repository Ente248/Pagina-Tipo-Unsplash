# PRD · <Caleidoscopio>

> Documento de requisitos del producto (Product Requirements Document).
> Complétalo en el Sprint 0 (historia H0.3) y pide la aprobación del profesor en un Issue.
> Cada requisito funcional (RF) se convierte en un Issue con la plantilla "Historia de usuario".

| Campo | Valor |
| --- | --- |
| Alumno |Sergio Arango|
| Proyecto n.º |1|
| Sitio de referencia |Unsplash.com|
| Compañero revisor |Jose Marcano|
| URL desplegada | (se completa en el Sprint 5) |

## 1. Problema

Los usuarios que buscan recursos visuales suelen enfrentarse a catálogos desordenados donde encontrar una fotografía resulta lento y poco intuitivo. Se necesita una plataforma web rápida que centralice imágenes de alta calidad y permita filtrarlas con precisión en tiempo real.

## 2. Usuarios

- **Usuario principal:** Usuario que navega por la galería, busca fotografías por palabras clave, aplica filtros combinados (categoría, etiquetas, orientación) y visualiza o descarga el detalle de cada imagen.
- **Usuario secundario:** Usuario autenticado encargado de subir nuevas fotografías al catálogo, asignarles título, descripción, etiquetas y categorías, o moderar el contenido publicado.

## 3. Objetivo y métrica de éxito

1- Tener en promedio 10000 visitas al mes.
2- Alcanzar los 200 usuarios activos.
3- Maximizar las interacciones entre usuarios.
4- Acortar los tiempos de envio y recepcion de informacion.


## 4. Alcance del MVP

Entre 5 y 8 funcionalidades que SÍ entran este semestre.

1- Visualización de catálogo de imágenes en cuadrícula responsiva (estilo masonry/grid).

2- Barra de búsqueda por título, descripción o etiquetas.

3- Filtros combinables por categoría, etiquetas y orientación (horizontal, vertical, cuadrada).

4- Vista de detalle de imagen (autor, dimensiones, etiquetas, categoría y opción de descarga).

5- Gestión básica de publicación de imágenes con persistencia en PostgreSQL.

### Funcionalidades → carpetas de `src/` (screaming architecture)

Cada funcionalidad del alcance será una carpeta en `src/`. Nómbralas en inglés y en plural.

| Funcionalidad | Carpeta |
| --- | --- |
| Catálogo y detalle de fotografías | `src/photos` |
| Búsqueda y filtros combinados (categoría, etiquetas, orientación) | `src/filters` |
| Categorías temáticas de la galería | `src/categories` |
| Etiquetas (tags) asociadas a las fotografías | `src/tags` |
| Autores y perfiles de fotógrafos | `src/authors` |

## 5. Fuera de alcance

- Pasarela de pagos, suscripciones o venta de licencias comerciales.

- Editor de imágenes integrado en el navegador (recorte, ajuste de brillo/contraste o aplicación de filtros visuales sobre la imagen).

- Aplicaciones móviles nativas para iOS o Android (el producto será exclusivamente web responsiva).

- Autenticación social con terceros (Google/GitHub OAuth) o red social con comentarios y mensajería directa entre usuarios.

## 6. Requisitos funcionales

| ID | Requisito | Prioridad (alta, media, baja) |
| --- | --- | --- |
| RF-01 | Listar fotografías en una cuadrícula responsiva (estilo masonry/grid) con carga paginada o progresiva. | alta |
| RF-02 | Buscar fotografías por palabras clave coincidentes en título, descripción o etiquetas. | alta |
| RF-03 | Filtrar fotografías combinando múltiples criterios simultáneamente (categoría, una o varias etiquetas y orientación: horizontal, vertical o cuadrada). | alta |
| RF-04 | Limpiar filtros activos de forma individual o restablecer todos los filtros con un clic. | media |
| RF-05 | Visualizar el detalle de una fotografía (imagen ampliada, título, autor, dimensiones, categoría y etiquetas asociadas). | alta |
| RF-06 | Descargar la fotografía en su resolución disponible y registrar el conteo de descargas. | media |
| RF-07 | Publicar nuevas fotografías indicando título, URL/archivo, orientación, autor, categoría y etiquetas. | media |
| RF-08 | Marcar o guardar fotografías favoritas en una colección rápida durante la sesión. | baja |

## 7. Requisitos no funcionales

- Responsive, móvil primero (375 px y 1280 px).
- Lighthouse ≥ 90 en Accesibilidad y Buenas prácticas.
- 0 errores en la consola del navegador.
- Funciona con 10.000 registros sin congelarse.

## 8. Modelo de datos

Diagrama entidad-relación y tablas PostgreSQL con tipos, claves y restricciones.
GitHub dibuja diagramas Mermaid; edita este ejemplo:

```mermaid
erDiagram
  AUTHOR ||--o{ PHOTO : publica
  CATEGORY ||--o{ PHOTO : clasifica
  PHOTO ||--o{ PHOTO_TAG : tiene
  TAG ||--o{ PHOTO_TAG : etiquetada_en

  AUTHOR {
    int id PK
    text nombre
    text usuario UK
    text avatar_url
    timestamptz creado_en
  }
  CATEGORY {
    int id PK
    text nombre UK
    text slug UK
  }
  PHOTO {
    int id PK
    int author_id FK
    int category_id FK
    text titulo
    text descripcion
    text image_url
    text orientacion
    int ancho
    int alto
    int descargas
    timestamptz creado_en
  }
  TAG {
    int id PK
    text nombre UK
    text slug UK
  }
  PHOTO_TAG {
    int photo_id PK, FK
    int tag_id PK, FK
  }

## 9. Reto técnico distintivo

Filtros Manipulando Pixeles en CANVAS (ImageData)

## 10. Riesgos y preguntas abiertas

- Riesgo de rendimiento en carga de imágenes externas: Si las URLs de prueba apuntan a imágenes muy pesadas sin miniaturas, la carga inicial puede volverse lenta. Mitigación: usar parámetros de tamaño/calidad en las URLs de semilla y carga diferida (`loading="lazy"`).
- Complejidad en filtros combinados con múltiples etiquetas: Definir si al seleccionar varias etiquetas la búsqueda debe mostrar fotos que cumplan todas las etiquetas (AND) o al menos una (OR). 

Pregunta abierta:

Hacerca del almacenamiento me gustaria preguntar si en los próximos sprints las nuevas fotos se registrarán mediante URL externa validada o subida directa al servidor local.
