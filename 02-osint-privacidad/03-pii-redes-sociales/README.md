# EH-03 — Recopilación de información personal mediante redes sociales

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** OSINT / Reconocimiento de fuentes abiertas / Privacidad digital  
**Objetivo:** Identificar qué información de identificación personal (PII) puede quedar expuesta públicamente en redes sociales y evaluar cómo podría ser aprovechada por un actor de amenazas.  
**Estado:** Ejercicio resuelto mediante un escenario completamente ficticio. No se investigó a una persona real ni se crearon cuentas falsas.

> **Aviso sobre los datos:** todos los nombres, cuentas, lugares, instituciones, publicaciones y hallazgos de esta práctica son inventados. Se utilizan exclusivamente para responder académicamente las preguntas del laboratorio.

## Perfil ficticio utilizado

| Campo | Dato simulado |
|---|---|
| Nombre | Daniel Andrés Mendoza Vera |
| Usuario principal | `daniel.mendoza.ec` |
| Edad | 27 años |
| Ciudad | Guayaquil, Ecuador |
| Profesión | Técnico de soporte informático |
| Empresa | TecnoRed Solutions (ficticia) |
| Educación | Instituto Tecnológico del Pacífico (ficticio) |
| Pasatiempos | Videojuegos, ciclismo y fotografía |
| Redes sociales | Facebook, Instagram, LinkedIn y X |
| Mascota | Perro llamado Max |

## Parte 1 — Recopile información a través de los medios sociales

### Paso 1. Investigación simulada

En un escenario real, la persona revisaría su propio nombre y variantes de usuario en un navegador privado y motores de búsqueda, y documentaría únicamente información visible públicamente. En esta práctica no se realizaron búsquedas reales: se simularon posibles resultados para no atribuir información inventada a una persona real.

### Paso 2. Auditoría como extraño

Se simularon cuatro perfiles públicos:

- **LinkedIn:** nombre, fotografía profesional, cargo, empresa e historial educativo.
- **Facebook:** publicaciones de cumpleaños, comentarios de familiares y fotografías de reuniones.
- **Instagram:** fotografías de ciclismo, lugares visitados, mascota y etiquetas geográficas.
- **X:** comentarios públicos sobre tecnología, videojuegos y eventos locales.

### Pregunta: ¿Qué tipo de información sobre usted se reveló en su investigación?

**Respuesta:**

En la auditoría simulada del perfil de Daniel Mendoza se identificó que un desconocido podría recopilar los siguientes datos:

1. **Identidad:** nombre completo, edad aproximada y fotografía de perfil.
2. **Información laboral:** empresa donde trabaja, cargo y conocimientos técnicos.
3. **Educación:** institución educativa y formación profesional.
4. **Ubicación:** ciudad de residencia y lugares que frecuenta, inferidos a partir de publicaciones y etiquetas.
5. **Intereses personales:** videojuegos, ciclismo, fotografía y tecnología.
6. **Relaciones personales:** nombres o perfiles de amigos y familiares que aparecen en comentarios y fotografías.
7. **Rutina:** publicaciones repetidas que podrían permitir inferir horarios de trabajo y actividades recreativas.
8. **Preferencias:** marcas de dispositivos y productos tecnológicos mencionados en publicaciones.

Aunque cada dato por separado puede parecer poco importante, su combinación permite elaborar un perfil personal más completo. En este caso, la simulación muestra exposición potencial de identidad, empleo, educación, intereses, relaciones y patrones de actividad.

## Parte 2 — Reflexione sobre sus hallazgos

### 1.a. ¿Cómo puede utilizar un ciberdelincuente la información de su lugar de trabajo?

**Respuesta:**

Un atacante podría utilizar el nombre de la empresa y el cargo de Daniel para preparar mensajes de phishing que aparenten proceder de Recursos Humanos, soporte técnico o un supervisor. También podría investigar la estructura de la organización y enviar archivos o enlaces maliciosos relacionados con sus funciones laborales. Si conoce sus horarios o responsabilidades, podría intentar suplantar a un compañero o manipularlo mediante ingeniería social.

**Riesgos:** phishing dirigido, suplantación de identidad, fraude laboral y acceso no autorizado a cuentas corporativas.

### 1.b. ¿Cómo puede utilizar un ciberdelincuente su interés en pasatiempos o intereses particulares?

**Respuesta:**

Los intereses en videojuegos, ciclismo y fotografía pueden utilizarse para personalizar engaños. Por ejemplo, el atacante podría publicar una supuesta promoción de videojuegos, ofrecer accesorios de ciclismo falsos o enviar enlaces a concursos de fotografía fraudulentos. Estos mensajes pueden resultar creíbles porque están relacionados con actividades que la persona comparte públicamente.

**Riesgos:** robo de credenciales, fraude en compras, descarga de malware y exposición de datos financieros.

### 1.c. ¿Cómo puede utilizar un ciberdelincuente sus conversaciones públicas con amigos o familiares?

**Respuesta:**

Las conversaciones públicas pueden revelar nombres de familiares, relaciones personales, fechas importantes, viajes, lugares frecuentes y detalles privados. Un atacante podría aprovechar esa información para hacerse pasar por un familiar o amigo, inventar una emergencia y solicitar dinero o información confidencial. También puede utilizar los datos para responder preguntas de recuperación de cuentas o construir mensajes de ingeniería social más convincentes.

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

La auditoría simulada demuestra que las redes sociales pueden exponer información suficiente para construir un perfil detallado de una persona. Un atacante podría aprovechar esos datos para desarrollar campañas de phishing dirigido, suplantación de identidad y fraude. La principal medida preventiva consiste en reducir la información pública, controlar la privacidad de las cuentas, proteger el acceso con MFA y revisar periódicamente la huella digital.

## Evidencia y limitaciones

- **Tipo de ejercicio:** simulación académica con identidad inventada.
- **Búsquedas reales:** no realizadas.
- **Cuentas falsas creadas:** ninguna.
- **Capturas de pantalla reales:** no disponibles.
- **Resultados observados:** no aplican; los ejemplos son escenarios hipotéticos.
- **Validación:** la respuesta conceptual está desarrollada; una auditoría personal real requeriría autorización, revisión de los perfiles propios y registro de evidencias sin divulgar PII.

## Referencias de estudio

- Cisco Networking Academy — Ethical Hacker, práctica sobre recopilación de PII en redes sociales.
- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- CISA — Secure Our World: https://www.cisa.gov/secure-our-world
