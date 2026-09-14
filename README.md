# App Aprobadores — Financiera Confianza

Este repositorio es documentación, no código fuente.

App Aprobadores es propiedad de Financiera Confianza. El código fuente es confidencial y no puede publicarse. Este repositorio  documenta el proyecto, mi contribución y capturas de pantalla — no contiene ni contendrá archivos de código de la app real.

## Descripción

App Android nativa (Java) usada internamente por analistas de Financiera Confianza para autorizar operaciones crediticias sobre el core bancario **Bantotal**, vía procedures **GeneXus** expuestos como servicios SOAP/XML. La app permite revisar bandejas de operaciones pendientes y confirmar o rechazar cada una, con observaciones.

## Demo

![Flujo de autorización](screenshots/demo.gif)

*Bandeja de Tasas Pasivas → detalle de la operación → confirmación con contraseña → validación → resultado. Datos de la captura ficticios (`CLIENTE GENERICO`, `ASESOR DEFAULT`).*

## Mi contribución

**Backend / Integración**
- Diseño e implementación (jul. 2022) de la capa de comunicación (Controller) para la confirmación/autorización de tres flujos: tasas pasivas, retiros y operaciones de clientes migrantes.
- Armado manual de XML según el protocolo propietario **BT Services** de Bantotal (canal, servicio, campos `CODAUTORIZA`/`PORDEN`/`TSAPROB`/`PRESULT`/`OBSERV`).
- Generación de una trama de seguridad (hash) por request, siguiendo el algoritmo esperado por el backend.
- Consumo vía `HttpsURLConnection`, con manejo de reconexión de sesión (re-login automático) y verificación de conectividad/certificate pinning antes de cada llamada.
- Integración de esta lógica dentro de los `AsyncTask` ya existentes en la app (código de 2017), sin alterar la arquitectura ni los flujos que ya funcionaban en producción.
- Diseño e implementación de los Procedures GeneXus correspondientes, expuestos como servicios SOAP sobre Bantotal.

**Frontend / UI-UX**
- Consumo de esos mismos servicios SOAP desde el cliente Android (mismo patrón: `HttpsURLConnection` + XML armado/parseado a mano).
- Rediseño del shell de navegación principal de la app (`BottomNavigationView` de Material Components), reemplazando el menú lateral (drawer) anterior.
- Integración de librerías de terceros: **Lottie** (animaciones de estado), **Animatoo** (transiciones entre pantallas), **PhotoView** (zoom de imágenes/documentos).
- Desarrollo de pantallas modales de confirmación para mis tres flujos.
- Actualización visual de las pantallas de mis módulos al nuevo lenguaje visual.

## Lo que no es mío (para que quede claro)

- La bandeja de consulta/listado de operaciones (UI y backend) de estos mismos módulos: código preexistente de 2017.
- El módulo de Créditos de la app: no participé en su desarrollo.
- El resto de módulos/pantallas de la app no mencionados arriba.

## Stack completo de la app (toda la app, no solo mi parte)

**Base**
Java (0% Kotlin) · Android SDK `minSdk 19` / `target-compileSdk 33` · Gradle (AGP 7.0.4)

**UI**
AndroidX (`appcompat`, `constraintlayout`, `cardview`, `recyclerview`) · Material Components · ViewBinding · Lottie · Animatoo · PhotoView · RoundCornerProgressBar

**Red / integración**
`HttpsURLConnection` + XML manual (`XmlSerializer`/`XmlPullParser`) — patrón dominante · Retrofit2 + `converter-simplexml` (solo módulo PDM) · OkHttp + Conscrypt · Certificate pinning (inconsistente entre módulos)

**Persistencia local**
SQLite vía `SQLiteOpenHelper` (solo módulo PDM)

**Documentos**
`android-pdf-viewer`, `commons-io`

**Seguridad**
SHA-512 (hash de PIN local en `SharedPreferences`)

**Build/firma**
ProGuard (minify release) · firma con keystore propio

---

*Para más detalle técnico o para revisar código, con gusto lo explico en una entrevista — solo no puedo compartir el repositorio real por confidencialidad del cliente.*
