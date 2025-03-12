# Setting Up an Angular Project

## Introduction
Angular is a TypeScript-based front-end framework developed by Google. It is used for building single-page web applications (SPAs) with a structured and maintainable approach. This guide will help you set up an Angular project, install necessary dependencies, and understand the basic flow of an Angular application. Angular follows a component-based architecture and provides powerful features like dependency injection, declarative templates, and comprehensive tooling for large-scale application development.

## Prerequisites
Before creating an Angular project, ensure you have the following installed on your system:

- **Node.js** (Download from [nodejs.org](https://nodejs.org/))
- **Angular CLI** (Command Line Interface for Angular)
- **Code Editor** (Visual Studio Code recommended)

## Step 1: Install Angular CLI
Angular CLI simplifies the setup and management of Angular applications. To install it globally, run:

```sh
npm install -g @angular/cli
```

To verify the installation, use:

```sh
ng version
```

## Step 2: Create a New Angular Project
To create a new Angular project, run:

```sh
ng new my-angular-app
```

Follow the prompts to select configuration options such as routing and stylesheets (CSS, SCSS, etc.).

Navigate into the project folder:

```sh
cd my-angular-app
```

## Step 3: Serve the Application
Start the development server with:

```sh
ng serve
```

By default, the app runs at `http://localhost:4200/`.

## Angular Project Structure
A typical Angular project contains the following structure:

```
my-angular-app/
├── src/
│   ├── app/
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component.css
│   │   ├── app.module.ts
│   │   ├── home/
│   │   │   ├── home.component.ts
│   │   │   ├── home.component.html
│   │   │   ├── home.component.css
│   │   ├── contact/
│   │   │   ├── contact.component.ts
│   │   │   ├── contact.component.html
│   │   │   ├── contact.component.css
│   │   ├── about/
│   │   │   ├── about.component.ts
│   │   │   ├── about.component.html
│   │   │   ├── about.component.css
│   ├── assets/
│   ├── environments/
│   ├── main.ts
│   ├── index.html
│   ├── styles.css
├── angular.json
├── package.json
```
Main Files

index.html
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>MyAngularApp</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```
app.component.html
```html
<!-- app.component.html -->
<div class="container">
  <nav>
    <a routerLink="/">Home</a>
    <a routerLink="/about">About</a>
    <a routerLink="/contact">Contact</a>
  </nav>
  <!-- This is where the routed component will be displayed -->
  <router-outlet></router-outlet>
</div>
```
### Angular App Component (`app.component.ts`)

```typescript
import { Component } from '@angular/core';
import { RouterModule, RouterOutlet } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,  // This component is standalone
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  imports: [RouterOutlet, RouterModule]  // Import your components here
})
export class AppComponent {
  title = 'my-angular-app';
  message: string = '';

  sayHello() {
    this.message = 'Hello from Angular!';
  }
}
```

---

### Angular Routing Configuration (`app.routes.ts`)

```typescript
import { Routes } from '@angular/router';
import { AboutComponent } from './about/about.component';
import { ContactComponent } from './contact/contact.component';
import { HomeComponent } from './home/home.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'contact', component: ContactComponent },
  { path: '**', redirectTo: '' } // Wildcard route for unknown paths
];
```


## Additional Components

### Home Component
#### **home.component.html**
```html
<div class="home-container">
  <h2>Welcome to Our Website</h2>
  <p>This is the home page of our Angular application. Feel free to explore!</p>
  <div class="button-container">
    <button (click)="navigateToAbout()">Learn More About Us</button>
    <button (click)="navigateToContact()">Get in Touch</button>
  </div>
</div>
```

#### **home.component.css**
```css
.home-container {
  padding: 20px;
  text-align: center;
}

h2 {
  color: #2c3e50;
  margin-bottom: 20px;
}

p {
  color: #666;
  font-size: 18px;
  line-height: 1.6;
  margin-bottom: 30px;
}
```
<img width="853" alt="image" src="https://github.com/user-attachments/assets/78b85edd-b490-4b78-8ebf-54c8677ddab1" />

### Contact Component
#### **contact.component.ts**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-contact',
  templateUrl: './contact.component.html',
  styleUrls: ['./contact.component.css']
})
export class ContactComponent {}
```

#### **contact.component.html**
```html
<p>contact works!</p>
```
<img width="245" alt="image" src="https://github.com/user-attachments/assets/f8d788bd-1915-4062-9a7f-4ee198591c65" />

### About Component
#### **about.component.ts**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-about',
  templateUrl: './about.component.html',
  styleUrls: ['./about.component.css']
})
export class AboutComponent {}
```

#### **about.component.html**
```html
<p>about works!</p>
```
<img width="277" alt="image" src="https://github.com/user-attachments/assets/82449563-6ac2-463a-bb21-f36e6a7fc377" />

## Step 4: Add Components
Generate a new component using:

```sh
ng generate component home
ng generate component contact
ng generate component about
```

This creates `home.component.ts`, `home.component.html`, `home.component.css`, `contact.component.ts`, `contact.component.html`, `contact.component.css`, `about.component.ts`, `about.component.html`, and `about.component.css`.

## Conclusion
You now have a working Angular application with a clear understanding of its core components. From here, you can expand your project by adding services, directives, and more features to enhance your application.