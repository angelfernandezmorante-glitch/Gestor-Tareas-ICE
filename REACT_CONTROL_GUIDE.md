# React Control Guide

## Objetivo
Definir estructura, convenciones y patrones para desarrollar el MVP de Task Manager con React.

## Estructura recomendada
```
src/
├── components/
├── context/
├── services/
├── types/
├── utils/
├── hooks/
├── styles/
├── App.tsx
└── main.tsx
```

## Convenciones de nombres
- Componentes: `PascalCase`.
- Hooks: `useCamelCase`.
- Tipos: `PascalCase` con sufijo.
- Constantes: `UPPER_SNAKE_CASE`.
- Archivos de tipos: `.types.ts`.
- Estilos: `.styles.ts`.

## Configuración esencial
- TypeScript estricto.
- ESLint con reglas React y TypeScript.
- Prettier para formato.
- Vite como builder.

## Patrones de componentes
- Un componente = una carpeta.
- Separar tipos, estilos y tests.
- Preferir componentes funcionales.
- Mantener componentes bajo 200 líneas.

## Gestión de estado
- Usar `TaskContext` con `useReducer`.
- Guardar tareas en memoria.
- Exponer acciones: `addTask`, `completeTask`, `deleteTask`, `clearAllTasks`.
- Manejar `loading` y `error` en el contexto.

## Servicios y lógica
- `services/aiService.ts`: llamada a la API de IA.
- `services/apiConfig.ts`: configuración de endpoints y claves.
- `utils/iceCalculator.ts`: cálculo ICE puro.
- `utils/stringValidator.ts`: validación de inputs.

## Validaciones
- Título: 1-100 caracteres.
- Descripción: 10-500 caracteres.
- ICE: todos valores en [1,10].
- Si API falla, no crear tarea.

## UI y diseño con MUI
- Usar `TextField`, `Button`, `Card`, `Snackbar`.
- Feedback visual: errores en rojo, éxito en verde.
- Formulario limpio tras crear tarea.
- Deshabilitar botón mientras se procesa.

## Código limpio
- Aplicar SRP (una responsabilidad por componente).
- No repetir lógica.
- Nombres descriptivos.
- Funciones puras para cálculo y validación.

## Checklist rápido
- [ ] Estructura de carpetas adecuada.
- [ ] Tipos bien definidos.
- [ ] Estado global con contexto.
- [ ] Componentes claros y reutilizables.
- [ ] Integración de IA.
- [ ] Validación robusta.
- [ ] Mensajes de error útiles.
