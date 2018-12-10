# Análisis automático de accesibilidad web en sitios de universidades argentinas

Se recrea el análisis propuesto en [Accesibilidad de los Sitios Web de las Universidades Argentinas [2016]](http://sedici.unlp.edu.ar/bitstream/handle/10915/65715/Documento_completo.pdf-PDFA.pdf?sequence=1&isAllowed=y), presentado en el [Simposio Argentinode Informática en el Estado](http://47jaiio.sadio.org.ar/sie), automatizando el proceso de búsqueda de páginas de oferta académica, contacto o página con formulario y de evaluación en [TAW](https://www.tawdis.net) con [taw_scraper](https://github.com/lbellomo/taw_scraper).

## Cómo usar

```
git clone https://github.com/brivadeneira/analisis-accesibilidad-web-universidades-argentinas
cd analisis-accesibilidad-web-universidades-argentinas
pip -r install requirements.txt
```

En el directorio `datos` guardar un archivo *.csv* con la lista de sitios a analizar.

En el notebook `test-accesibilidad.ipynb`, en la línea `df = pd.read_csv('datos/unrc/unrc_sub_domains.csv', header=None)` reemplazar *'datos/unrc/unrc_sub_domains.csv'* por *'datos/nombre-de-archivo.csv'*

Descargar el scraper de taw:

```
git clone https://github.com/lbellomo/taw_scraper
```

En el último bloque del notebook, guardar en la variable `path-taw-scraper` por el path donde se encuentre el .py del scraper.

## Motivación

Se desarrollan los algoritmos de automatización de análisis de accesibilidad web para visibilizar las problemáticas al respecto en la Universidad Nacional de Río Cuarto, habiendo una [Comisión de atención a las personas con discapacidad](https://www.unrc.edu.ar/unrc/bienestar/com-atencion-pers-disc.php).


## Descripción

A partir de un *csv* con direcciones web de universidades argentinas, y según [Accesibilidad de los Sitios Web de las Universidades Argentinas [2016]](http://sedici.unlp.edu.ar/bitstream/handle/10915/65715/Documento_completo.pdf-PDFA.pdf?sequence=1&isAllowed=y), presentado en el [Simposio Argentinode Informática en el Estado](http://47jaiio.sadio.org.ar/sie), se busca y analiza:

* **Página principal**, (de la lista de sitios a analizar).
* **Página con oferta académica**, (buscando las palabras: "oferta", "carreras").
* **Página con formulario**, (se evalúa si la página de "contacto" contiene uno, de lo contrario se busca en todos los links de la página principal, siempre que no salgan del dominio del mismo.)

Se analizan las páginas con [Web accessibility test](https://www.tawdis.net), usando [un scraper de tawdis.net](https://github.com/lbellomo/taw_scraper) desarrollado especialmente para este análisis, obteniendo un *diccionario* con los resultados, según los [criterios de análisis de accesibilidad web](criterios-analisis.md)

Se asigna un resultado final para cada sitio de la lista, a partir del resultados de las páginas analizadas para cada uno de ellos (página principal, oferta académica, y página de contacto u otra con formulario), según:

* Si alguna de las páginas resulta en un **no**, resultado final = no.
* Si alguna de las páginas resulta en un **si**, resultado final = si.
* Si todas las páginas resultan en un **na**, resultado final = na.

Finalmente se asigna un **puntaje final** a cada sitio en cuestión:

Puntaje = (cant si + cant na) * 4

La ley vigente en Argentina exige un **puntaje mínimo de 100 puntos** (propuesto en dos etapas, de 80 y 100 correspondientemente)
