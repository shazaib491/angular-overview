### Forms in Angular (10 mins)

Forms are essential for any web application as they allow users to input and submit data. Angular provides robust support for handling forms and offers two approaches to create forms: Template-driven forms and Reactive forms.

#### Template-driven Forms

Template-driven forms are simpler to use and are based on directives in the template. They rely heavily on Angular's two-way data binding and are suitable for simple forms.

**1. Setting Up Template-driven Forms**
- **FormsModule**: Import the FormsModule in your AppModule.

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**2. Creating a Template-driven Form**
- **HTML Template**: Use Angular directives such as `ngModel` for binding form inputs to model properties.

```html
<!-- app.component.html -->
<form #form="ngForm" (ngSubmit)="onSubmit(form)">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" ngModel>
  </div>
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" ngModel>
  </div>
  <button type="submit">Submit</button>
</form>
<p *ngIf="submitted">Form Submitted! Name: {{ user.name }}, Email: {{ user.email }}</p>
```

**3. Handling Form Submission**
- **Component Class**: Define properties and a method to handle form submission.

```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  user = {
    name: '',
    email: ''
  };
  submitted = false;

  onSubmit(form: any): void {
    this.submitted = true;
    this.user.name = form.value.name;
    this.user.email = form.value.email;
  }
}
```

**4. Form Validation**
- **Built-in Validators**: Use built-in Angular validators such as `required`, `minlength`, `maxlength`, `pattern`.

```html
<!-- app.component.html -->
<form #form="ngForm" (ngSubmit)="onSubmit(form)">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" ngModel required minlength="3">
    <div *ngIf="form.controls['name']?.invalid && form.controls['name']?.touched">
      Name is required and should be at least 3 characters long.
    </div>
  </div>
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" ngModel required>
    <div *ngIf="form.controls['email']?.invalid && form.controls['email']?.touched">
      A valid email is required.
    </div>
  </div>
  <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

#### Reactive Forms

Reactive forms are more powerful and scalable, suitable for complex scenarios. They are defined in the component class rather than the template and offer more control over form validation and dynamic changes.

**1. Setting Up Reactive Forms**
- **ReactiveFormsModule**: Import the ReactiveFormsModule in your AppModule.

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**2. Creating a Reactive Form**
- **FormBuilder**: Use FormBuilder to create form controls and groups.

```typescript
// app.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  userForm: FormGroup;
  submitted = false;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.userForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]]
    });
  }

  onSubmit(): void {
    this.submitted = true;
    if (this.userForm.valid) {
      console.log(this.userForm.value);
    }
  }
}
```

**3. HTML Template for Reactive Form**
- **FormGroup and FormControlName**: Bind the form to the template using FormGroup and FormControlName.

```html
<!-- app.component.html -->
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" formControlName="name">
    <div *ngIf="userForm.get('name')?.invalid && userForm.get('name')?.touched">
      Name is required and should be at least 3 characters long.
    </div>
  </div>
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" formControlName="email">
    <div *ngIf="userForm.get('email')?.invalid && userForm.get('email')?.touched">
      A valid email is required.
    </div>
  </div>
  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>
<p *ngIf="submitted">Form Submitted! Name: {{ userForm.value.name }}, Email: {{ userForm.value.email }}</p>
```

**4. Dynamic Form Controls**
- **Adding Controls Dynamically**: Reactive forms allow adding or removing controls dynamically.

```typescript
// app.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, FormArray, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  userForm: FormGroup;

  constructor(private fb: FormBuilder) {}

  ngOnInit(): void {
    this.userForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]],
      addresses: this.fb.array([this.createAddress()])
    });
  }

  get addresses() {
    return this.userForm.get('addresses') as FormArray;
  }

  createAddress(): FormGroup {
    return this.fb.group({
      street: '',
      city: '',
      state: '',
      zip: ''
    });
  }

  addAddress(): void {
    this.addresses.push(this.createAddress());
  }

  onSubmit(): void {
    console.log(this.userForm.value);
  }
}
```

**HTML Template for Dynamic Form Controls**
```html
<!-- app.component.html -->
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" formControlName="name">
  </div>
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" formControlName="email">
  </div>
  <div formArrayName="addresses">
    <div *ngFor="let address of addresses.controls; let i = index" [formGroupName]="i">
      <h4>Address {{ i + 1 }}</h4>
      <label for="street">Street:</label>
      <input type="text" formControlName="street">
      <label for="city">City:</label>
      <input type="text" formControlName="city">
      <label for="state">State:</label>
      <input type="text" formControlName="state">
      <label for="zip">Zip:</label>
      <input type="text" formControlName="zip">
    </div>
  </div>
  <button type="button" (click)="addAddress()">Add Address</button>
  <button type="submit">Submit</button>
</form>
```

#### Summary
- **Template-driven Forms**: Easy to use, suitable for simple forms. Uses Angular directives and two-way data binding.
- **Reactive Forms**: More powerful and scalable, suitable for complex forms. Provides better control over form validation and dynamic changes.
- **FormsModule and ReactiveFormsModule**: Modules required to work with template-driven and reactive forms respectively.
- **Form Validation**: Angular offers built-in validators and the ability to create custom validators for both template-driven and reactive forms.
- **Dynamic Forms**: Reactive forms allow for adding and removing form controls dynamically, making them suitable for more complex scenarios.

By understanding these two approaches,

 developers can effectively create and manage forms in Angular applications, ensuring a smooth user experience and robust data handling.