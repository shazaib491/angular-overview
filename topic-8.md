### HTTP Client in Angular (10 mins)

HTTP Client in Angular is used to make HTTP requests to communicate with backend services. Angular provides a robust HTTP client module (`HttpClientModule`) which allows you to easily send and handle HTTP requests.

#### Key Concepts and Features

**1. Setting Up HTTP Client**
- **HttpClientModule**: To use the HttpClient service, you need to import HttpClientModule into your app module.

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**2. Making HTTP Requests**
- **HttpClient**: Angular's HttpClient service is used to make HTTP requests. You can inject this service into your components or services to perform HTTP operations.

**Example: Creating a Service to Make HTTP Requests**

```typescript
// data.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) {}

  fetchData(): Observable<any> {
    return this.http.get<any>(this.apiUrl);
  }
}
```

**3. Injecting and Using HttpClient in a Component**

```typescript
// app.component.ts
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  data: any;

  constructor(private dataService: DataService) {}

  ngOnInit(): void {
    this.dataService.fetchData().subscribe(
      response => this.data = response,
      error => console.error(error)
    );
  }
}
```

**4. Displaying Data in the Template**

```html
<!-- app.component.html -->
<div *ngIf="data">
  <pre>{{ data | json }}</pre>
</div>
```

**5. Handling HTTP Errors**
- Handling errors is an important aspect of making HTTP requests. You can use RxJS operators like `catchError` to handle errors.

**Example: Handling Errors in DataService**

```typescript
// data.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) {}

  fetchData(): Observable<any> {
    return this.http.get<any>(this.apiUrl).pipe(
      catchError(this.handleError)
    );
  }

  private handleError(error: HttpErrorResponse) {
    if (error.error instanceof ErrorEvent) {
      // Client-side error
      console.error('An error occurred:', error.error.message);
    } else {
      // Server-side error
      console.error(`Server returned code: ${error.status}, body was: ${error.error}`);
    }
    return throwError('Something bad happened; please try again later.');
  }
}
```

**6. Making Different Types of HTTP Requests**
- **GET Request**: Fetch data from a server.
  ```typescript
  this.http.get(url)
  ```
- **POST Request**: Send data to a server.
  ```typescript
  this.http.post(url, data)
  ```
- **PUT Request**: Update existing data on the server.
  ```typescript
  this.http.put(url, data)
  ```
- **DELETE Request**: Delete data from the server.
  ```typescript
  this.http.delete(url)
  ```

**Example: Performing Different Types of HTTP Requests**

```typescript
// data.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private apiUrl = 'https://api.example.com/data';

  constructor(private http: HttpClient) {}

  fetchData(): Observable<any> {
    return this.http.get<any>(this.apiUrl);
  }

  sendData(data: any): Observable<any> {
    return this.http.post<any>(this.apiUrl, data);
  }

  updateData(id: string, data: any): Observable<any> {
    return this.http.put<any>(`${this.apiUrl}/${id}`, data);
  }

  deleteData(id: string): Observable<any> {
    return this.http.delete<any>(`${this.apiUrl}/${id}`);
  }
}
```

**7. Interceptors**
- Interceptors are used to modify HTTP requests and responses globally.
- They are useful for adding authentication tokens, logging, and error handling.

**Example: Creating an HTTP Interceptor**

```typescript
// auth.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const clonedRequest = req.clone({
      headers: req.headers.set('Authorization', 'Bearer YOUR_TOKEN_HERE')
    });
    return next.handle(clonedRequest);
  }
}
```

**Registering the Interceptor**

```typescript
// app.module.ts
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './auth.interceptor';

@NgModule({
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ]
})
export class AppModule { }
```

#### Summary
- **HttpClientModule**: Module needed to use HttpClient in Angular.
- **HttpClient**: Service for making HTTP requests (GET, POST, PUT, DELETE).
- **Handling Errors**: Use RxJS operators like `catchError` for error handling.
- **Interceptors**: Modify HTTP requests and responses globally.

By understanding and using Angular's HttpClient, developers can efficiently communicate with backend services, handle errors, and enhance the application's capabilities with HTTP interceptors.