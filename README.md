<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa judul penelitian Anda?", "id": "Deteksi berat dan harga buah." },
  { "en": "Metode utama yang digunakan?", "id": "Algoritma YOLO untuk deteksi objek." },
  { "en": "Kenapa memilih judul ini?", "id": "Otomatisasi proses pengecekan spesifikasi buah." },
  { "en": "Apa masalah utamanya?", "id": "Pengecekan manual tidak efisien." },
  { "en": "Tujuan utama penelitian ini?", "id": "Membangun model deteksi buah otomatis." },
  { "en": "Objek apa yang dideteksi?", "id": "Beberapa jenis buah dalam dataset." },
  { "en": "Parameter buah apa saja?", "id": "Jenis, ukuran, bentuk, dan posisi." },
  { "en": "Kenapa menggunakan algoritma YOLO?", "id": "Kemampuan deteksi objek secara real-time." },
  { "en": "Versi YOLO berapa yang dipakai?", "id": "Menggunakan model YOLOv8n pada implementasi." },
  { "en": "Apa latar belakang masalahnya?", "id": "Pemanfaatan potensi buah tropis Indonesia." },
  { "en": "Apa itu Revolusi Oranye?", "id": "Inisiatif kebijakan optimalkan produksi buah." },
  { "en": "Bagaimana cara YOLO mengenali buah?", "id": "Melatih model pada citra digital." },
  { "en": "Bagaimana cara estimasi beratnya?", "id": "Ekstraksi fitur visual bounding box." },
  { "en": "Bagaimana perbandingan dengan metode manual?", "id": "Analisis efisiensi, kecepatan, dan konsistensi." },
  { "en": "Apa batasan dataset Anda?", "id": "Hanya menguji buah dalam dataset." },
  { "en": "Faktor apa yang tidak diukur?", "id": "Tekstur atau kondisi dalam buah." },
  { "en": "Perangkat input utama apa?", "id": "Menggunakan Modul Camera Pi." },
  { "en": "Apa itu arsitektur terdistribusi?", "id": "Sistem client-server untuk pemrosesan." },
  { "en": "Kenapa pakai arsitektur terdistribusi?", "id": "Keterbatasan komputasi pada Raspberry Pi." },
  { "en": "Siapa client dalam arsitektur?", "id": "Raspberry Pi sebagai node sensor." },
  { "en": "Siapa server dalam arsitektur?", "id": "Laptop sebagai node pemroses pusat." },
  { "en": "Apa tugas utama Raspberry Pi?", "id": "Menangkap gambar dan menampilkan hasil." },
  { "en": "Apa tugas utama Laptop?", "id": "Inferensi model deep learning YOLO." },
  { "en": "Bagaimana data gambar dikirim?", "id": "Streaming melalui jaringan Wi-Fi." },
  { "en": "Protokol apa untuk streaming video?", "id": "Menggunakan HTTP (web server video)." },
  { "en": "Bagaimana hasil deteksi dikirim?", "id": "Menggunakan protokol MQTT (publisher-subscriber)." },
  { "en": "Kenapa memilih MQTT?", "id": "Ringan untuk pengiriman data hasil." },
  { "en": "Apa output dari sistem?", "id": "Menampilkan harga dan berat buah." },
  { "en": "Di mana hasil ditampilkan?", "id": "Pada layar LCD I2C 20x4." },
  { "en": "Bagaimana alur kerja sistem?", "id": "Akuisisi, pemrosesan, pengiriman, penampilan." },
  { "en": "Dasar estimasi beratnya apa?", "id": "Luas area piksel bounding box." },
  { "en": "Bagaimana ukuran buah diklasifikasikan?", "id": "Kecil, sedang, atau besar." },
  { "en": "Apa kelemahan estimasi berat ini?", "id": "Sangat sensitif terhadap jarak kamera." },
  { "en": "Bagaimana jika jarak kamera berubah?", "id": "Estimasi berat menjadi tidak akurat." },
  { "en": "Berapa akurasi deteksi jenis buah?", "id": "Mencapai akurasi contoh 96%." },
  { "en": "Berapa akurasi klasifikasi ukuran?", "id": "Mencapai akurasi contoh 91%." },
  { "en": "Apa penyebab error klasifikasi ukuran?", "id": "Objek di batas kategori ukuran." },
  { "en": "Bagaimana stabilitas komunikasi sistem?", "id": "Sangat andal, 100% pesan diterima." },
  { "en": "Berapa latensi end-to-end sistem?", "id": "Rata-rata di bawah satu detik." },
  { "en": "Apa komponen latensi terbesar?", "id": "Waktu pemrosesan inferensi di laptop." },
  { "en": "Apa keunggulan sistem dibanding manual?", "id": "Lebih cepat, konsisten, dan efisien." },
  { "en": "Berapa kecepatan proses sistem?", "id": "Kurang dari satu detik per pengecekan." },
  { "en": "Bagaimana konsistensi sistem ini?", "id": "Menawarkan konsistensi absolut 100%." },
  { "en": "Apa keterbatasan utama sistem?", "id": "Ketergantungan pada jarak kamera konstan." },
  { "en": "Keterbatasan lain dari sistem ini?", "id": "Memerlukan jaringan Wi-Fi yang stabil." },
  { "en": "Saran untuk masalah jarak?", "id": "Implementasi normalisasi skala jarak." },
  { "en": "Bagaimana cara meningkatkan presisi berat?", "id": "Transisi menggunakan model regresi matematis." },
  { "en": "Bagaimana sistemnya bisa lebih mandiri?", "id": "Migrasi ke single-board computer kuat." },
  { "en": "Contoh SBC yang disarankan?", "id": "Perangkat seperti NVIDIA Jetson Nano." },
  { "en": "Fitur apa yang bisa ditambahkan?", "id": "Deteksi kualitas dan tingkat kematangan." },
  { "en": "Bagaimana meningkatkan fleksibilitas model?", "id": "Perluasan dataset dan variasi objek." },
  { "en": "Bagaimana meningkatkan antarmuka pengguna?", "id": "Mengganti LCD dengan layar sentuh." },
  { "en": "Langkah selanjutnya setelah prototipe?", "id": "Merancang Printed Circuit Board (PCB)." },
  { "en": "Mikrokontroler apa yang Anda gunakan?", "id": "Menggunakan Raspberry Pi Zero 2 W." },
  { "en": "Apa fungsi LCD 20x4?", "id": "Menampilkan informasi hasil secara visual." },
  { "en": "Apa fungsi adaptor 5V?", "id": "Sumber daya utama untuk sistem." },
  { "en": "Apa fungsi kabel jumper?", "id": "Menghubungkan antar komponen elektronik." },
  { "en": "Kenapa menggunakan breadboard?", "id": "Media koneksi sementara untuk prototipe." },
  { "en": "Bagaimana alur flowchart programnya?", "id": "Mulai, deteksi, proses, tampilkan hasil." },
  { "en": "Apa yang terjadi jika buah tak dikenal?", "id": "Proses akan berhenti tanpa hasil." },
  { "en": "Apa itu preprocessing citra?", "id": "Proses meningkatkan kualitas gambar." },
  { "en": "Kenapa preprocessing penting?", "id": "Memastikan fitur utama dapat dikenali." },
  { "en": "Bagaimana Anda melatih model?", "id": "Dataset buah yang telah dilabeli." },
  { "en": "Apa itu evaluasi model?", "id": "Menguji akurasi dengan data nyata." },
  { "en": "Apa bedanya dengan penelitian Huda (2025)?", "id": "Penelitian Huda identifikasi kematangan mangga." },
  { "en": "Apa bedanya dengan penelitian Qutrido (2023)?", "id": "Penelitian Qutrido deteksi penyakit tanaman." },
  { "en": "Apa bedanya dengan penelitian Abdulloh (2025)?", "id": "Penelitian Abdulloh deteksi kualitas cabai." },
  { "en": "Apa resolusi modul kamera?", "id": "Menggunakan Modul Kamera 5MP." },
  { "en": "Bagaimana kamera terhubung ke Raspberry?", "id": "Melalui antarmuka port CSI." },
  { "en": "Apa itu node sensor?", "id": "Ujung tombak sistem, berinteraksi fisik." },
  { "en": "Apa itu node pemroses?", "id": "Pusat 'otak' dari sistem ini." },
  { "en": "Bagaimana cara laptop menerima video?", "id": "Koneksi ke siaran via OpenCV." },
  { "en": "Apa itu 'bounding box'?", "id": "Kotak pembatas hasil deteksi objek." },
  { "en": "Tingkat keyakinan deteksi berapa?", "id": "Di atas ambang batas yang ditentukan." },
  { "en": "Bagaimana data harga disimpan?", "id": "Dalam kamus DATA_BUAH pada skrip." },
  { "en": "Apa itu 'ground truth'?", "id": "Data pembanding untuk mengukur akurasi." },
  { "en": "Apakah sistem ini 'real-time'?", "id": "Tidak, latensi di bawah satu detik." },
  { "en": "Kenapa latensi ini bisa diterima?", "id": "Cukup responsif untuk aplikasi kasir." },
  { "en": "Apa paradigma pengembangan yang ditunjukkan?", "id": "Pemisahan pemrosesan dari tugas sensorik." },
  { "en": "Apa kompromi metode estimasi berat?", "id": "Sederhana, cepat, tapi mengorbankan ketahanan." },
  { "en": "Bagaimana efisiensi alur kerja meningkat?", "id": "Integrasi beberapa langkah menjadi satu." },
  { "en": "Apa itu 'human error'?", "id": "Kesalahan yang disebabkan oleh manusia." },
  { "en": "Bagaimana sistem menghilangkan subjektivitas?", "id": "Menawarkan objektivitas terukur dan konsisten." },
  { "en": "Apa itu MQTT 'topic'?", "id": "Alamat tujuan untuk publikasi pesan." },
  { "en": "Apa 'topic' yang Anda gunakan?", "id": "sensor/deteksi untuk data hasil." },
  { "en": "Apa itu MQTT 'broker'?", "id": "Server pusat yang mengelola pesan." },
  { "en": "Apa itu 'subscriber' MQTT?", "id": "Klien yang menerima pesan topik." },
  { "en": "Apa itu 'publisher' MQTT?", "id": "Klien yang mengirim pesan topik." },
  { "en": "Format data yang dikirim MQTT?", "id": "Menggunakan format data JSON." },
  { "en": "Kenapa JSON dipilih?", "id": "Ringan dan mudah di-parsing." },
  { "en": "Sistem operasi pada Raspberry Pi?", "id": "Menggunakan Raspberry Pi OS Lite." },
  { "en": "Library Python apa untuk kamera?", "id": "Menggunakan library seperti Flask/picamera." },
  { "en": "Library Python apa untuk MQTT?", "id": "Menggunakan library Paho-MQTT." },
  { "en": "Apa itu Motion JPEG (MJPEG)?", "id": "Format streaming frame video terkompresi." },
  { "en": "Bagaimana pengujian bersifat holistik?", "id": "Mengevaluasi sistem sebagai satu kesatuan." },
  { "en": "Jarak kamera saat pengujian berapa?", "id": "Dilakukan pada jarak kamera tetap." },
  { "en": "Bagaimana cara menormalkan skala jarak?", "id": "Menggunakan objek referensi berukuran standar." },
  { "en": "Apa itu model regresi?", "id": "Memprediksi nilai kontinu, bukan kategori." },
  { "en": "Kenapa perlu perluasan dataset?", "id": "Meningkatkan fleksibilitas di lingkungan nyata." },
  { "en": "Bagaimana sistem menentukan harga buah?", "id": "Berdasarkan klasifikasi ukuran dan jenis." },
  { "en": "Apakah harga per buah atau per gram?", "id": "Harga ditentukan per buah terdeteksi." },
  { "en": "Bagaimana ambang batas ukuran ditentukan?", "id": "Didefinisikan dalam kamus DATA_BUAH." },
  { "en": "Apakah ambang batas ini tetap?", "id": "Ya, sesuai aturan yang ditetapkan." },
  { "en": "Apa isi dari kamus DATA_BUAH?", "id": "Aturan berat dan harga buah." },
  { "en": "Bagaimana jika ada dua buah berbeda?", "id": "Sistem menghitung total berat harganya." },
  { "en": "Bagaimana jika buah tumpang tindih?", "id": "Dokumen tidak membahas skenario ini." },
  { "en": "Apakah model bisa mendeteksi buah busuk?", "id": "Tidak, hanya mendeteksi jenis buah." },
  { "en": "Saran Anda untuk deteksi kualitas?", "id": "Menambah kemampuan deteksi tingkat kematangan." },
  { "en": "Mengapa memilih Raspberry Pi Zero 2W?", "id": "Ringkas dan hemat daya ideal." },
  { "en": "Apa kelemahan Pi Zero 2W?", "id": "Memiliki keterbatasan komputasi untuk AI." },
  { "en": "Alternatif selain arsitektur terdistribusi?", "id": "Sistem standalone pada SBC kuat." },
  { "en": "Kenapa tidak langsung pakai Jetson Nano?", "id": "Sebagai arah pengembangan sistem selanjutnya." },
  { "en": "Apa keuntungan utama arsitektur Anda?", "id": "Mendelegasikan tugas komputasi AI berat." },
  { "en": "Bagaimana Anda memastikan jarak kamera tetap?", "id": "Pengujian dilakukan pada jarak tetap." },
  { "en": "Akurasi 91% untuk ukuran, cukup?", "id": "Cukup baik untuk validasi metode." },
  { "en": "Bagaimana Anda mengukur latensi?", "id": "Waktu dari gambar ditangkap hingga hasil." },
  { "en": "Apakah latensi termasuk transmisi jaringan?", "id": "Ya, termasuk latensi jaringan video/MQTT." },
  { "en": "Bagaimana sistem menangani error jaringan?", "id": "Dokumen tidak menjelaskan penanganan error." },
  { "en": "Apa kontribusi utama penelitian Anda?", "id": "Efektivitas arsitektur terdistribusi untuk AIoT." },
  { "en": "Apa itu 'fine-tuning' parameter?", "id": "Proses optimasi untuk meningkatkan performa." },
  { "en": "Apakah Anda melakukan 'fine-tuning'?", "id": "Disebutkan sebagai bagian proses optimasi." },
  { "en": "Bagaimana Anda memberi label dataset?", "id": "Studi lain menggunakan Roboflow." },
  { "en": "Dari mana sumber dataset Anda?", "id": "Dataset publik dan pemotretan langsung." },
  { "en": "Berapa banyak gambar di dataset?", "id": "Dokumen tidak merinci jumlah gambar." },
  { "en": "Bagaimana kondisi pencahayaan mempengaruhi sistem?", "id": "Dataset mencakup berbagai kondisi pencahayaan." },
  { "en": "Bagaimana sudut pandang mempengaruhi deteksi?", "id": "Dataset mencakup berbagai sudut pandang." },
  { "en": "Apa fungsi dari skrip di laptop?", "id": "Melakukan inferensi, logika, dan publikasi." },
  { "en": "Library apa yang dipakai untuk inferensi?", "id": "Menggunakan library model YOLOv8n." },
  { "en": "Library apa yang dipakai di laptop?", "id": "OpenCV untuk video, Paho-MQTT." },
  { "en": "Bagaimana Anda memulai seluruh sistem?", "id": "Dokumen tidak menjelaskan prosedur startup." },
  { "en": "Apakah sistem berjalan otomatis?", "id": "Melibatkan dua skrip yang berjalan simultan." },
  { "en": "Bagaimana skrip di Pi dan laptop berkomunikasi?", "id": "Melalui jaringan Wi-Fi lokal." },
  { "en": "Apa alamat IP contoh Anda?", "id": "Contohnya http://192.168.1.6:5000/video_feed." },
  { "en": "Apakah berat yang ditampilkan estimasi?", "id": "Ya, estimasi berdasarkan luas piksel." },
  { "en": "Apa satuan berat yang ditampilkan?", "id": "Ditampilkan dalam satuan gram." },
  { "en": "Apakah penelitian ini menggunakan OpenCV?", "id": "Ya, untuk preprocessing dan koneksi video." },
  { "en": "Contoh preprocessing dengan OpenCV?", "id": "Normalisasi warna dan penghapusan noise." },
  { "en": "Apa itu segmentasi objek?", "id": "Memisahkan objek dari latar belakangnya." },
  { "en": "Bagaimana Anda memvalidasi fungsionalitas komunikasi?", "id": "Pengujian selama 30 menit kontinu." },
  { "en": "Bagaimana Anda mengukur objektivitas sistem?", "id": "Dibandingkan dengan subjektivitas estimasi manual." },
  { "en": "Apa implikasi dari penelitian ini?", "id": "Paradigma kuat pengembangan aplikasi IoT/AI." },
  { "en": "Apakah metode ini bisa untuk sayuran?", "id": "Bisa, dengan melatih dataset sayuran." },
  { "en": "Bagaimana penelitian ini mendukung Revolusi Oranye?", "id": "Adopsi teknologi canggih bidang pertanian." },
  { "en": "Siapa dosen pembimbing Anda?", "id": "Julpri Andika, S.T., M.Sc." },
  { "en": "Dari universitas mana Anda berasal?", "id": "Universitas Mercu Buana Jakarta." },
  { "en": "Tahun berapa penelitian ini diajukan?", "id": "Diajukan pada tahun 2025." },
  { "en": "Bagaimana cara kerja saklar?", "id": "Mematikan dan mengaktifkan alatnya." },
  { "en": "Apa fungsi bracket besi?", "id": "Sebagai penyangga komponen perangkat keras." },
  { "en": "Apa fungsi Micro SD Card?", "id": "Penyimpanan sistem operasi Raspberry Pi." },
  { "en": "Apa fungsi akrilik?", "id": "Sebagai bahan casing atau housing." },
  { "en": "Bagaimana Anda mendefinisikan buah segar?", "id": "Buah yang bisa langsung dikonsumsi." },
  { "en": "Apakah sistem Anda mendeteksi kesegaran?", "id": "Tidak, sistem tidak mendeteksi kesegaran." },
  { "en": "Siapa yang melakukan penelitian serupa?", "id": "Lusiana dkk (2023) untuk kesegaran." },
  { "en": "Apa tantangan penggunaan YOLO?", "id": "Variasi besar warna dan tekstur." },
  { "en": "Bagaimana mengatasi tantangan YOLO tersebut?", "id": "Optimasi parameter atau peningkatan dataset." },
  { "en": "Apa itu 'offload' komputasi?", "id": "Memindahkan tugas berat ke mesin lain." },
  { "en": "Kenapa perlu 'offload' komputasi?", "id": "Karena Raspberry Pi memiliki sumber daya terbatas." },
  { "en": "Apakah sistem bisa bekerja tanpa internet?", "id": "Tidak, perlu Wi-Fi untuk komunikasi lokal." },
  { "en": "Apakah perlu koneksi ke internet global?", "id": "Tidak, hanya perlu jaringan lokal." },
  { "en": "Bagaimana hasil ditampilkan di LCD?", "id": "Pi menerima JSON lalu menampilkannya." },
  { "en": "Bagaimana jika ada lebih dari 3 ukuran?", "id": "Perlu menambah kategori dalam logika." },
  { "en": "Apakah model regresi lebih sulit?", "id": "Memerlukan pengumpulan data berat aktual." },
  { "en": "Apa keuntungan PCB kustom?", "id": "Meningkatkan keandalan dan daya tahan." },
  { "en": "Apa itu 'enklosur'?", "id": "Casing pelindung untuk perangkat elektronik." },
  { "en": "Apakah sistem ini portabel?", "id": "Kurang portabel, tergantung laptop/Wi-Fi." },
  { "en": "Bagaimana membuat sistem menjadi portabel?", "id": "Migrasi ke sistem standalone sepenuhnya." },
  { "en": "Apa beda Raspberry Pi dan Arduino?", "id": "Raspberry Pi adalah komputer single-board." },
  { "en": "Kenapa tidak pakai Arduino?", "id": "Tidak cukup kuat untuk pemrosesan citra." },
  { "en": "Apa itu I2C pada LCD?", "id": "Protokol komunikasi hemat pin." },
  { "en": "Berapa pin yang dibutuhkan I2C?", "id": "Membutuhkan total empat pin saja." },
  { "en": "Apa itu 'overhead' data?", "id": "Data tambahan selain data inti." },
  { "en": "Kenapa MQTT memiliki 'overhead' rendah?", "id": "Protokol dirancang sangat efisien." },
  { "en": "Apa contoh objek referensi?", "id": "Sebuah koin atau stiker khusus." },
  { "en": "Bagaimana objek referensi membantu?", "id": "Mengkalibrasi rasio piksel ke ukuran." },
  { "en": "Apakah akurasi akan 100% dengan regresi?", "id": "Tidak, tapi akan lebih presisi." },
  { "en": "Apa itu 'heuristik'?", "id": "Pendekatan praktis, tidak dijamin optimal." },
  { "en": "Apakah klasifikasi ukuran Anda heuristik?", "id": "Ya, berdasarkan aturan luas piksel." },
  { "en": "Bagaimana sistem Anda dibandingkan penelitian Surya (2024)?", "id": "Surya (2024) menghitung jeruk dipohon." },
  { "en": "Bagaimana penelitian Ferdi (2025) relevan?", "id": "Optimasi model klasifikasi tingkat kematangan." },
  { "en": "Apa peran laptop AMD Athlon?", "id": "Sebagai node pemroses pusat sistem." },
  { "en": "Apakah jenis laptop mempengaruhi performa?", "id": "Ya, terutama pada kecepatan inferensi." },
  { "en": "Bagaimana jika laptop lebih lambat?", "id": "Latensi end-to-end akan meningkat." },
  { "en": "Apakah program di laptop kompleks?", "id": "Berpusat pada satu skrip Python." },
  { "en": "Apakah ada GUI di laptop?", "id": "Ya, jendela OpenCV menampilkan deteksi." },
  { "en": "Apa yang ditampilkan di jendela laptop?", "id": "Video dengan bounding box deteksi." },
  { "en": "Apakah angka di 'bounding box' itu?", "id": "Tingkat keyakinan (confidence score) deteksi." },
  { "en": "Apa arti confidence score 0.73?", "id": "Keyakinan 73% objek terdeteksi benar." },
  { "en": "Bagaimana Anda menangani deteksi ganda?", "id": "Sistem menghitung total semua deteksi." },
  { "en": "Bagaimana Anda membedakan dua apel?", "id": "Sistem mendeteksi sebagai dua objek." },
  { "en": "Apakah bentuk buah mempengaruhi luas piksel?", "id": "Ya, orientasi buah dapat mempengaruhinya." },
  { "en": "Apakah sistem bisa menghitung jumlah buah?", "id": "Ya, berdasarkan jumlah deteksi valid." },
  { "en": "Apa kesimpulan paling penting?", "id": "Sistem terdistribusi ini efektif, andal." },
  { "en": "Apa saran paling penting?", "id": "Implementasi normalisasi skala jarak." },
  { "en": "Apakah Anda menguji di kondisi gelap?", "id": "Dokumen tidak merinci pengujian gelap." },
  { "en": "Bagaimana performa di cahaya redup?", "id": "Kemungkinan akurasi akan menurun." },
  { "en": "Apa itu 'robustness' sistem?", "id": "Ketahanan sistem terhadap berbagai kondisi." },
  { "en": "Apakah sistem Anda sudah 'robust'?", "id": "Belum, sensitif terhadap perubahan jarak." },
  { "en": "Bagaimana sistem direset antar transaksi?", "id": "Dokumen tidak membahas reset transaksi." },
  { "en": "Bagaimana harga buah di-update?", "id": "Logika didefinisikan dalam kamus DATA_BUAH." },
  { "en": "Apakah harga bisa diubah pengguna?", "id": "Dokumen tidak menyebutkan antarmuka pengguna." },
  { "en": "Kenapa pakai Raspberry Pi OS Lite?", "id": "Dokumen tidak menjelaskan alasan spesifik." },
  { "en": "Apa keuntungan versi OS Lite?", "id": "Umumnya lebih ringan tanpa desktop." },
  { "en": "Bagaimana jika hasil teks panjang?", "id": "Kemungkinan terpotong di LCD 20x4." },
  { "en": "Apa pengaruh resolusi kamera?", "id": "Resolusi mempengaruhi detail deteksi objek." },
  { "en": "Bagaimana jika kamera resolusi rendah?", "id": "Akurasi deteksi ukuran bisa menurun." },
  { "en": "Bagaimana jika kamera resolusi tinggi?", "id": "Membutuhkan lebih banyak daya komputasi." },
  { "en": "Apakah ada pertimbangan keamanan jaringan?", "id": "Dokumen tidak membahas aspek keamanan." },
  { "en": "Bagaimana jika MQTT broker gagal?", "id": "Sistem tidak akan bisa menampilkan hasil." },
  { "en": "Bagaimana jika buah terdeteksi tapi tiada di kamus?", "id": "Sistem tidak bisa menghitung harganya." },
  { "en": "Kenapa streaming video pakai Flask/picamera?", "id": "Sebagai contoh web server sederhana." },
  { "en": "Alternatif selain Flask untuk streaming?", "id": "Protokol lain seperti RTSP (tidak disebut)." },
  { "en": "Bagaimana skalabilitas kamus DATA_BUAH?", "id": "Kurang skalabel untuk banyak buah." },
  { "en": "Apa trade-off latensi dan akurasi?", "id": "Model lebih cepat seringkali kurang akurat." },
  { "en": "Bisakah latensi dikurangi lebih lanjut?", "id": "Mungkin dengan mengorbankan tingkat akurasi." },
  { "en": "Apakah ini contoh 'edge computing'?", "id": "Ya, pemrosesan awal di perangkat 'edge'." },
  { "en": "Apa beda 'edge' dan 'cloud'?", "id": "Edge lokal, cloud terpusat jauh." },
  { "en": "Bagaimana fault tolerance sistem ini?", "id": "Rendah, sangat bergantung pada semua komponen." },
  { "en": "Apa itu 'dependency' sistem ini?", "id": "Ketergantungan pada laptop dan jaringan Wi-Fi." },
  { "en": "Bagaimana performa jika banyak buah?", "id": "Latensi pemrosesan laptop bisa meningkat." },
  { "en": "Siapa target pengguna sistem ini?", "id": "Ritel, pertanian, atau industri makanan." },
  { "en": "Apakah prototipe ini siap produksi?", "id": "Tidak, masih perlu pengembangan lanjut." },
  { "en": "Apa langkah menuju produk jadi?", "id": "Desain PCB kustom dan enklosur." },
  { "en": "Apa itu 'inferensi' dalam AI?", "id": "Proses menjalankan model untuk prediksi." },
  { "en": "Di mana proses inferensi terjadi?", "id": "Terjadi pada node pemroses (laptop)." },
  { "en": "Kenapa inferensi tidak di Raspberry Pi?", "id": "Keterbatasan sumber daya komputasi Pi." },
  { "en": "Bagaimana jika Wi-Fi terputus-sambung?", "id": "Akan mengganggu streaming dan pengiriman hasil." },
  { "en": "Bisakah sistem berjalan via kabel LAN?", "id": "Secara teori bisa, dokumen tidak sebut." },
  { "en": "Apa itu 'klasifikasi' dalam AI?", "id": "Mengelompokkan data ke dalam kategori." },
  { "en": "Di mana proses klasifikasi terjadi?", "id": "Di laptop, setelah deteksi objek." },
  { "en": "Dasar klasifikasi ukuran Anda apa?", "id": "Ambang batas luas area piksel." },
  { "en": "Apakah model Anda bisa membedakan kultivar?", "id": "Tidak, hanya jenis buah umum." },
  { "en": "Bagaimana jika dua jenis apel berbeda?", "id": "Kemungkinan terdeteksi sebagai 'apple' saja." },
  { "en": "Apakah warna buah jadi fitur utama?", "id": "Model belajar dari bentuk, warna, tekstur." },
  { "en": "Bagaimana jika ada apel hijau?", "id": "Tergantung apakah ada di dataset." },
  { "en": "Apa itu 'dataset augmentation'?", "id": "Memperbanyak data latih secara artifisial." },
  { "en": "Apakah Anda menggunakan 'augmentation'?", "id": "Dokumen tidak merinci hal tersebut." },
  { "en": "Manfaat 'augmentation' untuk apa?", "id": "Meningkatkan ketahanan (robustness) model." },
  { "en": "Bagaimana Anda memverifikasi output LCD?", "id": "Pengujian fungsionalitas output pada LCD." },
  { "en": "Apa itu 'deployment' model?", "id": "Integrasi model ke dalam sistem." },
  { "en": "Di mana model Anda di-deploy?", "id": "Diintegrasikan ke dalam skrip Python laptop." },
  { "en": "Bagaimana Anda mengukur efisiensi alur kerja?", "id": "Dengan mengintegrasikan beberapa langkah kerja." },
  { "en": "Parameter perbandingan dengan metode manual?", "id": "Kecepatan, akurasi, konsistensi, dan efisiensi." },
  { "en": "Apakah ada bias dalam dataset?", "id": "Mungkin jika data tidak cukup beragam." },
  { "en": "Bagaimana cara mengurangi bias?", "id": "Menggunakan dataset yang lebih beragam." },
  { "en": "Apa itu 'overfitting' pada model?", "id": "Model terlalu bagus di data latih." },
  { "en": "Apa itu 'underfitting' pada model?", "id": "Model tidak belajar dengan baik." },
  { "en": "Bagaimana cara menghindari 'overfitting'?", "id": "Menggunakan dataset validasi dan augmentasi." },
  { "en": "Apakah biaya sistem ini mahal?", "id": "Menggunakan perangkat keras yang umum." },
  { "en": "Komponen termahal mungkin apa?", "id": "Laptop sebagai node pemroses." },
  { "en": "Bagaimana sistem menangani buah lonjong?", "id": "Bounding box akan menyesuaikan bentuknya." },
  { "en": "Apakah orientasi buah mempengaruhi estimasi?", "id": "Ya, bisa mengubah luas piksel." },
  { "en": "Bagaimana cara mengatasi masalah orientasi?", "id": "Melatih dengan berbagai orientasi buah." },
  { "en": "Apakah ada kalibrasi kamera?", "id": "Dokumen tidak menyebutkan proses kalibrasi." },
  { "en": "Apa itu kalibrasi kamera?", "id": "Memperbaiki distorsi lensa dan perspektif." },
  { "en": "Apakah sistem bisa untuk objek non-buah?", "id": "Bisa, jika dilatih ulang." },
  { "en": "Berapa lama waktu pelatihan model?", "id": "Dokumen tidak menyebutkan durasi pelatihan." },
  { "en": "Perangkat keras apa untuk pelatihan?", "id": "Biasanya GPU untuk mempercepat proses." },
  { "en": "Apakah Anda melatih model sendiri?", "id": "Dokumen tidak merinci, menggunakan YOLOv8n." },
  { "en": "Apa itu model 'pre-trained'?", "id": "Model yang sudah dilatih sebelumnya." },
  { "en": "Apakah Anda menggunakan model 'pre-trained'?", "id": "Sangat mungkin, ini praktek umum." },
  { "en": "Apa keuntungan model 'pre-trained'?", "id": "Menghemat waktu dan sumber daya." },
  { "en": "Apakah sistem bisa mendeteksi tangan manusia?", "id": "Tergantung dataset, mungkin bisa mengganggu." },
  { "en": "Bagaimana mengatasi gangguan latar belakang?", "id": "Dengan dataset yang latar belakangnya beragam." },
  { "en": "Apakah 'bounding box' bisa tumpang tindih?", "id": "Ya, jika objek berdekatan." },
  { "en": "Apa itu Non-Maximum Suppression (NMS)?", "id": "Menghilangkan bounding box yang tumpang tindih." },
  { "en": "Apakah YOLO menggunakan NMS?", "id": "Ya, itu bagian standar algoritma." },
  { "en": "Bagaimana Anda memilih CONF_THRESHOLD?", "id": "Nilai ambang untuk memfilter deteksi." },
  { "en": "Apa yang terjadi jika threshold rendah?", "id": "Banyak deteksi salah (false positives)." },
  { "en": "Apa yang terjadi jika threshold tinggi?", "id": "Banyak objek terlewat (false negatives)." },
  { "en": "Bagaimana Anda memvalidasi estimasi berat?", "id": "Dengan membandingkan klasifikasi ukuran." },
  { "en": "Apakah Anda menimbang buah secara manual?", "id": "Tidak, hanya validasi klasifikasi ukuran." },
  { "en": "Bagaimana cara kerja model regresi?", "id": "Memetakan input ke output kontinu." },
  { "en": "Contoh input-output model regresi?", "id": "Input luas piksel, output berat." },
  { "en": "Apakah sistem ini inovatif?", "id": "Inovatif dalam arsitektur terdistribusi-nya." },
  { "en": "Bagaimana Anda menguji latensi end-to-end?", "id": "Mengukur total waktu tunda sistem." },
  { "en": "Apakah pengujian dilakukan berulang kali?", "id": "Pengujian dilakukan untuk mendapatkan rata-rata." },
  { "en": "Apa itu 'standar deviasi' latensi?", "id": "Menunjukkan variasi dari waktu latensi." },
  { "en": "Apakah Anda menghitung standar deviasi?", "id": "Dokumen tidak menyajikan data tersebut." },
  { "en": "Mengapa konsistensi 100% penting?", "id": "Menghilangkan human error dan subjektivitas." },
  { "en": "Apakah sistem ini sepenuhnya otomatis?", "id": "Ya, dari identifikasi hingga kalkulasi." },
  { "en": "Bagaimana peran manusia dalam sistem?", "id": "Menempatkan buah dan memulai sistem." },
  { "en": "Apakah sistem bisa berjalan 24/7?", "id": "Secara teknis bisa, perlu stabilitas." },
  { "en": "Apa tantangan operasional jangka panjang?", "id": "Perawatan perangkat keras dan pembaruan perangkat lunak." },
  { "en": "Bagaimana jika model menjadi usang?", "id": "Perlu dilatih ulang dengan data baru." },
  { "en": "Seberapa sering model perlu dilatih ulang?", "id": "Tergantung perubahan jenis buah/lingkungan." },
  { "en": "Apa yang Anda pelajari dari proyek ini?", "id": "Penerapan AI pada perangkat keras terbatas." },
  { "en": "Apa bagian tersulit dari proyek?", "id": "Dokumen tidak merinci kesulitan personal." },
  { "en": "Bagaimana Anda memecahkan masalah saat pengembangan?", "id": "Dokumen tidak merinci proses debugging." },
  { "en": "Apakah kode Anda tersedia open-source?", "id": "Dokumen tidak menyebutkan ketersediaan kode." },
  { "en": "Bagaimana penelitian ini bisa dikomersialkan?", "id": "Perlu pengembangan menjadi produk jadi." },
  { "en": "Apa langkah pertama untuk komersialisasi?", "id": "Membuat prototipe yang lebih robust." },
  { "en": "Berapa estimasi biaya prototipe ini?", "id": "Relatif rendah, menggunakan komponen umum." },
  { "en": "Apakah sistem bisa salah mengidentifikasi buah?", "id": "Ya, akurasi deteksi tidak 100%." },
  { "en": "Apa dampak kesalahan identifikasi?", "id": "Perhitungan harga dan berat salah." },
  { "en": "Apa judul Bab I?", "id": "Pendahuluan, berisi latar belakang masalah." },
  { "en": "Apa judul Bab II?", "id": "Tinjauan Pustaka, berisi teori pendukung." },
  { "en": "Apa judul Bab III?", "id": "Perancangan Alat dan Sistem." },
  { "en": "Apa judul Bab IV?", "id": "Implementasi, Pengujian, dan Pembahasan." },
  { "en": "Apa judul Bab V?", "id": "Kesimpulan dan Saran untuk pengembangan." },
  { "en": "Bagaimana sistematika penulisan membantu pembaca?", "id": "Memberi informasi rinci tiap bab." },
  { "en": "Apa itu studi literatur?", "id": "Penelitian yang sudah dilakukan sebelumnya." },
  { "en": "Kenapa studi literatur itu penting?", "id": "Menunjukkan posisi penelitian Anda saat ini." },
  { "en": "Apa perbedaan utama sistem Anda?", "id": "Fokus pada arsitektur terdistribusi." },
  { "en": "Bagaimana jika buah bergerak saat difoto?", "id": "Bisa menyebabkan 'motion blur', akurasi turun." },
  { "en": "Apakah sistem menangani 'motion blur'?", "id": "Dokumen tidak menyebutkan penanganan ini." },
  { "en": "Bagaimana jika ada bayangan kuat?", "id": "Dapat mengganggu deteksi dan fitur." },
  { "en": "Bagaimana sistem mengatasi bayangan?", "id": "Dataset mencakup berbagai kondisi pencahayaan." },
  { "en": "Bagaimana jika buah terpotong sebagian?", "id": "Deteksi mungkin gagal atau tidak akurat." },
  { "en": "Apakah sistem mendeteksi buah terpotong?", "id": "Dokumen tidak membahas skenario ini." },
  { "en": "Mengapa menggunakan kabel jumper 30cm?", "id": "Dokumen tidak merinci alasan panjangnya." },
  { "en": "Apa fungsi 'tahapan penelitian'?", "id": "Menjelaskan alur pengerjaan secara garis besar." },
  { "en": "Fitur apa saja pada alat?", "id": "Pengumpulan data, preprocessing, pelatihan, implementasi." },
  { "en": "Bagaimana Anda memastikan model akurat?", "id": "Evaluasi dan optimasi dengan data nyata." },
  { "en": "Apa itu 'Akurasi' dalam Bab III?", "id": "Penjelasan faktor yang mempengaruhi akurasi." },
  { "en": "Faktor utama yang mempengaruhi akurasi?", "id": "Kualitas data, preprocessing, dan pelatihan." },
  { "en": "Apa itu 'Bangun Model'?", "id": "Penentuan komponen berdasarkan fitur alat." },
  { "en": "Bagaimana arsitektur menjawab rumusan masalah 1?", "id": "Dengan merancang sistem deteksi fungsional." },
  { "en": "Bagaimana estimasi berat menjawab rumusan masalah 2?", "id": "Ekstraksi fitur luas area piksel." },
  { "en": "Bagaimana analisis kinerja menjawab rumusan masalah 3?", "id": "Membandingkan sistem dengan metode manual." },
  { "en": "Bagaimana Anda mencapai Tujuan Penelitian 1?", "id": "Membangun sistem identifikasi buah otomatis." },
  { "en": "Bagaimana Anda mencapai Tujuan Penelitian 2?", "id": "Implementasi metode ekstraksi fitur visual." },
  { "en": "Bagaimana Anda mencapai Tujuan Penelitian 3?", "id": "Analisis komparatif kinerja sistem otomatis." },
  { "en": "Apa dampak ekonomi dari sistem ini?", "id": "Meningkatkan efisiensi proses bisnis ritel." },
  { "en": "Apa dampak sosial dari sistem ini?", "id": "Potensi pergeseran pekerjaan manual." },
  { "en": "Bagaimana sistem ini diintegrasikan ke kasir?", "id": "Dapat dihubungkan ke sistem Point-of-Sale." },
  { "en": "Apakah sistem bisa mencetak struk?", "id": "Tidak, hanya menampilkan pada layar LCD." },
  { "en": "Bagaimana cara menambahkan buah baru?", "id": "Melatih ulang model dengan data baru." },
  { "en": "Apakah proses tambah buah itu mudah?", "id": "Membutuhkan proses pengumpulan data, pelabelan." },
  { "en": "Bagaimana jika LCD gagal berfungsi?", "id": "Hasil tidak bisa ditampilkan ke pengguna." },
  { "en": "Apakah sistem tetap bekerja tanpa LCD?", "id": "Ya, proses di laptop tetap berjalan." },
  { "en": "Kenapa pakai LCD daripada monitor HDMI?", "id": "Lebih ringkas dan hemat daya." },
  { "en": "Protokol apa selain MQTT untuk data?", "id": "Bisa pakai HTTP POST (tidak disebut)." },
  { "en": "Kenapa MQTT lebih baik dari HTTP?", "id": "Lebih ringan dan dirancang untuk IoT." },
  { "en": "Apa itu 'QoS' dalam MQTT?", "id": "Quality of Service, jaminan pengiriman pesan." },
  { "en": "Apakah Anda mengatur 'QoS'?", "id": "Dokumen tidak merinci konfigurasi MQTT." },
  { "en": "Bagaimana jika latar belakang ramai?", "id": "Dapat menurunkan akurasi deteksi model." },
  { "en": "Saran untuk latar belakang ramai?", "id": "Gunakan latar belakang yang terkontrol/kontras." },
  { "en": "Apakah bentuk 'bounding box' selalu persegi?", "id": "Ya, bentuknya selalu persegi panjang." },
  { "en": "Bagaimana dengan buah yang sangat bulat?", "id": "Bounding box akan tetap persegi." },
  { "en": "Apa itu 'image segmentation'?", "id": "Membuat outline presisi, bukan kotak." },
  { "en": "Apakah 'segmentation' lebih baik?", "id": "Lebih akurat untuk luas, komputasi berat." },
  { "en": "Bisakah YOLO melakukan 'segmentation'?", "id": "Ya, ada varian YOLO untuk segmentasi." },
  { "en": "Kenapa Anda tidak pakai 'segmentation'?", "id": "Deteksi kotak sudah cukup untuk tujuan." },
  { "en": "Apa perbedaan utama YOLOv5 dan YOLOv8?", "id": "YOLOv8 lebih baru, arsitektur berbeda." },
  { "en": "Kenapa pakai YOLOv8n bukan YOLOv8s/m/l/x?", "id": "Varian 'n' (nano) paling ringan." },
  { "en": "Apa trade-off pakai YOLOv8n?", "id": "Kecepatan tinggi, akurasi sedikit lebih rendah." },
  { "en": "Apakah trade-off ini cocok?", "id": "Ya, untuk komputasi di laptop umum." },
  { "en": "Bagaimana Anda memvalidasi data 'ground truth'?", "id": "Dokumen tidak merinci proses validasi." },
  { "en": "Apakah ada potensi kesalahan label?", "id": "Ya, kesalahan manusia saat pelabelan." },
  { "en": "Bagaimana kesalahan label mempengaruhi model?", "id": "Model akan belajar informasi yang salah." },
  { "en": "Berapa 'epoch' pelatihan model Anda?", "id": "Dokumen tidak menyebutkan hyperparameter pelatihan." },
  { "en": "Apa itu 'epoch' dalam pelatihan?", "id": "Satu siklus penuh melewati dataset." },
  { "en": "Apa itu 'learning rate'?", "id": "Parameter yang mengontrol seberapa cepat model belajar." },
  { "en": "Bagaimana Anda memilih 'learning rate'?", "id": "Dokumen tidak merinci optimasi hyperparameter." },
  { "en": "Apa peran optimasi pada penelitian ini?", "id": "Meningkatkan kinerja sistem secara keseluruhan." },
  { "en": "Skenario pengujian Anda ada berapa?", "id": "Ada empat skenario pengujian utama." },
  { "en": "Sebutkan satu skenario pengujian.", "id": "Pengujian Akurasi Deteksi dan Klasifikasi." },
  { "en": "Sebutkan skenario pengujian yang lain.", "id": "Pengujian Fungsionalitas Komunikasi dan Latensi." },
  { "en": "Apakah pengujian dilakukan pada satu buah?", "id": "Gambar pengujian menunjukkan beberapa buah." },
  { "en": "Bagaimana sistem Anda menangani kegagalan daya?", "id": "Tidak ada backup daya, sistem mati." },
  { "en": "Saran untuk kegagalan daya?", "id": "Bisa menggunakan baterai atau UPS." },
  { "en": "Berapa konsumsi daya sistem ini?", "id": "Dokumen tidak merinci konsumsi daya." },
  { "en": "Apakah sistem menghasilkan panas berlebih?", "id": "Komputasi berat di laptop bisa panas." },
  { "en": "Apakah perlu sistem pendingin tambahan?", "id": "Umumnya laptop sudah memiliki pendingin." },
  { "en": "Bagaimana cuaca/suhu mempengaruhi alat?", "id": "Suhu ekstrim dapat mempengaruhi elektronik." },
  { "en": "Apakah alat ini tahan air?", "id": "Tidak, prototipe tidak dirancang tahan air." },
  { "en": "Bagaimana jika buah basah/berembun?", "id": "Pantulan cahaya bisa mempengaruhi deteksi." },
  { "en": "Apakah ada proses 'cleaning' data?", "id": "Preprocessing citra bisa dianggap 'cleaning'." },
  { "en": "Bagaimana Anda memastikan etika penelitian?", "id": "Dokumen tidak membahas pertimbangan etika." },
  { "en": "Apakah Anda menggunakan data pribadi?", "id": "Tidak, hanya menggunakan gambar buah." },
  { "en": "Bagaimana Anda mengelola versi kode?", "id": "Dokumen tidak membahas manajemen versi." },
  { "en": "Apakah ada dokumentasi kode?", "id": "Dokumen tidak menyebutkan dokumentasi kode." },
  { "en": "Bagaimana jika ada bug di program?", "id": "Perlu proses 'debugging' untuk memperbaikinya." },
  { "en": "Apa tantangan terbesar dalam implementasi?", "id": "Integrasi dua entitas (Pi, Laptop)." },
  { "en": "Apa yang paling membanggakan dari proyek ini?", "id": "Berhasil membangun sistem terdistribusi fungsional." },
  { "en": "Apakah Anda akan melanjutkan proyek ini?", "id": "Bab V memberikan saran pengembangan lanjut." },
  { "en": "Bagaimana proyek ini relevan dengan Teknik Elektro?", "id": "Integrasi sistem elektronik dan perangkat lunak." },
  { "en": "Hubungan proyek dengan 'computer vision'?", "id": "Ini adalah aplikasi computer vision." },
  { "en": "Hubungan proyek dengan 'machine learning'?", "id": "YOLO adalah algoritma machine learning." },
  { "en": "Hubungan proyek dengan 'IoT'?", "id": "Contoh aplikasi 'Internet of Things'." },
  { "en": "Kenapa judulnya 'Penerapan Algoritma YOLO'?", "id": "Karena fokusnya pada implementasi YOLO." },
  { "en": "Kenapa tidak 'Penciptaan Algoritma Baru'?", "id": "Karena menggunakan algoritma yang sudah ada." },
  { "en": "Apa batasan utama metode luas piksel?", "id": "Tidak bisa membedakan densitas buah." },
  { "en": "Bagaimana jika ada buah ringan tapi besar?", "id": "Estimasi beratnya kemungkinan akan salah." },
  { "en": "Bagaimana model regresi mengatasi ini?", "id": "Model regresi juga masih terbatas." },
  { "en": "Solusi terbaik untuk berat akurat?", "id": "Integrasi dengan timbangan digital (sensor fusi)." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari beberapa sensor." },
  { "en": "Apakah Anda mempertimbangkan 'sensor fusion'?", "id": "Tidak disebutkan dalam dokumen ini." },
  { "en": "Apa saran terakhir Anda untuk pembaca?", "id": "Saran ada di Bab 5.2." },
  { "en": "Apa itu 'bounding box'?", "id": "Kotak pembatas hasil deteksi objek." },
  { "en": "Bagaimana sistem menentukan ukuran 'bounding box'?", "id": "Algoritma YOLO yang menentukannya otomatis." },
  { "en": "Apakah akurasi estimasi berat seragam?", "id": "Tidak, ada kesalahan di batas kategori." },
  { "en": "Apa itu 'arsitektur client-server'?", "id": "Model komputasi dengan penyedia/pengguna layanan." },
  { "en": "Siapa 'client' dalam sistem Anda?", "id": "Raspberry Pi yang meminta pemrosesan." },
  { "en": "Siapa 'server' dalam sistem Anda?", "id": "Laptop yang menyediakan layanan inferensi." },
  { "en": "Apakah penelitian ini menciptakan algoritma?", "id": "Tidak, ini penerapan algoritma YOLO." },
  { "en": "Kenapa hanya menguji beberapa buah?", "id": "Sesuai batasan masalah penelitian." },
  { "en": "Apakah sistem bisa belajar sendiri?", "id": "Tidak, perlu dilatih ulang manual." },
  { "en": "Bagaimana cara kerja 'deep learning'?", "id": "Belajar dari data lewat jaringan syaraf." },
  { "en": "Apakah YOLO termasuk 'deep learning'?", "id": "Ya, menggunakan 'deep neural networks'." },
  { "en": "Apa perbedaan dengan 'machine learning' tradisional?", "id": "Deep learning otomatis ekstraksi fitur." },
  { "en": "Bagaimana Anda memastikan validitas hasil?", "id": "Melalui protokol pengujian yang terstruktur." },
  { "en": "Apa itu 'protokol pengujian'?", "id": "Prosedur terstruktur untuk mengevaluasi sistem." },
  { "en": "Apakah Anda menguji ketahanan fisik?", "id": "Tidak, pengujian fokus pada fungsionalitas." },
  { "en": "Bagaimana jika buah warnanya tidak umum?", "id": "Gambar apel biru berhasil dideteksi." },
  { "en": "Apa arti deteksi apel biru?", "id": "Model fokus pada bentuk, bukan warna." },
  { "en": "Bagaimana sistem menghitung total harga?", "id": "Menjumlahkan harga setiap buah terdeteksi." },
  { "en": "Apa itu 'node' dalam sistem?", "id": "Entitas komputasi (Pi atau Laptop)." },
  { "en": "Berapa banyak 'node' di sistem?", "id": "Dua, yaitu node sensor dan pemroses." },
  { "en": "Bagaimana 'node' saling menemukan?", "id": "Melalui alamat IP di jaringan lokal." },
  { "en": "Apa itu 'streaming'?", "id": "Mengirim data media secara terus-menerus." },
  { "en": "Kenapa video perlu di-'stream'?", "id": "Untuk analisis 'real-time' di laptop." },
  { "en": "Bagaimana jika ada dua kamera?", "id": "Arsitektur saat ini untuk satu kamera." },
  { "en": "Apakah sistem bisa dikembangkan multi-kamera?", "id": "Bisa, tapi memerlukan modifikasi signifikan." },
  { "en": "Apa itu 'JSON'?", "id": "Format pertukaran data yang ringan." },
  { "en": "Kenapa pakai JSON bukan XML?", "id": "JSON lebih ringan dan ringkas." },
  { "en": "Apa itu 'parsing' JSON?", "id": "Membaca dan menginterpretasi data JSON." },
  { "en": "Siapa yang melakukan 'parsing' JSON?", "id": "Skrip di Raspberry Pi (subscriber)." },
  { "en": "Apa itu 'antarmuka CSI'?", "id": "Camera Serial Interface untuk kamera Pi." },
  { "en": "Apa keuntungan antarmuka CSI?", "id": "Bandwidth tinggi, latensi rendah." },
  { "en": "Bagaimana jika pakai kamera USB?", "id": "Bisa, tapi performa mungkin berbeda." },
  { "en": "Apa itu 'GPIO'?", "id": "General Purpose Input/Output pin." },
  { "en": "Komponen apa yang pakai GPIO?", "id": "Layar LCD I2C 20x4." },
  { "en": "Apa itu 'overhead' protokol?", "id": "Data tambahan yang dibutuhkan protokol." },
  { "en": "Apakah HTTP punya 'overhead' besar?", "id": "Relatif lebih besar dari MQTT." },
  { "en": "Bagaimana jika buah sangat kecil?", "id": "Mungkin sulit terdeteksi oleh model." },
  { "en": "Apa batasan ukuran objek deteksi?", "id": "Tergantung resolusi dan kualitas model." },
  { "en": "Penelitian Huda (2025) pakai metode apa?", "id": "CRISP-DM (Cross-Industry Standard Process)." },
  { "en": "Apakah Anda mengikuti metodologi CRISP-DM?", "id": "Dokumen tidak menyebutkan metodologi ini." },
  { "en": "Abdulloh (2025) pakai platform apa?", "id": "Menggunakan Roboflow untuk pelabelan data." },
  { "en": "Apa keuntungan pakai Roboflow?", "id": "Memudahkan proses pelabelan dan augmentasi." },
  { "en": "Apa itu 'akurasi' vs 'presisi'?", "id": "Akurasi keseluruhan, presisi deteksi positif." },
  { "en": "Metrik evaluasi apa yang Anda gunakan?", "id": "Akurasi deteksi dan klasifikasi ukuran." },
  { "en": "Apakah Anda mengukur 'recall'?", "id": "Tidak disebutkan dalam tabel hasil." },
  { "en": "Apa itu 'recall'?", "id": "Kemampuan model menemukan semua objek relevan." },
  { "en": "Bagaimana jika 'recall' rendah?", "id": "Banyak buah yang tidak terdeteksi." },
  { "en": "Bagaimana 'robustness' berhubungan dengan generalisasi?", "id": "Model robust dapat generalisasi baik." },
  { "en": "Apa tujuan utama dari 'casing'?", "id": "Melindungi komponen elektronik dari luar." },
  { "en": "Bahan 'casing' yang Anda pakai?", "id": "Terbuat dari akrilik dan bracket." },
  { "en": "Kenapa perlu meningkatkan antarmuka pengguna?", "id": "Untuk pengalaman pengguna yang lebih baik." },
  { "en": "Apa kekurangan antarmuka LCD 20x4?", "id": "Terbatas, hanya bisa menampilkan teks." },
  { "en": "Bagaimana layar sentuh memperbaiki ini?", "id": "Memungkinkan antarmuka grafis dan interaktif." },
  { "en": "Apakah sistem bisa terhubung ke database?", "id": "Bisa, sebagai pengembangan di masa depan." },
  { "en": "Apa manfaat terhubung ke database?", "id": "Pencatatan data penjualan dan inventaris." },
  { "en": "Bagaimana Anda memastikan repetisi pengujian?", "id": "Dengan menggunakan protokol pengujian terstruktur." },
  { "en": "Apakah hasil bisa direproduksi?", "id": "Seharusnya bisa dengan perangkat/kondisi sama." },
  { "en": "Apa itu 'analisis sintetik'?", "id": "Interpretasi mendalam dari seluruh hasil." },
  { "en": "Di mana analisis sintetik dilakukan?", "id": "Pada bagian Pembahasan (Bab 4.3)." },
  { "en": "Bagaimana pembahasan menjawab rumusan masalah?", "id": "Setiap sub-bab mengupas satu rumusan." },
  { "en": "Apa itu 'kompromi yang disengaja'?", "id": "Pilihan desain sadar dengan kelemahan." },
  { "en": "Apa kompromi utama dalam sistem?", "id": "Metode estimasi berat yang sederhana." },
  { "en": "Bagaimana jika tidak ada kompromi?", "id": "Sistem menjadi lebih kompleks/mahal." },
  { "en": "Apa itu 'alur kerja holistik'?", "id": "Melihat keseluruhan proses sebagai satu." },
  { "en": "Bagaimana sistem memperbaiki alur kerja?", "id": "Mengotomatiskan dan mengintegrasikan banyak langkah." },
  { "en": "Ketergantungan sistem ini pada apa?", "id": "Jarak kamera, laptop, dan jaringan." },
  { "en": "Bagaimana cara kerja normalisasi skala?", "id": "Mengkalibrasi piksel berdasarkan objek referensi." },
  { "en": "Apakah objek referensi harus selalu ada?", "id": "Ya, harus ada di bidang pandang." },
  { "en": "Apa itu 'estimasi kontinu'?", "id": "Hasil berupa angka, bukan kategori." },
  { "en": "Kenapa estimasi kontinu lebih baik?", "id": "Lebih presisi daripada tiga kategori." },
  { "en": "Apakah PCB membuat alat lebih kecil?", "id": "Ya, umumnya lebih ringkas/rapi." },
  { "en": "Apakah PCB mengurangi kemungkinan error?", "id": "Ya, koneksi lebih stabil daripada breadboard." },
  { "en": "Bagaimana Anda menyimpulkan hasil penelitian?", "id": "Sistem berhasil dibangun dan lebih unggul." },
  { "en": "Apa inti dari saran Anda?", "id": "Mengatasi keterbatasan utama yang ada." },
  { "en": "Apakah saran Anda realistis?", "id": "Ya, berdasarkan teknologi yang ada." },
  { "en": "Siapa saja penulis di daftar pustaka?", "id": "Peneliti sebelumnya yang karyanya relevan." },
  { "en": "Kenapa daftar pustaka penting?", "id": "Menunjukkan dasar ilmiah dan menghindari plagiarisme." },
  { "en": "Format sitasi apa yang digunakan?", "id": "Dokumen tidak mengikuti format standar." },
  { "en": "Apakah ada sumber dari jurnal internasional?", "id": "Ya, seperti Journal of Electronic Imaging." },
  { "en": "Apakah ada sumber dari konferensi?", "id": "Ya, dari German Research Center." },
  { "en": "Apa peran Raspberry Pi Foundation?", "id": "Menyediakan komputer single-board berbiaya rendah." },
  { "en": "Apakah Raspberry Pi open-source?", "id": "Perangkat lunaknya sebagian besar open-source." },
  { "en": "Apakah sistem Anda bisa untuk industri?", "id": "Perlu pengembangan lanjut untuk keandalan." },
  { "en": "Bagaimana skalabilitas sistem untuk industri?", "id": "Arsitektur saat ini kurang skalabel." },
  { "en": "Bagaimana cara membuat sistem skalabel?", "id": "Menggunakan arsitektur microservices atau cloud." },
  { "en": "Apakah Anda mempertimbangkan biaya operasional?", "id": "Dokumen tidak membahas biaya operasional." },
  { "en": "Biaya operasionalnya meliputi apa saja?", "id": "Listrik, perawatan, dan pembaruan perangkat lunak." },
  { "en": "Bagaimana jika model salah klasifikasi?", "id": "Harga dan berat yang ditampilkan salah." },
  { "en": "Apakah ada mekanisme verifikasi manual?", "id": "Tidak ada, sistem berjalan otomatis." },
  { "en": "Bagaimana jika pengguna tidak setuju hasil?", "id": "Tidak ada mekanisme untuk intervensi." },
  { "en": "Saran untuk verifikasi hasil?", "id": "Tampilkan gambar dengan bounding box." },
  { "en": "Apakah gambar di laporan hasil nyata?", "id": "Ya, dari pengujian alat langsung." },
  { "en": "Berapa harga dan berat salak?", "id": "Harga 12.000, berat 83.48 gram." },
  { "en": "Berapa harga dan berat dua apel?", "id": "Harga 18.000, berat 250 gram." },
  { "en": "Apakah angka ini konsisten?", "id": "Sistem memberikan konsistensi 100%." },
  { "en": "Apa kata kunci dari penelitian ini?", "id": "YOLO, Raspberry Pi, Deteksi Buah." },
  { "en": "Apa kontribusi orisinal penelitian Anda?", "id": "Penerapan arsitektur terdistribusi untuk AIoT." },
  { "en": "Kenapa metodologi Anda bisa dipercaya?", "id": "Menggunakan protokol pengujian yang terstruktur." },
  { "en": "Bagaimana Bab II mendukung perancangan Anda?", "id": "Menjadi landasan teori pemilihan komponen." },
  { "en": "Hubungkan batasan masalah dengan kesimpulan.", "id": "Kesimpulan menjawab tujuan dalam batasan." },
  { "en": "Kenapa memilih arsitektur terdistribusi?", "id": "Mengatasi keterbatasan komputasi Raspberry Pi." },
  { "en": "Alternatif arsitektur selain yang dipilih?", "id": "Sistem standalone dengan SBC lebih kuat." },
  { "en": "Apa justifikasi tidak memakai timbangan?", "id": "Fokus pada estimasi berbasis computer vision." },
  { "en": "Bagaimana Anda memitigasi risiko utama?", "id": "Dengan melakukan pengujian pada jarak tetap." },
  { "en": "Risiko utama proyek Anda apa?", "id": "Estimasi berat tidak akurat karena jarak." },
  { "en": "Bagaimana teori dari Pagnutti (2017) diterapkan?", "id": "Penggunaan modul kamera Raspberry Pi." },
  { "en": "Apakah sistem Anda memenuhi semua tujuan?", "id": "Ya, semua tujuan penelitian tercapai." },
  { "en": "Jika ada satu hal untuk diubah?", "id": "Dokumen tidak menyebutkan refleksi personal." },
  { "en": "Apa definisi 'sukses' untuk proyek ini?", "id": "Sistem fungsional yang lebih unggul." },
  { "en": "Bagaimana Anda memvalidasi kamus DATA_BUAH?", "id": "Dokumen tidak merinci validasi kamus." },
  { "en": "Bagaimana jika data di kamus salah?", "id": "Hasil perhitungan harga menjadi salah." },
  { "en": "Kenapa tidak pakai database untuk harga?", "id": "Kamus Python lebih sederhana untuk prototipe." },
  { "en": "Seberapa 'robust' metode estimasi Anda?", "id": "Tidak 'robust' terhadap perubahan jarak." },
  { "en": "Kenapa menerima keterbatasan 'robustness' ini?", "id": "Kompromi untuk kesederhanaan implementasi." },
  { "en": "Apa perbedaan 'efektif' dan 'efisien'?", "id": "Efektif mencapai tujuan, efisien hemat sumberdaya." },
  { "en": "Apakah sistem Anda efektif dan efisien?", "id": "Ya, efektif dan sangat efisien." },
  { "en": "Bagaimana Anda menjamin objektivitas perbandingan?", "id": "Dengan parameter terukur (kecepatan, konsistensi)." },
  { "en": "Kritik paling mendasar untuk sistem Anda?", "id": "Ketergantungan pada jarak kamera tetap." },
  { "en": "Bagaimana Anda menjawab kritik tersebut?", "id": "Dengan memberikan saran normalisasi skala jarak." },
  { "en": "Apa implikasi dari latensi <1 detik?", "id": "Cukup responsif untuk aplikasi target (kasir)." },
  { "en": "Bagaimana jika targetnya lini produksi cepat?", "id": "Latensi saat ini mungkin terlalu tinggi." },
  { "en": "Bagaimana relevansi paper H. Tavakol (2021)?", "id": "Paper tersebut juga menggunakan deteksi objek." },
  { "en": "Apa perbedaan dengan deteksi objek kecil?", "id": "Buah relatif objek yang lebih besar." },
  { "en": "Bagaimana Anda memastikan tidak ada plagiarisme?", "id": "Dengan menyertakan studi literatur relevan." },
  { "en": "Kenapa struktur Bab IV seperti itu?", "id": "Menjelaskan implementasi, pengujian, lalu pembahasan." },
  { "en": "Kenapa pembahasan dipisah dari hasil?", "id": "Untuk analisis dan interpretasi data mendalam." },
  { "en": "Bagaimana Anda menghubungkan setiap hasil pengujian?", "id": "Dalam pembahasan untuk evaluasi holistik." },
  { "en": "Apa arti 'holistik' dalam konteks ini?", "id": "Mengevaluasi sistem sebagai satu kesatuan." },
  { "en": "Apa asumsi terbesar yang Anda buat?", "id": "Asumsi bahwa jarak kamera selalu tetap." },
  { "en": "Bagaimana jika asumsi itu salah?", "id": "Kinerja klasifikasi ukuran akan menurun." },
  { "en": "Apa arti 'heuristik' dalam klasifikasi Anda?", "id": "Menggunakan aturan praktis, bukan matematis." },
  { "en": "Apa kelemahan pendekatan heuristik?", "id": "Kurang presisi dan kurang fleksibel." },
  { "en": "Bagaimana Anda memilih hyperparameter YOLOv8n?", "id": "Dokumen tidak merinci proses pemilihan." },
  { "en": "Apa dampak dari hyperparameter yang buruk?", "id": "Akurasi dan kecepatan model menurun." },
  { "en": "Bagaimana Anda memastikan dataset tidak bias?", "id": "Mengumpulkan data beragam kondisi pencahayaan." },
  { "en": "Apa itu 'bias' dalam konteks AI?", "id": "Model cenderung favoritisme pada data tertentu." },
  { "en": "Visi 5 tahun untuk teknologi ini?", "id": "Sistem standalone cerdas di setiap toko." },
  { "en": "Bagaimana Anda mengukur 'kemudahan penggunaan'?", "id": "Aspek ini tidak diukur secara kuantitatif." },
  { "en": "Apa metrik untuk 'konsistensi'?", "id": "100% hasil sama untuk input sama." },
  { "en": "Bagaimana Anda mengukur 'kecepatan' manual?", "id": "Estimasi waktu proses manual (5-10 detik)." },
  { "en": "Siapa yang melakukan proses manual ini?", "id": "Manusia (kasir atau pekerja)." },
  { "en": "Apa itu 'human error' dalam konteks ini?", "id": "Kesalahan estimasi atau input data manual." },
  { "en": "Bagaimana sistem Anda meminimalkan 'human error'?", "id": "Dengan mengotomatiskan seluruh proses pengecekan." },
  { "en": "Apa yang terjadi jika buah bergulir?", "id": "Dapat menyebabkan deteksi tidak stabil." },
  { "en": "Bagaimana Anda menangani 'backpressure' data?", "id": "Dokumen tidak membahas skenario ini." },
  { "en": "Apa itu 'backpressure'?", "id": "Produser mengirim data lebih cepat." },
  { "en": "Pelajaran terpenting dari kegagalan/error?", "id": "Dokumen tidak merinci proses pembelajaran." },
  { "en": "Bagaimana Anda memverifikasi 'ground truth'?", "id": "Membandingkan output dengan observasi manual." },
  { "en": "Apakah 'ground truth' Anda bisa salah?", "id": "Bisa, jika ada kesalahan observasi." },
  { "en": "Bagaimana penelitian ini berkontribusi ke ilmu pengetahuan?", "id": "Demonstrasi arsitektur AIoT yang efektif." },
  { "en": "Bagaimana Anda mengelola cakupan proyek?", "id": "Melalui batasan masalah yang jelas." },
  { "en": "Kenapa tidak semua saran diimplementasikan?", "id": "Keterbatasan waktu dan sumber daya penelitian." },
  { "en": "Saran mana yang prioritas tertinggi?", "id": "Implementasi normalisasi skala jarak." },
  { "en": "Kenapa itu prioritas tertinggi?", "id": "Mengatasi kelemahan paling fundamental sistem." },
  { "en": "Bagaimana Anda memilih judul penelitian?", "id": "Mencerminkan metode, objek, dan tujuan." },
  { "en": "Apa kata kunci yang hilang dari judul?", "id": "Mungkin 'Terdistribusi' atau 'Raspberry Pi'." },
  { "en": "Bagaimana Anda mendefinisikan 'sistem modern'?", "id": "Menggunakan teknologi AI dan IoT." },
  { "en": "Apa yang membuat sistem ini 'lengkap'?", "id": "Mencakup identifikasi, estimasi berat, harga." },
  { "en": "Apakah ada standar industri untuk ini?", "id": "Dokumen tidak menyebutkan standar industri." },
  { "en": "Bagaimana sistem Anda dibandingkan standar itu?", "id": "Tidak bisa dibandingkan tanpa standar." },
  { "en": "Apa yang akan Anda sampaikan di awal presentasi?", "id": "Masalah utama dan solusi yang diusulkan." },
  { "en": "Apa yang akan Anda sampaikan di akhir?", "id": "Kesimpulan, kontribusi, dan saran pengembangan." },
  { "en": "Bagaimana Anda menunjukkan demo alat?", "id": "Menunjukkan alur kerja dari input-output." },
  { "en": "Apa yang terjadi jika demo gagal?", "id": "Harus bisa menjelaskan penyebabnya." },
  { "en": "Bagaimana skalabilitas dari sisi perangkat lunak?", "id": "Skrip Python tunggal, kurang skalabel." },
  { "en": "Bagaimana cara membuatnya lebih skalabel?", "id": "Menggunakan arsitektur modular atau microservices." },
  { "en": "Apa itu 'dependency hell'?", "id": "Masalah konflik antar versi library." },
  { "en": "Bagaimana Anda mengelola dependensi Python?", "id": "Dokumen tidak menyebutkan manajemen dependensi." },
  { "en": "Apakah penelitian ini bisa dipatenkan?", "id": "Penerapan metode umum, sulit dipatenkan." },
  { "en": "Apa yang bisa dipatenkan dari sini?", "id": "Mungkin desain sistem spesifiknya." },
  { "en": "Bagaimana Anda menguji selama pandemi/keterbatasan?", "id": "Dokumen tidak menyebutkan tantangan eksternal." },
  { "en": "Apa perbedaan 'prototipe' dan 'produk'?", "id": "Prototipe untuk bukti konsep, produk untuk pasar." },
  { "en": "Sistem Anda di level mana?", "id": "Level prototipe fungsional (bukti konsep)." },
  { "en": "Apa metrik kesuksesan untuk produk?", "id": "Keandalan, biaya, kemudahan penggunaan, ROI." },
  { "en": "Bagaimana Anda mengukur keandalan (reliability)?", "id": "Waktu rata-rata antar kegagalan (MTBF)." },
  { "en": "Apakah Anda mengukur keandalan sistem?", "id": "Tidak, hanya stabilitas komunikasi singkat." },
  { "en": "Apa itu 'Return on Investment' (ROI)?", "id": "Ukuran profitabilitas dari sebuah investasi." },
  { "en": "Bagaimana menghitung ROI untuk sistem ini?", "id": "Membandingkan biaya dengan peningkatan efisiensi." },
  { "en": "Apakah model bisa membedakan buah matang?", "id": "Tidak, tapi disarankan untuk pengembangan." },
  { "en": "Bagaimana penelitian lain deteksi kematangan?", "id": "Menggunakan ciri warna dan tekstur." },
  { "en": "Kenapa Anda tidak mengimplementasikannya?", "id": "Diluar batasan masalah penelitian ini." },
  { "en": "Bagaimana Anda memastikan etika dalam AI?", "id": "Memastikan data tidak bias, transparan." },
  { "en": "Apakah sistem Anda transparan?", "id": "Cukup transparan, logika aturan jelas." },
  { "en": "Apa itu 'black box' dalam AI?", "id": "Model yang logikanya sulit dijelaskan." },
  { "en": "Apakah YOLO sebuah 'black box'?", "id": "Sebagian besar dianggap 'black box'." },
  { "en": "Bagaimana Anda mempercayai 'black box'?", "id": "Melalui pengujian dan evaluasi ekstensif." },
  { "en": "Terakhir, kenapa penelitian ini penting?", "id": "Menunjukkan solusi AI praktis dan terjangkau." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
