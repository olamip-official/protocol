# Proceso de lanzamiento

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Este documento define el proceso estándar que utilizo para crear, probar y publicar lanzamientos de la especificación OLAMIP y herramientas relacionadas. Por ahora, todos los lanzamientos son gestionados y propiedad mía.

## 1. Versionado

Sigo [Semantic Versioning 2.0.0](https://semver.org/):

- **MAJOR** cuando realizo cambios incompatibles en el formato o protocolo.
- **MINOR** cuando añado nueva funcionalidad o secciones de forma compatible con versiones anteriores.
- **PATCH** cuando corrijo errores, mejoro redacción o aclaro reglas sin cambiar comportamiento.

Ejemplo: `1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

## 2. Modelo de ramificación

Utilizo un flujo de trabajo Git simple:

- `main` — la rama estable, siempre lista para lanzamiento.
- Ramas de características o correcciones (ej. `spec/delta-formats`, `fix/tag-rules`) — ramas de corta duración para cambios.
- Ramas de lanzamiento (ej. `release/v1.1.0`) — creadas justo antes de publicar un lanzamiento.

## 3. Requisitos previos para un lanzamiento

Antes de comenzar un lanzamiento, me aseguro de:

- Todos los cambios planeados para este lanzamiento están fusionados en `main`.
- Todas las verificaciones automatizadas (linting, validación de esquema, scripts de compilación) pasan.
- Todas las incidencias y notas que pertenecen a este lanzamiento están cerradas o documentadas.
- `CHANGELOG.md` está actualizado con entradas claras y legibles para esta versión.
- La documentación está actualizada, incluyendo:
  - `file-format-specification.md`
  - `olamip-delta-file-format-specification.md`
  - `docs/introduction.md`
  - `faq.md`
- Los archivos de ejemplo `olamip.json` y `olamip-delta.json` aún validan contra las reglas más recientes.

## 4. Crear un candidato de lanzamiento

1. Crear una rama de lanzamiento desde `main`:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Actualizar números de versión (si se usan en herramientas, scripts o activos etiquetados, no solo en texto de especificación).
3. Actualizar `CHANGELOG.md` con un encabezado para el lanzamiento y notas para esta versión.
4. Confirmar los cambios:

   ```bash
   git add .
   git commit -m "chore(release): preparar v1.1.0"
   ```

5. Enviar la rama:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Ejecutar validación local o CI nuevamente y asegurar que todo pase.

## 5. Probar el candidato de lanzamiento

Dado que solo una persona gestiona los lanzamientos, mi prueba personal es la principal puerta de control:

- Valido que los archivos de ejemplo `olamip.json` y `olamip-delta.json` aún se ajusten al esquema.
- Verifico que todos los ejemplos en los dos archivos de especificación aún coincidan con las reglas descritas.
- Leo las secciones actualizadas en la especificación y documentación para detectar inconsistencias.

Si encuentro problemas significativos:

- Los corrijo en la rama `release`.
- Actualizo `CHANGELOG.md` y confirmo nuevamente.
- Revalido antes de continuar.

## 6. Finalizar el lanzamiento

Cuando el candidato está estable:

1. Etiquetar el lanzamiento en Git:

   ```bash
   git tag -a vX.Y.Z -m "Lanzamiento v1.1.0"
   git push origin vX.Y.Z
   ```

2. Crear un lanzamiento en GitHub (o equivalente):

   - Usar la etiqueta `vX.Y.Z`.
   - Pegar la sección relevante de `CHANGELOG.md` en la descripción del lanzamiento.
   - Adjuntar artefactos generados (ej. archivos de esquema, herramientas CLI, tarballs de instantáneas) si corresponde.

## 7. Pasos posteriores al lanzamiento

Después de que el lanzamiento se publique:

- Actualizo notas internas o hojas de ruta sobre lo que se ha enviado.
- Muevo elementos diferidos a un backlog o notas para el próximo lanzamiento.
- Si existe un canal de comunicación (ej. Twitter, boletín, Discusiones de GitHub), opcionalmente anuncio la nueva versión y resalto cambios.

## 8. Gobernanza y supervisión

Incluso con un solo mantenedor, mantengo una capa de gobernanza:

- Cualquier cambio que afecte el esquema principal, campos requeridos o versión del protocolo debe documentarse claramente en el changelog.
- Cambios significativos en el propio proceso de lanzamiento (ej. cambio a un nuevo esquema de versionado o modelo de ramificación) deben documentarse en una breve nota de gobernanza o registro de decisiones en `governance/decision-records/`.

## 9. Retroceso o parches

Si un lanzamiento contiene un error crítico:

- Creo un lanzamiento de parche lo antes posible (ej. `v1.1.1`).
- Documento claramente la corrección en `CHANGELOG.md`.
- Si es necesario, actualizo documentación o canales de comunicación para notar que la versión anterior está deprecada o desaconsejada.

## 10. Automatización y escalabilidad futura

Siempre que sea posible, automatizo:

- Pasos repetitivos como:
  - Verificar validez JSON.
  - Validar archivos de ejemplo contra el esquema.
  - Generar o limpiar texto estilo changelog.
- Mantengo el proceso de lanzamiento simple para que, si más tarde se unen colaboradores, pueda transferir o compartir los mismos pasos.

Este proceso de lanzamiento es propiedad y mantenimiento mío y puede actualizarse mediante el flujo de trabajo normal cuando mejore el proceso mismo.
