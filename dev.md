---
title: Programación
class: dev
layout: default
permalink: /dev/
menu_order: 1
images:
  cms:
    - title: Secciones
      thumb: /assets/images/dev/cms1_thumb.png
      image: /assets/images/dev/cms1.png
    - title: Editor de textos
      thumb: /assets/images/dev/cms2_thumb.png
      image: /assets/images/dev/cms2.png
    - title: Editor de imágenes
      thumb: /assets/images/dev/cms3_thumb.png
      image: /assets/images/dev/cms3.png
  ppbox:
  - title: Categorías
    thumb: /assets/images/dev/ppbox1_thumb.png
    image: /assets/images/dev/ppbox1.png
  - title: Crear o editar categoría
    thumb: /assets/images/dev/ppbox2_thumb.png
    image: /assets/images/dev/ppbox2.png
  - title: Subir imágenes
    thumb: /assets/images/dev/ppbox3_thumb.png
    image: /assets/images/dev/ppbox3.png
---

## Gestor de Contentidos Universal

Desarrollar un sitio web puede ser una tarea relativamente sencilla. Pero todo sitio web precisa de un sistema que permita a los usuarios modificar el contendido de cada una de sus páginas o secciones. Mediante esta aplicación podemos gestionar el contenido de cualquier sitio web, **sin importar cómo haya sido desarrollado**. Y aquí radica su principal virtud, ya que permite al equipo de desarrollo usar el framework que consideren más oportuno.

Se trata de un Gestor de Contenidos que hemos usado en infinidad de proyectos, con gran satisfacción por parte del cliente, y de la que nos sentimos especialmente orgullosos.

*("Click" para ampliar)*

<div class="thumbnails">
  {%- for image in page.images.cms -%}
  <a href="{{ image.image }}"><img src="{{ image.thumb }}" alt="" title="{{ image.title }}" ></a>
  {%- endfor -%}
</div>

## Gestor de fotografías online

Mediante esta aplicación podemos organizar nuestras fotos en categorías y subcateogorías, pudiendo una misma foto pertenecer a varias categorías. Además, la aplicación sube las fotos a tu cuenta de Dropbox, de manera que puedas disponer de ellas en cualquier momento y lugar.

*("Click" para ampliar)*

<div class="thumbnails">
  {%- for image in page.images.ppbox -%}
  <a href="{{ image.image }}"><img src="{{ image.thumb }}" alt="" title="{{ image.title }}" ></a>
  {%- endfor -%}
</div>
