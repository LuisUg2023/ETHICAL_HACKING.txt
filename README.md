# Portfolio de Laboratorios — Cisco Ethical Hacker

Repositorio de aprendizaje práctico en ciberseguridad. Documenta progresivamente los laboratorios realizados, los comandos ejecutados, las evidencias obtenidas, el análisis técnico y las medidas de mitigación.

> **Regla de integridad:** una práctica se marca como verificada solo cuando se ha ejecutado y sus evidencias han sido revisadas. Los resultados de ejemplo o pendientes de comprobar se identifican expresamente.

## Objetivos
- Construir un registro verificable del aprendizaje práctico de Ethical Hacking.
- Documentar reconocimiento, enumeración, análisis y validación de seguridad.
- Relacionar cada técnica con su propósito, resultados, riesgos y controles defensivos.
- Mantener evidencias reproducibles sin publicar credenciales, datos personales ni información sensible.

## Laboratorios incluidos

| ID | Práctica | Herramientas | Estado |
|---|---|---|---|
| EH-01 | Reconocimiento y enumeración DNS con DNSRecon | Kali Linux, DNSRecon, dig, nslookup | Documentada; revalidación pendiente |
| EH-02 | Búsquedas de DNS | nslookup, dig, whois, host | Guía documentada; validar datos dinámicos |
| EH-03 | Recopilación de PII en redes sociales | OSINT, redes sociales | Simulación con identidades ficticias |
| EH-04 | Análisis de metadatos EXIF | ExifTool, Kali Linux | Guía documentada; ejecución pendiente |
| EH-05 | Google Dorks para reconocimiento OSINT | Google Search | Guía documentada; consultas autorizadas pendientes |
| EH-06 | Conociendo más sobre la organización | EmailHarvester, SpiderFoot, ExifTool, servicios de comprobación de exposición | Guía documentada; ejecución pendiente |
| EH-07 | Identificación y análisis de certificados SSL/TLS | crt.sh, curl, jq, OpenSSL, SSLScan | Guía documentada; ejecución y evidencias pendientes |
| EH-08 | Recon-ng: reconocimiento OSINT modular | Recon-ng, marketplace, módulos OSINT | Guía documentada; ejecución y evidencias pendientes |
| EH-09 | Shodan: búsqueda de dispositivos y servicios expuestos | Shodan Web, Shodan CLI, API | Guía documentada; consultas y evidencias pendientes |

## Estructura
~~~text
.
├── README.md
├── index.html
├── 00-guia/
│   └── plantilla-practica.md
├── 01-reconocimiento-dns/
│   ├── 01-dnsrecon/
│   └── 02-busquedas-dns/
├── 02-osint-privacidad/
│   ├── README.md
│   ├── 01-pii-redes-sociales/
│   ├── 02-analisis-metadatos-exif/
│   ├── 03-google-dorks/
│   ├── 04-conociendo-la-organizacion/
│   ├── 05-recon-ng/
│   └── 06-shodan/
├── 03-seguridad-comunicaciones/
│   ├── README.md
│   └── 01-certificados-ssl/
│       └── README.md
└── evidencias/
~~~

## Cómo agregar una práctica
1. Asignar un identificador correlativo: EH-10, EH-11, etc.
2. Crear una carpeta dentro de la categoría correspondiente.
3. Copiar la plantilla de 00-guia/plantilla-practica.md.
4. Registrar objetivo, entorno, comandos exactos y salida real.
5. Analizar hallazgos, impacto y mitigaciones.
6. Guardar capturas o archivos sanitizados en evidencias/.
7. Actualizar la tabla de laboratorios, el árbol de directorios y el panel index.html.

## Convención de identificadores
- Utilizar códigos basados en letras griegas para identidades ficticias, perfiles, organizaciones y sistemas (por ejemplo, PERSONA-ALFA-01 u ORG-BETA-02).
- Utilizar exclusivamente dígitos latinos 0–9 para los componentes numéricos.
- No usar nombres de personas, empresas o instituciones reales como datos de simulación.
- Identificar expresamente todo escenario hipotético; no presentar datos inventados como hallazgos observados.

## Criterios de evidencia
- Incluir fecha, sistema operativo, herramienta y versión cuando estén disponibles.
- Diferenciar **resultado observado**, **interpretación** y **dato pendiente de validar**.
- No inventar direcciones IP, registros DNS, resultados WHOIS, certificados ni vulnerabilidades.
- Ocultar tokens, contraseñas, direcciones de correo, coordenadas GPS y datos personales.
- Ejecutar pruebas activas únicamente en sistemas propios o con autorización expresa.

## Alcance y ética
Este repositorio tiene fines educativos y defensivos. Las consultas OSINT deben limitarse a información pública y a un alcance autorizado. No se deben divulgar documentos sensibles, coordenadas GPS, credenciales filtradas ni PII de terceros. Las pruebas activas y consultas automatizadas se ejecutan solo dentro de un alcance autorizado.

## Portfolio web
Abrir [index.html](./index.html) para consultar el índice visual de prácticas. Puede publicarse con GitHub Pages desde la rama main, carpeta raíz, si se habilita Pages en la configuración del repositorio.
