# EH-05 — Google Dorks para reconocimiento OSINT

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** OSINT / Reconocimiento pasivo / Motores de búsqueda  
**Objetivo:** Utilizar operadores avanzados de Google para acotar búsquedas de información pública y comprender cómo una indexación inadecuada puede exponer recursos de una organización.  
**Estado:** Guía de laboratorio. Las consultas son ejemplos didácticos; no se afirma que hayan sido ejecutadas ni que hayan producido resultados.

> **Uso responsable:** limitar las búsquedas a dominios propios, entornos de laboratorio o activos para los que exista autorización. No descargar, abrir, copiar ni divulgar documentos sensibles encontrados accidentalmente. Si aparece información privada, detener la exploración y notificar al responsable del sitio por un canal apropiado.

## 1. ¿Qué son los Google Dorks?

Google Dorks (también llamados Google Hacking) son consultas que combinan términos y operadores de búsqueda para filtrar páginas indexadas. No son una vulnerabilidad por sí mismos: permiten localizar contenido que el buscador ya puede encontrar.

Los resultados dependen de la indexación, región, idioma, configuración y cambios del motor de búsqueda. Una consulta sin resultados no demuestra que un recurso no exista.

## 2. Operadores básicos

| Operador | Función | Ejemplo inocuo |
|---|---|---|
| `site:` | Restringe resultados a un dominio o sitio. | `site:example.com` |
| `filetype:` | Filtra por extensión o tipo de archivo. | `site:example.com filetype:pdf` |
| `intitle:` | Busca términos en el título de la página. | `site:example.com intitle:manual` |
| `inurl:` | Busca términos en la URL. | `site:example.com inurl:ayuda` |
| `intext:` | Busca términos dentro del texto. | `site:example.com intext:contacto` |
| Comillas `"..."` | Busca una frase exacta. | `site:example.com "política de privacidad"` |
| Signo menos `-` | Excluye un término. | `site:example.com manual -borrador` |
| `OR` | Busca una alternativa u otra. | `site:example.com (manual OR guía)` |

Los operadores se pueden combinar. El buscador puede interpretar o ignorar algunos operadores; revisa siempre la consulta y los resultados.

## 3. Preparar una búsqueda dentro de un alcance autorizado

Usa `example.com`, dominio reservado para ejemplos, para comprender la sintaxis. Para una auditoría real, reemplázalo únicamente por un dominio propio o expresamente autorizado.

### Consulta 1 — Documentos públicos PDF

```text
site:example.com filetype:pdf
```

**Interpretación:** limita los resultados al sitio de ejemplo y a documentos PDF indexados.

### Consulta 2 — Manuales publicados

```text
site:example.com intitle:manual
```

**Interpretación:** busca páginas cuyo título incluya el término `manual`.

### Consulta 3 — Páginas de ayuda

```text
site:example.com inurl:ayuda
```

**Interpretación:** busca URLs que contengan la palabra `ayuda`.

### Consulta 4 — Políticas de privacidad

```text
site:example.com "política de privacidad"
```

**Interpretación:** busca la frase exacta dentro del contenido indexado del dominio.

### Consulta 5 — Tipos de documentos autorizados

```text
site:example.com (filetype:pdf OR filetype:docx)
```

**Interpretación:** busca archivos PDF o DOCX indexados en el dominio de ejemplo. Solo se deben revisar documentos destinados a publicación.

## 4. Flujo de trabajo del laboratorio

1. Definir por escrito el dominio y el alcance autorizado.
2. Comenzar con consultas generales como `site:DOMINIO-AUTORIZADO`.
3. Aplicar filtros de tipo de archivo, título, URL o frase.
4. Registrar únicamente metadatos no sensibles del resultado: fecha, consulta, título público y URL pública.
5. Si se observa un posible archivo privado, no abrirlo ni descargarlo; registrar el hallazgo mínimo necesario y notificar al propietario.
6. Recomendar controles: retirar la publicación, corregir permisos, configurar autenticación y revisar la política de indexación.
7. Repetir la búsqueda después de la corrección para comprobar la reducción de exposición, sin intentar eludir controles de acceso.

## 5. Registro de consultas y resultados

Completa con resultados realmente observados. No inventes títulos, enlaces ni cantidades.

| N.º | Consulta ejecutada | Dominio autorizado | Resultado observado | Evidencia |
|---|---|---|---|---|
| 1 | `site:DOMINIO-AUTORIZADO` | Pendiente | Pendiente | Pendiente |
| 2 | `site:DOMINIO-AUTORIZADO filetype:pdf` | Pendiente | Pendiente | Pendiente |
| 3 | `site:DOMINIO-AUTORIZADO intitle:manual` | Pendiente | Pendiente | Pendiente |
| 4 | `site:DOMINIO-AUTORIZADO inurl:ayuda` | Pendiente | Pendiente | Pendiente |

### Preguntas de análisis

1. ¿Qué operadores se utilizaron y qué filtro aplicó cada uno?
2. ¿Qué tipo de contenido público apareció?
3. ¿Se encontró algún indicio de exposición accidental? Describe solo la categoría, sin reproducir datos sensibles.
4. ¿Qué controles podrían evitar la indexación de contenido que no debe ser público?
5. ¿Por qué `robots.txt` no debe considerarse un mecanismo de control de acceso?

## 6. Controles defensivos para administradores

- No almacenar información confidencial en directorios públicos del servidor web.
- Aplicar autenticación y autorización en el servidor; ocultar enlaces no protege un recurso.
- Revisar permisos y configuración de publicación de documentos.
- Utilizar `noindex` cuando corresponda para controlar la indexación, recordando que no sustituye la autenticación.
- Retirar del servidor archivos publicados por error y solicitar la actualización o eliminación de resultados en el buscador.
- Revisar periódicamente los recursos públicos y los registros del servidor.
- Evitar publicar copias de respaldo, archivos temporales o documentación interna.

**Importante:** `robots.txt` comunica directivas a rastreadores cooperativos; no impide que una persona acceda directamente a un archivo ni protege contenido confidencial.

## 7. Evidencias requeridas

- Captura de la consulta con el dominio autorizado visible.
- Captura de los resultados públicos, ocultando cualquier dato personal o sensible.
- Tabla con consulta, fecha y resultado observado.
- Conclusión sobre exposición y controles defensivos.

**Ejecución pendiente:** esta guía no contiene resultados reales de Google. Añade capturas obtenidas durante tu propio laboratorio autorizado antes de marcar la práctica como verificada.

## Referencias

- Google Search Help — Refine web searches: https://support.google.com/websearch/answer/2466433
- Google Search Central — Robots.txt: https://developers.google.com/search/docs/crawling-indexing/robots/intro
- OWASP — Information Gathering: https://owasp.org/www-project-web-security-testing-guide/
