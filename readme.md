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

## Running RabbitMQ as Message Broker

Saya menjalankan RabbitMQ menggunakan Docker dengan command:

```bash
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management
```

RabbitMQ berhasil berjalan dan RabbitMQ Management UI dapat diakses melalui `http://localhost:15672`.

![RabbitMQ running](./images/rabbitmq-running.png)

## Sending and Processing Event

Ketika publisher dijalankan dengan `cargo run`, publisher mengirim 5 event `UserCreatedEventMessage` ke message broker RabbitMQ melalui queue `user_created`.

Subscriber yang sedang berjalan sudah terhubung ke RabbitMQ dan mendengarkan queue yang sama, sehingga event yang masuk ke broker dapat diterima dan diproses oleh subscriber.

Ini disebut event-driven architecture karena publisher tidak memanggil subscriber secara langsung. Publisher hanya menerbitkan event ke message broker. Setelah itu, subscriber yang tertarik pada queue tersebut akan mengambil dan memproses event secara asynchronous.

![Subscriber received messages](./images/subscriber-received-messages.png)

![Publisher cargo run](./images/publisher-cargo-run.png)

![RabbitMQ connection or queue](./images/rabbitmq-connection-or-queue.png)

## Monitoring Chart Based on Publisher

Setelah publisher dijalankan beberapa kali, RabbitMQ Management UI menunjukkan spike pada chart. Spike tersebut muncul karena setiap kali publisher dijalankan, program mengirim 5 event ke message broker.

Jika publisher dijalankan 3 kali, maka ada sekitar 15 event yang dikirim ke RabbitMQ. Semakin sering publisher dijalankan dalam waktu singkat, semakin terlihat kenaikan aktivitas publish message pada chart RabbitMQ.

![RabbitMQ publisher spike chart](./images/rabbitmq-publisher-spike-chart.png)
