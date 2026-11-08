# 03 — Seguridad de las comunicaciones

Esta sección reúne las prácticas relacionadas con la protección de las comunicaciones, los protocolos criptográficos, los certificados digitales y la configuración segura de los servicios de red.

## Prácticas

| ID | Práctica | Herramientas | Estado |
|---|---|---|---|
| EH-07 | [Identificación y análisis de certificados SSL/TLS](./01-certificados-ssl/README.md) | crt.sh, curl, jq, OpenSSL, SSLScan | Guía documentada; ejecución y evidencias pendientes |

## Organización de esta sección

Cada práctica se almacena en su propia carpeta numerada. Esta separación permite incorporar nuevas actividades sin mezclar sus instrucciones, comandos, resultados y evidencias.

~~~text
03-seguridad-comunicaciones/
├── README.md
└── 01-certificados-ssl/
    └── README.md
~~~

## Criterios de documentación

- Registrar el objetivo, el alcance y las herramientas utilizadas.
- Diferenciar consultas pasivas de pruebas activas.
- Ejecutar escaneos únicamente contra sistemas propios o con autorización expresa.
- Adjuntar salidas reales, fechadas y sanitizadas.
- No presentar ejemplos hipotéticos como resultados observados.

## Volver al índice

- [Índice general del portfolio](../README.md)
- [EH-07 — Certificados SSL/TLS](./01-certificados-ssl/README.md)
