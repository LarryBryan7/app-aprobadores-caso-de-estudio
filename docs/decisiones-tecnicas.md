# Decisiones técnicas

Decisiones que pueden derivarse razonablemente del contexto del proyecto. No incluye nada que no haya sido confirmado.

## Android nativo (Java) como plataforma cliente

La app se desarrolló en Android nativo con Java. No participé en la decisión original de la plataforma (la app ya existía cuando me incorporé a estos módulos), pero sí desarrollé mis tres flujos siguiendo ese mismo enfoque, por consistencia con el resto de la aplicación.

## SOAP como mecanismo de integración

El ecosistema GeneXus/Bantotal exponía sus Procedures como servicios web SOAP. Usar SOAP no fue una preferencia tecnológica personal, sino la forma de integración ya establecida por ese ecosistema — la decisión relevante de mi parte fue diseñar los Procedures y el consumo del lado Android de forma consistente con ese estándar.

## `BottomNavigationView` para la navegación principal

La app usaba originalmente un menú lateral (drawer). Propuse e implementé el cambio a `BottomNavigationView` (Material Components) porque:
- Da acceso directo a las categorías principales (Autorizaciones, Operaciones, Protocolos PDM, Excepciones) sin un paso intermedio de abrir el drawer.
- Es un patrón de navegación más alineado a Material Design para aplicaciones con un número reducido de secciones de primer nivel.

## Lottie para estados de carga/confirmación

Se usó Lottie para comunicar visualmente los estados de "procesando" y "confirmado" durante el envío de una autorización — una operación que involucra una llamada de red hacia un sistema externo (Bantotal), donde es importante que el analista perciba que la acción está en curso y no piense que la app se congeló.

## PhotoView para documentos/imágenes

Las operaciones podían tener imágenes o documentos asociados. Se integró PhotoView para permitir al analista hacer zoom sobre esas imágenes antes de tomar la decisión de autorizar o rechazar — relevante quando el detalle visual importa para validar la operación.
