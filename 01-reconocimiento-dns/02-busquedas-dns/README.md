# EH-02 — Práctica de laboratorio: Búsquedas de DNS

**Curso:** Cisco NetAcad — Ethical Hacker  
**Herramientas:** `nslookup`, `dig`, `whois`, `host`  
**Estado:** Guía de resolución documentada; los datos variables deben validarse en la ejecución real del laboratorio.

## 1. Objetivo
Consultar registros DNS, obtener información de dominios y direcciones IP, comparar herramientas de consulta y realizar resolución DNS inversa.

## 2. Parte 1 — Consultas con nslookup

### Consultar registros MX
```text
nslookup
> set type=mx
> cisco.com
```
`set type=mx` cambia el tipo de registro solicitado a MX.

### Consultar un servidor DNS
```text
nslookup
> set type=a
> ns1.cisco.com
```
En los datos trabajados anteriormente se identificó la IPv4 `72.163.5.201` para `ns1.cisco.com`. Para obtener IPv6, ejecutar:
```text
nslookup
> set type=aaaa
> ns1.cisco.com
```
No se registra una IPv6 como resultado confirmado porque no se dispone de esa salida.

### Consultar con un servidor DNS específico y tipo ANY
```text
nslookup
> server 8.8.8.8
> set type=any
> netacad.com
```
**Nota:** una consulta ANY no garantiza que el servidor entregue todos los registros. Algunos servidores limitan o sintetizan la respuesta.

## 3. Parte 2 — Consultas WHOIS

```bash
whois cisco.com
whois netacad.com
whois 72.163.5.201
```

WHOIS puede mostrar registrar, fechas de registro/expiración, servidores de nombres, organización responsable y datos de asignación IP. La información disponible depende del registro, del RIR y de las políticas de privacidad.

En el análisis anterior se identificó para la dirección `72.163.5.201` el rango `72.163.0.0 - 72.163.255.255`, equivalente a `72.163.0.0/16`, y la referencia de organización Cisco Systems, Inc. Se debe conservar la salida WHOIS original para confirmar los campos y la fecha de consulta.

## 4. Parte 3 — Consultas con dig

```bash
dig cisco.com
dig cisco.com A
dig cisco.com AAAA
dig cisco.com NS
dig cisco.com MX
dig netacad.com ANY
dig @8.8.8.8 cisco.com NS
```

- `dig` consulta A si no se especifica un tipo.
- `AAAA` solicita direcciones IPv6.
- `NS` consulta servidores de nombres.
- `MX` consulta servidores de correo.
- `ANY` solicita información general, pero no asegura una lista completa.

La salida de dig se organiza en secciones como QUESTION, ANSWER, AUTHORITY y ADDITIONAL, lo que facilita el análisis técnico.

## 5. Parte 4 — Resolución DNS inversa

La resolución inversa utiliza registros PTR. Para consultar:
```bash
dig -x 72.163.5.201
dig -x 72.163.1.1
nslookup 72.163.5.201
host 72.163.10.1
```

El nombre PTR de `72.163.1.1` debe obtenerse ejecutando la consulta. No se debe afirmar que corresponde a un router, gateway o dirección virtual HSRP sin evidencia del resultado. Si el nombre devuelto contiene una referencia explícita a HSRP, puede analizarse esa hipótesis con el contexto del laboratorio.

## 6. Comparación de herramientas

| Herramienta | Uso principal | Característica |
|---|---|---|
| nslookup | Consultas DNS directas e interactivas | Sencilla para consultas rápidas |
| dig | Consultas DNS detalladas | Muestra secciones y metadatos de la respuesta |
| host | Resolución directa e inversa | Salida breve y legible |
| whois | Datos de registro/asignación | Información de dominio o bloque IP, no sustituye una consulta DNS |

## 7. Respuestas conceptuales de la práctica

1. Para consultar MX en nslookup se utiliza `set type=mx`.
2. La IPv4 de `ns1.cisco.com` registrada en el material trabajado fue `72.163.5.201`. La IPv6 queda pendiente de consulta AAAA.
3. ANY solicita información general, pero la respuesta depende del servidor DNS y no equivale a descargar toda la zona.
4. WHOIS permite comparar registrar, fechas, contactos publicados y servidores de nombres. Completar con las salidas actuales de ambos dominios.
5. El rango trabajado para `72.163.5.201` fue `72.163.0.0/16`; verificar con WHOIS/RDAP antes de presentar como dato vigente.
6. dig consulta A por defecto cuando no se especifica otro tipo. El comportamiento de nslookup puede variar por plataforma y configuración; registrar la salida exacta usada en el laboratorio.
7. dig facilita el análisis detallado de las secciones de respuesta; nslookup prioriza simplicidad.
8. El registro de resolución inversa es PTR.
9. El significado de `72.163.1.1` depende del PTR real. No asumir HSRP sin evidencia.
10. host suele producir una salida concisa; dig ofrece más detalle; nslookup permite modo interactivo.
11. La combinación de nslookup, dig, host y whois ayuda a recopilar y contrastar información DNS durante el reconocimiento autorizado.

## 8. Evidencias que deben adjuntarse
- [ ] Salida de `nslookup` para MX de cisco.com.
- [ ] Consulta A y AAAA de ns1.cisco.com.
- [ ] Consulta ANY de netacad.com.
- [ ] Salidas WHOIS de cisco.com, netacad.com y 72.163.5.201.
- [ ] Salida de `dig cisco.com`.
- [ ] Consultas PTR de 72.163.5.201 y 72.163.1.1.
- [ ] Consultas `host` y comparación de resultados.

Guardar evidencia:
```bash
mkdir -p evidencias/EH-02
dig cisco.com | tee evidencias/EH-02/dig-cisco.txt
dig -x 72.163.5.201 | tee evidencias/EH-02/ptr-72.163.5.201.txt
whois 72.163.5.201 | tee evidencias/EH-02/whois-ip.txt
```

## 9. Conclusión
La práctica permite diferenciar la resolución directa de nombres, la resolución inversa y la consulta de información de registro. La documentación conceptual está preparada; las respuestas que dependen de IPv6, WHOIS y PTR deben completarse con la salida real del laboratorio.

## 10. Referencias
- Cisco Networking Academy: material de la práctica Búsquedas de DNS.
- BIND dig: https://bind9.readthedocs.io/
- ICANN Lookup: https://lookup.icann.org/
- ARIN RDAP: https://search.arin.net/rdap/
