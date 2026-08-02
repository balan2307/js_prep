# React vs Angular Notes

## 📚 Library vs Framework

### Library
A **library** is a collection of reusable code that solves a specific problem. Your application decides **when and how** to use it.

**Characteristics**
- Your application **calls the library**.
- You control the application's architecture.
- You choose the supporting libraries.
- Maximum flexibility.
- Few or no enforced conventions.

**Examples:** React, Axios, Lodash

---

### Framework
A **framework** provides the overall structure for building an application. It defines how different parts of the application should be organized and interact.

**Characteristics**
- The **framework calls your code** (Inversion of Control).
- Provides built-in solutions for common problems.
- Enforces conventions and best practices.
- Better consistency across large teams.

For example, Angular already provides conventions for:

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

React does **not** enforce any project structure.

For example, both of these are perfectly valid:

```text
src/
 ├── components/
 ├── hooks/
 ├── pages/
 └── utils/
```

or

```text
src/
 ├── features/
 │   ├── meeting/
 │   ├── chat/
 │   └── auth/
```

React gives developers the freedom to organize the project in whatever way best suits the application.

> **Key Difference:** A framework is **opinionated** and encourages a standard way of building applications, whereas a library gives developers the flexibility to decide the architecture.

---

# ⚛️ React vs Angular

| React | Angular |
|--------|----------|
| UI Library | Full Framework |
| Developed by Meta | Developed by Google |
| Uses JSX | Uses TypeScript & Templates |
| Virtual DOM + Fiber | Change Detection |
| One-way Data Flow | Supports Two-way Data Binding |
| Third-party routing/state management | Built-in Routing, Forms, HTTP, DI |
| Flexible | Opinionated |
| Easier learning curve | Steeper learning curve |

---

# ✅ When to Use React

Choose React when:

- Building highly interactive SPAs.
- You need flexibility in choosing libraries.
- Your team already has React expertise.
- You are building reusable UI components or SDKs.
- You want a lightweight UI layer.
- Faster development and easier onboarding are priorities.

**Typical Stack**

- React
- React Router
- Zustand / Redux
- React Query
- Axios / Fetch

---

# ✅ When to Use Angular

Choose Angular when:

- Building large enterprise applications.
- Multiple teams need a standardized architecture.
- You want built-in Routing, Forms, HTTP Client, and Dependency Injection.
- Convention and maintainability are more important than flexibility.
- Your organization already uses Angular.

---

# 🚀 Why We Used React in Our Project (JioMeet)

JioMeet is a **real-time collaboration platform** with features like:

- Video conferencing
- Chat
- Screen sharing
- Participant management
- Network status
- Meeting controls

We chose **React** because:

- Component-based architecture made it easy to build reusable UI components like Participant Tile, Chat Panel, and Meeting Controls.
- We exposed our WebRTC SDK using **custom React Hooks** and reusable components, making integration simple for React applications.
- React gave us the flexibility to choose libraries based on our project requirements.
- React has a huge ecosystem, making development faster.
- Most frontend developers already know React, improving SDK adoption and reducing the learning curve.
- Our team already had strong React expertise, allowing faster development and easier maintenance.

> **Note:** Angular could also build the same application. We chose React not because Angular couldn't do it, but because React better matched our team's expertise, flexibility requirements, SDK integration strategy, and the ecosystem of our consumers.

---

# 💡 Interview Summary

### Library vs Framework

- **Library:** Your code calls the library.
- **Framework:** The framework calls your code.
- **Library:** Gives flexibility; you decide the architecture.
- **Framework:** Provides conventions and a predefined way to organize applications.

### React vs Angular

- **React** is a UI library focused on rendering the UI and lets developers choose the rest of the technology stack.
- **Angular** is a complete framework that provides routing, forms, dependency injection, HTTP client, CLI, and a standardized project structure.

### Why React?

- Flexible architecture.
- Component-based design.
- Excellent developer experience.
- Huge ecosystem.
- Easy SDK integration.
- Strong community support.
- Faster onboarding for developers.
