# EH-10 — Enumeración con Nmap

## 1. Datos generales
- **Curso:** Cisco NetAcad — Ethical Hacker
- **Práctica:** Enumeración con Nmap
- **Categoría:** Reconocimiento activo y enumeración de redes
- **Sistema:** Kali Linux
- **Herramienta:** Nmap 7.95 (versión observada en la salida compartida)
- **Red del laboratorio:** `10.6.6.0/24`
- **Host indicado por Cisco:** `10.6.6.23`

**Criterio de documentación:** los resultados que el usuario ejecutó y compartió se identifican como observados. Los ejemplos didácticos adicionales se identifican expresamente como simulados; no son evidencia de ejecución.

## 2. Objetivos
1. Utilizar Nmap para descubrir hosts y enumerar servicios.
2. Identificar puertos TCP, servicios y versiones anunciadas.
3. Comprender la enumeración de recursos SMB mediante NSE.
4. Interpretar los resultados desde una perspectiva defensiva.
5. Registrar resultados y distinguir evidencia observada de escenarios de práctica.

## 3. Escenario y alcance
El laboratorio Cisco plantea el análisis de una red virtual y del host `10.6.6.23`, dentro del segmento `10.6.6.0/24`. Todas las pruebas descritas se limitan a la topología autorizada del curso.

| Elemento | Valor |
|---|---|
| Segmento | `10.6.6.0/24` |
| Objetivo individual | `10.6.6.23` |
| Analizador | Kali Linux |
| Herramienta | Nmap |
| Tipo de actividad | Descubrimiento, escaneo de puertos y enumeración de servicios |

## 4. Fundamento técnico
**Nmap (Network Mapper)** permite descubrir equipos, examinar puertos y reconocer servicios. Los resultados dependen de la conectividad, las rutas, los filtros, los permisos y las respuestas del objetivo.

- **Host discovery:** determina si un host responde a las sondas de descubrimiento.
- **Port scanning:** clasifica puertos como abiertos, cerrados o filtrados.
- **Service/version detection:** intenta identificar aplicaciones y versiones.
- **OS detection:** estima el sistema operativo a partir de respuestas de red.
- **NSE:** motor de scripts de Nmap para automatizar tareas de reconocimiento y comprobación.

La ausencia de respuesta a una sonda no demuestra por sí sola que el equipo esté apagado: puede haber filtrado, aislamiento de red o una ruta incorrecta.

## 5. Preparación del entorno

### 5.1 Comprobar Nmap
```bash
nmap --version
```
Muestra la versión instalada. En la ejecución compartida para esta práctica, Nmap informó la versión **7.95**.

### 5.2 Revisar interfaz y rutas
```bash
ip -br address
ip route
ip route get 10.6.6.23
```
- `ip -br address`: resume interfaces y direcciones IP.
- `ip route`: muestra la tabla de enrutamiento.
- `ip route get`: indica la ruta e interfaz que el sistema utilizaría para llegar al objetivo.

Estas comprobaciones permiten detectar si Kali está conectada al segmento correcto antes de interpretar un resultado de escaneo.

## 6. Procedimiento de enumeración

### 6.1 Comprobar conectividad ICMP
```bash
ping -c 4 10.6.6.23
```
Envía cuatro solicitudes ICMP Echo al objetivo. Una respuesta confirma que el host responde a ICMP; la falta de respuesta puede deberse a filtrado y no confirma que esté apagado.

### 6.2 Descubrir un único host
```bash
nmap -sn 10.6.6.23
```
La opción `-sn` realiza descubrimiento de host y omite el escaneo de puertos.

### 6.3 Escanear la red del laboratorio
```bash
nmap -sn 10.6.6.0/24
```
Examina las 256 direcciones del bloque IPv4 `/24` mediante técnicas de descubrimiento de hosts. No es un escaneo de puertos.

**Resultado real compartido por el usuario:**
```text
Starting Nmap 7.95 ( https://nmap.org ) at 2026-11-08 12:58 EST
Stats: 0:00:54 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 26.37% done; ETC: 13:02 (0:02:31 remaining)
Stats: 0:00:59 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 28.81% done; ETC: 13:02 (0:02:28 remaining)
Stats: 0:01:01 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 29.98% done; ETC: 13:02 (0:02:25 remaining)
Stats: 0:01:03 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 30.76% done; ETC: 13:02 (0:02:24 remaining)
Stats: 0:01:05 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 31.74% done; ETC: 13:02 (0:02:22 remaining)
Stats: 0:01:08 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 33.20% done; ETC: 13:02 (0:02:19 remaining)
Stats: 0:01:12 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 35.16% done; ETC: 13:02 (0:02:15 remaining)
Stats: 0:01:14 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 36.13% done; ETC: 13:02 (0:02:13 remaining)
Stats: 0:01:17 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 37.60% done; ETC: 13:02 (0:02:09 remaining)
Stats: 0:03:06 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 89.40% done; ETC: 13:02 (0:00:22 remaining)
Stats: 0:03:29 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 100.00% done; ETC: 13:02 (0:00:00 remaining)
Nmap done: 256 IP addresses (0 hosts up) scanned in 210.05 seconds
```

**Interpretación del resultado observado:**
- Nmap examinó 256 direcciones del rango.
- No identificó hosts activos mediante las sondas utilizadas: `0 hosts up`.
- La duración informada fue de **210,05 segundos**, aproximadamente **3 minutos y 30 segundos**.
- Este resultado no coincide con el ejemplo del curso que muestra seis hosts activos. No se deben reemplazar los cero hosts observados por los seis del ejemplo.
- El resultado no permite concluir por sí solo que todos los equipos estén apagados. Es necesario revisar la dirección IP de Kali, la ruta, el modo de red de la máquina virtual y posibles filtros.

### 6.4 Escaneo TCP básico del objetivo
```bash
nmap 10.6.6.23
```
Realiza el escaneo TCP predeterminado de los puertos más comunes de Nmap.

### 6.5 Detección de servicios y versiones
```bash
nmap -sV 10.6.6.23
```
La opción `-sV` envía sondas para intentar identificar los servicios y sus versiones en los puertos detectados.

### 6.6 Detección del sistema operativo
```bash
sudo nmap -O 10.6.6.23
```
La opción `-O` habilita la detección de sistema operativo. Requiere condiciones de red adecuadas y puede no producir una identificación concluyente.

### 6.7 Guardar la salida
```bash
mkdir -p evidencias/EH-10
nmap -sV -oN evidencias/EH-10/nmap-servicios.txt 10.6.6.23
```
- `mkdir -p`: crea el directorio de salida.
- `-sV`: solicita detección de versiones.
- `-oN`: guarda el resultado en formato normal legible.

## 7. Enumeración de usuarios SMB con NSE

El laboratorio puede incluir la comprobación de servicios SMB en los puertos TCP 139 y 445. El script `smb-enum-users.nse` intenta obtener información de cuentas de usuario expuestas por el servicio SMB.

### 7.1 Comprobar que el script existe
```bash
ls /usr/share/nmap/scripts/smb-enum-users.nse
```
Si el archivo no se encuentra, se puede actualizar la base local de scripts con:
```bash
sudo nmap --script-updatedb
```

### 7.2 Ejecutar la enumeración en el host autorizado
```bash
nmap --script smb-enum-users.nse -p139,445 10.6.6.23
```

| Elemento | Función |
|---|---|
| `nmap` | Ejecuta el escáner. |
| `--script` | Selecciona un script NSE. |
| `smb-enum-users.nse` | Intenta enumerar cuentas de usuario SMB. |
| `-p139,445` | Limita la comprobación a los puertos TCP 139 y 445. |
| `10.6.6.23` | Host objetivo del laboratorio Cisco. |

El resultado depende de que el host sea alcanzable, que SMB esté disponible y que la configuración del servidor permita obtener esa información. Un resultado sin usuarios enumerados no demuestra que no existan cuentas.

### 7.3 Ejemplo didáctico de salida SMB (simulado)
Los nombres son ficticios y no corresponden al resultado real del host Cisco.

```text
PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Host script results:
| smb-enum-users:
|   Users
|     LAB_USER_A
|     LAB_USER_B
|     LAB_SUPPORT
```

**Interpretación del ejemplo:** si el script revela cuentas, el analista debe determinar si esa enumeración es necesaria para la operación. La exposición innecesaria de nombres de usuario facilita la preparación de ataques de fuerza bruta o ingeniería social.

## 8. Opciones de Nmap de referencia

| Opción | Función |
|---|---|
| `-A` | Activa detección de SO, versiones, scripts por defecto y traceroute. Genera más tráfico. |
| `-O` | Estima el sistema operativo. |
| `-p` | Especifica puertos o rangos, por ejemplo `-p 22,80,443`. |
| `-sF` | Realiza un escaneo TCP FIN. Su interpretación depende del sistema y de los filtros. |
| `-sn` | Descubrimiento de hosts sin escaneo de puertos. |
| `-sS` | Escaneo TCP SYN; normalmente requiere privilegios elevados. |
| `-sT` | Escaneo TCP connect(), completa la conexión TCP. |
| `-sV` | Detecta servicios y versiones. |
| `-T0` a `-T5` | Ajusta la temporización; valores altos aumentan la velocidad y el volumen de tráfico. |
| `--script` | Ejecuta uno o varios scripts NSE seleccionados. |

## 9. Ejemplo integrado de resultados para practicar el análisis (simulado)

Esta tabla es un escenario didáctico independiente. No describe el resultado real del escaneo de red compartido.

| Puerto/protocolo | Estado simulado | Servicio de ejemplo | Interpretación defensiva |
|---|---|---|---|
| 22/tcp | open | SSH | Restringir acceso administrativo y usar autenticación robusta. |
| 80/tcp | open | HTTP | Revisar si debe redirigir a HTTPS y evitar transmitir credenciales sin cifrado. |
| 139/tcp | open | NetBIOS-SSN | Limitar SMB/NetBIOS a redes internas autorizadas. |
| 445/tcp | open | Microsoft-DS/SMB | Revisar versión SMB, permisos compartidos y reglas de firewall. |
| 443/tcp | open | HTTPS | Revisar configuración TLS, certificados y protocolos permitidos. |

No se debe inferir que estos puertos están abiertos en `10.6.6.23`; son valores ilustrativos para practicar la lectura de una tabla de Nmap.

## 10. Análisis de seguridad
1. Contrastar cada puerto con el inventario y la función del equipo.
2. Determinar si el servicio es necesario y si está expuesto a los segmentos adecuados.
3. Verificar versiones y soporte del fabricante antes de afirmar que existe una vulnerabilidad.
4. Revisar las políticas de firewall, segmentación y autenticación.
5. Considerar falsos positivos, filtrado, banners modificados y limitaciones de detección.
6. En SMB, revisar el acceso a recursos compartidos, la exposición de cuentas y la configuración de SMBv1.

### Análisis del resultado real de descubrimiento
El escaneo de red registrado detectó cero hosts activos en 210,05 segundos. Por tanto, esa ejecución no permite elaborar un inventario de hosts activos ni atribuir servicios a los equipos del segmento. La diferencia con los seis hosts del ejemplo Cisco debe investigarse revisando primero la configuración de red de Kali y la conectividad hacia el segmento del laboratorio.

## 11. Mitigaciones defensivas
- Deshabilitar servicios y protocolos que no sean necesarios.
- Restringir TCP 139 y 445 mediante firewall a los equipos que requieran SMB.
- Deshabilitar SMBv1 cuando no exista una dependencia justificada.
- Aplicar permisos mínimos a carpetas compartidas y cuentas de servicio.
- Evitar la enumeración anónima de usuarios y recursos SMB.
- Mantener sistemas operativos y servicios actualizados.
- Segmentar redes administrativas, servidores y estaciones de trabajo.
- Registrar y revisar conexiones SMB y eventos de autenticación.

## 12. Conclusiones
La ejecución real compartida de `nmap -sn 10.6.6.0/24` examinó 256 direcciones y no detectó hosts activos; Nmap informó una duración de 210,05 segundos. El resultado se conserva tal como fue observado y no se reemplaza por los seis hosts del ejemplo Cisco.

La práctica documenta además los comandos para escaneo TCP, identificación de versiones, detección de sistema operativo y enumeración SMB. Los resultados ilustrativos de puertos y usuarios están marcados como simulados y sirven únicamente para aprender a interpretar las salidas. La enumeración SMB debe realizarse exclusivamente contra el equipo autorizado del laboratorio.

## 13. Referencias
- Cisco NetAcad — *Práctica de laboratorio: Enumeración con Nmap* (material del curso).
- Nmap Reference Guide: https://nmap.org/book/man.html
- Nmap NSE Documentation: https://nmap.org/book/nse.html
- Nmap Script smb-enum-users: https://nmap.org/nsedoc/scripts/smb-enum-users.html

## 14. Resumen de validación documental
- [x] Objetivos, alcance y conceptos explicados.
- [x] Comandos de descubrimiento, escaneo y detección documentados.
- [x] Resultado real del escaneo de red incorporado sin alteraciones.
- [x] Duración y cantidad de hosts detectados analizadas.
- [x] Enumeración SMB y parámetros explicados.
- [x] Ejemplos simulados identificados explícitamente.
- [x] Análisis y mitigaciones defensivas incluidos.
