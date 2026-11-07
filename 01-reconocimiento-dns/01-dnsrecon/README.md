# EH-01 — Reconocimiento y enumeración DNS con DNSRecon

**Área:** Reconocimiento / OSINT técnico  
**Entorno:** Kali Linux  
**Objetivo de laboratorio documentado:** `h4cker.org`  
**Estado:** Documentada a partir de la salida compartida; repetir la consulta para obtener evidencia fechada y verificar si los registros siguen vigentes.

## 1. Objetivo
Utilizar DNSRecon para identificar y analizar registros DNS públicos de un dominio, comprender su infraestructura de resolución y reconocer información que puede ser útil durante una evaluación de seguridad autorizada.

## 2. Preparación en Kali Linux
```bash
sudo apt update
sudo apt install dnsrecon -y
dnsrecon -h
```

Comprobar herramientas complementarias:
```bash
dig -v
nslookup -version
```

## 3. Reconocimiento básico
```bash
nslookup h4cker.org
dig h4cker.org A
dig +short h4cker.org A
dnsrecon -d h4cker.org
dnsrecon -d h4cker.org -t std
```

La opción `-d` indica el dominio objetivo. El tipo `std` realiza comprobaciones estándar de registros DNS.

## 4. Enumeración adicional
Solo dentro del alcance permitido por el laboratorio:
```bash
dnsrecon -d DOMINIO_AUTORIZADO -t brt -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

Consultas específicas:
```bash
dig DOMINIO_AUTORIZADO MX
dig DOMINIO_AUTORIZADO NS
dig DOMINIO_AUTORIZADO SOA
dig DOMINIO_AUTORIZADO TXT
dig DOMINIO_AUTORIZADO AAAA
```

La transferencia de zona AXFR debe probarse únicamente cuando el laboratorio lo autorice:
```bash
dig @SERVIDOR_DNS DOMINIO_AUTORIZADO AXFR
```

## 5. Resultados observados en la salida compartida
La ejecución compartida de `dnsrecon -d h4cker.org` mostró:

| Elemento | Resultado observado | Interpretación |
|---|---|---|
| A | `185.199.109.153` | IPv4 asociada al dominio en esa consulta |
| SOA | `ns-cloud-c1.googledomains.com` — `216.239.32.108` | Servidor indicado en el registro de autoridad |
| NS | `ns-cloud-c1` a `ns-cloud-c4.googledomains.com` | Servidores de nombres publicados |
| MX | `aspmx.l.google.com` y `alt1` a `alt4.aspmx.l.google.com` | Infraestructura de correo de Google |
| TXT/SPF | `v=spf1 include:_spf.google.com ~all` | Política SPF que autoriza el mecanismo incluido y aplica softfail al resto |
| DNSSEC | DNSKEY con NSEC3, ZSK, KSK y RSASHA256 | Se observó configuración de DNSSEC |
| SRV | 0 registros encontrados | No se encontraron registros SRV en esa ejecución |

### Advertencia de interpretación
Los valores anteriores representan la salida compartida durante el ejercicio, no una garantía de que los registros continúen iguales. DNS es dinámico. La IP `185.199.109.153` debe tratarse como resultado observado en esa fecha, no como valor permanente.

La salida también presentó un `DeprecationWarning` relacionado con `dns.resolver.Resolver.resolve()`. Es una advertencia de compatibilidad de la biblioteca; por sí sola no indica que DNSRecon haya fallado.

## 6. Análisis de seguridad
- Los registros A y AAAA permiten identificar direcciones asociadas a un dominio.
- Los NS y SOA revelan información sobre la administración y delegación DNS.
- Los MX exponen proveedores y servidores de correo.
- Los TXT pueden revelar políticas SPF, verificaciones de servicios y otros metadatos.
- DNSSEC aporta autenticidad e integridad a la respuesta DNS cuando está correctamente desplegado; no cifra las consultas.
- La ausencia de SRV en el resultado no demuestra que no existan servicios en otros nombres o zonas.

## 7. Riesgos y controles defensivos
| Exposición | Riesgo potencial | Control recomendado |
|---|---|---|
| Registros DNS innecesarios | Divulgación de nombres y servicios | Revisar y depurar registros públicos |
| SPF permisivo o incorrecto | Suplantación de correo | Revisar SPF y complementar con DKIM/DMARC |
| DNSSEC ausente o mal configurado | Riesgo de manipulación de respuestas | Validar despliegue y renovación de claves |
| Subdominios olvidados | Superficie de ataque no inventariada | Inventario, responsables y revisión periódica |

## 8. Evidencias pendientes
- [ ] Captura de `dnsrecon -d h4cker.org` con fecha.
- [ ] Versión de Kali y DNSRecon.
- [ ] Guardar salida en texto, eliminando datos sensibles si los hubiera.
- [ ] Repetir `dig h4cker.org A` y comparar resultados.
- [ ] Registrar si la advertencia deprecada persiste.

Para guardar una salida:
```bash
mkdir -p evidencias
dnsrecon -d h4cker.org | tee evidencias/EH-01-dnsrecon.txt
```

## 9. Conclusión
DNSRecon permite automatizar la recopilación de registros DNS y organizar información útil para el reconocimiento inicial. La salida analizada permitió identificar registros de direccionamiento, autoridad, nombres, correo, SPF y DNSSEC. El siguiente paso es repetir la ejecución y adjuntar evidencia fechada para convertir esta documentación en una práctica verificada.

## 10. Referencias
- DNSRecon: https://github.com/darkoperator/dnsrecon
- Kali Linux Tools: https://www.kali.org/tools/dnsrecon/
- SecLists: https://github.com/danielmiessler/SecLists
