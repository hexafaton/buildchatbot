================================================================================
               APLIKASI TARA (CHATBOT WHATSAPP KANTOR PERTANAHAN)
       Layanan Asisten Virtual Pertanahan - Kantor Pertanahan Kab. Bojonegoro
================================================================================

1. DESKRIPSI UMUM APLIKASI
--------------------------
TARA adalah aplikasi asisten virtual berbasis WhatsApp (Chatbot AI) yang dirancang khusus untuk melayani masyarakat di wilayah hukum Kantor Pertanahan Kabupaten Bojonegoro (Kementerian Agraria dan Tata Ruang/Badan Pertanahan Nasional) secara 24/7. 

Aplikasi ini menggunakan teknologi Kecerdasan Buatan (AI) untuk menjawab pertanyaan seputar informasi layanan pertanahan, memberikan syarat dokumen pengurusan tanah, melakukan pencarian informasi pendukung secara cerdas di web, serta melacak status pemrosesan berkas pemohon secara real-time dari database internal kantor pertanahan. Dilengkapi pula dengan dashboard monitor berbasis web untuk memantau status koneksi WhatsApp melalui pemindaian kode QR.


2. TEKNOLOGI YANG DIGUNAKAN (TECH STACK)
----------------------------------------
Aplikasi dikembangkan menggunakan kombinasi teknologi modern demi menjamin kecepatan respons, keandalan, dan kemudahan integrasi:

- Core Runtime       : Node.js (Runtime JavaScript sisi server yang cepat dan efisien)
- API & Web Server   : Express.js (Menyediakan backend REST API dan menyajikan halaman monitor status QR WhatsApp)
- Desain & Interface : Vanilla HTML & CSS (Tampilan pemantauan status QR yang minimalis, modern, responsif, dan bebas ketergantungan library pihak ketiga)
- WhatsApp Gateway   : whatsapp-web.js / Baileys (Menghubungkan aplikasi Node.js dengan protokol resmi WhatsApp Web/Mobile Client)
- AI & LLM Engine    : OpenAI GPT-4o API (Sebagai otak pemroses pesan warga dengan kemampuan natural language processing yang humanis dan cerdas)
- Database Integrasi : MySQL / MariaDB (Terhubung langsung ke tabel `daftar_berkas` BPN lokal untuk validasi nomor berkas secara instan)
- Web Search Engine  : Tavily API & DuckDuckScrape (Membantu AI melakukan pencarian informasi aktual di internet saat mendeteksi pertanyaan dinamis)
- Keamanan Kode      : javascript-obfuscator (Mengamankan logika kode produksi dari pencurian properti intelektual sebelum dideploy)
- Manajemen Proses   : PM2 (Process manager untuk menjaga aplikasi tetap aktif berjalan di latar belakang server VPS Ubuntu)


3. FITUR-FITUR UTAMA APLIKASI
-----------------------------
A. CHATBOT PINTAR 24/7 (AI-POWERED)
   - Komunikasi Alami: Memahami bahasa sehari-hari masyarakat (non-kaku) dan memberikan jawaban yang santun, informatif, serta runtut.
   - Deteksi Niat (Intent Detection): Otomatis memilah apakah pesan bertipe sapaan umum, pertanyaan syarat layanan, pelacakan berkas, atau kalkulasi angka.

B. PELACAKAN STATUS BERKAS REAL-TIME
   - Validasi Nomor Berkas: Warga cukup mengirimkan pesan dengan format nomor berkas (misalnya `1234/2026`).
   - Verifikasi Dua Langkah: Sistem akan mencari data di database terlebih dahulu, lalu meminta konfirmasi nama pemohon demi menjaga privasi sebelum menampilkan status pemrosesan berkas secara detail.

C. SYARAT LAYANAN PERTANAHAN LENGKAP
   - Menyimpan daftar basis data terstruktur berisi 7 Kategori Utama dan 17+ Sub-layanan pertanahan (termasuk Peralihan Hak Jual Beli, Waris, Hibah, Roya, Pemecahan Bidang, Validasi Analog, hingga SKPT) beserta persyaratan dokumen detailnya.

D. INTEGRASI PENDUKUNG (TOOLS INTERN)
   - Holiday Checker: Mengecek apakah hari tertentu adalah Libur Nasional, guna memberikan informasi jadwal loket fisik yang tepat.
   - Calculator Tool: Membantu kalkulasi aritmatika sederhana langsung di dalam percakapan.
   - Web Search Tool: Melakukan pencarian cepat di web untuk menjawab informasi di luar prosedur baku pertanahan.

E. MONITOR STATUS & QR SCANNER WEB PORTAL
   - Menyediakan tampilan halaman web dinamis untuk administrator/petugas IT guna melakukan pemindaian QR code untuk pertama kali atau menyambungkan kembali sesi WhatsApp yang terputus.


4. STRUKTUR FOLDER PROJEK
-------------------------
- agent/                 : Logika utama agen kecerdasan buatan (AI)
  - core/                : Inti pemrosesan pesan masuk dan alur respons.
  - prompts/             : Kumpulan template perintah dasar (system prompt) AI.
  - tools/               : Sensor/alat bantu yang bisa dipanggil oleh AI:
    - database_tool.js   : Kueri database MariaDB untuk mencari berkas.
    - web_search.js      : Mesin pencarian eksternal via Tavily/DuckDuckScrape.
    - holiday_tool.js    : Logika validasi tanggal libur nasional Indonesia.
    - calculator.js      : Modul penyelesaian perhitungan matematika dasar.
    - greeting_tool.js   : Generator respons pembuka dan penutup ramah.
    - mysql_tool.js      : Utilitas koneksi database raw MySQL.
    - file_handler.js    : Handler operasi berkas sementara.
  - workflows/           : Alur percakapan terstruktur (session handling).
- api/                   : Server backend Express.js
  - public/              : Aset visual monitor web (CSS, JS, dan HTML statis).
  - server.js            : Server web penyedia QR Code scanner dan pemantau status bot.
- config/                : Pengaturan konfigurasi lingkungan
  - env.js               : Loader dan verifikator variabel lingkungan (.env).
- dist/                  : Folder berisi kode terkompilasi dan ter-obfuscate siap rilis.
- llm/                   : Penghubung modul model bahasa besar (OpenAI Client).
- memory/                : Manajemen memori percakapan jangka pendek (Session Manager).
- scripts/               : Berisi skrip otomatisasi build (`build.js`) untuk enkripsi kode.
- index.js               : Titik masuk (entry point) utama untuk menjalankan chatbot.
- layanan.js             : Database statis daftar persyaratan dokumen seluruh layanan BPN.


5. ALUR KERJA PENGGUNA (USER FLOW)
----------------------------------
A. ALUR MASYARAKAT (WHATSAPP USER):
   Mengirim Chat -> Pesan Diproses AI -> (Jika Tanya Syarat) TARA Menyajikan Syarat Instan -> (Jika Cek Berkas) Input Nomor Berkas -> Konfirmasi Nama Pemohon -> TARA Menyajikan Progress Berkas Terbaru.

B. ALUR ADMINISTRATOR / PETUGAS IT (WEB MONITOR):
   Akses Web Status -> Scan QR Code WhatsApp -> Bot Aktif -> Pantau Status Koneksi Bot -> Deploy & Kelola Proses via PM2 di VPS Ubuntu.
================================================================================
