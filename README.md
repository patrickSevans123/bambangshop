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

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough? </br>
Subscriber perlu diimplementasikan sebagai sebuah interface pada aplikasi Bambang Shop karena subscriber pasti terdiri dari berbagai jenis subscriber yang men-subscribe subject berbeda. Penggunaan interface ini dilakukan untuk menerapkan Dependency Inversion Principle dimana setiap kode harus bergantung pada abstraksi, bukan implementasi konkrit.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case? </br>
Dalam konteks id dan url yang unique pada Subscriber. DashMap akan menjadi opsi terbaik karena dipastikan bahwa setiap elemen pada DashMap merupakan elemen yang unik. Di sisi lain, Vec memerlukan waktu yang cukup kompleks, yaitu O(n) untuk memeriksa apakah suatu elemen memiliki duplikat. Vec juga tidak di-design untuk melakukan multi-threading. Oleh karena itu, DashMap lebih baik digunakan untuk kasus ini.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead? </br>
Singleton pattern dan DashMap memiliki peranan yang berbeda pada kode. Singleton pattern berfungsi untuk memastikan suatu class hanya memiliki sebuah instance. DashMap sendiri memiiliki fungsi untuk menyimpan data sebagai dictionary. Dalam kasus ini, kita membutuhkan keduanya, yaitu singleton pattern dan DashMap. Singleton digunakan agar class hanya memiliki sebuah instance dan DashMap digunakan agar suatu instance dapat diakses dalam proses multithreading.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model? </br>
Pemisahan "Service" dan "Repository" dilakukan untuk memenuhi prinsip Single Responsibility Principle dimana sebuah objek dari suatu role hanya bisa melakukan yang role itu lakukan. Service melakukan bagiannya dimana Service mengurus logika aplikasi yang memproses sebuah input dari bagian lain untuk mengupdate status dari model terkait. Repository juga melakukan bagiannya untuk mengurus penyimpanan data, terutama mengenai menyimpan model ke dalam database.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model? </br>
Apabila kita hanya menggunakan Model dan tidak menggunakan interaksi antar model seperti Program, Subscriber, dan Notification, satu model dengan yang lain akan berinteraksi secara langsung. Artinya, hal ini akan meningkatkan dependensi (coupling), melanggar prinsip Single Responsibility Principle dan menyebabkan kesulitan dalam melakukan maintenance.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects. </br>
Postman merupakan suatu aplikasi yang berfungsi untuk mengirimkan request ke aplikasi kita dan melihat response dari aplikasi tersebut. Aplikasi ini biasanya digunakan untuk mengecek apakah aplikasi me-response suatu request dari pengguna dengan benar.


#### Reflection Publisher-3
