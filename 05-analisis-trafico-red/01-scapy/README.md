# EH-11 — Elaboración y captura de paquetes con Scapy

## 1. Objetivo

Explorar la biblioteca Scapy en Kali Linux, identificar interfaces de red, capturar tráfico autorizado, inspeccionar paquetes y preparar archivos PCAP para su análisis con Wireshark. La práctica también registra el problema de permisos observado al invocar `sniff()`.

## 2. Alcance y seguridad

- Entorno: máquina Kali Linux utilizada para el laboratorio Cisco NetAcad.
- Interfaz indicada por el laboratorio: `br-internal`.
- Dirección de la interfaz observada: `10.6.6.1/24`.
- Host de destino indicado por la guía Cisco: `10.6.6.23`. Esta dirección procede de las instrucciones del laboratorio; no se ha confirmado aquí que el host esté activo.
- Las capturas y pruebas activas deben limitarse a la red propia del laboratorio y a sistemas autorizados.
- No ejecutar spoofing, DoS, DDoS ni pruebas contra redes públicas.

## 3. Identificación de interfaces: resultado observado

Se recibió la salida de `ifconfig`. En ella aparecen las siguientes interfaces con dirección IPv4:

| Interfaz | IPv4 observada | Máscara | Estado mostrado |
|---|---:|---:|---|
| `br-339414195aeb` | `10.5.5.1` | `255.255.255.0` | UP, BROADCAST, RUNNING, MULTICAST |
| `br-355ee7945a88` | `192.168.0.1` | `255.255.255.0` | UP, BROADCAST, RUNNING, MULTICAST |
| `br-internal` | `10.6.6.1` | `255.255.255.0` | UP, BROADCAST, RUNNING, MULTICAST |
| `docker0` | `172.17.0.1` | `255.255.0.0` | UP, BROADCAST, RUNNING, MULTICAST |
| `eth0` | `10.0.2.15` | `255.255.255.0` | UP, BROADCAST, RUNNING, MULTICAST |
| `lo` | `127.0.0.1` | `255.0.0.0` | LOOPBACK, RUNNING |

También aparecen varias interfaces virtuales `veth*`, sin dirección IPv4 mostrada en la salida compartida.

### Contadores observados en el momento de ejecutar ifconfig

| Interfaz | RX paquetes | RX bytes | TX paquetes | TX bytes |
|---|---:|---:|---:|---:|
| `br-339414195aeb` | 45 | 2638 | 16 | 2406 |
| `br-355ee7945a88` | 62 | 7639 | 15 | 2360 |
| `br-internal` | 47 | 2728 | 18 | 2622 |
| `docker0` | 72 | 8343 | 31 | 4169 |
| `eth0` | 3118 | 3486171 | 1590 | 183223 |
| `lo` | 4 | 240 | 4 | 240 |

Los contadores son una fotografía del estado en ese momento; no identifican por sí solos qué protocolos o conversaciones generaron esos paquetes.

### Interpretación

La interfaz `br-internal` existe y se muestra activa, con dirección `10.6.6.1/24` y broadcast `10.6.6.255`. Por tanto, no corresponde asumir que el nombre de interfaz de la guía Cisco falta en este entorno.

La presencia de interfaces `br-*`, `docker0` y `veth*` es consistente con interfaces virtuales de puente y pares virtuales de red. La salida por sí sola no permite determinar la topología completa ni confirmar que el host `10.6.6.23` esté disponible.

## 4. Error observado al ejecutar sniff()

La captura de pantalla aportada muestra que al ejecutar:

```python
sniff()
```

Scapy termina con:

```text
PermissionError: [Errno 1] Operation not permitted
```

El traceback llega a la creación de un socket Linux de tipo `AF_PACKET` (`socket.AF_PACKET`, `socket.SOCK_RAW`). La operación es rechazada por el sistema.

### Análisis técnico

- `sniff()` necesita abrir un socket de captura de paquetes.
- En Linux, esa operación requiere privilegios o capacidades de red apropiadas, como `CAP_NET_RAW`.
- El error demuestra que el proceso que ejecutó Scapy no pudo abrir el socket.
- La captura de pantalla no demuestra que se haya capturado tráfico ni que la interfaz `br-internal` sea incorrecta.
- Como el entorno muestra interfaces virtuales de puente, también debe comprobarse si Kali se ejecuta dentro de un contenedor o entorno restringido. Ejecutar Scapy con `sudo` no garantiza que un contenedor tenga concedidas las capacidades necesarias.

## 5. Verificaciones para resolver el problema de captura

Ejecutar desde la terminal de Kali, fuera del intérprete de Scapy:

### 5.1 Confirmar interfaz y dirección

```bash
ip -br address
ip link show br-internal
```

### 5.2 Confirmar la ruta hacia el host indicado por la práctica

```bash
ip route
ip route get 10.6.6.23
```

El resultado de estos comandos debe conservarse literalmente. La salida de `ifconfig` ya confirma que `br-internal` tiene `10.6.6.1/24`, pero todavía no se ha compartido el resultado de las rutas.

### 5.3 Iniciar Scapy con privilegios

```bash
sudo scapy
```

Dentro de Scapy, probar una captura limitada por tiempo:

```python
a = sniff(iface="br-internal", count=10, timeout=30)
a.summary()
```

Si vuelve a aparecer `Operation not permitted`, no repetir indefinidamente el comando: comprobar el tipo de entorno y las capacidades disponibles. En un contenedor, la administración del host debe conceder la capacidad de captura correspondiente; no se debe debilitar la seguridad global del sistema como solución improvisada.

## 6. Generar tráfico ICMP dentro del laboratorio

Solo después de confirmar que la ruta y el destino pertenecen al laboratorio autorizado, abrir dos terminales.

Terminal 1 — iniciar la captura:

```python
a = sniff(iface="br-internal", filter="icmp", count=10, timeout=30)
a.summary()
```

Terminal 2 — generar tráfico ICMP hacia el host del laboratorio:

```bash
ping -c 5 10.6.6.23
```

La captura debe iniciarse antes del ping. Si el host responde, se pueden observar solicitudes y respuestas ICMP. Si no hay paquetes, eso no permite concluir por sí solo que Scapy esté fallando: hay que revisar la ruta, la disponibilidad del host, el filtrado y la interfaz de captura.

Para comprobar si circula otro tipo de tráfico, se puede realizar una captura temporal sin filtro:

```python
b = sniff(iface="br-internal", count=10, timeout=30)
b.summary()
```

## 7. Inspección y exportación de paquetes

Cuando exista una captura real:

```python
a.summary()
a.nsummary()
a[0].show()
wrpcap("capture1.pcap", a)
```

- `summary()`: muestra un resumen de cada paquete.
- `nsummary()`: muestra un resumen numerado.
- `show()`: permite inspeccionar los campos del paquete seleccionado.
- `wrpcap()`: guarda los paquetes capturados en un archivo PCAP para su análisis posterior con Wireshark.

No se debe crear ni presentar un PCAP vacío como evidencia de tráfico capturado.

## 8. Estado de la evidencia

### Evidencia recibida

- Salida de `ifconfig` compartida en la conversación: confirma que `br-internal` existe, está activa y tiene `10.6.6.1/24`.
- Captura de pantalla del traceback de Scapy: documenta el error `PermissionError: [Errno 1] Operation not permitted` al invocar `sniff()`.

### Resultados que todavía no están demostrados

- Captura efectiva de paquetes mediante `sniff()`.
- Tráfico ICMP entre `10.6.6.1` y `10.6.6.23`.
- Disponibilidad del host `10.6.6.23`.
- Exportación y análisis de un archivo `capture1.pcap`.
- Resultado de ejecutar Scapy con privilegios y capacidades suficientes.

No se asignan resultados a estas verificaciones hasta recibir sus salidas reales.

## 9. Preguntas de análisis

**¿Por qué sniff() puede fallar aunque la interfaz exista?**

Porque la existencia y el estado UP de una interfaz no conceden automáticamente al proceso permiso para abrir un socket de captura. El error observado corresponde a una operación de socket rechazada por el sistema.

**¿Por qué no basta con ejecutar sniff() para observar tráfico?**

Porque `sniff()` escucha pasivamente. Para observar paquetes debe seleccionarse la interfaz por la que circula el tráfico y debe existir tráfico durante el intervalo de captura.

**¿Qué significa que br-internal tenga contadores RX y TX distintos de cero?**

Que la interfaz había contabilizado recepción y transmisión de paquetes antes o en el momento de ejecutar `ifconfig`. Esos contadores no sustituyen una captura ni permiten identificar el contenido de los paquetes.

## 10. Referencias

- Cisco Networking Academy, práctica de laboratorio “Elaboración de paquetes con Scapy” proporcionada para esta actividad.
- Documentación de Scapy: https://scapy.readthedocs.io/
- Manual de Linux para sockets: https://man7.org/linux/man-pages/man7/packet.7.html
- Manual de Linux para capacidades: https://man7.org/linux/man-pages/man7/capabilities.7.html

## Volver al índice

- [Portfolio Cisco Ethical Hacker](../../README.md)
- [Índice visual](../../index.html)
