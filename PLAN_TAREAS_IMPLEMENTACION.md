# 📋 Plan de Tareas de Implementación
## MVP: Gestor de Tareas Inteligente con Modelo ICE

**Proyecto**: Task Manager con evaluación automática ICE  
**Versión**: 1.0  
**Fecha**: Mayo 2026  
**Duración estimada total**: 40-50 horas  

---

## Resumen Ejecutivo

El proyecto se divide en **8 tareas secuenciales** que siguen un flujo lógico:
1. Preparación del entorno
2. Definición de tipos y servicios
3. Gestión de estado
4. Componentes de infraestructura
5. Componentes principales (UI)
6. Integración con IA
7. Testing y validación
8. Deployment y documentación

---

## 📌 TAREA 1: Setup y Configuración Inicial

**Descripción**: Crear la estructura base del proyecto React con TypeScript, Material UI, ESLint, Prettier y todas las dependencias necesarias.

**Duración estimada**: 3-4 horas

**Dependencias**: Ninguna (tarea inicial)

**Archivos a crear/modificar**:
```
├── package.json
├── tsconfig.json
├── vite.config.ts
├── .eslintrc.cjs
├── .prettierrc
├── .env
├── .env.example
├── .gitignore
├── src/
│   ├── main.tsx
│   ├── index.css
│   ├── vite-env.d.ts
│   └── App.tsx (stub)
└── public/
    └── index.html
```

**Actividades**:
- [ ] Crear proyecto con `npm create vite@latest . -- --template react-ts`
- [ ] Instalar dependencias de React y Material UI
- [ ] Instalar y configurar ESLint para TypeScript + React
- [ ] Instalar y configurar Prettier
- [ ] Configurar `tsconfig.json` en modo estricto
- [ ] Crear `.env` con placeholders para API Key
- [ ] Crear carpeta de estructura base (`src/types/`, `src/services/`, etc.)
- [ ] Configurar vite.config.ts (si es necesario)
- [ ] Crear `.gitignore` apropiado
- [ ] Verificar que `npm run dev` funciona sin errores

**Criterios de aceptación**:
- ✅ Proyecto arranca sin errores en `npm run dev`
- ✅ TypeScript compila sin warnings en modo estricto
- ✅ ESLint y Prettier funcionan correctamente
- ✅ Estructura de carpetas lista para siguiente tarea
- ✅ Console sin errores ni warnings

**Comandos**:
```bash
npm create vite@latest . -- --template react-ts
npm install
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
npm install -D eslint prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser
npm run dev
```

---

## 📌 TAREA 2: Tipos, Interfaces y Servicios Base

**Descripción**: Definir todas las interfaces TypeScript, tipos de datos y servicios utilitarios que servirán de base para todo el proyecto.

**Duración estimada**: 4-5 horas

**Dependencias**: Tarea 1 ✓

**Archivos a crear**:
```
src/
├── types/
│   ├── task.types.ts
│   ├── ice.types.ts
│   └── api.types.ts
├── utils/
│   ├── constants.ts
│   ├── iceCalculator.ts
│   ├── stringValidator.ts
│   ├── errorHandler.ts
│   └── idGenerator.ts
└── services/
    └── apiConfig.ts
```

**Contenido detallado**:

### `types/task.types.ts`
```typescript
export interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  iceScore: ICEScore;
  createdAt: Date;
}

export type TaskInput = Omit<Task, 'id' | 'createdAt'>;
```

### `types/ice.types.ts`
```typescript
export interface ICEScore {
  impacto: number;        // 1-10
  confianza: number;      // 1-10
  esfuerzo: number;       // 1-10
  total: number;          // (impacto + confianza) / esfuerzo
}
```

### `types/api.types.ts`
```typescript
export interface GeminiResponse {
  candidates: Array<{
    content: {
      parts: Array<{ text: string }>;
    };
  }>;
}

export interface APIError {
  code: string;
  message: string;
  recoverable: boolean;
}
```

### `utils/constants.ts`
- `VALIDATION_RULES` (longitudes, rangos)
- `ICE_RANGES` (min, max para cada valor)
- `API_TIMEOUT` (ms)
- `MAX_RETRIES` (intentos)
- `ERROR_MESSAGES` (mensajes estandarizados)

### `utils/iceCalculator.ts`
- `calculateICEScore(impacto, confianza, esfuerzo): number`
- `formatICEScore(total): string` (con decimales y formato)

### `utils/stringValidator.ts`
- `validateTitle(title): ValidationResult`
- `validateDescription(description): ValidationResult`
- `validateICEValues(impacto, confianza, esfuerzo): ValidationResult`

### `utils/errorHandler.ts`
```typescript
export class ErrorHandler {
  static handleAPIError(error: unknown): AppError
  static isRetryable(error: AppError): boolean
  static formatErrorMessage(error: AppError): string
}
```

### `utils/idGenerator.ts`
- `generateId(): string` (UUID o similar)
- `generateTimestamp(): string`

### `services/apiConfig.ts`
```typescript
export const API_CONFIG = {
  GEMINI_API_KEY: import.meta.env.VITE_GEMINI_API_KEY,
  TIMEOUT: 15000,
  MAX_RETRIES: 3,
  ENDPOINT: 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent',
}
```

**Actividades**:
- [ ] Crear todas las interfaces en `types/`
- [ ] Implementar funciones utilitarias en `utils/`
- [ ] Crear servicios base en `services/`
- [ ] Documentar cada tipo y función
- [ ] Crear tests unitarios para validadores y calculadores
- [ ] Verificar que no hay errores TypeScript

**Criterios de aceptación**:
- ✅ Todos los tipos compilados sin errores
- ✅ Funciones puras testadas
- ✅ No hay `any` en TypeScript
- ✅ Validadores cubren todos los casos
- ✅ Manejo de errores centralizado

**Tests a crear**:
- `iceCalculator.test.ts`: Validar cálculo de puntaje
- `stringValidator.test.ts`: Validación de entrada
- `errorHandler.test.ts`: Manejo de errores

---

## 📌 TAREA 3: Context API y Gestión de Estado

**Descripción**: Implementar la gestión de estado global usando Context API con useReducer para manejar tareas, errores y estados de carga.

**Duración estimada**: 5-6 horas

**Dependencias**: Tarea 2 ✓

**Archivos a crear**:
```
src/
├── context/
│   ├── TaskContext.tsx
│   ├── TaskContext.types.ts
│   └── useTaskContext.ts
└── hooks/
    ├── useTask.ts
    ├── useICECalculation.ts
    └── useApiCall.ts
```

**Contenido detallado**:

### `context/TaskContext.types.ts`
```typescript
export interface TaskContextType {
  tasks: Task[];
  isLoading: boolean;
  error: string | null;
  addTask: (task: TaskInput) => Promise<void>;
  completeTask: (id: string) => void;
  deleteTask: (id: string) => void;
  clearAllTasks: () => void;
}

export type TaskAction =
  | { type: 'ADD_TASK'; payload: Task }
  | { type: 'COMPLETE_TASK'; payload: string }
  | { type: 'DELETE_TASK'; payload: string }
  | { type: 'SET_LOADING'; payload: boolean }
  | { type: 'SET_ERROR'; payload: string | null }
  | { type: 'CLEAR_ALL_TASKS' };
```

### `context/TaskContext.tsx`
- Crear contexto con estado inicial
- Implementar reducer con todas las acciones
- Exportar `TaskProvider` como componente wrapper
- Manejar errores y loading states

### `context/useTaskContext.ts`
- Hook personalizado para acceder al contexto
- Validar que se usa dentro de TaskProvider

### `hooks/useTask.ts`
```typescript
export const useTask = () => {
  // Wrapper sobre useTaskContext para lógica de tareas
  // Métodos: addTask, completeTask, deleteTask
}
```

### `hooks/useICECalculation.ts`
```typescript
export const useICECalculation = () => {
  // Hook para llamar API de IA
  // Retorna: { iceScore, isLoading, error }
}
```

### `hooks/useApiCall.ts`
```typescript
export const useApiCall = <T,>(apiCall: () => Promise<T>) => {
  // Hook genérico para cualquier llamada API
  // Maneja loading, error, retry, timeout
}
```

**Actividades**:
- [ ] Definir tipos de estado y acciones
- [ ] Implementar reducer puro
- [ ] Crear TaskProvider component
- [ ] Implementar hook useTaskContext
- [ ] Crear hooks personalizados para lógica
- [ ] Agregar error boundaries si es necesario
- [ ] Testear con React DevTools

**Criterios de aceptación**:
- ✅ Todas las acciones del reducer funcionan
- ✅ No hay memory leaks
- ✅ Error handling centralizado
- ✅ Loading states correctos
- ✅ Context accesible desde cualquier componente

**Tests a crear**:
- `TaskContext.test.tsx`: Testar reducer y acciones
- `useTaskContext.test.ts`: Validar hook

---

## 📌 TAREA 4: Componentes Comunes y de Infraestructura

**Descripción**: Crear componentes reutilizables de Material UI que sirven como base para otros componentes (alerts, spinners, diálogos).

**Duración estimada**: 3-4 horas

**Dependencias**: Tarea 1 ✓, Tarea 2 ✓

**Archivos a crear**:
```
src/
├── components/
│   ├── common/
│   │   ├── ErrorAlert.tsx
│   │   ├── ErrorAlert.types.ts
│   │   ├── LoadingSpinner.tsx
│   │   ├── ConfirmDialog.tsx
│   │   ├── ConfirmDialog.types.ts
│   │   └── Layout.tsx
│   └── styles/
│       ├── theme.ts
│       ├── palette.ts
│       └── global.css
```

**Componentes a implementar**:

### `ErrorAlert.tsx`
- Snackbar con mensaje de error
- Props: `open`, `message`, `onClose`, `severity`
- Usar `@mui/material/Alert` y `Snackbar`

### `LoadingSpinner.tsx`
- CircularProgress centrado
- Props: `fullHeight?`
- Usar `@mui/material/CircularProgress`

### `ConfirmDialog.tsx`
- Dialog de confirmación
- Props: `open`, `title`, `message`, `onConfirm`, `onCancel`
- Usar `@mui/material/Dialog`

### `Layout.tsx`
- Componente wrapper principal
- Props: `children`
- Incluye: AppBar, Container, Footer opcional

### `theme.ts`
```typescript
export const theme = createTheme({
  palette: {
    primary: { main: '#1976d2' },
    success: { main: '#4caf50' },
    error: { main: '#f44336' },
    warning: { main: '#ff9800' },
    // ... más colores
  },
  typography: { /* ... */ },
  components: { /* ... */ },
})
```

**Actividades**:
- [ ] Crear tema de Material UI
- [ ] Implementar ErrorAlert
- [ ] Implementar LoadingSpinner
- [ ] Implementar ConfirmDialog
- [ ] Implementar Layout wrapper
- [ ] Crear variables CSS globales
- [ ] Testear componentes en Storybook (opcional)

**Criterios de aceptación**:
- ✅ Todos los componentes renderean sin errores
- ✅ Tema consistente en toda la app
- ✅ Componentes reutilizables
- ✅ Accesibles (ARIA labels)
- ✅ Responsive en móvil

**Tests a crear**:
- `ErrorAlert.test.tsx`
- `LoadingSpinner.test.tsx`
- `ConfirmDialog.test.tsx`

---

## 📌 TAREA 5: Componentes Principales - Formulario, Lista y Tarjeta

**Descripción**: Implementar los tres componentes más importantes: TaskForm (entrada), TaskList (contenedor) y TaskCard (presentación individual).

**Duración estimada**: 8-10 horas

**Dependencias**: Tarea 3 ✓, Tarea 4 ✓

**Archivos a crear**:
```
src/components/
├── TaskForm/
│   ├── TaskForm.tsx
│   ├── TaskForm.types.ts
│   ├── TaskForm.styles.ts
│   └── TaskForm.test.tsx
├── TaskList/
│   ├── TaskList.tsx
│   ├── TaskList.types.ts
│   ├── TaskList.styles.ts
│   └── TaskList.test.tsx
└── TaskCard/
    ├── TaskCard.tsx
    ├── TaskCard.types.ts
    ├── TaskCard.styles.ts
    └── TaskCard.test.tsx
```

**Componentes a implementar**:

### `TaskForm.tsx`
**Funcionalidad**:
- Input para título (requerido, 1-100 caracteres)
- TextArea para descripción (requerida, 10-500 caracteres)
- Button para crear tarea
- Loading state mientras se llama API
- Mostrar errores de validación en tiempo real
- Reset de formulario tras éxito

**Props**:
```typescript
interface TaskFormProps {
  onTaskCreated?: (task: Task) => void;
}
```

**Material UI components**:
- `TextField` (título y descripción)
- `Button` (crear)
- `CircularProgress` (loading)
- `FormHelperText` (errores)

### `TaskList.tsx`
**Funcionalidad**:
- Renderizar lista de tareas
- Mensaje si no hay tareas
- Filtro completadas/pendientes (botones)
- Scroll vertical si hay muchas tareas
- Botón "Limpiar todas" con confirmación

**Props**:
```typescript
interface TaskListProps {
  tasks: Task[];
  onCompleteTask: (id: string) => void;
  onDeleteTask: (id: string) => void;
}
```

**Material UI components**:
- `Box` (contenedor)
- `Button` (filtros)
- `List` o `Stack` (lista)

### `TaskCard.tsx`
**Funcionalidad**:
- Mostrar título, descripción, fecha
- Mostrar ICE score con colores
- Botón "Completar" (marca como done)
- Botón "Eliminar" con confirmación
- Estilos diferentes si está completada
- Hover effect

**Props**:
```typescript
interface TaskCardProps {
  task: Task;
  onComplete: (id: string) => void;
  onDelete: (id: string) => void;
}
```

**Material UI components**:
- `Card`
- `CardContent`
- `CardActions`
- `CardHeader`
- `IconButton`
- `Button`
- `Typography`
- `Chip` (para ICE score)

**Actividades**:
- [ ] Implementar TaskForm con validación en tiempo real
- [ ] Integrar validadores de Tarea 2
- [ ] Implementar TaskList con filtros
- [ ] Implementar TaskCard con interactividad
- [ ] Crear estilos con sx prop y `.styles.ts`
- [ ] Integrar con context (useTaskContext)
- [ ] Testear manejo de errores
- [ ] Testear responsividad

**Criterios de aceptación**:
- ✅ TaskForm valida entrada y previene submit si hay errores
- ✅ TaskList renderiza correctamente
- ✅ TaskCard responde a clicks
- ✅ Todos los componentes responsivos
- ✅ No hay console errors
- ✅ Tests cobertura >80%

**Tests a crear**:
- `TaskForm.test.tsx`
- `TaskList.test.tsx`
- `TaskCard.test.tsx`

---

## 📌 TAREA 6: Componente ICEScore y Integración con API de IA

**Descripción**: Crear el componente ICEScore y conectar con la API de Google Gemini para calcular valores automáticos desde la descripción de la tarea.

**Duración estimada**: 7-8 horas

**Dependencias**: Tarea 2 ✓, Tarea 5 ✓

**Archivos a crear**:
```
src/
├── components/
│   └── ICEScore/
│       ├── ICEScore.tsx
│       ├── ICEScore.types.ts
│       ├── ICEScore.styles.ts
│       └── ICEScore.test.tsx
└── services/
    ├── geminiService.ts
    └── geminiService.test.ts
```

**Componentes a implementar**:

### `ICEScore.tsx`
**Funcionalidad**:
- Mostrar valores de Impacto, Confianza, Esfuerzo en tarjetas pequeñas
- Mostrar puntaje total con color según valor:
  - Verde: ICE > 5 (muy recomendada)
  - Naranja: ICE 2-5 (moderada)
  - Rojo: ICE < 2 (baja prioridad)
- Mostrar ícono de progreso
- Formato: "ICE Score: 7.5"

**Props**:
```typescript
interface ICEScoreProps {
  score: ICEScore;
  variant?: 'contained' | 'outlined';
}
```

**Material UI components**:
- `Box` (contenedor)
- `Chip` (valores individuales)
- `Typography`
- `Icon` (de @mui/icons-material)
- Color coding con palette

### `geminiService.ts`
**Funcionalidad**:
- Llamar API de Google Gemini
- Enviar descripción y prompt
- Parsear respuesta JSON
- Validar valores en rango 1-10
- Clamping automático si está fuera de rango
- Manejo de errores con reintentos
- Timeout después de 15 segundos

**Métodos**:
```typescript
export const getICEScoreFromAI = async (
  description: string
): Promise<ICEScore> => {
  // Llamar API, parsear, validar, retornar
}

export const callGeminiAPI = async (prompt: string): Promise<string> => {
  // Llamada bruta a API
}
```

**Prompt de sistema**:
```
Eres un experto en priorización usando el modelo ICE.

Dada esta tarea:
"[DESCRIPCIÓN]"

Evalúa EXACTAMENTE en este formato JSON:
{
  "impacto": [1-10],
  "confianza": [1-10],
  "esfuerzo": [1-10]
}

Definiciones:
- Impacto: Beneficio generado (1=ninguno, 10=transformador)
- Confianza: Seguridad en alcanzar beneficio (1=dudoso, 10=seguro)
- Esfuerzo: Trabajo requerido (1=trivial, 10=enorme)
```

**Actividades**:
- [ ] Crear servicio de Gemini con manejo de errores
- [ ] Implementar retry logic con backoff
- [ ] Implementar timeout de 15s
- [ ] Implementar clamping de valores
- [ ] Crear componente ICEScore
- [ ] Integrar con TaskForm (llamar API al crear tarea)
- [ ] Mostrar loading spinner durante llamada
- [ ] Testear con diferentes respuestas de API

**Criterios de aceptación**:
- ✅ ICEScore se calcula correctamente
- ✅ Valores fuera de rango se clampean
- ✅ Loading state mostrado
- ✅ Errores manejados gracefully
- ✅ Colores corretos según puntaje
- ✅ Timeout funciona
- ✅ Reintenta en caso de fallo

**Tests a crear**:
- `ICEScore.test.tsx`
- `geminiService.test.ts` (tests sin hacer llamadas reales)

---

## 📌 TAREA 7: Integración Completa y Testing

**Descripción**: Ensamblar todos los componentes en App.tsx, crear flujo completo end-to-end, y ejecutar suite de tests completa.

**Duración estimada**: 6-7 horas

**Dependencias**: Tarea 5 ✓, Tarea 6 ✓

**Archivos a modificar/crear**:
```
src/
├── App.tsx
├── App.styles.ts
├── main.tsx
└── tests/
    ├── integration/
    │   ├── createTask.integration.test.tsx
    │   └── completeTask.integration.test.tsx
    └── e2e/ (opcional)
```

**Contenido de App.tsx**:

```typescript
export const App = () => {
  return (
    <ThemeProvider theme={theme}>
      <TaskProvider>
        <Layout>
          <TaskForm />
          <TaskList />
          <ErrorAlert />
        </Layout>
      </TaskProvider>
    </ThemeProvider>
  );
};
```

**Actividades**:
- [ ] Crear App.tsx con todos los componentes
- [ ] Envolver con TaskProvider y ThemeProvider
- [ ] Implementar Layout principal
- [ ] Conectar flujo completo: Form → API → List
- [ ] Crear tests de integración
- [ ] Testear flujo completo: crear → validar → API → mostrar
- [ ] Testear manejo de errores end-to-end
- [ ] Verificar responsividad en distintos tamaños
- [ ] Ejecutar test coverage (target: >80%)
- [ ] Limpiar console.logs

**Tests de integración a crear**:
- Crear tarea sin errores
- Crear tarea con descripción < 10 caracteres (error)
- API llama correctamente con descripción
- Tarea aparece en lista tras creación
- Completar tarea cambia estado
- Eliminar tarea la quita de lista
- Limpiar todas confirma y borra todo

**Criterios de aceptación**:
- ✅ Flujo completo funciona sin errores
- ✅ Tests cobertura >80%
- ✅ No hay memory leaks
- ✅ Responsivo en móvil y desktop
- ✅ Manejo de errores en todos los casos
- ✅ Console sin warnings

---

## 📌 TAREA 8: Optimización, Pulido y Deployment

**Descripción**: Optimizar performance, revisar accesibilidad, documentar código y preparar para deployment.

**Duración estimada**: 5-6 horas

**Dependencias**: Tarea 7 ✓

**Archivos a crear/modificar**:
```
├── README.md
├── DEPLOYMENT.md
├── .github/
│   └── workflows/
│       └── deploy.yml (opcional)
├── vite.config.ts (revisión)
└── package.json (revisión)
```

**Actividades**:
- [ ] Revisar bundle size con `npm run build`
- [ ] Usar React.memo en componentes costosos si es necesario
- [ ] Implementar code splitting (lazy loading) si aplica
- [ ] Auditoría de Lighthouse (Performance, Accessibility)
- [ ] Verificar WCAG 2.1 Level AA
- [ ] Testing en navegadores: Chrome, Firefox, Safari, Edge
- [ ] Testing en móvil: iOS Safari, Android Chrome
- [ ] Documentar API con JSDoc
- [ ] Crear README con instrucciones de setup
- [ ] Crear DEPLOYMENT.md con pasos para prod
- [ ] Limpiar TODO comments y console.logs
- [ ] Crear script de deploy (Vercel, Netlify, etc.)
- [ ] Configurar variables de entorno en hosting

**Documentación a crear**:
- README.md: Setup, cómo usar, scripts, troubleshooting
- DEPLOYMENT.md: Pasos para deploy a Vercel/Netlify
- JSDoc comments en funciones principales
- Guía de contribución (opcional)

**Optimizaciones**:
- [ ] Lazy load de componentes grandes
- [ ] Memoización de callbacks si es necesario
- [ ] Image optimization (si hay imágenes)
- [ ] CSS minification (Vite lo hace)
- [ ] Tree shaking de dependencias
- [ ] Prefetch de recursos críticos

**Testing final**:
- [ ] Lighthouse score >90
- [ ] Todos los tests pasan
- [ ] No hay console errors
- [ ] Funciona offline (si aplica cache)
- [ ] API key está en variables de entorno
- [ ] No hay secrets en código

**Criterios de aceptación**:
- ✅ Bundle size < 300KB gzipped
- ✅ Lighthouse score >85
- ✅ Todos los tests pasan
- ✅ Documentación completa
- ✅ Listo para production
- ✅ Variables de entorno configuradas

**Deployment a**:
- **Vercel** (recomendado para Next.js pero funciona con Vite)
- **Netlify** (excelente para apps estáticas)
- **GitHub Pages** (si es repo público)

---

## 📊 Tabla de Progreso General

| # | Tarea | Duración | Estado | Fecha Inicio | Fecha Fin |
|---|-------|----------|--------|--------------|-----------|
| 1 | Setup Inicial | 3-4h | ⏳ | | |
| 2 | Tipos y Servicios | 4-5h | ⏳ | | |
| 3 | Context y Hooks | 5-6h | ⏳ | | |
| 4 | Componentes Comunes | 3-4h | ⏳ | | |
| 5 | Componentes Principales | 8-10h | ⏳ | | |
| 6 | ICEScore e IA | 7-8h | ⏳ | | |
| 7 | Integración y Testing | 6-7h | ⏳ | | |
| 8 | Optimización y Deploy | 5-6h | ⏳ | | |
| **TOTAL** | | **41-50h** | | | |

**Leyenda**: ⏳ Pendiente | 🔄 En Progreso | ✅ Completada | ⚠️ Bloqueada

---

## 📋 Checklist Pre-Inicio

Antes de comenzar, verificar:

- [ ] Node.js v16+ instalado (`node -v`)
- [ ] npm v8+ instalado (`npm -v`)
- [ ] Editor (VS Code) con extensiones:
  - [ ] ESLint
  - [ ] Prettier
  - [ ] Thunder Client o Postman (para testing API)
- [ ] API Key de Google Gemini obtenida en https://makersuite.google.com/app/apikey
- [ ] Repositorio Git inicializado (`git init`)
- [ ] `.gitignore` creado (excluir `node_modules`, `.env`, `dist/`)
- [ ] Rama main preparada

---

## 🔗 Dependencias Entre Tareas

```
Tarea 1 (Setup)
    ↓
    ├─→ Tarea 2 (Tipos y Servicios)
    │       ↓
    │       ├─→ Tarea 3 (Context)
    │       │       ↓
    │       │       ├─→ Tarea 5 (Componentes)
    │       │               ↓
    │       │               ├─→ Tarea 7 (Integración)
    │       │                       ↓
    │       │                       └─→ Tarea 8 (Deploy)
    │       │
    │       └─→ Tarea 4 (Comunes)
    │           ↓
    │           └─→ Tarea 5 (Componentes)
    │
    └─→ Tarea 6 (ICEScore)
            ↓
            └─→ Tarea 7 (Integración)
```

---

## 🎯 Criterios de Éxito General

✅ **Funcionalidad**:
- App completa funciona sin errores
- Flujo crear → API → mostrar ICE funciona
- Validaciones previenen datos malos
- Errores manejados gracefully

✅ **Código**:
- TypeScript estricto, sin `any`
- ESLint sin warnings
- Tests >80% cobertura
- Clean Code aplicado (SRP, DRY, etc.)

✅ **Experiencia**:
- Interfaz intuitiva y responsiva
- Performance >85 Lighthouse
- Accesible (WCAG 2.1 AA)
- Sin console errors

✅ **Documentación**:
- README claro
- Código comentado donde es complejo
- Guía de deployment

---

## 📞 Notas Importantes

1. **MVP simple**: Sin localStorage, sin backend real, sin multi-usuario
2. **Data en memoria**: Se pierden al recargar página (por diseño)
3. **Validación local primero**: Antes de llamar API
4. **Error recovery**: Siempre permitir reintento
5. **A11y**: Navegable con teclado, screen readers soportados
6. **Responsivo**: Funciona en móvil y desktop

---

**Versión del Plan**: 1.0  
**Última actualización**: Mayo 2026  
**Autor**: Equipo de Desarrollo
