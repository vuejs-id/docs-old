---
title: Conditional Rendering
type: guide
order: 7
---

## v-if

<!-- In string templates, for example Handlebars, we would write a conditional block like this: -->
Pada string templates, sebagai contoh Handlebars, kita akan menulis sebuah blok kondisional seperti ini:

``` html
<!-- Handlebars template -->
{{#if ok}}
  <h1>Yes</h1>
{{/if}}
```

<!-- In Vue.js, we use the `v-if` directive to achieve the same: -->
Pada Vue.js, kita menggunakan `v-if` directive untuk mendapatkan hal yang sama:

``` html
<h1 v-if="ok">Yes</h1>
```

<!-- It is also possible to add an "else" block with `v-else`: -->
Memungkinkan juga untuk menambahkan blok "else" dengan `v-else`:

``` html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

## Template v-if

<!-- Because `v-if` is a directive, it has to be attached to a single element. But what if we want to toggle more than one element? In this case we can use `v-if` on a `<template>` element, which serves as an invisible wrapper. The final rendered result will not include the `<template>` element. -->
Karena `v-if` adalah sebuah directive, dia harus terikat dengan sebuah elemen tunggal. Tetapi, bagaimana jika kita ingin men-toggle lebih dari satu elemen? Dalam kasus ini kita bisa menggunakan `v-if` pada sebuah elemen `<template>`, yang akan bertindak sebagai sebuah invisible wrapper.

``` html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

## v-show

<!-- Another option for conditionally displaying an element is the `v-show` directive. The usage is largely the same: -->
Opsi lain untuk menampilkan sebuah elemen secara kondisional adalah `v-show` directive. Penggunaannya sebagian besar sama:

``` html
<h1 v-show="ok">Hello!</h1>
```

<!-- The difference is that an element with `v-show` will always be rendered and remain in the DOM; `v-show` simply toggles the `display` CSS property of the element. -->
Perbedaannya adalah elemen dengan `v-show` akan selalu dirender dan berada di dalam DOM; `v-show` secara singkat men-toggle `tampilan` properti CSS dari elemen tersebut.

<!-- Note that `v-show` doesn't support the `<template>` syntax. -->
Catatan bahwa `v-show` tidak mendukung sintaks `<template>`.

## v-else

<!-- You can use the `v-else` directive to indicate an "else block" for `v-if` or `v-show`: -->
Anda bisa menggunakan `v-else` directive untuk mengindikasi sebuah "blok else" untuk `v-if` atau `v-show`:


``` html
<div v-if="Math.random() > 0.5">
  Sorry
</div>
<div v-else>
  Not sorry
</div>
```

<!-- The `v-else` element must immediately follow the `v-if` or `v-show` element - otherwise it will not be recognized. -->
Elemen `v-else` harus segera diikuti oleh elemen `v-if` atau `v-show` - jika tidak dia tidak akan dikenali.

### Component caveat

<!-- When used with components and `v-show`, `v-else` doesn't get applied properly due to directives priorities. So instead of doing this: -->
Ketika digunakan dengan komponen dan `v-show`, `v-else` tidak mendapatkan properti yang diterapkan karena prioritas directives. Jadi, daripada melakukan ini:

```html
<custom-component v-show="condition"></custom-component>
<p v-else>This could be a component too</p>
```

<!-- Replace the `v-else` with another `v-show`: -->
Ganti `v-else` dengan `v-show`:

```html
<custom-component v-show="condition"></custom-component>
<p v-show="!condition">This could be a component too</p>
```

<!-- It does work as intended with `v-if`. -->
Cara kerja dan hasilnya sama saja dengan `v-if`.

## v-if vs. v-show

<!-- When a `v-if` block is toggled, Vue.js will have to perform a partial compilation/teardown process, because the template content inside `v-if` can also contain data bindings or child components. `v-if` is "real" conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles. -->
Ketika sebuah blok `v-if` ditoggle, Vue.js harus melakukan proses kompilasi parsial/sebagian, karena konten template didalam `v-if` juga dapat mengandung data binding atau komponen anak. `v-if` merupakan rendering kondisional secara "nyata" karena dia harus memastikan event listeners dan komponen anak didalam blok kondisional benar-benar dihancurkan dan dibuat ulang ketika toggles.  

<!-- `v-if` is also **lazy**: if the condition is false on initial render, it will not do anything - partial compilation won't start until the condition becomes true for the first time (and the compilation is subsequently cached). -->
`v-if` juga **malas/lazy**: jika kondisi false pada awal render, dia tidak akan melakukan apapun - kompilasi parsial tidak akan mulai hingga kondisi menjadi true untuk pertama kali (dan kompilasi pun kemudian di-cached).

<!-- In comparison, `v-show` is much simpler - the element is always compiled and preserved, with just simple CSS-based toggling. -->
Sebagai perbandingan, `v-show` lebih ringkas - elemennya selalu dikompilasi dan disimpan, dengan hanya simple CSS-based toggling.

<!-- Generally speaking, `v-if` has higher toggle costs while `v-show` has higher initial render costs. So prefer `v-show` if you need to toggle something very often, and prefer `v-if` if the condition is unlikely to change at runtime. -->
Secara umum, `v-if` mempunyai higher toggle costs sementara `v-show` mempunyai higher initial render costs. Jadi  pilih `v-show` jika anda memerlukan untuk men-toggle sesuatu secara sering, dan pilih `v-if` jika kondisinya tidak sering berubah selama berjalan.
