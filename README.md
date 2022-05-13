# Arduino IDE

Pustaka ini digunakan Arduino IDE untuk menghubungkan perangkat ke platform Dashboard menggunakan protokol MQTT berdasarkan [arduino IDE](https://github.com/bagussatoto/arduino.git)


Unduh versi terbaru dari [rilis](https://github.com/bagussatoto/arduino.git) atau juga lebih baik jika unduh dan install melalui Library Manager pada Arduino IDE.

Contoh berikut menggunakan ESP32 Development Board dan terhubung dengan broker EMQX:
```c++
#include <WiFi.h>
#include <Dashboard.h>
#include "Connection.h"

WiFiClient net;
Dashboard dashboard;
DashboardTimer timer;                   // Gunakan timer agar dapat mengeksekusi perintah setiap sekian milidetik tanpa blocking.

// Ubah nilai berikut sesuai jaringan Anda.
const char ssid[] = "ssid";
const char pass[] = "pass";
const char server[] = "broker.emqx.io";
const String authProject = "YOUR_DASHBOARD_AUTH_PROJECT";
// Atur Client ID dengan nomor acak. Anda bisa menggantinya dengan Client ID apapun.
// String CleintId = "YourClientId";
const String clientId = "bagussatoto-" + String(random(0xffff), HEX);

void setupDashboard() {
  Serial.println("Menghubungkan ke WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(1000);
  }

  Serial.print("\nMenghubungkan ke server/broker");
  while (!dashboard.connect(clientId.c_str())) {
    Serial.print(".");
    delay(1000);
  }
  Serial.println("\nTerhubung ke server!");

  dashboard.subscribe(authProject+"/data/#");
}

void subscribe(String &topic, String &message) {
  Serial.println("data masuk: \n" + topic + " - " + message);
}

void publish() {
  dashboard.publish(authProject, "data/hello", "world");     // Publish ke topik "authproject/data/hello" dengan pesan "world".
}

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, pass);
  dashboard.begin(server, net);

  dashboard.onMessage(subscribe);       // Lakukan subscribe pada fungsi subscribe().
  timer.setInterval(1000, publish);     // Lakukan publish setiap 1000 milidetik.

  setupDashboard();
}

void loop() {
  dashboard.loop();
  timer.run();                          // Jalankan timer.

  // Periksa apakah perangkat masih terhubung.
  if (!dashboard.connected()) {
    setupDashboard();
  }
}
```

## 👦🏽 Siapa pembuat aplikasi ini?

| Profile        |  Keterangan                      |
|----------------|----------------------------------|
| Nama           | Bagus Budi Satoto                |
| Jurusan        | S1 - Informatika                 |
| Kampus         | Universitas Amikom Yogyakarta    |

## 🧑🏽‍💻 Social Media 
<div>
      <p><i class="fas fa-envelope-open-text"></i><a href="mailto:" target="_blank"> Mail : bagusbudi1308@gmail.com</a></p>   
      <p><i class="fab fa-whatsapp"></i> <a href="https://wa.me/082136094607"> Wa.Business : Bagus
      <p><i class="fab fa-whatsapp"></i> <a href="https://wa.me/08988325547"> Wa.Pribadi : Bagus Satoto
      <p><i class="fab fa-github"></i> <a href="https://github.com/bagussatoto"> Github : Bagus Budi Satoto</a></p>  
      <p><i class="fab fa-facebook"></i> <a href="https://www.facebook.com/bagussatoto1"> Facebook : Bagus Budi Satoto</a></p>
      <p><i class="fab fa-instagram"></i> <a href="https://www.instagram.com/bagus_satoto1"> Instagram : Bagus Budi Satoto</a></p>    
      <p><i class="fab fa-linkedin"></i> <a href="https://www.linkedin.com/in/bagussatoto/"> Linkedin : Bagus Budi Satoto</a></p>
      <p><i class="fab fab-paypal"></i> <a href="https://www.PayPal.Me/bagussatoto1"> PayPal : Bagus Budi Satoto </a></p>
      
</div>



## Perangkat Yang Didukung

Pustaka menggunakan Arduino IDE untuk berinteraksi dengan perangkat keras jaringan. Artinya pustaka ini dapat digunakan pada perangkat keras apapun yang memiliki interaktifitas API tersebut termasuk papan dan shield seperti:

 - ESP8266 Development Board
 - ESP32 Development Board
 - Arduino Ethernet
 - Arduino Ethernet Shield
 - Arduino YUN & YUN-Shield
 - Arduino WiFi Shield
 - Arduino/Genuino WiFi101 Shield
 - Arduino MKR GSM 1400
 - Intel Galileo/Edison


## 📌 Request Fitur Baru dan Pelaporan Bug

Anda dapat meminta fitur baru maupun melaporkan bug melalui menu **issues** yang sudah disediakan oleh GitHub (lihat menu di atas), posting issues baru dan kita akan berdiskusi disana.

## 🛒 Berkontribusi

Siapapun dapat berkontribusi pada proyek ini mulai dari pemrograman, pembuakan buku manual, sampai dengan mengenalkan produk ini kepada Mahasiswa 
Untuk belajar agar mengurangi kesenjangan pendidikan teknologi dengan cara membuat postingan issue di repository ini.

## Lisensi

Kode program dilisensikan dibawah GNU GENERAL PUBLIC LICENSE
