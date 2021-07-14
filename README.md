
# **Inventory System - Project Documentation**


## **Introduction**

Dalam menjalankan kegiatan peminjaman barang pada lab FTI, mahasiswa seringkali mengalami kesulitan untuk melakukan peminjaman barang dikarenakan metode manual yang masih dilakukan, yaitu menggunakan kertas dan alat tulis. Pendataan data peminjaman juga susah dilakukan, seperti pada aspek pelacakan terhadap barang yang sedang dipinjam atau telah dikembalikan, kapan barang tersebut dipinjam, dan oleh siapa barang tersebut dipinjam. 

Menggunakan kuesioner singkat, mahasiswa UMN menilai bahwa proses peminjaman lab FTI kurang praktis. Kesulitan utama yang dihadapi ketika meminjam adalah harus menulis peminjaman dengan menggunakan pulpen secara manual. Berdasarkan masalah tersebut, penulis dari grup 1 mengusulkan pembuatan proses pendataan barang secara automasi dengan menggunakan teknologi sistem tertanam RFID. Sistem RFID dinilai akan mempercepat proses peminjaman karena pencatatan dengan paperless lebih mudah dan efisien. Administrasi data dengan paperless dinilai lebih baik karena lebih rentan terhadap hilangnya dokumen, lebih mudah pencarian data, serta lebih mudah presentasi data. [1]

Sistem yang diusulkan akan dibuat dengan menggunakan Raspberry Pi Zero sebagai CPU dengan menggunakan RC522 RFID module untuk memberikan label pada barang dan membaca barang sewaktu seorang mahasiswa ingin meminjam barang tersebut dan juga untuk membaca kartu mahasiswa untuk mendata data sang peminjam barang. Sistem tersebut mampu merekam seluruh transaksi pinjam meminjam yang terjadi, sehingga kita dapat mengetahui kapan barang tersebut dipinjam, siapa peminjam barang tersebut. 


### **System Overview**
![image](https://user-images.githubusercontent.com/58260433/125574311-932c9d45-0ec4-4497-92d9-aa5d0886faca.png)

Berikut merupakan overview dari sistem peminjaman yang kelompok kami develop. Sistem utama kami menggunakan Raspberry Pi Zero karena kapabilitas yang disediakan cocok dengan keperluan sistem kami. Raspberry Pi Zero akan ditambahkan modul RFID RC522 untuk membaca RFID dan modul ini berkomunikasi dengan protokol SPI. Raspberry Pi juga tersambung dengan monitor (dapat berupa komputer ataupun monitor biasa) untuk menyediakan UI yang jelas. User dapat memilih apakah ingin meminjam atau mengembalikan barang. UI juga menyediakan petunjuk yang jelas agar proses peminjaman lebih lancar. 

Sistem menyimpan data pada Google Sheets. Data berupa barang apa saja yang tersedia pada lab, siapa saja yang pernah meminjam, serta peminjaman yang sedang aktif. Untuk melakukan update data pada Google Sheet, Raspberry Pi Zero akan tersambung dengan internet menggunakan WiFi Module yang ada pada Raspberry Pi Zero. Hasil dari proses peminjaman atau pengembalian ditunjukkan pada UI. 


## **Hardware**


### **Overview**

![image](https://user-images.githubusercontent.com/58260433/125574336-e49fb8a5-dc7b-46e4-847e-a9c4160aa263.png)
![image](https://user-images.githubusercontent.com/58260433/125574339-1a63fcf1-5178-4ece-b112-0cba33ffb113.png)

Dalam pembuatan sistem ini kami memilih untuk menggunakan Raspberry Pi Zero. Raspberry Pi Zero merupakan single computer board yang memiliki fleksibilitas tinggi sehingga kita dapat mengkonfigurasi Raspberry Pi Zero sesuai dengan kebutuhan kita. Kelebihan lain dari Raspberry Pi Zero merupakan output HDMI yang dapat kita langsung gunakan untuk membuat interface terhadap user. Selain Raspberry Pi Zero, kami menggunakan RC522 RFID sebagai modul untuk melakukan scan sinyal RFID. 

* RFID Reader diberikan sumber daya oleh Raspberry Pi Zero dengan 3.3V pin.
* RFID Reader berkomunikasi dengan Raspberry Pi Zero menggunakan protokol SPI.
* Raspberry Pi Zero mendapatkan sumber daya dari power supply 5V.
* Koneksi WiFi digunakan untuk mengambil informasi dari Google Sheet API.
* Desktop monitor merupakan tampilan yang akan user lihat ketika melakukan peminjaman.


### **Raspberry Pi Zero**

Raspberry Pi Zero merupakan sebuah komputer yang sangat kecil dan dapat dimodifikasi sesuai dengan keinginan kita.

Berikut merupakan spesifikasi lengkap dari Raspberry Pi Zero:

    * Dimensions: 65mm × 30mm × 5mm
    * CPU: ARM11 running at 1GHz 
    * RAM: 512MB 
    * Wireless: 2.4GHz 802.11n wireless LAN
    * CPU: ARM11 running at 1GHz 
    * RAM: 512MB 
    * Power: 5V, supplied via micro USB connector
    * Video & Audio: 1080P HD video & stereo audio via mini-HDMI connector
    * GPIO: 40-pin GPIO, unpopulated
    * GPIO Benchmark: 1.752 kHz
    * Memory Throughput: 150.04 MBps for Memory Bandwidth Read, 290.19 for Memory Bandwidth Write 
    * Power Draw: 0.8 Watts on idle; 1.6 Watts on Load

Berikut merupakan konektivitas pin dari Raspberry Pi Zero



    * SS connects to Pin 24.
    * SCK connects to Pin 23.
    * MOSI connects to Pin 19.
    * MISO connects to Pin 21.
    * GND connects to Pin 6.
    * RST connects to Pin 22.
    * 3.3v connects to Pin 1.
    * Raspberry Pi dengan RFID Reader
    * GPIO, dengan SPI Protocol, 3.3V 
    * Raspberry Pi dengan PC
    * Micro USB to USB


### **RC522 RFID Reader Module**

RC522 merupakan sebuah modul yang dapat mendeteksi sinyal 13.56MHz. Dengan modul ini, kita dapat mengidentifikasi barang-barang yang akan dipinjam, dikembalikan serta KTM mahasiswa.

Berikut merupakan spesifikasi lengkap dari RC522:



    * Frequency Range: 13.56 MHz
    * Supply Voltage: 2.5 ~3.3 V
    * Logic Inputs: 5V tolerant
    * Working Temperature: -30 ~ +85 C
    * Max Read Range: 5 cm
    * SPI interface data rate: Max 10 Mbps
    * Built-in Temperature Sensors to stop RF transmission if overheating is detected by switching off the antenna drivers. RF transmission is resumed if temperature is below 125 C

Secara sederhana, modul ini dapat direpresentasikan dengan gambar diatas. Pertama-tama sinyal analog akan diterima oleh antena dan diproses dalam analog interface. Kemudian Contactless UART akan berkomunikasi dan mengatur protokol yang akan digunakan untuk berkomunikasi dengan host. Kemudian hasil dari contactless UART akan ditampung di dalam FIFO buffer dan dilanjutkan komunikasi dengan host. Alur yang sama juga digunakan untuk mentransmisi informasi dari host menuju ke analog interface.



* EL-MF1-T1 RFID Tag
    * Frequency: 13.56 MHz
    * Memory: 1 KB
    * Communication Baud Rate: 106 Kbit/s
    * Read range: 1-5 cm
    * R/W Life Cycle: 100.000 times
    * Data Retention: > 10 years

Tag berbentuk keychain yang dapat dikaitkan pada peralatan laboratorium. Keyring/ split ring bisa dilepas dan tag ditempelkan pada perangkat menggunakan Double-sided Tape.



* MF1 IC S50
    * Frequency: 13.56 MHz
    * Memory: 1 KB
    * Communication Baud Rate: 106 Kbit/s
    * High data integrity: 16 Bit CRC, parity, bit coding, bit counting 
    * True anticollision 
    * Typical ticketing transaction: &lt; 100 ms (including backup management)


## **Component Setup**

![image](https://user-images.githubusercontent.com/58260433/125574373-5207dd2e-919b-49ee-b9ed-b1ed0dd9bce8.png)
![image](https://user-images.githubusercontent.com/58260433/125574376-01480cf6-85c4-4dae-a137-b9e79ad74f16.png)

Raspberry pi dan RFID reader dibungkus dengan kotak hasil 3D printer. Komponen-komponen tersebut disolder pada perfboard. Akses port dari komponen tersedia pada lubang yang kotak sudah sediakan. 


## **Software**

![image](https://user-images.githubusercontent.com/58260433/125574397-de16c2d0-d683-48be-ad5e-8e37f3bc90fe.png)


### **Software Module Overview**

Dalam project ini, kami menggunakan bahasa pemrograman Python dikarenakan library yang banyak tersedia di internet sehingga dapat mendukung kami membuat project ini. Secara garis besar, sistem bekerja berdasarkan modul berikut:



* Reservation dan Returning Module

    Reservation dan Returning module menggunakan data yang didapat RFID Reader untuk melakukan reservasi barang dan pengembalian barang. RFID Reader berupa contended-child, dan akses untuk module tersebut diatur oleh MAIN module, Reservation dan Returning module tidak akan dijalankan bersamaan.

* RFID Reader Module

    Raspberry Pi menggunakan library SimpleMFRC522 untuk berkomunikasi dengan RFID Reader. Library ini menyediakan fungsi untuk berkomunikasi dengan RFID Reader dan menyediakan implementasi metode komunikasi yang digunakan, yaitu SPI protocol. Fungsi yang relevan untuk sistem ini berupa fungsi read RFID tag dan fungsi write RFID tag.

* Database (gspread library)

    Module yang mengatur interaksi sistem dengan database. Google Sheets digunakan sebagai database untuk menyimpan data barang dan data peminjaman. Sistem berkomunikasi dengan Google Sheets API menggunakan gspread library. gspread menyediakan fungsi untuk autentikasi sehingga Raspberry Pi dapat memodifikasi isi dari sheets. Library ini berkomunikasi dengan melalui internet. Implementasi konektivitas ke internet diatur oleh Raspbian OS dan hanya digunakan melalui database module.

* Display Module

    Module tkinter yang digunakan untuk menampilkan UI adalah blocking GUI, CPU thread Raspberry Pi tidak dapat menjalankan proses lain ketika menampilkan GUI tersebut. Tkinter berupa event-based programming, maka tkinter module akan block semua proses pada thread sampai suatu event terjadi. 


    RFID reader library juga berupa proses blocking, maka diterapkan multithreading untuk mengatasi dua proses blocking. Tkinter module berupa thread utama yang dijalankan oleh Raspberry Pi. Ketika peminjaman atau pengembalian terjadi (event), tkinter module akan menjalankan proses RFID reader pada thread lain, dan menunggu hasil dari thread tersebut. Ketika RFID reader berhasil membaca dan mengembalikan hasil bacaan, tkinter module akan memproses hasil bacaan berdasarkan peminjaman atau pengembalian



### **Software Relation Diagram**

![image](https://user-images.githubusercontent.com/58260433/125574408-ad00dac4-a13a-4005-867f-3404e711fd2b.png)


* TKinter GUI adalah library python yang digunakan untuk menampilkan UI. Menyediakan dua pilihan utama, yaitu untuk meminjam dan untuk mengembalikan barang.  
* Raspberry Pi Zero berkomunikasi dengan RFID dengan python library SimpleMFRC522.
* Data yang diterima akan diunggah ke internet menggunakan library gspread. WiFi Module dapat melakukan Wireless Connection ke Router terdekat.
* OS Raspberry Pi yang memiliki GUI akan ditampilkan kepada user melalui Monitor yang terhubung menggunakan HDMI. Dalam tampilan tersebut akan terdapat tampilan file Google Spreadsheet yang terhubung dengan Raspberry Pi.
* Sumber tegangan Raspberry Pi didapat dari power socket di tembok melalui adapter dan kabel micro USB.
* Menyimpan data sementara dari database pada temporary data


### **Software Files**



* [Write.py] User dapat menuliskan nama benda yang ingin ditempel dengan tag. Untuk penggunaan sistem ini, user harus mengisi data berupa ID dan nama benda tersebut. 
* [Read.py] User dapat membaca isi dari RFID tag, berupa ID tag dan nama benda. 
* [main_gui.py]  User dapat menjalankan file untuk menampilkan GUI dari sistem. 


### **Software GUI**

Berikut tampilan dari GUI sistem kami. 

![image](https://user-images.githubusercontent.com/58260433/125574419-d7ea1dcf-9446-433b-8b0e-791e26041f65.png)

Ketika membuka program [main_gui.py], user disambut dengan window sebagai berikut. User harus melakukan scan KTM sebelum memulai proses peminjaman. Jika Tag yang di scan tidak dikenal, login akan gagal.

![image](https://user-images.githubusercontent.com/58260433/125574427-61885911-87ec-464f-957e-910e322861f2.png)

Setelah sukses login, button untuk melakukan peminjaman atau pengembalian, serta button logout, akan dapat digunakan. 

![image](https://user-images.githubusercontent.com/58260433/125574451-d6e70b2f-e4d8-4e99-9097-59b22ec41977.png)

Proses peminjaman dimulai dengan melakukan klik Button “BORROW Lab Equipments”. User harus scan RFID tag barang yang ingin dipinjam. Untuk konfirmasi proses peminjaman, user melakukan scan KTM untuk mengakhiri transaksi peminjaman. GUI menampilkan barang apa saja yang user sudah scan. Text berwarna kuning berupa warning jika error terjadi.

![image](https://user-images.githubusercontent.com/58260433/125574459-1fbee740-2b6d-4fd9-ae25-39e1e99be1e9.png)

Jika proses peminjaman berhasil, akan ditampilkan dialog seperti ini. 

![image](https://user-images.githubusercontent.com/58260433/125574465-7a33fb57-0aa9-4f40-a563-6bca29320450.png)

Proses pengembalian dimulai dengan melakukan klik button “RETURN Lab Equipments”. GUI menampilkan barang apa saja yang user sedang meminjam. User harus scan RFID tag tiap barang untuk mengembalikan barang tersebut. User melakukan scan KTM untuk konfirmasi proses pengembalian. Text berwarna kuning menandakan error yang terjadi. Barang yang berwarna hijau adalah barang yang ditandai akan dikembalikan. 

![image](https://user-images.githubusercontent.com/58260433/125574470-d58583a1-b842-4279-9f4b-557848416216.png)

Jika proses pengembalian berhasil, akan ditampilkan dialog seperti ini. User dapat kemudian melakukan logout dengan button “Logout”.  


### **Reference**

[1] Susanty, Wiwin & Thamrin, Taqwan & Erlangga, Erlangga & Cucus, Ahmad. (2012). Document Management System Based on Paperless. 
