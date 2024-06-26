### Components (20 mins)

#### Detailed Overview of Components

**What are Components?**
- **Definition**: Components are the fundamental building blocks of Angular applications. They encapsulate the template, styles, and logic.
- **Purpose**: Components control views (HTML) displayed in the application.

**Creating a Component**
- **Using Angular CLI**: The easiest way to create a component is using Angular CLI.
  - Command: `ng generate component component-name` or `ng g c component-name`
  - Example:
    ```bash
    ng generate component example
    ```
  - This command generates:
    - `example.component.ts`: The TypeScript file for the component logic.
    - `example.component.html`: The HTML template file.
    - `example.component.css`: The CSS file for component-specific styles.
    - `example.component.spec.ts`: The test file for the component.

**Component Structure**
- **TypeScript Class**: Contains the logic for the component.
  - Example:
    ```typescript
    import { Component, OnInit } from '@angular/core';

    @Component({
      selector: 'app-example',
      templateUrl: './example.component.html',
      styleUrls: ['./example.component.css']
    })
    export class ExampleComponent implements OnInit {
      message: string;

      constructor() {
        this.message = 'Hello, Angular!';
      }

      ngOnInit(): void {}
    }
    ```
- **HTML Template**: Defines the view for the component.
  - Example:
    ```html
    <div>
      {{ message }}
    </div>
    ```
- **CSS File**: Contains styles specific to the component.
  - Example:
    ```css
    div {
      color: blue;
    }
    ```

**Component Metadata**
- **Selector**: Defines the HTML tag for the component.
- **TemplateUrl**: Points to the HTML template file.
- **StyleUrls**: Points to the CSS file(s).

**Data Binding**
- **Interpolation**: Used to display data from the component.
  - Syntax: `{{ data }}` 
  - Example: `<p>{{ message }}</p>`
- **Property Binding**: Binds data from the component to the template.
  - Syntax: `[property]="value"`
  - Example: `<input [value]="message">`
- **Event Binding**: Handles events in the template and triggers methods in the component.
  - Syntax: `(event)="handler"`
  - Example: `<button (click)="handleClick()">Click Me</button>`
  - Handling the event in the component:
    ```typescript
    handleClick() {
      alert('Button clicked!');
    }
    ```

**Directives**
- **Structural Directives**: Change the structure of the DOM.
  - `*ngIf`: Conditionally includes a template based on a boolean expression.
    ```html
    <div *ngIf="isVisible">This is visible</div>
    ```
  - `*ngFor`: Iterates over a collection.
    ```html
    <ul>
      <li *ngFor="let item of items">{{ item }}</li>
    </ul>
    ```
- **Attribute Directives**: Change the appearance or behavior of an element.
  - `ngClass`: Adds and removes CSS classes.
    ```html
    <div [ngClass]="{ 'active': isActive }">Content</div>
    ```

**Lifecycle Hooks**
- **ngOnInit**: Called once the component is initialized.
- **ngOnChanges**: Called when an input property changes.
- **ngOnDestroy**: Called just before the component is destroyed.
  - Example:
    ```typescript
    import { Component, OnInit, OnDestroy } from '@angular/core';

    @Component({
      selector: 'app-lifecycle',
      template: '<p>Lifecycle hooks</p>'
    })
    export class LifecycleComponent implements OnInit, OnDestroy {
      ngOnInit() {
        console.log('Component initialized');
      }

      ngOnDestroy() {
        console.log('Component destroyed');
      }
    }
    ```

**Input and Output Properties**
- **Input Properties**: Allow data to be passed into a component.
  - Example:
    ```typescript
    import { Component, Input } from '@angular/core';

    @Component({
      selector: 'app-child',
      template: '<p>{{ childMessage }}</p>'
    })
    export class ChildComponent {
      @Input() childMessage: string;
    }
    ```
- **Output Properties**: Emit events from a child component to the parent component.
  - Example:
    ```typescript
    import { Component, Output, EventEmitter } from '@angular/core';

    @Component({
      selector: 'app-child',
      template: '<button (click)="sendMessage()">Send Message</button>'
    })
    export class ChildComponent {
      @Output() messageEvent = new EventEmitter<string>();

      sendMessage() {
        this.messageEvent.emit('Hello Parent!');
      }
    }
    ```

### Practical Example:
1. **Creating a Parent and Child Component**:
   - Create `ParentComponent` and `ChildComponent` using Angular CLI:
     ```bash
     ng generate component parent
     ng generate component child
     ```
2. **Parent Component Template**:
   - `parent.component.html`:
     ```html
     <app-child (messageEvent)="receiveMessage($event)"></app-child>
     <p>Message from Child: {{ message }}</p>
     ```
3. **Parent Component Class**:
   - `parent.component.ts`:
     ```typescript
     import { Component } from '@angular/core';

     @Component({
       selector: 'app-parent',
       templateUrl: './parent.component.html',
       styleUrls: ['./parent.component.css']
     })
     export class ParentComponent {
       message: string;

       receiveMessage($event: string) {
         this.message = $event;
       }
     }
     ```
4. **Child Component Template**:
   - `child.component.html`:
     ```html
     <button (click)="sendMessage()">Send Message</button>
     ```
5. **Child Component Class**:
   - `child.component.ts`:
     ```typescript
     import { Component, Output, EventEmitter } from '@angular/core';

     @Component({
       selector: 'app-child',
       templateUrl: './child.component.html',
       styleUrls: ['./child.component.css']
     })
     export class ChildComponent {
       @Output() messageEvent = new EventEmitter<string>();

       sendMessage() {
         this.messageEvent.emit('Hello Parent!');
       }
     }
     ```

This detailed overview covers all the key aspects of Angular components and their practical usage, providing a comprehensive understanding of how to work with components in Angular.