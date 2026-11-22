# Implementa Quiz: registrar los resultados de un cuestionario HTML5 en Google Drive

Con el Asistente v2 de Implementa Quiz conviertes un cuestionario HTML5 en una versión que envía sus resultados a una hoja de cálculo dentro de una carpeta de Drive. El cuestionario se procesa en tu navegador; el registro lo escribe un script de Google Apps Script que se ejecuta con tu cuenta.

## Dos modelos

|  | Modelo institucional | Modelo personal |
|---|---|---|
| Archivo del asistente | `index.html` | `index-otrascuentas.html` |
| Quién puede responder | Solo cuentas del dominio institucional | Cuentas institucionales y cualquier otra cuenta de Google |
| Acceso de la aplicación web | Solo dentro del dominio | Cualquier persona |
| Qué se registra como identificación | El correo institucional | El correo institucional, o el nombre completo si usa otra cuenta |
| Al abrir el cuestionario adaptado | Nada adicional | Pregunta con qué cuenta ingresó y, si es otra, pide nombre y apellido |

## Antes de empezar

- Una cuenta de Google propietaria de la carpeta (será la cuenta que ejecute el script).
- El cuestionario en un archivo HTML5 (tamaño recomendado: hasta 5 MB).
- El dominio de tu institución, por ejemplo `@pascualbravo.edu.co`.

## Fase 1. Crear la carpeta en Drive

1. Crea una carpeta nueva en Google Drive. Allí se guardará la hoja *Registro de cuestionarios*.
2. Compártela con la institución y asígnale el rol **Editor**.
3. Copia el enlace de la carpeta (tiene la forma `https://drive.google.com/drive/folders/...`).

## Fase 2. Preparar el cuestionario en el asistente

Abre `index.html` (modelo institucional) o `index-otrascuentas.html` (modelo personal). La herramienta no guarda tu avance, así que mantén esta pestaña abierta hasta la Fase 4.

4. En **Paso 01 · Configuración**, arrastra o selecciona el cuestionario en *Archivo HTML5*.
5. Pega el enlace de la carpeta en *Nueva carpeta de Google Drive*. El asistente extrae el ID automáticamente.
6. Escribe el dominio en *Dominio institucional* (`@pascualbravo.edu.co` u otro).
7. Opcional: revisa *Nombre del cuestionario* (se detecta desde el HTML) y elige la *Versión* (A, B, Pretest, Postest o 1.0).
8. Clic en *Analizar cuestionario*. El botón solo se activa cuando el archivo, el enlace y el dominio están completos.
9. En **Paso 02 · Diagnóstico**, revisa los conteos: preguntas, campos de respuesta, señales de resultado y scripts. Deja el mapeo en sus valores predeterminados (*Detectar automáticamente*, *Detectar resultado visible*, *Al finalizar o enviar*) y cámbialo solo si el registro no captura bien el nombre, el resultado o el momento del envío.
10. Clic en *Generar versión adaptada*.

## Fase 3. Crear y activar el script en Apps Script

Haz esta fase con la cuenta propietaria de la carpeta: el script se ejecuta como esa cuenta.

11. En **Paso 03 · Entregables**, clic en *Descargar Apps Script*. Se descarga el archivo `Codigo.gs`.
12. Entra a [Google Apps Script](https://script.google.com/home/?hl=es-419), clic en *Nuevo proyecto* y ponle nombre.
13. Abre `Codigo.gs` con el bloc de notas, selecciona todo el contenido y cópialo.
14. En el editor, borra `function myFunction()` y pega el código copiado. Guarda el proyecto.
15. Selecciona la función `configurarRegistro` y clic en *Ejecutar*. Autoriza los permisos que pida (Drive y Hojas de cálculo). Al terminar, la hoja *Registro de cuestionarios* debe aparecer en tu carpeta.
16. Clic en *Implementar* > *Nueva implementación* y elige el tipo *Aplicación web*.
17. Configura **Ejecutar como: Yo** (la cuenta dueña de la carpeta) y **Quién tiene acceso: Solo dentro del dominio** (cuenta institucional).
18. Clic en *Implementar* y copia la URL de la aplicación web. Termina en `/exec`; guárdala.

## Fase 4. Conectar, descargar y probar

19. Vuelve al asistente (Paso 03) y pega la URL en *URL del despliegue de Apps Script*. Queda incorporada en el HTML que descargues.
20. Clic en *Descargar HTML adaptado*. El archivo se llama `cuestionario_adaptado_...html`.
21. Pruébalo con una cuenta institucional distinta de la propietaria: responde el cuestionario y revisa que la hoja reciba una fila con fecha, correo, cuestionario, versión, puntaje, nivel, dimensiones y respuestas.
22. Comparte el cuestionario adaptado. ¡Listo!

## Modelo personal (`index-otrascuentas.html`)

El procedimiento es el mismo, con estas diferencias:

- **Fase 2:** Usa `index-otrascuentas.html` como asistente.
- **Paso 17:** Configura **Quién tiene acceso: Cualquier persona**. Es obligatorio: con *Solo dentro del dominio*, las cuentas externas no llegan al script y sus resultados no se guardan.
- **Paso 18:** Además de copiar la URL, puedes abrirla en el navegador: en este modelo el script crea la hoja de registro si aún no existe.
- **Paso 20:** El HTML adaptado pide, al abrirlo, *¿Con qué cuenta de Google ingresaste?*. Quien elige *Cuenta institucional* queda registrado con su correo; quien elige *Otra cuenta de Google* escribe nombre y apellido.
- **Paso 21:** Prueba con una cuenta institucional y con otra cuenta de Google. En este caso el nombre completo aparece en la columna *Correo institucional*.

Como el acceso es abierto, el nombre es autodeclarado: el script no puede verificarlo.

## Verificación final

- [ ] Ejecuté `configurarRegistro()` y la hoja apareció en la carpeta nueva.
- [ ] Pegué la URL `/exec` en el asistente y descargué el HTML adaptado después de hacerlo.
- [ ] Probé con una cuenta distinta de la propietaria (en el modelo personal, también con una externa).
- [ ] La fila contiene nombre, versión, puntaje y respuestas.
- [ ] Confirmé permisos, privacidad y que no hay envíos duplicados.

## Problemas frecuentes

- ***Analizar cuestionario* está desactivado.** Falta el archivo HTML5, el enlace de la carpeta o el dominio.
- **Aparece «Cuenta institucional no autorizada».** La cuenta que responde no pertenece al dominio configurado. Usa el modelo personal si debe aceptar otras cuentas.
- **Aparece «Ejecuta configurarRegistro() primero».** Falta el paso 15. Ejecútalo y vuelve a probar.
- **Cambié el código del script y no se nota el cambio.** Ve a *Implementar* > *Administrar implementaciones* > *Editar* y elige *Versión: Nueva versión*.
- **No llegan los resultados de cuentas externas.** La aplicación web no está en *Cualquier persona*. Corrígelo y publica una nueva versión.
