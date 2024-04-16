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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. ***In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?***

    Dalam implementasi pola Observer, sangat disarankan untuk menggunakan trait dibandingkan dengan single Model struct. Trait di Rust memungkinkan definisi fungsi yang harus diimplementasikan oleh setiap subscriber, memastikan bahwa semua tipe subscriber memenuhi persyaratan tertentu yang diperlukan untuk pola ini. Akan tetapi dalam kasus BambangShop karena subscriber hanya terdiri dari satu tipe saja maka penggunaan single Model struct sudah cukup.

2. ***id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?***

    Penggunaan DashMap dianjurkan daripada Vec karena DashMap memberi kemudahan dalam menjaga keunikan dan efisiensi dalam pencarian. DashMap, yang bekerja seperti dictionary, memungkinkan akses cepat berbasis key yang menghasilkan waktu pencarian rata-rata yang konstan, O(1). Ini lebih efisien dibandingkan dengan Vec, di mana pencarian elemen membutuhkan iterasi melalui semua elemen sehingga memiliki kompleksitas waktu O(n).

3. ***When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?***

    Dalam konteks aplikasi yang melibatkan banyak thread, memilih DashMap merupakan opsi yang lebih efektif daripada mengandalkan Singleton pattern. DashMap, yang menyediakan thread-safe hashmap dengan penguncian per entry, memfasilitasi akses data yang lebih cepat dan efisien, ideal untuk situasi konkurensi tinggi. Berbeda dengan Singleton pattern yang sering kali memerlukan penguncian seluruh data, penggunaan DashMap memungkinkan operasi yang lebih granular dengan penguncian yang lebih minim. Hal ini secara signifikan meningkatkan throughput dan performa sistem, menghindari bottleneck yang umum terjadi pada penggunaan pola Singleton dalam lingkungan multi-thread.

#### Reflection Publisher-2

1. ***In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?***

    "Service" dan "Repository" dipisah untuk mendukung prinsip Single Responsibility (SRP) dan Separation of Concerns. "Repository" bertanggung jawab untuk interaksi langsung dengan database, menyediakan cara yang konsisten untuk akses data, sementara "Service" mengelola logic yang lebih kompleks. Pendekatan ini dapat meningkatkan modularitas, yang memudahkan testing, maintenance, dan scalability dari aplikasi.

2. ***What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?***

    Jika hanya menggunakan Model untuk menangani logic dan akses data, kompleksitas kode dalam setiap model akan meningkat signifikan. Hal ini dapat menyebabkan Model menjadi terlalu besar dan sulit untuk dikelola karena harus menangani banyak tanggung jawab yang seharusnya dibagi ke dalam komponen yang lebih kecil dan spesifik. Interaksi antara berbagai model seperti Program, Subscriber, dan Notification juga akan menjadi lebih rumit dan kurang jelas, karena batasan antara apa yang merupakan data dan apa yang merupakan logic menjadi tidak jelas.

3. ***Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.***

    Iya, Postman merupakan alat yang sangat berguna untuk menguji API dalam pengembangan aplikasi. Dengan Postman, pengguna dapat dengan mudah mengirimkan permintaan HTTP ke endpoint API, memeriksa respons, dan bahkan mengautomasi pengujian menggunakan runner yang mereka sediakan. Fitur seperti environment variables memungkinkan pengguna untuk menyimpan konfigurasi yang dapat digunakan di berbagai permintaan, sementara fitur pre-request scripts dan tests membantu dalam menyiapkan kondisi sebelum permintaan dan memvalidasi respons yang diterima. Fitur-fitur ini sangat membantu dalam group project atau proyek software engineering lainnya.

#### Reflection Publisher-3

1. ***Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?***

    Dalam kasus tutorial ini, variasi Observer Pattern yang digunakan adalah model Push. Dalam model ini, publisher secara aktif mengirimkan (push) data ke subscriber setiap kali terjadi perubahan atau peristiwa baru, seperti pada implementasi `notify` dalam `NotificationService` yang mengirimkan pemberitahuan ke semua subscriber terkait setiap kali produk dibuat, diperbarui, atau dihapus.

2. ***What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)***

    - **Keuntungan**
        - Subscriber memiliki kontrol lebih atas informasi yang diterima dengan memilih kapan meminta update, memungkinkan pengaturan informasi sesuai kebutuhan masing-masing.
        - Publisher menghemat bandwidth dan pemrosesan karena hanya mengirim data saat diminta, efisien untuk situasi di mana perubahan sering terjadi tetapi tidak semua relevan bagi setiap subscriber.
    - **Kerugian**
        - Terdapat latensi tambahan dalam penerimaan data karena subscriber harus membuat permintaan untuk mendapatkan update, yang menunda waktu respon.
        - Beban untuk memeriksa update terletak pada subscriber, yang bisa tidak efisien jika banyak subscriber yang perlu sering memeriksa pembaruan.

3. ***Explain what will happen to the program if we decide to not use multi-threading in the notification process.***

    - Server mungkin mengalami penundaan dalam merespons permintaan lain saat sedang mengirim notifikasi ke subscriber karena perlu menyelesaikan pengiriman notifikasi ke satu subscriber sebelum melanjutkan ke yang berikutnya.
    - Jika notifikasi membutuhkan waktu lama untuk diproses, terutama jika jumlah subscriber banyak, permintaan pengguna lain mungkin mengalami timeout karena menunggu notifikasi selesai diproses.
    - Subscriber mungkin menerima notifikasi dengan kecepatan yang berbeda tergantung pada beban server saat itu, yang dapat menyebabkan pengalaman pengguna yang tidak konsisten.