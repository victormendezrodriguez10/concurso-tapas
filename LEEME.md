# Concurso de tapas en Vinateros

Web para el concurso del sábado 26 de septiembre de 2026 en Calle Vinateros, 2 (Arganda del Rey).

Todo está en `index.html`. Funciona de dos maneras:

- **Modo local** (tal como está ahora): los datos se guardan solo en el navegador donde se abre.
  Sirve para probarla y también para hacer el concurso con un único móvil que se va pasando
  (cada persona se inscribe y recupera su perfil con "Recupera tu inscripción" y su PIN).
- **Modo nube** (recomendado para que cada amigo use su móvil): hace falta un proyecto gratuito de
  Firebase. Son unos 10 minutos. Pasos abajo.

## 1. Cambiar textos y código de anfitrión

Al principio del `<script>` de `index.html` está el bloque `EVENTO`:

- `hora`: pon por ejemplo `"a partir de las 14:00"` si quieres que salga.
- `codigoAnfitrion`: el código para entrar en el panel del anfitrión (pestaña Resultados, abajo).
  Ahora es `VINATEROS1998*`.

## 2. Crear la base de datos en Firebase (modo nube)

> Hecho el 15/09/2026: proyecto `concurso-de-tapas-9b176`, con la configuración ya pegada en
> `index.html`. Los pasos quedan por si hay que repetirlo en otro proyecto.

1. Entra en https://console.firebase.google.com con tu cuenta de Google y pulsa **Crear proyecto**.
   Nombre: `tapas-vinateros` (o el que quieras). Puedes desactivar Google Analytics.
2. En el menú de la izquierda: **Compilación → Firestore Database → Crear base de datos**.
   Ubicación: `eur3 (europe-west)`. Modo: **Empezar en modo de prueba**.
3. Ve a la pestaña **Reglas** de Firestore, sustituye lo que hay por esto y pulsa **Publicar**
   (el modo de prueba caduca a los 30 días; con estas reglas no caduca):

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```

   Es una base de datos abierta: cualquiera con el enlace puede escribir. Para un concurso entre
   amigos es suficiente. Cuando acabe el concurso puedes borrar el proyecto.
4. Pulsa el icono de la rueda dentada (arriba a la izquierda) → **Configuración del proyecto** →
   baja hasta **Tus apps** → pulsa el icono `</>` (web). Ponle un apodo, no marques Hosting y pulsa
   **Registrar app**. Te mostrará un bloque `const firebaseConfig = { ... }`.
5. Copia ese objeto y pégalo en `index.html` sustituyendo la línea:

   ```js
   const FIREBASE_CONFIG = null;
   ```

   por

   ```js
   const FIREBASE_CONFIG = { apiKey: "...", authDomain: "...", projectId: "...", ... };
   ```

Al abrir la página verás arriba a la derecha el punto verde con "Conectado".

## 3. Publicar la web (GitHub Pages)

Igual que con la invitación de boda:

1. Crea un repositorio nuevo en GitHub (por ejemplo `concurso-tapas`) y sube `index.html`.
2. En el repositorio: **Settings → Pages → Branch: main / (root) → Save**.
3. En un minuto tendrás la web en `https://TUUSUARIO.github.io/concurso-tapas/`.

Manda ese enlace al grupo. El botón "Copiar enlace para invitar" de la pestaña Normas lo copia
con un mensajito.

## 4. Las tres fases (solo tú las cambias)

El concurso pasa por tres fases. Se cambian desde el **panel del anfitrión** (pestaña Resultados,
abajo del todo, con tu código) y el cambio llega al momento a todos los móviles.

1. **Inscripción** (desde ya hasta el miércoles 23 a las 23:59; después la web cierra sola la inscripción). Cada persona abre el enlace, entra en **Tapas** y se
   inscribe con nombre, nombre del plato, presentación, foto y un PIN de 4 cifras. Las tapas de los
   demás no se ven: solo cuántas hay. Cada uno puede editar la suya hasta que empiece el concurso.
2. **Cata**. El día 26 pulsas **Empezar el concurso**: se cierran las inscripciones, se revelan las
   tapas (sin decir de quién es cada una) y se abre la ficha de **Votar**. Cada persona puntúa cada
   tapa menos la suya. En Resultados solo se ve quién ha votado ya y a quién le falta.
3. **Resultados**. Cuando todos han votado pulsas **Publicar resultados** y todo el mundo ve la tapa
   ganadora, la clasificación con medias por criterio y la mejor de cada categoría. Si alguien no va
   a votar, el botón te avisa de cuántos faltan y puedes publicar igualmente tocándolo dos veces.

Con **Volver a la fase anterior** puedes deshacer un paso si te has equivocado. También puedes
eliminar inscripciones erróneas o reiniciar todo el concurso.

Las notas individuales nunca se muestran: solo medias. Los nombres de quién ha hecho cada tapa
solo aparecen cuando se publican los resultados.
