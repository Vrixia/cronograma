# Cronograma de Breakout Protocol

Página con el cronograma del proyecto: diagrama de Gantt, lista de tareas editable y resumen de avance. Se publica en GitHub Pages y guarda los datos en Firebase (Cloud Firestore), así que cualquier persona con el enlace ve lo mismo y los cambios aparecen en tiempo real.

## Archivos

| Archivo | Qué es |
| --- | --- |
| `index.html` | La página completa (diseño, Gantt, tabla y lógica). |
| `firebase-config.js` | Aquí van los datos de tu proyecto de Firebase. Es el único archivo que tienes que editar. |
| `firestore.rules` | Reglas de seguridad para pegar en Firebase. |

Mientras `firebase-config.js` esté vacío, la página funciona en **modo local**: sirve para probarla, pero los cambios solo se guardan en el navegador de quien los hace.

## 1. Crear el proyecto de Firebase (gratis)

1. Entra a <https://console.firebase.google.com> con tu cuenta de Google y elige **Crear un proyecto**.
2. Ponle un nombre (por ejemplo `breakout-cronograma`). Puedes desactivar Google Analytics; no se usa.
3. El plan gratuito (Spark) no pide tarjeta y alcanza de sobra para este cronograma.

## 2. Registrar la página y copiar la configuración

1. En la página principal del proyecto, haz clic en el ícono **`</>`** (Web) para agregar una app.
2. Ponle un apodo (por ejemplo `cronograma`). **No** marques Firebase Hosting. Haz clic en **Registrar app**.
3. Firebase te muestra un bloque `const firebaseConfig = { ... }`. Copia cada valor (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`) dentro de las comillas correspondientes en `firebase-config.js`.

Si después necesitas volver a ver esos datos: engrane ⚙ > **Configuración del proyecto** > sección **Tus apps** > opción **Config**.

Estos datos no son secretos; Firebase está diseñado para que vayan dentro de la página. Lo que protege la base de datos son las reglas del paso 4.

## 3. Crear la base de datos

1. En el menú de la izquierda, entra a **Firestore Database** (dentro de **Compilación** / *Build*) y haz clic en **Crear base de datos**.
2. Si te pregunta la edición, elige **Standard**.
3. Deja el ID `(default)` y elige una ubicación cercana, por ejemplo `nam5 (United States)`. La ubicación no se puede cambiar después.
4. Elige **Iniciar en modo de producción** y termina de crearla.

## 4. Publicar las reglas

1. Dentro de Firestore Database, abre la pestaña **Reglas**.
2. Borra todo lo que aparece, pega el contenido completo de `firestore.rules` y haz clic en **Publicar**.

Sin este paso, la página mostrará el aviso “Firebase rechazó el acceso”.

## 5. Publicar en GitHub Pages

1. Sube `index.html`, `firebase-config.js`, `firestore.rules` y este `README.md` a un repositorio público (los tres primeros deben quedar en la raíz).
2. En el repositorio: **Settings** > **Pages** > **Build and deployment** > *Source*: **Deploy from a branch**, rama `main`, carpeta `/ (root)`, **Save**.
3. En uno o dos minutos la página queda en `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`.

## 6. Primera visita

La primera vez que alguien abre la página, se cargan automáticamente las 44 tareas iniciales en Firestore. Después de eso ya no se vuelven a cargar, aunque se borren tareas. Arriba a la derecha debe decir **Sincronizado**; si no aparece nada ahí, la página no está leyendo los datos de `firebase-config.js` y los cambios solo se están guardando en ese navegador.

Comparte el enlace con el equipo y con la profesora.

## Cómo se usa

- **Marcar una tarea como terminada:** la casilla de la izquierda (en el Gantt o en la lista). La tarea pasa a *Completada* y se guarda la fecha de ese día. Si se terminó otro día, cambia la fecha en la columna **Completada el**.
- **Desmarcar:** pide confirmación, regresa la tarea a su estado anterior y borra la fecha de completado.
- **Editar o eliminar:** clic en el nombre de la tarea o en su barra del Gantt. Al eliminar aparece **Deshacer** durante unos segundos.
- **Escalas del Gantt:** *Días*, *Semanas* o *Completo* (todo el proyecto en pantalla).
- **Desfase:** compara la fecha en que se completó contra la fecha límite. En el Gantt, el rombo blanco marca el día en que se completó y lo rayado en rojo son días de retraso.
- **Descargar CSV:** respaldo de todas las tareas que se abre en Excel o Google Sheets.

## Cambios frecuentes

- **Nombres o colores del equipo:** en `index.html`, busca `const PEOPLE` y edita `name`, `initials` o `hex`. Los filtros, la simbología y las barras se actualizan solos.
- **Empezar de cero:** en Firestore, borra la colección `tasks` y el documento `meta/app`, y recarga la página; se vuelven a cargar las 44 tareas iniciales.
- **Límites del plan gratuito:** 50,000 lecturas y 20,000 escrituras al día. Cada visita lee una vez todas las tareas, así que no se acerca a ese límite.
