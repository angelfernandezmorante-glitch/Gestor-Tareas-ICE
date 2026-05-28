# MVP: Intelligent Task Manager con ICE

## Objetivo
Crear un MVP educativo de React que permita gestionar tareas simples y calcular un puntaje ICE inteligente usando una API de IA.

## Funcionalidades clave
- Crear tareas con título y descripción.
- Mostrar lista de tareas.
- Marcar tareas completadas.
- Eliminar tareas.
- Calcular ICE automático en el frontend.

## Reglas del MVP
- Sin backend ni base de datos real.
- Todos los datos se mantienen en memoria.
- No hay persistencia entre recargas.
- La IA se consulta desde el navegador.
- Las claves API se usan solo para desarrollo/muestra.

## Cálculo ICE
- Impacto: 1-10.
- Confianza: 1-10.
- Esfuerzo: 1-10.
- Fórmula: ICE = (Impacto + Confianza) / Esfuerzo.
- Si la IA devuelve valores fuera de rango, se ajustan a [1, 10].
- Si Esfuerzo es 0, se usa 1 para evitar división por cero.

## API de IA recomendada
- Preferido: Google Gemini.
- Alternativa: Hugging Face.
- OpenAI solo si hay presupuesto.
- La llamada debe retornar JSON válido.

## Prompt recomendado
Eres un experto en priorización de tareas usando ICE.

Dada esta tarea:
"[DESCRIPCIÓN_DE_LA_TAREA]"

Responde solo el JSON siguiente:
{
  "impacto": [entero 1-10],
  "confianza": [entero 1-10],
  "esfuerzo": [entero 1-10]
}

## Exclusiones
No incluye:
- Backend.
- Autenticación.
- Persistencia en servidor.
- Persistencia en localStorage.
- Multiusuario.
- Etiquetas o filtros avanzados.
- Reintentos automáticos en la API.

## Flujo básico
1. El usuario abre la app.
2. Completa título y descripción.
3. Se valida localmente.
4. Se consulta la IA.
5. Se calcula ICE.
6. Se muestra la tarea en la lista.
7. Se puede completar o eliminar.

## Criterios de aceptación
- ✅ Crear tareas con título y descripción.
- ✅ Consumir API de IA al crear tareas.
- ✅ Mostrar puntaje ICE en cada tarea.
- ✅ Completar y eliminar tareas.
- ✅ Validar longitudes de título y descripción.
- ✅ Manejar errores de API con mensaje claro.

## Estructura sugerida
- `TaskForm`: formulario de creación.
- `TaskList`: lista de tareas.
- `TaskCard`: tarjeta de tarea.
- `ICEScore`: visualización del puntaje.
- `TaskContext`: estado global.

## Notas de diseño
- UI simple y móvil primero.
- Indicador de carga al llamar IA.
- Mensajes claros de éxito y error.
- Colores para estado completado y pendiente.
