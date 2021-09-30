[BACK](./README.md)

# Components

## Membuat Component pada Angular

```
$ ng generate component <name>
```

Hasil output terminal ketika perintah tersebut dieksekusi:

```
$ ng generate component parent
CREATE src/app/parent/parent.component.css (0 bytes)
CREATE src/app/parent/parent.component.html (21 bytes)
CREATE src/app/parent/parent.component.spec.ts (626 bytes)
CREATE src/app/parent/parent.component.ts (275 bytes)
UPDATE src/app/app.module.ts (475 bytes)
```

Terdapat 4 buah file yang dibuat:

- `.css`: Berkaitan dengan styling
- `.html`: Berkaitan dengan template
- `.spec.ts`: Berkaitan dengan testing
- `.ts`: Berkaitan dengan logic

`app.module.ts` akan di-update secara otomatis. Update tersebut adalah untuk menyertakan component yang baru saja dibuat ke aplikasi utama, sehingga component tersebut siap untuk digunakan.

**`./src/app/app.module.ts`**

```ts
...
import { AppComponent } from './app.component';
import { ParentComponent } from './parent/parent.component';

@NgModule({
  declarations: [
    AppComponent,
    ParentComponent
  ],
...
```

## Menyertakan Component pada App

**`./src/app/app.component.html`**

```html
<h1>Main App</h1>
<app-parent></app-parent>
```

`app-parent` harus sesuai dengan `selector` dari `.src/app/parent/parent.component.ts`.

## Child Component

Sebuah component pada Angular dapat tersusun dari beberapa component lainnya. Misal terdapat sebuah component `EmployeeListComponent`. Komponen tersebut dapat terdiri dari component yang lebih dasar, yaitu `EmployeeComponent`. Jadi component `EmployeeListComponent` tersusun dari beberapa component `EmployeeComponent`.

1. Buat sebuah component dengan nama `ChildComponent`
   
   ```
   $ ng generate component child
   ```

2. Sertakan component tersebut pada `ParentComponent`

   **`./src/app/parent/parent.component.html`**

   ```html
   <h2>Parent</h2>
   <app-child></app-child>
   ```

## Property pada Component

Sebuah component dapat memiliki property.

**`./src/app/parent/parent.component.ts`**

```ts
...
export class ParentComponent implements OnInit {

  title = 'Parent Component';

  constructor() { }

  ngOnInit(): void {
  }

}
```

## Menampilkan Property pada Template

**`./src/app/parent/parent.component.html`**

```html
<h2>{{ title }}</h2>
<app-child></app-child>
```

## Mengirim Data dari Parent ke Child

- **`./src/app/parent/parent.component.ts`**
   
  ```ts
  ...
  export class ParentComponent implements OnInit {
  
    title = 'Parent Component';
    valueToChild = 'Hello child!';
  
    constructor() { }
  
    ngOnInit(): void {
    }
  
  }
  ```
  
- **`./src/app/parent/parent.component.html`**
  
  ```html
  <h2>{{ title }}</h2>
  <app-child [valueFromParent]="valueToChild"></app-child>
  ```
  
- **`./src/app/child/child.component.ts`**
  
  ```ts
  import { Component, Input, OnInit } from '@angular/core';
  ...
  export class ChildComponent implements OnInit {
  
    @Input() valueFromParent = '';
  
    title = 'Child Component';
  
    constructor() { }
  
    ngOnInit(): void {
    }
  
  }
  ```
  
- **`./src/app/child/child.component.html`**
  
  ```html
  <div>{{ title }}</div>
  <div>{{ valueFromParent }}</div>
  ```

## Event Emitter

Sebuah child dapat mengirimkan sebuah event kepada parent.

- **`./src/app/child/child.component.ts`**

  ```ts
  import { Component, EventEmitter, Input, OnInit, Output } from '@angular/core';
  ...
  export class ChildComponent implements OnInit {
  
    @Input() valueFromParent = '';
    @Output() childEvent = new EventEmitter();
  
    title = 'Child Component';
  
    constructor() { }
  
    ngOnInit(): void {
    }
  
    onClick(): void {
      this.childEvent.emit();
    }
  
  }
  ```

- **`./src/app/child/child.component.html`**
  
  ```html
  <div>{{ title }}</div>
  <div>{{ valueFromParent }}</div>
  <button (click)="onClick()">Send data to parent</button>
  ```

  Pada contoh di atas:
  
  1. Kita menambahkan `EventEmitter` untuk di-import oleh `child.components.ts`.
  2. Kita membuat sebuah property `childEvent` dengan decorator `@Output()`. Property tersebut adalah sebuah instance `EventEmitter` yang dapat mengirimkan sinyal event.
  3. Kita menambahkan `button` pada `child.component.html`. Apabila button tersebut di-click, maka method `onClick()` pada `child.component.ts` akan dieksekusi.
  4. Pada saat `onClick()` dieksekusi, maka method tersebut akan memanggil method `emit()` dari property `childEvent`.

- **`./src/app/parent/parent.component.ts`**

  ```ts
  ...
  export class ParentComponent implements OnInit {

    title = 'Parent Component';
    valueToChild = 'Hello child!'

    constructor() { }

    ngOnInit(): void {
    }

    handleChild(): void {
        console.log('Received an event from child');
    }

  }
  ```

- **`./src/app/parent/parent.component.html`**

  ```html
  <h2>{{ title }}</h2>
  <app-child [valueFromParent]="valueToChild" (childEvent)="handleChild()"></app-child>
  ```

  Pada contoh di atas:
  
  1. Kita menambahkan sebuah method `handleChild()` pada `parent.component.ts`.
  2. Pada `parent.component.html` kita menangkap event `childEvent`. Apabila terdapat event tersebut, maka method `handleChild()` akan dieksekusi. Nama event `childEvent` harus sesuai dengan property yang ada pada `child.component.ts`.

## Mengirim Data dari Child ke Parent menggunakan Event Emitter

- **`./src/app/child/child.component.ts`**

  ```ts
  ...
  export class ChildComponent implements OnInit {

    @Input() valueFromParent = '';
    @Output() childEvent = new EventEmitter<string>();

    title = 'Child Component';

    constructor() { }

    ngOnInit(): void {
    }

    onClick(): void {
      this.childEvent.emit('some text');
    }

  }
  ```

- **`./src/app/parent/parent.component.ts`**

  ```ts
  ...
    handleChild(input: string): void {
      console.log(input);
    }
  ...
  ```

- **`./src/app/parent/parent.component.html`**

  ```html
  <h2>{{ title }}</h2>
  <app-child [valueFromParent]="valueToChild" (childEvent)="handleChild($event)"></app-child>
  ```

[BACK](./README.md)
