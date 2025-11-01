---

````markdown
# SOLID Principles in Software Design  
### Building Scalable, Maintainable, and Flexible Applications with Clean Code  

**Author:** Albin Aji  
**Date:** September 17, 2025  

---

## 🧠 Introduction

The **SOLID principles** are the foundation of clean, scalable, and maintainable software design.  
When combined with **Clean Architecture**, they help developers build applications that are modular, testable, and easy to extend.

---

## 1. Single Responsibility Principle (SRP)

> **A class should have only one reason to change.**  
Each module, class, or function should focus on a *single responsibility*.

**❌ Bad design — too many roles mixed:**
```ts
class Student {
  createStudentAccount() { /* logic */ }
  calculateStudentGrade() { /* logic */ }
  generateStudentData() { /* logic */ }
}
````

**✅ Better design — one class, one job:**

```ts
class StudentAccount {
  createStudentAccount() { /* logic */ }
}
class StudentGrade {
  calculateStudentGrade() { /* logic */ }
}
class StudentData {
  generateStudentData() { /* logic */ }
}
```

**Clean Architecture tie-in:**
SRP ensures that entities and use cases are separated, making each class easy to test, update, or reuse.

---

## 2. Open/Closed Principle (OCP)

> **Software entities should be open for extension but closed for modification.**

**❌ Bad design — new shapes require editing old code:**

```ts
function computeAreasOfShapes(shapes) {
  return shapes.reduce((area, shape) => {
    if (shape instanceof Rectangle) return area + shape.width * shape.height;
    if (shape instanceof Triangle) return area + shape.base * shape.height * 0.5;
    if (shape instanceof Circle) return area + shape.radius * Math.PI;
    return area;
  }, 0);
}
```

**✅ Better design — extend without changing:**

```ts
interface Shape {
  getArea(): number;
}

class Rectangle implements Shape {
  constructor(public width: number, public height: number) {}
  getArea() { return this.width * this.height; }
}

class Triangle implements Shape {
  constructor(public base: number, public height: number) {}
  getArea() { return this.base * this.height * 0.5; }
}

function computeAreasOfShapes(shapes: Shape[]) {
  return shapes.reduce((area, shape) => area + shape.getArea(), 0);
}
```

**Clean Architecture tie-in:**
By depending on abstractions, new features can be added without modifying existing core logic.

---

## 3. Liskov Substitution Principle (LSP)

> **Subtypes should be fully substitutable for their base types.**

**❌ Violation — subclass breaks expected behavior:**

```ts
class Rectangle {
  setWidth(width: number) { this.width = width; }
  setHeight(height: number) { this.height = height; }
  getArea() { return this.width * this.height; }
}

class Square extends Rectangle {
  setWidth(width: number) { this.width = width; this.height = width; }
  setHeight(height: number) { this.width = height; this.height = height; }
}
```

Although a square *is* a rectangle mathematically, substituting one for another in code causes unexpected behavior — violating LSP.

**Clean Architecture tie-in:**
LSP ensures implementations can be swapped freely — for example, replacing one email provider with another without changing business logic.

---

## 4. Interface Segregation Principle (ISP)

> **Clients should not be forced to implement methods they do not use.**

**❌ Overloaded interface:**

```ts
interface ShapeInterface {
  calculateArea(): number;
  calculateVolume(): number;
}

class Square implements ShapeInterface {
  calculateArea() { /* ... */ }
  calculateVolume() { throw new Error("Not supported"); }
}
```

**✅ Segregated, focused interfaces:**

```ts
interface Area {
  calculateArea(): number;
}
interface Volume {
  calculateVolume(): number;
}

class Square implements Area {
  calculateArea() { /* ... */ }
}
class Cylinder implements Area, Volume {
  calculateArea() { /* ... */ }
  calculateVolume() { /* ... */ }
}
```

**Clean Architecture tie-in:**
Focused interfaces like `IUserRepository` and `IEmailService` keep your contracts small and avoid unnecessary dependencies.

---

## 5. Dependency Inversion Principle (DIP)

> **High-level modules should not depend on low-level modules.**
> Both should depend on abstractions.

**❌ Bad design — direct dependency:**

```ts
class OrderService {
  constructor(private database: MySQLDatabase) {}
  create(order: Order) { this.database.create(order); }
}
```

**✅ Better design — depend on abstraction:**

```ts
interface Database {
  create(order: Order): void;
  update(order: Order): void;
}

class MySQLDatabase implements Database {
  create(order: Order) { /* MySQL logic */ }
  update(order: Order) { /* MySQL logic */ }
}

class OrderService {
  constructor(private database: Database) {}
  create(order: Order) { this.database.create(order); }
}
```

**Clean Architecture tie-in:**
This allows switching databases (PostgreSQL, MongoDB, Firebase, etc.) without changing `OrderService`.
Your business logic remains independent of infrastructure.

---

## 🧩 Wrapping It All Up

When combined with **Clean Architecture**, the SOLID principles help you build modular, testable, and future-proof software.

| Principle   | Core Idea                                   | Benefit                                 |
| ----------- | ------------------------------------------- | --------------------------------------- |
| **S — SRP** | One class, one responsibility               | Focused and testable code               |
| **O — OCP** | Open for extension, closed for modification | Easier to extend without refactoring    |
| **L — LSP** | Subtypes replace base types safely          | Reusable and interchangeable components |
| **I — ISP** | Small, specific interfaces                  | Avoids bloated and brittle contracts    |
| **D — DIP** | Depend on abstractions, not concretions     | Decouples logic from frameworks         |

---

## 🧱 Final Thoughts

The SOLID principles are not just theoretical — they are **practical guidelines** for writing clean, extensible, and maintainable software.
When applied consistently within **Clean Architecture**, they lead to systems that are:

* Modular
* Easy to test
* Flexible to change
* Independent of frameworks and external tools

> **In short:** SOLID + Clean Architecture = Clean, Scalable, and Sustainable Code ✅

---

### 📚 References

* *Robert C. Martin (Uncle Bob)* — *Agile Software Development: Principles, Patterns, and Practices*
* *Clean Architecture* — *A Craftsman’s Guide to Software Structure and Design*

---

```