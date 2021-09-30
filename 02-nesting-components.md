# Nesting Components

Sebuah aplikasi Angular dibangun dari kumpulan component. Sebuah component dapat tersusun dari component lainnya. Sebagai contoh pada bahasan sebelumnya, component `ParentComponent` memiliki component `ChildComponent`.

**`./src/app/parent/parent.component.html`**

```html
<h2>Parent</h2>
<app-child></app-child>
```

Pada Angular terdapat dua cara untuk melakukan nesting component:

1. Secara langsung meletakkan selector child component pada template parent component
2. Menggunakan content projection

Untuk keperluan demo, mari kita buat 3 buah component.

```
$ ng generate component parent
$ ng generate component first-child
$ ng generate component second-child
```

Kemudian ubah `app.component.html` seperti di bawah.

**`./src/app/app.component.html`**

```html
<h1>Main App</h1>
<app-parent></app-parent>
```

## View Child

Apabila kita meletakkan component child pada template component parent, maka child tersebut disebut sebagai `ViewChild`.

**`./src/app/parent/parent.component.html`**

```html
<h2>Parent</h2>
<app-first-child></app-first-child>
```

Pada contoh di atas, component `FirstChild` merupakan `ViewChild` dari component `Parent`.

Kita dapat juga menyertakan `SecondChild` pada template component `FirstChild`.

**`./src/app/first-child/first-child.component.html`**

```html
<h3>First Child</h3>
<app-second-child></app-second-child>
```

Pada contoh di atas, maka component `SecondChild` merupakan `ViewChild` dari component `FirstChild`.

## Content Child

Sebuah tag HTML dapat memiliki content. Sebagai contoh adalah seperti di bawah.

```html
<h1>Content!</h1>
```

Kita juga dapat melakukan hal yang sama pada component. Sebagai contoh adalah seperti di bawah.

**`./src/app/parent/parent.component.html`**

```html
<h2>Parent</h2>
<app-first-child>
  <app-second-child></app-second-child>
</app-first-child>
```

Pada contoh di atas, `SecondChild` merupakan content dari `FirstChild`. Namun pada saat kita melihat hasilnya di browser, maka `SecondChild` tidak di-render. Hal tersebut dikarenakan kita harus memberitahu Angular di mana kita akan me-render content `SecondChild` tersebut. Maka dari itu ubah `first-child.component.html` menjadi seperti di bawah.

**`./src/app/first-child/first-child.component.html`**

```html
<h3>First Child</h3>
<ng-content></ng-content>
```

Pada contoh di atas kita tidak secara langsung meletakkan component `SecondChild` pada template component `FirstChild`. Dengan cara tersebut, maka component `SecondChild` merupakan `ContentChild` dari component `FirstChild`. Sedangkan component `FirstChild` sendiri merupakan `ViewChild` dari component `Parent`.

### Lebih dari Satu Content Child

Coba kita buat sebuah component yaitu `ThirdChild`. Kemudian ubah `parent.component.html` menjadi seperti di bawah.

**`./src/app/parent/parent.component.html`**

```html
<h2>Parent</h2>
<app-first-child>
  <app-second-child></app-second-child>
  <app-third-child></app-third-child>
</app-first-child>
```

Pada contoh di atas, content dari component `FirstChild` ada 2 yaitu `SecondChild` dan `ThirdChild`. Hasil dari contoh tersebut adalah component `ThirdChild` akan di-render setelah `SecondChild`. Bagaimana agar kita dapat menentukan urutan render melalui `first-child.component.html`? Caranya adalah menggunakan Multi-slot content projection. Ubah `parent.component.html` menjadi seperti di bawah.

**`./src/app/parent/parent.component.html`**

```html
<h2>Parent</h2>
<app-first-child>
  <app-second-child secondChild></app-second-child>
  <app-third-child thirdChild></app-third-child>
</app-first-child>
```

Kemudian ubah `first-child.component.html` menjadi seperti di bawah.

**`./src/app/first-child/first-child.component.html`**

```html
<h3>First Child</h3>
<ng-content select="[thirdChild]"></ng-content>
<ng-content select="[secondChild]"></ng-content>
```

Hasilnya adalah component `ThirdChild` akan dirender sebelum component `SecondChild`.

Sebagai catatan:

- Content tidak harus berupa component, namun dapat berupa tag HTML lain.
- Apabila terdapat `<ng-content>` yang tidak memiliki attribute `select`, maka `<ng-content>` tersebut akan menerima component-component lain yang tidak di-`select`.
