# UX/UI Design Component Flows

## Resumen
El MVP es una SPA que permite crear tareas y ver su puntaje ICE. El diseño debe ser simple, educativo y móvil primero.

## Flujo de usuario
1. El usuario entra a la app.
2. Completa título y descripción.
3. Presiona crear tarea.
4. Aparece carga mientras la IA procesa.
5. La tarea se agrega a la lista.
6. El usuario puede completar o eliminar la tarea.

## Pantallas y secciones
- Barra superior con título y ayuda.
- Sección de formulario.
- Sección de lista de tareas.
- Mensajes de estado y errores.

## Componentes clave
- `AppBar`: encabezado.
- `TaskForm`: entrada de datos.
- `TaskList`: lista de `TaskCard`.
- `TaskCard`: estado, ICE y acciones.
- `ICEScore`: visualización de métricas.
- `Snackbar`: notificaciones.

## Estado visual
- Tarea pendiente: fondo blanco.
- Tarea completada: gris y tachado.
- ICE alto: verde.
- ICE medio: amarillo.
- ICE bajo: rojo.

## Interacciones principales
- Botón crear deshabilitado con errores.
- Spinner mientras se consulta IA.
- Mensajes claros si hay fallo de API.
- Actualización inmediata de la lista.

## Diseño de formulario
- `TextField` para título.
- `TextField` multiline para descripción.
- Validación en tiempo real.
- Botón primario para crear.

## Diseño de lista
- Mostrar conteo de tareas.
- Ordenar por puntaje ICE descendente.
- Incluir botones `Completar` y `Eliminar`.

## Respuesta de errores
- Mostrar alerta cuando falla la API.
- No crear tareas inválidas.
- Explicar al usuario si la descripción es muy corta.

## Nota de accesibilidad
- Usar contrastes suficientes.
- Etiquetas claras en campos.
- Mensajes legibles.
- Feedback visible en pantalla.
