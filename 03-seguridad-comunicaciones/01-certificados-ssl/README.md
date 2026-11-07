# EH-04 — Identificación y análisis de certificados SSL/TLS con crt.sh y SSLScan

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** Seguridad de comunicaciones / Certificate Transparency  
**Dominio de práctica:** `h4cker.org`  
**Estado:** Guía documentada. El ejemplo de nombres y características de certificados es hipotético; las consultas reales deben ejecutarse y guardarse como evidencia.

> **Integridad académica:** crt.sh y SSLScan no se han ejecutado en esta sesión. No presentar los valores de ejemplo como hallazgos reales.

## 1. Objetivos
- Consultar certificados públicos registrados en Certificate Transparency (CT).
- Extraer nombres DNS (SAN) que aparezcan en los certificados.
- Inspeccionar el certificado que presenta un servidor mediante OpenSSL.
- Enumerar protocolos y suites TLS mediante SSLScan, únicamente con autorización.
- Diferenciar registros históricos de CT de servicios que estén activos actualmente.

## 2. Conceptos técnicos
- **SSL/TLS:** protocolos criptográficos que protegen comunicaciones en tránsito. SSL está obsoleto; en la práctica actual se analiza TLS.
- **Certificate Transparency (CT):** sistema de registros públicos que permite auditar certificados emitidos por autoridades certificadoras.
- **crt.sh:** interfaz de consulta de registros CT.
- **SAN (Subject Alternative Name):** extensión del certificado que enumera los nombres DNS cubiertos.
- **OpenSSL:** conjunto de herramientas criptográficas; `s_client` permite establecer una conexión TLS y `x509` inspecciona certificados X.509.
- **SSLScan:** herramienta que conecta con un servidor y enumera su configuración SSL/TLS. Es una prueba activa.

## 3. Entorno y herramientas
Kali Linux o Linux compatible; `curl`, `jq`, `openssl` y `sslscan`.

### Comprobar la disponibilidad
```bash
command -v curl
command -v jq
command -v openssl
command -v sslscan
```

**Qué hace cada línea:**
- `command -v curl`: muestra la ruta del ejecutable curl si está instalado.
- `command -v jq`: comprueba si jq está disponible para procesar JSON.
- `command -v openssl`: comprueba la disponibilidad de OpenSSL.
- `command -v sslscan`: comprueba si SSLScan está instalado.

### Instalar herramientas si faltan (Kali/Debian)
```bash
sudo apt update
sudo apt install curl jq openssl sslscan -y
```

**Qué hace cada línea:**
- `sudo apt update`: actualiza el índice local de paquetes; no instala ni actualiza por sí solo los programas.
- `sudo apt install curl jq openssl sslscan -y`: instala las cuatro herramientas; `-y` acepta automáticamente las confirmaciones del gestor de paquetes.

## 4. Parte A — Consulta pasiva de crt.sh

Abrir en el navegador: https://crt.sh/?q=%25.h4cker.org

Descargar los resultados JSON:
```bash
mkdir -p evidencias/EH-04
curl -fLsS 'https://crt.sh/?q=%25.h4cker.org&output=json' -o evidencias/EH-04/crtsh-h4cker.json
```

**Qué hace cada línea:**
- `mkdir -p evidencias/EH-04`: crea la carpeta para las evidencias; `-p` evita error si ya existe.
- `curl -fLsS 'URL' -o archivo.json`: solicita la consulta JSON a crt.sh y guarda la respuesta en el archivo indicado. `-f` falla ante errores HTTP, `-L` sigue redirecciones, `-s` reduce la salida y `-S` muestra errores, `-o` define el archivo de destino.

Extraer nombres DNS sin duplicados:
```bash
jq -r '.[].name_value' evidencias/EH-04/crtsh-h4cker.json | sort -u > evidencias/EH-04/subdominios-ct.txt
cat evidencias/EH-04/subdominios-ct.txt
```

**Qué hace cada línea:**
- `jq -r '.[].name_value' archivo.json`: recorre los objetos del arreglo JSON y muestra el campo `name_value` sin comillas.
- `| sort -u`: recibe esos nombres por la tubería, los ordena y elimina duplicados.
- `> evidencias/EH-04/subdominios-ct.txt`: guarda el resultado en un archivo; sobrescribe el archivo si ya existía.
- `cat evidencias/EH-04/subdominios-ct.txt`: muestra en pantalla la lista obtenida.

Consultar datos de los certificados:
```bash
jq -r '.[] | [.id, .issuer_name, .common_name, .name_value, .not_before, .not_after] | @tsv' evidencias/EH-04/crtsh-h4cker.json
```

**Qué hace la línea:**
- `jq -r`: procesa el JSON y evita comillas en la salida.
- `.[ ]` (en el comando se escribe `.[ ]` sin espacios como `.[ ]`? No): la expresión correcta es `.[]`, que recorre cada certificado.
- `[ ... ]`: selecciona ID, emisor, nombre común, nombres SAN y fechas de vigencia.
- `@tsv`: presenta los campos separados por tabuladores para facilitar su lectura.

**Interpretación:** `not_before` y `not_after` son fechas de vigencia registradas en CT. Un certificado puede ser antiguo, renovado o revocado; un nombre DNS encontrado no demuestra que el host siga activo.

## 5. Parte B — Inspección de certificado con OpenSSL

Esta conexión consulta el certificado que presenta el servicio. Realizarla solo dentro del alcance autorizado por el laboratorio.

Mostrar la negociación y cadena TLS:
```bash
openssl s_client -connect h4cker.org:443 -servername h4cker.org </dev/null
```

**Qué hace cada parte:**
- `openssl s_client`: inicia un cliente TLS de diagnóstico.
- `-connect h4cker.org:443`: conecta al puerto 443 del dominio.
- `-servername h4cker.org`: envía SNI (Server Name Indication), para que el servidor seleccione el certificado correspondiente al nombre solicitado.
- `</dev/null`: envía fin de entrada inmediatamente, evitando que el comando espere datos escritos por teclado.

Extraer campos principales del certificado:
```bash
openssl s_client -connect h4cker.org:443 -servername h4cker.org </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates -serial -fingerprint -sha256
```

**Qué hace cada parte:**
- El primer `openssl s_client ...` establece la conexión TLS y obtiene la cadena/certificado.
- `2>/dev/null` descarta los mensajes de diagnóstico enviados por el proceso a stderr.
- `|` pasa la salida al siguiente comando.
- `openssl x509` interpreta el certificado X.509 recibido.
- `-noout` evita imprimir el certificado codificado completo.
- `-subject` muestra el titular; `-issuer` muestra la autoridad emisora.
- `-dates` muestra inicio y fin de vigencia.
- `-serial` muestra el número de serie.
- `-fingerprint -sha256` calcula la huella SHA-256 del certificado.

Revisar SAN, clave pública y firma:
```bash
openssl s_client -connect h4cker.org:443 -servername h4cker.org </dev/null 2>/dev/null | openssl x509 -noout -text | grep -E 'Subject:|Issuer:|DNS:|Public Key Algorithm|Public-Key|Signature Algorithm|Not Before|Not After'
```

**Qué hace cada parte:**
- `openssl x509 -noout -text`: decodifica y muestra los campos del certificado en formato legible.
- `grep -E 'patrón'`: filtra las líneas que coinciden con cualquiera de los patrones indicados.
- `Subject` e `Issuer`: ayudan a identificar titular y emisor.
- `DNS:`: permite localizar nombres SAN.
- `Public Key Algorithm` y `Public-Key`: indican algoritmo y tamaño de clave.
- `Signature Algorithm`: muestra el algoritmo de firma del certificado.
- `Not Before` y `Not After`: indican el periodo de validez.

## 6. Parte C — Enumeración TLS con SSLScan

**Advertencia:** SSLScan realiza conexiones activas. No escanear infraestructura ajena sin autorización. Si el ejercicio no autoriza el dominio, utilizar un servidor de laboratorio propio.

Ejecutar SSLScan:
```bash
sslscan h4cker.org
```

**Qué hace la línea:** ejecuta SSLScan contra el servicio del dominio y puerto TLS predeterminado, mostrando protocolos aceptados, suites criptográficas, preferencias y datos del certificado que el servidor presenta.

Guardar el resultado:
```bash
sslscan h4cker.org | tee evidencias/EH-04/sslscan-h4cker.txt
```

**Qué hace cada parte:**
- `sslscan h4cker.org`: realiza el análisis TLS del host.
- `|`: envía la salida estándar al siguiente programa.
- `tee archivo.txt`: muestra el resultado en pantalla y, al mismo tiempo, lo guarda en el archivo indicado.

Revisar protocolos, suites, certificado, emisor, fechas y tamaño de clave. Interpretar los resultados según la configuración esperada y las recomendaciones actuales; la presencia de un protocolo o suite no basta por sí sola para declarar una vulnerabilidad.

## 7. Ejemplo de resultados — SIMULACIÓN DIDÁCTICA

**Advertencia: datos hipotéticos; no son resultados comprobados.**

| Elemento | Ejemplo simulado | Interpretación |
|---|---|---|
| Nombres CT | `www.h4cker.org`, `blog.h4cker.org`, `dev.h4cker.org`, `api.h4cker.org`, `mail.h4cker.org` | Nombres que hipotéticamente aparecerían en certificados históricos |
| Autoridad emisora | Let's Encrypt | Ejemplo de CA pública |
| Clave pública | RSA, 2048 bits | Tamaño ilustrativo |
| Firma | SHA-256 con RSA | Ejemplo de algoritmo de firma |
| Protocolos TLS | TLS 1.2 y TLS 1.3 | Protocolos hipotéticos aceptados |
| Vigencia | Pendiente | Debe obtenerse de los datos reales `not_before` y `not_after` |
| Suites criptográficas | Pendiente | Deben copiarse de la salida real de SSLScan |

No utilizar esta tabla como evidencia del dominio. Sustituirla por la salida real después de ejecutar las consultas.

## 8. Análisis de seguridad y controles

| Posible hallazgo | Riesgo | Control |
|---|---|---|
| Subdominios antiguos en CT | Revelación de nombres de entornos olvidados | Inventariar DNS y retirar servicios abandonados |
| Certificado vencido | Errores de confianza y disponibilidad | Automatizar renovación y alertas |
| SAN innecesariamente amplio | Divulgación de nombres | Limitar SAN a los nombres requeridos |
| Protocolos o suites heredados | Protección criptográfica insuficiente | Deshabilitar configuraciones obsoletas |
| Certificados inconsistentes entre nodos | Configuración desigual | Centralizar y auditar despliegues |

## 9. Registrar fecha y versiones

```bash
date -Is
curl --version | head -n 1
jq --version
openssl version
sslscan --version
```

**Qué hace cada línea:**
- `date -Is`: muestra fecha y hora en formato ISO 8601, con zona horaria.
- `curl --version`: muestra versión y características de curl.
- `| head -n 1`: conserva únicamente la primera línea de la salida.
- `jq --version`: muestra la versión de jq.
- `openssl version`: muestra la versión de OpenSSL.
- `sslscan --version`: muestra la versión de SSLScan.

## 10. Evidencias que deben adjuntarse
- [ ] JSON descargado de crt.sh y fecha de consulta.
- [ ] Lista de nombres DNS extraídos.
- [ ] Salida de OpenSSL, si la conexión está autorizada.
- [ ] Salida de SSLScan, si el escaneo está autorizado.
- [ ] Captura de terminal o crt.sh, si el laboratorio la solicita.
- [ ] Sistema operativo y versiones de las herramientas.
- [ ] Identificación expresa de cualquier dato simulado.

## 11. Conclusiones
crt.sh permite consultar certificados registrados públicamente en Certificate Transparency y descubrir nombres DNS históricos. OpenSSL permite inspeccionar el certificado que presenta un servidor, mientras SSLScan enumera la configuración TLS mediante conexiones activas. Es necesario diferenciar certificados históricos de servicios activos y mantener separadas las simulaciones de las evidencias reales. La práctica quedará verificada cuando se adjunten las salidas reales, fechadas y obtenidas dentro del alcance autorizado.

## 12. Referencias
- crt.sh: https://crt.sh/
- Certificate Transparency: https://certificate.transparency.dev/
- SSLScan: https://github.com/rbsec/sslscan
- OpenSSL: https://docs.openssl.org/
- OWASP TLS Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html
