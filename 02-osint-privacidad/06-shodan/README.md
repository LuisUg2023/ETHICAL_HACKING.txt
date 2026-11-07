# EH-09 — Práctica de laboratorio: Búsquedas de Shodan

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** OSINT técnico / reconocimiento de activos expuestos  
**Entorno indicado por Cisco:** Kali Linux personalizado para Ethical Hacker y acceso a Internet  
**Estado:** Contenido documentado a partir del laboratorio adjunto; ejecución y evidencias reales pendientes.

## 1. Objetivos del laboratorio

- Crear una cuenta de Shodan y obtener una clave API.
- Utilizar el sitio web de Shodan para buscar dispositivos IoT.
- Examinar la información general y los puertos asociados a un resultado.
- Aplicar filtros para refinar búsquedas.
- Utilizar Shodan desde la línea de comandos (CLI).

## 2. Trasfondo y alcance

Shodan es un motor de búsqueda de dispositivos y servicios conectados a Internet. Puede indexar información de teléfonos, dispositivos inteligentes, cámaras, servidores y otros sistemas. Los resultados ayudan a comprender qué servicios son visibles desde Internet y qué información anuncian.

Los dispositivos IoT pueden estar mal configurados o no recibir mantenimiento adecuado. La organización propietaria debe inventariarlos, limitar su exposición y mantenerlos actualizados.

**Regla de autorización:** este laboratorio se realiza únicamente sobre sistemas, dominios, direcciones IP o rangos que sean propios o para los que exista autorización expresa. No intentes iniciar sesión en dispositivos encontrados, probar contraseñas predeterminadas, explotar vulnerabilidades ni acceder a cámaras o paneles ajenos. Una búsqueda en Shodan no constituye autorización para interactuar con el activo.

Las consultas públicas de demostración incluidas por el material Cisco (por ejemplo, `webcam` y la búsqueda FTP de San José) pueden mostrar activos de terceros. Se documentan para reconocer la sintaxis y el propósito didáctico, pero no se deben usar para seleccionar, contactar, escanear o acceder a dispositivos ajenos. No publiques IP, banners ni datos identificables de terceros.

## 3. Recursos necesarios

- Máquina virtual Kali Linux del curso Ethical Hacker.
- Acceso a Internet.
- Navegador web.
- Cuenta de Shodan.
- Shodan CLI (el material Cisco indica que la biblioteca se encuentra instalada por defecto en la VM del curso; comprobarlo en la versión utilizada).

## 4. Parte 1 — Crear una cuenta de Shodan y obtener una clave API

1. Inicia sesión en la VM Kali Linux.
2. Abre el navegador y visita https://www.shodan.io/.
3. Selecciona **Login** y luego **Register** para crear una cuenta, si todavía no tienes una.
4. Completa el registro y activa la cuenta desde el correo de confirmación.
5. Inicia sesión y revisa **Getting Started**, en particular **Search Query Fundamentals**.
6. En la sección de cuenta, abre **Account > Overview** y localiza la clave API.

**Protección de la clave API:** la clave es un secreto. No la pegues en este README, capturas, repositorios, scripts públicos ni mensajes. Si necesitas mostrar el procedimiento, utiliza el marcador `TU_API_KEY`. Si una clave se expone, revócala o rótala desde la cuenta de Shodan.

### Pregunta de comprobación 1

**Según Shodan, ¿cuál es la unidad fundamental de datos que recopila?**

Respuesta del material Cisco: el **banner** (el contenido anunciado por un servicio y registrado por Shodan).

## 5. Parte 2 — Usar el sitio web de Shodan

### Paso 1. Realizar una búsqueda de dispositivos IoT

El laboratorio Cisco propone introducir `webcam` en la barra de búsqueda. La página de resultados presenta estadísticas de resumen, por ejemplo:

- Cantidad de resultados que coinciden con la consulta.
- Países principales.
- Organizaciones.
- Productos.
- Sistemas operativos.

La cantidad de resultados visibles depende del estado de sesión, el tipo de cuenta, los créditos y las condiciones vigentes del servicio; no se debe asumir que los límites descritos en una versión antigua del curso siguen siendo iguales.

**Actividad segura:** utiliza la consulta general únicamente para comprender la interfaz y las estadísticas agregadas. No abras ni interactúes con dispositivos de terceros. Si el ejercicio exige examinar una ficha individual, utiliza un activo propio o un objetivo de laboratorio expresamente autorizado.

### Preguntas de comprobación 2–5

1. **¿Cuál es el principal país con cámaras web encontrado por Shodan?**  
   Respuesta histórica del material Cisco: Estados Unidos. Los resultados actuales pueden cambiar.

2. **¿Qué información se incluye en la sección Información general?**  
   Nombres de host, dominios, país, ciudad, organización, ISP y ASN.

3. **¿Qué puertos están abiertos en la dirección IP seleccionada?**  
   La respuesta depende del activo observado. El material menciona como ejemplos de puertos comunes para cámaras web 8081, 8088 y 80; no son resultados garantizados ni deben atribuirse a un dispositivo sin evidencia.

4. **¿Qué información está disponible para los puertos abiertos?**  
   Según el servicio, pueden aparecer encabezados, páginas web, certificados y otra información del servicio.

### Paso 2. Comprender los filtros de Shodan

El material Cisco presenta la sintaxis `filtro:valor`. Si el valor contiene espacios, se escribe entre comillas dobles.

| Filtro del material | Propósito |
|---|---|
| `country:XX` | Código de país de dos letras. |
| `city:nombre` | Ciudad. |
| `region:nombre` | Región o estado. |
| `product:nombre` | Producto identificado. |
| `version:XX` | Versión de un producto. |
| `vuln:CVE-AAAA-NNNN` | Resultados asociados a un identificador CVE, si la función está disponible. |

El laboratorio Cisco incluye estos ejemplos didácticos:

```text
webcam city:Toronto
port:21 country:US region:CA city:"San Jose" 230
```

La segunda consulta combina el puerto TCP 21 (FTP), filtros geográficos y el texto `230`, código de respuesta FTP asociado a un inicio de sesión satisfactorio.

**Advertencia:** son consultas históricas del material del curso, no instrucciones para investigar o acceder a servidores ajenos. No utilices sus resultados para probar inicios de sesión anónimos ni para conectarte a los servicios. Para una evaluación práctica, limita los filtros a activos bajo tu control y evita publicar resultados de terceros. La sintaxis y disponibilidad de filtros pueden cambiar según la cuenta y la versión del servicio.

### Preguntas de comprobación 6–7

1. **¿Cuántos servidores FTP encontró Shodan en San José que permitían inicios de sesión anónimos?**  
   El material Cisco señala que, al redactarse el curso, el resultado era 847. Es un dato histórico y variable; no representa el resultado actual.

2. **¿Qué información adicional puede aparecer en Información general respecto del resultado anterior?**  
   Proveedor de nube, región de nube y servicio en la nube.

## 6. Parte 3 — Utilizar Shodan desde la CLI

### Paso 1. Comprobar la instalación

En la VM Kali del curso, abre una terminal y ejecuta:

```bash
command -v shodan
shodan -h
```

- `command -v shodan`: comprueba si el ejecutable está disponible en el PATH.
- `shodan -h`: muestra la ayuda y los comandos disponibles.

El material Cisco indica que Shodan se instala por defecto en la VM del curso. Si el comando no existe, consulta la documentación oficial de instalación para tu versión de Kali; no asumas que el paquete está presente en cualquier instalación.

### Paso 2. Inicializar la CLI

En la web de Shodan, entra en **Account > Overview** y copia la clave API. Después, en la terminal:

```bash
shodan init TU_API_KEY
```

Sustituye `TU_API_KEY` por la clave real únicamente en tu terminal local. No guardes el valor en el repositorio ni compartas una captura donde sea visible. El material Cisco espera el mensaje `Successfully initialized`.

### Paso 3. Mostrar la ayuda

```bash
shodan -h
```

Revisa los comandos disponibles en la versión instalada.

### Paso 4. Realizar la búsqueda CLI indicada por el curso

El material Cisco muestra:

```bash
shodan search webcam
```

Esta consulta es una demostración amplia del curso y puede devolver IP y puertos de terceros. No la uses para seleccionar objetivos ni para acceder a dispositivos. En un ejercicio práctico propio, limita la búsqueda a un activo autorizado, por ejemplo:

```bash
shodan search --limit 20 'hostname:DOMINIO_PROPIO'
```

Sustituye `DOMINIO_PROPIO` por un dominio bajo tu control. La opción `--limit` limita la cantidad de resultados mostrados. La disponibilidad de filtros en CLI puede depender del tipo de cuenta y de los créditos.

### Paso 5. Consultar información adicional

El material Cisco incluye estos comandos:

```bash
shodan info
shodan myip
shodan stats webcam
```

- `shodan info`: muestra información de la cuenta y créditos de consulta/escaneo disponibles. Los créditos pueden ser cero en cuentas sin créditos.
- `shodan myip`: muestra la dirección IP pública de origen que Shodan observa para la conexión.
- `shodan stats webcam`: muestra estadísticas resumidas de la consulta. La consulta `webcam` es amplia; úsala solo como demostración de estadísticas y no investigues ni contactes los resultados.

Los comandos disponibles y los permisos de cuenta pueden variar. Consulta `shodan -h` y la documentación oficial si algún subcomando no está disponible.

## 7. Interpretación técnica

Al revisar información de un activo propio/autorizado, registra únicamente lo necesario:

| Elemento | Interpretación |
|---|---|
| Banner | Unidad de datos fundamental descrita por Shodan; contiene información anunciada por un servicio. |
| Hostname / dominio | Nombre asociado al activo indexado. |
| País / ciudad / región | Ubicación aproximada asociada al registro; no equivale a una ubicación física exacta. |
| Organización / ISP / ASN | Datos de red y proveedor asociados. |
| Puerto | Punto de escucha detectado por Shodan en la observación indexada. |
| Producto / versión | Identificación anunciada; puede ser incompleta o inexacta. |
| Encabezados HTTP | Metadatos que pueden revelar servidor, tecnologías u otra información. |
| Certificado | Datos públicos del certificado TLS, cuando están disponibles. |
| Fecha de observación | Contexto temporal del dato; no confirma que el servicio siga expuesto ahora. |

Un banner o una versión identificada no confirma por sí sola una vulnerabilidad explotable. Contrasta la información con el inventario del propietario, la configuración real y los avisos del fabricante.

## 8. Registro de resultados

Completa esta tabla con tus observaciones reales. No rellenes los campos con valores inventados:

| Campo | Resultado |
|---|---|
| Fecha y hora | Pendiente |
| Versión de Kali | Pendiente |
| Versión de Shodan CLI | Pendiente |
| Cuenta/API configurada | Sí/No (nunca registrar la clave) |
| Tipo de consulta | Web / CLI |
| Consulta aplicada | Pendiente; sin secretos |
| Activo autorizado (identificador anonimizado) | Pendiente |
| Estadísticas resumidas | Pendiente |
| Puertos/servicios del activo propio | Pendiente |
| Observaciones | Pendiente |
| Riesgo validado por el propietario | Pendiente |
| Medida correctiva | Pendiente |

### Evidencias que puedes guardar

- Captura de la cuenta con la clave API completamente oculta.
- Salida de `shodan -h` y `shodan info`, ocultando información de cuenta que no sea necesaria.
- Consulta limitada a un dominio propio y resultados sanitizados.
- Captura de estadísticas sin IP ni datos de terceros.
- Análisis y recomendación defensiva.

No subas claves API, IP públicas sensibles, banners completos, datos personales ni exportaciones sin sanitizar. Guarda evidencias en `evidencias/EH-09/` solo después de revisarlas.

## 9. Pregunta de reflexión

**¿Qué características de Shodan son especialmente valiosas para los administradores de TI?**

Respuesta orientativa del material Cisco: la información de encabezados HTTP puede ayudar a identificar y reducir la divulgación de datos sensibles. Las búsquedas por CVE o producto pueden ayudar a localizar activos que requieren actualización o retirada. Para convertir un resultado en una acción, el administrador debe verificar que el activo le pertenece y validar la configuración desde su entorno.

## 10. Conclusiones y controles defensivos

- Shodan permite observar qué información de servicios conectados a Internet aparece indexada.
- Los banners, puertos, productos, encabezados y certificados ayudan a identificar exposición potencial.
- Los resultados son una fuente de reconocimiento, no una prueba definitiva de vulnerabilidad ni de estado actual.
- Los administradores deben mantener inventario, cerrar servicios innecesarios, restringir interfaces administrativas, aplicar actualizaciones, configurar TLS y reducir la divulgación de metadatos.
- La revisión debe realizarse dentro de un alcance autorizado y con evidencias sanitizadas.

## 11. Referencias

- Cisco NetAcad — *Práctica de laboratorio: Búsquedas de Shodan* (material proporcionado para esta documentación).
- Shodan: https://www.shodan.io/
- Shodan Help Center: https://help.shodan.io/
- Shodan CLI: https://cli.shodan.io/
- Shodan Developer API: https://developer.shodan.io/

---

**Integridad académica:** esta página documenta el procedimiento del laboratorio. No afirma que las búsquedas hayan sido ejecutadas ni que los datos históricos sean resultados actuales. Completa la sección de resultados únicamente con salidas observadas en tu Kali y dentro de un alcance autorizado.
