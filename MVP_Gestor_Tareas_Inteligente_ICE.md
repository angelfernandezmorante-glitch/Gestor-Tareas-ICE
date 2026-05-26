# Alcance funcional del MVP: Gestor de Tareas Inteligente (ToDo) con modelo ICE

## Objetivo
Crear una aplicación React simple y didáctica que permita gestionar tareas básicas y calcular un puntaje ICE (Impacto, Confianza, Esfuerzo) automático a partir de la descripción de la tarea mediante una API de IA gratuita.

## Características principales del MVP

### 1. Gestión básica de tareas
- Crear tareas con:
  - Título (requerido, 1-100 caracteres)
  - Descripción (requerida, 10-500 caracteres)
  - ~~Prioridad opcional~~ (REMOVIDO: no impacta en alcance MVP)
- Listar tareas en una vista simple.
- Marcar tareas como completadas.
- Eliminar tareas.

### 2. Cálculo ICE inteligente
- Cada tarea muestra:
  - Impacto (1-10)
  - Confianza (1-10)
  - Esfuerzo (1-10)
  - **Puntaje ICE = (Impacto + Confianza) / Esfuerzo** (rango: 0.2-20)
- Se calcula mediante una llamada a una API de IA usando la **descripción**.
- Los valores se validan dentro del rango 1-10 antes de calcular.
- Si la IA devuelve valores fuera de rango, se clampean a [1,10].

### 3. Interfaz sin backend
- La aplicación usa solo React y ejecuta toda la lógica en el navegador.
- No hay servidor ni base de datos real.
- **Los datos se almacenan EN MEMORIA durante la sesión** (se pierden al recargar la página).
- **⚠️ ADVERTENCIA**: `localStorage` NO está incluido para mantener MVP simple y educativo.

### 4. Uso de API de IA gratuita
- Integración con una API pública o gratuita de IA para obtener la valoración ICE.
- La llamada se realiza desde el frontend.
- **⚠️ ADVERTENCIA EDUCATIVA**: Las claves API se almacenan en código/env variables del navegador (NO para producción).
- **Requisito**: Conectividad a Internet permanente para funcionar.
### APIs recomendadas (opciones gratuitas)

#### 1. Google Gemini API (RECOMENDADO - Más confiable)
- **Ventajas**: Modelo potente, tier gratuito estable, excelente para JSON.
- **Límite**: 60 solicitudes/minuto (suficiente para 1 usuario en curso).
- **Autenticación**: Clave API gratuita desde https://makersuite.google.com/app/apikey
- **Endpoint**: `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent`
- **Ventaja sobre alternativas**: Respuestas JSON más consistentes, menor latencia.
- **Ejemplo de llamada**:
  ```javascript
  const response = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=${GEMINI_API_KEY}`,
    {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        contents: [{
          parts: [{ text: "Tu prompt aquí" }]
        }]
      }),
    }
  );
  const data = await response.json();
  // Respuesta: data.candidates[0].content.parts[0].text
  ```


#### 2. Hugging Face Inference API (Alternativa)
- **Ventajas**: Gratuita, modelos variados.
- **Desventaja**: A veces hay queue/espera, latencia variable.
- **Autenticación**: Token gratuito desde huggingface.co
- **Modelo sugerido**: `mistralai/Mistral-7B-Instruct-v0.1`

#### 3. OpenAI API (NO RECOMENDADO para MVP)
- **Ventajas**: Modelo muy potente.
- **Desventaja**: Créditos gratuitos se agotan rápido (~$5).
- **Para el curso**: Solo si hay presupuesto dedicado.

### Prompt recomendado para evaluar ICE
```
Eres un experto en priorización de tareas usando el modelo ICE.

Dada esta tarea:
"[DESCRIPCIÓN DE LA TAREA]"

Evalúa y responde EXACTAMENTE en este formato JSON (solo el JSON, sin texto adicional):
{
  "impacto": [número entero 1-10],
  "confianza": [número entero 1-10],
  "esfuerzo": [número entero 1-10]
}

Definiciones:
- Impacto: ¿Cuánto beneficio generaría completar esto? (1=ninguno, 10=transformador)
- Confianza: ¿Qué tan seguro estás de alcanzar este beneficio? (1=muy dudoso, 10=muy seguro)
- Esfuerzo: ¿Cuánto trabajo requiere? (1=trivial, 10=enorme)
```
## Exclusiones claras (Fuera de alcance MVP)
No incluye:
- ❌ Backend.
- ❌ Autenticación de usuarios.
- ❌ Persistencia en servidor o base de datos.
- ❌ Persistencia en localStorage (solo memoria).
- ❌ Paginación de tareas.
- ❌ Multiusuario.
- ❌ Etiquetas (tags).
- ❌ Prioridad en tareas (REMOVIDO por simplicidad).
- ❌ Reintentos automáticos en fallos de API.
- ❌ Barra de progreso visual para ICE (solo mostrar número).

## Flujo de uso del MVP
1. El usuario abre la app React.
2. Crea una nueva tarea con título y descripción.
3. La app envía la descripción a la API de IA gratuita.
4. La API devuelve valores para Impacto, Confianza y Esfuerzo.
5. La app muestra el puntaje ICE calculado junto con la tarea.
6. El usuario puede marcar la tarea como completada o eliminarla.

## Requisitos de desarrollo
- Framework: React (sin backend).
- Enfoque: sencillo y apropiado para curso de React.
- UI: formulario de tarea + lista de tareas + tarjeta de detalles ICE.
- API de IA: Google Gemini (recomendado), accesible desde el frontend.
- **Validaciones**:
  - Título: requerido, 1-100 caracteres, no vacío.
  - Descripción: requerida, 10-500 caracteres (mínimo 10 para análisis significativo).
  - Valores ICE: validados en rango [1,10], si están fuera se clampean.
  - Mostrar error al usuario si validación falla.

## Arquitectura y estructura de componentes

### Componentes React principales
```
App.tsx
├── TaskForm.tsx (formulario para crear tareas)
├── TaskList.tsx (lista de tareas)
│   └── TaskCard.tsx (tarjeta individual de tarea)
└── ICEScore.tsx (visualización del puntaje ICE)
```

### Estado de la aplicación (Context o useState)
```javascript
{
  tasks: [
    {
      id: string,
      title: string,
      description: string,
      priority: "baja" | "media" | "alta",
      completed: boolean,
      iceScore: {
        impacto: number,
        confianza: number,
        esfuerzo: number,
        total: number,
        razonamiento: string
      },
      createdAt: Date
    }
  ]
}
```

### Flujo de datos
1. Usuario ingresa título y descripción en `TaskForm`.
2. Se valida que título y descripción no estén vacíos.
3. Se envía descripción a la API de IA elegida.
4. La API devuelve valores ICE.
5. Se calcula: `ICE = (Impacto + Confianza) / Esfuerzo`.
6. Se almacena la tarea en estado local.
7. `TaskList` renderiza las tareas con sus puntajes ICE.

## Criterios de aceptación
- ✅ El usuario puede crear una tarea con título y descripción.
- ✅ Al crear una tarea, la app consume automáticamente la API de IA.
- ✅ La API devuelve valores de Impacto, Confianza y Esfuerzo.
- ✅ Se muestra el puntaje ICE en la tarjeta de la tarea.
- ✅ El usuario puede marcar una tarea como completada.
- ✅ El usuario puede eliminar una tarea.
- ✅ La interfaz es clara, responsiva y educativa.
- ✅ No hay dependencia de backend.

## Consideraciones de desarrollo

### Manejo de errores
- Si API falla: mostrar mensaje "No se pudo obtener ICE. Intenta más tarde" y NO crear la tarea.
- Si validación falla: mostrar mensaje específico en el formulario (rojo).
- Timeout: máximo 15 segundos para llamada a API.
- Si JSON es inválido: mostrar error genérico "Respuesta inválida de API".
- Si valores están fuera de rango: clampear automáticamente a [1,10].

### Performance
- Deshabilitar botón "Crear" mientras se espera respuesta de API.
- Mostrar indicador de carga (spinner) durante llamada a API.
- Validar entrada localmente ANTES de enviar a API (evitar envíos innecesarios).

### Almacenamiento
- **Datos en memoria**: se pierden al F5 o cerrar pestaña (por diseño).
- **SIN localStorage**: mantener MVP simple y educativo.
- **Botón "Limpiar todas" opcional**: para que estudiante pueda resetear durante pruebas.

### UX/UI mínimo
- **Colores**: Verde para completadas, gris para pendientes.
- **ICE Visual**: Badge con número (0.2-20, mostrar 1 decimal).
- **Feedback**: Toast o mensaje bajo formulario ("Tarea creada" / "Error").
- **Responsivo**: Mobile-first, funciona en teléfono.

## Matriz de decisiones - Casos límite

| Escenario | Decisión |
|-----------|----------|
| **API devuelve JSON inválido** | Mostrar error genérico, NO crear tarea |
| **Impacto/Confianza/Esfuerzo = 0 o negativo** | Clampear a 1 |
| **Impacto/Confianza/Esfuerzo > 10** | Clampear a 10 |
| **Esfuerzo = 0** | Clampear a 1 (evita división por cero) |
| **Timeout > 15 segundos** | Cancelar y mostrar "Timeout - intenta de nuevo" |
| **Sin conexión a Internet** | Mostrar "Sin conexión" |
| **Descripción < 10 caracteres** | Bloquear con mensaje de validación |
| **Descripción > 500 caracteres** | Truncar a 500 o rechazar |
| **API Rate limit (60 req/min)** | Mostrar "Demasiadas solicitudes, espera un momento" |
| **Usuario recarga página** | Datos se pierden (por diseño - memoria, no localStorage) |
| **Usuario completa tarea y luego recarga** | Tarea desaparece (sin persistencia) |

## Decisiones arquitectónicas resueltas

### ✅ Persistencia: SOLO MEMORIA
- **Decisión**: Datos en memoria, se pierden al F5.
- **Por qué**: MVP educativo, evita complejidad de localStorage.
- **Impacto**: Estudiante aprende React sin dependencias de storage.

### ✅ Prioridad: REMOVIDA
- **Decisión**: No incluir prioridad (baja/media/alta).
- **Por qué**: ICE ya proporciona priorización, agregar más es alcance innecesario.
- **Impacto**: Interfaz más simple, menos campos.

### ✅ Reintentos: NO AUTOMÁTICOS
- **Decisión**: Si falla la API, mostrar error; sin reintentos automáticos.
- **Por qué**: Simplifica lógica, estudiante entiende flujo lineal.
- **Impacto**: UX más predecible, código más legible.

### ✅ Almacenamiento ICE: VALIDAR Y CLAMPEAR
- **Decisión**: Si valores fuera de rango → clampear a [1,10].
- **Por qué**: Evita errores matemáticos, garantiza ICE válido.
- **Impacto**: Nunca se divide por cero, siempre rango 0.2-20.

### ✅ API Principal: GOOGLE GEMINI
- **Decisión**: Google Gemini como recomendada, Hugging Face como alternativa.
- **Por qué**: Gemini es más confiable, JSON más consistente, menor latencia.
- **Impacto**: MVP tiene menos probabilidad de errores de parsing.

### ✅ Validación: CLIENTE + API
- **Decisión**: Validar localmente (longitud, formato) ANTES de enviar a API.
- **Por qué**: Evita llamadas API innecesarias, ahorrar cuota.
- **Impacto**: Mejor UX, menos fallos.

## Resultados esperados
- Un MVP funcional que demuestre:
  - creación y gestión de tareas
  - integración básica con IA para cálculo ICE
  - uso de React como herramienta principal
- Un producto simple y educativo, fácil de entender para estudiantes.

## Stack tecnológico recomendado
- **Framework**: React 18+
- **Lenguaje**: TypeScript (opcional pero recomendado)
- **Styling**: Tailwind CSS o CSS Modules (sencillo)
- **Gestión de estado**: useState + useContext (sin Redux)
- **Llamadas HTTP**: Fetch API (nativa)
- **Empaquetado**: Vite o Create React App

## Riesgos identificados y mitigación

| Riesgo | Impacto | Mitigación |
|--------|---------|-----------|
| **CORS bloqueado en API** | 🔴 Fatal | Usar Google Gemini (CORS-friendly) |
| **API Keys expuestas** | 🟠 Alto | Documentar que es MVP/educación, NO producción |
| **Rate limit superado (60 req/min)** | 🟠 Medio | Mensaje al usuario, esperar 1 minuto |
| **Latencia > 15 seg** | 🟡 Bajo | Timeout, reintentar con Google Gemini |
| **JSON inválido de IA** | 🟡 Bajo | Validación con try-catch, mensaje de error |
| **Valores fuera de rango [1-10]** | 🟢 Bajo | Clamping automático a rango válido |
| **Usuario recarga y pierde datos** | 🟢 Bajo | Esperado por diseño, sin localStorage |
| **Esfuerzo = 0 (división por cero)** | 🟢 Bajo | Clamping a mínimo 1 |

**Riesgo NO mitigado (aceptado)**: Sin persistencia entre sesiones → datos se pierden (POR DISEÑO para MVP educativo).
