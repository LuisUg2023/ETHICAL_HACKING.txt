# 05 — Análisis de tráfico de red

Esta sección reúne laboratorios de inspección de paquetes, captura de tráfico y análisis de protocolos en redes de prueba autorizadas.

## Prácticas

| ID | Práctica | Herramientas | Estado |
|---|---|---|---|
| EH-11 | [Elaboración y captura de paquetes con Scapy](./01-scapy/README.md) | Scapy, Kali Linux, Wireshark | Interfaz `br-internal` identificada; captura bloqueada por error de permisos |

## Organización

```text
05-analisis-trafico-red/
└── 01-scapy/
    ├── README.md
    └── evidencias/
        └── README.md
```

## Criterios de análisis

- Seleccionar la interfaz que corresponde a la red autorizada.
- Distinguir la existencia de una interfaz de los permisos necesarios para capturar tráfico.
- Conservar salidas reales y capturas de pantalla sin modificar los resultados.
- No inferir disponibilidad de un host solo por la dirección que indica la guía.
- Exportar PCAP únicamente cuando se haya realizado una captura efectiva.

## Volver al índice

- [Portfolio Cisco Ethical Hacker](../README.md)
- [EH-11 — Scapy](./01-scapy/README.md)
