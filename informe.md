# Práctica 1: dos servidores web con Nginx y Docker

**Estudiante:** Pedro Pozo  
**Institución:** Instituto Técnico Superior Sudamericano  
**Ciclo:** Cuarto

## 1. Título

Creación y personalización de dos servidores web Nginx mediante contenedores Docker y manipulación de archivos en Linux.

## 2. Fundamentos:

Un servidor web recibe una solicitud del navegador y responde con el contenido de una página. En esta práctica se utilizó Nginx para mostrar documentos HTML. Son páginas estáticas porque su contenido está guardado en un archivo y no depende de una base de datos. Al visitar la dirección del servidor, el navegador recibe ese archivo y muestra los títulos, párrafos y demás elementos que contiene. ([NGINX, s. f.](https://nginx.org/en/docs/beginners_guide.html))

Docker permite ejecutar programas en contenedores. Una imagen contiene los archivos que sirven como base para crearlos; el contenedor es la instancia que se pone en funcionamiento. En este caso se utilizó la misma imagen de Nginx para crear dos contenedores, llamados `nginx1` y `nginx2`. Aunque parten de la misma imagen, cada uno tiene sus propios archivos y puede mostrar una página distinta. La opción `-d` permite dejarlos funcionando en segundo plano. ([Docker, s. f.-c](https://docs.docker.com/engine/containers/run/))

Para acceder a las páginas es necesario publicar los puertos. Nginx escucha en el puerto 80 dentro de cada contenedor. En la computadora se asignó el puerto 8089 al primero y el 8090 al segundo. Por eso, `8089:80` significa que las solicitudes recibidas en el puerto 8089 se dirigen al puerto 80 de `nginx1`. Usar puertos distintos en el anfitrión permite consultar ambos servidores al mismo tiempo. ([Docker, s. f.-b](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/))

El archivo `index.html` contiene la página inicial del servidor. HTML organiza la información mediante etiquetas: por ejemplo, `h1` sirve para un título y `p` para un párrafo. La codificación UTF-8 permite mostrar correctamente las tildes. En el primer documento se colocó el nombre del instituto y en el segundo, el nombre del estudiante y su ciclo. Los estilos CSS incluidos en cada archivo dan formato a esos datos. ([MDN Web Docs, s. f.](https://developer.mozilla.org/es/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax))

El comando `docker cp` permite copiar archivos entre la computadora y un contenedor. El proceso consiste en sacar el archivo original, editarlo y devolverlo a la misma ruta. Las copias locales se llamaron `index1.html` e `index2.html` para distinguirlas, pero dentro de cada contenedor conservaron el nombre `index.html`. Antes de copiar hay que revisar la carpeta de trabajo y el nombre del contenedor para no modificar el servidor equivocado. ([Docker, s. f.-a](https://docs.docker.com/reference/cli/docker/container/cp/))

La comprobación se realizó desde la terminal y el navegador. `docker ps` permitió revisar el estado de los contenedores y `curl` obtuvo las páginas por HTTP. La respuesta 200 indicó que la solicitud se atendió correctamente. También se revisaron los textos y se compararon las respuestas con los archivos locales. Así se comprobó que cada puerto mostraba la página correspondiente.

<img src="images/arquitectura.png" alt="Diagrama del navegador, los puertos 8089 y 8090 y los dos contenedores Nginx" width="800">

*Figura 1. Esquema de los dos contenedores y sus puertos.*

<img src="images/flujo-archivos.png" alt="Diagrama de copia del archivo desde cada contenedor, edición local y devolución como index.html" width="800">

*Figura 2. Copia, edición y devolución del archivo HTML.*

## 3. Conocimientos previos.

- Uso de una terminal Linux y comandos como `pwd`, `mkdir`, `cd`, `ls` y `cat`.
- Diferencia entre rutas absolutas y relativas.
- Edición y guardado de archivos con Nano o Vi.
- Estructura básica de un documento HTML y codificación UTF-8.
- Diferencia entre imagen, contenedor, anfitrión y puerto.
- Uso del navegador, captura de pantalla y manejo básico de GitHub.

## 4. Objetivos a alcanzar

- Crear dos contenedores llamados `nginx1` y `nginx2` a partir de la imagen `nginx`.
- Publicar sus servicios en los puertos 8089 y 8090 del anfitrión.
- Copiar, editar y devolver el archivo `index.html` de cada contenedor.
- Mostrar información institucional en el primero e información del estudiante en el segundo.
- Verificar el servicio HTTP y la correspondencia entre cada puerto y su contenido.
- Documentar la práctica en Markdown y compartir el informe mediante GitHub.

## 5. Equipo necesario:

| Recurso | Entorno verificado o recurso para la entrega |
| --- | --- |
| Computador | Equipo con Windows y Ubuntu 24.04.4 LTS mediante WSL2 |
| Terminal | Sesión Linux dentro de Ubuntu/WSL |
| Docker | Cliente y servidor Docker Engine 29.8.0, arquitectura Linux/amd64 |
| Imagen | Imagen oficial `nginx:latest`; Nginx 1.31.7 comprobado en ambos contenedores |
| Editor | Vi disponible en `/usr/bin/vi`, utilizado en modo Ex |
| Comprobación | Navegador y `curl` |
| Conectividad | Acceso a Docker Hub y GitHub |
| Cuenta | Cuenta GitHub del estudiante para publicar el repositorio |
| Audio | Micrófono y herramienta que permita guardar o exportar MP3 |

El registro contiene las salidas de `docker version`, `/etc/os-release` y `nginx -v` en ambos contenedores. La imagen descargada tiene el identificador `sha256:bc4cad49969aaf95eb4eecec5572846f86e59f1e8332aba36ed64b91aff8b724`. Los comandos de la sección siguiente se ejecutan dentro de Linux, aunque el navegador se abra desde Windows.

## 6. Material de apoyo.

- Consigna de la Práctica 1 proporcionada por la asignatura.
- [Plantilla del informe](https://github.com/maguaman2/informe-tendencias/blob/main/informe.md).
- [Docker: ejecución de contenedores](https://docs.docker.com/engine/containers/run/).
- [Docker: copia de archivos](https://docs.docker.com/reference/cli/docker/container/cp/).
- [Docker: publicación de puertos](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/).
- [Imagen oficial de Nginx](https://hub.docker.com/_/nginx).
- [Guía introductoria de Nginx](https://nginx.org/en/docs/beginners_guide.html).
- [Manual de Nano](https://www.nano-editor.org/dist/latest/nano.html).
- [Video guía indicado por el docente](https://drive.google.com/file/d/1UTxRWaQPCHITq7u5l0mXEmRk49m__tlf/view). Recurso indicado en la consigna.

## 7. Procedimiento

La práctica se realizó desde la terminal de Ubuntu. Primero se crearon los dos contenedores, luego se cambió el contenido de sus páginas y al final se revisó el resultado en el navegador.

### Paso 1. Preparar el trabajo

Se utilizó una carpeta para guardar los archivos de la práctica. Antes de crear los servidores, se comprobó que Docker estuviera funcionando:

```bash
docker version
```

### Paso 2. Crear los dos contenedores

Se creó el primer contenedor con el nombre `nginx1` y el puerto 8089:

```bash
docker run -d --name nginx1 -p 8089:80 nginx
```

Después se creó `nginx2` con el puerto 8090:

```bash
docker run -d --name nginx2 -p 8090:80 nginx
```

Los dos utilizaron la misma imagen de Nginx. Se asignó un puerto diferente a cada uno para poder abrir sus páginas por separado.

### Paso 3. Copiar la página del primer contenedor

Se copió el archivo `index.html` de `nginx1` a la carpeta de trabajo. La copia se llamó `index1.html` para identificarla:

```bash
docker cp nginx1:/usr/share/nginx/html/index.html ./index1.html
```

### Paso 4. Personalizar la página institucional

Se editó `index1.html` con Vi, un editor de texto que funciona en la terminal. Se reemplazó la página de bienvenida de Nginx por el nombre del Instituto Técnico Superior Sudamericano y una descripción de la práctica. El contenido completo está en [index1.html](index1.html).

Durante la edición se borró por error la última línea del archivo. Al revisar la copia se detectó el problema y se corrigió antes de enviarla al contenedor.

Una vez guardados los cambios, se devolvió el archivo a `nginx1`:

```bash
docker cp index1.html nginx1:/usr/share/nginx/html/index.html
```

Dentro del contenedor se mantuvo el nombre `index.html`, que corresponde a la página inicial de Nginx.

### Paso 5. Copiar la página del segundo contenedor

Se repitió el proceso con `nginx2`. Esta vez la copia se guardó como `index2.html`:

```bash
docker cp nginx2:/usr/share/nginx/html/index.html ./index2.html
```

### Paso 6. Personalizar la página del estudiante

Se editó el segundo archivo con Vi para colocar los datos de Pedro Pozo, el nombre del instituto y el cuarto ciclo. El resultado está en [index2.html](index2.html).

Después de guardar y revisar el contenido, se copió el archivo al segundo contenedor:

```bash
docker cp index2.html nginx2:/usr/share/nginx/html/index.html
```

### Paso 7. Comprobar las dos páginas

Se revisó con `docker ps` que los dos contenedores estuvieran activos. Luego se abrió cada dirección en el navegador:

- [http://localhost:8089](http://localhost:8089/): mostró la información del instituto.
- [http://localhost:8090](http://localhost:8090/): mostró la información de Pedro Pozo.

También se comprobó desde la terminal que ambas páginas respondieran correctamente. Las dos devolvieron el código HTTP 200 y su contenido coincidió con los archivos guardados en la computadora.

### Paso 8. Guardar las evidencias y publicar el informe

Se guardaron capturas de las dos páginas y las copias de los archivos HTML. Las capturas aparecen en la sección de resultados. Los comandos y las comprobaciones se pueden consultar en el [registro de ejecución](evidencias/registro-ejecucion.txt), dentro de la carpeta [evidencias](evidencias/).

Finalmente, se organizó el informe en Markdown y se subió a GitHub junto con las páginas y las imágenes.

**Enlace de entrega:** [Informe de la Práctica 1 en GitHub](https://github.com/pozosaurio-stack/practica-1-nginx-docker/blob/main/informe.md).

El audio con la reflexión del estudiante se entrega por separado en formato MP3. La consigna pide una duración de 60 segundos.

## 8. Resultados esperados:

| Aspecto | Resultado esperado | Resultado observado |
| --- | --- | --- |
| Contenedores | `nginx1` y `nginx2` activos | Ambos figuran activos en el registro, con `8089:80` y `8090:80` respectivamente |
| Primer servicio | Puerto 8089, HTTP 200, información institucional | HTTP 200; se verificaron «Información institucional» y «Instituto Técnico Superior Sudamericano» |
| Segundo servicio | Puerto 8090, HTTP 200, información de Pedro Pozo | HTTP 200; se verificaron «Información personal» y «Pedro Pozo» |
| Copias de archivos | Ambos HTML servidos coinciden con las copias locales | `cmp` sin diferencias y hashes SHA-256 iguales en cada pareja |
| Capturas | Evidencia real, con títulos y ancho máximo de 800 px | Dos capturas de las páginas servidas por HTTP, incluidas abajo |
| Informe Markdown | Documento con evidencias y fuentes | Informe, páginas, registros e imágenes organizados en el repositorio `practica-1-nginx-docker`; enlace de entrega en el paso 8 |
| Reflexión oral | MP3 con voz del estudiante y duración de 60 segundos | Grabación preparada por el estudiante para adjuntarla a la entrega |

<img src="evidencias/04-institucional.png" alt="Captura real del navegador con la página institucional servida por nginx1 en el puerto 8089" width="800">

*Figura 3. Página institucional del Instituto Técnico Superior Sudamericano servida por nginx1. Dirección consultada: [localhost:8089](http://localhost:8089/).*

<img src="evidencias/05-personal.png" alt="Captura real del navegador con la página personal de Pedro Pozo servida por nginx2 en el puerto 8090" width="800">

*Figura 4. Página personal de Pedro Pozo servida por nginx2. Dirección consultada: [localhost:8090](http://localhost:8090/).*

**Conclusión:** los dos servidores funcionaron con la misma imagen de Nginx y mostraron contenidos distintos. El puerto 8089 presentó la información del instituto y el 8090, los datos de Pedro Pozo. La práctica permitió comprobar cómo se copia y modifica una página dentro de un contenedor. El error de edición también mostró la importancia de revisar el archivo antes de devolverlo al servidor. Las dos respuestas HTTP fueron 200 y coincidieron con los HTML guardados en la computadora.

## 9. Bibliografía

- Docker. (s. f.-a). *[docker container cp](https://docs.docker.com/reference/cli/docker/container/cp/)*. Docker Docs. Recuperado el 19 de octubre de 2026.
- Docker. (s. f.-b). *[Publishing and exposing ports](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/)*. Docker Docs. Recuperado el 19 de octubre de 2026.
- Docker. (s. f.-c). *[Running containers](https://docs.docker.com/engine/containers/run/)*. Docker Docs. Recuperado el 19 de octubre de 2026.
- Docker Official Images. (s. f.). *[nginx](https://hub.docker.com/_/nginx)*. Docker Hub. Recuperado el 19 de octubre de 2026.
- GNU nano. (s. f.). *[The GNU nano text editor](https://www.nano-editor.org/dist/latest/nano.html)*. Recuperado el 19 de octubre de 2026.
- maguaman2. (s. f.). *[informe.md](https://github.com/maguaman2/informe-tendencias/blob/main/informe.md)* [Plantilla de informe]. GitHub. Recuperado el 19 de octubre de 2026.
- MDN Web Docs. (s. f.). *[Sintaxis básica de HTML](https://developer.mozilla.org/es/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)*. Recuperado el 19 de octubre de 2026.
- NGINX. (s. f.). *[Beginner’s guide](https://nginx.org/en/docs/beginners_guide.html)*. Recuperado el 19 de octubre de 2026.
