# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Menurut saya, saat ini penggunaan satu model `Subscriber` sudah cukup untuk menerapkan pola Observer. Saat ini hanya ada satu entitas yang diamati yaitu model `Product` dengan atribut tipe yang dapat di-subscribe oleh `Subscriber`. Karena observer dalam sistem ini hanya terdiri dari satu jenis maka penggunaan interface atau trait masih belum diperlukan. Interface akan lebih bermanfaat apabila terdapat banyak jenis observer yang memiliki perilaku berbeda dan perlu diakomodasi dalam sistem yang lebih fleksibel.

> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Penggunaan `DashMap` dalam kasus ini sudah tepat walaupun `id` pada `Product` dan `url` pada `Subscriber` bersifat unik. Hal ini karena kita ingin memetakan dan menyimpan `Subscriber` berdasarkan `product_type` dari `Product`. Jika kita menggunakan `Vec`, kita tetap memerlukan struktur data tambahan untuk menghubungkan index `Vec` dengan `product_type`. Selain itu, menyimpan `url` dalam `Vec` akan menyulitkan proses penghapusan `Subscriber` karena membutuhkan pencarian secara linear untuk menemukan entri yang sesuai. Dengan `DashMap`, proses pencarian dan penghapusan dapat dilakukan secara efisien dalam waktu konstan. `DashMap` juga memudahkan kita menyimpan pasangan `id` dan `url` dalam satu struktur yang ringkas dan terorganisir dan tidak perlu membuat dua array terpisah. Selain itu `DashMap` juga memiliki kemampuan akses secara _concurrent_, sehingga jauh lebih aman dan optimal digunakan apabila akan dijalankan dalam _multi-threading_ di masa depan.

> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Karena BambangShop berjalan secara _multi-threading_ maka penggunaan `DashMap` akan lebih tepat dibandingkan menggunakan Singleton pattern. `DashMap` memliki kemampuan _concurrent access_ secara _thread-safe_, sehingga data `SUBSCRIBERS` bisa diakses tanpa perlu _locking_ manual. Sementara itu, menerapkan Singleton di Rust juga lebih  kompleks karena keterbatasan pada static _variable_ dan _mutability_. Untuk menyimpan `Subscriber` berdasarkan `product_type`, `DashMap` juga lebih praktis, efisien, dan minim risiko dibanding menggunakan Singleton.

#### Reflection Publisher-2

#### Reflection Publisher-3
