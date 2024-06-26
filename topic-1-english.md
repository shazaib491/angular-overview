### Introduction to Angular and Project Structure (10 mins)

#### What is Angular?
Angular is a platform and framework for building single-page client applications using HTML and TypeScript. Initially, there was AngularJS, released in 2010. Angular 2+ (a completely different framework) was launched in 2016. There are significant differences between AngularJS and Angular 2+, such as modular architecture, improved performance, and modern development practices.

**Core Features of Angular**
1. **Component-based Architecture**: Angular applications are divided into small components. Each component has its own HTML, CSS, and TypeScript class.
2. **Dependency Injection (DI)**: Angular uses Dependency Injection to efficiently manage object creation and dependencies.
3. **Reactive Programming with RxJS**: Angular uses the RxJS library for reactive programming, which helps handle asynchronous operations.

**Benefits of Using Angular**
- **Modular Development**: Allows breaking the application into smaller modules, making development and maintenance easier.
- **Performance**: Uses techniques like ahead-of-time (AOT) compilation and tree-shaking to make the application fast and efficient.
- **Community and Support**: Strong community and Google support make it a robust and reliable framework.

#### Setting Up Angular Project and Folder Structure

**Setting Up Angular Project**
1. **Install Node.js and npm**:
   - Install Node.js from the [official website](https://nodejs.org/). npm comes bundled with Node.js.
2. **Install Angular CLI**:
   - Install Angular CLI globally:
     ```bash
     npm install -g @angular/cli
     ```
3. **Create a New Angular Project**:
   - Create a new project using Angular CLI:
     ```bash
     ng new my-angular-app
     cd my-angular-app
     ```
4. **Run the Angular Application**:
   - Start the Angular development server:
     ```bash
     ng serve
     ```
   - Open `http://localhost:4200` in a browser to see the running application.

**Angular Project Structure**
To understand the Angular project structure, let's explain the folders and their contents in a table format:

| Folder/File        | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **e2e/**           | End-to-end testing related files.                                            |
| **node_modules/**  | Project dependencies installed through npm.                                  |
| **src/**           | Source code for the actual application.                                      |
| ├── **app/**       | Main application module and components.                                      |
| │   ├── `app.component.ts`  | Main app component TypeScript file.                               |
| │   ├── `app.component.html`| Main app component HTML file.                                      |
| │   ├── `app.component.css` | Main app component CSS file.                                       |
| │   └── `app.module.ts`     | Main application module file.                                      |
| ├── **assets/**    | Static files like images and styles.                                         |
| ├── **environments/** | Environment-specific configurations (development, production, etc.).    |
| │   ├── `environment.ts` | Development environment configuration.                               |
| │   └── `environment.prod.ts` | Production environment configuration.                         |
| ├── `main.ts`      | Application entry point that bootstraps Angular.                              |
| ├── `index.html`   | Main HTML file that hosts the application.                                    |
| ├── `styles.css`   | Global styles used in the application.                                        |
| └── `polyfills.ts` | Necessary scripts for older browser compatibility.                           |
| **angular.json**   | Angular CLI configuration file.                                               |
| **package.json**   | Project metadata and dependencies information.                                |
| **tsconfig.json**  | TypeScript configuration file.                                                |
| **tslint.json**    | Linting rules for TypeScript.                                                 |

#### Practical Examples

**Component Creation Example**:
Generate a component using Angular CLI:
```bash
ng generate component example
```
This command creates the following files in the `src/app/example/` folder:
- `example.component.ts`
- `example.component.html`
- `example.component.css`
- `example.component.spec.ts`

**Service Creation Example**:
Generate a service using Angular CLI:
```bash
ng generate service example
```
This command creates the `src/app/example.service.ts` file.

**Component Code Example**:
```typescript
// example.component.ts
import { Component, OnInit } from '@angular/core';
import { ExampleService } from '../example.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent implements OnInit {
  data: string;
  constructor(private exampleService: ExampleService) {}
  ngOnInit() {
    this.data = this.exampleService.getData();
  }
}
```

**Service Code Example**:
```typescript
// example.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class ExampleService {
  constructor() {}
  getData() {
    return 'Hello from Example Service!';
  }
}
```

This introduction session provides students with a basic understanding of Angular and a detailed overview of the project structure, enabling them to develop their applications easily.
