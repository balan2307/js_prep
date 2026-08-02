# React vs Angular Notes

## 📚 Library vs Framework

### Library
- Solves a **specific problem**.
- **Your application calls the library.**
- You decide the application architecture and choose supporting libraries.
- Offers maximum flexibility.

**Examples:** React, Axios, Lodash

### Framework
- Provides the **complete application structure**.
- **The framework calls your code** (Inversion of Control).
- Comes with built-in features and conventions.
- Offers consistency and standardization.

**Examples:** Angular, Spring Boot, Django

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

> **Note:** Angular could also build the same application. We chose React not because Angular couldn't do it, but because React better matched our team's expertise, our SDK integration strategy, flexibility requirements, and the ecosystem of our consumers.

---

# 💡 Interview Summary

### Library vs Framework

- **Library:** Your code calls the library.
- **Framework:** The framework calls your code.

### React vs Angular

- **React** is a UI library that lets developers choose the rest of the stack.
- **Angular** is a complete framework with built-in routing, forms, HTTP, dependency injection, and application structure.

### Why React?

- Flexible
- Component-based
- Excellent developer experience
- Huge ecosystem
- Easy SDK integration
- Strong community support
- Faster onboarding for developers
