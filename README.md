# Práctica 1: dos servidores web con Nginx y Docker

**Estudiante:** Pedro Pozo  
**Institución:** Instituto Técnico Superior Sudamericano  
**Ciclo:** Cuarto

Esta práctica desarrolla la creación de dos contenedores Nginx y la manipulación de sus archivos HTML desde Linux. El primer servidor presenta información institucional; el segundo, información del estudiante. El procedimiento, los fundamentos, las comprobaciones y las evidencias se documentan en el [informe Markdown](informe.md).

| Contenedor | Archivo local | Contenido | Puerto del anfitrión → contenedor |
| --- | --- | --- | --- |
| `nginx1` | [index1.html](index1.html) | Información institucional | `8089 → 80` |
| `nginx2` | [index2.html](index2.html) | Información personal de Pedro Pozo | `8090 → 80` |

Los archivos locales tienen nombres diferentes para distinguirlos. Dentro de cada contenedor, ambos se publican en `/usr/share/nginx/html/index.html`.

## Contenido de la entrega

- [Informe de la práctica](informe.md), organizado según los diez apartados de la plantilla docente.
- [Página institucional](index1.html) y [página personal](index2.html).
- Diagramas explicativos en [images/](images/) y registros de comprobación en [evidencias/](evidencias/).
- [Guion de apoyo para el audio](audio/guion-audio.md), que Pedro debe adaptar a su experiencia y grabar con su propia voz.

**Audio requerido:** Pedro debe grabar una reflexión de **60 segundos** y exportarla como `reflexion-pedro-pozo.mp3`. El guion escrito no sustituye ese archivo. El MP3 se adjunta a la entrega junto con el enlace del informe.

## Consulta y reproducción

Seguir los pasos de [la sección 8 del informe](informe.md#8-procedimiento) desde una terminal Linux con Docker disponible. Las direcciones del ejercicio son [localhost:8089](http://localhost:8089/) y [localhost:8090](http://localhost:8090/). Solo responden cuando los contenedores están funcionando en el equipo correspondiente; no son enlaces de alojamiento público.

Para consultar nuevamente los contenedores ya creados, abrir Ubuntu/WSL, esperar a que `sudo docker info` responda y ejecutar:

```bash
sudo docker start nginx1 nginx2
```

Mantener abierta la sesión de Ubuntu mientras se consultan las páginas. No repetir `docker run` si los contenedores con esos nombres ya existen.

El informe se encuentra en la raíz del repositorio [practica-1-nginx-docker](https://github.com/pozosaurio-stack/practica-1-nginx-docker). El [enlace directo al informe](https://github.com/pozosaurio-stack/practica-1-nginx-docker/blob/main/informe.md) es el enlace de entrega.

## Número de semana

La consigna dice «semana 1» en el título y «semana 2» en la entrega. Estos materiales utilizan **Práctica 1** para identificar el ejercicio. Confirmar en el aula virtual cuál es la etiqueta correcta al enviarlo.
