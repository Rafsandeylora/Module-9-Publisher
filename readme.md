# Tutorial 8 - Publisher

Nama: Rafsanjani 
NPM: 2406495400

## Reflection 1

### 1. How much data will the publisher program send to the message broker in one run?

Dalam satu kali eksekusi, program publisher akan mengirim 5 data/event ke message broker. Setiap event bertipe `UserCreatedEventMessage` dan berisi dua field, yaitu `user_id` dan `user_name`.

Event yang dikirim adalah:

1. `2406495400-Amir`
2. `2406495400-Budi`
3. `2406495400-Cica`
4. `2406495400-Dira`
5. `2406495400-Emir`

### 2. The URL `amqp://guest:guest@localhost:5672` is the same as in the subscriber program. What does it mean?

URL tersebut sama karena publisher dan subscriber harus terhubung ke message broker RabbitMQ yang sama. Publisher menggunakan URL ini untuk mengirim event ke RabbitMQ, sedangkan subscriber menggunakan URL ini untuk menerima event dari RabbitMQ.

Bagian-bagiannya adalah:

- `guest` pertama adalah username RabbitMQ.
- `guest` kedua adalah password RabbitMQ.
- `localhost` berarti RabbitMQ berjalan di komputer lokal.
- `5672` adalah port AMQP yang digunakan program Rust untuk berkomunikasi dengan RabbitMQ.

Karena keduanya memakai broker dan queue yang sama, event yang dikirim publisher ke queue `user_created` dapat diterima oleh subscriber.