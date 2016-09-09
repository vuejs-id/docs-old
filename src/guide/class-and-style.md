---
title: Class and Style Bindings
type: guide
order: 6
---

<!-- A common need for data binding is manipulating an element's class list and its inline styles. Since they are both attributes, we can use `v-bind` to handle them: we just need to calculate a final string with our expressions. However, meddling with string concatenation is annoying and error-prone. For this reason, Vue.js provides special enhancements when `v-bind` is used for `class` and `style`. In addition to Strings, the expressions can also evaluate to Objects or Arrays. -->
Salah satu kebutuhan umum dari data binding adalah memanipulasi sebuah daftar class elemen dan inline styles. Karena kedua-duanya merupakan atribut, kita bisa menggunakan `v-bind` untuk menangani  mereka: kita hanya perlu melakukan perhitungan/kalkulasi sebuah string akhir dengan ekspresi kita. Namun, mencampur dengan penggabungan string membosankan dan rentan error. Untuk alasan inilah, Vue.js menyediakan tambahan spesial ketika `v-bind` digunakan untuk `class` dan `style`. Selain untuk Strings, ekpresi tersebut juga bisa mengevaluasi Objek dan Larik/Array.

## Binding HTML Classes

<!-- <p class="tip">Although you can use mustache interpolations such as `{% raw %}class="{{ className }}"{% endraw %}` to bind the class, it is not recommended to mix that style with `v-bind:class`. Use one or the other!</p> -->
<p class="tip">Meskipun anda bisa menggunakan interpolasi kurung kurawal ganda seperti `{% raw %}class="{{ className }}"{% endraw %}` untuk mengikat class, tidak dianjurkan untuk mencampur syle tersebut dengan `v-bind:class`. Gunakan salah satu atau yang lainnya!</p>

### Objek Sintaks

<!-- We can pass an Object to `v-bind:class` to dynamically toggle classes. Note the `v-bind:class` directive can co-exist with the plain `class` attribute: -->
Kita bisa melewatkan sebuah Objek ke `v-bind:class` untuk secara dinamis men-toggle class. Catatan bahwa `v-bind:class` directive dapat berdampingan dengan plain `class` attribute:

``` html
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>
```
``` js
data: {
  isA: true,
  isB: false
}
```

<!-- Which will render: -->
Yang akan merender:

``` html
<div class="static class-a"></div>
```

<!-- When `isA` and `isB` changes, the class list will be updated accordingly. For example, if `isB` becomes `true`, the class list will become `"static class-a class-b"`. -->
Ketika `isA` dan `isB` berubah, daftar class juga akan berubah. Sebagai contoh, jika `isB` menjadi `true`, daftar kelas akan menjadi `"static class-a class-b"`.

<!-- And you can directly bind to an object in data as well: -->
Dan anda juga bisa secara langsung mengikat ke sebuah objek di dalam sebuah data:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    'class-a': true,
    'class-b': false
  }
}
```

<!-- This will render the same result. As you may have noticed, we can also bind to a [computed property](computed.html) that returns an Object. This is a common and powerful pattern. -->
Ini akan merender hasil yang sama. Seperti yang mungkin anda telah sadari, kita juga bisa mengikat ke sebuah [computed property](computed.html) yang mengembalikan sebuah Objek. Ini adalah sebuah hal yang lumrah dan merupakan pola yang  powerfull.

### Sintaks Larik/Array

<!-- We can pass an Array to `v-bind:class` to apply a list of classes: -->
Kita bisa melewatkan sebuah larik/array ke `v-bind:class` untuk menerapkan sebuah daftar class:

``` html
<div v-bind:class="[classA, classB]">
```
``` js
data: {
  classA: 'class-a',
  classB: 'class-b'
}
```

<!-- Which will render: -->
Yang akan merender:

``` html
<div class="class-a class-b"></div>
```

<!-- If you would like to also toggle a class in the list conditionally, you can do it with a ternary expression: -->
Jika anda juga ingin men-toggle sebuah class di daftar secara kondisional, anda bisa melakukannya dengan sebuah ekpresi ternari:

``` html
<div v-bind:class="[classA, isB ? classB : '']">
```

<!-- This will always apply `classA`, but will only apply `classB` when `isB` is `true`. -->
Ini akan selalu menerapkan `classA`, tetapi hanya akan menerapkan `classB` ketika `isB` bernilai `true`.

<!-- However, this can be a bit verbose if you have multiple conditional classes. In version 1.0.19+, it's also possible to use the Object syntax inside Array syntax: -->
Meskipun demikian, ini bisa menjadi sedikit berlebihan dalam penulisan jika anda memiliki banyak kondisi class. Pada versi 1.0.19+, memungkinkan untuk menggunakan Sintaks Objek didalam Sintaks larik/array:


``` html
<div v-bind:class="[classA, { classB: isB, classC: isC }]">
```

## Binding Inline Styles

### Sintaks Objek

<!-- The Object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case for the CSS property names: -->
Sintaks objek untuk `v-bind:style` sangat mudah dan jelas - dia mirip dengan CSS, kecuali dia adalah objek Javascript. Anda bisa menggunakan camelCase atau kebab-case untuk nama properti CSS:


``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

<!-- It is often a good idea to bind to a style object directly so that the template is cleaner: -->
Sering kali mengikat sebuah object style secara langsung merupakan ide yang bagus sehingga template menjadi lebih rapi.

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

<!-- Again, the Object syntax is often used in conjunction with computed properties that return Objects. -->
Sekali lagi, Sintaks objek sering kali digunakan serangkai dengan computed properties yang mengembalikan Objek.

### Sintaks Larik/Array

<!-- The Array syntax for `v-bind:style` allows you to apply multiple style objects to the same element: -->
Sintaks larik/array untuk `v-bind:Style` membolehkan anda untuk menerapkan banyak style objects ke element yang sama:

``` html
<div v-bind:style="[styleObjectA, styleObjectB]">
```

### Auto-prefixing

<!-- When you use a CSS property that requires vendor prefixes in `v-bind:style`, for example `transform`, Vue.js will automatically detect and add appropriate prefixes to the applied styles. -->
Ketika anda menggunakan sebuah properti CSS yang memerlukan awalan vendor pada `v-bind:style`, sebagai contoh `transform`, Vue.js akan secara otomatis mendeteksi dan menambahkan awalan yang sesuai untuk style yang diterapkan.
