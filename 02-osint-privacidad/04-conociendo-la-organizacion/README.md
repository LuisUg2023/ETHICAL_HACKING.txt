# EH-06 — Conociendo más sobre la organización

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** Reconocimiento OSINT / Exposición de correos / Metadatos  
**Objetivos del laboratorio:**
1. Investigar de forma responsable si una dirección de correo propia o autorizada aparece en incidentes de exposición de datos.
2. Identificar direcciones de correo corporativas publicadas mediante EmailHarvester.
3. Utilizar SpiderFoot para organizar tareas de reconocimiento OSINT.
4. Consultar los metadatos y formatos de archivo compatibles con ExifTool.

**Entorno:** Kali Linux (máquina virtual del curso) y acceso a Internet.  
**Estado:** Guía documentada a partir del laboratorio Cisco. La ejecución, los resultados y las capturas reales están pendientes.

> **Alcance y privacidad:** trabaja únicamente con tu propio correo, cuentas de prueba o un dominio para el que tengas autorización. No busques, descargues ni difundas contraseñas, bases de datos robadas o información privada de terceros. Si detectas datos expuestos, registra solo la evidencia mínima, detén la consulta y notifica al responsable. No publiques direcciones de correo reales en GitHub.

## 1. Contexto

El reconocimiento de una organización puede revelar información publicada en Internet, como direcciones de correo, metadatos de documentos e información técnica relacionada con sus activos. Estos datos pueden ayudar a evaluar la exposición, pero deben tratarse como información potencialmente sensible.

Esta práctica reúne tres actividades del material Cisco: consulta responsable de exposición de correos, recolección de direcciones públicas con EmailHarvester y análisis de metadatos con ExifTool. SpiderFoot permite coordinar módulos de reconocimiento desde una interfaz web local.

## 2. Parte 1 — Comprobar exposición de una dirección de correo

### 2.1 Recursos mencionados en el laboratorio

- Have I Been Pwned: https://haveibeenpwned.com/
- F-Secure: https://www.f-secure.com/
- HackNotice: https://hacknotice.com/
- BreachDirectory: servicio de búsqueda de exposición; verifica legitimidad y condiciones antes de usarlo.
- Keeper: https://www.keepersecurity.com/

La disponibilidad, funciones y políticas de estos servicios pueden cambiar. Prioriza proveedores reconocidos y no introduzcas contraseñas en sitios de consulta de filtraciones.

### 2.2 Procedimiento

1. Selecciona una dirección de correo de prueba que controles.
2. Abre el servicio elegido y sigue su procedimiento de verificación.
3. Registra si el servicio informa incidentes asociados y las fechas o categorías que muestre.
4. No intentes obtener los archivos de la filtración ni reutilizar credenciales expuestas.
5. Si el correo pertenece a una organización, realiza la comprobación solo con autorización y coordina la comunicación con su responsable de seguridad.

### 2.3 Interpretación

- Un resultado positivo indica que el proveedor relaciona el correo con uno o más incidentes incluidos en su base de datos; no demuestra que la cuenta esté comprometida actualmente.
- Un resultado negativo no garantiza que la dirección nunca haya sido expuesta: las bases de datos pueden ser incompletas.
- Una exposición de contraseña requiere cambio inmediato de la contraseña afectada, revisión de reutilización en otros servicios y activación de MFA.
- No registrar en este repositorio el correo consultado, contraseñas, tokens ni detalles que permitan identificar a una persona.

**Registro de práctica (sin PII):**

| Campo | Resultado |
|---|---|
| Tipo de cuenta | Cuenta propia / cuenta de prueba / dominio autorizado |
| Servicio consultado | Pendiente |
| Fecha de consulta | Pendiente |
| ¿Se informó exposición? | Pendiente: sí / no / no concluyente |
| Número o categoría de incidentes | Pendiente; omitir datos sensibles |
| Medida recomendada | Pendiente |

## 3. Parte 2 — EmailHarvester: búsqueda de correos publicados para un dominio

EmailHarvester recopila direcciones de correo que aparecen en fuentes públicas asociadas a un dominio. Los resultados pueden estar incompletos, desactualizados o incluir falsos positivos.

### 3.1 Comprobar e instalar la herramienta

En la terminal de Kali:

```bash
emailharvester
```

Si Kali solicita instalar el paquete, confirma únicamente si proviene de los repositorios configurados y revisa la operación. Después consulta la ayuda:

```bash
emailharvester -h
```

**Qué hace cada comando:**
- `emailharvester`: intenta iniciar la herramienta; en una instalación nueva puede activar el mecanismo de instalación de Kali o indicar que el paquete no está disponible.
- `-h`: solicita la ayuda y muestra las opciones reconocidas por la versión instalada.

Si el comando no está disponible, busca el nombre del paquete en la documentación/repositorios actuales de Kali. No descargues scripts aleatorios para sustituir la herramienta.

### 3.2 Identificar la opción de dominio

Ejemplo de sintaxis del laboratorio:

```bash
emailharvester -d DOMINIO_AUTORIZADO
```

- `emailharvester`: ejecuta la herramienta.
- `-d`: indica que se realizará la búsqueda asociada a un dominio.
- `DOMINIO_AUTORIZADO`: reemplázalo por un dominio propio o expresamente autorizado; no escribas literalmente ese texto como objetivo.

El laboratorio Cisco menciona como ejemplos `h4cker.org`, `hackxor.net` y `scanme.nmap.org`. Que aparezcan en el material didáctico no constituye por sí solo autorización para realizar pruebas sobre ellos. Utiliza tu propio dominio o el alcance que el instructor haya autorizado.

### 3.3 Guardar resultados

La opción indicada por el laboratorio para especificar el nombre de salida es `-s`:

```bash
emailharvester -d DOMINIO_AUTORIZADO -s resultado-laboratorio
```

- `-s`: especifica el nombre base o destino de los archivos generados, según la versión instalada.
- El material del curso indica que se generan archivos XML y de texto. Comprueba el comportamiento exacto con `emailharvester -h`.
- Si no se especifica una ruta, el material menciona una carpeta predeterminada bajo `/usr/share/emailharvester`; verifica la ruta real en tu versión antes de asumir que existe.

Inspeccionar los archivos generados:

```bash
ls -lah resultado-laboratorio*
file resultado-laboratorio*
```

- `ls -lah`: lista archivos coincidentes, mostrando permisos, tamaño y detalles en formato legible.
- `file`: identifica el tipo de contenido del archivo.

**No subir los resultados originales a GitHub.** Pueden contener direcciones personales. Para evidencias públicas, reemplaza correos por valores como `usuario-alfa@dominio-ejemplo.invalid` y elimina cualquier dato identificativo.

## 4. Parte 3 — SpiderFoot para investigar indicadores OSINT

SpiderFoot automatiza consultas OSINT mediante módulos. Algunos módulos requieren claves API y pueden consultar servicios externos. Antes de habilitarlos, revisa su descripción, los datos que enviarán y sus condiciones de uso.

### 4.1 Iniciar la interfaz local

```bash
spiderfoot -l 127.0.0.1:5001
```

**Qué significa cada parte:**
- `spiderfoot`: inicia la aplicación SpiderFoot.
- `-l`: especifica la dirección de escucha.
- `127.0.0.1`: limita la escucha al equipo local (localhost); evita exponer la interfaz a otras redes.
- `:5001`: puerto TCP utilizado por la interfaz web.

Mantén abierta la terminal mientras utilizas la aplicación. En el navegador de Kali, abre:

```text
http://127.0.0.1:5001
```

Si el servicio no inicia, revisa el mensaje de error y la documentación de la versión instalada; no abras el puerto a Internet como solución.

### 4.2 Revisar módulos

1. En la interfaz, abre **Settings**.
2. Lee la descripción de los módulos disponibles.
3. Identifica cuáles sirven para investigar direcciones de correo, dominios, presencia pública y posibles incidentes.
4. Revisa si cada módulo necesita una API key, qué datos transmite y si tiene límites de consulta.
5. Configura solo los módulos necesarios y con claves obtenidas directamente del proveedor.

### 4.3 Crear un escaneo controlado

1. Abre **New Scan** y selecciona **By Module** si deseas elegir módulos específicos.
2. Selecciona módulos apropiados para un correo propio, una dirección de prueba o un dominio autorizado.
3. Introduce un nombre de escaneo que no contenga datos personales.
4. Define como objetivo únicamente el indicador autorizado.
5. Revisa la configuración y pulsa **Run Scan Now**.
6. Examina los resultados y anota la fuente, fecha, tipo de dato y nivel de confianza.

No selecciones módulos para buscar cuentas de terceros, extraer credenciales, acceder a archivos de filtraciones o realizar acciones intrusivas. Los resultados de SpiderFoot son pistas OSINT y deben verificarse con fuentes legítimas.

### 4.4 Módulos y servicios que aparecen en el laboratorio

El material enumera, entre otros, Ahmia, AccountFinder, Archive.org, Bing, Leak-Lookup, CommonCrawl, Dehashed, DuckDuckGo y EmailCrawlr. La disponibilidad de los módulos y sus requisitos pueden variar por versión.

No es necesario activar todos. Algunos servicios pueden tratar información sensible o requerir condiciones de acceso específicas. Para este ejercicio, prioriza fuentes públicas, módulos pasivos y objetivos de prueba propios.

## 5. Parte 4 — ExifTool: etiquetas y formatos compatibles

Esta parte complementa la práctica EH-04 de análisis EXIF. Aquí se revisan las opciones de ayuda y los formatos de archivo que ExifTool puede procesar.

### 5.1 Instalar ExifTool

```bash
sudo apt update
sudo apt install libimage-exiftool-perl -y
exiftool -ver
```

- `sudo apt update`: actualiza el índice de paquetes.
- `sudo apt install libimage-exiftool-perl -y`: instala ExifTool desde los repositorios configurados; `-y` confirma la instalación.
- `exiftool -ver`: muestra la versión instalada.

### 5.2 Consultar etiquetas y formatos

```bash
exiftool -list
exiftool -listf
```

**Qué hace cada comando:**
- `exiftool -list`: muestra las etiquetas de metadatos que ExifTool reconoce o puede procesar.
- `exiftool -listf`: muestra los tipos de archivo y formatos que la versión instalada reconoce.

### 5.3 Tabla de formatos de archivo

La lista exacta depende de la versión de ExifTool. Completa la tabla contrastando la salida de `exiftool -listf`; estos son ejemplos habituales, no una transcripción de tu equipo.

| Tipo | Formatos / extensiones de ejemplo |
|---|---|
| Documentos | PDF, TXT, DOC, DOCX, HTML |
| Audio | MP3, WAV, FLAC, M4A |
| Video | MP4, MOV, AVI, MKV |
| Gráficos / imágenes | JPG, JPEG, PNG, TIFF, GIF, WEBP |
| Otros archivos | ZIP, algunos formatos RAW de cámaras y formatos específicos de aplicaciones |

**Importante:** que ExifTool reconozca un formato no significa que todos los archivos de ese formato contengan metadatos, ni que todas las etiquetas sean legibles o editables. Verifica los formatos que aparecen realmente en tu versión.

Para inspeccionar un archivo de laboratorio propio:

```bash
exiftool archivo-prueba.jpg
```

Para la explicación completa de extracción, interpretación y saneamiento de metadatos, consulta [EH-04 — Análisis de metadatos EXIF](../02-analisis-metadatos-exif/README.md).

## 6. Registro de resultados y evidencias

Completa los campos únicamente después de ejecutar la práctica. No inventes direcciones, incidentes, resultados de SpiderFoot ni archivos encontrados.

| Actividad | Evidencia a conservar | Estado |
|---|---|---|
| Consulta de exposición de correo propio | Captura sanitizada y fecha; sin dirección de correo | Pendiente |
| EmailHarvester | Versión, comando sanitizado y resumen de resultados | Pendiente |
| SpiderFoot | Captura de interfaz local, módulos seleccionados y resumen | Pendiente |
| ExifTool `-list` | Captura o extracto de etiquetas | Pendiente |
| ExifTool `-listf` | Tabla de formatos verificados | Pendiente |

### Preguntas de análisis

1. ¿Qué utilidad tiene comprobar la exposición de una dirección de correo propia?
2. ¿Por qué un resultado negativo no demuestra que nunca haya existido una filtración?
3. ¿Qué opciones de EmailHarvester se utilizaron y qué archivos generó tu versión?
4. ¿Qué diferencia existe entre una dirección publicada en una fuente abierta y una credencial comprometida?
5. ¿Qué módulos de SpiderFoot seleccionaste y qué fuente alimentó cada resultado?
6. ¿Qué formatos reconoció realmente ExifTool en tu instalación?
7. ¿Qué medidas reducirían la exposición de información de empleados y documentos?

## 7. Recomendaciones defensivas

- Usar MFA y contraseñas únicas; cambiar credenciales expuestas y revisar su reutilización.
- Mantener inventario de correos corporativos y establecer un procedimiento de notificación de incidentes.
- Evitar publicar direcciones personales cuando no sean necesarias; preferir buzones funcionales.
- Revisar documentos e imágenes antes de publicarlos y eliminar metadatos innecesarios.
- Limitar permisos y acceso a documentos internos.
- Evaluar los resultados OSINT con contexto y fuentes verificables; no asumir que cada resultado es vigente o correcto.
- Mantener las API keys fuera de capturas, terminales compartidas y repositorios.

## 8. Referencias

- Cisco NetAcad — Laboratorio “Conociendo más sobre la organización”.
- Kali Linux Tools: https://www.kali.org/tools/
- ExifTool: https://exiftool.org/
- SpiderFoot: https://github.com/smicallef/spiderfoot
- Have I Been Pwned: https://haveibeenpwned.com/
