# EH-10 — Enumeración con Nmap

## 1. Datos generales
- **Curso:** Cisco NetAcad — Ethical Hacker
- **Práctica de referencia:** Práctica de laboratorio — Enumeración con Nmap
- **Categoría:** Reconocimiento activo y enumeración de redes
- **Estado:** Guía documentada; ejecución y evidencias pendientes
- **Sistema previsto:** Kali Linux (máquina virtual del laboratorio Cisco)
- **Herramienta principal:** Nmap
- **Objetivo indicado por Cisco:** `10.6.6.23`
- **Red de laboratorio indicada:** `10.6.6.0/24`

> **Importante:** las direcciones anteriores pertenecen al escenario de laboratorio descrito por Cisco. Ejecuta los comandos únicamente dentro de la topología autorizada del curso y confirma que estás conectado a esa red antes de escanear. No sustituyas el objetivo por una IP pública ni por equipos de terceros. Este documento no afirma que los comandos se hayan ejecutado ni inventa resultados.

## 2. Objetivos
1. Investigar Nmap y su función en el descubrimiento y mapeo de redes.
2. Realizar escaneos básicos sobre el host asignado por el laboratorio.
3. Identificar puertos TCP abiertos, servicios y versiones anunciadas.
4. Interpretar los resultados para determinar qué servicios podrían requerir una revisión defensiva.

## 3. Escenario y alcance
El material Cisco describe una actividad inusual observada mediante Wireshark en una máquina de la DMZ. Se solicita realizar reconocimiento activo para identificar los servicios que ofrece y evaluar si existen aplicaciones que puedan representar un riesgo.

| Elemento | Valor del escenario |
|---|---|
| Red virtual | `10.6.6.0/24` |
| Host objetivo | `10.6.6.23` |
| Sistema de análisis | Kali Linux |
| Técnica | Escaneo de red y enumeración de servicios |
| Restricción | Solo la topología proporcionada y autorizada por Cisco |

La topología del archivo de Cisco también muestra otros equipos de la red virtual. No se deben escanear rangos completos ni direcciones adicionales salvo que el instructor lo solicite expresamente.

## 4. Fundamento técnico
**Nmap (Network Mapper)** es una herramienta de código abierto para descubrir hosts, identificar puertos y obtener información de servicios. Sus resultados dependen del tipo de escaneo, los filtros de red, los permisos y el estado del objetivo.

Conceptos clave:
- **Host discovery:** determina si un equipo parece estar activo.
- **Port scanning:** comprueba el estado de puertos TCP o UDP.
- **Service/version detection:** intenta identificar el servicio y su versión.
- **OS detection:** estima el sistema operativo mediante características de la pila de red; no siempre es concluyente.
- **NSE:** motor de scripts de Nmap. Los scripts pueden generar tráfico adicional; en esta práctica inicial no se ejecutan scripts intrusivos.

## 5. Preparación del entorno

### Paso 1. Confirmar que Nmap está instalado
```bash
nmap --version
```
**Explicación:** muestra la versión instalada y permite comprobar que el ejecutable está disponible.

Si no está instalado en la Kali del laboratorio:
```bash
sudo apt update
sudo apt install nmap
```
**Explicación:**
- `sudo`: ejecuta la operación con privilegios administrativos.
- `apt update`: actualiza el índice local de paquetes.
- `apt install nmap`: instala Nmap desde los repositorios configurados.

### Paso 2. Confirmar la interfaz y la dirección local
```bash
ip -br address
ip route
```
**Explicación:**
- `ip -br address`: muestra interfaces y direcciones IP en formato resumido.
- `ip route`: muestra las rutas disponibles y ayuda a confirmar la red conectada.

Comprueba que la máquina Kali está conectada al segmento de laboratorio esperado. Si no estás en la red Cisco indicada, detente y no ejecutes los escaneos de las siguientes secciones.

## 6. Procedimiento de enumeración

### Paso 3. Comprobar disponibilidad del host objetivo
```bash
ping -c 4 10.6.6.23
```
**Explicación:**
- `ping`: envía solicitudes ICMP Echo.
- `-c 4`: limita la prueba a cuatro solicitudes.
- `10.6.6.23`: host asignado en el escenario.

**Interpretación:** recibir respuestas indica que el host responde a ICMP. La ausencia de respuesta no demuestra que esté apagado: un firewall puede bloquear ICMP.

### Paso 4. Realizar descubrimiento de un único host
```bash
nmap -sn 10.6.6.23
```
**Explicación:**
- `nmap`: inicia el escáner.
- `-sn`: realiza descubrimiento de host y omite el escaneo de puertos.
- `10.6.6.23`: limita el alcance a la IP indicada.

Registra si Nmap informa que el host está activo. No interpretes la ausencia de respuesta como prueba definitiva de que el equipo no existe.

### Paso 5. Escaneo básico de puertos TCP
```bash
nmap 10.6.6.23
```
**Explicación:** realiza el escaneo TCP predeterminado de los puertos más comunes de Nmap. El resultado puede clasificar puertos como `open`, `closed` o `filtered`.

### Paso 6. Identificar servicios y versiones
```bash
nmap -sV 10.6.6.23
```
**Explicación:**
- `-sV`: sondea los puertos detectados para intentar identificar el servicio y su versión.
- La identificación es una estimación basada en respuestas; puede ser incompleta o incorrecta.

### Paso 7. Revisar el sistema operativo estimado
```bash
sudo nmap -O 10.6.6.23
```
**Explicación:**
- `sudo`: permite utilizar las técnicas que requieren privilegios elevados.
- `-O`: habilita la detección de sistema operativo.
- La detección puede fallar si hay pocos puertos observables, filtrado o dispositivos intermedios.

### Paso 8. Guardar resultados en un archivo
```bash
mkdir -p evidencias/EH-10
nmap -sV -oN evidencias/EH-10/nmap-servicios.txt 10.6.6.23
```
**Explicación:**
- `mkdir -p`: crea el directorio de evidencias si todavía no existe.
- `-sV`: intenta identificar servicios y versiones.
- `-oN`: guarda la salida en formato normal legible.
- La ruta del archivo permite conservar el resultado para su análisis posterior.

El archivo solo contendrá datos reales después de ejecutar el comando en la máquina del laboratorio.

## 7. Opciones de Nmap que debes reconocer

| Opción | Función |
|---|---|
| `-A` | Habilita detección de SO, versiones, scripts por defecto y traceroute. Es más intrusiva; no usar sin indicación y autorización del laboratorio. |
| `-O` | Detección estimada del sistema operativo. |
| `-p` | Especifica puertos o rangos de puertos, por ejemplo `-p 22,80,443`. |
| `-sF` | Escaneo TCP FIN. Su interpretación depende del sistema y de los filtros. |
| `-sn` | Descubrimiento de hosts sin escaneo de puertos. |
| `-sS` | Escaneo TCP SYN; normalmente requiere privilegios elevados. |
| `-sT` | Escaneo TCP connect(), completa la conexión TCP. |
| `-sV` | Detección de servicios y versiones. |
| `-T0` a `-T5` | Plantillas de temporización; valores altos son más rápidos y pueden resultar más ruidosos. |

No ejecutes todas las opciones en bloque. Selecciona la técnica que corresponda al objetivo del paso y a las instrucciones del instructor.

## 8. Registro de resultados observados

Completa esta sección con la salida obtenida en Kali. No rellenes los campos con resultados supuestos.

| Comprobación | Resultado real |
|---|---|
| Versión de Nmap | Pendiente |
| Conectividad ICMP | Pendiente |
| Estado del host según `-sn` | Pendiente |
| Puertos TCP detectados | Pendiente |
| Servicios y versiones | Pendiente |
| Sistema operativo estimado | Pendiente |
| Archivo de salida generado | Pendiente |

### Plantilla para describir cada puerto
| Puerto/protocolo | Estado | Servicio detectado | Versión | Observación |
|---|---|---|---|---|
| Pendiente | Pendiente | Pendiente | Pendiente | Completar con la salida real |

## 9. Análisis de seguridad
Al interpretar los resultados:
1. Verifica si cada puerto abierto corresponde a un servicio esperado para el rol del host.
2. Comprueba si la versión identificada tiene soporte del fabricante y actualizaciones disponibles.
3. No declares una vulnerabilidad solo porque un puerto esté abierto o porque Nmap identifique una versión.
4. Valida los hallazgos con inventario, configuración del servicio y fuentes oficiales del fabricante.
5. Considera falsos positivos, banners modificados, proxies, firewalls y limitaciones de detección.

## 10. Mitigaciones defensivas
- Deshabilitar servicios que no sean necesarios.
- Restringir puertos mediante reglas de firewall y segmentación de red.
- Mantener servicios y sistemas operativos actualizados.
- Evitar exponer interfaces administrativas a redes no confiables.
- Aplicar autenticación robusta y registros de auditoría.
- Repetir el escaneo autorizado después de aplicar cambios para comprobar su eficacia.

## 11. Evidencias pendientes
- [ ] Captura de `nmap --version`.
- [ ] Captura de la interfaz y rutas de Kali, ocultando datos ajenos al laboratorio.
- [ ] Salida real de `ping` o explicación de por qué no responde.
- [ ] Salida real de `nmap -sn`.
- [ ] Salida real de escaneo de puertos.
- [ ] Salida real de `nmap -sV`.
- [ ] Salida real de detección de SO, si se ejecutó.
- [ ] Archivo `nmap-servicios.txt` revisado y sanitizado.

## 12. Conclusiones
La práctica introduce el reconocimiento activo con Nmap y la enumeración de puertos, servicios y versiones. La documentación establece un procedimiento reproducible y acotado al host indicado por Cisco. Las conclusiones técnicas sobre el equipo quedan pendientes hasta disponer de resultados obtenidos en la máquina Kali del laboratorio.

## 13. Referencias
- Cisco NetAcad — *Práctica de laboratorio: Enumeración con Nmap* (material proporcionado para el curso).
- Nmap Reference Guide: https://nmap.org/book/man.html
- Nmap Documentation: https://nmap.org/docs.html

## 14. Estado de validación
- [x] Objetivos y alcance documentados.
- [x] Comandos y opciones explicados.
- [x] Restricciones de autorización indicadas.
- [ ] Comandos ejecutados en la topología Cisco.
- [ ] Resultados reales registrados.
- [ ] Evidencias guardadas y sanitizadas.
- [ ] Análisis validado con los resultados.
