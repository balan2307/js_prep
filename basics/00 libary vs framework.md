# 📚 Library vs Framework

## Library

A **library** is a collection of reusable code that solves a specific problem. Your application decides **when and how** to use it.

### Characteristics

- Your application **calls the library**.
- You control the application's architecture.
- You decide the folder structure.
- You choose the supporting libraries.
- Maximum flexibility.
- Few or no enforced conventions.

### How React Works

When using React, **your application starts React**.

```jsx
function App() {
  return <h1>Hello World</h1>;
}

ReactDOM.createRoot(root).render(<App />);
```

Here, **your code** explicitly calls:

```js
ReactDOM.createRoot(root).render(<App />);
```

React then renders your components.

Even though React internally calls your component:

```js
App();
```

it only does so to determine **what UI should be rendered**.

React **does not control your entire application**.

It does **not** decide:

- Which router you use
- Which state management library you use
- Which HTTP client you use
- How your folders should be organized
- How authentication is implemented

Those decisions are left to the developer.

> **Think of React as an employee.** You hire React to build the UI, but you decide everything else.

---

## Framework

A **framework** provides the complete structure for building an application. Instead of your application controlling everything, the framework controls the application's lifecycle.

### Characteristics

- The **framework calls your code** (Inversion of Control).
- Provides built-in solutions.
- Enforces conventions.
- Standard project structure.
- Better consistency across large teams.

Angular already provides conventions for:

- Components
- Services
- Routing
- Dependency Injection
- Forms
- HTTP Client
- Guards
- Pipes
- Interceptors

Typical Angular project structure:

```text
src/
 ├── app/
 │   ├── components/
 │   ├── services/
 │   ├── guards/
 │   ├── pipes/
 │   ├── interceptors/
 │   └── app-routing.module.ts
```

### How Angular Works

Angular bootstraps the application.

When you write:

```ts
@Component({
  selector: 'app-home'
})
export class HomeComponent {

  ngOnInit() {
    console.log('Component initialized');
  }

}
```

You **never call**:

```ts
ngOnInit();
```

Angular automatically:

- Creates the component.
- Creates services.
- Injects dependencies.
- Calls lifecycle hooks.
- Manages routing.
- Handles forms.
- Starts change detection.

Your application **lives inside Angular's lifecycle**.

> **Think of Angular as a construction company.** The company already has a process—you simply plug your code into that process.

---

## React vs Angular (Who Controls the Flow?)

### React (Library)

```text
Your Application
        │
        ▼
Start React
        │
        ▼
React renders App()
        │
        ▼
UI Rendered
```

Your application is in control.

---

### Angular (Framework)

```text
Angular Bootstraps
        │
        ▼
Creates Components
        │
        ▼
Creates Services
        │
        ▼
Dependency Injection
        │
        ▼
Calls ngOnInit()
        │
        ▼
Handles Routing & Forms
```

Angular controls the application's lifecycle.

---

## Key Interview Takeaway

- **Library:** Your application calls the library to solve a specific problem. The library only handles the responsibility it was designed for.
- **Framework:** The framework controls the application's lifecycle and invokes your code at predefined points.

### One-line Interview Answer

> **React is a library because my application owns the architecture and only uses React to render the UI. Angular is a framework because it owns the application's lifecycle—it bootstraps the application, creates components and services, performs dependency injection, and invokes my code through lifecycle hooks like `ngOnInit()`.**
