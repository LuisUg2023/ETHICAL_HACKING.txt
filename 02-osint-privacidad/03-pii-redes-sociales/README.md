# EH-03 — Recopilación de información personal mediante redes sociales

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** OSINT / Reconocimiento de fuentes abiertas / Privacidad digital  
**Objetivo:** Identificar qué información de identificación personal (PII) puede quedar expuesta públicamente en redes sociales y evaluar cómo podría ser aprovechada por un actor de amenazas.  
**Estado:** Ejercicio resuelto mediante un escenario completamente ficticio. No se investigó a una persona real ni se crearon cuentas falsas.

> **Convención de identificación:** se utilizan nombres-código basados en letras del alfabeto griego (ALFA, BETA, GAMMA, DELTA, EPSILON, ZETA, ETA, THETA, LAMBDA y OMEGA). Todos los números se escriben con dígitos latinos (0–9). Los códigos son etiquetas de laboratorio, no nombres personales ni identidades reales.

> **Aviso sobre los datos:** todos los perfiles, usuarios, organizaciones, instituciones, publicaciones y hallazgos de esta práctica son escenarios inventados. No se afirma que existan en redes sociales ni en registros públicos.

## Perfil ficticio utilizado

| Campo | Dato simulado |
|---|---|
| Identificador de persona | PERSONA-ALFA-01 |
| Usuario principal | usuario_beta_07 |
| Edad | 27 años (dato simulado) |
| Ciudad | LOCALIDAD-GAMMA-03 (ubicación ficticia de laboratorio) |
| Profesión | Técnico de soporte informático |
| Empresa | ORG-DELTA-02 |
| Educación | INST-EPSILON-04 |
| Pasatiempos | Videojuegos, ciclismo y fotografía |
| Perfiles simulados | RED-ZETA-01, RED-ETA-02, RED-THETA-03 y RED-LAMBDA-04 |
| Mascota | ANIMAL-OMEGA-01 |

Los identificadores anteriores son etiquetas artificiales para el ejercicio. No representan nombres, usuarios, empresas o instituciones verificadas.

## Parte 1 — Recopile información a través de los medios sociales

### Paso 1. Investigación simulada

En un escenario real, la persona revisaría su propio nombre y variantes de usuario en un navegador privado y motores de búsqueda, y documentaría únicamente información visible públicamente. En esta práctica no se realizaron búsquedas reales: se simularon posibles resultados para no atribuir información inventada a una persona real.

### Paso 2. Auditoría como extraño

Se simularon cuatro perfiles públicos identificados únicamente por códigos:

- **RED-ZETA-01:** cargo, organización codificada e historial educativo.
- **RED-ETA-02:** publicaciones de cumpleaños, comentarios de contactos y fotografías de reuniones.
- **RED-THETA-03:** fotografías de ciclismo, lugares visitados, mascota y etiquetas geográficas.
- **RED-LAMBDA-04:** comentarios públicos sobre tecnología, videojuegos y eventos locales.

### Pregunta: ¿Qué tipo de información sobre usted se reveló en su investigación?

**Respuesta:**

En la auditoría simulada de PERSONA-ALFA-01 se identificó que un desconocido podría recopilar los siguientes datos:

1. **Identidad:** código de perfil, edad aproximada y fotografía simulada.
2. **Información laboral:** organización codificada, cargo y conocimientos técnicos.
3. **Educación:** institución identificada mediante un código y formación profesional.
4. **Ubicación:** localidad codificada y lugares que frecuenta, inferidos a partir de publicaciones y etiquetas.
5. **Intereses personales:** videojuegos, ciclismo, fotografía y tecnología.
6. **Relaciones personales:** perfiles codificados de contactos que aparecen en comentarios y fotografías.
7. **Rutina:** publicaciones repetidas que podrían permitir inferir horarios de trabajo y actividades recreativas.
8. **Preferencias:** marcas de dispositivos y productos tecnológicos mencionados en publicaciones simuladas.

Aunque cada dato por separado puede parecer poco importante, su combinación permite elaborar un perfil personal más completo. En este caso, la simulación muestra exposición potencial de identidad, empleo, educación, intereses, relaciones y patrones de actividad.

## Parte 2 — Reflexione sobre sus hallazgos

### 1.a. ¿Cómo puede utilizar un ciberdelincuente la información de su lugar de trabajo?

**Respuesta:**

Un atacante podría utilizar la organización codificada y el cargo de PERSONA-ALFA-01 para preparar mensajes de phishing que aparenten proceder de Recursos Humanos, soporte técnico o un supervisor. También podría investigar la estructura de una organización y enviar archivos o enlaces maliciosos relacionados con sus funciones laborales. Si conoce sus horarios o responsabilidades, podría intentar suplantar a un compañero o manipularlo mediante ingeniería social.

**Riesgos:** phishing dirigido, suplantación de identidad, fraude laboral y acceso no autorizado a cuentas corporativas.

### 1.b. ¿Cómo puede utilizar un ciberdelincuente su interés en pasatiempos o intereses particulares?

**Respuesta:**

Los intereses en videojuegos, ciclismo y fotografía pueden utilizarse para personalizar engaños. Por ejemplo, el atacante podría publicar una supuesta promoción de videojuegos, ofrecer accesorios de ciclismo falsos o enviar enlaces a concursos de fotografía fraudulentos. Estos mensajes pueden resultar creíbles porque están relacionados con actividades que el perfil comparte públicamente.

**Riesgos:** robo de credenciales, fraude en compras, descarga de malware y exposición de datos financieros.

### 1.c. ¿Cómo puede utilizar un ciberdelincuente sus conversaciones públicas con amigos o familiares?

**Respuesta:**

Las conversaciones públicas pueden revelar códigos de contacto, relaciones personales, fechas importantes, viajes, lugares frecuentes y detalles privados. Un atacante podría aprovechar esa información para hacerse pasar por un familiar o contacto, inventar una emergencia y solicitar dinero o información confidencial. También puede utilizar los datos para responder preguntas de recuperación de cuentas o construir mensajes de ingeniería social más convincentes.

**Riesgos:** fraude por suplantación, estafas familiares, robo de cuentas y exposición de información privada de terceros.

### 2. ¿Qué mejores prácticas recomendaría para mantener su identidad segura en las redes sociales?

**Respuesta:**

1. Configurar los perfiles como privados y limitar quién puede ver publicaciones, fotografías y datos personales.
2. No publicar dirección, teléfono, documentos de identidad, información financiera ni datos de acceso.
3. Evitar publicar en tiempo real la ubicación, los viajes o las rutinas diarias.
4. Desactivar la geolocalización automática de fotografías cuando no sea necesaria y revisar los metadatos antes de compartir imágenes.
5. Revisar etiquetas, menciones y publicaciones realizadas por amigos y familiares.
6. Utilizar contraseñas únicas y robustas para cada servicio, preferiblemente administradas con un gestor de contraseñas.
7. Activar la autenticación multifactor (MFA).
8. Desconfiar de enlaces, promociones, archivos y mensajes que soliciten credenciales o información sensible.
9. Revisar periódicamente las sesiones abiertas, aplicaciones conectadas y permisos de las cuentas.
10. Eliminar publicaciones antiguas que revelen información personal innecesaria.
11. No aceptar solicitudes de amistad o contacto de perfiles desconocidos sin verificar su autenticidad.
12. Mantener actualizados el sistema operativo, navegador y aplicaciones.

## Análisis técnico: PII y OSINT

La PII (Personally Identifiable Information) es información que identifica o puede ayudar a identificar a una persona, directa o indirectamente. OSINT (Open Source Intelligence) es la recopilación y análisis de información disponible en fuentes abiertas.

La práctica demuestra que la exposición no depende únicamente de una publicación individual. La correlación de información laboral, educativa, geográfica y social puede incrementar el riesgo de ingeniería social y suplantación de identidad.

## Conclusión

La auditoría simulada demuestra que las redes sociales pueden exponer información suficiente para construir un perfil detallado. Un atacante podría aprovechar esos datos para desarrollar campañas de phishing dirigido, suplantación de identidad y fraude. La principal medida preventiva consiste en reducir la información pública, controlar la privacidad de las cuentas, proteger el acceso con MFA y revisar periódicamente la huella digital.

## Evidencia y limitaciones

- **Tipo de ejercicio:** simulación académica con identificadores codificados.
- **Convención:** letras griegas para códigos alfabéticos y dígitos latinos (0–9) para números.
- **Búsquedas reales:** no realizadas.
- **Cuentas falsas creadas:** ninguna.
- **Capturas de pantalla reales:** no disponibles.
- **Resultados observados:** no aplican; los ejemplos son escenarios hipotéticos.
- **Validación:** la respuesta conceptual está desarrollada; una auditoría real requeriría autorización, revisión de perfiles propios y registro de evidencias sin divulgar PII.

## Referencias de estudio

- Cisco Networking Academy — Ethical Hacker, práctica sobre recopilación de PII en redes sociales.
- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- CISA — Secure Our World: https://www.cisa.gov/secure-our-world
