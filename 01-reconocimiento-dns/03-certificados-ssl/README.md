# EH-04 — Identificación y análisis de certificados SSL/TLS con crt.sh y SSLScan

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** Reconocimiento / OSINT técnico / Seguridad de comunicaciones  
**Objetivo de práctica:** `h4cker.org`  
**Estado:** Documentación preparada con ejemplo simulado. La consulta actual a crt.sh y las pruebas SSLScan no se han ejecutado ni verificado en esta sesión.

> **Integridad de resultados:** los nombres de subdominios y características de certificado incluidos en la sección de ejemplo son ilustrativos. No deben presentarse como hallazgos reales de `h4cker.org`. Para completar la práctica verificada, ejecuta los comandos, conserva las salidas y sustituye el ejemplo por los datos observados.

## 1. Objetivos

- Consultar los registros públicos de Certificate Transparency (CT) mediante crt.sh.
- Identificar nombres DNS incluidos en certificados emitidos para un dominio.
- Comprender los campos Subject, Issuer, Subject Alternative Name (SAN), vigencia y algoritmo de clave.
- Revisar la configuración SSL/TLS de un servicio autorizado con SSLScan.
- Diferenciar la información histórica de CT de los servicios que actualmente están activos.

## 2. Fundamento técnico

**crt.sh** es un buscador de certificados que consulta registros públicos de Certificate Transparency. Los registros CT permiten observar certificados que fueron emitidos y registrados; no prueban que el subdominio siga activo, que el certificado continúe instalado ni que el servicio sea vulnerable.

**SSLScan** establece conexiones con el servicio TLS indicado y enumera protocolos, suites criptográficas y detalles del certificado que presenta el servidor. Es una prueba activa: debe ejecutarse únicamente contra sistemas propios o con autorización expresa.

## 3. Entorno y herramientas

- Sistema operativo: Kali Linux o distribución Linux compatible.
- Herramientas: `curl`, `jq`, `openssl`, `sslscan`.
- Dominio de consulta pasiva: `h4cker.org`.
- Servicio para análisis activo: solo si se cuenta con autorización.

Comprobar herramientas:
```bash
command -v curl
command -v jq
command -v openssl
command -v sslscan
```

En Kali, si falta alguna herramienta:
```bash
sudo apt update
sudo apt install curl jq openssl sslscan -y
```

## 4. Parte A — Consulta de Certificate Transparency con crt.sh

Abrir la consulta en el navegador:

https://crt.sh/?q=%25.h4cker.org

Descargar el resultado JSON:
```bash
mkdir -p evidencias/EH-04
curl -fLsS 'https://crt.sh/?q=%25.h4cker.org&output=json' \
  -o evidencias/EH-04/crtsh-h4cker.json
```

Extraer y ordenar los nombres DNS:
```bash
jq -r '.[].name_value' evidencias/EH-04/crtsh-h4cker.json \
  | sort -u > evidencias/EH-04/subdominios-ct.txt

cat evidencias/EH-04/subdominios-ct.txt
```

Para revisar campos de certificados en el JSON:
```bash
jq -r '.[] | [.id, .issuer_name, .common_name, .name_value, .not_before, .not_after] | @tsv' \
  evidencias/EH-04/crtsh-h4cker.json
```

**Interpretación:** los valores `not_before` y `not_after` corresponden a las fechas de inicio y fin de vigencia registradas para cada certificado. Puede haber certificados duplicados, renovaciones y varios nombres SAN. Un nombre hallado en CT no debe considerarse un host activo sin una comprobación adicional autorizada.

## 5. Parte B — Análisis del certificado presentado por un servidor

Solo con autorización para conectarse al servicio:
```bash
openssl s_client -connect h4cker.org:443 -servername h4cker.org </dev/null
```

Extraer el certificado presentado:
```bash
openssl s_client -connect h4cker.org:443 -servername h4cker.org </dev/null 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates -serial -fingerprint -sha256
```

Consultar Subject Alternative Names y algoritmo de clave:
```bash
openssl s_client -connect h4cker.org:443 -servername h4cker.org </dev/null 2>/dev/null \
  | openssl x509 -noout -text \
  | grep -E 'Subject:|Issuer:|DNS:|Public Key Algorithm|Public-Key|Signature Algorithm|Not Before|Not After'
```

Revisar los campos:
- **Subject:** identidad del titular indicada en el certificado.
- **Issuer:** autoridad certificadora que lo emitió.
- **SAN:** nombres DNS cubiertos por el certificado.
- **Not Before / Not After:** periodo de validez.
- **Public Key Algorithm / Public-Key:** algoritmo y tamaño de clave pública.
- **Signature Algorithm:** algoritmo de firma del certificado.
- **SHA-256 Fingerprint:** huella para identificar y comparar el certificado.

## 6. Parte C — Enumeración TLS con SSLScan

**Advertencia:** SSLScan realiza conexiones activas al host. No ejecutar contra infraestructura ajena sin autorización. Si el laboratorio no autoriza el escaneo de `h4cker.org`, utiliza una máquina local o un servidor de prácticas autorizado.

Comando:
```bash
sslscan h4cker.org
```

Guardar la salida:
```bash
sslscan h4cker.org | tee evidencias/EH-04/sslscan-h4cker.txt
```

Revisar, según lo que informe la versión instalada:
- Protocolos SSL/TLS aceptados o rechazados.
- Suites criptográficas habilitadas.
- Preferencia del servidor.
- Certificado, emisor y fechas.
- Algoritmo y tamaño de clave.
- Posibles configuraciones heredadas o débiles.

No concluir que existe una vulnerabilidad únicamente porque una herramienta muestre un protocolo o una suite: hay que interpretar el resultado, el contexto y los requisitos de seguridad aplicables.

## 7. Ejemplo de resultados — SIMULACIÓN DIDÁCTICA

**Advertencia: datos hipotéticos; no son resultados comprobados.**

La siguiente tabla muestra cómo presentar los resultados si una consulta devolviera registros semejantes. No confirma que estos nombres o parámetros pertenezcan actualmente al dominio:

| Elemento | Ejemplo simulado | Interpretación |
|---|---|---|
| Nombres CT | `www.h4cker.org`, `blog.h4cker.org`, `dev.h4cker.org`, `api.h4cker.org`, `mail.h4cker.org` | Nombres que hipotéticamente aparecerían en SAN de certificados históricos |
| Autoridad emisora | Let's Encrypt | Ejemplo de CA pública |
| Clave pública | RSA, 2048 bits | Tamaño de clave ilustrativo |
| Firma | SHA-256 con RSA | Ejemplo de algoritmo de firma |
| Protocolos TLS | TLS 1.2 y TLS 1.3 | Ejemplo de protocolos que podría aceptar un servidor |
| Vigencia | No especificada | Debe extraerse de los campos `not_before` y `not_after` reales |
| Suites criptográficas | No especificadas | Deben copiarse de la salida real de SSLScan |

**Conclusión de la simulación:** los certificados CT podrían revelar nombres DNS adicionales y aportar información histórica sobre la infraestructura. La prueba SSLScan permitiría conocer la configuración TLS que presenta un servidor en el momento de la conexión. Ninguna de estas observaciones debe convertirse en un hallazgo real hasta ejecutar y conservar las consultas.

## 8. Análisis de seguridad y controles

| Hallazgo que podría observarse | Riesgo potencial | Recomendación defensiva |
|---|---|---|
| Subdominios antiguos en CT | Exposición de nombres de entornos olvidados | Mantener inventario DNS y retirar servicios abandonados |
| Certificados vencidos o próximos a vencer | Errores de confianza y disponibilidad | Automatizar renovación y alertas de expiración |
| SAN demasiado amplio | Divulgación de nombres internos o innecesarios | Limitar SAN a nombres requeridos |
| Protocolos o suites obsoletos | Menor nivel de protección criptográfica | Deshabilitar protocolos heredados y suites débiles |
| Certificados distintos entre nodos | Configuración inconsistente | Centralizar y auditar despliegues de certificados |

## 9. Evidencias que deben adjuntarse

- [ ] JSON descargado de crt.sh con fecha de consulta.
- [ ] Lista deduplicada de nombres DNS extraídos.
- [ ] Salida de OpenSSL, solo si la conexión está autorizada.
- [ ] Salida completa de SSLScan, solo si el escaneo está autorizado.
- [ ] Captura de crt.sh o terminal, si el laboratorio la solicita.
- [ ] Sistema operativo y versiones de las herramientas.
- [ ] Identificación expresa de cualquier dato de ejemplo o simulado.

Registrar versiones:
```bash
date -Is
curl --version | head -n 1
jq --version
openssl version
sslscan --version
```

## 10. Conclusiones

crt.sh facilita la consulta pasiva de certificados registrados en Certificate Transparency y puede ayudar a descubrir nombres DNS históricos. OpenSSL permite inspeccionar los campos de un certificado presentado por un servicio, mientras que SSLScan enumera la configuración TLS mediante conexiones activas. Es fundamental separar los certificados históricos de los servicios activos y no presentar resultados simulados como evidencia. La práctica quedará verificada cuando se adjunten las salidas reales, fechadas y obtenidas dentro del alcance autorizado.

## 11. Referencias

- crt.sh: https://crt.sh/
- Certificate Transparency — Google: https://certificate.transparency.dev/
- SSLScan: https://github.com/rbsec/sslscan
- OpenSSL s_client: https://docs.openssl.org/
- OWASP Transport Layer Security Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html
