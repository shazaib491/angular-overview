### Introduction to Angular and Project Structure (10 mins)

#### What is Angular?
Angular ek platform aur framework hai jo single-page client applications banane ke liye use hota hai using HTML aur TypeScript. Pehle AngularJS tha, jo 2010 me release hua tha. Angular 2+ (jo totally different framework hai) 2016 me launch hua. AngularJS aur Angular 2+ me kaafi differences hain, jaise modular architecture, improved performance, aur modern development practices.

**Core Features of Angular**
1. **Component-based Architecture**: Angular applications ko chhote components me divide kiya jata hai. Har component ka apna HTML, CSS aur TypeScript class hota hai.
2. **Dependency Injection (DI)**: Angular me Dependency Injection ka use hota hai to efficiently manage object creation and dependencies.
3. **Reactive Programming with RxJS**: Angular reactive programming ke liye RxJS library ka use karta hai, jo asynchronous operations handle karne me madad karti hai.

**Benefits of Using Angular**
- **Modular Development**: Application ko chhote modules me divide karne ki suvidha milti hai, jo ki development aur maintenance ko easy banata hai.
- **Performance**: Angular me ahead-of-time (AOT) compilation aur tree-shaking jaisi techniques use hoti hain jo application ko fast aur efficient banati hain.
- **Community and Support**: Angular ki strong community aur Google ka support milta hai, jo ki isse robust aur reliable framework banata hai.

#### Setting Up Angular Project and Folder Structure

**Setting Up Angular Project**
1. **Install Node.js and npm**:
   - Node.js ko [official website](https://nodejs.org/) se install karein. Node.js ke sath npm bhi install ho jata hai.
2. **Install Angular CLI**:
   - Angular CLI ko globally install karein:
     ```bash
     npm install -g @angular/cli
     ```
3. **Create a New Angular Project**:
   - Angular CLI use karke ek naya project create karein:
     ```bash
     ng new my-angular-app
     cd my-angular-app
     ```
4. **Run the Angular Application**:
   - Angular development server ko start karein:
     ```bash
     ng serve
     ```
   - Browser me jaake `http://localhost:4200` open karein to dekhein ki application chal rahi hai.

**Angular Project Structure**
Angular project structure ko samajhne ke liye, hum folders aur unke contents ko table ke format me explain karte hain:

| Folder/File        | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **e2e/**           | End-to-end testing related files.                                            |
| **node_modules/**  | Project dependencies jo npm install ke through install hoti hain.            |
| **src/**           | Source code jo actual application banane ke liye use hota hai.               |
| ├── **app/**       | Main application module aur components yahan hote hain.                      |
| │   ├── `app.component.ts`  | Main app component TypeScript file.                               |
| │   ├── `app.component.html`| Main app component HTML file.                                      |
| │   ├── `app.component.css` | Main app component CSS file.                                       |
| │   └── `app.module.ts`     | Main application module file.                                      |
| ├── **assets/**    | Static files jaise images aur styles.                                        |
| ├── **environments/** | Environment-specific configurations (development, production, etc.).    |
| │   ├── `environment.ts` | Development environment configuration.                               |
| │   └── `environment.prod.ts` | Production environment configuration.                         |
| ├── `main.ts`      | Application entry point jo Angular ko bootstrap karta hai.                   |
| ├── `index.html`   | Main HTML file jo application ko host karti hai.                             |
| ├── `styles.css`   | Global styles jo application me use hoti hain.                               |
| └── `polyfills.ts` | Older browsers ke compatibility ke liye necessary scripts.                   |
| **angular.json**   | Angular CLI configuration file.                                              |
| **package.json**   | Project metadata aur dependencies ki information.                            |
| **tsconfig.json**  | TypeScript configuration file.                                               |
| **tslint.json**    | Linting rules for TypeScript.                                                |

#### Practical Examples

**Component Creation Example**:
Angular CLI ka use karke ek component generate karte hain:
```bash
ng generate component example
```
Is command se `src/app/example/` folder me following files create hoti hain:
- `example.component.ts`
- `example.component.html`
- `example.component.css`
- `example.component.spec.ts`

**Service Creation Example**:
Angular CLI ka use karke ek service generate karte hain:
```bash
ng generate service example
```
Is command se `src/app/example.service.ts` file create hoti hai.

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

Is introduction session se students ko Angular ka basic understanding aur project structure ka detailed overview mil jayega, jisse wo apni applications easily develop kar paayenge.
