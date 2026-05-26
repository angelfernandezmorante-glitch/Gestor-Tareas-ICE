# 📋 Guía de Control para Desarrollo ReactJS
## MVP: Gestor de Tareas Inteligente con Modelo ICE

**Versión**: 1.0  
**Última actualización**: Mayo 2026  
**Tecnologías**: React 18+, TypeScript, Material UI, Vite  
**Enfoque**: Clean Code, Código Simple y Mantenible

---

## 📑 Tabla de Contenidos

1. [Estructura del Proyecto](#estructura-del-proyecto)
2. [Configuración Inicial](#configuración-inicial)
3. [Estándares de Código](#estándares-de-código)
4. [Arquitectura de Componentes](#arquitectura-de-componentes)
5. [Gestión de Estado](#gestión-de-estado)
6. [Guía de Material UI](#guía-de-material-ui)
7. [Validaciones y Manejo de Errores](#validaciones-y-manejo-de-errores)
8. [Checklist de Desarrollo](#checklist-de-desarrollo)

---

## 🗂️ Estructura del Proyecto

### Estructura de carpetas recomendada

```
src/
├── components/
│   ├── TaskForm/
│   │   ├── TaskForm.tsx
│   │   ├── TaskForm.types.ts
│   │   ├── TaskForm.styles.ts
│   │   └── TaskForm.test.tsx
│   ├── TaskList/
│   │   ├── TaskList.tsx
│   │   ├── TaskList.types.ts
│   │   ├── TaskList.styles.ts
│   │   └── TaskList.test.tsx
│   ├── TaskCard/
│   │   ├── TaskCard.tsx
│   │   ├── TaskCard.types.ts
│   │   ├── TaskCard.styles.ts
│   │   └── TaskCard.test.tsx
│   ├── ICEScore/
│   │   ├── ICEScore.tsx
│   │   ├── ICEScore.types.ts
│   │   ├── ICEScore.styles.ts
│   │   └── ICEScore.test.tsx
│   └── common/
│       ├── ErrorAlert.tsx
│       ├── LoadingSpinner.tsx
│       └── ConfirmDialog.tsx
├── context/
│   ├── TaskContext.tsx
│   ├── TaskContext.types.ts
│   └── useTaskContext.ts
├── services/
│   ├── aiService.ts
│   ├── geminiService.ts
│   ├── validationService.ts
│   └── apiConfig.ts
├── types/
│   ├── task.types.ts
│   ├── ice.types.ts
│   └── api.types.ts
├── utils/
│   ├── iceCalculator.ts
│   ├── stringValidator.ts
│   ├── errorHandler.ts
│   └── constants.ts
├── hooks/
│   ├── useTask.ts
│   ├── useICECalculation.ts
│   └── useApiCall.ts
├── styles/
│   ├── theme.ts
│   ├── palette.ts
│   └── global.css
├── App.tsx
├── App.styles.ts
└── main.tsx

public/
├── index.html
└── favicon.svg

tests/
└── setup.ts
```

### Reglas de estructura

- **Un componente = Una carpeta**: Cada componente React tiene su propia carpeta
- **Colocalización**: Todos los archivos relacionados en la misma carpeta
- **Separación de responsabilidades**: `.types.ts` para tipos, `.styles.ts` para estilos, `.test.tsx` para tests
- **Services reutilizables**: Lógica de negocio en `services/`
- **Hooks personalizados**: En carpeta `hooks/`
- **Types globales**: En carpeta `types/`
- **Utilidades puras**: Funciones sin estado en `utils/`

---

## ⚙️ Configuración Inicial

### 1. Crear proyecto con Vite

```bash
npm create vite@latest . -- --template react-ts
npm install
```

### 2. Instalar dependencias

```bash
# React y React DOM (ya incluidas)
npm install react react-dom

# Material UI
npm install @mui/material @emotion/react @emotion/styled
npm install @mui/icons-material

# TypeScript (dev)
npm install -D typescript @types/react @types/react-dom @types/node

# Linting y Formatting
npm install -D eslint eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-@typescript-eslint
npm install -D prettier

# Testing (opcional pero recomendado)
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

### 3. Configuración de TypeScript

**tsconfig.json**:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "baseUrl": "./src"
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### 4. Configuración de ESLint

**.eslintrc.cjs**:
```javascript
module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:@typescript-eslint/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs'],
  parser: '@typescript-eslint/parser',
  plugins: ['react-refresh', '@typescript-eslint'],
  rules: {
    'react/react-in-jsx-scope': 'off',
    'react-refresh/only-exports-components': 'warn',
    'no-console': ['warn', { allow: ['warn', 'error'] }],
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
  },
}
```

### 5. Configuración de Prettier

**.prettierrc**:
```json
{
  "singleQuote": true,
  "semi": false,
  "trailingComma": "es5",
  "tabWidth": 2,
  "useTabs": false,
  "printWidth": 80,
  "arrowParens": "always"
}
```

---

## 💻 Estándares de Código

### Convenciones de Nombres

| Elemento | Convención | Ejemplo |
|----------|-----------|---------|
| **Componentes** | PascalCase | `TaskForm.tsx`, `TaskCard.tsx` |
| **Hooks** | camelCase con prefijo `use` | `useTask.ts`, `useICECalculation.ts` |
| **Variables/Funciones** | camelCase | `taskTitle`, `calculateICE()` |
| **Constantes** | UPPER_SNAKE_CASE | `MAX_TITLE_LENGTH`, `API_TIMEOUT` |
| **Tipos/Interfaces** | PascalCase con sufijo | `TaskType`, `ICEScoreType` |
| **Archivos de tipos** | `.types.ts` | `task.types.ts` |
| **Archivos de estilos** | `.styles.ts` | `TaskCard.styles.ts` |

### Principios Clean Code a Aplicar

#### 1. **Single Responsibility Principle (SRP)**
```typescript
// ✅ BIEN: Componente con una responsabilidad
export const TaskForm: React.FC<TaskFormProps> = ({ onAddTask }) => {
  // Solo renderiza el formulario
  return (
    <form onSubmit={handleSubmit}>
      {/* JSX aquí */}
    </form>
  );
};

// ❌ MAL: Componente que hace demasiadas cosas
export const TaskManager = () => {
  // Validación, API, estado, UI todo junto
  // Difícil de testear y mantener
};
```

#### 2. **Don't Repeat Yourself (DRY)**
```typescript
// ✅ BIEN: Función reutilizable
export const validateTaskInput = (
  title: string,
  description: string
): ValidationResult => {
  return {
    isValid: title.length > 0 && description.length >= 10,
    errors: [],
  };
};

// ❌ MAL: Validación repetida en cada componente
```

#### 3. **Mantener Componentes Pequeños**
```typescript
// ✅ BIEN: Componentes enfocados (100-200 líneas máximo)
export const TaskCard: React.FC<TaskCardProps> = ({ task, onComplete, onDelete }) => {
  return (
    <Card>
      <CardContent>
        <Typography>{task.title}</Typography>
        <ICEScore score={task.iceScore} />
      </CardContent>
      <CardActions>
        <Button onClick={onComplete}>Completar</Button>
        <Button onClick={onDelete}>Eliminar</Button>
      </CardActions>
    </Card>
  );
};

// ❌ MAL: Componente que hace todo (500+ líneas)
```

#### 4. **Nombres Significativos**
```typescript
// ✅ BIEN: Nombres claros y descriptivos
const [isLoadingICEScore, setIsLoadingICEScore] = useState(false);
const handleCompleteTask = (taskId: string) => { /* ... */ };

// ❌ MAL: Nombres cortos o ambiguos
const [loading, setLoading] = useState(false);
const handleClick = () => { /* ... */ };
```

#### 5. **Funciones Puras**
```typescript
// ✅ BIEN: Función pura sin efectos secundarios
export const calculateICEScore = (
  impacto: number,
  confianza: number,
  esfuerzo: number
): number => {
  return (impacto + confianza) / esfuerzo;
};

// ❌ MAL: Función impura que modifica estado externo
let totalScore = 0;
export const calculateICEScore = (impacto: number, confianza: number) => {
  totalScore += impacto + confianza; // ¡Efecto secundario!
  return totalScore;
};
```

### Reglas de TypeScript Estrictas

```typescript
// ✅ Tipado completo
interface TaskProps {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  iceScore: {
    impacto: number;
    confianza: number;
    esfuerzo: number;
    total: number;
  };
}

export const TaskCard: React.FC<TaskProps> = ({ task }) => {
  return <div>{task.title}</div>;
};

// ❌ Tipado incompleto
export const TaskCard = ({ task }: any) => { /* ... */ };
```

---

## 🏗️ Arquitectura de Componentes

### Árbol de Componentes

```
┌─────────────────────────────────┐
│          App.tsx                │
│  (Orquestador principal)        │
└────────────┬────────────────────┘
             │
    ┌────────┼─────────┐
    │        │         │
    ▼        ▼         ▼
┌────────┐ ┌────────┐ ┌────────┐
│TaskForm│ │TaskList│ │ Snackbar
│        │ │        │ │(Notif) │
└────────┘ └───┬────┘ └────────┘
               │
          ┌────┴──────┐
          │            │
          ▼            ▼
      ┌────────┐  ┌────────┐
      │TaskCard│  │TaskCard│
      ├────────┤  ├────────┤
      │ICEScore│  │ICEScore│
      └────────┘  └────────┘
```

### Componentes Funcionales Recomendados

#### **App.tsx** (Orquestador)
- Manage global state con Context
- Renderiza TaskForm y TaskList
- Maneja notificaciones globales

#### **TaskForm.tsx** (Entrada de datos)
- Input fields para título y descripción
- Button para crear tarea
- Validación local
- Indicador de carga mientras se llama API

#### **TaskList.tsx** (Contenedor)
- Renderiza lista de TaskCard
- Mensaje cuando no hay tareas
- Lógica de filtrado (completadas/pendientes)

#### **TaskCard.tsx** (Componente presentacional)
- Renderiza una tarea individual
- Botones para completar/eliminar
- Integra ICEScore

#### **ICEScore.tsx** (Componente especializado)
- Renderiza valores de Impacto, Confianza, Esfuerzo
- Muestra el puntaje total
- Código de colores según el puntaje

### Composición vs Herencia

```typescript
// ✅ Composición: Preferida
export const TaskCard: React.FC<TaskCardProps> = ({ task, footer }) => {
  return (
    <Card>
      <CardContent>
        <Typography>{task.title}</Typography>
      </CardContent>
      <CardActions>
        {footer}
      </CardActions>
    </Card>
  );
};

// ❌ Herencia: No usar en React
class BaseTaskCard extends React.Component { /* ... */ }
class AdvancedTaskCard extends BaseTaskCard { /* ... */ }
```

---

## 🎯 Gestión de Estado

### Opción Recomendada: Context API + useReducer

La gestión de estado se realiza con **Context API** para simplicidad, evitando Redux innecesario.

#### Estructura de Context

**context/TaskContext.types.ts**:
```typescript
export interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  iceScore: ICEScore;
  createdAt: Date;
}

export interface ICEScore {
  impacto: number;
  confianza: number;
  esfuerzo: number;
  total: number;
}

export interface TaskContextType {
  tasks: Task[];
  isLoading: boolean;
  error: string | null;
  addTask: (task: Omit<Task, 'id' | 'createdAt'>) => Promise<void>;
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

**context/TaskContext.tsx**:
```typescript
import React, { createContext, useReducer, useCallback } from 'react';
import { TaskContextType, TaskAction, Task } from './TaskContext.types';
import { generateId } from 'utils/idGenerator';

const initialState: TaskContextType = {
  tasks: [],
  isLoading: false,
  error: null,
  addTask: async () => {},
  completeTask: () => {},
  deleteTask: () => {},
  clearAllTasks: () => {},
};

export const TaskContext = createContext<TaskContextType>(initialState);

const taskReducer = (state: TaskContextType, action: TaskAction): TaskContextType => {
  switch (action.type) {
    case 'ADD_TASK':
      return {
        ...state,
        tasks: [action.payload, ...state.tasks],
        error: null,
      };
    case 'COMPLETE_TASK':
      return {
        ...state,
        tasks: state.tasks.map((task) =>
          task.id === action.payload ? { ...task, completed: true } : task
        ),
      };
    case 'DELETE_TASK':
      return {
        ...state,
        tasks: state.tasks.filter((task) => task.id !== action.payload),
      };
    case 'SET_LOADING':
      return { ...state, isLoading: action.payload };
    case 'SET_ERROR':
      return { ...state, error: action.payload };
    case 'CLEAR_ALL_TASKS':
      return { ...state, tasks: [] };
    default:
      return state;
  }
};

interface TaskProviderProps {
  children: React.ReactNode;
}

export const TaskProvider: React.FC<TaskProviderProps> = ({ children }) => {
  const [state, dispatch] = useReducer(taskReducer, initialState);

  const addTask = useCallback(
    async (task: Omit<Task, 'id' | 'createdAt'>) => {
      dispatch({ type: 'SET_LOADING', payload: true });
      try {
        const newTask: Task = {
          ...task,
          id: generateId(),
          createdAt: new Date(),
        };
        dispatch({ type: 'ADD_TASK', payload: newTask });
      } catch (error) {
        dispatch({
          type: 'SET_ERROR',
          payload: error instanceof Error ? error.message : 'Error desconocido',
        });
      } finally {
        dispatch({ type: 'SET_LOADING', payload: false });
      }
    },
    []
  );

  const completeTask = useCallback((id: string) => {
    dispatch({ type: 'COMPLETE_TASK', payload: id });
  }, []);

  const deleteTask = useCallback((id: string) => {
    dispatch({ type: 'DELETE_TASK', payload: id });
  }, []);

  const clearAllTasks = useCallback(() => {
    dispatch({ type: 'CLEAR_ALL_TASKS' });
  }, []);

  const value: TaskContextType = {
    ...state,
    addTask,
    completeTask,
    deleteTask,
    clearAllTasks,
  };

  return <TaskContext.Provider value={value}>{children}</TaskContext.Provider>;
};
```

**context/useTaskContext.ts**:
```typescript
import { useContext } from 'react';
import { TaskContext } from './TaskContext';

export const useTaskContext = () => {
  const context = useContext(TaskContext);
  if (!context) {
    throw new Error('useTaskContext debe usarse dentro de TaskProvider');
  }
  return context;
};
```

### NO usar localStorage (por simplicidad en MVP)

```typescript
// ❌ Excluir: Los datos se pierden al recargar (por diseño)
// NO implementar localStorage.setItem/getItem en este MVP
```

---

## 🎨 Guía de Material UI

### Tema Recomendado

**styles/theme.ts**:
```typescript
import { createTheme, ThemeProvider } from '@mui/material/styles';

export const theme = createTheme({
  palette: {
    primary: {
      main: '#1976d2',
      light: '#42a5f5',
      dark: '#1565c0',
    },
    success: {
      main: '#4caf50',
    },
    error: {
      main: '#f44336',
    },
    warning: {
      main: '#ff9800',
    },
    background: {
      default: '#fafafa',
      paper: '#ffffff',
    },
  },
  typography: {
    fontFamily: 'Roboto, sans-serif',
    h1: { fontSize: '2rem', fontWeight: 700 },
    h2: { fontSize: '1.5rem', fontWeight: 700 },
    body1: { fontSize: '1rem' },
    body2: { fontSize: '0.875rem' },
  },
  shape: {
    borderRadius: 8,
  },
});
```

### Componentes Material UI a Usar

#### Para Formularios
```typescript
import {
  TextField,
  Button,
  FormControl,
  InputLabel,
  Select,
  Checkbox,
  FormControlLabel,
} from '@mui/material';

// Ejemplo: TaskForm.tsx
<TextField
  label="Título"
  variant="outlined"
  fullWidth
  value={title}
  onChange={(e) => setTitle(e.target.value)}
  error={!!titleError}
  helperText={titleError}
  placeholder="Ej: Implementar autenticación"
/>

<TextField
  label="Descripción"
  variant="outlined"
  fullWidth
  multiline
  rows={4}
  value={description}
  onChange={(e) => setDescription(e.target.value)}
  error={!!descriptionError}
  helperText={descriptionError}
  placeholder="Mínimo 10 caracteres"
/>

<Button variant="contained" fullWidth type="submit">
  Crear Tarea
</Button>
```

#### Para Tarjetas
```typescript
import {
  Card,
  CardContent,
  CardActions,
  CardHeader,
  Typography,
  IconButton,
  Chip,
} from '@mui/material';
import { DeleteOutlined, CheckCircleOutlined } from '@mui/icons-material';

// Ejemplo: TaskCard.tsx
<Card sx={{ mb: 2 }}>
  <CardHeader
    title={task.title}
    subheader={new Date(task.createdAt).toLocaleDateString()}
  />
  <CardContent>
    <Typography color="textSecondary" paragraph>
      {task.description}
    </Typography>
    <ICEScore score={task.iceScore} />
  </CardContent>
  <CardActions>
    <Button startIcon={<CheckCircleOutlined />} onClick={onComplete}>
      Completar
    </Button>
    <IconButton onClick={onDelete} color="error">
      <DeleteOutlined />
    </IconButton>
  </CardActions>
</Card>
```

#### Para Notificaciones
```typescript
import { Alert, AlertTitle, Snackbar } from '@mui/material';

// Ejemplo: ErrorAlert.tsx
<Snackbar
  open={open}
  autoHideDuration={6000}
  onClose={handleClose}
  anchorOrigin={{ vertical: 'top', horizontal: 'right' }}
>
  <Alert severity="error" onClose={handleClose}>
    <AlertTitle>Error</AlertTitle>
    {message}
  </Alert>
</Snackbar>
```

#### Para Cargando
```typescript
import { CircularProgress, Box } from '@mui/material';

// Ejemplo: LoadingSpinner.tsx
<Box display="flex" justifyContent="center" alignItems="center" minHeight="100vh">
  <CircularProgress />
</Box>
```

### Estilos con `sx` Prop (Preferido sobre makeStyles)

```typescript
// ✅ BIEN: Usar sx prop para estilos simples
<Box
  sx={{
    display: 'flex',
    flexDirection: 'column',
    gap: 2,
    p: 2,
    bgcolor: 'background.paper',
    borderRadius: 1,
  }}
>
  {/* Contenido */}
</Box>

// Usar makeStyles solo para componentes con lógica de estilos compleja
// La mayoría no lo necesitará
```

### Estilos Dedicados (`.styles.ts`)

Para componentes con estilos más complejos:

**components/TaskCard/TaskCard.styles.ts**:
```typescript
import { SxProps, Theme } from '@mui/material/styles';

export const taskCardStyles: SxProps<Theme> = {
  card: {
    mb: 2,
    '&:hover': {
      boxShadow: 4,
      transform: 'translateY(-2px)',
      transition: 'all 0.3s ease',
    },
  },
  completedContent: {
    textDecoration: 'line-through',
    opacity: 0.6,
  },
};
```

---

## ✅ Validaciones y Manejo de Errores

### Validación de Entrada

**utils/stringValidator.ts**:
```typescript
export interface ValidationResult {
  isValid: boolean;
  error: string | null;
}

export const validateTitle = (title: string): ValidationResult => {
  if (!title || title.trim().length === 0) {
    return { isValid: false, error: 'El título es requerido' };
  }
  if (title.length < 1 || title.length > 100) {
    return {
      isValid: false,
      error: 'El título debe tener entre 1 y 100 caracteres',
    };
  }
  return { isValid: true, error: null };
};

export const validateDescription = (description: string): ValidationResult => {
  if (!description || description.trim().length === 0) {
    return { isValid: false, error: 'La descripción es requerida' };
  }
  if (description.length < 10 || description.length > 500) {
    return {
      isValid: false,
      error: 'La descripción debe tener entre 10 y 500 caracteres',
    };
  }
  return { isValid: true, error: null };
};

export const validateICEValues = (
  impacto: number,
  confianza: number,
  esfuerzo: number
): { isValid: boolean; clampedValues: ICEScore } => {
  const clamp = (value: number) => Math.max(1, Math.min(10, value));

  return {
    isValid: impacto >= 1 && impacto <= 10 && confianza >= 1 && confianza <= 10 && esfuerzo >= 1 && esfuerzo <= 10,
    clampedValues: {
      impacto: clamp(impacto),
      confianza: clamp(confianza),
      esfuerzo: clamp(esfuerzo),
    },
  };
};
```

### Manejo de Errores

**services/errorHandler.ts**:
```typescript
export type ErrorSeverity = 'error' | 'warning' | 'info';

export interface AppError {
  message: string;
  severity: ErrorSeverity;
  code?: string;
  recoverable: boolean;
}

export class ErrorHandler {
  static handleAPIError(error: unknown): AppError {
    if (error instanceof Response) {
      if (error.status === 429) {
        return {
          message: 'Demasiadas solicitudes. Intenta nuevamente en unos momentos.',
          severity: 'warning',
          code: 'RATE_LIMIT',
          recoverable: true,
        };
      }
      if (error.status >= 500) {
        return {
          message: 'Error del servidor. Intenta más tarde.',
          severity: 'error',
          code: 'SERVER_ERROR',
          recoverable: true,
        };
      }
    }

    if (error instanceof TypeError && error.message.includes('fetch')) {
      return {
        message: 'Error de conexión. Verifica tu internet.',
        severity: 'error',
        code: 'NETWORK_ERROR',
        recoverable: true,
      };
    }

    return {
      message: error instanceof Error ? error.message : 'Error desconocido',
      severity: 'error',
      code: 'UNKNOWN_ERROR',
      recoverable: false,
    };
  }

  static isRetryable(error: AppError): boolean {
    return error.recoverable && ['RATE_LIMIT', 'NETWORK_ERROR'].includes(error.code || '');
  }
}
```

### Validación en Formulario

**components/TaskForm/TaskForm.tsx** (excerpt):
```typescript
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();

  // Validar entrada local PRIMERO
  const titleValidation = validateTitle(title);
  const descriptionValidation = validateDescription(description);

  if (!titleValidation.isValid || !descriptionValidation.isValid) {
    setTitleError(titleValidation.error);
    setDescriptionError(descriptionValidation.error);
    return;
  }

  // Si validaciones OK, llamar API
  setIsLoading(true);
  try {
    const iceScore = await getICEScore(description);
    await addTask({
      title,
      description,
      iceScore,
      completed: false,
    });
    resetForm();
  } catch (error) {
    const appError = ErrorHandler.handleAPIError(error);
    setError(appError.message);
  } finally {
    setIsLoading(false);
  }
};
```

---

## 🚀 Checklist de Desarrollo

### Fase 1: Setup Inicial ✓

- [ ] Crear proyecto con Vite + React + TypeScript
- [ ] Instalar dependencias (Material UI, ESLint, Prettier)
- [ ] Configurar `tsconfig.json` en modo estricto
- [ ] Configurar ESLint y Prettier
- [ ] Crear estructura de carpetas
- [ ] Configurar tema de Material UI

### Fase 2: Tipos y Servicios ✓

- [ ] Definir tipos en `types/task.types.ts`
- [ ] Crear `types/ice.types.ts`
- [ ] Crear `services/validationService.ts` con validadores
- [ ] Crear `utils/iceCalculator.ts` para calcular puntaje
- [ ] Configurar `apiConfig.ts` con API Key
- [ ] Crear `services/geminiService.ts` para llamadas a API

### Fase 3: Context y Hooks ✓

- [ ] Crear `TaskContext.tsx` con reducer
- [ ] Crear `useTaskContext.ts` hook
- [ ] Crear `hooks/useTask.ts` para lógica de tareas
- [ ] Crear `hooks/useICECalculation.ts` para llamadas API
- [ ] Crear `hooks/useApiCall.ts` para manejo genérico de API

### Fase 4: Componentes Funcionales ✓

- [ ] Crear `components/TaskForm/TaskForm.tsx`
  - [ ] Inputs para título y descripción
  - [ ] Validación en tiempo real
  - [ ] Indicador de carga
  - [ ] Botón de envío
  
- [ ] Crear `components/TaskCard/TaskCard.tsx`
  - [ ] Renderizar titulo y descripción
  - [ ] Botón de completar
  - [ ] Botón de eliminar
  - [ ] Integrar ICEScore

- [ ] Crear `components/TaskList/TaskList.tsx`
  - [ ] Renderizar lista de tareas
  - [ ] Mensaje si no hay tareas
  - [ ] Filtrado completadas/pendientes (opcional)

- [ ] Crear `components/ICEScore/ICEScore.tsx`
  - [ ] Mostrar valores individuales
  - [ ] Mostrar puntaje total
  - [ ] Código de colores

- [ ] Crear componentes comunes:
  - [ ] `components/common/ErrorAlert.tsx`
  - [ ] `components/common/LoadingSpinner.tsx`
  - [ ] `components/common/ConfirmDialog.tsx`

### Fase 5: Aplicación Principal ✓

- [ ] Crear `App.tsx`
  - [ ] Envolver con TaskProvider
  - [ ] Envolver con ThemeProvider
  - [ ] Renderizar TaskForm y TaskList
  - [ ] Manejo de notificaciones globales

- [ ] Crear `main.tsx` con React.StrictMode
- [ ] Crear `App.styles.ts` si necesario

### Fase 6: Testing ✓

- [ ] Configurar vitest
- [ ] Crear `TaskForm.test.tsx`
- [ ] Crear `TaskCard.test.tsx`
- [ ] Crear `iceCalculator.test.ts`
- [ ] Crear `validationService.test.ts`

### Fase 7: Integración e Implantación ✓

- [ ] Integrar API de Gemini
- [ ] Probar flujo completo: crear → validar → API → mostrar
- [ ] Probar manejo de errores (API offline, JSON inválido, timeout)
- [ ] Probar responsividad en móvil
- [ ] Limpiar console.logs
- [ ] Auditar TypeScript (sin errores)
- [ ] Auditar ESLint (sin warnings)
- [ ] Ejecutar tests
- [ ] Verificar performance
- [ ] Documentar API y componentes

### Fase 8: Optimización Final ✓

- [ ] Revisar bundle size
- [ ] Lazy loading de componentes si aplica
- [ ] Memoizar componentes si es necesario (React.memo)
- [ ] Profiling con React DevTools
- [ ] Accesibilidad (WCAG)
- [ ] SEO metadatos
- [ ] Deploy en hosting (Vercel, Netlify, etc.)

---

## 📊 Referencia Rápida de Componentes

### Firma de Componentes

Todos los componentes siguen este patrón:

```typescript
import { FC } from 'react';
import { Box } from '@mui/material';

interface ComponentNameProps {
  // Props aquí
}

export const ComponentName: FC<ComponentNameProps> = ({ prop1, prop2 }) => {
  return <Box>{/* JSX */}</Box>;
};

export default ComponentName;
```

### Estructura de Archivos por Componente

```
components/ComponentName/
├── ComponentName.tsx          # Lógica y renderizado
├── ComponentName.types.ts     # Interfaces y tipos
├── ComponentName.styles.ts    # Estilos con SxProps
└── ComponentName.test.tsx     # Tests
```

---

## 🔑 Variables de Entorno

**`.env`**:
```bash
# API de IA
VITE_GEMINI_API_KEY=tu_api_key_aqui

# Configuración
VITE_API_TIMEOUT=15000
VITE_MAX_RETRIES=3
```

**`apiConfig.ts`**:
```typescript
export const API_CONFIG = {
  GEMINI_API_KEY: import.meta.env.VITE_GEMINI_API_KEY,
  TIMEOUT: parseInt(import.meta.env.VITE_API_TIMEOUT || '15000'),
  MAX_RETRIES: parseInt(import.meta.env.VITE_MAX_RETRIES || '3'),
  GEMINI_ENDPOINT: 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent',
};
```

---

## 📚 Referencias y Recursos

### Documentación
- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Material UI Documentation](https://mui.com/material-ui/getting-started/)
- [Vite Guide](https://vitejs.dev/guide/)

### Clean Code
- Clean Code in TypeScript
- SOLID Principles
- React Best Practices

### Testing
- Vitest Documentation
- Testing Library Best Practices

---

## ⚖️ Principios Rectores del Proyecto

1. **Simplicidad**: No agregar librerías si Material UI ya lo cubre
2. **Claridad**: Código legible > código "inteligente"
3. **Mantenibilidad**: Fácil de entender 6 meses después
4. **Educativo**: Código que sirva como ejemplo para estudiantes
5. **Type-Safety**: TypeScript en modo estricto siempre
6. **Sin Backend**: Todo en memoria, como está especificado

---

**Última revisión**: Mayo 2026  
**Estado**: Activo  
**Responsable**: Equipo de Desarrollo
