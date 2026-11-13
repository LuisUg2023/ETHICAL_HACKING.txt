# Evidencias — EH-11 Scapy

## Evidencia recibida y registrada

1. **Salida de `ifconfig` compartida en la conversación:** muestra la interfaz `br-internal` en estado UP, con IPv4 `10.6.6.1`, máscara `255.255.255.0` y broadcast `10.6.6.255`. También se registraron los contadores RX/TX visibles en esa salida.
2. **Captura del error de Scapy:** muestra el traceback de `sniff()` que termina en `PermissionError: [Errno 1] Operation not permitted` durante la creación de un socket `AF_PACKET`.

## Evidencia de captura de tráfico

Todavía no se ha recibido una captura exitosa de paquetes ni un archivo PCAP. Por esa razón, esta carpeta no incluye un PCAP ni una captura de tráfico simulada.

Cuando se obtenga evidencia real, agregarla con nombres descriptivos, por ejemplo:

- `01-ifconfig-interfaces.png`
- `02-scapy-sniff-permission-error.png`
- `03-scapy-icmp-capture.png`
- `capture1.pcap`

Antes de publicar, revisar que las capturas no expongan credenciales, datos personales ni información de redes ajenas al laboratorio.
