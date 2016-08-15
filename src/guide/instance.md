---
title: The Vue Instance
type: guide
order: 3
---

#Konstruktor

<!-- Every Vue.js app is bootstrapped by creating a **root Vue instance** with the `Vue` constructor function: -->
Setiap aplikasi Vue.js dibootstrapp dengan membuat sebuah **root Vue Instance** dengan fungsi konstruktor `Vue`:

``` js
var vm = new Vue({
  // options
})
```

<!-- A Vue instance is essentially a **ViewModel** as defined in the [MVVM pattern](https://en.wikipedia.org/wiki/Model_View_ViewModel), hence the variable name `vm` you will see throughout the docs. -->
Sebuah Vue instance secara esensial adalah **ViewModel** seperti yang didefinisikan di pola [MVVM](https://en.wikipedia.org/wiki/Model_View_ViewModel), jadi variabel bernama `vm` akan sering anda lihat sepanjang dokumentasi.

<!-- When you instantiate a Vue instance, you need to pass in an **options object** which can contain options for data, template, element to mount on, methods, lifecycle callbacks and more. The full list of options can be found in the [API reference](/api). -->
Ketika anda menginstant sebuah Vue instance, anda perlu melewatkan sebuah **options object** yang dapat menampung opsi untuk data, template, elemen untuk dikaitkan, metode, lifecycle callbacks, dll. Daftar lengkap opsi tersebut bisa dilihat di [referensi API](/api).

<!-- The `Vue` constructor can be extended to create reusable **component constructors** with pre-defined options: -->
Konstruktor `Vue` dapat diperluas untuk membuat **component constructors** yang dapat digunakan ulang kembali dengan opsi yang telah didefinisikan sebelumnya:

``` js
var MyComponent = Vue.extend({
  // extension options
})

// semua instance `MyComponent` dibuat oleh
// opsi yang telah didefinisikan sebelumnya
var myComponentInstance = new MyComponent()
```

<!-- Although you can create extended instances imperatively, in most cases you will be registering a component constructor as a custom element and composing them in templates declaratively. We will talk about the component system in detail later. For now, you just need to know that all Vue.js components are essentially extended Vue instances. -->
Meskipun anda bisa membuat instance yang diperluas secara imperatif, dalam banyak kasus anda akan mendaftarkan sebuah konstruktor komponen sebagai custom element dan menyusunnya di template secara deklaratif. Kita akan membahas tentang sistem komponen secara detail nanti. Untuk sekarang, anda hanya perlu tahu bahwa semua komponen Vue.js pada dasarnya merupakan perluasan Vue instances.

## Properti and Methods

<!-- Each Vue instance **proxies** all the properties found in its `data` object: -->
Setiap Vue instance **proxies** semua properti yang ada di objek `data`:
``` js
var data = { a: 1 }
var vm = new Vue({
  data: data
})

vm.a === data.a // -> true

// merubah setting properti juga mempengaruhi data asli
vm.a = 2
data.a // -> 2

// ... dan juga sebaliknya
data.a = 3
vm.a // -> 3
```

<!-- It should be noted that only these proxied properties are **reactive**. If you attach a new property to the instance after it has been created, it will not trigger any view updates. We will discuss the reactivity system in detail later. -->
Perlu diperhatikan bahwa hanya proxied properties yang **reaktif**. Jika anda menyematkan sebuah properti baru ke instance setelah instance tersebut dibuat, instance tersebut tidak akan memicu apapun perubahan yang terjadi pada tampilan. Kita akan membahas tentang reactivity system secara detail nanti.

<!-- In addition to data properties, Vue instances expose a number of useful instance properties and methods. These properties and methods are prefixed with `$` to differentiate from proxied data properties. For example: -->
Sebagai tambahan untuk properti data, Vue instance mempunyai beberapa instance properti dan methods yang berguna. Properti dan methods ini diawali dengan `$` untuk membedakan dari proxied data properties. Sebgai contoh:

``` js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true

// $watch adalah sebuah contoh instance method
vm.$watch('a', function (newVal, oldVal) {
  // callback ini akan dipanggil ketika `vm.a` berubah
})
```

<!-- Consult the [API reference](/api) for the full list of instance properties and methods. -->
Silahkan lihat [referensi API](/api) untuk mengetahui daftar lengkap instance properties dan methods.

## Siklus Hidup Vue Instance

<!-- Each Vue instance goes through a series of initialization steps when it is created - for example, it needs to set up data observation, compile the template, and create the necessary data bindings. Along the way, it will also invoke some **lifecycle hooks**, which give us the opportunity to execute custom logic. For example, the `created` hook is called after the instance is created: -->
Setiap Vue instance melalui sebuah rangkaian langkah-langkah inisialisasi ketika dia dibuat -  sebagai contoh, dia perlu untuk mempersiapkan observasi data, kompilasi template, dan membuat ikatan data yang diperlukan. Selama proses berlangsung, dia juga akan meminta beberapa **lifecycle hooks**, yang akan memberikan kita kesempatan untuk mengeksekusi custom logic. Sebagai contoh, `created` hook dipanggil setelah instance dibuat.


``` js
var vm = new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` menunjuk kepada vm instance
    console.log('a is: ' + this.a)
  }
})
// -> "a is: 1"
```

<!-- There are also other hooks which will be called at different stages of the instance's lifecycle, for example `compiled`, `ready` and `destroyed`. All lifecycle hooks are called with their `this` context pointing to the Vue instance invoking it. Some users may have been wondering where the concept of "controllers" lives in the Vue.js world, and the answer is: there are no controllers in Vue.js. Your custom logic for a component would be split among these lifecycle hooks. -->
Ada juga hook/pengaitan lain yang akan dipanggil pada keadaan yang berbeda dari siklus hidup instance, sebagai contoh `compiled`, `ready`, dan `destroyed`. Semua siklus hidup hook/pengaitan dipanggil dengan `this` konteks yang menunjuk ke Vue instance yang memanggilnya. Beberapa pengguna mungkin bertanya-tanya dimana konsep "controller" berada di Vue.js, dan jawabannya adalah: tidak ada controller/pengendali di Vue.js. Custom logic anda untuk sebuah komponen akan dipisah di antara siklus hidup hook/pengaitan.

## Diagram Siklus Hidup

<!-- Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but this diagram will be helpful in the future. -->
Di bawah ini adalah sebuah diagram siklus hidup instance. Untuk sekarang anda tidak perlu memahaminya secara keseluruhan, tetapi diagram ini nantinya akan berguna suatu hari nanti.

![Siklus Hidup](/images/lifecycle.png)
