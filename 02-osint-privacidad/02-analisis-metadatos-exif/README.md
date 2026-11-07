# EH-04 — Análisis de metadatos EXIF en archivos

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** OSINT / Metadatos / Privacidad digital  
**Objetivo:** Examinar metadatos de imágenes para identificar información que pueda revelar dispositivo, fecha, software o ubicación, y proponer medidas para reducir la exposición.  
**Estado:** Guía reproducible. La ejecución y las evidencias deben completarse con un archivo de laboratorio propio.

> **Alcance:** utilizar fotografías propias, imágenes de prueba o archivos para los que se tenga autorización. No analizar ni publicar metadatos de fotografías privadas de terceros. Las coordenadas GPS y otros datos personales deben ocultarse en las evidencias.

## 1. ¿Qué son los metadatos EXIF?

EXIF (Exchangeable Image File Format) es un conjunto de metadatos que puede almacenarse en fotografías y otros archivos multimedia. Según el dispositivo y la configuración, puede contener:

- Marca y modelo de cámara o teléfono.
- Fecha y hora de captura.
- Orientación, dimensiones y parámetros de exposición.
- Software utilizado para editar o exportar la imagen.
- Coordenadas GPS, si la geolocalización estaba habilitada.
- Miniaturas u otros bloques de metadatos.

Los metadatos no siempre están presentes y pueden modificarse o eliminarse. La ausencia de GPS no demuestra que una imagen nunca haya tenido ubicación.

## 2. Preparar el entorno en Kali Linux

### 2.1 Comprobar e instalar ExifTool

```bash
command -v exiftool
sudo apt update
sudo apt install libimage-exiftool-perl -y
exiftool -ver
```

**Qué hace cada comando:**

| Comando | Explicación |
|---|---|
| `command -v exiftool` | Comprueba si el ejecutable está disponible en las rutas del sistema. |
| `sudo apt update` | Actualiza el índice local de paquetes; no instala programas por sí solo. |
| `sudo apt install libimage-exiftool-perl -y` | Instala ExifTool desde los repositorios configurados. `-y` confirma automáticamente la instalación. |
| `exiftool -ver` | Muestra la versión instalada para registrar el entorno de la práctica. |

### 2.2 Crear un directorio de trabajo

```bash
mkdir -p ~/laboratorios/eh04-exif/entrada
cd ~/laboratorios/eh04-exif
pwd
```

| Comando | Explicación |
|---|---|
| `mkdir -p ...` | Crea el directorio y sus carpetas superiores si no existen; no genera error si ya existen. |
| `cd ...` | Cambia el directorio de trabajo a la carpeta del laboratorio. |
| `pwd` | Imprime la ruta absoluta del directorio actual. |

Copia una fotografía propia a `entrada/` usando el explorador de archivos o un método local autorizado. No publiques la imagen original si contiene información privada.

### 2.3 Listar los archivos de entrada

```bash
find entrada -maxdepth 1 -type f -printf '%f\n'
```

- `find entrada`: busca elementos dentro de la carpeta indicada.
- `-maxdepth 1`: limita la búsqueda al primer nivel.
- `-type f`: muestra únicamente archivos regulares.
- `-printf '%f\n'`: imprime solo el nombre del archivo y un salto de línea.

## 3. Inspeccionar los metadatos

Sustituye `foto-prueba.jpg` por el nombre exacto de tu archivo:

```bash
exiftool entrada/foto-prueba.jpg
```

**Qué hace:** lee y presenta los metadatos reconocidos por ExifTool. No modifica el archivo.

Para consultar campos seleccionados:

```bash
exiftool -DateTimeOriginal -Make -Model -Software -GPSLatitude -GPSLongitude entrada/foto-prueba.jpg
```

- `-DateTimeOriginal`: fecha y hora de captura, si existe.
- `-Make`: fabricante del dispositivo.
- `-Model`: modelo del dispositivo.
- `-Software`: software registrado en los metadatos.
- `-GPSLatitude` y `-GPSLongitude`: coordenadas geográficas, si están presentes.
- La ruta final identifica el archivo que se examina.

> **Privacidad:** no copies coordenadas GPS, números de serie ni identificadores únicos a un repositorio público. Si aparecen, redacta esos valores antes de compartir la salida.

Guardar un informe de texto local:

```bash
exiftool entrada/foto-prueba.jpg > informe-exif.txt
```

- `>` redirige la salida estándar a un archivo.
- Si el archivo de destino ya existe, se sobrescribe.
- El informe puede contener PII; mantenlo local o sanitízalo antes de publicarlo.

## 4. Comparar antes y después de eliminar metadatos

ExifTool permite eliminar metadatos de una copia, conservando el original:

```bash
cp entrada/foto-prueba.jpg foto-sin-metadatos.jpg
exiftool -all= -overwrite_original foto-sin-metadatos.jpg
exiftool foto-sin-metadatos.jpg
```

| Elemento | Explicación |
|---|---|
| `cp origen destino` | Crea una copia de trabajo para no alterar el original. |
| `-all=` | Solicita eliminar los metadatos que ExifTool puede retirar. |
| `-overwrite_original` | Evita conservar una copia adicional con sufijo `_original`; úsalo solo sobre la copia de laboratorio. |
| Último `exiftool` | Vuelve a inspeccionar el archivo para comprobar qué metadatos permanecen. |

La eliminación puede no cubrir todos los metadatos de todos los formatos. Para compartir una imagen, también conviene exportarla desde una aplicación que permita quitar ubicación y datos identificativos, y verificar el resultado.

## 5. Registro de resultados

Completa esta tabla con la salida real de tu archivo. No rellenes los campos con valores inventados.

| Campo | Resultado observado |
|---|---|
| Nombre/código del archivo | Pendiente |
| Formato y dimensiones | Pendiente |
| Marca/modelo | Pendiente o no presente |
| Fecha de captura | Pendiente o no presente |
| Software | Pendiente o no presente |
| GPS | Pendiente; no publicar coordenadas |
| Metadatos retirados en la copia | Pendiente de verificación |

### Preguntas de análisis

1. ¿Qué metadatos estaban presentes y cuáles no?
2. ¿La imagen revelaba dispositivo, fecha o ubicación?
3. ¿Qué riesgos de privacidad podrían derivarse de esos datos?
4. ¿Qué diferencias se observaron tras limpiar la copia?
5. ¿Qué limitaciones tuvo la herramienta o el formato?

## 6. Riesgos y controles defensivos

**Riesgos:** exposición de ubicación residencial o laboral, inferencia de rutinas, identificación del dispositivo y correlación de fotografías con perfiles públicos.

**Controles:** desactivar geolocalización cuando no sea necesaria, revisar metadatos antes de publicar, compartir copias saneadas, limitar la información de las imágenes y verificar los archivos exportados.

## 7. Evidencias que deben adjuntarse

- Captura de `exiftool -ver`.
- Captura de la inspección de una imagen de prueba, con GPS, seriales y datos personales redactados.
- Captura de la inspección posterior a la limpieza.
- Tabla de resultados observados y conclusión.

**No se adjunta evidencia real todavía.** Esta documentación describe el procedimiento; los resultados deben proceder de la ejecución en Kali Linux.

## Referencias

- ExifTool: https://exiftool.org/
- ExifTool FAQ: https://exiftool.org/faq.html
