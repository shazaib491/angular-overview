### Routing in Angular (15 mins)

Routing is a core concept in Angular, enabling navigation between different views or pages in a single-page application (SPA). Angular's Router module provides powerful and flexible routing capabilities.

#### Key Concepts and Features

**1. Setting Up Routing in an Angular Application**
- **Router Module**: Angular's RouterModule is used to configure and manage routes in the application.
- **Routes**: A route is a definition (object) that maps a URL path to a component.

**Example: Basic Routing Setup**
```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

const routes: Routes = [
  { path: '', component: HomeComponent }, // Default route
  { path: 'about', component: AboutComponent } // Route for /about
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
- **Routes Array**: Defines the application routes.
  - `path`: URL path.
  - `component`: Component to display when the path is matched.
- **RouterModule.forRoot()**: Configures the router at the application level. Only called once, usually in the AppModule.

**Importing AppRoutingModule in AppModule**
```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    AboutComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**2. RouterLink and RouterOutlet**
- **RouterLink**: Directive for linking to routes.
  - Example:
    ```html
    <!-- app.component.html -->
    <nav>
      <a routerLink="/">Home</a>
      <a routerLink="/about">About</a>
    </nav>
    <router-outlet></router-outlet>
    ```
- **RouterOutlet**: Directive that acts as a placeholder for the routed component's template.
  - The component corresponding to the active route is displayed here.

**3. Route Parameters and Query Parameters**
- **Route Parameters**: Used to capture values from the URL.
  - Example:
    ```typescript
    // app-routing.module.ts
    const routes: Routes = [
      { path: 'product/:id', component: ProductComponent }
    ];
    ```
    ```typescript
    // product.component.ts
    import { ActivatedRoute } from '@angular/router';

    export class ProductComponent implements OnInit {
      id: string;

      constructor(private route: ActivatedRoute) {}

      ngOnInit(): void {
        this.id = this.route.snapshot.paramMap.get('id');
      }
    }
    ```
- **Query Parameters**: Used to pass optional parameters to a route.
  - Example:
    ```typescript
    // navigating to a route with query params
    import { Router } from '@angular/router';

    constructor(private router: Router) {}

    navigateToProduct() {
      this.router.navigate(['/product', 123], { queryParams: { ref: 'home' } });
    }
    ```
    ```typescript
    // product.component.ts
    import { ActivatedRoute } from '@angular/router';

    export class ProductComponent implements OnInit {
      ref: string;

      constructor(private route: ActivatedRoute) {}

      ngOnInit(): void {
        this.route.queryParams.subscribe(params => {
          this.ref = params['ref'];
        });
      }
    }
    ```

**4. Child Routes and Nested Routing**
- Child routes are used to define routes within other routes.
- Useful for creating complex layouts with nested views.

**Example: Nested Routes**
```typescript
// app-routing.module.ts
const routes: Routes = [
  { path: 'user', component: UserComponent, children: [
    { path: 'profile', component: UserProfileComponent },
    { path: 'settings', component: UserSettingsComponent }
  ]}
];
```
```html
<!-- user.component.html -->
<h2>User Section</h2>
<nav>
  <a routerLink="profile">Profile</a>
  <a routerLink="settings">Settings</a>
</nav>
<router-outlet></router-outlet>
```
- `<router-outlet>` in `UserComponent` will display the nested components based on the child routes.

**5. Lazy Loading Modules**
- Lazy loading is a technique to load feature modules on demand rather than upfront.
- Improves initial load time by splitting the application into multiple bundles.

**Example: Configuring Lazy Loading**
```typescript
// app-routing.module.ts
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule)
  }
];
```
- The `AdminModule` will be loaded only when the user navigates to the `/admin` route.

**6. Route Guards**
- Route guards determine whether a route can be activated, deactivated, or loaded.
- Types of guards: `CanActivate`, `CanActivateChild`, `CanDeactivate`, `Resolve`, `CanLoad`.

**Example: Using CanActivate Guard**
```typescript
// auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isLoggedIn()) {
      return true;
    } else {
      this.router.navigate(['/login']);
      return false;
    }
  }
}
```
```typescript
// app-routing.module.ts
const routes: Routes = [
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] }
];
```
- The `AuthGuard` checks if the user is logged in before activating the `DashboardComponent` route.

#### Summary
- **RouterModule**: Core module for configuring and managing routes.
- **Routes Array**: Defines application routes with paths and components.
- **RouterLink and RouterOutlet**: Directives for navigation and displaying routed views.
- **Route Parameters**: Capture values from the URL.
- **Query Parameters**: Pass optional parameters to a route.
- **Nested Routes**: Define routes within other routes for complex layouts.
- **Lazy Loading**: Load feature modules on demand to improve performance.
- **Route Guards**: Protect routes using guard services.

By mastering Angular's routing capabilities, developers can build dynamic, responsive, and scalable single-page applications with ease.