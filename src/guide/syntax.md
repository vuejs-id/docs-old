---
title: Sintaks Data Binding
type: guide
order: 4
---

<!-- Vue.js uses a DOM-based templating implementation. This means that all Vue.js templates are essentially valid, parsable HTML enhanced with some special attributes. Keep that in mind, since this makes Vue templates fundamentally different from string-based templates. -->
Vue.js menggunakan implementasi template berbasis DOM. Ini artinya semua template Vue.js secara esensial adalah HTML yang valid, dapat diparsing/diterjemahkan serta diperkaya dengan beberapa atribut spesial. Hal ini perlu diingat karena hal inilah yang membuat template di Vue secara fundamental berbeda dengan template berbasis string.

## Interpolasi

### Teks

<!-- The most basic form of data binding is text interpolation using the "Mustache" syntax (double curly braces): -->
Hal yang paling mendasar dari data binding adalah interpolasi teks menggunakan sintaks "Mustache/Kumis" (kurung kurawal ganda):

``` html
<span>Pesan: {{ msg }}</span>
```

<!-- The mustache tag will be replaced with the value of the `msg` property on the corresponding data object. It will also be updated whenever the data object's `msg` property changes. -->
Tag kumis akan digantikan oleh nilai properti `msg` dari objek data yang bersangkutan. Dia juga akan diperbarui kapanpun properti `msg` objek data berubah.

<!-- You can also perform one-time interpolations that do not update on data change: -->
Anda juga bisa melakukan interpolasi satu kali yang tidak memperbarui ketika data berubah:


``` html
<span>Ini tidak akan pernah berubah: {{* msg }}</span>
```

### Raw HTML

<!-- The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use triple mustaches: -->
Kurung kurawal ganda menginterpretasi data sebagai teks murni, bukan HTML. Supaya dapat menghasilkan keluaran HTML yang sebenarnya, anda perlu menggunakan kurung kurawal tiga kali:


``` html
<div>{{{ raw_html }}}</div>
```

<!-- The contents are inserted as plain HTML - data bindings are ignored. If you need to reuse template pieces, you should use [partials](/api/#partial). -->
Konten akan dimasukkan sebagai HTML murni - data bindings diabaikan. Jika anda perlu menggunakan ulang potongan-potongan template, anda sebaiknya menggunakan [parsial](/api/#partial).  

<!-- <p class="tip">Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). Only use HTML interpolation on trusted content and **never** on user-provided content.</p> -->
<p>Merender arbitrary HTML secara dinamis di website anda bisa jadi sangat berbahaya karena dia dapat membawa kita ke  [serangan XSS](https://en.wikipedia.org/wiki/Cross-site_scripting) secara mudah. Gunakan interpolasi HTML hanya pada konten yang dapat dipercaya dan **jangan pernah** menggunakannya pada konten yang disediakan user.</p>

### Atribut

<!-- Mustaches can also be used inside HTML attributes: -->
Kurung kurawal ganda juga dapat digunakan di dalam atribut HTML:

``` html
<div id="item-{{ id }}"></div>
```

<!-- Note that attribute interpolations are disallowed in Vue.js directives and special attributes. Don't worry, Vue.js will raise warnings for you when mustaches are used in wrong places. -->
Perlu diperhatikan bahwa interpolasi atribut tidak diizinkan di directive Vue.js dan atribut spesial. Jangan khawatir, Vue.js akan memberikan peringatan kepada anda ketika kurung kurawal ganda digunakan pada tempat yang tidak semestinya.

## Binding Expressions

<!-- The text we put inside mustache tags are called **binding expressions**. In Vue.js, a binding expression consists of a single JavaScript expression optionally followed by one or more filters. -->
Teks yang kita letakkan di dalam kurung kurawal ganda disebut **binding expressions**. Pada Vue.js, sebuah binding expressions terdiri dari sebuah ekspresi javascript tunggal, diikuti oleh satu atau lebih filter yang sifatnya opsional.

### Ekspresi JavaScript

<!-- So far we've only been binding to simple property keys in our templates. But Vue.js actually supports the full power of JavaScript expressions inside data bindings: -->
Sejauh ini kita hanya membuat ikatan/binding ke simple property keys di template kita. Tetapi sebenarnya Vue.js mendukung penuh ekspresi Javascript di dalam data binding.

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}
```

<!-- These expressions will be evaluated in the data scope of the owner Vue instance. One restriction is that each binding can only contain **one single expression**, so the following will **NOT** work: -->
Ekpresi tersebut akan dievaluasi di dalam data scope dari pemilik Vue instance. Satu yang tidak diperbolehkan adalah setiap ikatan/binding hanya dapat memuat **satu ekspresi tunggal**, jadi ekspresi seperti berikut **tidak akan** bekerja:

``` html
<!-- ini adalah sebuah pernyataan, bukan sebuah ekspresi: -->
{{ var a = 1 }}

<!-- kontrol aliran juga tidak akan bekerja, gunakan ekspresi ternari -->
{{ if (ok) { return message } }}
```

### Filter

<!-- Vue.js allows you to append optional "filters" to the end of an expression, denoted by the "pipe" symbol: -->
Vue.js membolehkan anda untuk menambahkan "filter" secara opsional ke akhir dari sebuah ekpresi, dilambangkan oleh simbol "pipa":

``` html
{{ message | capitalize }}
```

<!-- Here we are "piping" the value of the `message` expression through the built-in `capitalize` filter, which is in fact just a JavaScript function that returns the capitalized value. Vue.js provides a number of built-in filters, and we will talk about how to write your own filters later. -->
Disini kita "piping/meneruskan" nilai dari ekspresi `message` ke filter `capitalize` yang sudah tersedia, dimana pada kenyataannya hanyalah sebuah fungsi Javascript biasa yang mengembalikan nilai/isi yang telah dirubah ke huruf kapital.

<!-- Note that the pipe syntax is not part of JavaScript syntax, therefore you cannot mix filters inside expressions; you can only append them at the end of an expression. -->
Harap diperhatikan bahwa sintaks pipa bukan merupakan bagian sintaks JavaScript, jadi anda tidak dapat mencampur filter di dalam ekspresi:

<!-- Filters can be chained: -->
Filter juga dapat digunakan secara berantai:

``` html
{{ message | filterA | filterB }}
```

<!-- Filters can also take arguments: -->
Filter juga dapat menerima argumen:

``` html
{{ message | filterA 'arg1' arg2 }}
```

<!-- The filter function always receives the expression's value as the first argument. Quoted arguments are interpreted as plain string, while un-quoted ones will be evaluated as expressions. Here, the plain string `'arg1'` will be passed into the filter as the second argument, and the value of expression `arg2` will be evaluated and passed in as the third argument. -->
Fungsi filter selalu mengembalikan nilai ekspresi sebagai argumen pertama. Argumen yang diberi tanda kutip diinterpretasi sebagai string murni, sementara argumen yang tidak diberi tanda kutip akan dievaluasi sebagai ekspresi. Dalam contoh diatas, string murni `'arg1'` akan diteruskan ke filter sebagai argumen kedua, dan nilai dari ekspresi `arg2` akan dievaluasi dan dieruskan sebagai argumen ketiga.

## Directives

<!-- Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **binding expressions**, so the rules about JavaScript expressions and filters mentioned above apply here as well. A directive's job is to reactively apply special behavior to the DOM when the value of its expression changes. Let's review the example we saw in the introduction: -->
Directive adalah atribut spesial dengan awalan `v-`. Nilai atribut directive diharapkan menjadi **binding expressions**, jadi aturan tentang ekpresi JavaScript dan filter yang disebutkan diatas juga berlaku disini. Tugas sebuah directive adalah secara reaktif menerapkan perilaku spesial ke DOM ketika nilai dari ekspresi tersebut berubah. Mari kita membahas ulang contoh yang kita lihat di bab pengenalan:


``` html
<p v-if="greeting">Halo!</p>
```

<!-- Here, the `v-if` directive would remove/insert the `<p>` element based on the truthiness of the value of the expression `greeting`. -->
Disini, directive `v-if` akan menghapus/menempatkan elemen `<p>` berdasarkan nilai kebenaran dari ekspresi `greeting`.

### Argumen

<!-- Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute: -->
Beberapa directive dapat menerima sebuah "argumen", disimbolkan dengan dengan tanda titik dua setelah nama directive. Sebagai contoh, directive `v-bind` digunakan untuk memperbarui sebuah atribut HTML secara reaktif:

``` html
<a v-bind:href="url"></a>
```

<!-- Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`. You may have noticed this achieves the same result as an attribute interpolation using `{% raw %}href="{{url}}"{% endraw %}`: that is correct, and in fact, attribute interpolations are translated into `v-bind` bindings internally. -->
Pada contoh diatas, `href` adalah sebuah argumen, yang akan memberi tahu directive `v-bind` untuk mengikat atribut elemen `href` ke nilai dari ekspresi `url`. Anda mungkin memperhatikan bahwa hal tersebut memberikan hasil yang sama dengan interpolasi atribut menggunakan `{% raw %}href="{{url}}"{% endraw %}`: itu benar, dan faktanya, interpolasi atribut diterjemahkan menjadi ikatan `v-bind` secara internal.

<!-- Another example is the `v-on` directive, which listens to DOM events: -->
Contoh lain adalah directive `v-on`, yang mendengarkan event DOM:

``` html
<a v-on:click="doSomething">
```

<!-- Here the argument is the event name to listen to. We will talk about event handling in more detail too. -->
Contoh diatas menunjukkan argumen adalah event apa yang didengarkan/diperhatikan. Kita juga akan membahas tentang penanganan event secara detail.

### Modifiers

<!-- Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.literal` modifier tells the directive to interpret its attribute value as a literal string rather than an expression: -->
Pengubah/modifiers adalah akhiran spesial yang disimbolkan dengan tanda titik, yang mengindikasikan sebuah directive seharusnya diikat dengan berbagai cara spesial. Sebagai contoh, modifier `.literal` memberi tahu directive untuk menginterpretasi nilai atribut dia sebagai literal string daripada sebuah ekpresi.

``` html
<a v-bind:href.literal="/a/b/c"></a>
```

<!-- Of course, this seems pointless because we can just do `href="/a/b/c"` instead of using a directive. The example here is just for demonstrating the syntax. We will see more practical uses of modifiers later. -->
Tentu saja, ini menjadi hal yang tak berarti karena kita bisa saja melakukan `href="/a/b/c"` dibanding kita menggunakan sebuah directive. Contoh disini hanya untuk menunjukkan sintaks. Kita akan melihat lebih banyak penggunaan praktis modifiers nanti.

## Cara Singkat Penulisan

<!-- The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building an [SPA](https://en.wikipedia.org/wiki/Single-page_application) where Vue.js manages every template. Therefore, Vue.js provides special shorthands for two of the most often used directives, `v-bind` and `v-on`: -->
Awalan `v-` memberikan tanda visual untuk mengidentifikasi atribut spesifik untuk Vue di template anda. Ini akan sangat berguna ketika anda menggunakan Vue.js untuk menerapkan perilaku dinamis ke beberapa markup yang telah ada, tetapi terasa banyak,bertele-tele untuk beberapa directive yang sering digunakan. Pada waktu yang sama, kebutuhan akan awalan `v-` menjadi kurang penting ketika kita membangun sebuah [SPA](https://en.wikipedia.org/wiki/Single-page_application) dimana Vue.js mengelola semua template. Dengan demikian, Vue.js menyediakan cara singkat penulisan spesial untuk dua directive yang sering digunakan, `v-bind` dan `v-on`:

### Cara Singkat Penulisan `v-bind`

``` html
<!-- sintaks lengkap -->
<a v-bind:href="url"></a>

<!-- cara singkat penulisan -->
<a :href="url"></a>

atau

<!-- sintaks lengkap -->
<button v-bind:disabled="someDynamicCondition">Tombol</button>

<!-- cara singkat penulisan -->
<button :disabled="someDynamicCondition">Tombol</button>
```

### Cara Singkat Penulisan `v-on`

``` html
<!-- sintaks lengkap -->
<a v-on:click="doSomething"></a>

<!-- cara singkat penulisan -->
<a @click="doSomething"></a>
```

<!-- They may look a bit different from normal HTML, but all Vue.js supported browsers can parse it correctly, and they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later. -->
Contoh diatas mungkin terlihat sedikit berbeda dengan HTML biasa, tapi semua peramban/browsers yang didukung Vue.js dapat menguraikannya dengan benar, dan mereka tidak terlihat di hasil akhir markup yang telah dirender. Cara penulisan singkat opsional, tapi anda akan menyukainya ketika anda belajar akan kegunaannya nanti.
