# Portfolio de Laboratorios — Cisco Ethical Hacker

Repositorio de aprendizaje práctico en ciberseguridad. Documenta progresivamente los laboratorios realizados, los comandos ejecutados, las evidencias obtenidas, el análisis técnico y las medidas de mitigación.

> **Regla de integridad:** una práctica se marca como completada solo cuando se ha ejecutado y sus evidencias han sido revisadas. Los resultados de ejemplo o pendientes de comprobar se identifican expresamente.

## Objetivos
- Construir un registro verificable del aprendizaje práctico de Ethical Hacking.
- Documentar reconocimiento, enumeración, análisis y validación de seguridad.
- Relacionar cada técnica con su propósito, resultados, riesgos y controles defensivos.
- Mantener evidencias reproducibles sin publicar credenciales, datos personales ni información sensible.

## Laboratorios incluidos

| ID | Práctica | Herramientas | Estado |
|---|---|---|---|
| EH-01 | Reconocimiento y enumeración DNS con DNSRecon | Kali Linux, DNSRecon, dig, nslookup | Documentada; salida de laboratorio analizada |
| EH-02 | Búsquedas de DNS | nslookup, dig, whois, host | Guía documentada; validar datos dinámicos |
| EH-03 | Recopilación de PII en redes sociales | OSINT, redes sociales | Resuelta mediante simulación con códigos griegos; sin búsquedas reales |

## Estructura
```text
.
├── README.md
├── index.html
├── 00-guia/
│   └── plantilla-practica.md
├── 01-reconocimiento-dns/
│   ├── 01-dnsrecon/
│   │   └── README.md
│   └── 02-busquedas-dns/
│       └── README.md
├── 02-osint-privacidad/
│   └── 03-pii-redes-sociales/
│       └── README.md
└── evidencias/
    └── README.md
```

## Cómo agregar una práctica
1. Asignar un identificador correlativo: `EH-03`, `EH-04`, etc.
2. Crear una carpeta dentro de la categoría correspondiente.
3. Copiar la plantilla de `00-guia/plantilla-practica.md`.
4. Registrar objetivo, entorno, comandos exactos y salida real.
5. Analizar hallazgos, impacto y mitigaciones.
6. Guardar capturas o archivos sanitizados en `evidencias/`.
7. Actualizar la tabla de laboratorios, el árbol de directorios y el panel `index.html`.\n\n## Convención de identificadores\n- Utilizar códigos basados en letras griegas para identidades ficticias, perfiles, organizaciones y sistemas (por ejemplo, `PERSONA-ALFA-01` u `ORG-BETA-02`).\n- Utilizar exclusivamente dígitos latinos `0–9` para los componentes numéricos.\n- No usar nombres de personas, empresas o instituciones reales como datos de simulación.\n- Identificar expresamente todo escenario hipotético; no presentar datos inventados como hallazgos observados.

## Criterios de evidencia
- Incluir fecha, sistema operativo, herramienta y versión cuando estén disponibles.
- Diferenciar **resultado observado**, **interpretación** y **dato pendiente de validar**.
- No inventar direcciones IP, registros DNS, resultados WHOIS ni vulnerabilidades.
- Ocultar tokens, contraseñas, direcciones privadas y datos personales.
- Ejecutar pruebas únicamente en sistemas propios o con autorización expresa.

## Alcance y ética
Este repositorio tiene fines educativos y defensivos. Las consultas de reconocimiento deben respetar las políticas del objetivo y las instrucciones del laboratorio Cisco NetAcad. Las pruebas activas se realizan únicamente en entornos autorizados.

## Portfolio web
Abrir [index.html](./index.html) para consultar el índice visual de prácticas. Puede publicarse con GitHub Pages desde la rama `main`, carpeta raíz, si se habilita Pages en la configuración del repositorio.
