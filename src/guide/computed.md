---
title: Computed Properties
type: guide
order: 5
---

<!-- In-template expressions are very convenient, but they are really meant for simple operations only. Templates are meant to describe the structure of your view. Putting too much logic into your templates can make them bloated and hard to maintain. This is why Vue.js limits binding expressions to one expression only. For any logic that requires more than one expression, you should use a **computed property**. -->
In-template expression memang sangat mudah untuk digunakan, tapi hanya dapat dipakai untuk operasi sederhana saja. Template dimaksudkan untuk membuat view Anda. Terlalu banyak logic di dalam template dapat membuatnya sesak / penuh kode, dan sulit untuk di-maintain. Inilah mengapa Vue.js membatasi binding expression hanya untuk satu expression saja. Untuk logic apapun yang memerlukan lebih dari satu expression, Anda diharuskan menggunakan **computed property**

<!-- ### Basic Example -->
### Contoh Dasar

``` html
<div id="example">
  a={{ a }}, b={{ b }}
</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    // a computed getter
    b: function () {
      // `this` points to the vm instance
      return this.a + 1
    }
  }
})
```

Hasil:

{% raw %}
<div id="example" class="demo">
  a={{ a }}, b={{ b }}
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    b: function () {
      return this.a + 1
    }
  }
})
</script>
{% endraw %}

<!-- Here we have declared a computed property `b`. The function we provided will be used as the getter function for the property `vm.b`: -->
Di sini kita mendeklarasikan sebuah computed property `b`. Fungsi yang kami sediakan akan digunakan sebagai getter function untuk property `vm.b`:

``` js
console.log(vm.b) // -> 2
vm.a = 2
console.log(vm.b) // -> 3
```

<!-- You can open the console and play with the example vm yourself. The value of `vm.b` is always dependent on the value of `vm.a`. -->
Anda dapat membuka console dan bermain dengan contoh vm yang Anda buat sendiri. Value dari `vm.b` selalu bergantung pada value dari `vm.a`

<!-- You can data-bind to computed properties in templates just like a normal property. Vue is aware that `vm.b` depends on `vm.a`, so it will update any bindings that depends on `vm.b` when `vm.a` changes. And the best part is that we've created this dependency relationship declaratively: the computed getter function is pure and has no side effects, which makes it easy to test and reason about. -->
Anda dapat melakukan data-bind untuk computed property di dalam template seperti property normal biasa. Vue.js tahu bahwa `vm.b` tergantung pada `vm.a`, jadi ia akan meng-update setap binding yang bergantung pada `vm.b` ketika value `vm.a` berubah. Dan bagian terbaiknya adalah kita membuat dependency relationship (ketergantungan yang saling berhubungan) ini secara deklaratif: computed getter function murni dan tanpa efek samping, yang mana hal tersebut membuatnya menjadi mudah digunakan.

### Computed Property vs. $watch

<!-- Vue.js does provide an API method called `$watch` that allows you to observe data changes on a Vue instance. When you have some data that needs to change based on some other data, it is tempting to use `$watch` - especially if you are coming from an AngularJS background. However, it is often a better idea to use a computed property rather than an imperative `$watch` callback. Consider this example: -->
Vue.js juga menyediakan method API yang bernama `$watch` yang mengizinkan Anda untuk mengamati perubahan data pada Vue instance. Ketika Anda memiliki beberapa data yang perlu berubah berdasarkan data yang lain, cobalah untuk menggunakan `$watch` - terutama jika Anda sebelumnya sudah terbiasa dengan AngularJS. Bagaimanapun, merupakan ide yang bagus untuk menggunakan computed property daripada menggunakan `$watch` callback yang impoeratif itu. Perhatikan contoh berikut:

``` html
<div id="demo">{{fullName}}</div>
```

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  }
})

vm.$watch('firstName', function (val) {
  this.fullName = val + ' ' + this.lastName
})

vm.$watch('lastName', function (val) {
  this.fullName = this.firstName + ' ' + val
})
```

<!-- The above code is imperative and repetitive. Compare it with a computed property version: -->
Kode di atas adalah kode yang impoeratif dan repetitif. Bandingkan dengan versi computed property seperti di bawah ini:

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

<!-- Much better, isn't it? -->
Lebih baik, bukan?

### Computed Setter

<!-- Computed properties are by default getter-only, but you can also provide a setter when you need it: -->
Computed properties pada mulanya hanyalah sebuah getter (getter-only), akan tetapi Anda juga dapat menyediakan sebuah setter ketika membutuhkannya:

``` js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

<!-- Now when you call `vm.fullName = 'John Doe'`, the setter will be invoked and `vm.firstName` and `vm.lastName` will be updated accordingly. -->
Sekarang ketika Anda memanggil `vm.fullName = 'John Doe'`, setter tersebut akan terpanggil dan `vm.firstName` dan `vm.lastName`juga akan ikut terbaharui.

<!-- The technical details behind how computed properties are updated are [discussed in another section](reactivity.html#Inside-Computed-Properties) dedicated to the reactivity system. -->
Detil teknis dibalik cara kerja computed property [didiskusikan halaman yang lain](reactivity.html#Inside-Computed-Properties) yang membahas tentang reactivity system (sistem reaktifitas).
