# EH-08 — Recon-ng: reconocimiento OSINT modular

**Curso:** Cisco NetAcad — Ethical Hacker  
**Área:** Reconocimiento OSINT / Enumeración de dominios  
**Entorno:** Kali Linux  
**Estado:** Guía documentada; ejecución y evidencias reales pendientes.

## 1. Objetivos
- Iniciar Recon-ng y explorar su consola.
- Buscar módulos instalados y en el marketplace.
- Actualizar el marketplace, instalar y cargar un módulo.
- Configurar la fuente de datos y comprender el flujo de trabajo.
- Conocer el almacenamiento y la generación de informes.

> **Autorización:** utiliza dominios propios o expresamente autorizados. Algunos módulos consultan servicios externos y requieren API keys. No recopiles PII de terceros ni publiques claves, correos, tokens o resultados sensibles.

## 2. ¿Qué es Recon-ng?
Recon-ng es un framework modular de reconocimiento OSINT. Sus módulos consultan fuentes de información, organizan los resultados en un espacio de trabajo y permiten preparar informes. La disponibilidad de módulos, nombres, dependencias y proveedores cambia entre versiones.

El material Cisco menciona fuentes como Facebook, Indeed, Flickr, Instagram, Shodan, LinkedIn y YouTube. No implica que todos los módulos sigan disponibles ni que puedan utilizarse sin credenciales, API keys o autorización.

## 3. Preparación e instalación

~~~bash
sudo apt update
apt policy recon-ng
sudo apt install recon-ng -y
recon-ng --version
~~~

- **sudo apt update:** actualiza el índice local de paquetes.
- **apt policy recon-ng:** muestra las versiones disponibles y su origen.
- **sudo apt install recon-ng -y:** instala el paquete si existe en los repositorios configurados.
- **recon-ng --version:** comprueba la versión; si la opción no está admitida, inicia la herramienta y consulta help.

No descargues instaladores de fuentes no verificadas.

## 4. Procedimiento del laboratorio Cisco

Los siguientes comandos se introducen dentro de la consola Recon-ng, salvo que se indique lo contrario. La sintaxis puede variar según la versión.

### Paso 1. Iniciar Recon-ng
~~~bash
recon-ng
~~~
Inicia la consola interactiva.

### Paso 2. Ver los comandos disponibles
~~~text
help
~~~
Muestra los comandos y una descripción de su uso.

### Paso 3. Buscar módulos disponibles
~~~text
modules search
~~~
Busca módulos disponibles localmente. Consulta la ayuda si tu versión requiere filtros.

### Paso 4. Actualizar el marketplace
~~~text
marketplace refresh
~~~
Actualiza el catálogo de módulos. Requiere conectividad.

### Paso 5. Buscar en el marketplace
~~~text
marketplace search
~~~
Consulta el catálogo remoto. Busca términos relacionados con dominios y hosts, y verifica que el módulo esté disponible en tu versión.

### Paso 6. Instalar un módulo
Ejemplo citado en el laboratorio:
~~~text
marketplace install recon/domains-hosts/brute_hosts
~~~
El módulo intenta descubrir nombres de host mediante una lista de palabras y consultas DNS. Es una técnica activa de enumeración; úsala solo en dominios autorizados. Si no aparece en el marketplace, no fuerces su instalación.

### Paso 7. Mostrar los módulos instalados
~~~text
modules search
~~~
Vuelve a consultar los módulos disponibles localmente y comprueba si aparece el módulo instalado.

### Paso 8. Cargar un módulo
~~~text
modules load recon/domains-hosts/brute_hosts
~~~
Carga el módulo en el espacio de trabajo. Utiliza el identificador exacto que muestre tu marketplace.

### Paso 9. Cambiar la fuente
Primero consulta las opciones:
~~~text
options list
~~~
Si la opción de entrada se denomina SOURCE, establece un dominio autorizado:
~~~text
options set SOURCE DOMINIO_AUTORIZADO
options list
~~~
Sustituye DOMINIO_AUTORIZADO por un dominio propio o expresamente autorizado. No interpretes los dominios citados en material didáctico como autorización automática.

### Ejecutar el módulo
~~~text
run
~~~
Ejecuta el módulo con la fuente configurada. Los resultados dependen del DNS, la lista de palabras, la versión y el alcance. No se incluyen resultados ficticios.

## 5. Informes y API keys
Recon-ng organiza los datos en su espacio de trabajo. Antes de exportar, revisa la procedencia, elimina PII innecesaria y no incluyas API keys ni tokens. Consulta help para conocer los comandos de reporting/export de tu versión: la sintaxis puede cambiar.

Algunos módulos necesitan claves API. Obtenlas directamente del proveedor, configura las claves mediante el mecanismo previsto y protege los archivos locales de configuración.

## 6. Registro de ejecución

| Elemento | Registro |
|---|---|
| Fecha y hora | Pendiente |
| Kali Linux / versión | Pendiente |
| Recon-ng / versión | Pendiente |
| Marketplace actualizado | Pendiente |
| Módulo consultado e instalado | Pendiente |
| Dominio autorizado (etiqueta anonimizada) | Pendiente |
| Opciones configuradas | Pendiente |
| Ejecución y resultados | Pendiente |
| Exportación / formato | Pendiente |

**Evidencias:** capturas del inicio, help, búsqueda, marketplace, instalación, opciones y resultados sanitizados. No subas bases de datos completas ni exportaciones originales.

## 7. Preguntas de análisis
1. ¿Qué diferencia existe entre módulos locales y módulos del marketplace?
2. ¿Por qué se actualiza el marketplace?
3. ¿Qué función cumple SOURCE?
4. ¿Qué limitaciones introducen los módulos que requieren API keys?
5. ¿Qué diferencia existe entre OSINT pasivo y enumeración DNS por fuerza bruta?
6. ¿Por qué deben registrarse la fuente y la fecha?
7. ¿Qué información debe excluirse de un informe público?

## 8. Recomendaciones defensivas
- Mantener inventario de dominios y subdominios.
- Retirar registros DNS obsoletos.
- Proteger API keys con mínimo privilegio.
- Verificar los resultados antes de declarar un hallazgo.
- Respetar límites de consulta y el alcance autorizado.

## 9. Referencias
- Cisco NetAcad — Material de laboratorio Recon-ng.
- Recon-ng: https://github.com/lanmaster53/recon-ng
- Kali Linux Tools: https://www.kali.org/tools/
