- [**Angular Component Lifecycle Explained**](#angular-component-lifecycle-explained)
  - [**Component Lifecycle Hooks**](#component-lifecycle-hooks)
  - [**Detailed Explanation of Each Hook**](#detailed-explanation-of-each-hook)
    - [**1. `ngOnChanges()`**](#1-ngonchanges)
    - [**2. `ngOnInit()`**](#2-ngoninit)
    - [**3. `ngDoCheck()`**](#3-ngdocheck)
    - [**4. `ngAfterContentInit()`**](#4-ngaftercontentinit)
    - [**5. `ngAfterContentChecked()`**](#5-ngaftercontentchecked)
    - [**6. `ngAfterViewInit()`**](#6-ngafterviewinit)
    - [**7. `ngAfterViewChecked()`**](#7-ngafterviewchecked)
    - [**8. `ngOnDestroy()`**](#8-ngondestroy)
  - [**Lifecycle Flow Order**](#lifecycle-flow-order)
  - [**Interview Question: Explain Component Lifecycle**](#interview-question-explain-component-lifecycle)
    - [**Sample Answer**](#sample-answer)
- [Directives](#directives)
    - [**1️⃣ Structural Directives**](#1️⃣-structural-directives)
      - [**Common Examples:**](#common-examples)
      - [**Key Characteristics:**](#key-characteristics)
    - [**2️⃣ Attribute Directives**](#2️⃣-attribute-directives)
      - [**Common Examples:**](#common-examples-1)
      - [**Key Characteristics:**](#key-characteristics-1)
    - [**3️⃣ Component Directives**](#3️⃣-component-directives)
      - [**Key Characteristics:**](#key-characteristics-2)
      - [**Example:**](#example)
      - [**Key Characteristics:**](#key-characteristics-3)
    - [**4️⃣ Custom Directives**](#4️⃣-custom-directives)
      - [**Creating a Custom Attribute Directive:**](#creating-a-custom-attribute-directive)
    - [**Key Points to Remember:**](#key-points-to-remember)

## template language 
square brackets indicate javascript:
```html
<app-greeting [message] = "homeMessage()" />
```

## Signals
They need to be called like methods
```typescript
<header>
    <nav>{{title()}}</nav>
</header>

export class HeaderComponent {
  title = signal('my first angular app');
}
```

Without signals (old/traditional) - no need of `()`:
```typescript
<header>
    <nav>{{title}}</nav>
</header>

export class HeaderComponent {
  title = signal('my first angular app');
}
```

Advice: start using Signals

## passing params from parent to child component
parent - import the child
```typescript
@Component({
  selector: 'app-home',
  imports: [GreetingComponent],
  templateUrl: './home.component.html',
  styleUrl: './home.component.scss'
})
export class HomeComponent {
  homeMessage = signal('home message');
}
```
parent - pass param
```typescript
// hardcoded
<app-greeting [message] = "'Hello world2'" />

// using signal
<app-greeting [message] = "homeMessage()" />
```

## event listeners

## Optimizing images (to-do)

Import the NgOptimizedImage directive
```typescript
import {NgOptimizedImage} from '@angular/common';

@Component({
    ...
    imports: [NgOptimizedImage]
});
```

Update the src attribute to be ngSrc

# **Angular Component Lifecycle Explained**  
Angular components go through a well-defined lifecycle, from creation to destruction. The lifecycle is controlled by **lifecycle hooks**, which allow you to run code at different stages of a component's existence.

---

## **Component Lifecycle Hooks**
Below is a breakdown of the key lifecycle hooks in order of execution:

| Hook | Triggered When? | Common Use Cases |
|------|---------------|------------------|
| `ngOnChanges()` | When input properties change | Detect input property changes |
| `ngOnInit()` | After the component is initialized | Fetch initial data, setup subscriptions |
| `ngDoCheck()` | During every change detection cycle | Detect and act on changes manually |
| `ngAfterContentInit()` | After `<ng-content>` is projected | Work with projected content |
| `ngAfterContentChecked()` | After projected content is checked | Respond to changes in content |
| `ngAfterViewInit()` | After child views (components) are initialized | Access and manipulate child components |
| `ngAfterViewChecked()` | After child views are checked | Handle updates after view changes |
| `ngOnDestroy()` | Before component is destroyed | Cleanup subscriptions, timers, and memory |

---

## **Detailed Explanation of Each Hook**
### **1. `ngOnChanges()`**
- Called when an `@Input()` property of the component changes.
- Runs **before** `ngOnInit()`.

```typescript
ngOnChanges(changes: SimpleChanges) {
  console.log('Input property changed:', changes);
}
```

✅ **Use Case:** Detect and react to changes in `@Input()` values.

---

### **2. `ngOnInit()`**
- Runs **once** after component initialization.
- Good for initializing data, making API calls.

```typescript
ngOnInit() {
  console.log('Component initialized');
  this.loadData();
}
```

✅ **Use Case:** Fetch initial data from an API.

---

### **3. `ngDoCheck()`**
- Called during every change detection cycle.
- Useful for manually detecting changes.

```typescript
ngDoCheck() {
  console.log('Change detection triggered');
}
```

✅ **Use Case:** Detect deep object changes when `ngOnChanges()` isn't sufficient.

---

### **4. `ngAfterContentInit()`**
- Runs **once** after content is projected into `<ng-content>`.

```typescript
ngAfterContentInit() {
  console.log('Projected content initialized');
}
```

✅ **Use Case:** Work with dynamically inserted content.

---

### **5. `ngAfterContentChecked()`**
- Called after every content projection change.

```typescript
ngAfterContentChecked() {
  console.log('Projected content checked');
}
```

✅ **Use Case:** Respond to updates in projected content.

---

### **6. `ngAfterViewInit()`**
- Runs once after **child components** are initialized.

```typescript
ngAfterViewInit() {
  console.log('Child views initialized');
}
```

✅ **Use Case:** Access child components with `@ViewChild()`.

---

### **7. `ngAfterViewChecked()`**
- Called after every view update.

```typescript
ngAfterViewChecked() {
  console.log('Child views checked');
}
```

✅ **Use Case:** Respond to view updates, avoid performance-heavy tasks here.

---

### **8. `ngOnDestroy()`**
- Called **before** the component is destroyed.
- Important for cleaning up **subscriptions, intervals, and event listeners**.

```typescript
ngOnDestroy() {
  console.log('Component destroyed');
  this.subscription.unsubscribe(); // Cleanup
}
```

✅ **Use Case:** Prevent memory leaks by unsubscribing from Observables.

---

## **Lifecycle Flow Order**
1. **Component Created**
   - `ngOnChanges()` (if inputs exist)
   - `ngOnInit()`
   - `ngDoCheck()`
2. **Content Projection**
   - `ngAfterContentInit()`
   - `ngAfterContentChecked()`
3. **Child Views Initialized**
   - `ngAfterViewInit()`
   - `ngAfterViewChecked()`
4. **Change Detection Runs (`ngDoCheck()`, `ngAfterViewChecked()`, etc.)**
5. **Component Destroyed**
   - `ngOnDestroy()`

---

## **Interview Question: Explain Component Lifecycle**
### **Sample Answer**
*"In Angular, a component goes through a series of lifecycle stages. When it is created, `ngOnInit()` runs for initialization, and `ngOnChanges()` detects changes in `@Input()` properties. Angular then checks for updates using `ngDoCheck()`, and handles projected content via `ngAfterContentInit()` and `ngAfterContentChecked()`. After child views are initialized, `ngAfterViewInit()` and `ngAfterViewChecked()` run. When the component is destroyed, `ngOnDestroy()` is called to clean up resources like subscriptions. These hooks help developers manage component behavior at different stages."*

---

# Directives
In Angular, **directives** are special markers on elements that tell Angular to do something to that DOM element (or its children). They are used to manipulate the DOM or behavior of elements in a declarative way.

Angular has three main types of directives:

---

### **1️⃣ Structural Directives**

Structural directives change the structure of the DOM by adding or removing elements. These directives are typically used to control visibility or create dynamic content based on conditions.

#### **Common Examples:**

- **`*ngIf`**
  - Conditionally adds or removes an element from the DOM.
  - If the expression evaluates to `true`, the element is added; otherwise, it is removed.
  ```html
  <div *ngIf="isLoggedIn">Welcome back!</div>
  ```

- **`*ngFor`**
  - Loops through an array and creates an element for each item in the array.
  ```html
  <ul>
    <li *ngFor="let item of items">{{ item }}</li>
  </ul>
  ```

- **`*ngSwitch`**
  - Conditionally displays one element from a set of elements based on a given expression.
  ```html
  <div [ngSwitch]="condition">
    <div *ngSwitchCase="'A'">Case A</div>
    <div *ngSwitchCase="'B'">Case B</div>
    <div *ngSwitchDefault>Default Case</div>
  </div>
  ```

#### **Key Characteristics:**
- Structural directives usually have an asterisk (`*`) prefix.
- They modify the layout of the DOM (i.e., adding/removing elements).
- Examples include `*ngIf`, `*ngFor`, `*ngSwitch`.

---

### **2️⃣ Attribute Directives**

Attribute directives modify the behavior or appearance of an existing element, component, or another directive. They do not change the structure of the DOM but rather adjust the attributes of DOM elements.

#### **Common Examples:**

- **`ngClass`**
  - Dynamically adds or removes CSS classes based on an expression.
  ```html
  <div [ngClass]="{'active': isActive}">This is a box</div>
  ```

- **`ngStyle`**
  - Dynamically updates inline styles for an element.
  ```html
  <div [ngStyle]="{'color': color, 'font-size': size + 'px'}">Styled Text</div>
  ```

- **`[disabled]`** (Angular's built-in attribute directive)
  - Disables or enables a button or input field based on a condition.
  ```html
  <button [disabled]="isDisabled">Click me</button>
  ```

- **`ngModel`**
  - Two-way binding for form elements.
  ```html
  <input [(ngModel)]="username" />
  ```

#### **Key Characteristics:**
- Attribute directives modify the behavior or appearance of an element.
- They are not used to add or remove elements from the DOM.
- Examples include `ngClass`, `ngStyle`, `ngModel`.

---

### **3️⃣ Component Directives**

Technically, components are directives with templates. They allow you to define custom, reusable UI elements with their own templates and styles.

#### **Key Characteristics:**
- A **component** directive is essentially a class with a template and styles attached to it.
- The difference from other directives is that a component has an associated template and a view.

#### **Example:**
- Creating a simple custom component directive:
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-my-component',
  template: `<div>Hello, this is a custom component!</div>`,
  styles: ['div { color: blue; }']
})
export class MyComponent {}
```
Usage:
```html
<app-my-component></app-my-component>
```

#### **Key Characteristics:**
- Components are always part of Angular's directive system but are used for more complex structures that include templates, styles, and logic.

---

### **4️⃣ Custom Directives**

You can also create **custom directives** for handling custom behaviors. These can be either **attribute** or **structural** directives, depending on what you want to achieve.

#### **Creating a Custom Attribute Directive:**

For example, let's create a directive that changes the background color of an element on hover:

```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHoverColor]'  // This will be used as an attribute on HTML elements
})
export class HoverColorDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.changeColor('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.changeColor('transparent');
  }

  private changeColor(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

Usage:
```html
<div appHoverColor>Hover over me to change my background color!</div>
```

---

### **Key Points to Remember:**

1. **Structural Directives**:
   - Used to change the structure of the DOM (e.g., `*ngIf`, `*ngFor`, `*ngSwitch`).
   - Can add or remove elements.

2. **Attribute Directives**:
   - Used to change the behavior or appearance of elements (e.g., `ngClass`, `ngStyle`, `ngModel`).
   - Do not modify the DOM structure, but modify properties or styles.

3. **Component Directives**:
   - Directives with associated templates and views. Essentially, components are a specialized type of directive.

4. **Custom Directives**:
   - You can create your own directives to add custom behavior, either as structural or attribute directives.

---

Would you like to see more examples or a detailed explanation of how to create custom directives? 😊