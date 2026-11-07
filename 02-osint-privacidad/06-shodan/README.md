# EH-09 — Shodan: búsqueda de dispositivos y servicios expuestos

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** OSINT técnico / Superficie de exposición  
**Entorno:** Kali Linux y navegador web  
**Estado:** Guía documentada; consultas y evidencias reales pendientes.

## 1. Objetivos
- Comprender qué información indexa Shodan.
- Consultar activos y servicios expuestos mediante filtros.
- Interpretar banners, puertos, productos y metadatos.
- Diferenciar información indexada de una vulnerabilidad confirmada.
- Documentar la exposición de activos propios o autorizados.

> **Alcance:** consulta únicamente dominios, IP y rangos que controles o tengas autorización expresa para evaluar. No intentes iniciar sesión, explotar servicios ni probar credenciales. Una ficha visible en Shodan no autoriza a interactuar con el sistema. No publiques IP sensibles, banners privados ni nombres internos.

## 2. ¿Qué es Shodan?
Shodan es un motor de búsqueda que indexa información observada en servicios y dispositivos conectados a Internet. Sus resultados pueden incluir IP públicas, puertos, banners, nombres de producto, versiones y datos de certificados.

Un banner puede estar incompleto, desactualizado o ser inexacto. Una versión identificada no demuestra por sí sola que exista una vulnerabilidad explotable. La validación debe realizarla el propietario del activo.

## 3. Acceso web
1. Abre https://www.shodan.io/.
2. Revisa la documentación y las condiciones de uso.
3. Inicia sesión si necesitas funciones que requieran cuenta.
4. Limita las búsquedas a un dominio o IP propios/autorizados.
5. Conserva solo datos mínimos y sanitizados.

Los filtros, límites y funciones dependen de la cuenta y pueden cambiar.

## 4. Consultas y filtros
Ejemplos de sintaxis para el laboratorio. Sustituye los marcadores por activos propios o autorizados:

~~~text
hostname:DOMINIO_AUTORIZADO
domain:DOMINIO_AUTORIZADO
ip:IP_PUBLICA_AUTORIZADA
port:443 hostname:DOMINIO_AUTORIZADO
ssl:DOMINIO_AUTORIZADO
~~~

| Filtro | Uso general |
|---|---|
| hostname: | Busca resultados asociados a un nombre de host. |
| domain: | Busca resultados relacionados con un dominio. |
| ip: | Limita la búsqueda a una dirección IP. |
| port: | Filtra por puerto de servicio. |
| ssl: | Busca información relacionada con SSL/TLS; confirma la sintaxis vigente. |

Consulta la referencia oficial para conocer los filtros admitidos en el momento de realizar la práctica. No uses búsquedas amplias para localizar dispositivos vulnerables de terceros.

## 5. Shodan CLI en Kali Linux (opcional)

Comprueba si ya está instalada:
~~~bash
shodan version
~~~

Si utilizas pipx:
~~~bash
sudo apt update
sudo apt install pipx -y
pipx ensurepath
pipx install shodan
~~~

- **sudo apt update:** actualiza el índice de paquetes.
- **sudo apt install pipx -y:** instala pipx desde los repositorios configurados.
- **pipx ensurepath:** configura la ruta de ejecutables instalados por pipx.
- **pipx install shodan:** instala la CLI en un entorno aislado.

Si pipx solicita reiniciar la terminal para actualizar PATH, hazlo y luego comprueba:
~~~bash
shodan version
shodan --help
~~~

### 5.1 Configurar la API key
Obtén tu clave directamente desde tu cuenta de Shodan y configúrala localmente:

~~~bash
shodan init TU_API_KEY
~~~

No pegues la clave en este README, capturas o comandos compartidos. Protege el equipo y los archivos de configuración locales.

### 5.2 Consultas CLI dentro del alcance
~~~bash
shodan search --limit 20 'hostname:DOMINIO_AUTORIZADO'
shodan host IP_PUBLICA_AUTORIZADA
~~~

- **shodan search:** consulta el índice.
- **--limit 20:** limita la cantidad máxima de resultados mostrados.
- **hostname:** aplica el filtro de nombre de host.
- **shodan host:** consulta la ficha de una IP.
- Sustituye los marcadores por activos autorizados; no ejecutes los ejemplos literalmente.

La salida depende de la indexación y de los permisos de la cuenta. No se incorporan resultados supuestos.

## 6. Interpretación de resultados

| Campo | Qué revisar |
|---|---|
| IP / hostname | Confirmar que pertenece al inventario autorizado. |
| Puerto | Determinar si el servicio debe exponerse a Internet. |
| Transporte | TCP/UDP, cuando se muestre. |
| Producto / servicio | Identificación anunciada por el banner. |
| Versión | Puede estar ausente, desactualizada o ser inexacta. |
| Banner | Metadatos anunciados por el servicio; sanitizar al compartir. |
| Certificado TLS | Vigencia, nombres y emisor, si se muestran. |
| Fecha de observación | Momento en que Shodan observó el dato; no garantiza estado actual. |

### Validación de un posible hallazgo
1. Confirma que el activo pertenece al alcance.
2. Contrasta con el inventario y la configuración interna.
3. Valida desde el lado del propietario, sin explotar el servicio.
4. Contrasta versiones con avisos del fabricante.
5. Registra incertidumbres, falsos positivos y fecha de observación.
6. Comunica el hallazgo por un canal seguro.

## 7. Registro y evidencias

| Campo | Resultado |
|---|---|
| Fecha de consulta | Pendiente |
| Activo autorizado (etiqueta anonimizada) | Pendiente |
| Tipo de consulta | Web / CLI |
| Filtros utilizados | Pendiente |
| Número de resultados | Pendiente |
| Servicios identificados | Pendiente; resumen sanitizado |
| Posibles exposiciones | Pendiente de validar |
| Recomendación defensiva | Pendiente |

Evidencias sugeridas: captura de búsqueda con IP/dominio anonimizados, versión y ayuda de CLI, filtros usados y resumen sanitizado. No subas exportaciones completas, IP sensibles, datos personales, nombres internos ni tokens.

## 8. Preguntas de análisis
1. ¿Qué diferencia existe entre Shodan y un escáner que ejecuta pruebas directamente?
2. ¿Qué información puede revelar un banner?
3. ¿Por qué importa la fecha de observación?
4. ¿Por qué una versión detectada no confirma una vulnerabilidad?
5. ¿Qué riesgos supone dejar interfaces administrativas expuestas?
6. ¿Qué controles reducen la superficie de exposición?
7. ¿Cómo documentar un hallazgo sin revelar información sensible?

## 9. Recomendaciones defensivas
- Mantener inventario de activos y servicios expuestos.
- Cerrar puertos y servicios innecesarios.
- Restringir interfaces administrativas mediante VPN, ACL y MFA.
- Actualizar servicios y retirar versiones obsoletas.
- Configurar TLS y renovar certificados.
- Monitorizar la superficie de exposición externa.
- Reducir la divulgación innecesaria de versiones y detalles internos.
- Establecer un proceso de remediación.

## 10. Referencias
- Shodan: https://www.shodan.io/
- Shodan Help Center: https://help.shodan.io/
- Shodan CLI: https://cli.shodan.io/
- Shodan Developer API: https://developer.shodan.io/
- Cisco NetAcad — Material de reconocimiento OSINT.
