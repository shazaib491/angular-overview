### Services and Dependency Injection (15 mins)

#### What are Services in Angular?
Services are used to share data or logic across different parts of an application. They are typically used for tasks such as fetching data from a server, logging, user authentication, and other business logic. Services are a fundamental aspect of Angular applications, enabling code reusability and separation of concerns.

**Key Characteristics of Services:**
- Singleton by default: Angular creates only one instance of a service for the entire application.
- Injectable: Services can be injected into components or other services using Angular's Dependency Injection (DI) system.

#### Creating a Service
Services are typically created using Angular CLI to ensure proper setup and boilerplate code.

**Using Angular CLI:**
```bash
ng generate service example
```
or
```bash
ng g s example
```
This command creates a service file (`example.service.ts`) and a corresponding test file (`example.service.spec.ts`).

**Example: `example.service.ts`**
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class ExampleService {
  constructor() { }

  getData(): string {
    return 'Hello from Example Service!';
  }
}
```
- **@Injectable**: This decorator marks the class as a service that can be injected. The providedIn: 'root' metadata makes the service available application-wide as a singleton.

#### Dependency Injection (DI)
Dependency Injection is a design pattern used to implement IoC (Inversion of Control). It allows a class to receive its dependencies from an external source rather than creating them itself. Angular's DI system is hierarchical, providing flexibility and modularity in the way dependencies are managed and injected.

**Key Concepts:**
- **Injector**: The DI system uses an injector to create and manage dependencies.
- **Provider**: A provider tells the injector how to create the service.

**Types of Providers:**
- **Class Provider**: The most common provider, used for services created with `@Injectable`.
- **Value Provider**: Provides a static value.
- **Factory Provider**: Provides a factory function that returns the dependency.

**Example: Injecting a Service into a Component**

**Component Code:**
```typescript
// example.component.ts
import { Component, OnInit } from '@angular/core';
import { ExampleService } from './example.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent implements OnInit {
  data: string;

  constructor(private exampleService: ExampleService) { }

  ngOnInit(): void {
    this.data = this.exampleService.getData();
  }
}
```
- **Constructor Injection**: The `ExampleService` is injected into the `ExampleComponent` via the constructor. Angular's DI system handles the instantiation and provision of the service instance.

**Component Template:**
```html
<!-- example.component.html -->
<p>{{ data }}</p>
```
- The `data` property, which is populated using the `ExampleService`, is displayed in the template.

#### Hierarchical Dependency Injection
Angular's DI system is hierarchical. Services can be registered at different levels:
- **Root Level**: Services provided in the root injector (`providedIn: 'root'`) are singleton and available throughout the application.
- **Module Level**: Services provided in a specific module injector are available only within that module and its child modules.
- **Component Level**: Services provided in a component's injector are available only within that component and its child components.

**Example: Module-Level Providers**
```typescript
// example.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ExampleComponent } from './example.component';
import { ExampleService } from './example.service';

@NgModule({
  declarations: [ExampleComponent],
  imports: [CommonModule],
  providers: [ExampleService] // Service provided at the module level
})
export class ExampleModule { }
```
- The `ExampleService` is provided only to the `ExampleModule` and its components.

**Example: Component-Level Providers**
```typescript
// example.component.ts
import { Component, OnInit } from '@angular/core';
import { ExampleService } from './example.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css'],
  providers: [ExampleService] // Service provided at the component level
})
export class ExampleComponent implements OnInit {
  data: string;

  constructor(private exampleService: ExampleService) { }

  ngOnInit(): void {
    this.data = this.exampleService.getData();
  }
}
```
- The `ExampleService` is provided only to the `ExampleComponent` and its child components.

#### Using Services for HTTP Requests
One of the most common uses of services in Angular is to handle HTTP requests to fetch or send data to a backend server.

**Example: `http-client.service.ts`**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class HttpClientService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) { }

  fetchData(): Observable<any> {
    return this.http.get<any>(this.apiUrl);
  }
}
```
- **HttpClient**: Angular's `HttpClient` service is used to make HTTP requests.
- **Observable**: The `HttpClient` methods return RxJS observables, which are used to handle asynchronous data streams.

**Injecting HttpClientService into a Component**
```typescript
// data.component.ts
import { Component, OnInit } from '@angular/core';
import { HttpClientService } from './http-client.service';

@Component({
  selector: 'app-data',
  templateUrl: './data.component.html',
  styleUrls: ['./data.component.css']
})
export class DataComponent implements OnInit {
  data: any;

  constructor(private httpClientService: HttpClientService) { }

  ngOnInit(): void {
    this.httpClientService.fetchData().subscribe(
      response => this.data = response,
      error => console.error(error)
    );
  }
}
```
- The `HttpClientService` is injected into the `DataComponent`, and its `fetchData` method is called to fetch data from the API.

#### Summary
- **Services**: Used to encapsulate and share data or logic across different parts of an Angular application.
- **Dependency Injection**: Design pattern used to manage dependencies and promote loose coupling.
- **Providers**: Configure how a service is created and managed by the DI system.
- **Hierarchical DI**: Allows services to be scoped at different levels (root, module, component).
- **HttpClient**: Angular's service for making HTTP requests, often used within services to interact with backend APIs.

By leveraging services and Angular's powerful DI system, developers can create modular, reusable, and maintainable code, ensuring a clear separation of concerns and efficient management of application logic.