# React Architecture

## Summary

React architecture is defined in order to build and implement resilient features
with familiar patterns that can be implemented by any developer on the team.

All UI work in this app is built around a **layered architecture**:

- Clear separation between:
  - **Data fetching / side effects**
  - **View-model / container logic**
  - **Pure presentation / layout**
- Predictable file locations for each concern.
- Easy unit testing and Storybook stories with **minimal mocking**.
- Ability to move components between screens without surprising coupling.

Every page/screen should follow these patterns.

## Responsibilities

For any feature or screen `<Feature>` (e.g. Dashboard, Settings, Profile), we work with **three main layers** plus leaf components:

1. **Context layer** (`src/contexts/<Feature>`)
2. **Container layer** (`src/containers/<Feature>`)
3. **Page layer** (`src/pages/<Feature>.tsx`)
4. **Presentational components** (`src/components/<feature>/*` or similar)

---

### Context layer (data / side effects)

**Location**

- `src/contexts/<Feature>/index.tsx` (or `src/contexts/<Feature>/<Feature>Context.tsx`)

**Responsibilities**

- Own **all external I/O** for that feature:
  - TanStack Query **queries** and **mutations**
  - Calls to Supabase, REST APIs, etc.
- Construct a **context value** that represents the feature’s state as a **pure data object**, including:
  - Data fields used by the UI
  - Derived values that are shared across multiple containers
  - Event handlers that trigger mutations or cross-feature actions
- Provide a **React context + hook**:
  - `FeatureContext`
  - `FeatureProvider`
  - `FeatureConsumer`
  - `useFeatureContext()`

**Rules**

- ✅ **MUST** use TanStack Query for server data (where applicable).
- ✅ **MUST** expose a **stable “view props” shape** (see §3) as the context value, or a superset of it.
- ✅ **MAY** include internal provider-only state (loading flags, error info) not exposed to containers if not needed.
- ❌ **MUST NOT** render complex layout.
- ❌ **MUST NOT** import screen-level presentational components.

Think of contexts as **“feature state providers”**, not UI.

---

### Container layer (view-model / binding)

**Location**

- `src/containers/<Feature>/<Feature>.tsx`

**Responsibilities**

- Consume the feature context via `useFeatureContext()`.
- Perform **feature-specific mapping** from context data to the **page’s presentation props**.
- Hold **local UI state** that is:
  - Specific to this screen/view
  - Not shared with other features
  - Not worth lifting to a shared context (e.g. active tab, local filters, open/close state)
- Assemble and pass a **single, well-typed props object** down into the page-level presentational component.

**Rules**

- ✅ **MUST** be the place where context is bound to the page’s presentation props.
- ✅ **MAY** compute derived values from context data that only this screen needs.
- ✅ **MAY** own local `useState`/`useReducer` for UI concerns.
- ❌ **MUST NOT** call external APIs or TanStack Query directly.
- ❌ **MUST NOT** create new React contexts (those belong in `src/contexts`).
- ❌ **SHOULD NOT** directly import low-level data/access libraries (Supabase clients, HTTP clients, etc.).

Think of containers as **“adapters”** between the context value and the page/presentation props.

---

Page layer (routing entry / composition root)

**Location**

- `src/pages/<Feature>.tsx` (or the equivalent in your router structure)

**Responsibilities**

- Serve as the **route entry point** for the feature.
- Wire together:
  - Context provider(s) for this feature
  - Feature container
- Optionally include **lightweight page-level wrappers**:
  - Layout components
  - Head metadata
  - Route guards

Example pattern (conceptual):

```tsx
// src/pages/Feature.tsx
export default function FeaturePage() {
  return (
    <FeatureContextProvider>
      <FeatureContainer />
    </FeatureContextProvider>
  );
}
```

Rules

- ✅ **SHOULD** be very thin; ideally just providers + container.
- ❌ **MUST NOT** contain business logic, queries, or heavy state.
- ❌ **MUST NOT** be the place where props for presentational components are manually built – that’s the container’s job.

Think of pages as “router glue”.

---

### Presentational components (pure UI)

**Location**

- `src/components/<feature>/...`  
  (e.g. `src/components/dashboard/Header.tsx`, `src/components/dashboard/TopRow.tsx`, etc.)

**Responsibilities**

- Receive **plain props** and render JSX.
- Compose other presentational components.
- Be **framework-agnostic** to the rest of the app:
  - No direct context usage for feature state.
  - No external I/O.
- Be suitable for:
  - **Unit tests** using plain props
  - **Storybook stories** using mock data

**Rules**

- ✅ **MUST** have explicit, exported prop types/interfaces.
- ✅ **MUST** avoid reading feature context directly (except for truly global UI, like theme).
- ✅ **MAY** hold transient UI state (hover, open/closed) that does not affect business logic.
- ❌ **MUST NOT** call TanStack Query or other data sources.
- ❌ **MUST NOT** know about app-level routing or feature containers.

Think of presentation components as **“render functions with props”**.

---

### Feature “view props” contract

Each screen should define a **single, typed “view props” object** that contains everything required to render the page-level presentation.

For a given feature `<Feature>`, define:

- `FeatureViewProps` — the canonical, stable contract for that screen.

This type:

- Drives the page-level presentation component.
- Serves as the Storybook and unit test interface.
- Is the target shape produced by the context + container pipeline.

#### Pattern

1. Define `FeatureViewProps` next to the page-level presentation component  
   (e.g. `src/components/<feature>/<Feature>View.tsx`).

2. The **container** constructs a `FeatureViewProps` object from `useFeatureContext()`.

3. The **context** exposes:
   - Values that map directly into `FeatureViewProps`, or
   - Richer internal data from which the container derives `FeatureViewProps`.

#### Rules

- `FeatureViewProps` should remain stable; changes indicate intentional UI API changes.
- Structure `FeatureViewProps` into nested sub-props (e.g. `header`, `summary`, `metrics`, `list`, etc.).
- Export reusable sub-prop interfaces for child components.
- Do **not** surface raw API data here if it should be normalized earlier (context or container).

### Storybook and testing

This architecture makes testing and Storybook usage predictable and low-friction.

#### Page-level presentation

- Storybook stories should instantiate `FeatureViewProps` with hard-coded mock data.
- Unit tests should render the page-level presentation component with a direct `FeatureViewProps` object.

#### Child presentational components

- Each child component exports its own `Props` interface.
- Tests and stories should:
  - Pass props directly,
  - Avoid mocking feature context or network layers.

#### Context & container

- **Context tests:**

  - Mock TanStack Query results,
  - Assert that exposed values match the expected context shape and derived data.

- **Container tests:**
  - Wrap with a mock `FeatureContext.Provider`,
  - Verify that the container maps context → `FeatureViewProps` correctly.
