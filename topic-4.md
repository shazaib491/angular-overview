### Modules in Angular (10 mins)

#### What are Angular Modules?
Angular modules, or NgModules, are containers for a cohesive block of code dedicated to an application domain, a workflow, or a closely related set of capabilities. An Angular application is defined by a set of NgModules. 

**Purpose of NgModules:**
- To organize an application into cohesive blocks of functionality.
- To manage dependencies between different parts of the application.
- To facilitate lazy loading of modules to improve performance.

#### Key Concepts and Features

**1. Root Module:**
- The root module is the main module that Angular uses to bootstrap the application.
- It is conventionally named `AppModule`.
- It is defined in the `app.module.ts` file.

**Example: `app.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { ExampleComponent } from './example/example.component';

@NgModule({
  declarations: [
    AppComponent,
    ExampleComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
- **declarations**: Declares the components, directives, and pipes that belong to this module.
- **imports**: Imports other modules whose exported classes are needed by component templates declared in this module.
- **providers**: Registers services that the module contributes to the global collection of services.
- **bootstrap**: Identifies the root component that Angular should bootstrap when it starts the application.

**2. Feature Modules:**
- Feature modules are used to encapsulate and organize related components, directives, services, and pipes.
- They help in dividing the application into smaller, more manageable parts.
- They can be eagerly loaded (loaded at the start) or lazily loaded (loaded on demand).

**Creating a Feature Module:**
- Using Angular CLI:
  ```bash
  ng generate module feature --routing
  ```
  This command creates a new module along with a routing module.

**Example: `feature.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FeatureComponent } from './feature/feature.component';
import { FeatureRoutingModule } from './feature-routing.module';

@NgModule({
  declarations: [
    FeatureComponent
  ],
  imports: [
    CommonModule,
    FeatureRoutingModule
  ]
})
export class FeatureModule { }
```

**Example: `feature-routing.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { FeatureComponent } from './feature/feature.component';

const routes: Routes = [
  { path: '', component: FeatureComponent }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class FeatureRoutingModule { }
```

**3. Shared Modules:**
- Shared modules are used to organize and share common components, directives, and pipes across the application.
- They help in avoiding code duplication.

**Creating a Shared Module:**
- Using Angular CLI:
  ```bash
  ng generate module shared
  ```

**Example: `shared.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { CommonComponent } from './common/common.component';

@NgModule({
  declarations: [
    CommonComponent
  ],
  imports: [
    CommonModule
  ],
  exports: [
    CommonComponent
  ]
})
export class SharedModule { }
```

**4. Lazy Loading Modules:**
- Lazy loading is a technique to load modules on demand, reducing the initial load time of the application.
- It is configured using Angular's routing module.

**Configuring Lazy Loading:**
**Example: `app-routing.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  {
    path: 'feature',
    loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule)
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

#### Summary

- **NgModules**: Containers for a cohesive block of code.
- **Root Module**: The main module bootstrapped by Angular.
- **Feature Modules**: Encapsulate related functionalities, can be eagerly or lazily loaded.
- **Shared Modules**: Organize and share common components across the application.
- **Lazy Loading**: Technique to load modules on demand to improve performance.

By understanding and using Angular modules effectively, developers can create scalable, maintainable, and efficient applications.