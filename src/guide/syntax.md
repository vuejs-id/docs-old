---
title: Sintaks Data Binding
type: guide
order: 4
---

<!-- Vue.js uses a DOM-based templating implementation. This means that all Vue.js templates are essentially valid, parsable HTML enhanced with some special attributes. Keep that in mind, since this makes Vue templates fundamentally different from string-based templates. -->
Vue.js menggunakan implementasi template berbasis DOM (Document Object Model). Inilah yang menjadikan semua template Vue.js pada dasarnya valid, parsable HTML yang ditingkatkan dengan beberapa atribut spesial. Ingatlah, karena inilah yang membuat template pada Vue pada dasarnya berbeda dengan template berbasis string.

<!-- ## Interpolations -->
## Interpolasi

<!-- ### Text -->
### Teks

<!-- The most basic form of data binding is text interpolation using the "Mustache" syntax (double curly braces): -->
Bentuk paling dasar dari data binding adalah interpolasi teks dengan menggunakan sintaks "Kumis" (kurung kurawal ganda):

``` html
<span>Message: {{ msg }}</span>
```

<!-- The mustache tag will be replaced with the value of the `msg` property on the corresponding data object. It will also be updated whenever the data object's `msg` property changes. -->
Tag kumis tersebut akan digantikan dengan value (nilai) dari property `msg` pada data object yang berhubungan. Ia juga akan diperbaharui ketika properti data object `msg` berubah.

<!-- You can also perform one-time interpolations that do not update on data change: -->
Anda juga dapat melakukan one-time interpolations (interpolasi satu kali) dimana tidak akan diperbaharui ketika data berubah:

``` html
<span>Tulisan ini tidak akan berubah: {{* msg }}</span>
```

### Raw HTML

<!-- The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use triple mustaches: -->
Tanda kurung kurawal ganda meng-interpretasi data sebagai teks biasa, dan bukan tag HTML. Jika Anda ingin menghasilkan tag HTML, Anda membutuhkan tripel kurung kurawal.

``` html
<div>{{{ raw_html }}}</div>
```

<!-- The contents are inserted as plain HTML - data bindings are ignored. If you need to reuse template pieces, you should use [partials](/api/#partial). -->
Konten yang disisipkan sebagai HTML biasa - data binding mengabaikannya. Jika Anda membutuhkan potongan template yang dapat digunakan ulang, Anda dapat menggunakan [partials](/api/#partial).

<!-- <p class="tip">Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). Only use HTML interpolation on trusted content and **never** on user-provided content.</p> -->
<p class="tip">Me-render HTML semaunya secara dinamis pada website dapat membahayakan, karena ia dapat dengan mudah menyebabkan [serangan XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Hanya gunakan interpolasi HTML pada konten yang terpercaya, dan **jangan pernah** menggunakannya pada user-provided content (konten yang berasal dari pengguna).</p>

<!-- ### Attributes -->
### Atribut

<!-- Mustaches can also be used inside HTML attributes: -->
Kurung kurawal juga dapat digunakan di dalam atribut HTML:

``` html
<div id="item-{{ id }}"></div>
```

<!-- Note that attribute interpolations are disallowed in Vue.js directives and special attributes. Don't worry, Vue.js will raise warnings for you when mustaches are used in wrong places. -->
Perlu dicatat jika interpolasi atribut tidak diperkenankan untuk digunakan pada directive Vue.js dan atribut khusus. Jangan panik, Vue.js akan memunculkan peringatan untuk Anda jika kurung kurawal dipakai pada tempat yang salah.

## Binding Expressions

<!-- The text we put inside mustache tags are called **binding expressions**. In Vue.js, a binding expression consists of a single JavaScript expression optionally followed by one or more filters. -->
Teks yang kita masukkan kedalam kurung kurawal disebut dengan **binding expressions**. Pada Vue.js, sebuah binding expression terdiri dari expression JavaScript tunggal yang secara opsional diikuti oleh satu atau lebih filter.

### JavaScript Expressions

<!-- So far we've only been binding to simple property keys in our templates. But Vue.js actually supports the full power of JavaScript expressions inside data bindings: -->
Sejauh ini kita hanya mem-binding property key yang sederhana pada template kita. Namun Vue.js sebenarnya mendukung kekuatan penuh dari JavaScript expression di dalam data binding:

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}
```

<!-- These expressions will be evaluated in the data scope of the owner Vue instance. One restriction is that each binding can only contain **one single expression**, so the following will **NOT** work: -->
Expressions tersebut akan dievaluasi di dalam data scope pada Vue instance. Namun perlu diingat, bahwa setiap binding hanya boleh terdapat **satu expression tunggal**, jadi contoh di bawah ini **TIDAK** dapat berjalan:

``` html
<!-- ini adalah sebuah statement, dan bukan sebuah expression: -->
{{ var a = 1 }}

<!-- flow control juga tidak dapat bekerja, gunakan ternary expressions -->
{{ if (ok) { return message } }}
```

### Filters

<!-- Vue.js allows you to append optional "filters" to the end of an expression, denoted by the "pipe" symbol: -->
Vue.js mengizinkan Anda untuk menambahkan "filter" opsional untuk mengubah hasil akhir dari sebuah expression. Fitler dilambangkan dengan simbol "pipe" (pipa).

``` html
{{ message | capitalize }}
```

<!-- Here we are "piping" the value of the `message` expression through the built-in `capitalize` filter, which is in fact just a JavaScript function that returns the capitalized value. Vue.js provides a number of built-in filters, and we will talk about how to write your own filters later. -->
Di sini kita "mem-pipa" value dari expression `message` melalui filter bawaan yang bernama `capitalize`, dimana sebenarnya filter tersebut hanyalah sebuah function pada JavaScript yang me-return value dengan huruf kapital. Vue.js menyediakan beberapa filter bawaan, dan kita akan membicarakannya serta bagaimana cara membuat filter kita sendiri nanti.

<!-- Note that the pipe syntax is not part of JavaScript syntax, therefore you cannot mix filters inside expressions; you can only append them at the end of an expression. -->
Perlu dicatat bahwa simbol "pipa" bukanlah bagian dari sintaks JavaScript, karena itu anda tidak dapat mencampurkan filter di dalam sebuah expression; Anda hanya dapat menambahkannya di akhir sebuah expression.

<!-- Filters can be chained: -->
Filter juga dapat dirantai/disambungkan (chained):

``` html
{{ message | filterA | filterB }}
```

Filters can also take arguments:

``` html
{{ message | filterA 'arg1' arg2 }}
```

<!-- The filter function always receives the expression's value as the first argument. Quoted arguments are interpreted as plain string, while un-quoted ones will be evaluated as expressions. Here, the plain string `'arg1'` will be passed into the filter as the second argument, and the value of expression `arg2` will be evaluated and passed in as the third argument. -->
Function filter selalu menerima value expression sebagai argument pertama. Argument yang berada dalam tanda kutip dianggap sebagai string biasa, sementara itu argument tanpa tanda kutip dianggap sebagai expression. Di atas, terdapat string biasa `'arg1'` yang akan di-passing ke dalam filter sebagai argument ke-dua, dan value dari expression `arg2` akan di evaluasi dan di-pass ke dalam argument ke-tiga.

## Directives

<!-- Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **binding expressions**, so the rules about JavaScript expressions and filters mentioned above apply here as well. A directive's job is to reactively apply special behavior to the DOM when the value of its expression changes. Let's review the example we saw in the introduction: -->
Directive adalah atribut khusus yang diawali dengan awalan `v-`. Value pada atribut directive diharapkan menjadi **binding expressions**, jadi aturan tentang expression JavaScript dan filter yang disebutkan di atas juga berlaku di sini. Tugas sebuah directive adalah untuk melakukan behavior (tingkah laku) spesial secara reaktif kepada DOM ketika sebuah value dari expression tersebut berubah. Mari kita lihat contoh yang sudah kita lihat pada halaman introduction.

``` html
<p v-if="greeting">Hello!</p>
```

<!-- Here, the `v-if` directive would remove/insert the `<p>` element based on the truthiness of the value of the expression `greeting`. -->
Di sini, directive `v-if` akan me-remove/meng-insert elemen `<p>` berdasarkan value dari expression `greeting`.

### Arguments

<!-- Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute: -->
Beberapa directive dapat mengambil "argument", yang ditandai dengan tanda titik dua (colon) setelah nama directive. Sebagai contoh, directive `v-bind` dapat digunakan untuk mengupdate sebuah atribut HTML secara reaktif.

``` html
<a v-bind:href="url"></a>
```

<!-- Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`. You may have noticed this achieves the same result as an attribute interpolation using `{% raw %}href="{{url}}"{% endraw %}`: that is correct, and in fact, attribute interpolations are translated into `v-bind` bindings internally. -->
Di sini `href` adalah argument, dimana memberitahu kepada directive `v-bind` untuk mem-bind elemen atribut `href` kepada value yang dihasilkan dari expression `url`. Anda mungkin memperhatikan bahwa cara ini memiliki hasil yang sama dengan cara ketika kita menggunakan interpolasi atribut `{% raw %}href="{{url}}"{% endraw %}`. Memang benar, karena pada kenyataanya secara internal interpolasi atribut diterjemahkan ke dalam binding `v-bind`.

<!-- Another example is the `v-on` directive, which listens to DOM events: -->
Contoh lainnya adalah directive `v-on`, dimana ia mendegarkan/memantau event (kejadian) yang terjadi pada DOM:

``` html
<a v-on:click="doSomething">
```

<!-- Here the argument is the event name to listen to. We will talk about event handling in more detail too. -->
Di sini argumen tersebut adalah nama event yang ingin didengarkan (dipantau). Kita akan berbicara mengenai penanganan event (event handling) lebih rinci nanti.

### Modifiers

<!-- Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.literal` modifier tells the directive to interpret its attribute value as a literal string rather than an expression: -->
Modifiers adalah postfixes spesial yang ditandai dengan sebuah tanda titik, dimana hal tersebut mengindikasikan jika sebuah directive harusnya dilambungkan/diloncatkan (bound) secara khusus. Sebagai contoh, modifier `.literal` memberitahu kepada directive untuk meng-interpretasi value atributnya sebagai sebuah string biasa (literal string) dan bukan sebagai expression:

``` html
<a v-bind:href.literal="/a/b/c"></a>
```

<!-- Of course, this seems pointless because we can just do `href="/a/b/c"` instead of using a directive. The example here is just for demonstrating the syntax. We will see more practical uses of modifiers later. -->
Tentu saja, ini nampaknya sangat tidak berarti karena kita dapat cukup mengetik `href="/a/b/c"` daripada menggunakan sebuah directive. Contoh di sini hanyalah untuk mendemonstrasikan sintaks. Kita akan lihat pemanfaatan modifier nanti.

## Shorthands

<!-- The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building an [SPA](https://en.wikipedia.org/wiki/Single-page_application) where Vue.js manages every template. Therefore, Vue.js provides special shorthands for two of the most often used directives, `v-bind` and `v-on`: -->
Awalan `v-` menyediakan bantuan visual untuk mengidentifikasi atribut spesifik milik Vue.js pada template Anda. Ini sangat bermanfaat ketika Anda menggunakan Vue.js untuk memakai behavior (tingkah laku) dinamis kepada beberapa markup yang ada, akan tetapi terkadang terasa terlalu panjang terutama pada beberapa directive yang sering digunakan. Pada saat yang sama, kebutuhan akan awalan `v-` menjadi kurang begitu penting ketika Anda membangun sebuah [SPA](https://en.wikipedia.org/wiki/Single-page_application) dimana Vue.js mengatur setiap template yang ada. Oleh karenanya, Vue.js menyediakan shorthands untuk dua directive yang paling sering dipakai, yakni `v-bind` dan `v-on`:

### `v-bind` Shorthand

``` html
<!-- full syntax -->
<a v-bind:href="url"></a>

<!-- shorthand -->
<a :href="url"></a>

or

<!-- full syntax -->
<button v-bind:disabled="someDynamicCondition">Button</button>

<!-- shorthand -->
<button :disabled="someDynamicCondition">Button</button>
```


### `v-on` Shorthand

``` html
<!-- full syntax -->
<a v-on:click="doSomething"></a>

<!-- shorthand -->
<a @click="doSomething"></a>
```

<!-- They may look a bit different from normal HTML, but all Vue.js supported browsers can parse it correctly, and they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later. -->
Mungkin terlihat sedikit berbeda jika dibandingkan dengan halaman HTML biasa, namun semua browser yang mendukung Vue.js dapat mem-parse kode tersebut dengan sempurna, dan kode tersebut tidak tampil pada layar browser. Shorthand tersebut memang bersifat opsional, tapi Anda akan menyukainya ketika Anda mempelajari lebih lanjut mengenai penggunaannya nanti.
