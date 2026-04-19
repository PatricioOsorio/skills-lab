---
name: bem-styling-standards-angular
description: >
  Estandar de estilos para Angular con BEM por bloque semantico + prefijo por iniciales,
  Tailwind CSS 4 y DaisyUI 5. Usalo para crear o refactorizar pages, layouts y
  componentes UI con HTML semantico y CSS-first.
---

# BEM Styling Standards

## Proposito

Mantener HTML limpio y semantico.
Mover composicion visual a CSS del componente.
Usar BEM con bloque semantico + elementos con iniciales del bloque.

## Stack

- Angular
- CSS nativo (sin Sass)
- Tailwind CSS 4+
- DaisyUI 5+
- BEM con prefijo por iniciales

## Regla Principal

Por defecto:

- HTML usa bloque semantico como contenedor base.
- Hijos del bloque usan prefijo por iniciales del bloque.
- Tailwind y DaisyUI viven en CSS con `@apply`.

Excepciones permitidas en HTML solo si hay razon clara:

- accesibilidad o legibilidad puntual
- caso local donde mover a CSS agrega mas complejidad

Evita templates saturados de utilidades.

## Convencion BEM

Usa:

- `block`
- `xx__element`
- `xx__element--modifier`
- `block--modifier` (solo si el modificador aplica al contenedor)

Donde `xx` son iniciales del bloque semantico.

Ejemplos de bloque semantico:

- `empty-state`
- `page-header`
- `stats-grid`
- `content-panel`

### Regla de iniciales (obligatoria)

1. Si bloque tiene varias palabras separadas por `-`, toma primera letra de cada palabra.
2. Si bloque tiene una sola palabra, renombra bloque a `<palabra>-container`.
3. Usa ese prefijo para todos los elementos del bloque.
4. Colisiones de prefijo se permiten por contexto local del componente.

Ejemplos:

- `empty-state` -> `es__title`, `es__button`, `es__button--primary`
- `page-header` -> `ph__title`, `ph__actions`
- `profile` debe ser `profile-container` -> `pc__avatar`, `pc__name`

## Regla Critica de Nesting (Angular + CSS nativo)

No uses sintaxis tipo Sass para concatenar BEM con `&`.
En CSS nativo, esto falla o genera warnings en Angular:

- `&__element`
- `&--modifier`

Hazlo asi (selector explicito):

```css
.empty-state {
  @apply rounded-box border border-base-300 bg-base-100 p-4;
}

.es__title {
  @apply text-lg font-semibold;
}

.empty-state--featured {
  @apply border-primary;
}

.es__button--primary {
  @apply border-primary;
}
```

## `:host` vs bloque

- `:host` controla comportamiento externo del componente (display, ancho, posicion).
- Bloque semantico controla layout y presentacion interna.

```css
:host {
  display: block;
}

.dashboard-panel {
  @apply flex flex-col gap-4;
}

.dp__header {
  @apply text-lg font-semibold text-base-content;
}
```

## `@reference` (regla validada)

Para usar `@apply` en CSS de componente, agrega `@reference` al stylesheet global.

### Alias recomendado (funciona en este proyecto)

Configurar en `package.json`:

```json
{
  "imports": {
    "#styles/app.css": "./src/styles/app.css"
  }
}
```

Usar en CSS de componente:

```css
@reference '#styles/app.css';
```

## DaisyUI + Tailwind

DaisyUI se usa como capa de componentes desde CSS.
Preferir wrappers BEM sobre clases sueltas de DaisyUI en HTML.

```css
.search-form {
  @apply flex flex-col gap-4;
}

.sf__input {
  @apply input input-bordered w-full;
}

.sf__input--error {
  @apply input-error;
}

.sf__submit {
  @apply btn btn-primary;
}
```

## Responsive

Responsive va en CSS, no en template:

```css
.page-layout {
  @apply flex flex-col gap-4;
  @apply md:flex-row md:gap-6;
}

.pl__sidebar {
  @apply w-full md:w-64;
}

.pl__content {
  @apply flex-1;
}
```

## Reglas Practicas

- Bloques semanticos y nombres consistentes.
- Un bloque base por componente visual principal.
- Elementos siempre con prefijo de iniciales del bloque.
- Estados y variantes con modificadores BEM.
- Excepcion en HTML solo si mejora claridad real.
- Ejecutar build luego de cambios estructurales de estilos.

## Ejemplos Validos e Invalidos

| Caso                    | Valido                             | Invalido                                |
| ----------------------- | ---------------------------------- | --------------------------------------- |
| Bloque + elemento       | `empty-state` + `es__title`        | `empty-state__title`                    |
| Modificador de elemento | `es__button--ghost`                | `empty-state__button--ghost`            |
| Bloque de una palabra   | `profile-container` + `pc__avatar` | `profile` + `p__avatar`                 |
| CSS nativo              | `.es__card {}`                     | `&__card {}`                            |
| Utilidades en template  | clases BEM                         | template con muchas utilidades Tailwind |

## Checklist de Migracion

1. Identificar bloque semantico principal del componente.
2. Si bloque tiene una palabra, renombrar a `<palabra>-container`.
3. Calcular prefijo por iniciales.
4. Renombrar `block__element` a `xx__element` en HTML y CSS.
5. Mantener modificadores como `xx__element--variant`.
6. Mover utilidades Tailwind/DaisyUI a CSS con `@apply`.
7. Agregar/verificar `@reference '#styles/app.css';` en CSS de componente.
8. Verificar que no exista `&__...` ni `&--...`.
9. Ejecutar build y validar estilos.

## Checklist Rapido

- HTML tiene bloque semantico y elementos por iniciales.
- CSS de componente tiene `@reference '#styles/app.css';`.
- No existe `&__...` ni `&--...` en CSS nativo.
- Tailwind y DaisyUI via `@apply` cuando conviene.
- Responsive en CSS.
- Build pasa sin errores de resolucion CSS.
