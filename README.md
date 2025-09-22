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


    { "en": "Mengapa Resistor Terbakar?", "id": "Arus Yang Mengalir Terlalu Besar." },
    { "en": "Solusi Resistor Terbakar?", "id": "Ganti Dengan Resistor Daya Lebih Tinggi." },
    { "en": "Apa Ciri Kapasitor Elektrolit Rusak?", "id": "Bagian Atasnya Menggembung Atau Bocor." },
    { "en": "Solusi Kapasitor Menggembung?", "id": "Segera Ganti Dengan Yang Baru." },
    { "en": "Mengapa LED Tidak Menyala?", "id": "Polaritas Terbalik Atau Kurang Tegangan." },
    { "en": "Solusi LED Tidak Menyala?", "id": "Periksa Polaritas Dan Resistor Pembatas." },
    { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Jalur Arus Berhambatan Sangat Rendah." },
    { "en": "Solusi Untuk Hubung Singkat?", "id": "Cari Dan Hilangkan Sambungan Yang Salah." },
    { "en": "Mengapa Sekring Putus?", "id": "Melindungi Rangkaian Dari Arus Berlebih." },
    { "en": "Solusi Sekring Putus?", "id": "Ganti Sekring Setelah Memperbaiki Masalah." },
    { "en": "Apa Itu Rangkaian Terbuka (Open Circuit)?", "id": "Jalur Arus Terputus Di Suatu Titik." },
    { "en": "Solusi Rangkaian Terbuka?", "id": "Cari Kabel Putus Atau Solderan Retak." },
    { "en": "Mengapa Solderan Bisa Retak?", "id": "Karena Getaran Atau Perubahan Suhu." },
    { "en": "Solusi Solderan Retak?", "id": "Lakukan Penyolderan Ulang Pada Titik Tersebut." },
    { "en": "Apa Penyebab Suara 'Hum' Pada Audio?", "id": "Masalah Grounding Atau Interferensi Listrik." },
    { "en": "Solusi Suara 'Hum'?", "id": "Perbaiki Ground Loop, Gunakan Kabel Terlindung." },
    { "en": "Mengapa Perangkat Elektronik Mati Total?", "id": "Masalah Pada Catu Daya Atau Kabel Power." },
    { "en": "Solusi Perangkat Mati Total?", "id": "Periksa Sekring, Kabel, Dan Saklar Daya." },
    { "en": "Apa Penyebab IC Menjadi Sangat Panas?", "id": "Tegangan Berlebih, Beban Berlebih, Osilasi." },
    { "en": "Solusi IC Terlalu Panas?", "id": "Periksa Tegangan, Tambahkan Pendingin (Heatsink)." },
    { "en": "Mengapa Potensiometer Menimbulkan Suara Kresek?", "id": "Jalur Karbon Kotor Atau Aus." },
    { "en": "Solusi Potensiometer Kresek?", "id": "Bersihkan Dengan Cairan Kontak Atau Ganti." },
    { "en": "Apa Itu 'Cold Solder Joint'?", "id": "Solderan Yang Tidak Menempel Sempurna." },
    { "en": "Bagaimana Ciri 'Cold Solder Joint'?", "id": "Terlihat Kusam Dan Tidak Mengkilap." },
    { "en": "Solusi 'Cold Solder Joint'?", "id": "Panaskan Ulang Dengan Solder Dan Timah." },
    { "en": "Mengapa Baterai Cepat Habis?", "id": "Baterai Tua, Hubung Singkat, Atau Arus Bocor." },
    { "en": "Solusi Baterai Cepat Habis?", "id": "Ganti Baterai, Periksa Rangkaian Untuk Kebocoran." },
    { "en": "Apa Penyebab Tampilan LCD Kosong?", "id": "Koneksi Buruk Atau Kontras Tidak Tepat." },
    { "en": "Solusi Tampilan LCD Kosong?", "id": "Periksa Kabel Fleksibel, Atur Ulang Kontras." },
    { "en": "Mengapa Saklar Tidak Berfungsi?", "id": "Kontak Internal Kotor Atau Rusak." },
    { "en": "Solusi Saklar Tidak Berfungsi?", "id": "Bersihkan Atau Ganti Saklar Tersebut." },
    { "en": "Apa Itu Kerusakan Akibat Listrik Statis (ESD)?", "id": "Kerusakan Komponen Sensitif Oleh Muatan Statis." },
    { "en": "Solusi Mencegah ESD?", "id": "Gunakan Gelang Anti-Statis Dan Alas Kerja." },
    { "en": "Mengapa Trafo Berdengung Keras?", "id": "Laminasi Inti Besi Longgar Atau Beban Berlebih." },
    { "en": "Solusi Trafo Berdengung?", "id": "Kencangkan Baut Inti, Kurangi Beban." },
    { "en": "Apa Penyebab Output Audio Terdistorsi?", "id": "Sinyal Input Terlalu Kuat (Clipping)." },
    { "en": "Solusi Audio Terdistorsi?", "id": "Turunkan Level Sinyal Input Atau Penguatan." },
    { "en": "Mengapa Relay Tidak Mau Bekerja?", "id": "Tegangan Koil Kurang Atau Kontak Rusak." },
    { "en": "Solusi Relay Tidak Bekerja?", "id": "Periksa Tegangan Penggerak, Ganti Relay." },
    { "en": "Apa Itu 'Ripple' Pada Catu Daya?", "id": "Sisa Tegangan AC Pada Output DC." },
    { "en": "Penyebab 'Ripple' Berlebihan?", "id": "Kapasitor Filter Kering Atau Nilainya Terlalu Kecil." },
    { "en": "Solusi 'Ripple' Berlebihan?", "id": "Ganti Kapasitor Filter Dengan Yang Baru." },
    { "en": "Mengapa Jalur PCB Terkelupas?", "id": "Penyolderan Terlalu Panas Atau Terlalu Lama." },
    { "en": "Solusi Jalur PCB Terkelupas?", "id": "Gunakan Kawat Jumper Untuk Sambungan." },
    { "en": "Apa Penyebab Korosi Pada Papan Sirkuit?", "id": "Kelembaban Atau Tumpahan Cairan." },
    { "en": "Solusi Korosi Pada PCB?", "id": "Bersihkan Dengan Alkohol Isopropil Dan Sikat." },
    { "en": "Mengapa Nilai Resistor Berubah?", "id": "Panas Berlebih Atau Usia Komponen." },
    { "en": "Solusi Resistor Berubah Nilai?", "id": "Ganti Dengan Resistor Baru Yang Sesuai." },
    { "en": "Apa Itu 'Noise' Dalam Elektronika?", "id": "Sinyal Listrik Acak Yang Tidak Diinginkan." },
    { "en": "Solusi Mengurangi 'Noise'?", "id": "Gunakan Kapasitor Bypass Dan Grounding Yang Baik." },
    { "en": "Mengapa Motor DC Tidak Berputar?", "id": "Tidak Ada Daya Atau Sikat Aus." },
    { "en": "Solusi Motor DC Tidak Berputar?", "id": "Periksa Koneksi Daya, Ganti Sikat Motor." },
    { "en": "Apa Penyebab Interferensi Frekuensi Radio (RFI)?", "id": "Peralatan Elektronik Lain Yang Memancarkan Gangguan." },
    { "en": "Solusi Mengatasi RFI?", "id": "Gunakan Perisai (Shielding) Dan Filter Ferit." },
    { "en": "Mengapa Transistor Menjadi Panas Saat Bekerja?", "id": "Disipasi Daya Selama Operasi Normal." },
    { "en": "Kapan Panas Transistor Menjadi Masalah?", "id": "Jika Terlalu Panas Untuk Disentuh." },
    { "en": "Solusi Transistor Terlalu Panas?", "id": "Pastikan Pemasangan Heatsink Sudah Benar." },
    { "en": "Apa Itu 'Thermal Runaway'?", "id": "Kondisi Panas Menyebabkan Arus Naik Terus." },
    { "en": "Solusi Mencegah 'Thermal Runaway'?", "id": "Gunakan Rangkaian Kompensasi Suhu." },
    { "en": "Mengapa Tampilan Tujuh Segmen Menampilkan Angka Aneh?", "id": "Input BCD Salah Atau IC Driver Rusak." },
    { "en": "Solusi Tampilan Tujuh Segmen Aneh?", "id": "Periksa Sinyal Input Logika, Ganti IC Driver." },
    { "en": "Apa Itu 'Floating Input' Pada Gerbang CMOS?", "id": "Input Yang Tidak Terhubung Ke Mana Pun." },
    { "en": "Masalah 'Floating Input'?", "id": "Menyebabkan Perilaku Tidak Terduga Dan Konsumsi Arus." },
    { "en": "Solusi 'Floating Input'?", "id": "Hubungkan Ke Vcc Atau Ground (Pull-up/Pull-down)." },
    { "en": "Mengapa Rangkaian Digital Gagal Bekerja?", "id": "Masalah Sinyal Clock Atau Tegangan Catu." },
    { "en": "Solusi Rangkaian Digital Gagal?", "id": "Periksa Osilator Clock Dan Tegangan Catu." },
    { "en": "Apa Itu 'Ground Loop'?", "id": "Ketika Ada Lebih Dari Satu Jalur Ground." },
    { "en": "Masalah Akibat 'Ground Loop'?", "id": "Menimbulkan Derau 'Hum' Pada Frekuensi Rendah." },
    { "en": "Solusi 'Ground Loop'?", "id": "Gunakan Titik Ground Tunggal (Star Ground)." },
    { "en": "Mengapa Kabel Terbakar?", "id": "Ukuran Kabel Terlalu Kecil Untuk Arus." },
    { "en": "Solusi Kabel Terbakar?", "id": "Ganti Dengan Kabel Berdiameter Lebih Besar." },
    { "en": "Apa Penyebab Tegangan Output Regulator Turun?", "id": "Beban Terlalu Berat Atau Regulator Rusak." },
    { "en": "Solusi Tegangan Regulator Turun?", "id": "Kurangi Beban Atau Ganti Regulator." },
    { "en": "Mengapa Baut Konektor Menjadi Longgar?", "id": "Getaran Mekanis Seiring Waktu." },
    { "en": "Solusi Baut Konektor Longgar?", "id": "Kencangkan Secara Berkala Sesuai Jadwal." },
    { "en": "Apa Itu Dioda Zener?", "id": "Dioda Untuk Menstabilkan Tegangan Referensi." },
    { "en": "Masalah Umum Dioda Zener?", "id": "Rusak Terbuka Atau Terhubung Singkat." },
    { "en": "Solusi Dioda Zener Rusak?", "id": "Ganti Dengan Yang Baru Sesuai Tegangan." },
    { "en": "Mengapa Speaker Suaranya Pecah?", "id": "Daya Amplifier Terlalu Besar Atau Konus Sobek." },
    { "en": "Solusi Speaker Pecah?", "id": "Kurangi Volume Atau Ganti Unit Speaker." },
    { "en": "Apa Penyebab Osilator Gagal Berosilasi?", "id": "Penguatan Loop Kurang Atau Komponen Rusak." },
    { "en": "Solusi Osilator Gagal?", "id": "Periksa Transistor/Op-Amp Dan Komponen Umpan Balik." },
    { "en": "Mengapa Multimeter Menunjukkan Angka Tidak Stabil?", "id": "Koneksi Probe Buruk Atau Baterai Lemah." },
    { "en": "Solusi Multimeter Tidak Stabil?", "id": "Pastikan Probe Terpasang Baik, Ganti Baterai." },
    { "en": "Apa Itu 'Crosstalk' Pada PCB?", "id": "Interferensi Antara Jalur Sinyal Berdekatan." },
    { "en": "Solusi Mengurangi 'Crosstalk'?", "id": "Jaga Jarak Jalur, Gunakan 'Ground Plane'." },
    { "en": "Mengapa Konektor USB Tidak Terdeteksi?", "id": "Kontak Kotor Atau Sambungan Solderan Rusak." },
    { "en": "Solusi Konektor USB Tidak Terdeteksi?", "id": "Bersihkan Kontak, Periksa Solderan Di Papan." },
    { "en": "Apa Penyebab Bau Plastik Terbakar?", "id": "Komponen Terlalu Panas Atau Hubung Singkat." },
    { "en": "Solusi Bau Terbakar?", "id": "Segera Matikan Daya Dan Periksa Komponen." },
    { "en": "Mengapa Mikrofon Menimbulkan 'Feedback'?", "id": "Suara Dari Speaker Masuk Kembali Ke Mikrofon." },
    { "en": "Solusi 'Feedback' Mikrofon?", "id": "Jauhkan Mikrofon Dari Speaker, Turunkan Penguatan." },
    { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage)?", "id": "Tegangan Maksimum Yang Bisa Ditahan Komponen." },
    { "en": "Masalah Jika Tegangan Tembus Terlampaui?", "id": "Komponen Akan Rusak Secara Permanen." },
    { "en": "Solusi Mencegah Tembus?", "id": "Pilih Komponen Dengan Rating Tegangan Cukup." },
    { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Yang Tidak Diinginkan Dalam Rangkaian." },
    { "en": "Penyebab Osilasi Parasitik?", "id": "Layout Buruk Atau Umpan Balik Liar." },
    { "en": "Solusi Osilasi Parasitik?", "id": "Tambahkan Kapasitor Bypass, Perbaiki Layout." },
    { "en": "Mengapa Kaki Komponen Berkarat?", "id": "Terkena Kelembaban Atau Oksidasi Udara." },
    { "en": "Solusi Kaki Komponen Berkarat?", "id": "Kerik Dengan Hati-hati Sebelum Menyolder." },
    { "en": "Apa Itu 'Dry Joint'?", "id": "Istilah Lain Untuk 'Cold Solder Joint'." },
    { "en": "Solusi 'Dry Joint'?", "id": "Solder Ulang Titik Sambungan Tersebut." },
    { "en": "Mengapa Kabel Fleksibel Mudah Rusak?", "id": "Sering Ditekuk Atau Terlipat." },
    { "en": "Solusi Kabel Fleksibel Rusak?", "id": "Ganti Dengan Yang Baru, Perkuat Bagian Tekukan." },
    { "en": "Apa Penyebab Remote TV Tidak Bekerja?", "id": "Baterai Habis Atau LED Inframerah Rusak." },
    { "en": "Solusi Remote TV Rusak?", "id": "Ganti Baterai, Periksa LED Dengan Kamera." },
    { "en": "Bagaimana Memeriksa LED Inframerah?", "id": "Arahkan Ke Kamera Ponsel, Akan Terlihat Menyala." },
    { "en": "Mengapa Ada Percikan Api Saat Mencolokkan Alat?", "id": "Arus Awal Yang Besar (Inrush Current)." },
    { "en": "Solusi Mengurangi Percikan Api?", "id": "Gunakan Rangkaian 'Soft Start'." },
    { "en": "Apa Itu Polaritas Terbalik?", "id": "Menghubungkan Positif Ke Negatif Dan Sebaliknya." },
    { "en": "Risiko Polaritas Terbalik?", "id": "Dapat Merusak Komponen Sensitif Secara Instan." },
    { "en": "Solusi Mencegah Polaritas Terbalik?", "id": "Gunakan Dioda Proteksi Atau Konektor Khusus." },
    { "en": "Mengapa Sensor Suhu Tidak Akurat?", "id": "Kalibrasi Buruk Atau Penempatan Salah." },
    { "en": "Solusi Sensor Suhu Tidak Akurat?", "id": "Kalibrasi Ulang, Jauhkan Dari Sumber Panas." },
    { "en": "Apa Itu 'Latch-up' Pada CMOS?", "id": "Kondisi Hubung Singkat Internal Yang Merusak." },
    { "en": "Solusi Mencegah 'Latch-up'?", "id": "Desain PCB Yang Baik, Hindari Tegangan Berlebih." },
    { "en": "Mengapa Transformator Menjadi Sangat Panas?", "id": "Beban Berlebih Atau Hubung Singkat Di Sekunder." },
    { "en": "Solusi Trafo Panas Berlebih?", "id": "Matikan, Kurangi Beban, Periksa Gulungan." },
    { "en": "Apa Ciri Gulungan Trafo Terbakar?", "id": "Bau Hangus Dan Warna Isolasi Menghitam." },
    { "en": "Solusi Gulungan Terbakar?", "id": "Ganti Transformator Atau Gulung Ulang." },
    { "en": "Mengapa Layar Sentuh Tidak Responsif?", "id": "Permukaan Kotor Atau Kalibrasi Bergeser." },
    { "en": "Solusi Layar Sentuh Tidak Responsif?", "id": "Bersihkan Layar, Lakukan Kalibrasi Ulang." },
    { "en": "Apa Penyebab Sinyal Wi-Fi Lemah?", "id": "Jarak Jauh, Halangan, Atau Interferensi." },
    { "en": "Solusi Sinyal Wi-Fi Lemah?", "id": "Pindahkan Router, Gunakan 'Repeater'." },
    { "en": "Mengapa Kipas Pendingin Berisik?", "id": "Bantalan (Bearing) Aus Atau Kotoran." },
    { "en": "Solusi Kipas Pendingin Berisik?", "id": "Bersihkan Dari Debu Atau Ganti Kipas." },
    { "en": "Apa Itu Tegangan Ofset Pada Op-Amp?", "id": "Tegangan Output Kecil Saat Input Nol." },
    { "en": "Solusi Mengatasi Tegangan Ofset?", "id": "Gunakan Op-Amp Presisi Atau Rangkaian Pembatalan." },
    { "en": "Mengapa Penguat Audio Menghasilkan Suara 'Pop'?", "id": "Transien DC Saat Dihidupkan Atau Dimatikan." },
    { "en": "Solusi Suara 'Pop'?", "id": "Gunakan Rangkaian 'Muting' Atau Penunda." },
    { "en": "Apa Itu 'Thermal Paste'?", "id": "Bahan Untuk Memperbaiki Transfer Panas." },
    { "en": "Di Mana 'Thermal Paste' Digunakan?", "id": "Antara IC Dan Heatsink." },
    { "en": "Masalah Jika 'Thermal Paste' Kering?", "id": "Transfer Panas Menjadi Tidak Efektif." },
    { "en": "Solusi 'Thermal Paste' Kering?", "id": "Ganti Dengan Yang Baru Secara Berkala." },
    { "en": "Mengapa Lampu Neon Berkedip Saat Dinyalakan?", "id": "Starter Atau Ballast Mengalami Masalah." },
    { "en": "Solusi Lampu Neon Berkedip?", "id": "Ganti Starter, Periksa Sambungan Ballast." },
    { "en": "Apa Penyebab Tombol Keyboard Macet?", "id": "Kotoran Atau Tumpahan Cairan Di Bawah Tombol." },
    { "en": "Solusi Tombol Keyboard Macet?", "id": "Bersihkan Dengan Udara Bertekanan Atau Bongkar." },
    { "en": "Mengapa Nilai Kapasitansi Berubah?", "id": "Karena Usia (Pengeringan Elektrolit)." },
    { "en": "Solusi Kapasitor Menua?", "id": "Ganti Kapasitor Setelah Beberapa Tahun Penggunaan." },
    { "en": "Apa Itu 'Solder Bridge'?", "id": "Timah Solder Yang Menghubungkan Dua Jalur." },
    { "en": "Bahaya 'Solder Bridge'?", "id": "Menyebabkan Hubung Singkat." },
    { "en": "Solusi 'Solder Bridge'?", "id": "Hilangkan Kelebihan Timah Dengan Solder Wick." },
    { "en": "Mengapa PCB Bengkok Atau Melengkung?", "id": "Panas Berlebih Selama Perakitan Atau Operasi." },
    { "en": "Solusi PCB Bengkok?", "id": "Gunakan Penyangga, Hindari Panas Berlebih." },
    { "en": "Apa Itu 'Whining Noise' Dari Catu Daya?", "id": "Getaran Frekuensi Tinggi Pada Komponen Magnetik." },
    { "en": "Solusi 'Whining Noise'?", "id": "Gunakan Lem Atau Pernis Pada Komponen." },
    { "en": "Mengapa Baterai Lithium Menggembung?", "id": "Pengisian Berlebih Atau Kerusakan Internal." },
    { "en": "Bahaya Baterai Menggembung?", "id": "Risiko Kebocoran Atau Kebakaran." },
    { "en": "Solusi Baterai Menggembung?", "id": "Berhenti Menggunakan Dan Buang Dengan Benar." },
    { "en": "Apa Penyebab Kesetrum Listrik Ringan?", "id": "Kebocoran Arus Atau Grounding Tidak Sempurna." },
    { "en": "Solusi Mencegah Kesetrum?", "id": "Pastikan Grounding Baik, Gunakan Alas Kaki Kering." },
    { "en": "Mengapa Frekuensi Kristal Osilator Bergeser?", "id": "Penuaan Kristal Atau Perubahan Suhu." },
    { "en": "Solusi Frekuensi Kristal Bergeser?", "id": "Gunakan Kristal Kompensasi Suhu (TCXO)." },
    { "en": "Apa Itu 'Memory Effect' Pada Baterai?", "id": "Kapasitas Tampak Menurun Jika Tidak Dikosongkan Penuh." },
    { "en": "Baterai Jenis Apa Yang Mengalaminya?", "id": "Baterai NiCd Dan NiMH Lama." },
    { "en": "Solusi 'Memory Effect'?", "id": "Lakukan Siklus Pengosongan Penuh Secara Berkala." },
    { "en": "Mengapa Layar CRT Bergetar?", "id": "Interferensi Medan Magnet Eksternal." },
    { "en": "Solusi Layar CRT Bergetar?", "id": "Jauhkan Dari Speaker Atau Trafo Besar." },
    { "en": "Apa Itu 'Screen Burn-in'?", "id": "Gambar Hantu Permanen Pada Layar." },
    { "en": "Penyebab 'Screen Burn-in'?", "id": "Menampilkan Gambar Statis Terlalu Lama." },
    { "en": "Solusi Mencegah 'Burn-in'?", "id": "Gunakan 'Screen Saver' Atau Ganti Gambar." },
    { "en": "Mengapa Mouse Optik Tidak Bergerak Lancar?", "id": "Permukaan Alas Tidak Cocok Atau Sensor Kotor." },
    { "en": "Solusi Mouse Optik Macet?", "id": "Gunakan Mousepad, Bersihkan Lensa Sensor." },
    { "en": "Apa Itu 'Stray Capacitance'?", "id": "Kapasitansi Yang Tidak Diinginkan Antar Konduktor." },
    { "en": "Masalah 'Stray Capacitance'?", "id": "Membatasi Kinerja Rangkaian Frekuensi Tinggi." },
    { "en": "Solusi 'Stray Capacitance'?", "id": "Layout PCB Yang Cermat, Gunakan 'Ground Plane'." },
    { "en": "Apa Itu 'Stray Inductance'?", "id": "Induktansi Yang Tidak Diinginkan Pada Kaki Komponen." },
    { "en": "Solusi 'Stray Inductance'?", "id": "Gunakan Kaki Komponen Sependek Mungkin." },
    { "en": "Mengapa Regulator Linear Sangat Panas?", "id": "Karena Membuang Kelebihan Tegangan Sebagai Panas." },
    { "en": "Solusi Efisiensi Regulator Linear?", "id": "Gunakan Regulator Switching." },
    { "en": "Apa Keuntungan Regulator Switching?", "id": "Efisiensi Jauh Lebih Tinggi." },
    { "en": "Apa Kerugian Regulator Switching?", "id": "Menghasilkan Lebih Banyak Derau Listrik." },
    { "en": "Mengapa Ada Busur Api Pada Saklar?", "id": "Karena Induktansi Rangkaian Saat Arus Diputus." },
    { "en": "Solusi Busur Api Saklar?", "id": "Gunakan Rangkaian Snubber (RC) Paralel Saklar." },
    { "en": "Mengapa Warna Pada Layar TV Pudar?", "id": "Penuaan Tabung Gambar Atau Lampu Latar." },
    { "en": "Solusi Warna TV Pudar?", "id": "Kalibrasi Ulang Warna, Ganti Lampu Latar." },
    { "en": "Apa Penyebab Kontak Relay Menempel?", "id": "Busur Api Saat Memutus Beban Induktif." },
    { "en": "Solusi Kontak Relay Menempel?", "id": "Gunakan Dioda Freewheeling Paralel Beban." },
    { "en": "Mengapa Ada Garis Horizontal Di Layar TV?", "id": "Masalah Pada Sirkuit Vertikal." },
    { "en": "Solusi Garis Horizontal?", "id": "Periksa Atau Ganti IC Vertikal." },
    { "en": "Mengapa Ada Garis Vertikal Di Layar TV?", "id": "Masalah Pada Sirkuit Horisontal." },
    { "en": "Solusi Garis Vertikal?", "id": "Periksa Solderan Pada Yoke Atau Trafo Horisontal." },
    { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Teknis Sebuah Komponen." },
    { "en": "Masalah Jika Tidak Membaca 'Datasheet'?", "id": "Bisa Salah Menggunakan Atau Merusak Komponen." },
    { "en": "Solusinya?", "id": "Selalu Baca 'Datasheet' Sebelum Menggunakan Komponen." },
    { "en": "Apa Itu 'Power Supply Rejection Ratio' (PSRR)?", "id": "Kemampuan Menolak Derau Dari Catu Daya." },
    { "en": "Masalah Jika PSRR Rendah?", "id": "Derau Catu Daya Masuk Ke Sinyal Output." },
    { "en": "Solusi Meningkatkan PSRR?", "id": "Gunakan Kapasitor Decoupling Yang Cukup." },
    { "en": "Mengapa Multimeter Menunjukkan 'OL'?", "id": "Artinya 'Overload' Atau 'Open Loop'." },
    { "en": "Solusi Saat Multimeter Menunjukkan 'OL'?", "id": "Pilih Rentang Pengukuran Yang Lebih Tinggi." },
    { "en": "Apa Itu 'Electromagnetic Interference' (EMI)?", "id": "Gangguan Akibat Radiasi Elektromagnetik." },
    { "en": "Solusi Mengurangi EMI?", "id": "Gunakan Casing Logam (Shielding)." },
    { "en": "Mengapa Solderan Sulit Menempel?", "id": "Ujung Solder Kotor Atau PCB Oksidasi." },
    { "en": "Solusi Solderan Sulit Menempel?", "id": "Bersihkan Ujung Solder Dan Permukaan PCB." },
    { "en": "Apa Itu 'Solder Wick' atau 'Desoldering Braid'?", "id": "Jalinan Tembaga Untuk Menghisap Timah." },
    { "en": "Apa Itu Pompa Sedot Timah?", "id": "Alat Vakum Untuk Menghilangkan Solderan." },
    { "en": "Mengapa Terjadi Kesalahan Paritas Memori?", "id": "Data Rusak Saat Ditulis Atau Dibaca." },
    { "en": "Solusi Kesalahan Paritas?", "id": "Ganti Modul Memori (RAM) Yang Rusak." },
    { "en": "Apa Penyebab 'Blue Screen of Death' (BSOD)?", "id": "Masalah Perangkat Keras Atau Driver." },
    { "en": "Solusi Awal Untuk BSOD?", "id": "Restart Komputer, Periksa Perangkat Keras Baru." },
    { "en": "Mengapa Hard Drive Mengeluarkan Bunyi Klik?", "id": "Kerusakan Mekanis Pada Head Pembaca." },
    { "en": "Solusi Hard Drive Berbunyi Klik?", "id": "Segera Cadangkan Data, Ganti Hard Drive." },
    { "en": "Apa Itu 'Bad Sector'?", "id": "Area Pada Hard Drive Yang Tidak Bisa Dibaca." },
    { "en": "Solusi Untuk 'Bad Sector'?", "id": "Jalankan Utilitas Pengecekan Disk." },
    { "en": "Mengapa Printer Tidak Mencetak?", "id": "Tinta Habis, Kertas Macet, Atau Masalah Driver." },
    { "en": "Solusi Awal Printer Macet?", "id": "Periksa Level Tinta Dan Laci Kertas." },
    { "en": "Apa Itu 'Ghosting' Pada Layar LCD?", "id": "Jejak Gambar Sebelumnya Tertinggal." },
    { "en": "Penyebab 'Ghosting'?", "id": "Waktu Respon Piksel Yang Lambat." },
    { "en": "Solusi 'Ghosting'?", "id": "Ganti Monitor Dengan Waktu Respon Cepat." },
    { "en": "Apa Itu Piksel Mati (Dead Pixel)?", "id": "Piksel Yang Selalu Mati (Hitam)." },
    { "en": "Apa Itu Piksel Terjebak (Stuck Pixel)?", "id": "Piksel Yang Selalu Menyala Satu Warna." },
    { "en": "Solusi Piksel Terjebak?", "id": "Gunakan Perangkat Lunak Atau Tekanan Lembut." },
    { "en": "Mengapa Lampu Latar Laptop Redup?", "id": "Inverter Rusak Atau Lampu CCFL Menua." },
    { "en": "Solusi Lampu Latar Redup?", "id": "Ganti Inverter Atau Unit Lampu Latar." },
    { "en": "Apa Itu 'Whine' Frekuensi Tinggi Dari Layar?", "id": "Getaran Komponen Di Sirkuit Inverter." },
    { "en": "Solusi 'Whine' Layar?", "id": "Biasanya Perlu Mengganti Papan Inverter." },
    { "en": "Mengapa Speaker Bluetooth Terputus-putus?", "id": "Jarak Terlalu Jauh Atau Ada Interferensi." },
    { "en": "Solusi Speaker Bluetooth Terputus?", "id": "Dekatkan Perangkat, Hindari Halangan." },
    { "en": "Apa Penyebab Korsleting Listrik?", "id": "Isolasi Kabel Rusak Bertemu Konduktor Lain." },
    { "en": "Solusi Mencegah Korsleting?", "id": "Gunakan Kabel Berkualitas Baik, Periksa Secara Berkala." },
    { "en": "Mengapa MCB (Miniature Circuit Breaker) Turun?", "id": "Beban Berlebih Atau Terjadi Hubung Singkat." },
    { "en": "Solusi MCB Turun?", "id": "Cabut Beberapa Alat, Naikkan Kembali MCB." },
    { "en": "Apa Perbedaan Sekring Dan MCB?", "id": "Sekring Sekali Pakai, MCB Bisa Direset." },
    { "en": "Mengapa Lampu Pijar Sering Putus?", "id": "Tegangan Listrik Tidak Stabil Atau Guncangan." },
    { "en": "Solusi Lampu Sering Putus?", "id": "Gunakan Stabilizer Tegangan, Pilih Lampu Berkualitas." },
    { "en": "Apa Itu 'Flickering' Pada Lampu LED?", "id": "Cahaya Berkedip Cepat Yang Mengganggu." },
    { "en": "Penyebab 'Flickering' LED?", "id": "Driver Tidak Cocok Atau Masalah Jaringan." },
    { "en": "Solusi 'Flickering' LED?", "id": "Ganti Driver Atau Gunakan Lampu LED Berkualitas." },
    { "en": "Mengapa Baterai Ponsel Panas Saat Diisi?", "id": "Proses Pengisian Normal Atau Pengisi Daya Buruk." },
    { "en": "Kapan Panas Baterai Menjadi Abnormal?", "id": "Jika Terlalu Panas Untuk Dipegang." },
    { "en": "Solusi Baterai Abnormal Panas?", "id": "Gunakan Pengisi Daya Asli, Hentikan Pengisian." },
    { "en": "Apa Penyebab Touchpad Laptop Tidak Bekerja?", "id": "Driver Salah Atau Touchpad Terkunci." },
    { "en": "Solusi Touchpad Tidak Bekerja?", "id": "Periksa Tombol Fungsi, Instal Ulang Driver." },
    { "en": "Mengapa Ada Bunyi Dengung Dari Stop Kontak?", "id": "Sambungan Kabel Longgar Di Dalamnya." },
    { "en": "Bahaya Stop Kontak Berdengung?", "id": "Risiko Panas Berlebih Dan Kebakaran." },
    { "en": "Solusi Stop Kontak Berdengung?", "id": "Matikan MCB, Kencangkan Sekrup Terminal." },
    { "en": "Apa Itu 'Arc Fault'?", "id": "Percikan Api Berbahaya Akibat Koneksi Buruk." },
    { "en": "Solusi Mencegah 'Arc Fault'?", "id": "Gunakan AFCI (Arc Fault Circuit Interrupter)." },
    { "en": "Apa Itu GFCI (Ground Fault Circuit Interrupter)?", "id": "Melindungi Dari Sengatan Listrik Akibat Kebocoran." },
    { "en": "Di Mana GFCI Wajib Dipasang?", "id": "Area Basah Seperti Kamar Mandi Dan Dapur." },
    { "en": "Mengapa Pengisi Daya Ponsel Berbunyi?", "id": "Getaran Komponen Internal Catu Daya Switching." },
    { "en": "Apakah Bunyi Itu Berbahaya?", "id": "Biasanya Tidak, Tapi Menandakan Komponen Murah." },
    { "en": "Mengapa Proyek Arduino Gagal Diunggah?", "id": "Port COM Salah, Driver Belum Terpasang." },
    { "en": "Solusi Gagal Unggah Arduino?", "id": "Pilih Port Yang Benar, Instal Driver CH340." },
    { "en": "Apa Itu 'Breadboard' Yang Buruk?", "id": "Kontak Internalnya Longgar Atau Oksidasi." },
    { "en": "Masalah 'Breadboard' Buruk?", "id": "Menyebabkan Koneksi Yang Tidak Andal." },
    { "en": "Solusi Masalah 'Breadboard'?", "id": "Ganti Dengan 'Breadboard' Berkualitas Baik." },
    { "en": "Mengapa Kabel Jumper Mudah Lepas?", "id": "Kualitas Ujung Pin Yang Buruk." },
    { "en": "Solusi Kabel Jumper Lepas?", "id": "Gunakan Kabel Jumper Dengan Pin Yang Kokoh." },
    { "en": "Apa Itu Resistansi Kontak?", "id": "Resistansi Kecil Pada Titik Sambungan." },
    { "en": "Masalah Resistansi Kontak Tinggi?", "id": "Menyebabkan Jatuh Tegangan Dan Pemanasan." },
    { "en": "Solusi Mengurangi Resistansi Kontak?", "id": "Gunakan Konektor Berlapis Emas, Jaga Kebersihan." },
    { "en": "Mengapa Osiloskop Menampilkan Derau?", "id": "Probe Ground Tidak Terhubung Dengan Baik." },
    { "en": "Apa Aturan Probe Ground Osiloskop?", "id": "Gunakan Kabel Ground Sependek Mungkin." },
    { "en": "Mengapa Alat Elektronik Bau Ozon?", "id": "Percikan Listrik Tegangan Tinggi Di Udara." },
    { "en": "Di Mana Bau Ozon Biasa Terjadi?", "id": "Dekat Motor Listrik Atau Trafo Flyback." },
    { "en": "Apa Itu 'Tin Whiskers'?", "id": "Filamen Timah Yang Tumbuh Dari Solderan." },
    { "en": "Masalah 'Tin Whiskers'?", "id": "Dapat Menyebabkan Hubung Singkat." },
    { "en": "Solusi Mencegah 'Tin Whiskers'?", "id": "Gunakan Solder Dengan Campuran Timbal." },
    { "en": "Mengapa Solder Bebas Timbal Lebih Rentan?", "id": "Karena Sifat Fisik Timah Murni." },
    { "en": "Apa Itu 'Electromigration'?", "id": "Pergerakan Ion Logam Akibat Aliran Elektron." },
    { "en": "Masalah 'Electromigration'?", "id": "Menipiskan Jalur Konduktor Dan Menyebabkan Kegagalan." },
    { "en": "Di Mana 'Electromigration' Menjadi Masalah?", "id": "Dalam Sirkuit Terpadu (IC)." },
    { "en": "Solusi Mengatasi 'Electromigration'?", "id": "Desain IC Dengan Jalur Yang Lebih Lebar." },
    { "en": "Mengapa Komponen SMD Sulit Disolder Manual?", "id": "Ukurannya Sangat Kecil." },
    { "en": "Solusi Menyolder SMD?", "id": "Gunakan Solder Uap Panas Dan Pinset." },
    { "en": "Apa Itu 'Reflow Soldering'?", "id": "Metode Penyolderan SMD Dengan Oven." },
    { "en": "Mengapa Pengukuran Resistansi Rendah Tidak Akurat?", "id": "Resistansi Kabel Probe Ikut Terukur." },
    { "en": "Solusi Pengukuran Resistansi Rendah?", "id": "Gunakan Metode Pengukuran Empat Kawat." },
    { "en": "Apa Itu Metode Empat Kawat?", "id": "Menggunakan Pasangan Kabel Terpisah Untuk Arus Dan Tegangan." },
    { "en": "Mengapa Terkadang Terjadi Guncangan Saat Menyentuh Casing Logam?", "id": "Casing Tidak Terhubung Ke Ground Dengan Baik." },
    { "en": "Solusi Casing Nyetrum?", "id": "Pastikan Kabel Ground Terpasang Dengan Benar." },
    { "en": "Apa Itu 'Antenna Effect' Pada PCB?", "id": "Jalur Panjang Bertindak Sebagai Antena." },
    { "en": "Masalah 'Antenna Effect'?", "id": "Menangkap Derau Atau Memancarkan EMI." },
    { "en": "Solusi 'Antenna Effect'?", "id": "Jaga Jalur Sinyal Sependek Mungkin." },
    { "en": "Mengapa Baterai Alkaline Bocor?", "id": "Reaksi Kimia Internal Seiring Waktu." },
    { "en": "Solusi Mencegah Kebocoran Baterai?", "id": "Keluarkan Baterai Jika Perangkat Tidak Digunakan." },
    { "en": "Cairan Bocoran Baterai Bersifat Apa?", "id": "Korosif, Harus Dibersihkan Dengan Hati-hati." },
    { "en": "Apa Itu 'Skin Effect'?", "id": "Kecenderungan Arus AC Mengalir Di Permukaan." },
    { "en": "Masalah 'Skin Effect'?", "id": "Meningkatkan Resistansi Efektif Pada Frekuensi Tinggi." },
    { "en": "Solusi 'Skin Effect'?", "id": "Gunakan Kawat Litz Atau Konduktor Berongga." },
    { "en": "Mengapa Ada Peringkat Tegangan Pada Kabel?", "id": "Menunjukkan Kemampuan Isolasi Maksimal." },
    { "en": "Masalah Jika Peringkat Tegangan Terlampaui?", "id": "Isolasi Bisa Tembus Dan Menyebabkan Korsleting." },
    { "en": "Solusi Memilih Kabel?", "id": "Pilih Sesuai Peringkat Tegangan Dan Arus." },
    { "en": "Apa Itu 'Inrush Current'?", "id": "Lonjakan Arus Sesaat Saat Dinyalakan." },
    { "en": "Penyebab 'Inrush Current'?", "id": "Pengisian Kapasitor Atau Pemanasan Filamen." },
    { "en": "Solusi 'Inrush Current'?", "id": "Gunakan Termistor NTC Atau Rangkaian Soft-Start." },
    { "en": "Mengapa Nilai Sekring Ditulis Dalam Ampere?", "id": "Karena Melindungi Berdasarkan Batas Arus." },
    { "en": "Apa Itu Sekring 'Slow-Blow'?", "id": "Sekring Yang Tahan Terhadap Lonjakan Arus Sesaat." },
    { "en": "Kapan Sekring 'Slow-Blow' Digunakan?", "id": "Pada Peralatan Dengan Motor Atau Trafo." },
    { "en": "Apa Itu Sekring 'Fast-Acting'?", "id": "Sekring Yang Putus Sangat Cepat." },
    { "en": "Kapan Sekring 'Fast-Acting' Digunakan?", "id": "Untuk Melindungi Rangkaian Elektronik Sensitif." },
    { "en": "Mengapa Mengganti Sekring Dengan Kawat Dilarang?", "id": "Karena Tidak Memberikan Perlindungan Yang Tepat." },
    { "en": "Apa Itu 'Derating' Komponen?", "id": "Menggunakan Komponen Di Bawah Peringkat Maksimalnya." },
    { "en": "Tujuan 'Derating'?", "id": "Meningkatkan Keandalan Dan Umur Komponen." },
    { "en": "Contoh 'Derating'?", "id": "Menggunakan Resistor 1 Watt Untuk Disipasi 0.5 Watt." },
    { "en": "Mengapa Ada Beberapa Lapisan Pada PCB?", "id": "Untuk Menampung Rangkaian Yang Lebih Kompleks." },
    { "en": "Lapisan Internal Biasanya Digunakan Untuk Apa?", "id": "Sebagai 'Power Plane' Dan 'Ground Plane'." },
    { "en": "Manfaat 'Power' Dan 'Ground Plane'?", "id": "Distribusi Daya Baik Dan Mengurangi Derau." },
    { "en": "Apa Itu 'Via' Pada PCB?", "id": "Lubang Konduktif Untuk Menghubungkan Antar Lapisan." },
    { "en": "Masalah Pada 'Via'?", "id": "Bisa Menambah Induktansi Parasitik." },
    { "en": "Apa Itu 'Jitter' Sinyal Clock?", "id": "Variasi Perioda Sinyal Clock Dari Waktu Ke Waktu." },
    { "en": "Masalah Akibat 'Jitter'?", "id": "Menyebabkan Kesalahan Pewaktuan Dalam Sistem Digital." },
    { "en": "Solusi Mengurangi 'Jitter'?", "id": "Gunakan Osilator Kristal Berkualitas Tinggi." },
    { "en": "Mengapa Kristal Osilator Perlu Kapasitor Beban?", "id": "Untuk Berosilasi Pada Frekuensi Yang Tepat." },
    { "en": "Masalah Jika Kapasitor Beban Salah?", "id": "Frekuensi Osilasi Akan Sedikit Bergeser." },
    { "en": "Apa Itu 'Decoupling' Dan 'Bypass'?", "id": "Dua Istilah Yang Sering Digunakan Bergantian." },
    { "en": "Tujuan Kapasitor 'Decoupling'?", "id": "Mengisolasi Derau Antar Bagian Rangkaian." },
    { "en": "Tujuan Kapasitor 'Bypass'?", "id": "Memberi Jalur Pintas Untuk Derau AC Ke Ground." },
    { "en": "Di Mana Kapasitor 'Bypass' Ditempatkan?", "id": "Sedekat Mungkin Dengan Pin Daya IC." },
    { "en": "Mengapa Nilai Kapasitor 'Bypass' Biasanya 0.1 µF?", "id": "Nilai Yang Baik Untuk Mem-bypass Frekuensi Menengah." },
    { "en": "Untuk Frekuensi Tinggi, Perlu Kapasitor Bagaimana?", "id": "Nilai Lebih Kecil (Contoh: 1 nF)." },
    { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Yang Tiba-tiba." },
    { "en": "Komponen Apa Yang Paling Rentan Terhadap ESD?", "id": "MOSFET Dan IC Berbasis CMOS." },
    { "en": "Bagaimana Cara Merusak Komponen Dengan ESD?", "id": "Dengan Menyentuhnya Tanpa Perlindungan." },
    { "en": "Apa Itu 'ESD Safe Workstation'?", "id": "Area Kerja Yang Dirancang Untuk Mencegah ESD." },
    { "en": "Terdiri Dari Apa Saja?", "id": "Alas Anti-statis, Gelang, Dan Titik Ground." },
    { "en": "Mengapa Kelembaban Udara Mempengaruhi ESD?", "id": "Udara Lembab Membantu Menghilangkan Muatan Statis." },
    { "en": "Kapan Risiko ESD Paling Tinggi?", "id": "Saat Udara Kering (Contoh: Musim Dingin)." },
    { "en": "Apa Itu 'Latch' Pada Pintu?", "id": "Mekanisme Pengunci." },
    { "en": "Apa Hubungannya Dengan SR Latch?", "id": "Sama-sama Mengunci Dalam Satu Keadaan." },
    { "en": "Mengapa Terjadi Pantulan (Bouncing) Pada Saklar?", "id": "Karena Sifat Mekanis Kontak Yang Memantul." },
    { "en": "Berapa Lama Pantulan Berlangsung?", "id": "Beberapa Milidetik." },
    { "en": "Masalah Pantulan Bagi Rangkaian Digital?", "id": "Dibaca Sebagai Banyak Penekanan Tombol." },
    { "en": "Solusi Pantulan Saklar (Debouncing)?", "id": "Gunakan Rangkaian RC Atau SR Latch." },
    { "en": "Bagaimana Rangkaian RC Melakukan 'Debouncing'?", "id": "Memperlambat Transisi Sinyal." },
    { "en": "Bagaimana SR Latch Melakukan 'Debouncing'?", "id": "Mengunci Keadaan Pada Kontak Pertama." },
    { "en": "Metode Mana Yang Lebih Andal?", "id": "Metode SR Latch Lebih Andal." },
    { "en": "Apa Itu 'Breakdown' Pada Isolator?", "id": "Ketika Isolator Gagal Dan Menjadi Konduktor." },
    { "en": "Penyebab 'Breakdown'?", "id": "Tegangan Terlalu Tinggi Melebihi Kekuatan Dielektrik." },
    { "en": "Apa Itu Kekuatan Dielektrik?", "id": "Kemampuan Isolator Menahan Medan Listrik." },
    { "en": "Mengapa PCB Diberi Lapisan Pelindung (Coating)?", "id": "Melindungi Dari Kelembaban, Debu, Dan Korosi." },
    { "en": "Apa Itu 'Conformal Coating'?", "id": "Lapisan Tipis Yang Mengikuti Kontur Papan." },
    { "en": "Mengapa Ada Lubang Pemasangan Pada PCB?", "id": "Untuk Memasang PCB Ke Casing Dengan Sekrup." },
    { "en": "Apakah Lubang Pemasangan Sering Terhubung Ke Ground?", "id": "Ya, Untuk Grounding Casing (Chassis Ground)." },
    { "en": "Apa Itu 'Heat Slug'?", "id": "Bagian Logam Pada IC Untuk Transfer Panas." },
    { "en": "'Heat Slug' Harus Terhubung Ke Mana?", "id": "Ke Heatsink Atau Area Tembaga Besar Di PCB." },
    { "en": "Mengapa Pengukuran Dengan Osiloskop Bisa Salah?", "id": "Kapasitansi Probe Membebani Rangkaian." },
    { "en": "Solusi Mengurangi Efek Probe?", "id": "Gunakan Probe 10x." },
    { "en": "Apa Artinya Probe 10x?", "id": "Meningkatkan Impedansi Input Sepuluh Kali Lipat." },
    { "en": "Apa Konsekuensi Menggunakan Probe 10x?", "id": "Sinyal Yang Diukur Dilemahkan Sepuluh Kali." },
    { "en": "Osiloskop Perlu Dikalibrasi?", "id": "Ya, Probe Perlu Dikompensasi." },
    { "en": "Bagaimana Cara Mengkompensasi Probe?", "id": "Mengatur Kapasitor Trimmer Pada Probe." },
    { "en": "Sinyal Apa Yang Digunakan Untuk Kompensasi?", "id": "Sinyal Gelombang Kotak Referensi Dari Osiloskop." },
    { "en": "Tampilan Gelombang Kotak Yang Baik Seperti Apa?", "id": "Sudutnya Tajam Sempurna Tanpa Overshoot." },
    { "en": "Apa Itu 'Aliasing' Pada Osiloskop Digital?", "id": "Bentuk Gelombang Salah Akibat Laju Sampling Rendah." },
    { "en": "Solusi Mencegah 'Aliasing'?", "id": "Gunakan Laju Sampling Jauh Lebih Tinggi." },
    { "en": "Aturan Umum Laju Sampling?", "id": "Minimal Dua Kali Frekuensi Sinyal Tertinggi." },
    { "en": "Aturan Ini Dikenal Sebagai Apa?", "id": "Teorema Sampling Nyquist-Shannon." },
    { "en": "Mengapa Membaca Manual Alat Itu Penting?", "id": "Untuk Memahami Semua Fitur Dan Batasannya." },
    { "en": "Apa Itu 'Autoranging' Multimeter?", "id": "Multimeter Secara Otomatis Memilih Rentang Ukur." },
    { "en": "Apa Keuntungan 'Autoranging'?", "id": "Lebih Mudah Digunakan." },
    { "en": "Apa Itu Pengukuran 'True RMS'?", "id": "Pengukuran Akurat Nilai RMS Untuk Gelombang Non-Sinusoidal." },
    { "en": "Multimeter Murah Biasanya Mengukur Apa?", "id": "RMS Rata-rata (Hanya Akurat Untuk Sinus)." },
    { "en": "Apa Itu 'Burden Voltage' Ammeter?", "id": "Jatuh Tegangan Pada Ammeter Saat Mengukur." },
    { "en": "Masalah 'Burden Voltage'?", "id": "Dapat Mengubah Perilaku Rangkaian Tegangan Rendah." },
    { "en": "Apa Itu 'Dielectric Absorption' Pada Kapasitor?", "id": "Efek Kapasitor 'Mengingat' Muatan Sebelumnya." },
    { "en": "Masalah 'Dielectric Absorption'?", "id": "Menyebabkan Ketidakakuratan Pada Rangkaian Presisi." },
    { "en": "Apa Itu ESR (Equivalent Series Resistance)?", "id": "Resistansi Internal Non-Ideal Sebuah Kapasitor." },
    { "en": "Masalah ESR Tinggi?", "id": "Menyebabkan Pemanasan Kapasitor Dan Inefisiensi." },
    { "en": "Di Mana ESR Menjadi Masalah Besar?", "id": "Pada Catu Daya Switching." },
    { "en": "Bagaimana Mengukur ESR?", "id": "Menggunakan ESR Meter Khusus." },
    { "en": "Kapasitor Kering Memiliki ESR Bagaimana?", "id": "ESR Menjadi Sangat Tinggi." },
    { "en": "Mengapa Kepala Sekrup Mudah Rusak?", "id": "Menggunakan Obeng Dengan Ukuran Yang Salah." },
    { "en": "Solusi Mencegah Kepala Sekrup Rusak?", "id":"Gunakan Mata Obeng Yang Pas Dan Tepat." },
    { "en": "Apa Itu 'Wire Gauge' (AWG)?", "id": "Standar Untuk Ukuran Diameter Kawat." },
    { "en": "Angka AWG Lebih Kecil Berarti Kawat Bagaimana?", "id": "Kawat Lebih Tebal." },
    { "en": "Kawat Lebih Tebal Bisa Menghantarkan Arus Bagaimana?", "id": "Arus Yang Lebih Besar." },
    { "en": "Apa Itu 'Strain Relief'?", "id": "Bagian Untuk Mencegah Kabel Tertekuk Tajam." },
    { "en": "Di Mana 'Strain Relief' Ditemukan?", "id": "Di Pangkal Kabel Pengisi Daya Atau Perkakas." },
    { "en": "Mengapa 'Strain Relief' Penting?", "id": "Mencegah Putusnya Konduktor Internal Kabel." },
    { "en": "Apa Itu 'Hot Swap'?", "id": "Memasang Atau Melepas Komponen Saat Daya Menyala." },
    { "en": "Apakah Semua Komponen Bisa Di-'Hot Swap'?", "id": "Tidak, Hanya Yang Dirancang Khusus." },
    { "en": "Contoh Komponen 'Hot Swap'?", "id": "USB Drive, Hard Drive Server." },
    { "en": "Risiko Melakukan 'Hot Swap' Sembarangan?", "id": "Kerusakan Akibat Lonjakan Arus Atau ESD." },
    { "en": "Apa Itu 'Brownout'?", "id": "Penurunan Tegangan Jaringan Listrik Sementara." },
    { "en": "Masalah Akibat 'Brownout'?", "id": "Dapat Menyebabkan Reset Atau Kerusakan Peralatan." },
    { "en": "Solusi Melindungi Dari 'Brownout'?", "id": "Gunakan UPS (Uninterruptible Power Supply)." },
    { "en": "Apa Itu 'Surge' atau Lonjakan Tegangan?", "id": "Kenaikan Tegangan Sangat Cepat Dan Singkat." },
    { "en": "Penyebab 'Surge'?", "id": "Sambaran Petir Atau Peralatan Besar Dinyalakan." },
    { "en": "Solusi Melindungi Dari 'Surge'?", "id": "Gunakan Pelindung Lonjakan (Surge Protector)." },
    { "en": "Komponen Apa Di Dalam 'Surge Protector'?", "id": "MOV (Metal Oxide Varistor)." },
    { "en": "Bagaimana Cara Kerja MOV?", "id": "Menjadi Konduktif Pada Tegangan Tinggi." },
    { "en": "Apakah MOV Bisa Aus?", "id": "Ya, Setelah Menyerap Beberapa Kali Lonjakan." },
    { "en": "Apa Itu 'Common-Mode Noise'?", "id": "Derau Yang Muncul Bersamaan Di Kedua Jalur." },
    { "en": "Apa Itu 'Differential-Mode Noise'?", "id": "Derau Yang Muncul Antara Dua Jalur." },
    { "en": "Solusi Mengatasi 'Common-Mode Noise'?", "id": "Gunakan 'Common-Mode Choke' (Filter Ferit)." },
    { "en": "Mengapa Kabel Data Seringkali Dipilin (Twisted Pair)?", "id": "Untuk Menolak Interferensi Elektromagnetik." },
    { "en": "Contoh Kabel 'Twisted Pair'?", "id": "Kabel Jaringan Ethernet." },
    { "en": "Mengapa Kabel Audio Berkualitas Menggunakan Pelindung?", "id": "Untuk Mencegah Derau 'Hum' Dan RFI." },
    { "en": "Pelindung Kabel (Shield) Dihubungkan Ke Mana?", "id": "Ke Ground Di Salah Satu Ujungnya." },
    { "en": "Mengapa Hanya Di Satu Ujung?", "id": "Untuk Menghindari 'Ground Loop'." },
    { "en": "Apa Itu 'Floating Ground'?", "id": "Ground Rangkaian Yang Tidak Terhubung Ke Tanah." },
    { "en": "Apakah 'Floating Ground' Berbahaya?", "id": "Bisa, Karena Potensialnya Bisa Mengambang Tinggi." },
    { "en": "Mengapa Ujung Solder Perlu Dilapisi Timah?", "id": "Untuk Transfer Panas Yang Baik Dan Mencegah Oksidasi." },
    { "en": "Apa Itu Fluks (Flux) Dalam Solder?", "id": "Bahan Kimia Untuk Membersihkan Permukaan Logam." },
    { "en": "Masalah Jika Menyolder Tanpa Fluks?", "id": "Solderan Tidak Akan Menempel Dengan Baik." },
    { "en": "Timah Solder Modern Sudah Mengandung Fluks?", "id": "Ya, Di Bagian Intinya (Rosin Core)." },
    { "en": "Mengapa Asap Solder Berbahaya?", "id": "Mengandung Uap Timbal Dan Bahan Kimia Fluks." },
    { "en": "Solusi Mengatasi Asap Solder?", "id": "Gunakan Kipas Penyedot Asap (Fume Extractor)." },
    { "en": "Berapa Suhu Ideal Untuk Menyolder?", "id": "Sekitar 300-350 Derajat Celcius." },
    { "en": "Masalah Jika Suhu Terlalu Rendah?", "id": "Menghasilkan 'Cold Solder Joint'." },
    { "en": "Masalah Jika Suhu Terlalu Tinggi?", "id": "Merusak Komponen Dan Mengelupaskan Jalur PCB." },
    { "en": "Apa Itu 'Wetting' Dalam Penyolderan?", "id": "Kemampuan Timah Cair Membasahi Permukaan Logam." },
    { "en": "'Wetting' Yang Baik Menghasilkan Solderan Bagaimana?", "id": "Mengkilap, Halus, Dan Berbentuk Cekung." },
    { "en": "Apa Itu 'Wicking'?", "id": "Terserapnya Solder Ke Dalam Jalinan Kawat." },
    { "en": "Apa Itu 'Tombstoning' Pada Komponen SMD?", "id": "Komponen Berdiri Tegak Seperti Batu Nisan." },
    { "en": "Penyebab 'Tombstoning'?", "id": "Pemanasan Yang Tidak Merata Pada Kedua Sisi." },
    { "en": "Apa Itu 'Solder Mask'?", "id": "Lapisan Pelindung Hijau/Biru/Merah Pada PCB." },
    { "en": "Fungsi 'Solder Mask'?", "id": "Mencegah 'Solder Bridge' Antar Jalur." },
    { "en": "Apa Itu 'Silkscreen'?", "id": "Tulisan Putih Pada PCB." },
    { "en": "Fungsi 'Silkscreen'?", "id": "Menunjukkan Nama Dan Posisi Komponen." },
    { "en": "Mengapa Ada Bau Ikan Dari Beberapa Plastik Elektronik?", "id": "Pelepasan Amina Dari Bahan Tahan Api." },
    { "en": "Apa Itu 'RoHS'?", "id": "Direktif Yang Membatasi Bahan Berbahaya." },
    { "en": "Bahan Apa Yang Dibatasi RoHS?", "id": "Timbal, Merkuri, Kadmium, Dll." },
    { "en": "Mengapa Solder Bebas Timbal Lebih Sulit Digunakan?", "id": "Membutuhkan Suhu Lebih Tinggi." },
    { "en": "Apa Itu 'Whiskering'?", "id": "Pertumbuhan Filamen Logam Dari Permukaan." },
    { "en": "Solder Bebas Timbal Rentan Terhadap 'Whiskering'?", "id": "Ya, Terutama Yang Berbasis Timah Murni." },
    { "en": "Mengapa Pin IC Diberi Jarak Tertentu?", "id": "Untuk Mencegah Hubung Singkat Dan 'Arcing'." },
    { "en": "Apa Itu 'Pitch' Kaki IC?", "id": "Jarak Antara Pusat Dua Kaki Berdekatan." },
    { "en": "Apa Itu 'Creepage' Dan 'Clearance'?", "id": "Jarak Aman Untuk Mencegah Lonjakan Listrik." },
    { "en": "'Creepage' Adalah Jarak Melalui Apa?", "id": "Melalui Permukaan Isolator." },
    { "en": "'Clearance' Adalah Jarak Melalui Apa?", "id": "Melalui Udara (Jarak Terpendek)." },
    { "en": "Di Mana Jarak Ini Sangat Penting?", "id": "Pada Rangkaian Tegangan Tinggi." },
    { "en": "Mengapa Ada Slot Atau Lubang Di PCB Tegangan Tinggi?", "id": "Untuk Menambah Jarak 'Creepage'." },
    { "en": "Apa Itu 'Potting' atau Pengecoran?", "id": "Mengisi Rangkaian Dengan Resin Epoksi." },
    { "en": "Tujuan 'Potting'?", "id": "Melindungi Dari Getaran, Kelembaban, Dan Kerahasiaan." },
    { "en": "Apa Kerugian 'Potting'?", "id": "Sirkuit Menjadi Tidak Dapat Diperbaiki." },
    { "en": "Mengapa Terkadang Kabel Diberi Ferit?", "id": "Sebagai Filter EMI Frekuensi Tinggi." },
    { "en": "Ferit Berfungsi Sebagai Apa?", "id": "Induktor Mode Bersama (Common-mode Choke)." },
    { "en": "Mengapa Tombol Remote Perlu Ditekan Keras?", "id": "Lapisan Konduktif Pada Karet Sudah Aus." },
    { "en": "Solusi Tombol Remote Keras?", "id": "Bersihkan Kontak Atau Gunakan Kit Perbaikan." },
    { "en": "Apa Itu 'Trimpot' Atau Potensiometer Trimmer?", "id": "Potensiometer Kecil Untuk Penyetelan Internal." },
    { "en": "Mengapa 'Trimpot' Perlu Disegel Setelah Penyetelan?", "id": "Agar Tidak Berubah Akibat Getaran." },
    { "en": "Apa Bahan Penyegel Yang Digunakan?", "id": "Cat Kuku Atau Lak Khusus." },
    { "en": "Apa Itu 'Murphy's Law' Dalam Elektronika?", "id": "Jika Sesuatu Bisa Salah, Maka Akan Salah." },
    { "en": "Contoh 'Murphy's Law'?", "id": "Memasang Komponen Terbalik Tepat Sebelum Demo." },
    { "en": "Solusi Melawan 'Murphy's Law'?", "id": "Periksa Kembali Pekerjaan Anda Tiga Kali." },
    { "en": "Mengapa Dokumentasi Proyek Itu Penting?", "id": "Agar Mudah Diperbaiki Atau Dikembangkan Nanti." },
    { "en": "Apa Itu Skema Rangkaian?", "id": "Diagram Yang Menunjukkan Koneksi Logis Komponen." },
    { "en": "Apa Itu Layout PCB?", "id": "Gambar Yang Menunjukkan Penempatan Fisik Komponen." },
    { "en": "Mengapa Skema Dan Layout Berbeda?", "id": "Layout Memperhatikan Aspek Fisik (Ukuran, Jarak)." },
    { "en": "Apa Itu 'Reverse Engineering'?", "id": "Menganalisis Produk Untuk Memahami Cara Kerjanya." },
    { "en": "Langkah Pertama 'Reverse Engineering' PCB?", "id": "Menggambar Ulang Skema Rangkaian Dari Layout." },
    { "en": "Mengapa Terkadang Komponen Dihapus Labelnya?", "id": "Untuk Mencegah Peniruan." },
    { "en": "Apa Itu 'Logic Gate'?", "id": "Gerbang Logika." },
    { "en": "Apa Itu 'Truth Table'?", "id": "Tabel Kebenaran." },
    { "en": "Fungsi Tabel Kebenaran?", "id": "Menjelaskan Operasi Gerbang Logika." },
    { "en": "Mengapa Alat Ukur Perlu Dikalibrasi?", "id": "Untuk Memastikan Akurasi Pengukurannya." },
    { "en": "Apa Itu Standar Kalibrasi?", "id": "Referensi Yang Diketahui Sangat Akurat." },
    { "en": "Apa Itu 'Drift'?", "id": "Pergeseran Lambat Nilai Komponen Seiring Waktu." },
    { "en": "Komponen Apa Yang Cenderung 'Drift'?", "id": "Resistor, Kapasitor, Kristal Osilator." },
    { "en": "Solusi Mengatasi 'Drift'?", "id": "Gunakan Komponen Berkualitas Tinggi, Lakukan Kalibrasi." },
    { "en": "Mengapa Kabel Koaksial Digunakan Untuk Sinyal RF?", "id": "Memberikan Perisai Yang Baik Terhadap Interferensi." },
    { "en": "Apa Itu Impedansi Karakteristik Kabel Koaksial?", "id": "Biasanya 50 Atau 75 Ohm." },
    { "en": "Pentingkah Mencocokkan Impedansi Ini?", "id": "Sangat Penting Untuk Mencegah Refleksi Sinyal." },
    { "en": "Apa Itu 'Printed Circuit Board' (PCB)?", "id": "Papan Untuk Menghubungkan Komponen Elektronik." },
    { "en": "Bahan Dasar PCB?", "id": "Fiberglass (FR-4) Dengan Lapisan Tembaga." },
    { "en": "Apa Itu Proses Etsa (Etching)?", "id": "Menghilangkan Tembaga Yang Tidak Diinginkan." },
    { "en": "Mengapa Ada Lubang Di PCB?", "id": "Untuk Memasang Kaki Komponen 'Through-Hole'." },
    { "en": "Apa Itu Komponen 'Through-Hole'?", "id": "Komponen Dengan Kaki Kawat Panjang." },
    { "en": "Apa Itu Komponen 'Surface-Mount' (SMD)?", "id": "Komponen Yang Disolder Di Permukaan Papan." },
    { "en": "Keuntungan SMD?", "id": "Ukuran Lebih Kecil, Perakitan Otomatis." },
    { "en": "Kerugian SMD?", "id": "Lebih Sulit Untuk Diperbaiki Secara Manual." },
    { "en": "Apa Itu 'Breadboard'?", "id": "Papan Prototipe Tanpa Solder." },
    { "en": "Masalah Umum 'Breadboard'?", "id": "Kontak Longgar, Kapasitansi Parasitik Tinggi." },
    { "en": "Apa Itu 'Perfboard' atau 'Veroboard'?", "id": "Papan Prototipe Dengan Lubang Dan Jalur Tembaga." },
    { "en": "Keuntungan 'Perfboard'?", "id": "Lebih Permanen Daripada 'Breadboard'." },
    { "en": "Mengapa Tegangan Baterai Baru Sedikit Lebih Tinggi?", "id": "Karena Belum Ada Beban Internal." },
    { "en": "Apa Itu 'Internal Resistance' Baterai?", "id": "Resistansi Thevenin Dari Baterai." },
    { "en": "Apa Yang Terjadi Pada Resistansi Internal Seiring Usia?", "id": "Akan Meningkat." },
    { "en": "Dampaknya?", "id": "Baterai Tidak Bisa Memberi Arus Besar." },
    { "en": "Mengapa Ada Peringkat 'C' Pada Baterai Lipo?", "id": "Menunjukkan Kemampuan Arus Pengosongan Maksimal." },
    { "en": "Arti 20C Pada Baterai 1000mAh?", "id": "Bisa Memberi Arus 20 * 1000mA = 20A." },
    { "en": "Bahaya Mengosongkan Baterai Lipo Berlebihan?", "id": "Kerusakan Permanen Pada Sel Baterai." },
    { "en": "Berapa Tegangan Minimum Per Sel Lipo?", "id": "Sekitar 3.0 Hingga 3.3 Volt." },
    { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sirkuit Elektronik Untuk Melindungi Baterai." },
    { "en": "Fungsi BMS?", "id": "Mencegah Pengisian/Pengosongan Berlebih, Menyeimbangkan Sel." },
    { "en": "Mengapa Sel Baterai Perlu Diseimbangkan?", "id": "Agar Semua Sel Memiliki Tingkat Muatan Sama." },
    { "en": "Apa Itu 'Thermal Imaging Camera'?", "id": "Kamera Yang Melihat Radiasi Panas." },
    { "en": "Kegunaannya Dalam Elektronika?", "id": "Menemukan Komponen Yang Terlalu Panas." },
    { "en": "Apa Itu Semprotan Pembeku (Freeze Spray)?", "id": "Cairan Yang Menguap Cepat Untuk Mendinginkan." },
    { "en": "Kegunaan Semprotan Pembeku?", "id": "Menemukan Komponen Rusak Yang Sensitif Suhu." },
    { "en": "Apa Itu 'Intermittent Fault'?", "id": "Masalah Yang Muncul Dan Hilang Secara Acak." },
    { "en": "'Intermittent Fault' Sulit Didiagnosis?", "id": "Ya, Salah Satu Masalah Paling Sulit." },
    { "en": "Penyebab Umum 'Intermittent Fault'?", "id": "Solderan Retak, Konektor Longgar." },
    { "en": "Solusi Menemukan 'Intermittent Fault'?", "id": "Goyangkan Papan, Ketuk Komponen, Gunakan Semprotan Pembeku." },
    { "en": "Mengapa Pengukuran Ohmmeter Dalam Rangkaian Bisa Salah?", "id": "Komponen Lain Mempengaruhi Hasil Pengukuran." },
    { "en": "Solusi Pengukuran Ohmmeter Akurat?", "id": "Lepaskan Satu Kaki Komponen Dari Rangkaian." },
    { "en": "Apa Itu Tes Dioda Pada Multimeter?", "id": "Mengukur Jatuh Tegangan Maju Dioda." },
    { "en": "Berapa Jatuh Tegangan Dioda Silikon Normal?", "id": "Sekitar 0.6 Hingga 0.7 Volt." },
    { "en": "Apa Artinya Jika Hasilnya Nol?", "id": "Dioda Terhubung Singkat." },
    { "en": "Apa Artinya Jika Hasilnya 'OL'?", "id": "Dioda Terbuka." },
    { "en": "Bagaimana Menguji Transistor BJT Dengan Multimeter?", "id": "Uji Seperti Dua Dioda Yang Terhubung." },
    { "en": "Sambungan Apa Saja Yang Diuji?", "id": "Basis-Emitor Dan Basis-Kolektor." },
    { "en": "Apa Itu 'Leakage Current'?", "id": "Arus Kecil Yang Mengalir Saat Seharusnya Tidak." },
    { "en": "Contoh 'Leakage Current'?", "id": "Arus Mundur Dioda, Arus Melalui Kapasitor." },
    { "en": "Apa Itu 'Magic Smoke'?", "id": "Istilah Candaan Untuk Asap Dari Komponen Terbakar." },
    { "en": "Apa Artinya Jika 'Magic Smoke' Keluar?", "id": "Komponen Tersebut Sudah Rusak Total." },
    { "en": "Apa Itu 'Hot-Air Rework Station'?", "id": "Alat Solder Menggunakan Udara Panas." },
    { "en": "Kegunaan 'Hot-Air Rework'?", "id": "Menyolder Dan Melepas Komponen SMD." },
    { "en": "Mengapa Pembumian (Grounding) Penting?", "id": "Untuk Keselamatan Dan Kinerja Rangkaian." },
    { "en": "Apa Beda 'Chassis Ground' Dan 'Signal Ground'?", "id": "'Chassis' Ke Casing, 'Signal' Referensi Sinyal." },
    { "en": "Haruskah Keduanya Selalu Dihubungkan?", "id": "Tergantung Desain, Kadang Perlu Diisolasi." },
    { "en": "Apa Itu 'Galvanic Isolation'?", "id": "Mengisolasi Bagian Rangkaian Tanpa Sambungan Listrik." },
    { "en": "Bagaimana Cara Mencapai 'Galvanic Isolation'?", "id": "Menggunakan Trafo, Optocoupler, Atau Kapasitor." },
    { "en": "Tujuan 'Galvanic Isolation'?", "id": "Keselamatan Dan Memutus 'Ground Loop'." },
    { "en": "Apa Itu 'Current Sensing Resistor'?", "id": "Resistor Bernilai Sangat Rendah Untuk Mengukur Arus." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Mengukur Jatuh Tegangan Kecil Di Atasnya." },
    { "en": "Mengapa Perangkat Lunak Perlu Di-'Debug'?", "id": "Untuk Menemukan Dan Memperbaiki Kesalahan (Bug)." },
    { "en": "Apa Itu 'Debugger' Perangkat Keras?", "id": "Alat Untuk Mengontrol Dan Memeriksa Mikrokontroler." },
    { "en": "Contoh 'Debugger'?", "id": "JTAG, SWD." },
    { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Yang Tertanam Dalam Perangkat Keras." },
    { "en": "Contoh 'Firmware'?", "id": "BIOS Komputer, Program Di Arduino." },
    { "en": "Apa Itu 'Bricking'?", "id": "Membuat Perangkat Elektronik Menjadi Tidak Berguna." },
    { "en": "Penyebab 'Bricking'?", "id": "Kegagalan Saat Memperbarui Firmware." },
    { "en": "Mengapa Perlu Membersihkan Fluks Setelah Menyolder?", "id": "Sisa Fluks Bisa Korosif Atau Konduktif." },
    { "en": "Cairan Apa Untuk Membersihkan Fluks?", "id": "Alkohol Isopropil (IPA)." },
    { "en": "Apa Itu 'Tinning' Ujung Kabel?", "id": "Melapisi Ujung Kabel Serabut Dengan Timah." },
    { "en": "Tujuan 'Tinning'?", "id": "Mencegah Ujungnya Terurai Dan Memudahkan Penyolderan." },
    { "en": "Apa Itu 'Heat Shrink Tubing'?", "id": "Selongsong Plastik Yang Mengerut Saat Dipanaskan." },
    { "en": "Fungsi 'Heat Shrink'?", "id": "Mengisolasi Sambungan Kabel." },
    { "en": "Mengapa Terkadang Dioda Dipasang Terbalik Paralel Relay?", "id": "Sebagai Dioda 'Freewheeling' Atau 'Flyback'." },
    { "en": "Fungsi Dioda 'Flyback'?", "id": "Melindungi Transistor Dari Lonjakan Tegangan Induktif." },
    { "en": "Apa Itu Beban Induktif?", "id": "Beban Yang Mengandung Kumparan (Motor, Relay)." },
    { "en": "Apa Itu 'Contact Bounce'?", "id": "Pantulan Kontak Mekanis Pada Saklar Atau Relay." },
    { "en": "Masalah 'Contact Bounce'?", "id": "Dapat Dibaca Salah Oleh Sirkuit Digital." },
    { "en": "Solusi 'Contact Bounce'?", "id": "Debouncing Dengan Perangkat Keras Atau Lunak." },
    { "en": "Apa Itu 'Current Loop' 4-20mA?", "id": "Standar Sinyal Analog Industri." },
    { "en": "Keuntungan 'Current Loop'?", "id": "Kebal Terhadap Derau Dan Jatuh Tegangan." },
    { "en": "Mengapa Dimulai Dari 4mA, Bukan 0mA?", "id": "Untuk Mendeteksi Kabel Putus (0mA = Rusak)." },
    { "en": "Apa Itu 'Differential Amplifier'?", "id": "Menguatkan Selisih Antara Dua Sinyal." },
    { "en": "Kegunaan 'Differential Amplifier'?", "id": "Menolak Derau 'Common-mode'." },
    { "en": "Apa Itu 'Instrumentation Amplifier'?", "id": "Penguat Diferensial Presisi Tinggi." },
    { "en": "Ciri Khasnya?", "id": "Impedansi Input Sangat Tinggi, CMRR Tinggi." },
    { "en": "Apa Itu 'Schematic Capture'?", "id": "Proses Menggambar Skema Rangkaian Di Komputer." },
    { "en": "Apa Itu 'PCB Layout Design'?", "id": "Proses Mendesain Penempatan Komponen Dan Jalur." },
    { "en": "Perangkat Lunak Untuk Ini Disebut Apa?", "id": "EDA (Electronic Design Automation) Software." },
    { "en": "Contoh Perangkat Lunak EDA?", "id": "Eagle, KiCad, Altium Designer." },
    { "en": "Apa Itu 'Gerber File'?", "id": "Format File Standar Untuk Manufaktur PCB." },
    { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Semua Komponen Yang Dibutuhkan." },
    { "en": "Apa Itu 'Pick and Place Machine'?", "id": "Mesin Otomatis Untuk Menempatkan Komponen SMD." },
    { "en": "Apa Itu 'Wave Soldering'?", "id": "Metode Penyolderan Untuk Komponen 'Through-Hole'." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Papan Dilewatkan Di Atas Gelombang Timah Cair." },
    { "en": "Apa Itu 'Reflow Oven'?", "id": "Oven Untuk Melelehkan Pasta Solder Komponen SMD." },
    { "en": "Ringkasan: Masalah Paling Umum?", "id": "Koneksi Buruk (Solderan, Kabel, Konektor)." },
    { "en": "Ringkasan: Solusi Pertama Untuk Perangkat Mati?", "id": "Periksa Sumber Daya (Baterai, Kabel, Sekring)." },
    { "en": "Ringkasan: Penyebab Utama Komponen Terbakar?", "id": "Arus Atau Tegangan Yang Berlebihan." },
    { "en": "Ringkasan: Alat Diagnostik Paling Dasar?", "id": "Multimeter." },
    { "en": "Ringkasan: Alat Untuk Melihat Sinyal?", "id": "Osiloskop." },
    { "en": "Ringkasan: Kunci Mencegah Kerusakan ESD?", "id": "Gunakan Gelang Anti-Statis." },
    { "en": "Ringkasan: Mengapa Komponen Menjadi Panas?", "id": "Disipasi Daya (Energi Yang Hilang)." },
    { "en": "Ringkasan: Solusi Komponen Panas?", "id": "Pendingin (Heatsink) Dan Ventilasi Yang Baik." },
    { "en": "Ringkasan: Masalah Umum Audio?", "id": "Derau 'Hum', Distorsi, Dan Suara 'Pop'." },
    { "en": "Ringkasan: Solusi Derau 'Hum'?", "id": "Perbaiki Grounding (Hindari Ground Loop)." },
    { "en": "Apa Itu 'Troubleshooting'?", "id": "Proses Sistematis Untuk Menemukan Dan Memperbaiki Masalah." },
    { "en": "Langkah Pertama 'Troubleshooting'?", "id": "Amati Gejala Dan Kumpulkan Informasi." },
    { "en": "Langkah Kedua 'Troubleshooting'?", "id": "Bagi Masalah Menjadi Bagian-bagian Kecil." },
    { "en": "Prinsip 'Divide and Conquer'?", "id": "Pisahkan Bagian Yang Bekerja Dan Yang Tidak." },
    { "en": "Contohnya?", "id": "Periksa Catu Daya Terlebih Dahulu." },
    { "en": "Langkah Ketiga 'Troubleshooting'?", "id": "Buat Hipotesis Tentang Penyebab Masalah." },
    { "en": "Langkah Keempat 'Troubleshooting'?", "id": "Uji Hipotesis Dengan Pengukuran." },
    { "en": "Langkah Terakhir 'Troubleshooting'?", "id": "Perbaiki Masalah Dan Verifikasi Solusinya." },
    { "en": "Mengapa Penting Untuk Mengubah Satu Hal Saja?", "id": "Agar Tahu Mana Yang Menjadi Solusinya." },
    { "en": "Apa Alat Bantu Selain Alat Ukur?", "id": "Skema Rangkaian Dan 'Datasheet'." },
    { "en": "Mengapa Indra Manusia Berguna?", "id": "Bisa Mendeteksi Bau Terbakar Atau Suara Aneh." },
    { "en": "Apa Itu 'Rubber Duck Debugging'?", "id": "Menjelaskan Masalah Kepada Objek Mati." },
    { "en": "Mengapa Ini Bekerja?", "id": "Membantu Menjernihkan Pikiran Dan Menemukan Kesalahan." },
    { "en": "Apa Itu 'Root Cause Analysis'?", "id": "Mencari Penyebab Akar Suatu Masalah." },
    { "en": "Mengapa Tidak Hanya Memperbaiki Gejala?", "id": "Karena Masalah Akan Terjadi Lagi." },
    { "en": "Apa Itu 'Interference Fit'?", "id": "Ketika Komponen Mekanis Saling Bertabrakan." },
    { "en": "Masalah Desain Mekanis Penting Dalam Elektronika?", "id": "Ya, Terutama Untuk Penempatan Konektor Dan Heatsink." },
    { "en": "Apa Itu 'Tolerance Stack-up'?", "id": "Akumulasi Toleransi Manufaktur." },
    { "en": "Masalah 'Tolerance Stack-up'?", "id": "Bisa Menyebabkan Casing Tidak Pas." },
    { "en": "Apa Itu 'First-Pass Success'?", "id": "Desain Berhasil Bekerja Pada Percobaan Pertama." },
    { "en": "Apakah Ini Umum Terjadi?", "id": "Jarang, Biasanya Perlu Beberapa Kali Revisi." },
    { "en": "Apa Itu Revisi PCB (Contoh: Rev A, Rev B)?", "id": "Versi Papan Yang Berbeda Setelah Perbaikan." },
    { "en": "Apa Itu 'Compliance Testing'?", "id": "Pengujian Untuk Memastikan Produk Memenuhi Standar." },
    { "en": "Contoh Standar?", "id": "FCC (Untuk EMI), UL/CE (Untuk Keselamatan)." },
    { "en": "Apa Itu FCC?", "id": "Federal Communications Commission (Di AS)." },
    { "en": "Apa Itu CE Marking?", "id": "Tanda Kepatuhan Untuk Produk Di Eropa." },
    { "en": "Apa Itu 'Mean Time Between Failures' (MTBF)?", "id": "Waktu Rata-rata Yang Diharapkan Antar Kegagalan." },
    { "en": "MTBF Tinggi Berarti Produk Bagaimana?", "id": "Sangat Andal." },
    { "en": "Apa Itu 'Failure Mode and Effects Analysis' (FMEA)?", "id": "Metodologi Untuk Menganalisis Potensi Kegagalan." },
    { "en": "Apa Itu 'Planned Obsolescence'?", "id": "Merancang Produk Agar Cepat Usang." },
    { "en": "Mengapa Penting Menggunakan Komponen Asli?", "id": "Komponen Palsu Memiliki Kinerja Buruk Dan Tidak Andal." },
    { "en": "Bagaimana Mengenali Komponen Palsu?", "id": "Cetakannya Buruk, Harganya Terlalu Murah." },
    { "en": "Apa Risiko Menggunakan Perangkat Lunak Bajakan?", "id": "Risiko Virus Dan Tidak Ada Dukungan Teknis." },
    { "en": "Apa Itu 'Open Source Hardware'?", "id": "Desain Perangkat Keras Yang Dibagikan Secara Bebas." },
    { "en": "Contoh 'Open Source Hardware'?", "id": "Arduino." },
    { "en": "Apa Itu 'Open Source Software'?", "id": "Perangkat Lunak Dengan Kode Sumber Terbuka." },
    { "en": "Contoh 'Open Source Software'?", "id": "Linux, KiCad." },
    { "en": "Apa Itu Forum Komunitas?", "id": "Tempat Bertanya Dan Berbagi Informasi." },
    { "en": "Mengapa Forum Bermanfaat Untuk 'Troubleshooting'?", "id": "Mungkin Orang Lain Pernah Mengalami Masalah Sama." },
    { "en": "Apa Itu 'Signal Ground'?", "id": "Titik Referensi Nol Volt Untuk Sinyal." },
    { "en": "Apa Itu 'Chassis Ground'?", "id": "Sambungan Ke Rangka Logam Perangkat." },
    { "en": "Apa Itu 'Earth Ground'?", "id": "Sambungan Fisik Ke Tanah." },
    { "en": "Ketiganya Bisa Menjadi Titik Yang Sama?", "id": "Ya, Seringkali Dihubungkan Bersama." },
    { "en": "Apa Itu Isolator?", "id": "Bahan Yang Tidak Menghantarkan Listrik." },
    { "en": "Contoh Isolator?", "id": "Karet, Plastik, Kaca, Udara Kering." },
    { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Menghantarkan Listrik Dengan Baik." },
    { "en": "Contoh Konduktor?", "id": "Tembaga, Perak, Emas, Aluminium." },
    { "en": "Apa Itu Semikonduktor?", "id": "Bahan Dengan Sifat Di Antara Keduanya." },
    { "en": "Contoh Semikonduktor?", "id": "Silikon, Germanium." },
    { "en": "Elektronik Modern Berbasis Apa?", "id": "Teknologi Semikonduktor." },
    { "en": "Apa Itu Dioda?", "id": "Komponen Yang Hanya Melewatkan Arus Satu Arah." },
    { "en": "Apa Itu Transistor?", "id": "Komponen Sebagai Saklar Atau Penguat." },
    { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Jutaan Transistor Dan Komponen Dalam Satu Chip." },
    { "en": "Apa Itu 'Through-hole Technology'?", "id": "Teknologi Pemasangan Komponen Dengan Kaki Kawat." },
    { "en": "Apa Itu 'Surface-mount Technology' (SMT)?", "id": "Teknologi Pemasangan Komponen Di Permukaan PCB." },
    { "en": "Teknologi Mana Yang Dominan Saat Ini?", "id": "SMT." },
    { "en": "Apa Itu 'Occam's Razor' Dalam 'Troubleshooting'?", "id": "Penjelasan Paling Sederhana Biasanya Yang Benar." },
    { "en": "Contohnya?", "id": "Periksa Kabel Power Sebelum Membongkar Catu Daya." },
    { "en": "Mengapa Keselamatan Selalu Menjadi Prioritas Utama?", "id": "Karena Listrik Bisa Berbahaya Dan Mematikan." },
    { "en": "Aturan Keselamatan Utama?", "id": "Jangan Pernah Bekerja Sendirian Pada Tegangan Tinggi." },
    { "en": "Aturan Lain?", "id": "Gunakan Satu Tangan Jika Memungkinkan." },
    { "en": "Mengapa Menggunakan Satu Tangan?", "id": "Mencegah Arus Mengalir Melewati Jantung." },
    { "en": "Apa Itu Trafo Isolasi?", "id": "Trafo Untuk Mengisolasi Rangkaian Dari Jaringan PLN." },
    { "en": "Mengapa Trafo Isolasi Meningkatkan Keselamatan?", "id": "Meniadakan Referensi Langsung Ke Tanah." },
    { "en": "Apa Yang Harus Dilakukan Sebelum Menyentuh Rangkaian?", "id": "Pastikan Semua Kapasitor Besar Sudah Dikosongkan." },
    { "en": "Bagaimana Cara Mengosongkan Kapasitor?", "id": "Hubung Singkat Dengan Resistor Daya." },
    { "en": "Mengapa Tidak Langsung Dihubung Singkat?", "id": "Menghasilkan Arus Sangat Besar Yang Berbahaya." },
    { "en": "Apa Itu Peringkat IP (Ingress Protection)?", "id": "Menunjukkan Tingkat Perlindungan Terhadap Debu Dan Air." },
    { "en": "Contoh IP67?", "id": "Tahan Debu Total Dan Tahan Terendam Air." },
    { "en": "Mengapa Penting Memahami Skema?", "id": "Karena Itu Adalah Peta Rangkaian." },
    { "en": "Tanpa Skema, Perbaikan Menjadi Bagaimana?", "id": "Jauh Lebih Sulit, Seperti Menebak-nebak." },
    { "en": "Apa Aturan Emas Terakhir Dalam Elektronika?", "id": "Jangan Pernah Berhenti Belajar." },
    { "en": "Apa Itu Osiloskop?", "id": "Alat Untuk Melihat Bentuk Gelombang Tegangan." },
    { "en": "Sumbu Horizontal Osiloskop Mewakili Apa?", "id": "Waktu." },
    { "en": "Sumbu Vertikal Osiloskop Mewakili Apa?", "id": "Tegangan." },
    { "en": "Apa Itu 'Triggering' Pada Osiloskop?", "id": "Menstabilkan Tampilan Gelombang Yang Berulang." },
    { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Untuk Menganalisis Banyak Sinyal Digital." },
    { "en": "Perbedaan Utama Dengan Osiloskop?", "id": "Melihat Logika (1/0), Bukan Level Analog." },
    { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Alat Untuk Melihat Sinyal Dalam Domain Frekuensi." },
    { "en": "Sumbu Horizontal 'Spectrum Analyzer'?", "id": "Frekuensi." },
    { "en": "Sumbu Vertikal 'Spectrum Analyzer'?", "id": "Amplitudo Atau Daya." },
    { "en": "Apa Itu Generator Fungsi?", "id": "Alat Untuk Menghasilkan Berbagai Bentuk Gelombang." },
    { "en": "Bentuk Gelombang Apa Saja Yang Dihasilkan?", "id": "Sinus, Kotak, Segitiga, Gigi Gergaji." },
    { "en": "Apa Itu Catu Daya Laboratorium?", "id": "Menyediakan Tegangan DC Variabel Yang Stabil." },
    { "en": "Fitur Penting Catu Daya Lab?", "id": "Pembatas Arus (Current Limiting)." },
    { "en": "Fungsi Pembatas Arus?", "id": "Melindungi Rangkaian Uji Dari Kerusakan." },
    { "en": "Apa Itu 'Prototyping'?", "id": "Proses Membuat Model Awal Sebuah Produk." },
    { "en": "Mengapa 'Prototyping' Penting?", "id": "Untuk Menguji Konsep Dan Menemukan Masalah." },
    { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Resmi Dari Pabrikan Komponen." },
    { "en": "Informasi Apa Yang Ada Di 'Datasheet'?", "id": "Peringkat Maksimum, Karakteristik, Contoh Aplikasi." },
    { "en": "Apa Itu 'Absolute Maximum Ratings'?", "id": "Batas Kondisi Yang Tidak Boleh Dilampaui." },
    { "en": "Masalah Jika Batas Ini Dilampaui?", "id": "Kerusakan Permanen Pada Komponen." },
    { "en": "Apa Itu 'Recommended Operating Conditions'?", "id": "Kondisi Di Mana Komponen Bekerja Optimal." },
    { "en": "Apa Itu 'Binning' Komponen?", "id": "Menyortir Komponen Berdasarkan Kinerja Aktualnya." },
    { "en": "Contoh 'Binning'?", "id": "Menyortir LED Berdasarkan Kecerahan Atau Warnanya." },
    { "en": "Mengapa Kadang Komponen Diberi Tanda Titik Warna?", "id": "Menandakan Toleransi Atau Kelompok Tertentu." },
    { "en": "Apa Itu Kode Warna Resistor?", "id": "Sistem Garis Warna Untuk Menandakan Nilai Resistansi." },
    { "en": "Bagaimana Menghafal Kode Warna?", "id": "Menggunakan Jembatan Keledai (Contoh: HiCoMeO)." },
    { "en": "Apa Itu Kode Angka Pada Kapasitor Keramik?", "id": "Menunjukkan Nilai Dalam Satuan Pikofarad (pF)." },
    { "en": "Arti Kode 104 Pada Kapasitor?", "id": "Satu Nol Diikuti Empat Nol (100,000 pF)." },
    { "en": "100,000 pF Sama Dengan Berapa?", "id": "Seratus Nanofarad (100 nF) Atau 0.1 µF." },
    { "en": "Apa Itu 'Footprint' Komponen?", "id": "Pola Kaki Komponen Pada Layout PCB." },
    { "en": "Masalah Jika 'Footprint' Salah?", "id": "Komponen Tidak Bisa Dipasang Di Papan." },
    { "en": "Apa Itu 'Library' Dalam Software EDA?", "id": "Kumpulan Simbol Skema Dan 'Footprint' Komponen." },
    { "en": "Apa Itu 'Design Rule Check' (DRC)?", "id": "Pemeriksaan Otomatis Terhadap Aturan Desain PCB." },
    { "en": "Aturan Apa Saja Yang Diperiksa?", "id": "Jarak Minimum Antar Jalur, Ukuran Lubang." },
    { "en": "Tujuan DRC?", "id": "Memastikan PCB Dapat Diproduksi Dengan Benar." },
    { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Lengkap Semua Komponen Dalam Desain." },
    { "en": "Informasi Apa Yang Ada Di BOM?", "id": "Nama Komponen, Nilai, Jumlah, Referensi." },
    { "en": "Mengapa BOM Penting Untuk Produksi?", "id": "Sebagai Dasar Untuk Pembelian Komponen." },
    { "en": "Apa Itu 'Fiducial Mark'?", "id": "Tanda Referensi Pada PCB Untuk Mesin Perakitan." },
    { "en": "Fungsi 'Fiducial Mark'?", "id": "Membantu Kamera Mesin Mengenali Orientasi Papan." },
    { "en": "Apa Itu 'Test Point'?", "id": "Titik Pada PCB Yang Dirancang Untuk Pengujian." },
    { "en": "Mengapa 'Test Point' Dibutuhkan?", "id": "Memudahkan Pengukuran Saat Produksi Atau Perbaikan." },
    { "en": "Apa Itu 'In-Circuit Testing' (ICT)?", "id": "Metode Pengujian Otomatis Pada Papan Jadi." },
    { "en": "Bagaimana Cara Kerja ICT?", "id": "Menggunakan Ranjang Paku (Bed of Nails) Untuk Mengakses Test Point." },
    { "en": "Apa Itu 'JTAG' (Joint Test Action Group)?", "id": "Standar Untuk Pengujian Dan Debugging Papan." },
    { "en": "Keuntungan JTAG?", "id": "Tidak Membutuhkan Banyak Test Point Fisik." },
    { "en": "Apa Itu 'Boundary Scan'?", "id": "Teknik Pengujian Menggunakan JTAG." },
    { "en": "Mengapa Ujung Probe Osiloskop Runcing?", "id": "Untuk Bisa Menyentuh Kaki IC Yang Kecil." },
    { "en": "Aksesori Apa Yang Ada Di Ujung Probe?", "id": "Klip Pengait, Klip Buaya, Jarum." },
    { "en": "Mengapa Kabel Probe Osiloskop Tebal?", "id": "Karena Merupakan Kabel Koaksial Terlindung." },
    { "en": "Tujuan Pelindung (Shield) Pada Kabel?", "id": "Mencegah Derau Mengganggu Sinyal Yang Diukur." },
    { "en": "Apa Itu 'Floating Measurement'?", "id": "Mengukur Tegangan Yang Tidak Mereferensi Ke Ground." },
    { "en": "Bahaya 'Floating Measurement' Dengan Osiloskop Biasa?", "id": "Dapat Menyebabkan Hubung Singkat Melalui Ground Probe." },
    { "en": "Solusi 'Floating Measurement'?", "id": "Gunakan Probe Diferensial Atau Osiloskop Terisolasi." },
    { "en": "Apa Itu Probe Diferensial?", "id": "Mengukur Selisih Tegangan Antara Dua Titik." },
    { "en": "Apa Itu 'Current Probe'?", "id": "Probe Khusus Untuk Mengukur Arus Tanpa Memutus Rangkaian." },
    { "en": "Bagaimana Cara Kerja 'Current Probe'?", "id": "Mendeteksi Medan Magnet Di Sekitar Kawat." },
    { "en": "Mengapa Penting Memeriksa Kembali Desain?", "id": "Kesalahan Kecil Bisa Membuat Proyek Gagal Total." },
    { "en": "Apa Itu 'Peer Review' Dalam Desain?", "id": "Meminta Rekan Kerja Untuk Memeriksa Desain Anda." },
    { "en": "Mengapa 'Peer Review' Bermanfaat?", "id": "Mata Yang Segar Seringkali Menemukan Kesalahan." },
    { "en": "Apa Itu 'Smoke Test'?", "id": "Tes Pertama Kali Menyalakan Rangkaian Baru." },
    { "en": "Mengapa Disebut 'Smoke Test'?", "id": "Melihat Apakah Ada Asap ('Magic Smoke') Keluar." },
    { "en": "Langkah Aman 'Smoke Test'?", "id": "Gunakan Catu Daya Dengan Pembatas Arus." },
    { "en": "Apa Itu 'Voltage Rail'?", "id": "Istilah Untuk Jalur Catu Daya (Vcc, Ground)." },
    { "en": "Apa Itu 'Power Integrity'?", "id": "Analisis Kualitas Distribusi Daya Pada PCB." },
    { "en": "Apa Itu 'Signal Integrity'?", "id": "Analisis Kualitas Sinyal Digital Pada PCB." },
    { "en": "Keduanya Penting Dalam Desain Apa?", "id": "Desain Berkecepatan Tinggi (High-Speed Design)." },
    { "en": "Apa Itu 'Termination' Jalur Transmisi?", "id": "Menambahkan Resistor Untuk Mencegah Refleksi." },
    { "en": "Nilai Resistor Terminasi Ditentukan Oleh Apa?", "id": "Impedansi Karakteristik Jalur." },
    { "en": "Masalah Jika Tidak Ada Terminasi?", "id": "Refleksi Menyebabkan 'Ringing' Dan Distorsi." },
    { "en": "Apa Itu 'Eye Diagram'?", "id": "Metode Visualisasi Kualitas Sinyal Digital." },
    { "en": "Bukaan 'Mata' Yang Lebar Berarti Apa?", "id": "Sinyal Bagus, Jitter Dan Derau Rendah." },
    { "en": "Bukaan 'Mata' Yang Tertutup Berarti Apa?", "id": "Sinyal Buruk, Tingkat Kesalahan Bit Tinggi." },
    { "en": "Apa Itu 'Bit Error Rate' (BER)?", "id": "Rasio Jumlah Bit Salah Terhadap Total Bit." },
    { "en": "Apa Itu 'Firmware Update'?", "id": "Memperbarui Perangkat Lunak Internal Sebuah Perangkat." },
    { "en": "Mengapa Perlu 'Firmware Update'?", "id": "Untuk Memperbaiki Bug Atau Menambah Fitur." },
    { "en": "Apa Itu 'Bootloader'?", "id": "Program Kecil Yang Berjalan Pertama Kali." },
    { "en": "Fungsi 'Bootloader'?", "id": "Memuat Sistem Operasi Atau Program Utama." },
    { "en": "Apa Itu 'JTAG' Port?", "id": "Port Untuk Debugging Dan Pemrograman Perangkat Keras." },
    { "en": "Apa Itu 'Daisy Chain'?", "id": "Menghubungkan Perangkat Secara Berantai." },
    { "en": "Contoh 'Daisy Chain' JTAG?", "id": "TDO Satu Chip Terhubung Ke TDI Chip Berikutnya." },
    { "en": "Apa Itu 'Socket' Untuk IC?", "id": "Dudukan Agar IC Bisa Dilepas Pasang." },
    { "en": "Kapan 'Socket' Digunakan?", "id": "Untuk Prototyping Atau IC Yang Mungkin Perlu Diganti." },
    { "en": "Kerugian 'Socket'?", "id": "Menambah Tinggi Dan Resistansi Kontak." },
    { "en": "Apa Itu 'Conduction'?", "id": "Perpindahan Panas Melalui Kontak Langsung." },
    { "en": "Apa Itu 'Convection'?", "id": "Perpindahan Panas Melalui Aliran Fluida (Udara)." },
    { "en": "Apa Itu 'Radiation'?", "id": "Perpindahan Panas Melalui Gelombang Elektromagnetik." },
    { "en": "Heatsink Bekerja Dengan Prinsip Apa?", "id": "Konduksi, Konveksi, Dan Radiasi." },
    { "en": "Mengapa Heatsink Diberi Sirip-sirip?", "id": "Untuk Memperluas Area Permukaan." },
    { "en": "Area Permukaan Lebih Luas Membantu Apa?", "id": "Meningkatkan Perpindahan Panas Konveksi." },
    { "en": "Mengapa Heatsink Sering Berwarna Hitam?", "id": "Warna Hitam Adalah Emitor Radiasi Yang Baik." },
    { "en": "Apa Itu 'Popcorn Noise'?", "id": "Derau Letupan Acak Pada Semikonduktor." },
    { "en": "Solusi Mengatasi 'Popcorn Noise'?", "id": "Gunakan Komponen Rendah Derau, Lakukan Seleksi." },
    { "en": "Mengapa Kapasitor Keramik Bisa Berderit?", "id": "Efek Piezoelektrik Akibat Tegangan AC." },
    { "en": "Solusi Kapasitor Berderit?", "id": "Gunakan Jenis Kapasitor Yang Berbeda." },
    { "en": "Apa Itu 'Inductive Kickback'?", "id": "Lonjakan Tegangan Saat Arus Induktor Diputus." },
    { "en": "Solusi 'Inductive Kickback'?", "id": "Gunakan Dioda Flyback Atau Rangkaian Snubber." },
    { "en": "Mengapa Output Op-Amp Terjebak Di Satu Sisi?", "id": "Masalah Umpan Balik Atau Kerusakan Internal." },
    { "en": "Solusi Output Op-Amp Terjebak?", "id": "Periksa Loop Umpan Balik, Ganti Op-Amp." },
    { "en": "Apa Itu 'Phase Margin' Yang Buruk?", "id": "Stabilitas Umpan Balik Yang Rendah." },
    { "en": "Masalah 'Phase Margin' Buruk?", "id": "Menyebabkan 'Ringing' Atau Osilasi." },
    { "en": "Solusi 'Phase Margin' Buruk?", "id": "Tambahkan Jaringan Kompensasi (Kapasitor)." },
    { "en": "Mengapa Regulator Tegangan Berosilasi?", "id": "Kapasitor Output Salah (Nilai Atau ESR)." },
    { "en": "Solusi Regulator Berosilasi?", "id": "Gunakan Jenis Dan Nilai Kapasitor Sesuai Datasheet." },
    { "en": "Apa Itu 'Motor Brushes'?", "id": "Sikat Karbon Untuk Menghantar Arus Ke Rotor." },
    { "en": "Masalah Jika Sikat Motor Aus?", "id": "Motor Kehilangan Tenaga Atau Berhenti Berputar." },
    { "en": "Solusi Sikat Aus?", "id": "Ganti Dengan Sikat Karbon Yang Baru." },
    { "en": "Mengapa Ada Derau Pada Pengukuran ADC?", "id": "Referensi Tegangan Berisik Atau Grounding Buruk." },
    { "en": "Solusi Derau ADC?", "id": "Gunakan Referensi Presisi, Pisahkan Ground Analog." },
    { "en": "Apa Itu 'Crosstalk' Pada ADC?", "id": "Sinyal Satu Kanal Bocor Ke Kanal Lain." },
    { "en": "Solusi 'Crosstalk' ADC?", "id": "Layout PCB Yang Baik, Impedansi Sumber Rendah." },
    { "en": "Mengapa Osilator Kristal Lambat 'Start-up'?", "id": "Penguatan Osilator Terlalu Rendah." },
    { "en": "Solusi Osilator Lambat 'Start-up'?", "id": "Gunakan Inverter Dengan Penguatan Lebih Tinggi." },
    { "en": "Apa Itu 'Glitches' Pada Output DAC?", "id": "Lonjakan Sesaat Saat Kode Input Berubah." },
    { "en": "Solusi 'Glitches' DAC?", "id": "Gunakan Rangkaian 'Deglitcher' (Sample-and-Hold)." },
    { "en": "Mengapa Koneksi Fiber Optik Gagal?", "id": "Ujung Fiber Kotor Atau Patah." },
    { "en": "Solusi Koneksi Fiber Optik Gagal?", "id": "Bersihkan Ujung Konektor Dengan Benar." },
    { "en": "Apa Itu 'Dark Current' Pada Sensor Gambar?", "id": "Arus Bocor Yang Muncul Tanpa Cahaya." },
    { "en": "'Dark Current' Meningkat Dengan Apa?", "id": "Suhu Dan Waktu Eksposur." },
    { "en": "Solusi 'Dark Current'?", "id": "Dinginkan Sensor, Kurangi Waktu Eksposur." },
    { "en": "Mengapa Tampilan VFD (Vacuum Fluorescent) Redup?", "id": "Penuaan Lapisan Fosfor Atau Filamen." },
    { "en": "Solusi Tampilan VFD Redup?", "id": "Naikkan Tegangan Filamen Sedikit (Riskan)." },
    { "en": "Apa Itu 'Memory Corruption'?", "id": "Data Dalam Memori Berubah Secara Tidak Sengaja." },
    { "en": "Penyebab 'Memory Corruption'?", "id": "Bug Perangkat Lunak Atau Kegagalan Perangkat Keras." },
    { "en": "Solusi 'Memory Corruption'?", "id": "Gunakan Kode Yang Aman, Lakukan Pengecekan Memori." },
    { "en": "Mengapa Relai Solid State (SSR) Gagal Mati?", "id": "Arus Bocor Atau Kerusakan TRIAC Internal." },
    { "en": "Solusi SSR Gagal Mati?", "id": "Tambahkan Snubber, Ganti SSR." },
    { "en": "Mengapa Sensor Efek Hall Tidak Bekerja?", "id": "Medan Magnet Terlalu Lemah Atau Polaritas Salah." },
    { "en": "Solusi Sensor Hall Gagal?", "id": "Gunakan Magnet Lebih Kuat, Periksa Orientasi." },
    { "en": "Apa Itu 'Input Bias Current' Op-Amp?", "id": "Arus DC Kecil Yang Dibutuhkan Input." },
    { "en": "Masalah 'Input Bias Current'?", "id": "Menyebabkan Jatuh Tegangan Pada Resistor Input." },
    { "en": "Solusi 'Input Bias Current'?", "id": "Gunakan Op-Amp FET-Input Atau Resistor Kompensasi." },
    { "en": "Apa Itu 'Input Offset Current'?", "id": "Selisih Antara Dua Arus Bias Input." },
    { "en": "Mengapa Saklar Membran Menjadi Tidak Responsif?", "id": "Lapisan Konduktif Aus Atau Teroksidasi." },
    { "en": "Solusi Saklar Membran Rusak?", "id": "Ganti Lapisan Saklar (Keypad)." },
    { "en": "Apa Itu 'ZIF Connector'?", "id": "Zero Insertion Force Connector." },
    { "en": "Masalah Umum 'ZIF Connector'?", "id": "Tuas Pengunci Patah Atau Kontak Tidak Pas." },
    { "en": "Solusi Masalah 'ZIF Connector'?", "id": "Pastikan Kabel Datar Terpasang Dengan Benar." },
    { "en": "Mengapa Ada Derau 'Shot Noise'?", "id": "Karena Sifat Diskrit Dari Pembawa Muatan." },
    { "en": "Derau 'Shot Noise' Sebanding Dengan Apa?", "id": "Akar Dari Arus DC." },
    { "en": "Solusi Mengurangi 'Shot Noise'?", "id": "Kurangi Arus Bias, Batasi Lebar Pita." },
    { "en": "Apa Itu Derau Termal (Johnson-Nyquist Noise)?", "id": "Derau Akibat Agitasi Termal Elektron." },
    { "en": "Derau Termal Sebanding Dengan Apa?", "id": "Akar Dari Resistansi Dan Suhu." },
    { "en": "Solusi Mengurangi Derau Termal?", "id": "Gunakan Resistor Nilai Rendah, Dinginkan Rangkaian." },
    { "en": "Mengapa Penguat RF Bisa Menginterferensi Dirinya Sendiri?", "id": "Kopling Antara Output Dan Input." },
    { "en": "Solusi Swainteferensi Penguat RF?", "id": "Gunakan Perisai Internal Dan Layout Yang Baik." },
    { "en": "Apa Itu 'Standing Wave' Pada Kabel?", "id": "Gelombang Stasioner Akibat Refleksi." },
    { "en": "Penyebab 'Standing Wave'?", "id": "Ketidakcocokan Impedansi." },
    { "en": "Solusi 'Standing Wave'?", "id": "Gunakan Terminator Atau Jaringan Pencocokan." },
    { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Tingkat Ketidakcocokan Impedansi." },
    { "en": "Mengapa Baterai Isi Ulang Kehilangan Kapasitas?", "id": "Degradasi Kimia Internal Seiring Siklus." },
    { "en": "Solusi Baterai Kehilangan Kapasitas?", "id": "Ganti Baterai Setelah Mencapai Batas Siklusnya." },
    { "en": "Apa Itu 'Self-Discharge'?", "id": "Baterai Kehilangan Muatan Meskipun Tidak Digunakan." },
    { "en": "Baterai Mana Yang Punya 'Self-Discharge' Rendah?", "id": "Baterai Lithium." },
    { "en": "Apa Itu 'Golden Sample'?", "id": "Unit Referensi Yang Diketahui Bekerja Sempurna." },
    { "en": "Kegunaan 'Golden Sample'?", "id": "Sebagai Pembanding Saat Melakukan 'Troubleshooting'." },
    { "en": "Mengapa Komponen Elektronik Memiliki Umur Pakai?", "id": "Karena Degradasi Fisik Dan Kimia." },
    { "en": "Komponen Apa Yang Paling Cepat Rusak?", "id": "Kapasitor Elektrolit Dan Kipas Pendingin." },
    { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-rata Yang Diharapkan Antar Kegagalan." },
    { "en": "MTBF Tinggi Berarti Produk Andal?", "id": "Ya, Benar." },
    { "en": "Mengapa Mesin Cuci Gagal Berputar?", "id": "Sikat Motor Aus Atau Kapasitor Start Rusak." },
    { "en": "Solusi Mesin Cuci Gagal Berputar?", "id": "Ganti Sikat Motor Atau Kapasitor." },
    { "en": "Apa Itu Kapasitor Start Motor?", "id": "Memberikan Torsi Awal Untuk Motor AC." },
    { "en": "Mengapa Kulkas Tidak Dingin?", "id": "Kompresor Gagal Start Atau Kehabisan Refrigeran." },
    { "en": "Solusi Kulkas Tidak Dingin?", "id": "Periksa Relay Starter Kompresor." },
    { "en": "Mengapa Mikrowave Tidak Panas?", "id": "Magnetron Rusak Atau Dioda Tegangan Tinggi Gagal." },
    { "en": "Bahaya Memperbaiki Mikrowave?", "id": "Kapasitor Tegangan Sangat Tinggi Yang Mematikan." },
    { "en": "Solusi Aman Memperbaiki Mikrowave?", "id": "Pastikan Kapasitor Dikosongkan Terlebih Dahulu." },
    { "en": "Apa Itu 'Flyback Transformer'?", "id": "Trafo Khusus Di TV CRT Dan Monitor." },
    { "en": "Fungsi 'Flyback Transformer'?", "id": "Menghasilkan Tegangan Sangat Tinggi Untuk Tabung." },
    { "en": "Ciri Kerusakan 'Flyback'?", "id": "Suara Berdesis Atau Bau Ozon." },
    { "en": "Mengapa Ada Peringatan Tegangan Tinggi?", "id": "Karena Sentuhan Bisa Berakibat Fatal." },
    { "en": "Solusi Saat Bekerja Dengan Tegangan Tinggi?", "id": "Gunakan Trafo Isolasi Dan Satu Tangan." },
    { "en": "Mengapa Menggunakan Satu Tangan?", "id": "Mencegah Arus Mengalir Melewati Jantung." },
    { "en": "Apa Itu 'Ground Fault'?", "id": "Ketika Arus Mengalir Langsung Ke Tanah." },
    { "en": "Bahaya 'Ground Fault'?", "id": "Risiko Sengatan Listrik Yang Parah." },
    { "en": "Alat Apa Yang Melindungi Dari 'Ground Fault'?", "id": "GFCI (Ground Fault Circuit Interrupter)." },
    { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Semua Komponen Untuk Satu Proyek." },
    { "en": "Masalah Jika BOM Salah?", "id": "Pembelian Komponen Salah, Produksi Tertunda." },
    { "en": "Solusi Kesalahan BOM?", "id": "Lakukan Verifikasi Ganda Sebelum Memesan." },
    { "en": "Apa Itu 'Component Footprint'?", "id": "Pola Kaki Komponen Pada Desain PCB." },
    { "en": "Masalah Jika 'Footprint' Salah?", "id": "Komponen Fisik Tidak Bisa Dipasang Di Papan." },
    { "en": "Solusi 'Footprint' Salah?", "id": "Selalu Periksa 'Footprint' Dengan 'Datasheet'." },
    { "en": "Apa Itu 'Design for Manufacturability' (DFM)?", "id": "Mendesain Produk Agar Mudah Diproduksi." },
    { "en": "Mengapa DFM Penting?", "id": "Mengurangi Biaya Dan Meningkatkan Hasil Produksi." },
    { "en": "Contoh Aturan DFM?", "id": "Gunakan Komponen Umum, Jaga Jarak Cukup." },
    { "en": "Apa Itu 'Design for Testability' (DFT)?", "id": "Mendesain Produk Agar Mudah Diuji." },
    { "en": "Mengapa DFT Penting?", "id": "Mempercepat Pengujian Produksi Dan Diagnostik." },
    { "en": "Contoh Aturan DFT?", "id": "Menambahkan 'Test Point' Di Titik-titik Kunci." },
    { "en": "Apa Itu 'Panelization'?", "id": "Mengatur Beberapa PCB Kecil Dalam Satu Panel." },
    { "en": "Tujuan 'Panelization'?", "id": "Mempercepat Proses Perakitan Otomatis." },
    { "en": "Bagaimana PCB Dipisahkan Dari Panel?", "id": "Dengan V-Score Atau Tab-Routing." },
    { "en": "Apa Itu 'V-Score'?", "id": "Alur Berbentuk V Untuk Memudahkan Pematahan." },
    { "en": "Apa Itu Pasta Solder?", "id": "Campuran Bola Timah Kecil Dan Fluks." },
    { "en": "Untuk Apa Pasta Solder Digunakan?", "id": "Untuk Perakitan Komponen SMD." },
    { "en": "Bagaimana Pasta Solder Diterapkan?", "id": "Menggunakan Stensil Logam." },
    { "en": "Apa Itu Oven 'Reflow'?", "id": "Oven Untuk Melelehkan Pasta Solder." },
    { "en": "Apa Itu Profil Suhu 'Reflow'?", "id": "Grafik Suhu Terhadap Waktu Selama Proses." },
    { "en": "Mengapa Profil Suhu Penting?", "id": "Pemanasan Yang Salah Bisa Merusak Komponen." },
    { "en": "Apa Itu 'Wave Soldering'?", "id": "Metode Solder Untuk Komponen 'Through-Hole'." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Melewatkan Bagian Bawah PCB Di Atas Gelombang Solder." },
    { "en": "Masalah Umum 'Wave Soldering'?", "id": "'Solder Bridge' Dan Solderan Tidak Sempurna." },
    { "en": "Apa Itu 'Selective Soldering'?", "id": "Menyolder Hanya Titik Tertentu Secara Otomatis." },
    { "en": "Apa Itu 'In-Circuit Test' (ICT)?", "id": "Pengujian Otomatis PCB Setelah Perakitan." },
    { "en": "Apa Itu 'Flying Probe Tester'?", "id": "Penguji PCB Tanpa Memerlukan Ranjang Paku." },
    { "en": "Keuntungan 'Flying Probe'?", "id": "Fleksibel, Tidak Perlu 'Fixture' Khusus." },
    { "en": "Kerugian 'Flying Probe'?", "id": "Lebih Lambat Dari ICT." },
    { "en": "Apa Itu 'Functional Test'?", "id": "Menguji Fungsi Papan Seolah-olah Digunakan." },
    { "en": "Mana Yang Dilakukan Lebih Dulu, ICT Atau Fungsional?", "id": "Biasanya ICT Terlebih Dahulu." },
    { "en": "Apa Itu 'First Pass Yield'?", "id": "Persentase Produk Yang Lulus Uji Pertama." },
    { "en": "First Pass Yield' Tinggi Menandakan Apa?", "id": "Proses Manufaktur Yang Baik." },
    { "en": "Apa Itu 'Rework'?", "id": "Proses Memperbaiki Papan Yang Gagal Uji." },
    { "en": "Apakah 'Rework' Diinginkan?", "id": "Tidak, Karena Menambah Biaya Dan Waktu." },
    { "en": "Mengapa Diperlukan Kontrol Versi (Version Control)?", "id": "Untuk Melacak Perubahan Pada Desain." },
    { "en": "Contoh Sistem Kontrol Versi?", "id": "Git." },
    { "en": "Apa Itu 'Engineering Change Order' (ECO)?", "id": "Dokumen Resmi Untuk Mengubah Desain." },
    { "en": "Mengapa ECO Diperlukan?", "id": "Untuk Memastikan Perubahan Terkendali Dan Terdokumentasi." },
    { "en": "Apa Itu 'End-of-Life' (EOL) Komponen?", "id": "Ketika Pabrikan Berhenti Memproduksi Komponen." },
    { "en": "Masalah Akibat EOL?", "id": "Memaksa Desain Ulang Produk." },
    { "en": "Solusi Mengatasi EOL?", "id": "Pilih Komponen Dengan Siklus Hidup Panjang." },
    { "en": "Apa Itu 'Component Lead Time'?", "id": "Waktu Tunggu Dari Pemesanan Hingga Pengiriman." },
    { "en": "Masalah 'Lead Time' Panjang?", "id": "Dapat Menunda Jadwal Produksi." },
    { "en": "Apa Itu 'Consigned Inventory'?", "id": "Komponen Dimiliki Pelanggan, Dirakit Oleh Kontraktor." },
    { "en": "Apa Itu 'Turnkey' Assembly?", "id": "Kontraktor Mengurus Pembelian Komponen Dan Perakitan." },
    { "en": "Apa Itu 'Gerber File'?", "id": "Format Standar Untuk Data Manufaktur PCB." },
    { "en": "Apa Yang Direpresentasikan 'Gerber File'?", "id": "Setiap Lapisan Tembaga, Solder Mask, Silkscreen." },
    { "en": "Apa Itu 'Drill File'?", "id": "File Yang Menentukan Posisi Dan Ukuran Lubang." },
    { "en": "Apa Itu 'Centroid File'?", "id": "Menentukan Posisi Dan Orientasi Komponen SMD." },
    { "en": "Untuk Mesin Apa 'Centroid File' Digunakan?", "id": "Mesin 'Pick and Place'." },
    { "en": "Apa Itu 'Solder Paste Inspection' (SPI)?", "id": "Pemeriksaan 3D Untuk Memastikan Volume Pasta Solder." },
    { "en": "Apa Itu 'Automated Optical Inspection' (AOI)?", "id": "Pemeriksaan Visual Otomatis Setelah Perakitan." },
    { "en": "Apa Yang Diperiksa AOI?", "id": "Kehadiran Komponen, Polaritas, Kualitas Solderan." },
    { "en": "Apa Itu 'X-Ray Inspection'?", "id": "Pemeriksaan Untuk Melihat Solderan Di Bawah Komponen." },
    { "en": "Untuk Komponen Apa 'X-Ray' Penting?", "id": "BGA (Ball Grid Array)." },
    { "en": "Apa Itu BGA?", "id": "IC Dengan Bola Solder Di Bagian Bawah." },
    { "en": "Masalah Menyolder BGA?", "id": "Sambungan Tidak Terlihat Secara Visual." },
    { "en": "Solusi Perbaikan BGA?", "id": "Memerlukan Stasiun 'Rework' BGA Khusus." },
    { "en": "Apa Itu 'Underfill'?", "id": "Epoksi Yang Diinjeksikan Di Bawah Chip." },
    { "en": "Tujuan 'Underfill'?", "id": "Meningkatkan Kekuatan Mekanis Dan Keandalan." },
    { "en": "Apa Itu 'Potting'?", "id": "Menyelubungi Seluruh Rangkaian Dengan Resin." },
    { "en": "Tujuan 'Potting'?", "id": "Perlindungan Terhadap Lingkungan Dan Getaran." },
    { "en": "Apa Itu 'Conformal Coating'?", "id": "Lapisan Pelindung Tipis Pada PCB." },
    { "en": "Apa Itu IPC?", "id": "Asosiasi Industri Elektronik Yang Menetapkan Standar." },
    { "en": "Contoh Standar IPC?", "id": "Standar Untuk Desain Dan Perakitan PCB." },
    { "en": "Apa Itu Kelas IPC (Kelas 1, 2, 3)?", "id": "Menentukan Tingkat Kualitas Dan Keandalan." },
    { "en": "Kelas Mana Yang Paling Ketat?", "id": "Kelas 3 (Contoh: Dirgantara, Medis)." },
    { "en": "Apa Itu 'Known Good Board'?", "id": "Papan Referensi Yang Digunakan Untuk Pengujian." },
    { "en": "Apa Itu 'Clamshell' Tester?", "id": "Penguji ICT Yang Mengakses Kedua Sisi Papan." },
    { "en": "Apa Itu 'Cable Harness'?", "id": "Kumpulan Kabel Yang Diikat Menjadi Satu." },
    { "en": "Mengapa Kabel Dibuat 'Harness'?", "id": "Agar Rapi Dan Mudah Dipasang." },
    { "en": "Apa Itu 'Continuity Test'?", "id": "Memeriksa Apakah Ada Sambungan Listrik." },
    { "en": "Apa Itu 'Hi-Pot Test'?", "id": "Tes Tegangan Tinggi Untuk Memeriksa Isolasi." },
    { "en": "Tujuan 'Hi-Pot Test'?", "id": "Memastikan Keamanan Produk Terhadap Kebocoran Arus." },
    { "en": "Apa Itu 'Burn-in Test'?", "id": "Mengoperasikan Produk Pada Kondisi Ekstrem." },
    { "en": "Tujuan 'Burn-in Test'?", "id": "Mendeteksi Kegagalan Dini ('Infant Mortality')." },
    { "en": "Apa Itu Kurva 'Bathtub'?", "id": "Grafik Tingkat Kegagalan Terhadap Waktu." },
    { "en": "Tiga Fase Kurva 'Bathtub'?", "id": "Kegagalan Dini, Umur Pakai, Aus." },
    { "en": "Apa Itu 'Material Safety Data Sheet' (MSDS)?", "id": "Dokumen Informasi Keamanan Bahan Kimia." },
    { "en": "Kapan MSDS Diperlukan?", "id": "Saat Bekerja Dengan Bahan Kimia Seperti Fluks." },
    { "en": "Apa Itu 'Clean Room'?", "id": "Ruangan Sangat Bersih Untuk Manufaktur Sensitif." },
    { "en": "Untuk Apa 'Clean Room' Digunakan?", "id": "Manufaktur Semikonduktor (IC) Dan Hard Drive." },
    { "en": "Mengapa Kebersihan Sangat Penting?", "id": "Partikel Debu Kecil Bisa Merusak Chip." },
    { "en": "Apa Itu 'Wafer' Silikon?", "id": "Kepingan Silikon Tipis, Dasar Dari IC." },
    { "en": "Apa Itu 'Die'?", "id": "Satu Potongan Chip Individual Dari 'Wafer'." },
    { "en": "Apa Itu 'Wire Bonding'?", "id": "Menghubungkan 'Die' Ke Kaki IC." },
    { "en": "Kawat Apa Yang Digunakan?", "id": "Kawat Emas Yang Sangat Tipis." },
    { "en": "Apa Itu 'Encapsulation'?", "id": "Proses Membungkus 'Die' Dengan Plastik Atau Keramik." },
    { "en": "Tujuan 'Encapsulation'?", "id": "Melindungi Chip Secara Fisik Dan Lingkungan." },
    { "en": "Apa Itu 'Yield' Dalam Manufaktur?", "id": "Persentase Produk Bagus Dari Total Produksi." },
    { "en": "'Yield' Tinggi Berarti Proses Efisien?", "id": "Ya, Sangat Efisien." },
    { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Masalah: Program Gagal Diunggah?", "id": "Port COM Salah Atau Driver Tidak Ada." },
    { "en": "Solusi Gagal Unggah?", "id": "Periksa Port Di Device Manager, Instal Driver." },
    { "en": "Apa Itu 'Watchdog Timer'?", "id": "Timer Yang Mereset Sistem Jika Macet." },
    { "en": "Masalah: Sistem Sering Reset Sendiri?", "id": "Mungkin 'Watchdog' Tidak Direset Dengan Benar." },
    { "en": "Solusi Sistem Sering Reset?", "id": "Pastikan Loop Utama Mereset 'Watchdog'." },
    { "en": "Apa Itu 'Interrupt'?", "id": "Sinyal Yang Menghentikan Program Utama Sementara." },
    { "en": "Masalah: 'Interrupt' Tidak Bekerja?", "id": "'Interrupt' Tidak Diaktifkan Secara Global." },
    { "en": "Solusi 'Interrupt' Gagal?", "id": "Panggil Fungsi Untuk Mengaktifkan 'Global Interrupt'." },
    { "en": "Apa Itu 'Race Condition' Dalam Perangkat Lunak?", "id": "Hasil Bergantung Pada Urutan Eksekusi Yang Tak Tentu." },
    { "en": "Solusi 'Race Condition'?", "id": "Gunakan 'Semaphore' Atau Nonaktifkan 'Interrupt' Sementara." },
    { "en": "Apa Itu 'Buffer Overflow'?", "id": "Menulis Data Melebihi Kapasitas Buffer." },
    { "en": "Masalah 'Buffer Overflow'?", "id": "Merusak Memori, Menyebabkan 'Crash' Atau Kerentanan." },
    { "en": "Solusi 'Buffer Overflow'?", "id": "Selalu Periksa Batas Ukuran Data." },
    { "en": "Mengapa Pin Digital Tidak Memberi Output?", "id": "Pin Belum Dikonfigurasi Sebagai Output." },
    { "en": "Solusi Pin Tidak Bekerja?", "id": "Set Arah Pin (Pin Mode) Sebagai Output." },
    { "en": "Apa Itu 'Debouncing' Perangkat Lunak?", "id": "Mengabaikan Perubahan Input Untuk Waktu Singkat." },
    { "en": "Tujuan 'Debouncing' Perangkat Lunak?", "id": "Mengatasi Pantulan Saklar Tanpa Perangkat Keras." },
    { "en": "Mengapa ADC Memberi Nilai Acak?", "id": "Pin Input Analog Mengambang (Floating)." },
    { "en": "Solusi ADC Acak?", "id": "Hubungkan Pin Ke Sinyal Atau Ground." },
    { "en": "Apa Itu 'Stack Overflow'?", "id": "Memori Stack Habis." },
    { "en": "Penyebab 'Stack Overflow'?", "id": "Rekursi Tak Terbatas Atau Variabel Lokal Besar." },
    { "en": "Solusi 'Stack Overflow'?", "id": "Hindari Rekursi Dalam, Optimalkan Penggunaan Memori." },
    { "en": "Mengapa Program Berjalan Lambat?", "id": "Penggunaan Fungsi 'Delay' Yang Berlebihan." },
    { "en": "Solusi Program Lambat?", "id": "Gunakan Pendekatan Non-blocking (Cek Waktu millis())." },
    { "en": "Apa Itu 'Volatile' Keyword?", "id": "Memberi Tahu Compiler Variabel Bisa Berubah Tiba-tiba." },
    { "en": "Kapan 'Volatile' Digunakan?", "id": "Pada Variabel Yang Digunakan Dalam ISR." },
    { "en": "Apa Itu ISR (Interrupt Service Routine)?", "id": "Fungsi Yang Dijalankan Saat Terjadi 'Interrupt'." },
    { "en": "Aturan Penting Untuk ISR?", "id": "Harus Dibuat Sangat Cepat Dan Singkat." },
    { "en": "Apa Itu 'Deadlock'?", "id": "Dua Proses Saling Menunggu Sumber Daya." },
    { "en": "Solusi 'Deadlock'?", "id": "Gunakan Hierarki Penguncian Sumber Daya." },
    { "en": "Mengapa Komunikasi Serial Gagal?", "id": "Baud Rate Tidak Cocok." },
    { "en": "Solusi Komunikasi Serial Gagal?", "id": "Pastikan Kedua Perangkat Menggunakan Baud Rate Sama." },
    { "en": "Apa Itu 'Framing Error'?", "id": "Kesalahan Sinkronisasi Dalam Komunikasi Serial." },
    { "en": "Apa Itu 'Endianness'?", "id": "Urutan Byte Dalam Representasi Data Multi-byte." },
    { "en": "Masalah 'Endianness'?", "id": "Sistem Berbeda Mungkin Menafsirkan Data Secara Salah." },
    { "en": "Solusi 'Endianness'?", "id": "Gunakan Format Data Standar Jaringan." },
    { "en": "Mengapa Perlu 'Pull-up' Untuk I2C?", "id": "Karena Bus I2C Berbasis 'Open-Drain'." },
    { "en": "Apa Itu 'Open-Drain'?", "id": "Output Hanya Bisa Menarik Ke Ground." },
    { "en": "Masalah: Perangkat I2C Tidak Terdeteksi?", "id": "Alamat Salah Atau Resistor 'Pull-up' Hilang." },
    { "en": "Solusi Perangkat I2C Gagal?", "id": "Jalankan I2C Scanner Untuk Mencari Alamat." },
    { "en": "Apa Itu 'Floating Point Arithmetic'?", "id": "Perhitungan Menggunakan Bilangan Pecahan." },
    { "en": "Masalah 'Floating Point' Di Mikrokontroler?", "id": "Sangat Lambat Dan Memakan Banyak Memori." },
    { "en": "Solusi 'Floating Point'?", "id": "Gunakan Aritmetika Bilangan Bulat Jika Mungkin." },
    { "en": "Apa Itu 'Fixed-Point Arithmetic'?", "id": "Simulasi Bilangan Pecahan Menggunakan Bilangan Bulat." },
    { "en": "Mengapa Program 'Hang'?", "id": "Terjebak Dalam Loop Tak Terbatas ('Infinite Loop')." },
    { "en": "Solusi Program 'Hang'?", "id": "Gunakan 'Debugger' Atau Cetak Status Untuk Melacak." },
    { "en": "Apa Itu 'Memory Leak'?", "id": "Alokasi Memori Yang Tidak Pernah Dibebaskan." },
    { "en": "Masalah 'Memory Leak'?", "id": "Sistem Akan Kehabisan Memori Dan 'Crash'." },
    { "en": "Solusi 'Memory Leak'?", "id": "Pastikan Setiap 'malloc' Memiliki 'free' Yang Sesuai." },
    { "en": "Apa Itu 'Pointer'?", "id": "Variabel Yang Menyimpan Alamat Memori." },
    { "en": "Masalah Umum 'Pointer'?", "id": "'Null Pointer Dereference' (Mengakses Alamat Nol)." },
    { "en": "Solusi Masalah 'Pointer'?", "id": "Selalu Periksa Apakah 'Pointer' Bukan NULL." },
    { "en": "Apa Itu 'Hard Fault'?", "id": "Jenis 'Exception' Fatal Pada Prosesor ARM." },
    { "en": "Penyebab 'Hard Fault'?", "id": "Akses Memori Ilegal, Pembagian Dengan Nol." },
    { "en": "Apa Itu 'Real-Time Operating System' (RTOS)?", "id": "Sistem Operasi Dengan Perilaku Waktu Deterministik." },
    { "en": "Masalah Dalam RTOS?", "id": "'Priority Inversion', 'Deadlock'." },
    { "en": "Apa Itu 'Priority Inversion'?", "id": "Tugas Prioritas Tinggi Tertunda Oleh Tugas Rendah." },
    { "en": "Solusi 'Priority Inversion'?", "id": "Gunakan 'Mutex' Dengan 'Priority Inheritance'." },
    { "en": "Mengapa Menulis Ke Flash Memori Lambat?", "id": "Proses Hapus Dan Tulis Secara Fisik Lambat." },
    { "en": "Apa Itu 'Wear Leveling'?", "id": "Teknik Untuk Meratakan Penggunaan Blok Flash." },
    { "en": "Tujuan 'Wear Leveling'?", "id": "Memperpanjang Umur Pakai Flash Memori." },
    { "en": "Mengapa Program Berbeda Setelah Kompilasi Ulang?", "id": "Variabel Belum Diinisialisasi Mendapat Nilai Acak." },
    { "en": "Solusinya?", "id": "Selalu Inisialisasi Variabel Sebelum Digunakan." },
    { "en": "Apa Itu 'Off-by-One Error'?", "id": "Kesalahan Umum Dalam Loop (Iterasi Kurang/Lebih)." },
    { "en": "Apa Itu 'Logic Bomb'?", "id": "Kode Jahat Yang Aktif Pada Kondisi Tertentu." },
    { "en": "Apa Itu 'Backdoor'?", "id": "Jalan Masuk Rahasia Ke Sistem." },
    { "en": "Apa Itu 'Static Analysis'?", "id": "Menganalisis Kode Tanpa Menjalankannya." },
    { "en": "Tujuan 'Static Analysis'?", "id": "Menemukan Potensi Bug Dan Kerentanan." },
    { "en": "Apa Itu 'Unit Testing'?", "id": "Menguji Bagian-bagian Kecil Kode Secara Terisolasi." },
    { "en": "Mengapa 'Unit Testing' Penting?", "id": "Memastikan Setiap Komponen Bekerja Dengan Benar." },
    { "en": "Apa Itu 'Code Review'?", "id": "Meminta Orang Lain Memeriksa Kode Anda." },
    { "en": "Manfaat 'Code Review'?", "id": "Menemukan Kesalahan, Meningkatkan Kualitas Kode." },
    { "en": "Mengapa Kode Perlu Diberi Komentar?", "id": "Untuk Menjelaskan Logika Yang Rumit." },
    { "en": "Apa Itu 'Magic Number'?", "id": "Angka Tanpa Penjelasan Dalam Kode." },
    { "en": "Masalah 'Magic Number'?", "id": "Membuat Kode Sulit Dibaca Dan Dipelihara." },
    { "en": "Solusinya?", "id": "Gunakan Konstanta Dengan Nama Yang Jelas." },
    { "en": "Apa Itu 'Spaghetti Code'?", "id": "Kode Yang Alurnya Sangat Rumit Dan Sulit Diikuti." },
    { "en": "Solusi 'Spaghetti Code'?", "id": "Lakukan 'Refactoring' Menjadi Fungsi Yang Lebih Kecil." },
    { "en": "Apa Itu 'Refactoring'?", "id": "Memperbaiki Struktur Kode Tanpa Mengubah Fungsinya." },
    { "en": "Mengapa Perlu Manajemen Konfigurasi?", "id": "Untuk Melacak Versi Perangkat Keras Dan Lunak." },
    { "en": "Apa Itu 'Brownout Detector' (BOD)?", "id": "Sirkuit Yang Mereset CPU Jika Tegangan Turun." },
    { "en": "Tujuan BOD?", "id": "Mencegah Perilaku Aneh Saat Tegangan Tidak Stabil." },
    { "en": "Mengapa Mikrokontroler Perlu Kristal Eksternal?", "id": "Untuk Operasi Clock Yang Lebih Akurat." },
    { "en": "Osilator Internal Mikrokontroler Kurang Akurat?", "id": "Ya, Dan Sensitif Terhadap Suhu." },
    { "en": "Apa Masalah Umum Catu Daya?", "id": "Tidak Ada Tegangan Output Sama Sekali." },
    { "en": "Solusi Pertama Catu Daya Mati?", "id": "Periksa Sekring Dan Kabel Input." },
    { "en": "Apa Itu Kapasitor Filter Utama?", "id": "Kapasitor Elektrolit Besar Setelah Penyearah." },
    { "en": "Masalah Jika Kapasitor Ini Kering?", "id": "Tegangan Output Turun Dan 'Ripple' Tinggi." },
    { "en": "Solusi Kapasitor Kering?", "id": "Ganti Dengan Kapasitor Baru Yang Sesuai." },
    { "en": "Apa Itu Penyearah Jembatan (Bridge Rectifier)?", "id": "Empat Dioda Untuk Mengubah AC Ke DC." },
    { "en": "Masalah Umum Penyearah Jembatan?", "id": "Salah Satu Dioda Terbuka Atau Terhubung Singkat." },
    { "en": "Solusinya?", "id": "Ganti Seluruh Jembatan Atau Dioda Rusak." },
    { "en": "Apa Itu 'Switching Mode Power Supply' (SMPS)?", "id": "Catu Daya Efisiensi Tinggi." },
    { "en": "Masalah Umum SMPS?", "id": "Transistor Switching Gagal Atau Kapasitor Rusak." },
    { "en": "Mengapa Memperbaiki SMPS Berbahaya?", "id": "Ada Tegangan Tinggi Di Sisi Primer." },
    { "en": "Apa Itu Sisi Primer Dan Sekunder?", "id": "Primer Terhubung Jala-jala, Sekunder Terisolasi." },
    { "en": "Bagaimana Isolasi Disediakan?", "id": "Menggunakan Transformator Frekuensi Tinggi." },
    { "en": "Mengapa Transistor Switching SMPS Sering Gagal?", "id": "Karena Bekerja Pada Tegangan Dan Arus Tinggi." },
    { "en": "Apa Itu Dioda 'Snubber'?", "id": "Dioda Untuk Meredam Lonjakan Tegangan." },
    { "en": "Di Mana Dioda 'Snubber' Digunakan?", "id": "Melintasi Transistor Switching." },
    { "en": "Apa Itu Umpan Balik (Feedback) Dalam SMPS?", "id": "Mengatur Tegangan Output Agar Tetap Stabil." },
    { "en": "Komponen Apa Yang Digunakan Untuk Umpan Balik?", "id": "Optocoupler Untuk Menjaga Isolasi." },
    { "en": "Masalah Jika Optocoupler Rusak?", "id": "Tegangan Output Menjadi Tidak Teratur." },
    { "en": "Apa Itu 'Inrush Current Limiter'?", "id": "Membatasi Arus Awal Saat SMPS Dinyalakan." },
    { "en": "Komponen Apa Yang Digunakan?", "id": "Termistor NTC." },
    { "en": "Mengapa SMPS Mengeluarkan Suara Berdesis?", "id": "Bekerja Dalam Mode 'Burst' Saat Beban Rendah." },
    { "en": "Apakah Desisan Itu Normal?", "id": "Seringkali Normal Untuk Beberapa Desain." },
    { "en": "Apa Itu Regulator Linear?", "id": "Regulator Yang Bekerja Seperti Resistor Variabel." },
    { "en": "Contoh IC Regulator Linear?", "id": "LM7805 (Untuk +5V) Dan LM7905 (Untuk -5V)." },
    { "en": "Masalah: Regulator 7805 Sangat Panas?", "id": "Perbedaan Tegangan Input Dan Output Terlalu Besar." },
    { "en": "Solusi Regulator Panas?", "id": "Gunakan Heatsink Atau Turunkan Tegangan Input." },
    { "en": "Apa Itu 'Dropout Voltage'?", "id": "Perbedaan Minimum Tegangan Input-Output." },
    { "en": "Apa Itu Regulator LDO (Low-Dropout)?", "id": "Regulator Dengan 'Dropout Voltage' Sangat Rendah." },
    { "en": "Kapan LDO Dibutuhkan?", "id": "Ketika Tegangan Input Sangat Dekat Dengan Output." },
    { "en": "Mengapa Perlu Kapasitor Pada Input Dan Output Regulator?", "id": "Untuk Stabilitas Dan Memfilter Derau." },
    { "en": "Masalah Jika Kapasitor Ini Hilang?", "id": "Regulator Bisa Berosilasi." },
    { "en": "Apa Itu Referensi Tegangan (Voltage Reference)?", "id": "Sirkuit Yang Menghasilkan Tegangan Sangat Stabil." },
    { "en": "Di Mana Referensi Tegangan Digunakan?", "id": "Dalam ADC, DAC, Dan Catu Daya Presisi." },
    { "en": "Apa Itu 'Crowbar Circuit'?", "id": "Sirkuit Proteksi Tegangan Berlebih." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Membuat Hubung Singkat Disengaja Untuk Membakar Sekring." },
    { "en": "Komponen Utama 'Crowbar'?", "id": "SCR (Silicon-Controlled Rectifier)." },
    { "en": "Apa Itu Proteksi Polaritas Terbalik?", "id": "Mencegah Kerusakan Jika Catu Daya Terhubung Terbalik." },
    { "en": "Cara Paling Sederhana?", "id": "Pasang Dioda Seri Di Jalur Input." },
    { "en": "Apa Kerugian Dioda Seri?", "id": "Menyebabkan Jatuh Tegangan Sekitar 0.7V." },
    { "en": "Apa Itu 'Polyfuse' atau PTC Resettable Fuse?", "id": "Sekring Yang Bisa Pulih Kembali." },
    { "en": "Bagaimana Cara Kerja 'Polyfuse'?", "id": "Resistansinya Meningkat Drastis Saat Arus Berlebih." },
    { "en": "Apa Itu 'Ground Bounce'?", "id": "Fluktuasi Tegangan Pada Jalur Ground." },
    { "en": "Penyebab 'Ground Bounce'?", "id": "Induktansi Jalur Ground Dan Arus Switching Cepat." },
    { "en": "Solusi 'Ground Bounce'?", "id": "Gunakan 'Ground Plane' Yang Solid." },
    { "en": "Apa Itu 'Power Plane'?", "id": "Lapisan Tembaga Khusus Untuk Distribusi Vcc." },
    { "en": "Mengapa Tegangan Baterai Diukur Tanpa Beban?", "id": "Untuk Mengetahui Tegangan Rangkaian Terbukanya." },
    { "en": "Apakah Pengukuran Ini Cukup?", "id": "Tidak, Baterai Rusak Bisa Menunjukkan Tegangan Normal." },
    { "en": "Tes Baterai Yang Baik Bagaimana?", "id": "Ukur Tegangan Saat Diberi Beban." },
    { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Cadangan Menggunakan Baterai." },
    { "en": "Masalah Umum UPS?", "id": "Baterai Perlu Diganti Setiap Beberapa Tahun." },
    { "en": "Apa Itu UPS 'Online'?", "id": "Selalu Memberi Daya Dari Baterai (Terisolasi Penuh)." },
    { "en": "Apa Itu UPS 'Offline'?", "id": "Beralih Ke Baterai Hanya Saat Listrik Padam." },
    { "en": "UPS Mana Yang Memberi Proteksi Lebih Baik?", "id": "UPS 'Online'." },
    { "en": "Apa Itu Inverter?", "id": "Mengubah Tegangan DC Menjadi Tegangan AC." },
    { "en": "Bentuk Gelombang Output Inverter Murah?", "id": "Kotak Termodifikasi (Modified Sine Wave)." },
    { "en": "Bentuk Gelombang Output Inverter Mahal?", "id": "Sinus Murni (Pure Sine Wave)." },
    { "en": "Peralatan Apa Yang Membutuhkan Sinus Murni?", "id": "Motor AC, Peralatan Audio, Peralatan Medis." },
    { "en": "Mengapa Inverter Membutuhkan Baterai Besar?", "id": "Karena Arus DC Yang Ditarik Sangat Tinggi." },
    { "en": "Apa Itu 'Vampire Power' atau 'Phantom Load'?", "id": "Daya Yang Dikonsumsi Alat Saat 'Standby'." },
    { "en": "Solusi Mengurangi 'Vampire Power'?", "id": "Cabut Alat Atau Gunakan Stop Kontak Saklar." },
    { "en": "Apa Itu 'Power Factor Correction' (PFC)?", "id": "Sirkuit Untuk Membuat Beban Terlihat Resistif." },
    { "en": "Mengapa PFC Diperlukan Dalam SMPS?", "id": "Untuk Memenuhi Standar Efisiensi Energi." },
    { "en": "Apa Itu PFC Aktif?", "id": "Menggunakan Sirkuit Elektronik (Boost Converter)." },
    { "en": "Apa Itu PFC Pasif?", "id": "Menggunakan Induktor Besar." },
    { "en": "Apa Itu 'Creepage'?", "id": "Jarak Terpendek Antara Konduktor Di Permukaan." },
    { "en": "Apa Itu 'Clearance'?", "id": "Jarak Terpendek Antara Konduktor Melalui Udara." },
    { "en": "Mengapa Keduanya Penting Dalam Tegangan Tinggi?", "id": "Untuk Mencegah Lonjakan Listrik ('Arcing')." },
    { "en": "Bagaimana Cara Menambah Jarak 'Creepage'?", "id": "Membuat Slot Atau Alur Di PCB." },
    { "en": "Apa Itu Trafo Isolasi?", "id": "Trafo Dengan Rasio 1:1 Untuk Keselamatan." },
    { "en": "Fungsi Trafo Isolasi?", "id": "Mengisolasi Rangkaian Dari Ground Jala-jala." },
    { "en": "Kapan Trafo Isolasi Wajib Digunakan?", "id": "Saat Memperbaiki Peralatan Seperti SMPS." },
    { "en": "Apa Itu Variac?", "id": "Transformator Variabel Untuk Mengatur Tegangan AC." },
    { "en": "Kegunaan Variac?", "id": "Menguji Rangkaian Pada Tegangan Rendah Secara Bertahap." },
    { "en": "Apa Itu 'Dim Bulb Tester'?", "id": "Lampu Pijar Seri Dengan Perangkat Uji." },
    { "en": "Fungsi 'Dim Bulb Tester'?", "id": "Membatasi Arus Jika Ada Hubung Singkat." },
    { "en": "Jika Ada Hubung Singkat, Lampu Bagaimana?", "id": "Akan Menyala Terang." },
    { "en": "Jika Rangkaian Normal, Lampu Bagaimana?", "id": "Menyala Redup Atau Mati Setelah Start." },
    { "en": "Apa Itu Tegangan Tembus Dioda (Breakdown Voltage)?", "id": "Tegangan Mundur Maksimum Sebelum Dioda Konduktif." },
    { "en": "Apa Itu Dioda Zener?", "id": "Dioda Yang Sengaja Dirancang Bekerja Di Daerah Tembus." },
    { "en": "Apa Itu TVS Diode (Transient Voltage Suppressor)?", "id": "Dioda Untuk Melindungi Dari Lonjakan Tegangan." },
    { "en": "Bagaimana Cara Kerja TVS Diode?", "id": "Menjepit (Clamp) Tegangan Pada Level Tertentu." },
    { "en": "Di Mana TVS Diode Dipasang?", "id": "Paralel Dengan Jalur Yang Dilindungi." },
    { "en": "Apa Itu 'Snubber Circuit'?", "id": "Jaringan RC Untuk Meredam 'Ringing'." },
    { "en": "Di Mana 'Snubber' Sering Ditemukan?", "id": "Melintasi Saklar, Relay, Atau Dioda Penyearah." },
    { "en": "Apa Itu 'Heatsinking'?", "id": "Proses Memindahkan Panas Dari Komponen." },
    { "en": "Selain Heatsink Logam, Apa Solusi Lain?", "id": "Menggunakan Area Tembaga Besar Di PCB." },
    { "en": "Apa Itu Resistansi Termal?", "id": "Ukuran Seberapa Sulit Panas Mengalir." },
    { "en": "Resistansi Termal Rendah Berarti Pendinginan Baik?", "id": "Ya, Sangat Baik." },
    { "en": "Apa Masalah Umum Penguat Audio?", "id": "Derau (Noise), Dengung (Hum), Dan Distorsi." },
    { "en": "Apa Itu 'Hum'?", "id": "Derau Frekuensi Rendah (50/60 Hz)." },
    { "en": "Penyebab 'Hum'?", "id": "Ground Loop Atau Kopling Dari Jaringan Listrik." },
    { "en": "Solusi 'Hum'?", "id": "Perbaiki Grounding, Gunakan Kabel Terlindung." },
    { "en": "Apa Itu 'Hiss'?", "id": "Derau Frekuensi Tinggi, Seperti Suara Desis." },
    { "en": "Penyebab 'Hiss'?", "id": "Derau Termal Dari Resistor Dan Transistor." },
    { "en": "Solusi 'Hiss'?", "id": "Gunakan Komponen Rendah Derau, Desain Penguatan Tepat." },
    { "en": "Apa Itu Distorsi 'Clipping'?", "id": "Puncak Gelombang Terpotong Rata." },
    { "en": "Penyebab 'Clipping'?", "id": "Penguatan Terlalu Tinggi Atau Sinyal Input Terlalu Besar." },
    { "en": "Solusinya?", "id": "Turunkan Penguatan Atau Level Input." },
    { "en": "Apa Itu Distorsi 'Crossover'?", "id": "Cacat Kecil Di Dekat Titik Nol Gelombang." },
    { "en": "Penyebab Distorsi 'Crossover'?", "id": "Pembiasan Salah Pada Penguat Kelas AB." },
    { "en": "Solusinya?", "id": "Atur Arus Bias (Bias Current) Dengan Benar." },
    { "en": "Mengapa Kabel Speaker Penting?", "id": "Resistansi Kabel Bisa Mempengaruhi Faktor Redaman." },
    { "en": "Solusinya?", "id": "Gunakan Kabel Tebal Dan Sependek Mungkin." },
    { "en": "Apa Itu Umpan Balik Akustik (Feedback)?", "id": "Suara Speaker Masuk Kembali Ke Mikrofon." },
    { "en": "Solusinya?", "id": "Turunkan Volume, Jauhkan Mikrofon, Gunakan Equalizer." },
    { "en": "Mengapa Radio AM Rawan Gangguan?", "id": "Karena Modulasi Amplitudo Sensitif Terhadap Derau." },
    { "en": "Mengapa Radio FM Lebih Kebal Derau?", "id": "Karena Modulasi Frekuensi Tidak Terpengaruh Amplitudo." },
    { "en": "Masalah: Penerimaan Radio Buruk?", "id": "Antena Tidak Baik Atau Sinyal Lemah." },
    { "en": "Solusinya?", "id": "Gunakan Antena Eksternal, Pindahkan Posisi Radio." },
    { "en": "Apa Itu 'Squelch'?", "id": "Sirkuit Untuk Membungkam Derau Saat Tidak Ada Sinyal." },
    { "en": "Apa Itu 'Automatic Gain Control' (AGC)?", "id": "Menjaga Level Output Tetap Konstan." },
    { "en": "Apa Itu 'Mixer' Dalam Radio?", "id": "Sirkuit Untuk Mengubah Frekuensi Sinyal." },
    { "en": "Apa Itu 'Local Oscillator' (LO)?", "id": "Osilator Internal Di Penerima Radio." },
    { "en": "Apa Itu Frekuensi Menengah (IF)?", "id": "Hasil Dari Pencampuran Sinyal RF Dan LO." },
    { "en": "Mengapa Perlu IF?", "id": "Lebih Mudah Untuk Menguatkan Dan Menyaring Satu Frekuensi Tetap." },
    { "en": "Ini Adalah Prinsip Penerima Apa?", "id": "Penerima Superheterodyne." },
    { "en": "Masalah 'Image Frequency'?", "id": "Frekuensi Lain Yang Juga Bisa Masuk Ke IF." },
    { "en": "Solusinya?", "id": "Gunakan Filter 'Preselector' Di Bagian Depan." },
    { "en": "Apa Itu 'Shielding' Dalam Rangkaian RF?", "id": "Menggunakan Casing Logam Untuk Memblokir Interferensi." },
    { "en": "Mengapa Layout PCB RF Sangat Kritis?", "id": "Jalur Bisa Bertindak Seperti Induktor Dan Kapasitor." },
    { "en": "Aturan Desain PCB RF?", "id": "Gunakan Ground Plane, Jalur Pendek, Sudut Tumpul." },
    { "en": "Apa Itu 'Microstrip'?", "id": "Jalur PCB Dengan Impedansi Karakteristik Terkontrol." },
    { "en": "Apa Itu Impedansi Karakteristik?", "id": "Impedansi Yang Dilihat Oleh Gelombang." },
    { "en": "Nilai Umumnya?", "id": "Lima Puluh Ohm (50 Ω)." },
    { "en": "Masalah: Refleksi Sinyal RF?", "id": "Ketidakcocokan Impedansi." },
    { "en": "Solusinya?", "id": "Lakukan Pencocokan Impedansi (Impedance Matching)." },
    { "en": "Mengapa Osilator RF Perlu Stabil?", "id": "Agar Frekuensi Pemancar/Penerima Tidak Bergeser." },
    { "en": "Osilator Apa Yang Sangat Stabil?", "id": "Osilator Kristal Atau PLL." },
    { "en": "Apa Itu 'Phase Noise' Osilator?", "id": "Ketidakstabilan Fasa Jangka Pendek." },
    { "en": "Masalah 'Phase Noise'?", "id": "Mengganggu Sinyal Yang Berdekatan." },
    { "en": "Apa Itu 'Gain Compression' Penguat RF?", "id": "Penguatan Menurun Pada Daya Input Tinggi." },
    { "en": "Apa Itu Titik Kompresi 1dB (P1dB)?", "id": "Titik Di Mana Penguatan Turun Sebesar 1dB." },
    { "en": "Apa Itu 'Intermodulation Distortion' (IMD)?", "id": "Produk Frekuensi Baru Akibat Non-Linearitas." },
    { "en": "Masalah IMD?", "id": "Menyebabkan Interferensi Di Dalam Dan Di Luar Pita." },
    { "en": "Apa Itu 'Third-Order Intercept Point' (IP3)?", "id": "Ukuran Kinerja Linearitas Sebuah Penguat." },
    { "en": "IP3 Tinggi Berarti Penguat Bagaimana?", "id": "Sangat Linear." },
    { "en": "Apa Itu 'Noise Figure' (NF)?", "id": "Ukuran Seberapa Banyak Derau Ditambahkan Oleh Rangkaian." },
    { "en": "NF Rendah Berarti Rangkaian Bagaimana?", "id": "Menambahkan Sangat Sedikit Derau." },
    { "en": "Bagian Mana Dari Penerima Yang Harus Paling Rendah Derau?", "id": "Penguat Paling Depan (LNA)." },
    { "en": "Apa Itu LNA (Low-Noise Amplifier)?", "id": "Penguat Dengan 'Noise Figure' Sangat Rendah." },
    { "en": "Mengapa LNA Ditempatkan Dekat Antena?", "id": "Untuk Menguatkan Sinyal Lemah Sebelum Kehilangan." },
    { "en": "Apa Itu 'Smith Chart'?", "id": "Alat Grafis Untuk Analisis Rangkaian RF." },
    { "en": "Untuk Apa 'Smith Chart' Digunakan?", "id": "Desain Jaringan Pencocokan Impedansi." },
    { "en": "Apa Itu 'Vector Network Analyzer' (VNA)?", "id": "Alat Untuk Mengukur Parameter-S Rangkaian RF." },
    { "en": "Apa Itu S-Parameters?", "id": "Parameter Yang Menggambarkan Refleksi Dan Transmisi." },
    { "en": "Mengapa Kabel Koaksial Digunakan Di RF?", "id": "Karena Sifatnya Sebagai Saluran Transmisi Terlindung." },
    { "en": "Konektor Apa Yang Umum Di RF?", "id": "BNC, SMA, N-Type." },
    { "en": "Masalah Jika Konektor RF Longgar?", "id": "Menyebabkan Refleksi Dan Kehilangan Sinyal." },
    { "en": "Solusinya?", "id": "Kencangkan Konektor Dengan Torsi Yang Tepat." },
    { "en": "Apa Itu 'Return Loss'?", "id": "Ukuran Seberapa Baik Pencocokan Impedansi." },
    { "en": "Return Loss' Tinggi Itu Baik Atau Buruk?", "id": "Baik, Artinya Sedikit Sinyal Yang Direfleksikan." },
    { "en": "Apa Itu 'Self-Resonant Frequency' (SRF)?", "id": "Frekuensi Di Mana Komponen Mulai Bertingkah Aneh." },
    { "en": "Bagaimana Perilaku Kapasitor Di Atas SRF?", "id": "Bertingkah Seperti Induktor." },
    { "en": "Bagaimana Perilaku Induktor Di Atas SRF?", "id": "Bertingkah Seperti Kapasitor." },
    { "en": "Penyebabnya?", "id": "Induktansi Dan Kapasitansi Parasitik Internal." },
    { "en": "Apa Itu 'Decoupling' Dalam RF?", "id": "Mengisolasi Bagian DC Dan RF Rangkaian." },
    { "en": "Komponen Apa Yang Digunakan?", "id": "Induktor (RF Choke) Dan Kapasitor." },
    { "en": "Apa Itu 'Bias Tee'?", "id": "Alat Untuk Menyuntikkan Tegangan DC Ke Kabel Koaksial." },
    { "en": "Untuk Apa 'Bias Tee' Digunakan?", "id": "Memberi Daya LNA Yang Terpasang Di Antena." },
    { "en": "Mengapa Grounding RF Sangat Penting?", "id": "Jalur Ground Pendek Mencegah Induktansi Liar." },
    { "en": "Apa Itu 'Via Stitching'?", "id": "Menambahkan Banyak 'Via' Untuk Menghubungkan Ground." },
    { "en": "Tujuannya?", "id": "Menciptakan Jalur Ground Impedansi Rendah." },
    { "en": "Apa Itu 'Harmonic Distortion'?", "id": "Munculnya Harmonisa Akibat Non-Linearitas." },
    { "en": "Solusinya?", "id": "Gunakan Filter Lolos-Rendah Di Output Pemancar." },
    { "en": "Apa Itu 'Spurious Emissions'?", "id": "Emisi Yang Tidak Diinginkan Di Luar Pita." },
    { "en": "Mengapa 'Spurious Emissions' Harus Ditekan?", "id": "Agar Tidak Mengganggu Layanan Radio Lain." },
    { "en": "Apa Itu 'Band-Pass Filter'?", "id": "Filter Yang Hanya Melewatkan Satu Rentang Frekuensi." },
    { "en": "Di Mana Ini Digunakan?", "id": "Di Bagian Depan Penerima Untuk Memilih Pita." },
    { "en": "Apa Itu 'Dummy Load'?", "id": "Beban 50 Ohm Untuk Menguji Pemancar." },
    { "en": "Mengapa Perlu 'Dummy Load'?", "id": "Agar Bisa Menguji Tanpa Memancarkan Sinyal." },
    { "en": "Apa Itu 'Power Amplifier' (PA)?", "id": "Penguat Terakhir Dalam Rantai Pemancar." },
    { "en": "Apa Tantangan Desain PA?", "id": "Efisiensi Tinggi Dan Linearitas Baik." },
    { "en": "Apa Itu 'Antenna Gain'?", "id": "Kemampuan Antena Memfokuskan Energi." },
    { "en": "Diukur Dalam Satuan Apa?", "id": "dBi (Desibel Terhadap Antena Isotropik)." },
    { "en": "Apa Itu Antena Isotropik?", "id": "Antena Teoretis Yang Memancar Sama Rata." },
    { "en": "Apa Masalah Umum Sensor?", "id": "Pembacaan Tidak Akurat Atau Tidak Stabil." },
    { "en": "Penyebab Pembacaan Sensor Tidak Akurat?", "id": "Kalibrasi Buruk, Derau, Atau Pemasangan Salah." },
    { "en": "Solusi Sensor Tidak Akurat?", "id": "Lakukan Kalibrasi Ulang Terhadap Referensi." },
    { "en": "Apa Itu Kalibrasi?", "id": "Menyesuaikan Output Sensor Agar Sesuai Standar." },
    { "en": "Mengapa Sensor Perlu Sinyal 'Conditioning'?", "id": "Karena Outputnya Seringkali Sangat Kecil." },
    { "en": "Apa Itu 'Signal Conditioning'?", "id": "Menguatkan, Menyaring, Dan Mengubah Sinyal Sensor." },
    { "en": "Rangkaian Apa Yang Digunakan?", "id": "Penguat Instrumentasi, Filter, Dan Konverter." },
    { "en": "Masalah: Sensor Ultrasonik Gagal Mendeteksi?", "id": "Halangan Tidak Memantulkan Suara Dengan Baik." },
    { "en": "Solusinya?", "id": "Pastikan Targetnya Keras Dan Datar." },
    { "en": "Masalah: Sensor Inframerah Memberi Pembacaan Salah?", "id": "Dipengaruhi Oleh Cahaya Matahari Atau Lampu." },
    { "en": "Solusinya?", "id": "Lindungi Sensor Dari Sumber Cahaya Lain." },
    { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Berbasis Efek Seebeck." },
    { "en": "Masalah Termokopel?", "id": "Menghasilkan Tegangan Sangat Kecil (mikrovolt)." },
    { "en": "Solusinya?", "id": "Memerlukan Penguat Presisi Tinggi." },
    { "en": "Apa Itu 'Cold Junction Compensation'?", "id": "Koreksi Untuk Suhu Di Titik Sambungan." },
    { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Perubahan Resistansi." },
    { "en": "Bahan Apa Yang Umum Digunakan?", "id": "Platinum (Contoh: PT100)." },
    { "en": "Masalah Pengukuran RTD?", "id": "Resistansi Kabel Bisa Mempengaruhi Akurasi." },
    { "en": "Solusinya?", "id": "Gunakan Konfigurasi Tiga Atau Empat Kawat." },
    { "en": "Apa Itu 'Strain Gauge'?", "id": "Sensor Untuk Mengukur Regangan Atau Tekanan." },
    { "en": "Bagaimana 'Strain Gauge' Bekerja?", "id": "Resistansinya Berubah Saat Diregangkan." },
    { "en": "Konfigurasi Rangkaian Apa Yang Digunakan?", "id": "Jembatan Wheatstone." },
    { "en": "Mengapa Menggunakan Jembatan Wheatstone?", "id": "Sangat Sensitif Terhadap Perubahan Resistansi Kecil." },
    { "en": "Apa Itu 'Load Cell'?", "id": "Sensor Berat Yang Menggunakan 'Strain Gauge'." },
    { "en": "Masalah: Sensor Kelembaban Tidak Akurat?", "id": "Terkontaminasi Oleh Debu Atau Bahan Kimia." },
    { "en": "Solusinya?", "id": "Bersihkan Sensor Atau Ganti Dengan Yang Baru." },
    { "en": "Apa Itu Akselerometer?", "id": "Sensor Untuk Mengukur Percepatan Dan Getaran." },
    { "en": "Apa Itu Giroskop?", "id": "Sensor Untuk Mengukur Kecepatan Sudut." },
    { "en": "Apa Itu IMU (Inertial Measurement Unit)?", "id": "Gabungan Akselerometer Dan Giroskop." },
    { "en": "Masalah Giroskop?", "id": "Mengalami 'Drift' (Kesalahan Terakumulasi) Seiring Waktu." },
    { "en": "Solusinya?", "id": "Gabungkan Datanya Dengan Sensor Lain (Magnetometer)." },
    { "en": "Apa Itu Antarmuka I2C?", "id": "Bus Komunikasi Dua Kawat (SDA, SCL)." },
    { "en": "Masalah I2C?", "id": "Alamat Perangkat Bertabrakan." },
    { "en": "Solusi Alamat Bertabrakan?", "id": "Gunakan Multiplexer I2C Atau Perangkat Dengan Alamat Bisa Diubah." },
    { "en": "Apa Itu Antarmuka SPI?", "id": "Bus Komunikasi Empat Kawat (MOSI, MISO, SCK, CS)." },
    { "en": "Keuntungan SPI Dibanding I2C?", "id": "Lebih Cepat Dan 'Full-Duplex'." },
    { "en": "Masalah SPI?", "id": "Membutuhkan Lebih Banyak Pin." },
    { "en": "Apa Itu UART?", "id": "Komunikasi Serial Asinkron (TX, RX)." },
    { "en": "Masalah UART?", "id": "Baud Rate, Bit Data, Paritas Harus Cocok." },
    { "en": "Apa Itu RS-232?", "id": "Standar Antarmuka Serial Tegangan Lebih Tinggi." },
    { "en": "Apa Itu RS-485?", "id": "Standar Serial Diferensial Jarak Jauh." },
    { "en": "Keuntungan RS-485?", "id": "Kebal Derau Dan Bisa Untuk Banyak Perangkat." },
    { "en": "Apa Itu CAN Bus?", "id": "Bus Komunikasi Yang Digunakan Di Otomotif." },
    { "en": "Keuntungan CAN Bus?", "id": "Sangat Andal Dan Tahan Gangguan." },
    { "en": "Masalah: Pembacaan Sensor Analog Berisik?", "id": "Kabel Sensor Terlalu Panjang Dan Tidak Terlindung." },
    { "en": "Solusinya?", "id": "Gunakan Kabel Terlindung, Pisahkan Dari Kabel Daya." },
    { "en": "Apa Itu Filter Digital?", "id": "Menyaring Derau Menggunakan Perangkat Lunak." },
    { "en": "Contoh Filter Digital?", "id": "Filter Rata-rata Bergerak (Moving Average)." },
    { "en": "Apa Itu Filter Kalman?", "id": "Filter Optimal Untuk Mengestimasi Keadaan Sistem." },
    { "en": "Mengapa Sensor Perlu Waktu Pemanasan?", "id": "Agar Mencapai Kestabilan Termal Dan Operasi." },
    { "en": "Apa Itu Histeresis Sensor?", "id": "Pembacaan Berbeda Saat Mendekati Dari Atas Atau Bawah." },
    { "en": "Apa Itu Linearitas Sensor?", "id": "Seberapa Lurus Kurva Respon Sensor." },
    { "en": "Apa Itu Resolusi Sensor?", "id": "Perubahan Terkecil Yang Bisa Dideteksi." },
    { "en": "Apa Itu Akurasi Sensor?", "id": "Seberapa Dekat Pembacaan Dengan Nilai Sebenarnya." },
    { "en": "Apa Itu Presisi Sensor?", "id": "Seberapa Konsisten Pembacaan Yang Berulang." },
    { "en": "Bisakah Sensor Presisi Tapi Tidak Akurat?", "id": "Ya, Jika Ada Ofset Sistematis." },
    { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistansinya Berubah Dengan Intensitas Cahaya." },
    { "en": "Masalah LDR?", "id": "Respon Lambat Dan Tidak Terlalu Akurat." },
    { "en": "Apa Itu Fotodioda?", "id": "Sensor Cahaya Yang Lebih Cepat Dan Linear." },
    { "en": "Apa Itu Fototransistor?", "id": "Sensor Cahaya Dengan Penguatan Internal." },
    { "en": "Masalah: Sensor Jarak Optik Gagal?", "id": "Warna Atau Tekstur Permukaan Mempengaruhi Pembacaan." },
    { "en": "Solusinya?", "id": "Gunakan Sensor Ultrasonik Yang Tidak Peduli Warna." },
    { "en": "Apa Itu 'Aliasing' Sinyal?", "id": "Frekuensi Tinggi Tampak Sebagai Frekuensi Rendah." },
    { "en": "Penyebab 'Aliasing'?", "id": "Laju Sampling Terlalu Rendah." },
    { "en": "Solusinya?", "id": "Gunakan Filter Anti-Aliasing (Lolos-Rendah)." },
    { "en": "Apa Itu 'Sample and Hold Circuit'?", "id": "Mencuplik Dan Menahan Tegangan Analog." },
    { "en": "Mengapa Diperlukan Sebelum ADC?", "id": "Agar Tegangan Stabil Selama Proses Konversi." },
    { "en": "Apa Itu 'Ground Bounce'?", "id": "Derau Pada Jalur Ground Akibat Switching Cepat." },
    { "en": "Masalah 'Ground Bounce'?", "id": "Dapat Merusak Pengukuran Analog Presisi." },
    { "en": "Solusi 'Ground Bounce'?", "id": "Pisahkan Ground Analog Dan Digital." },
    { "en": "Bagaimana Cara Menghubungkan Ground Terpisah?", "id": "Hubungkan Hanya Di Satu Titik (Star Ground)." },
    { "en": "Apa Itu 'Ferrite Bead'?", "id": "Komponen Untuk Menekan Derau Frekuensi Tinggi." },
    { "en": "Di Mana Ditempatkan?", "id": "Seri Dengan Jalur Catu Daya Sensor." },
    { "en": "Apa Itu Sensor Pasif?", "id": "Tidak Membutuhkan Catu Daya Eksternal." },
    { "en": "Contoh Sensor Pasif?", "id": "Termokopel, Tombol." },
    { "en": "Apa Itu Sensor Aktif?", "id": "Membutuhkan Catu Daya Untuk Bekerja." },
    { "en": "Contoh Sensor Aktif?", "id": "Akselerometer, Sensor Hall." },
    { "en": "Apa Itu 'Polling' Sensor?", "id": "Membaca Sensor Secara Berkala Dalam Loop." },
    { "en": "Apa Kekurangan 'Polling'?", "id": "Membuang Waktu CPU, Bisa Melewatkan Peristiwa Cepat." },
    { "en": "Apa Alternatif 'Polling'?", "id": "Menggunakan 'Interrupt'." },
    { "en": "Bagaimana 'Interrupt' Bekerja?", "id": "Sensor Memberi Sinyal Saat Ada Data Baru." },
    { "en": "Apa Itu 'Level Shifter'?", "id": "Sirkuit Untuk Mengubah Level Tegangan Logika." },
    { "en": "Kapan 'Level Shifter' Diperlukan?", "id": "Menghubungkan Mikrokontroler 5V Dengan Sensor 3.3V." },
    { "en": "Apa Risiko Menghubungkan 5V Ke Pin 3.3V?", "id": "Kerusakan Permanen Pada Pin Atau Chip." },
    { "en": "Banyak Pin Mikrokontroler '5V Tolerant'?", "id": "Ya, Beberapa Dirancang Untuk Menerima Tegangan 5V." },
    { "en": "Apa Itu 'Time of Flight' (ToF) Sensor?", "id": "Mengukur Jarak Berdasarkan Waktu Tempuh Cahaya." },
    { "en": "Keuntungan ToF?", "id": "Sangat Akurat Dan Jangkauan Jauh." },
    { "en": "Apa Itu 'Wheatstone Bridge'?", "id": "Rangkaian Untuk Mengukur Perubahan Resistansi Kecil." },
    { "en": "Di Mana Ini Digunakan?", "id": "Sensor Tekanan, Regangan, Dan Suhu (RTD)." },
    { "en": "Apa Itu 'Self-Heating' Pada Sensor?", "id": "Arus Pengukuran Menyebabkan Sensor Menjadi Panas." },
    { "en": "Masalah 'Self-Heating'?", "id": "Menyebabkan Kesalahan Pengukuran (Terutama Suhu)." },
    { "en": "Solusinya?", "id": "Gunakan Arus Pengukuran Sekecil Mungkin." },
    { "en": "Mengapa Keselamatan Listrik Penting?", "id": "Karena Listrik Bisa Melukai Dan Mematikan." },
    { "en": "Apa Bahaya Utama Listrik?", "id": "Sengatan (Shock), Luka Bakar, Kebakaran." },
    { "en": "Berapa Arus Yang Berbahaya Bagi Manusia?", "id": "Beberapa Puluh Miliampere Sudah Berbahaya." },
    { "en": "Mengapa Tegangan Rendah Bisa Berbahaya?", "id": "Jika Resistansi Tubuh Rendah (Kulit Basah)." },
    { "en": "Aturan Keselamatan Utama?", "id": "Selalu Anggap Rangkaian Bertegangan." },
    { "en": "Apa Yang Harus Dilakukan Sebelum Bekerja?", "id": "Matikan Dan Cabut Sumber Daya." },
    { "en": "Apa Itu 'Lockout-Tagout'?", "id": "Prosedur Untuk Memastikan Sirkuit Tetap Mati." },
    { "en": "Mengapa Perlu Mengosongkan Kapasitor?", "id": "Menyimpan Muatan Berbahaya Bahkan Setelah Daya Mati." },
    { "en": "Bagaimana Cara Aman Mengosongkan Kapasitor?", "id": "Gunakan Resistor Daya, Jangan Hubung Singkat." },
    { "en": "Mengapa Bekerja Sendirian Itu Berisiko?", "id": "Tidak Ada Yang Membantu Jika Terjadi Kecelakaan." },
    { "en": "Aturan Satu Tangan Berguna Untuk Apa?", "id": "Mencegah Arus Melintasi Jantung." },
    { "en": "Mengapa Perhiasan Logam Harus Dilepas?", "id": "Bisa Menyebabkan Hubung Singkat Atau Luka Bakar." },
    { "en": "Apa Itu Trafo Isolasi?", "id": "Trafo 1:1 Yang Mengisolasi Dari Ground PLN." },
    { "en": "Mengapa Trafo Isolasi Meningkatkan Keamanan?", "id": "Mencegah Sengatan Jika Menyentuh Satu Titik." },
    { "en": "Apa Itu GFCI?", "id": "Ground Fault Circuit Interrupter." },
    { "en": "Bagaimana GFCI Melindungi?", "id": "Mendeteksi Kebocoran Arus Kecil Ke Tanah." },
    { "en": "Apa Itu Keandalan (Reliability)?", "id": "Kemungkinan Produk Bekerja Tanpa Kegagalan." },
    { "en": "Apa Itu MTBF?", "id": "Mean Time Between Failures." },
    { "en": "MTBF Mengukur Apa?", "id": "Keandalan Produk Yang Dapat Diperbaiki." },
    { "en": "Apa Itu MTTF?", "id": "Mean Time To Failure." },
    { "en": "MTTF Mengukur Apa?", "id": "Keandalan Produk Yang Tidak Dapat Diperbaiki." },
    { "en": "Faktor Apa Yang Mempengaruhi Keandalan?", "id": "Suhu, Tegangan, Getaran, Kelembaban." },
    { "en": "Apa Itu 'Derating'?", "id": "Menggunakan Komponen Di Bawah Rating Maksimalnya." },
    { "en": "Mengapa 'Derating' Meningkatkan Keandalan?", "id": "Mengurangi Stres Pada Komponen." },
    { "en": "Komponen Apa Yang Paling Tidak Andal?", "id": "Komponen Dengan Bagian Bergerak (Kipas, Relay)." },
    { "en": "Komponen Elektronik Pasif Paling Tidak Andal?", "id": "Kapasitor Elektrolit." },
    { "en": "Apa Itu 'Infant Mortality'?", "id": "Tingkat Kegagalan Tinggi Di Awal Kehidupan Produk." },
    { "en": "Penyebab 'Infant Mortality'?", "id": "Cacat Produksi." },
    { "en": "Solusinya?", "id": "Lakukan Tes 'Burn-in'." },
    { "en": "Apa Itu Tes 'Burn-in'?", "id": "Mengoperasikan Produk Pada Kondisi Stres." },
    { "en": "Apa Itu 'Redundancy'?", "id": "Menyediakan Komponen Cadangan." },
    { "en": "Tujuan 'Redundancy'?", "id": "Sistem Tetap Bekerja Jika Satu Bagian Gagal." },
    { "en": "Contoh 'Redundancy'?", "id": "Catu Daya Ganda, Sistem Pengereman Ganda." },
    { "en": "Apa Itu 'Fail-Safe Design'?", "id": "Desain Yang Gagal Dalam Mode Aman." },
    { "en": "Contoh 'Fail-Safe'?", "id": "Rem Kereta Akan Aktif Jika Kehilangan Daya." },
    { "en": "Apa Itu FMEA?", "id": "Failure Mode and Effects Analysis." },
    { "en": "Tujuan FMEA?", "id": "Menganalisis Dan Mencegah Potensi Kegagalan." },
    { "en": "Mengapa Kelembaban Buruk Untuk Elektronik?", "id": "Menyebabkan Korosi Dan Hubung Singkat." },
    { "en": "Solusinya?", "id": "Gunakan 'Conformal Coating' Atau Casing Kedap." },
    { "en": "Apa Itu 'Silica Gel'?", "id": "Bahan Pengering Untuk Menyerap Kelembaban." },
    { "en": "Mengapa Getaran Buruk Untuk Elektronik?", "id": "Dapat Merusak Solderan Dan Konektor." },
    { "en": "Solusinya?", "id": "Gunakan Peredam Getaran Atau 'Potting'." },
    { "en": "Apa Itu Tes HALT?", "id": "Highly Accelerated Life Test." },
    { "en": "Tujuan Tes HALT?", "id": "Menemukan Titik Lemah Desain Dengan Cepat." },
    { "en": "Apa Itu Tes HASS?", "id": "Highly Accelerated Stress Screen." },
    { "en": "Tujuan Tes HASS?", "id": "Menyaring Cacat Produksi." },
    { "en": "Mengapa Suhu Tinggi Mempercepat Kegagalan?", "id": "Mempercepat Proses Degradasi Kimia." },
    { "en": "Aturan Umumnya?", "id": "Setiap Kenaikan 10°C, Umur Pakai Berkurang Setengah." },
    { "en": "Apa Itu Siklus Termal (Thermal Cycling)?", "id": "Perubahan Berulang Antara Suhu Panas Dan Dingin." },
    { "en": "Masalah Siklus Termal?", "id": "Menyebabkan Retak Akibat Ekspansi Termal Berbeda." },
    { "en": "Apa Itu Koefisien Ekspansi Termal (CTE)?", "id": "Ukuran Seberapa Besar Bahan Memuai." },
    { "en": "Masalah Jika CTE Berbeda Jauh?", "id": "Menimbulkan Stres Mekanis Pada Sambungan." },
    { "en": "Apa Itu 'Tin Pest'?", "id": "Transformasi Timah Menjadi Bubuk Abu-abu." },
    { "en": "Kapan 'Tin Pest' Terjadi?", "id": "Pada Suhu Sangat Rendah." },
    { "en": "Solusinya?", "id": "Gunakan Campuran Solder (Alloy)." },
    { "en": "Apa Itu Radiasi Pengion?", "id": "Radiasi Yang Dapat Merusak Semikonduktor." },
    { "en": "Di Mana Ini Menjadi Masalah?", "id": "Di Luar Angkasa Dan Aplikasi Nuklir." },
    { "en": "Solusinya?", "id": "Gunakan Komponen Tahan Radiasi ('Rad-Hard')." },
    { "en": "Apa Itu 'Single Event Upset' (SEU)?", "id": "Perubahan Bit Memori Akibat Partikel Radiasi." },
    { "en": "Solusinya?", "id": "Gunakan Memori ECC (Error-Correcting Code)." },
    { "en": "Apa Itu RoHS?", "id": "Restriction of Hazardous Substances." },
    { "en": "Tujuannya?", "id": "Mengurangi Dampak Lingkungan Dari Limbah Elektronik." },
    { "en": "Apa Itu WEEE?", "id": "Waste Electrical and Electronic Equipment Directive." },
    { "en": "Tujuannya?", "id": "Mendorong Daur Ulang Peralatan Elektronik." },
    { "en": "Mengapa Limbah Elektronik Berbahaya?", "id": "Mengandung Logam Berat Seperti Timbal Dan Merkuri." },
    { "en": "Apa Itu 'Conflict Minerals'?", "id": "Mineral Yang Ditambang Di Zona Konflik." },
    { "en": "Contohnya?", "id": "Timah, Tantalum, Tungsten, Emas." },
    { "en": "Mengapa Sirkuit Analog Lebih Sensitif Derau?", "id": "Karena Bekerja Dengan Level Sinyal Kecil." },
    { "en": "Apa Itu 'Signal-to-Noise Ratio' (SNR)?", "id": "Rasio Kekuatan Sinyal Terhadap Kekuatan Derau." },
    { "en": "SNR Tinggi Berarti Kualitas Sinyal Baik?", "id": "Ya, Sangat Baik." },
    { "en": "Apa Itu 'Shielded Twisted Pair' (STP)?", "id": "Kabel Pasangan Berpilin Dengan Lapisan Pelindung." },
    { "en": "Kapan STP Digunakan?", "id": "Di Lingkungan Dengan Interferensi Listrik Tinggi." },
    { "en": "Apa Itu 'Ergonomics'?", "id": "Ilmu Tentang Desain Produk Yang Nyaman." },
    { "en": "Mengapa Ergonomi Penting?", "id": "Meningkatkan Kegunaan Dan Mencegah Cedera." },
    { "en": "Apa Itu 'User Interface' (UI)?", "id": "Bagaimana Pengguna Berinteraksi Dengan Perangkat." },
    { "en": "UI Yang Baik Itu Seperti Apa?", "id": "Intuitif, Responsif, Dan Jelas." },
    { "en": "Apa Itu 'User Experience' (UX)?", "id": "Keseluruhan Pengalaman Pengguna Dengan Produk." },
    { "en": "Apa Itu 'Planned Maintenance'?", "id": "Perawatan Terjadwal Untuk Mencegah Kerusakan." },
    { "en": "Contohnya?", "id": "Membersihkan Filter Kipas, Mengganti Baterai Cadangan." },
    { "en": "Apa Itu 'Predictive Maintenance'?", "id": "Memprediksi Kapan Komponen Akan Gagal." },
    { "en": "Bagaimana Caranya?", "id": "Dengan Memantau Getaran, Suhu, Atau Parameter Lain." },
    { "en": "Apa Itu 'Red Tagging'?", "id": "Memberi Label Pada Peralatan Yang Rusak." },
    { "en": "Tujuannya?", "id": "Mencegah Penggunaan Peralatan Yang Tidak Aman." },
    { "en": "Apa Itu Peringkat IP?", "id": "Ingress Protection (Perlindungan Masuk)." },
    { "en": "Digit Pertama IP Menunjukkan Perlindungan Terhadap Apa?", "id": "Benda Padat (Debu)." },
    { "en": "Digit Kedua IP Menunjukkan Perlindungan Terhadap Apa?", "id": "Cairan (Air)." },
    { "en": "IP68 Berarti Apa?", "id": "Tahan Debu Total Dan Tahan Terendam Air." },
    { "en": "Apa Itu Peringkat NEMA?", "id": "Standar Casing (Enclosure) Di Amerika Utara." },
    { "en": "Apa Itu 'Intrinsically Safe'?", "id": "Peralatan Yang Dirancang Untuk Area Berbahaya." },
    { "en": "Bagaimana Caranya?", "id": "Membatasi Energi Listrik Agar Tidak Memicu Ledakan." },
    { "en": "Di Mana Ini Digunakan?", "id": "Pertambangan, Pabrik Kimia." },
    { "en": "Apa Itu 'Functional Safety'?", "id": "Bagian Dari Keamanan Yang Bergantung Pada Sistem." },
    { "en": "Contohnya?", "id": "Sistem Pengereman Darurat Otomatis." },
    { "en": "Apa Itu Standar SIL?", "id": "Safety Integrity Level." },
    { "en": "SIL Mengukur Apa?", "id": "Tingkat Pengurangan Risiko Oleh Fungsi Keselamatan." },
    { "en": "SIL 4 Adalah Tingkat Tertinggi?", "id": "Ya, Paling Andal Dan Paling Sulit Dicapai." },
    { "en": "Apa Itu 'Occam's Razor' Dalam Diagnostik?", "id": "Penjelasan Paling Sederhana Biasanya Yang Benar." },
    { "en": "Contoh Penerapannya?", "id": "Periksa Steker Sebelum Membongkar Catu Daya." },
    { "en": "Apa Itu 'Rubber Duck Debugging'?", "id": "Menjelaskan Masalah Ke Objek Mati." },
    { "en": "Mengapa Metode Ini Efektif?", "id": "Membantu Mengatur Pikiran Dan Menemukan Kesalahan Logika." },
    { "en": "Apa Itu 'Root Cause'?", "id": "Penyebab Paling Mendasar Dari Suatu Masalah." },
    { "en": "Mengapa Penting Mencari 'Root Cause'?", "id": "Agar Masalah Tidak Terulang Kembali." },
    { "en": "Apa Itu 'Five Whys'?", "id": "Teknik Bertanya 'Mengapa' Lima Kali." },
    { "en": "Tujuannya?", "id": "Untuk Menggali Hingga Menemukan 'Root Cause'." },
    { "en": "Apa Itu Diagram Ishikawa?", "id": "Diagram Tulang Ikan Untuk Analisis Sebab-Akibat." },
    { "en": "Kategori Utama Dalam Diagram Ishikawa?", "id": "Manusia, Mesin, Metode, Material, Lingkungan." },
    { "en": "Apa Itu 'Confirmation Bias'?", "id": "Kecenderungan Mencari Bukti Yang Mendukung Teori." },
    { "en": "Bagaimana Menghindari 'Confirmation Bias'?", "id": "Cobalah Untuk Membuktikan Teori Anda Salah." },
    { "en": "Mengapa Perlu Mendokumentasikan Proses Perbaikan?", "id": "Sebagai Referensi Jika Masalah Serupa Muncul." },
    { "en": "Apa Itu 'Triboelectric Effect'?", "id": "Timbulnya Muatan Statis Saat Benda Bergesekan." },
    { "en": "Ini Adalah Sumber Utama Dari Apa?", "id": "ESD (Electrostatic Discharge)." },
    { "en": "Mengapa Lantai Karpet Buruk Untuk Ruang Elektronik?", "id": "Menghasilkan Banyak Listrik Statis." },
    { "en": "Apa Itu 'Air Ionizer'?", "id": "Alat Untuk Menetralisir Muatan Statis Di Udara." },
    { "en": "Apa Itu 'Faraday Cage'?", "id": "Sangkar Konduktif Yang Memblokir Medan Elektromagnetik." },
    { "en": "Kantong Anti-Statis Adalah 'Faraday Cage'?", "id": "Ya, Versi Fleksibelnya." },
    { "en": "Apa Beda Kantong Pink Dan Kantong Metalik?", "id": "Pink Anti-Statis, Metalik Memberi Perisai Penuh." },
    { "en": "Kapan Menggunakan Kantong Metalik?", "id": "Untuk Komponen Yang Sangat Sensitif." },
    { "en": "Apa Itu 'Hot Plate' Untuk Elektronika?", "id": "Pemanas Untuk Pemanasan Awal PCB." },
    { "en": "Tujuannya?", "id": "Memudahkan Penyolderan Dan Mencegah Stres Termal." },
    { "en": "Apa Itu Mikroskop Digital?", "id": "Alat Bantu Untuk Inspeksi Solderan SMD." },
    { "en": "Apa Itu 'Component Tester'?", "id": "Alat Murah Untuk Mengidentifikasi Dan Menguji Komponen." },
    { "en": "Apa Itu 'Standard Operating Procedure' (SOP)?", "id": "Instruksi Langkah-demi-Langkah Untuk Suatu Tugas." },
    { "en": "Mengapa SOP Penting?", "id": "Memastikan Konsistensi Dan Mengurangi Kesalahan." },
    { "en": "Apa Itu 'Checklist'?", "id": "Daftar Pemeriksaan Untuk Memastikan Tidak Ada Yang Terlewat." },
    { "en": "Apa Itu 'Preventive Maintenance'?", "id": "Perawatan Terjadwal Untuk Mencegah Kegagalan." },
    { "en": "Contoh 'Preventive Maintenance'?", "id": "Membersihkan Debu Dari Kipas Pendingin." },
    { "en": "Apa Itu Kalibrasi?", "id": "Membandingkan Alat Ukur Dengan Standar." },
    { "en": "Mengapa Kalibrasi Rutin Penting?", "id": "Untuk Memastikan Akurasi Pengukuran." },
    { "en": "Apa Itu Standar NIST?", "id": "Standar Referensi Nasional Di Amerika Serikat." },
    { "en": "Apa Itu 'Human Error'?", "id": "Kesalahan Yang Disebabkan Oleh Manusia." },
    { "en": "Bagaimana Mengurangi 'Human Error'?", "id": "Dengan Pelatihan, SOP, Dan Desain Yang Baik." },
    { "en": "Apa Itu 'Poka-Yoke'?", "id": "Desain Anti-Salah (Mistake-Proofing)." },
    { "en": "Contoh 'Poka-Yoke'?", "id": "Konektor USB Yang Hanya Bisa Masuk Satu Arah." },
    { "en": "Apa Itu 'Safety Factor' Dalam Desain?", "id": "Merancang Sesuatu Lebih Kuat Dari Kebutuhan." },
    { "en": "Apa Itu 'Single Point of Failure'?", "id": "Satu Komponen Yang Jika Gagal, Seluruh Sistem Gagal." },
    { "en": "Desain Yang Baik Menghindari Ini?", "id": "Ya, Dengan Menggunakan Redundansi." },
    { "en": "Apa Itu 'Graceful Degradation'?", "id": "Sistem Tetap Berfungsi Dengan Kinerja Menurun." },
    { "en": "Contohnya?", "id": "Layar Ponsel Retak Tapi Masih Bisa Digunakan." },
    { "en": "Apa Itu 'Ergonomics' Dalam Desain Alat?", "id": "Membuat Alat Nyaman Dan Aman Digunakan." },
    { "en": "Apa Itu 'User-Centered Design'?", "id": "Proses Desain Yang Berfokus Pada Kebutuhan Pengguna." },
    { "en": "Mengapa Penting Membaca Ulasan Produk?", "id": "Untuk Mengetahui Masalah Umum Yang Dihadapi Pengguna." },
    { "en": "Apa Itu 'Teardown'?", "id": "Membongkar Produk Untuk Melihat Komponen Internalnya." },
    { "en": "Tujuan 'Teardown'?", "id": "Analisis Kompetitif Atau Belajar Teknik Desain." },
    { "en": "Mengapa Dokumentasi Adalah Bagian Kritis?", "id": "Tanpa Dokumentasi, Pengetahuan Akan Hilang." },
    { "en": "Apa Itu 'Tribology'?", "id": "Ilmu Tentang Gesekan, Keausan, Dan Pelumasan." },
    { "en": "Relevansinya?", "id": "Penting Untuk Komponen Mekanis (Saklar, Potensiometer)." },
    { "en": "Apa Itu 'Outgassing'?", "id": "Pelepasan Gas Dari Material." },
    { "en": "Di Mana 'Outgassing' Menjadi Masalah?", "id": "Di Lingkungan Vakum Seperti Luar Angkasa." },
    { "en": "Dampaknya?", "id": "Bisa Mengkontaminasi Lensa Atau Permukaan Sensitif." },
    { "en": "Apa Itu 'Cleanliness' Dalam Perakitan?", "id": "Menghindari Kontaminasi Seperti Minyak Jari." },
    { "en": "Mengapa Minyak Jari Buruk?", "id": "Bisa Menyebabkan Masalah Solderan Dan Korosi." },
    { "en": "Solusinya?", "id": "Gunakan Sarung Tangan Atau Cuci Tangan." },
    { "en": "Apa Itu 'Component Orientation'?", "id": "Arah Pemasangan Komponen." },
    { "en": "Mengapa Orientasi Penting?", "id": "Untuk Komponen Terpolarisasi (Dioda, Kapasitor)." },
    { "en": "Apa Itu 'Silkscreen'?", "id": "Tanda Putih Di PCB." },
    { "en": "Fungsi 'Silkscreen'?", "id": "Menunjukkan Orientasi Dan Nama Komponen." },
    { "en": "Apa Itu 'Pin 1 Indicator'?", "id": "Tanda (Titik/Garis) Untuk Menunjukkan Pin Nomor Satu." },
    { "en": "Pentingnya 'Pin 1 Indicator'?", "id": "Mencegah Pemasangan IC Terbalik." },
    { "en": "Apa Itu 'No-Clean Flux'?", "id": "Fluks Yang Tidak Perlu Dibersihkan." },
    { "en": "Kapan 'No-Clean Flux' Digunakan?", "id": "Dalam Produksi Massal Untuk Menghemat Waktu." },
    { "en": "Apa Itu 'Solderability'?", "id": "Kemudahan Suatu Permukaan Untuk Disolder." },
    { "en": "Faktor Yang Mempengaruhi 'Solderability'?", "id": "Kebersihan Permukaan Dan Jenis Lapisan Pelindung." },
    { "en": "Lapisan Apa Yang Umum Pada PCB?", "id": "HASL, ENIG, OSP." },
    { "en": "Apa Itu HASL?", "id": "Hot Air Solder Leveling (Paling Umum)." },
    { "en": "Apa Itu ENIG?", "id": "Electroless Nickel Immersion Gold (Permukaan Datar)." },
    { "en": "Kapan ENIG Digunakan?", "id": "Untuk Komponen BGA Dan 'Fine Pitch'." },
    { "en": "Apa Itu 'Tenting' Vias?", "id": "Menutupi Lubang Via Dengan Solder Mask." },
    { "en": "Tujuannya?", "id": "Mencegah Solder Terhisap Masuk Ke Lubang." },
    { "en": "Apa Itu 'Thermal Relief Pad'?", "id": "Pola Sambungan Untuk Memudahkan Penyolderan." },
    { "en": "Bagaimana Cara Kerjanya?", "id": "Mengurangi Aliran Panas Ke 'Ground Plane'." },
    { "en": "Apa Itu 'Annular Ring'?", "id": "Cincin Tembaga Di Sekitar Lubang." },
    { "en": "Fungsinya?", "id": "Memastikan Koneksi Yang Baik Setelah Pengeboran." },
    { "en": "Apa Itu 'Teardrop' Pada Jalur?", "id": "Menambahkan Tembaga Di Pertemuan Jalur Dan Pad." },
    { "en": "Tujuannya?", "id": "Meningkatkan Kekuatan Mekanis." },
    { "en": "Mengapa Belajar Dari Kegagalan Itu Penting?", "id": "Karena Memberi Pelajaran Yang Berharga." },
    { "en": "Apa Filosofi Perbaikan?", "id": "Jangan Pernah Menyerah, Selalu Ada Solusi." },
    { "en": "Apa Keterampilan Terpenting Seorang Teknisi?", "id": "Kemampuan 'Troubleshooting' Yang Logis." },
    { "en": "Apa Itu 'Occam's Screwdriver'?", "id": "Candaan: Komponen Paling Rusak Adalah Yang Paling Sulit Dijangkau." },
    { "en": "Apa Itu 'DFM'?", "id": "Design for Manufacturability." },
    { "en": "Apa Itu 'DFT'?", "id": "Design for Testability." },
    { "en": "Apa Itu 'DFA'?", "id": "Design for Assembly." },
    { "en": "Ketiganya Adalah Bagian Dari Apa?", "id": "Desain Produk Yang Baik Dan Efisien." },
    { "en": "Apa Itu 'Rapid Prototyping'?", "id": "Membuat Prototipe Dengan Cepat." },
    { "en": "Teknologi Apa Yang Digunakan?", "id": "Pencetakan 3D Dan Manufaktur PCB Cepat." },
    { "en": "Apa Kunci Terakhir Untuk Semua Masalah?", "id": "Kesabaran Dan Ketekunan." },
    { "en": "Apa Aturan Paling Mendasar Keselamatan Listrik?", "id": "Selalu Anggap Sirkuit Bertegangan Sampai Terbukti Aman." },
    { "en": "Mengapa Alas Kaki Kering Itu Penting?", "id": "Menambah Resistansi Tubuh Terhadap Tanah." },
    { "en": "Apa Itu 'Lockout-Tagout' (LOTO)?", "id": "Prosedur Mengunci Sumber Daya Selama Perbaikan." },
    { "en": "Mengapa LOTO Penting Dalam Industri?", "id": "Mencegah Orang Lain Menyalakan Mesin Secara Sengaja." },
    { "en": "Apa Yang Harus Diperiksa Sebelum Menggunakan Alat Ukur?", "id": "Kondisi Kabel Probe Dan Pengaturan Awal." },
    { "en": "Bahaya Mengukur Resistansi Pada Sirkuit Bertegangan?", "id": "Dapat Merusak Multimeter Secara Permanen." },
    { "en": "Apa Itu Kategori Pengukuran (CAT Rating)?", "id": "Menunjukkan Tingkat Keamanan Multimeter." },
    { "en": "CAT Rating Mana Yang Untuk Elektronika Daya?", "id": "CAT III Atau CAT IV." },
    { "en": "Mengapa Tidak Boleh Mengganti Sekring Dengan Nilai Lebih Tinggi?", "id": "Akan Menghilangkan Perlindungan Terhadap Arus Berlebih." },
    { "en": "Apa Tanda Bahaya Tegangan Tinggi?", "id": "Simbol Petir Kuning Dan Hitam." },
    { "en": "Bagaimana Cara Kerja Trafo Isolasi Untuk Keamanan?", "id": "Memutus Jalur Langsung Ke Ground Jaringan." },
    { "en": "Apa Itu Peringkat IP (Ingress Protection)?", "id": "Menunjukkan Perlindungan Casing Terhadap Debu Dan Air." },
    { "en": "IP54 Berarti Apa?", "id": "Terlindung Dari Debu Dan Cipratan Air." },
    { "en": "Apa Itu Standar UL?", "id": "Standar Keamanan Produk Dari Underwriters Laboratories." },
    { "en": "Apa Itu Tanda CE?", "id": "Menyatakan Kepatuhan Produk Dengan Standar Uni Eropa." },
    { "en": "Apa Itu 'Double Insulation'?", "id": "Peralatan Dengan Dua Tingkat Isolasi." },
    { "en": "Simbol 'Double Insulation'?", "id": "Dua Kotak Yang Saling Bertumpuk." },
    { "en": "Peralatan Ini Membutuhkan Kabel Ground?", "id": "Tidak, Tidak Memerlukan Kabel Ground." },
    { "en": "Mengapa Penting Menjaga Area Kerja Tetap Rapi?", "id": "Mengurangi Risiko Tersandung Dan Hubung Singkat." },
    { "en": "Di Mana Menyimpan Bahan Kimia Mudah Terbakar?", "id": "Di Lemari Khusus Yang Berventilasi." },
    { "en": "Apa Yang Harus Dilakukan Jika Terjadi Kebakaran Listrik?", "id": "Gunakan Alat Pemadam Api Kelas C." },
    { "en": "Mengapa Tidak Boleh Menggunakan Air?", "id": "Air Adalah Konduktor Dan Memperburuk Keadaan." },
    { "en": "Apa Pertolongan Pertama Untuk Sengatan Listrik?", "id": "Putuskan Sumber Listrik Terlebih Dahulu." },
    { "en": "Mengapa Tidak Langsung Menyentuh Korban?", "id": "Anda Juga Bisa Ikut Tersetrum." },
    { "en": "Apa Itu 'Material Safety Data Sheet' (MSDS)?", "id": "Informasi Tentang Bahaya Bahan Kimia." },
    { "en": "Apa Itu 'Fume Hood'?", "id": "Lemari Asam Untuk Bekerja Dengan Bahan Kimia." },
    { "en": "Mengapa Ventilasi Penting Saat Menyolder?", "id": "Untuk Menghilangkan Asap Fluks Yang Berbahaya." },
    { "en": "Bahaya Baterai Lithium Jika Tertusuk?", "id": "Dapat Mengeluarkan Asap Beracun Dan Terbakar." },
    { "en": "Bagaimana Cara Membuang Baterai Bekas?", "id": "Bawa Ke Titik Daur Ulang Limbah Elektronik." },
    { "en": "Mengapa Tidak Boleh Membuangnya Ke Tempat Sampah Biasa?", "id": "Mengandung Bahan Kimia Berbahaya Bagi Lingkungan." },
    { "en": "Apa Itu 'Ergonomics'?", "id": "Studi Tentang Desain Tempat Kerja Yang Efisien." },
    { "en": "Pencahayaan Baik Mengurangi Masalah Apa?", "id": "Kelelahan Mata Dan Kesalahan Perakitan." },
    { "en": "Apa Itu 'Pareto Principle' (Aturan 80/20)?", "id": "Delapan Puluh Persen Masalah Disebabkan Dua Puluh Persen Penyebab." },
    { "en": "Bagaimana Ini Diterapkan Dalam 'Troubleshooting'?", "id": "Fokus Pada Penyebab Paling Umum Terlebih Dahulu." },
    { "en": "Apa Itu 'Cognitive Bias'?", "id": "Pola Pikir Sistematis Yang Menyebabkan Kesalahan." },
    { "en": "Contoh 'Cognitive Bias'?", "id": "'Confirmation Bias' (Mencari Bukti Pendukung)." },
    { "en": "Bagaimana Cara Melawannya?", "id": "Selalu Pertanyakan Asumsi Awal Anda." },
    { "en": "Mengapa Istirahat Singkat Itu Penting?", "id": "Membantu Menjaga Konsentrasi Dan Menemukan Solusi." },
    { "en": "Apa Itu 'Heuristic'?", "id": "Jalan Pintas Mental Atau Aturan Praktis." },
    { "en": "Contoh 'Heuristic' Dalam Diagnostik?", "id": "'Jika Tidak Ada Daya, Periksa Sekring'." },
    { "en": "Apakah 'Heuristic' Selalu Benar?", "id": "Tidak, Tapi Seringkali Merupakan Titik Awal Yang Baik." },
    { "en": "Apa Itu 'Workmanship Standards'?", "id": "Standar Kualitas Untuk Pekerjaan Perakitan." },
    { "en": "Standar IPC-A-610 Untuk Apa?", "id": "Keterterimaan Rakitan Elektronik." },
    { "en": "Apa Yang Dinilai?", "id": "Kualitas Solderan, Pemasangan Komponen, Kebersihan." },
    { "en": "Mengapa Konsistensi Penting Dalam Produksi?", "id": "Memastikan Semua Unit Memiliki Kualitas Sama." },
    { "en": "Apa Itu 'Statistical Process Control' (SPC)?", "id": "Menggunakan Statistik Untuk Memantau Proses." },
    { "en": "Apa Itu 'Six Sigma'?", "id": "Metodologi Untuk Peningkatan Kualitas." },
    { "en": "Tujuannya?", "id": "Mengurangi Cacat Hingga Tingkat Sangat Rendah." },
    { "en": "Apa Itu 'Kaizen'?", "id": "Filosofi Jepang Tentang Perbaikan Berkelanjutan." },
    { "en": "Apa Itu 'Lean Manufacturing'?", "id": "Fokus Pada Penghapusan Pemborosan (Waste)." },
    { "en": "Tujuh Pemborosan Dalam 'Lean'?", "id": "Transportasi, Inventaris, Gerakan, Menunggu, Produksi Berlebih, Proses Berlebih, Cacat." },
    { "en": "Bagaimana Ini Relevan Dengan Elektronika?", "id": "Prinsip Yang Sama Diterapkan Dalam Desain Dan Manufaktur." },
    { "en": "Apa Itu 'Intellectual Property' (IP)?", "id": "Kekayaan Intelektual (Paten, Hak Cipta)." },
    { "en": "Mengapa IP Penting?", "id": "Melindungi Inovasi Dan Desain." },
    { "en": "Apa Itu Paten?", "id": "Hak Eksklusif Atas Sebuah Penemuan." },
    { "en": "Apa Itu Hak Cipta?", "id": "Melindungi Karya Orisinal (Perangkat Lunak, Skema)." },
    { "en": "Apa Itu Merek Dagang?", "id": "Melindungi Nama Atau Logo Produk." },
    { "en": "Apa Itu 'Non-Disclosure Agreement' (NDA)?", "id": "Perjanjian Kerahasiaan." },
    { "en": "Apa Itu 'Reverse Engineering'?", "id": "Membongkar Produk Untuk Mempelajarinya." },
    { "en": "Apakah 'Reverse Engineering' Legal?", "id": "Tergantung Pada Tujuan Dan Hukum Setempat." },
    { "en": "Mengapa Pelabelan Kabel Itu Penting?", "id": "Memudahkan Perbaikan Dan Modifikasi Di Masa Depan." },
    { "en": "Apa Standar Warna Kabel DC?", "id": "Merah Untuk Positif, Hitam Untuk Negatif." },
    { "en": "Apa Itu 'Pinout Diagram'?", "id": "Diagram Yang Menunjukkan Fungsi Setiap Pin." },
    { "en": "Mengapa 'Pinout Diagram' Penting?", "id": "Mencegah Kesalahan Koneksi Yang Fatal." },
    { "en": "Apa Itu 'Failsafe'?", "id": "Gagal Dalam Keadaan Aman." },
    { "en": "Apa Itu 'Fail-Operational'?", "id": "Tetap Beroperasi Meskipun Ada Kegagalan." },
    { "en": "Sistem Mana Yang Perlu 'Fail-Operational'?", "id": "Sistem Kritis (Pesawat Terbang, Reaktor)." },
    { "en": "Apa Itu 'Gage R&R'?", "id": "Gage Repeatability and Reproducibility." },
    { "en": "Untuk Apa Studi Ini?", "id": "Menganalisis Variasi Dalam Sistem Pengukuran." },
    { "en": "Apa Itu 'Repeatability'?", "id": "Variasi Saat Satu Orang Mengukur Berulang Kali." },
    { "en": "Apa Itu 'Reproducibility'?", "id": "Variasi Saat Orang Berbeda Melakukan Pengukuran." },
    { "en": "Sistem Pengukuran Yang Baik Harus Bagaimana?", "id": "Memiliki 'Repeatability' Dan 'Reproducibility' Yang Baik." },
    { "en": "Apa Itu 'Calibration Certificate'?", "id": "Sertifikat Yang Membuktikan Alat Telah Dikalibrasi." },
    { "en": "Apa Itu 'Traceability' Dalam Kalibrasi?", "id": "Kemampuan Melacak Kembali Ke Standar Nasional." },
    { "en": "Mengapa 'Documentation' Penting?", "id": "Merekam Pengetahuan Dan Memfasilitasi Kolaborasi." },
    { "en": "Apa Saja Dokumentasi Proyek Yang Baik?", "id": "Skema, BOM, Kode Sumber, Manual Pengguna." },
    { "en": "Apa Itu 'Change Log'?", "id": "Catatan Perubahan Yang Dibuat Pada Proyek." },
    { "en": "Mengapa Komunikasi Tim Penting?", "id": "Untuk Menghindari Kesalahpahaman Dan Kesalahan." },
    { "en": "Apa Alat Bantu Komunikasi Tim?", "id": "Rapat Rutin, Email, Platform Kolaborasi." },
    { "en": "Apa Itu 'Scope Creep'?", "id": "Penambahan Fitur Di Luar Rencana Awal." },
    { "en": "Masalah 'Scope Creep'?", "id": "Menyebabkan Penundaan Proyek Dan Pembengkakan Biaya." },
    { "en": "Solusinya?", "id": "Manajemen Proyek Yang Baik Dan Komunikasi Jelas." },
    { "en": "Apa Pelajaran Paling Penting Dalam Elektronika?", "id": "Dasar-dasar Adalah Segalanya." },
    { "en": "Contohnya?", "id": "Memahami Hukum Ohm Dan Kirchhoff Adalah Kunci." },
    { "en": "Apa Keterampilan Non-Teknis Yang Penting?", "id": "Pemecahan Masalah, Komunikasi, Dan Keingintahuan." },
    { "en": "Bagaimana Cara Tetap 'Up-to-date'?", "id": "Membaca Jurnal, Blog, Dan Terus Belajar." },
    { "en": "Apakah Gagal Itu Buruk?", "id": "Tidak, Gagal Adalah Kesempatan Untuk Belajar." },
    { "en": "Filosofi Seorang Insinyur?", "id": "Selalu Ada Cara Yang Lebih Baik." },
    { "en": "Apa Itu 'KISS Principle'?", "id": "'Keep It Simple, Stupid'." },
    { "en": "Mengapa Desain Sederhana Itu Baik?", "id": "Lebih Andal, Lebih Murah, Lebih Mudah Diperbaiki." },
    { "en": "Apa Langkah Terakhir Setelah Perbaikan?", "id": "Verifikasi Bahwa Semuanya Bekerja Dengan Benar." },
    { "en": "Dan Apa Lagi?", "id": "Dokumentasikan Perbaikan Yang Telah Dilakukan." }



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
