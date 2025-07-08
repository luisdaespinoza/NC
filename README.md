# NC por Patios
"NC por Patios" es una aplicación web diseñada para la gestión y visualización de No Conformidades (NC) en diferentes áreas o "patios" de una instalación. Permite a los usuarios marcar ubicaciones específicas en mapas, añadir detalles sobre las no conformidades, y realizar un seguimiento de su estado. La aplicación cuenta con un sistema de roles de usuario (Visualizador, Administrador, Super Administrador) para controlar el acceso y las funcionalidades.

## Características
* **Visualización de Mapas Interactivos:** Muestra diferentes mapas donde se pueden ubicar pines de no conformidades.

* **Gestión de Pines (No Conformidades):**

* **Creación, edición y eliminación de pines (solo para administradores).**

* **Asignación de título, descripción, tipo de NC, estatus, fecha de creación, encargado y URL de imagen.**

* **Posicionamiento interactivo de pines en el mapa mediante clic.**

* **Roles de Usuario:**

Visualizador: Solo puede ver los pines y la información detallada.

Administrador: Puede crear, editar y eliminar pines, así como descargar y subir datos CSV.

Super Administrador: Tiene todas las capacidades del administrador y, además, puede gestionar usuarios (añadir, eliminar).

* **Dashboard de Análisis:** Proporciona estadísticas y gráficos sobre las no conformidades por mapa, encargado, tipo y estatus, permitiendo una visión rápida del estado general.

* **Exportación/Importación de Datos:** Permite descargar todos los pines en formato CSV y subir nuevos pines desde un archivo CSV (solo para administradores), facilitando la migración o el respaldo de datos.

* **Autenticación Sencilla:** Sistema de login basado en usuarios y contraseñas.

* **Diseño Responsivo:** La interfaz se adapta para una visualización adecuada en diferentes dispositivos, desde computadoras de escritorio hasta móviles.

## Estructura del Proyecto
El proyecto se compone de los siguientes archivos principales HTML, que interactúan con Firebase Firestore para la gestión de datos:

* **login.html:** Página de inicio de sesión de la aplicación.

* **index.html:** La página principal donde se visualizan los mapas y se gestionan las No Conformidades.

* **user_management.html:** Interfaz para la creación y eliminación de usuarios (accesible solo para Super Administradores).

* **dashboard.html:** Página que presenta gráficos y tablas con el análisis de las No Conformidades.

* **404.html:** Página de error mostrada cuando una ruta no es encontrada.

## Tecnologías Utilizadas
### Frontend:

* HTML5

* CSS3 (con Bootstrap 5 para el framework UI)

* JavaScript ES6+

* Bootstrap Icons

* Chart.js (para los gráficos del dashboard)

* Backend (Base de Datos como Servicio):

* Firebase Firestore (para almacenar los datos de usuarios y pines).

## Configuración y Ejecución
**Para ejecutar esta aplicación localmente y conectarla a tu propia base de datos, sigue los siguientes pasos:**

* **1. Configuración de Firebase**
Necesitarás una cuenta de Google y acceso a la consola de Firebase.

Crea un Proyecto Firebase:

Dirígete a la Consola de Firebase.

Haz clic en "Agregar proyecto" y sigue las instrucciones para crear uno nuevo.

Configura Firestore Database:

Dentro de tu nuevo proyecto, en el menú lateral, selecciona "Firestore Database".

Haz clic en "Crear base de datos".

Selecciona un modo de inicio (para desarrollo puedes empezar en "Modo de prueba", pero para producción deberías usar "Modo de producción" con reglas de seguridad adecuadas).

Elige la ubicación de tu base de datos y haz clic en "Habilitar".

Crea dos colecciones principales:

users: Para almacenar la información de los usuarios (username, password, role).

pines: Para almacenar los datos de las no conformidades (título, descripción, coordenadas, etc.).

Obtén la Configuración de tu Aplicación Web:

En la visión general de tu proyecto de Firebase, haz clic en el icono &lt;/&gt; para añadir una aplicación web a tu proyecto.

Sigue los pasos y, al final, se te proporcionará un objeto firebaseConfig con tus credenciales. Cópialo.

* **2. Actualiza las Credenciales en el Código**
Abre los archivos login.html, index.html, user_management.html y dashboard.html en tu editor de código. En cada uno de ellos, busca la sección donde se define const firebaseConfig y reemplaza los valores existentes con los que obtuviste de tu proyecto Firebase:
```bash
const firebaseConfig = {
  apiKey: &quot;TU_API_KEY&quot;,
  authDomain: &quot;TU_AUTH_DOMAIN&quot;,
  projectId: &quot;TU_PROJECT_ID&quot;,
  storageBucket: &quot;TU_STORAGE_BUCKET&quot;,
  messagingSenderId: &quot;TU_MESSAGING_SENDER_ID&quot;,
  appId: &quot;TU_APP_ID&quot;
};
```
* **3. Despliegue Local**
Aunque puedes abrir los archivos HTML directamente en tu navegador para una prueba rápida, para un funcionamiento óptimo y para evitar posibles problemas de seguridad o CORS relacionados con Firebase, es altamente recomendable usar un servidor web local.

Puedes usar la extensión "Live Server" en Visual Studio Code o el paquete http-server de Node.js:

Instalar http-server (si no lo tienes):

```bash
npm install -g http-server
```
Navega a la Carpeta del Proyecto: Abre tu terminal o línea de comandos y navega hasta el directorio donde se encuentran tus archivos HTML:

```bash
cd /ruta/a/tu/proyecto/NC_por_Patios
```

Inicia el Servidor:

```bash
http-server
```

Esto iniciará un servidor en http://localhost:8080 (o un puerto similar). Abre esta URL en tu navegador.

* **4. Creación de Usuarios Iniciales**
Dado que la gestión de usuarios está controlada por roles, para poder usar la funcionalidad de gestión de usuarios (user_management.html), necesitarás crear al menos un usuario con el rol superadmin directamente en tu consola de Firebase Firestore.

## Pasos:

* En tu consola de Firebase, ve a "Firestore Database".

* Selecciona la colección users.

* Haz clic en "Agregar documento".

* Crea un nuevo documento con los siguientes campos (asegúrate de que los nombres de los campos coincidan exactamente):

username (string): superadmin (o el nombre de usuario que prefieras)

password (string): password123 (o una contraseña simple. **¡ADVERTENCIA: ESTO ES ALTAMENTE INSEGURO PARA PRODUCCIÓN! Ver sección de seguridad.)**

role (string): superadmin

* Una vez que hayas iniciado sesión con este usuario superadmin, podrás usar la interfaz de user_management.html para crear otros usuarios con roles de admin o visualizador.

## Consideraciones de Seguridad (¡Muy Importante!)
La aplicación, en su estado actual, almacena y compara contraseñas en texto plano directamente en Firebase Firestore. Esto es EXTREMADAMENTE INSEGURO y NO DEBE USARSE en un entorno de producción real. Una brecha de seguridad expondría todas las contraseñas de los usuarios.

Para una aplicación segura y lista para producción, se DEBEN implementar las siguientes mejoras de seguridad:

Hashing de Contraseñas: En lugar de almacenar contraseñas en texto plano, utiliza una función de hashing fuerte (como bcrypt o Argon2) para almacenar versiones hasheadas de las contraseñas. Compara el hash de la contraseña ingresada por el usuario con el hash almacenado, no con la contraseña en texto plano.

Autenticación de Firebase: Utiliza los métodos de autenticación nativos de Firebase (por ejemplo, correo electrónico/contraseña, autenticación de Google) que manejan el hashing de contraseñas, la gestión de sesiones y la seguridad de forma robusta.

Reglas de Seguridad de Firestore: Define reglas estrictas en Firebase Firestore para controlar el acceso a los datos. Asegúrate de que los usuarios solo puedan leer o escribir los datos que les corresponden según su rol, y que nadie pueda leer la colección users directamente. Por ejemplo, solo un superadmin debería poder escribir en users.

## Uso

* Iniciar Sesión: Accede a la página login.html e ingresa tus credenciales de usuario.

**Mapa Principal (index.html):**

* Selecciona el mapa que deseas visualizar del menú desplegable.

* Si tienes el rol de Administrador, haz clic en cualquier parte del mapa para añadir un nuevo pin de no conformidad, o haz clic en un pin existente para editarlo o eliminarlo.

* Utiliza el filtro por "Encargado" para ver solo los pines asignados a una persona específica.

* Gestión de Usuarios (user_management.html):

**Esta página es solo accesible para usuarios con el rol superadmin.**

* Aquí puedes añadir nuevos usuarios, especificar su nombre de usuario, contraseña y rol (visualizador, administrador, superadmin), y eliminar usuarios existentes (con la precaución de no eliminarte a ti mismo o a otros superadministradores accidentalmente).

* Dashboard de Análisis (dashboard.html):

* **Accede a esta página para ver un resumen visual de las no conformidades.**

* **Los gráficos muestran la distribución de NC por mapa, encargado, tipo y estatus.**

* **Puedes aplicar filtros para ajustar los datos visualizados.**

* **La tabla inferior lista todas las no conformidades, y puedes descargar los datos filtrados en formato CSV.**

**Autor:** [Luis David Espinoza @perreohipertenso]


**USO LIBRE**
