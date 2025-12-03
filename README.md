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


    { "en": "Berapa Ketinggian Orbit Geostasioner?", "id": "35.786 Kilometer Di Atas Khatulistiwa." },
    { "en": "Berapa Ketinggian Orbit LEO?", "id": "Di Bawah 2000 Kilometer." },
    { "en": "Apa Lebar Pita Transponder Umum?", "id": "36 MHz Atau 72 MHz." },
    { "en": "Apa Itu Uplink Frequency?", "id": "Frekuensi Dari Bumi Ke Satelit." },
    { "en": "Apa Itu Downlink Frequency?", "id": "Frekuensi Dari Satelit Ke Bumi." },
    { "en": "Apa Itu Cross-Polarization Interference?", "id": "Sinyal Bocor Antar Polarisasi Tegak Lurus." },
    { "en": "Apa Itu Sun Outage?", "id": "Matahari Di Belakang Satelit Ganggu Sinyal." },
    { "en": "Apa Itu Radar Cross Section?", "id": "Ukuran Pantulan Sinyal Target Radar." },
    { "en": "Apa Itu Efek Doppler Radar?", "id": "Pergeseran Frekuensi Akibat Gerakan Objek." },
    { "en": "Apa Itu Pulse Repetition Frequency?", "id": "Jumlah Pulsa Radar Per Detik." },
    { "en": "Apa Itu FMCW Radar?", "id": "Radar Gelombang Kontinyu Modulasi Frekuensi." },
    { "en": "Apa Itu Phased Array Radar?", "id": "Arahkan Sinyal Elektronik Tanpa Gerak." },
    { "en": "Apa Prinsip Dasar CT Scan?", "id": "Sinar-X Berputar Hasilkan Potongan Gambar." },
    { "en": "Apa Satuan Hounsfield Unit?", "id": "Skala Kepadatan Radiologi CT Scan." },
    { "en": "Apa Prinsip Dasar MRI?", "id": "Resonansi Magnetik Inti Atom Hidrogen." },
    { "en": "Apa Itu MRI T1 Weighted?", "id": "Kontras Berdasarkan Waktu Relaksasi T1." },
    { "en": "Apa Itu MRI T2 Weighted?", "id": "Kontras Berdasarkan Waktu Relaksasi T2." },
    { "en": "Apa Itu Piezoelectric Ultrasound?", "id": "Kristal Getar Hasilkan Gelombang Suara." },
    { "en": "Apa Itu USG Doppler?", "id": "Ukur Kecepatan Aliran Darah Pasien." },
    { "en": "Apa Itu Gelombang Otak Alpha?", "id": "Frekuensi 8 Hingga 12 Hertz." },
    { "en": "Apa Itu Gelombang Otak Delta?", "id": "Frekuensi 0.5 Hingga 4 Hertz." },
    { "en": "Apa Itu Defibrilator Bifasik?", "id": "Arus Kejut Mengalir Dua Arah." },
    { "en": "Apa Itu Defibrilator Monofasik?", "id": "Arus Kejut Mengalir Satu Arah." },
    { "en": "Apa Parameter H Swing Equation?", "id": "Konstanta Inersia Generator Listrik." },
    { "en": "Apa Itu Critical Clearing Angle?", "id": "Sudut Maksimum Sebelum Hilang Sinkronisasi." },
    { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Grafis Analisis Kestabilan Transien." },
    { "en": "Apa Itu Transient Stability?", "id": "Kestabilan Saat Gangguan Besar Mendadak." },
    { "en": "Apa Itu Steady State Stability?", "id": "Kestabilan Saat Perubahan Beban Kecil." },
    { "en": "Apa Itu Voltage Collapse?", "id": "Tegangan Jatuh Total Akibat Reaktif Kurang." },
    { "en": "Apa Kurva PV Sistem Tenaga?", "id": "Grafik Hubungan Daya Aktif Tegangan." },
    { "en": "Apa Kurva QV Sistem Tenaga?", "id": "Grafik Hubungan Daya Reaktif Tegangan." },
    { "en": "Apa Itu Load Shedding Frekuensi?", "id": "Lepas Beban Saat Frekuensi Anjlok." },
    { "en": "Apa Itu ROCOF Relay?", "id": "Deteksi Laju Perubahan Frekuensi Cepat." },
    { "en": "Apa Itu Vector Shift Relay?", "id": "Deteksi Lompatan Sudut Fasa Tegangan." },
    { "en": "Apa Itu LVRT (Low Voltage Ride Through)?", "id": "Generator Bertahan Saat Tegangan Kedip." },
    { "en": "Apa Itu Inersia Grid?", "id": "Energi Kinetik Massa Putar Generator." },
    { "en": "Apa Itu Virtual Inertia?", "id": "Inverter Tiru Sifat Massa Putar." },
    { "en": "Apa Algoritma Perturb And Observe?", "id": "Metode MPPT Cari Daya Puncak." },
    { "en": "Apa Itu Fill Factor Solar Cell?", "id": "Ukuran Kualitas Kurva IV Panel." },
    { "en": "Apa Itu Coulomb Counting Baterai?", "id": "Hitung Muatan Masuk Keluar Baterai." },
    { "en": "Apa Itu Kalman Filter Baterai?", "id": "Estimasi SoC Berdasarkan Model Matematis." },
    { "en": "Apa Itu Hub Ethernet?", "id": "Teruskan Data Ke Semua Port." },
    { "en": "Apa Itu Network Bridge?", "id": "Hubungkan Dua Segmen Jaringan Berbeda." },
    { "en": "Apa Itu Layer 3 Switch?", "id": "Switch Dengan Kemampuan Routing IP." },
    { "en": "Apa Itu ARP Request?", "id": "Tanya Siapa Pemilik IP Ini." },
    { "en": "Apa Itu ARP Reply?", "id": "Jawaban Ini MAC Address Saya." },
    { "en": "Apa Itu MTU Ethernet Standar?", "id": "1500 Byte Per Frame Data." },
    { "en": "Apa Itu Jumbo Frame Ethernet?", "id": "Frame Ethernet Lebih Dari 1500 Byte." },
    { "en": "Apa Itu TCP Three-way Handshake?", "id": "Proses Awal Buat Koneksi TCP." },
    { "en": "Apa Itu SYN Packet?", "id": "Paket Permintaan Koneksi TCP Awal." },
    { "en": "Apa Itu ACK Packet?", "id": "Paket Tanda Terima Data TCP." },
    { "en": "Apa Itu Protokol UDP?", "id": "Kirim Data Cepat Tanpa Konfirmasi." },
    { "en": "Apa Itu Port 8080?", "id": "Port Alternatif Untuk Web Server." },
    { "en": "Apa Itu Port 8443?", "id": "Port Alternatif Untuk HTTPS Aman." },
    { "en": "Apa Itu CIDR Notation?", "id": "Penulisan Subnet Mask Garis Miring." },
    { "en": "Apa Itu Loopback Interface?", "id": "Antarmuka Virtual Untuk Tes Diri." },
    { "en": "Apa Itu Native VLAN?", "id": "VLAN Tanpa Tag Di Jalur Trunk." },
    { "en": "Apa Itu Routing Table?", "id": "Peta Jalur Menuju Jaringan Lain." },
    { "en": "Apa Itu Static Routing?", "id": "Jalur Ditentukan Manual Oleh Admin." },
    { "en": "Apa Itu Dynamic Routing?", "id": "Jalur Ditentukan Otomatis Oleh Protokol." },
    { "en": "Apa Itu RIP Protocol?", "id": "Protokol Routing Berdasarkan Jumlah Lompatan." },
    { "en": "Apa Itu OSPF Protocol?", "id": "Protokol Routing Berdasarkan Status Link." },
    { "en": "Apa Itu BGP Protocol?", "id": "Protokol Routing Inti Jaringan Internet." },
    { "en": "Apa Itu NAT Overload (PAT)?", "id": "Banyak IP Lokal Satu IP Publik." },
    { "en": "Apa Itu Port Forwarding?", "id": "Buka Akses Dari Luar Ke Dalam." },
    { "en": "Apa Itu DMZ Host?", "id": "Komputer Terbuka Penuh Ke Internet." },
    { "en": "Apa Itu VPN Site-to-Site?", "id": "Hubungkan Dua Kantor Lewat Internet." },
    { "en": "Apa Itu VPN Client-to-Site?", "id": "User Remote Akses Jaringan Kantor." },
    { "en": "Apa Itu Fuzzification?", "id": "Ubah Nilai Tegas Jadi Fuzzy." },
    { "en": "Apa Itu Defuzzification?", "id": "Ubah Output Fuzzy Jadi Nilai Tegas." },
    { "en": "Apa Itu Neural Network Neuron?", "id": "Unit Pemroses Tiru Sel Otak." },
    { "en": "Apa Itu Activation Function?", "id": "Penentu Output Neuron Aktif Tidak." },
    { "en": "Apa Itu Sigmoid Function?", "id": "Fungsi Aktivasi Kurva S 0-1." },
    { "en": "Apa Itu ReLU Function?", "id": "Fungsi Aktivasi Linear Positif Saja." },
    { "en": "Apa Itu Training Data AI?", "id": "Data Untuk Melatih Model AI." },
    { "en": "Apa Itu Testing Data AI?", "id": "Data Untuk Uji Akurasi Model." },
    { "en": "Apa Itu Supervised Learning?", "id": "Belajar Dari Data Berlabel." },
    { "en": "Apa Itu Unsupervised Learning?", "id": "Cari Pola Dari Data Tanpa Label." },
    { "en": "Apa Itu Reinforcement Learning?", "id": "Belajar Dari Hadiah Dan Hukuman." },
    { "en": "Apa Itu Convolutional Neural Network?", "id": "AI Khusus Olah Gambar Visual." },
    { "en": "Apa Itu Recurrent Neural Network?", "id": "AI Khusus Data Urutan Waktu." },
    { "en": "Apa Itu Fog Computing?", "id": "Olah Data Antara Edge Dan Cloud." },
    { "en": "Apa Itu Quantum Bit (Qubit)?", "id": "Unit Dasar Informasi Komputer Kuantum." },
    { "en": "Apa Itu Superposition Kuantum?", "id": "Qubit Bisa 0 Dan 1 Bersamaan." },
    { "en": "Apa Itu Entanglement Kuantum?", "id": "Dua Qubit Saling Terhubung Jarak Jauh." },
    { "en": "Apa Itu Cryogenic Cooling?", "id": "Pendinginan Ekstrem Untuk Superkonduktor/Kuantum." },
    { "en": "Apa Itu Josephson Junction?", "id": "Komponen Kuantum Dua Superkonduktor Pisah." },
    { "en": "Apa Itu SQUID Sensor?", "id": "Sensor Medan Magnet Paling Sensitif." },
    { "en": "Apa Itu MEMS Accelerometer?", "id": "Sensor Percepatan Mekanis Ukuran Chip." },
    { "en": "Apa Itu MEMS Gyroscope?", "id": "Sensor Rotasi Mekanis Ukuran Chip." },
    { "en": "Apa Itu Sensor DLP?", "id": "Cermin Mikro Untuk Proyektor Digital." },
    { "en": "Apa Itu E-Ink Display?", "id": "Layar Kertas Elektronik Hemat Daya." },
    { "en": "Apa Keunggulan E-Ink?", "id": "Hanya Pakai Daya Saat Ganti Gambar." },
    { "en": "Apa Itu Inductive Coupling?", "id": "Transfer Energi Lewat Medan Magnet." },
    { "en": "Apa Itu Resonant Inductive Coupling?", "id": "Transfer Energi Jarak Menengah Efisien." },
    { "en": "Apa Itu Energy Harvesting?", "id": "Panen Energi Kecil Dari Lingkungan." },
    { "en": "Apa Itu Piezoelectric Harvesting?", "id": "Panen Listrik Dari Getaran Mekanis." },
    { "en": "Apa Itu Thermoelectric Harvesting?", "id": "Panen Listrik Dari Beda Suhu." },
    { "en": "Apa Itu RF Harvesting?", "id": "Panen Listrik Dari Sinyal Radio." },
    { "en": "Apa Itu Smart Meter AMI?", "id": "Meteran Listrik Komunikasi Dua Arah." },
    { "en": "Apa Itu WiFi 6 (802.11ax)?", "id": "Standar WiFi Cepat Efisiensi Tinggi." },
    { "en": "Apa Keunggulan WiFi 6?", "id": "Kapasitas Lebih Besar Latensi Rendah." },
    { "en": "Apa Itu OFDMA WiFi 6?", "id": "Bagi Saluran Jadi Sub-carrier Kecil." },
    { "en": "Apa Itu MU-MIMO?", "id": "Multi User Multiple Input Output." },
    { "en": "Apa Itu TWT (Target Wake Time)?", "id": "Jadwal Tidur Perangkat Hemat Baterai." },
    { "en": "Apa Itu BSS Coloring?", "id": "Kurangi Interferensi WiFi Tetangga Padat." },
    { "en": "Apa Itu WPA3 Security?", "id": "Enkripsi WiFi Lebih Kuat SAE." },
    { "en": "Apa Itu WiFi Mesh System?", "id": "Jaringan WiFi Tanpa Putus Rumah." },
    { "en": "Apa Itu Band Steering WiFi?", "id": "Otomatis Pilih Frekuensi Terbaik 2.4/5GHz." },
    { "en": "Apa Itu SSID Hiding?", "id": "Sembunyikan Nama Jaringan Dari Daftar." },
    { "en": "Apa Itu 5G NSA (Non-Standalone)?", "id": "Jaringan 5G Nebeng Core 4G." },
    { "en": "Apa Itu 5G SA (Standalone)?", "id": "Jaringan 5G Murni Core Baru." },
    { "en": "Apa Itu Millimeter Wave 5G?", "id": "Frekuensi Sangat Tinggi Jarak Pendek." },
    { "en": "Apa Itu Sub-6 GHz 5G?", "id": "Frekuensi Menengah Jangkauan Luas." },
    { "en": "Apa Itu Massive MIMO?", "id": "Antena Sangat Banyak Di BTS." },
    { "en": "Apa Itu Network Slicing 5G?", "id": "Jaringan Virtual Khusus Tiap Aplikasi." },
    { "en": "Apa Itu Latensi 5G URLLC?", "id": "Latensi Ultra Rendah Di Bawah 1ms." },
    { "en": "Apa Itu CNC G-Code G00?", "id": "Gerakan Cepat Tanpa Memotong." },
    { "en": "Apa Itu CNC G-Code G01?", "id": "Gerakan Potong Garis Lurus Terkontrol." },
    { "en": "Apa Itu CNC G-Code G02?", "id": "Gerakan Potong Melingkar Searah Jam." },
    { "en": "Apa Itu CNC G-Code G03?", "id": "Gerakan Potong Melingkar Lawan Jam." },
    { "en": "Apa Itu CNC M-Code M03?", "id": "Nyalakan Spindle Putar Searah Jam." },
    { "en": "Apa Itu CNC M-Code M05?", "id": "Matikan Putaran Spindle." },
    { "en": "Apa Itu CNC Homing Cycle?", "id": "Mesin Kembali Ke Titik Nol." },
    { "en": "Apa Itu Spindle Runout?", "id": "Penyimpangan Putaran Poros Mesin." },
    { "en": "Apa Itu Stepper Motor NEMA 23?", "id": "Motor Standar Mesin CNC Kecil." },
    { "en": "Apa Fungsi Ballscrew CNC?", "id": "Ubah Putaran Jadi Gerak Linear." },
    { "en": "Apa Kelebihan Ballscrew Dibanding Lead Screw?", "id": "Gesekan Rendah Presisi Tinggi." },
    { "en": "Apa Itu Limit Switch CNC?", "id": "Batas Fisik Gerakan Sumbu Mesin." },
    { "en": "Apa Itu Probe Z-Axis?", "id": "Sensor Ukur Tinggi Mata Pisau." },
    { "en": "Apa Itu HDMI CEC?", "id": "Satu Remote Kontrol Banyak Alat." },
    { "en": "Apa Itu HDMI ARC?", "id": "Audio Return Channel Ke Speaker." },
    { "en": "Apa Itu HDMI eARC?", "id": "Enhanced ARC Audio Kualitas Tinggi." },
    { "en": "Apa Itu HDCP 2.2?", "id": "Proteksi Konten Digital Resolusi 4K." },
    { "en": "Apa Itu EDID HDMI?", "id": "Data Identitas Layar Ke Sumber." },
    { "en": "Apa Itu Refresh Rate 144Hz?", "id": "Gambar Diperbarui 144 Kali Sedetik." },
    { "en": "Apa Itu HDR10 Display?", "id": "High Dynamic Range 10 Bit." },
    { "en": "Apa Itu Dolby Vision?", "id": "HDR Dinamis Metadata Per Frame." },
    { "en": "Apa Itu Chroma Subsampling 4:4:4?", "id": "Warna Penuh Tanpa Kompresi." },
    { "en": "Apa Itu DisplayPort DSC?", "id": "Kompresi Stream Tampilan Tanpa Rugi." },
    { "en": "Apa Itu Konduktor Super (Superconductor)?", "id": "Hambatan Nol Pada Suhu Kritis." },
    { "en": "Apa Itu Efek Meissner?", "id": "Superkonduktor Menolak Medan Magnet (Melayang)." },
    { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Kelebihan Hole (Muatan Positif)." },
    { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Kelebihan Elektron (Muatan Negatif)." },
    { "en": "Apa Itu PN Junction?", "id": "Pertemuan Tipe P Dan N." },
    { "en": "Apa Itu Depletion Layer?", "id": "Daerah Kosong Muatan Di Sambungan." },
    { "en": "Apa Itu Breakdown Voltage?", "id": "Tegangan Tembus Isolator Atau Dioda." },
    { "en": "Apa Itu Dielectric Strength?", "id": "Kuat Medan Listrik Maksimal Isolator." },
    { "en": "Apa Itu Permeabilitas Magnetik?", "id": "Kemampuan Bahan Alirkan Fluks Magnet." },
    { "en": "Apa Itu Histeresis Magnetik?", "id": "Energi Hilang Saat Magnetisasi Berulang." },
    { "en": "Apa Itu Suhu Curie?", "id": "Suhu Magnet Kehilangan Sifat Kemagnetan." },
    { "en": "Apa Itu Efek Hall?", "id": "Tegangan Muncul Akibat Medan Magnet." },
    { "en": "Apa Itu Efek Piezoelektrik?", "id": "Tekanan Hasilkan Listrik Pada Kristal." },
    { "en": "Apa Itu Efek Pyroelektrik?", "id": "Panas Hasilkan Listrik Pada Kristal." },
    { "en": "Apa Itu Efek Termoelektrik?", "id": "Beda Suhu Hasilkan Beda Tegangan." },
    { "en": "Apa Itu Seebeck Coefficient?", "id": "Ukuran Sensitivitas Termoelektrik Material." },
    { "en": "Apa Itu VRLA Battery Gel?", "id": "Aki Kering Elektrolit Gel Silika." },
    { "en": "Apa Itu VRLA Battery AGM?", "id": "Aki Kering Separator Serat Kaca." },
    { "en": "Apa Itu CCA (Cold Cranking Amps)?", "id": "Arus Start Maksimal Suhu Dingin." },
    { "en": "Apa Itu Reserve Capacity Aki?", "id": "Waktu Aki Bertahan Tanpa Alternator." },
    { "en": "Apa Tegangan Floating Aki 12V?", "id": "Sekitar 13.5 Hingga 13.8 Volt." },
    { "en": "Apa Tegangan Equalizing Aki 12V?", "id": "Sekitar 14.4 Hingga 15.5 Volt." },
    { "en": "Apa Itu Sulfasi Aki?", "id": "Kerak Putih Timbal Sulfat Elektroda." },
    { "en": "Apa Penyebab Aki Kembung?", "id": "Overcharging Panas Gas Terjebak." },
    { "en": "Apa Fungsi Hydrometer Aki?", "id": "Ukur Berat Jenis Air Zuur." },
    { "en": "Apa Berat Jenis Air Aki Penuh?", "id": "Sekitar 1.265 Hingga 1.280." },
    { "en": "Apa Itu Lithium Iron Phosphate (LFP)?", "id": "Baterai LiFePO4 Aman Dan Awet." },
    { "en": "Apa Tegangan Nominal LFP?", "id": "3.2 Volt Per Sel." },
    { "en": "Apa Itu Lithium Titanate (LTO)?", "id": "Baterai Super Cepat Cas Awet." },
    { "en": "Apa Tegangan Nominal LTO?", "id": "2.4 Volt Per Sel." },
    { "en": "Apa Itu Solid State Relay DC?", "id": "SSR Output DC Pakai MOSFET." },
    { "en": "Apa Itu Solid State Relay AC?", "id": "SSR Output AC Pakai Triac." },
    { "en": "Apa Itu Zero Cross SSR?", "id": "Nyala Saat Gelombang AC Nol." },
    { "en": "Apa Itu Random Turn-on SSR?", "id": "Nyala Seketika Sinyal Masuk." },
    { "en": "Apa Fungsi Heatsink SSR?", "id": "Wajib Buang Panas Saat Beban." },
    { "en": "Apa Itu Motor DC Brushless (BLDC)?", "id": "Motor Tanpa Sikat Efisiensi Tinggi." },
    { "en": "Apa Itu Motor DC Brushed?", "id": "Motor Pakai Sikat Karbon Murah." },
    { "en": "Apa Kelemahan Motor Brushed?", "id": "Sikat Habis Dan Ada Percikan." },
    { "en": "Apa Kelebihan Motor BLDC?", "id": "Awet, Hening, Torsi Tinggi." },
    { "en": "Apa Fungsi Hall Sensor BLDC?", "id": "Deteksi Posisi Rotor Untuk Komutasi." },
    { "en": "Apa Itu Sensorless BLDC?", "id": "Deteksi Posisi Lewat Back EMF." },
    { "en": "Apa Itu kV Rating Motor?", "id": "RPM Per Volt Tanpa Beban." },
    { "en": "Apa Itu Torsi Cogging?", "id": "Gerakan Patah-patah Saat Diputar Tangan." },
    { "en": "Apa Itu Slotless Stator BLDC?", "id": "Stator Tanpa Gigi Putaran Halus." },
    { "en": "Apa Itu Coreless DC Motor?", "id": "Rotor Tanpa Besi Sangat Ringan." },
    { "en": "Apa Aplikasi Coreless Motor?", "id": "Drone Mikro Dan Robot Kecil." },
    { "en": "Apa Itu Kabel NYMHY?", "id": "Kabel Serabut Putih Fleksibel." },
    { "en": "Apa Itu Kabel NYYHY?", "id": "Kabel Serabut Hitam Kuat Outdoor." },
    { "en": "Apa Itu Kabel Coaxial RG-11?", "id": "Kabel CCTV Jarak Jauh Tebal." },
    { "en": "Apa Itu Kabel Twinax SFP+?", "id": "Kabel Tembaga 10Gbps Jarak Pendek." },
    { "en": "Apa Itu Kabel Fiber OM5?", "id": "Fiber Multimode Lime Green Terbaru." },
    { "en": "Apa Itu Kabel Fiber OS2?", "id": "Fiber Single Mode Outdoor Kuning." },
    { "en": "Apa Fungsi Kabel Grounding Hijau?", "id": "Keselamatan Buang Arus Ke Tanah." },
    { "en": "Apa Itu Busbar Sisir MCB?", "id": "Plat Tembaga Jumper MCB Rapi." },
    { "en": "Apa Itu Terminal Block Rail?", "id": "Terminal Kabel Pasang Di Rel." },
    { "en": "Apa Fungsi End Stopper?", "id": "Pengunci Komponen Di DIN Rail." },
    { "en": "Apa Itu Cable Marker?", "id": "Label Nomor Penanda Ujung Kabel." },
    { "en": "Apa Itu Spiral Wrapping Band?", "id": "Pelindung Kabel Spiral Fleksibel." },
    { "en": "Apa Fungsi Cable Gland PG?", "id": "Kunci Kabel Masuk Panel Rapat." },
    { "en": "Apa Itu Panel Maker?", "id": "Teknisi Perakit Panel Listrik." },
    { "en": "Apa Itu WiFi 7 (802.11be)?", "id": "Standar WiFi Kecepatan Ekstrem Terbaru." },
    { "en": "Apa Itu MLO (Multi-Link Operation)?", "id": "Kirim Data Lewat Banyak Pita Frekuensi." },
    { "en": "Apa Itu 4096-QAM?", "id": "Modulasi Data WiFi 7 Sangat Padat." },
    { "en": "Apa Lebar Pita Saluran WiFi 7?", "id": "Hingga 320 Mega Hertz." },
    { "en": "Apa Itu Puncuring WiFi?", "id": "Gunakan Saluran Yang Terganggu Sebagian." },
    { "en": "Apa Itu Latensi Deterministik?", "id": "Waktu Tunda Yang Pasti Dan Rendah." },
    { "en": "Apa Itu IPv6 Link-Local?", "id": "Alamat Otomatis Komunikasi Jaringan Lokal." },
    { "en": "Apa Itu IPv6 Global Unicast?", "id": "Alamat Publik Bisa Akses Internet." },
    { "en": "Apa Itu IPv6 Anycast?", "id": "Satu Alamat Menuju Node Terdekat." },
    { "en": "Apa Itu SLAAC IPv6?", "id": "Konfigurasi Alamat Otomatis Tanpa Server." },
    { "en": "Apa Itu Broadcast Domain?", "id": "Area Jaringan Menerima Paket Broadcast." },
    { "en": "Apa Itu Collision Domain?", "id": "Area Potensi Tabrakan Data Ethernet." },
    { "en": "Apa Fungsi CSMA/CA WiFi?", "id": "Cek Saluran Kosong Sebelum Kirim." },
    { "en": "Apa Fungsi CSMA/CD Ethernet?", "id": "Deteksi Tabrakan Dan Kirim Ulang." },
    { "en": "Apa Itu Proportional Gain (Kp)?", "id": "Respon Langsung Terhadap Besar Error." },
    { "en": "Apa Itu Integral Gain (Ki)?", "id": "Koreksi Akumulasi Error Seiring Waktu." },
    { "en": "Apa Itu Derivative Gain (Kd)?", "id": "Respon Terhadap Laju Perubahan Error." },
    { "en": "Apa Itu Cohen-Coon Method?", "id": "Metode Tuning PID Respon Cepat." },
    { "en": "Apa Itu Zigler-Nichols Method?", "id": "Metode Tuning PID Berbasis Osilasi." },
    { "en": "Apa Itu Cascade Control Loop?", "id": "Satu Kontroler Mengatur Setpoint Lainnya." },
    { "en": "Apa Itu Feedforward Control?", "id": "Antisipasi Gangguan Sebelum Pengaruhi Proses." },
    { "en": "Apa Itu Dead Time Process?", "id": "Waktu Tunda Respon Sistem Fisik." },
    { "en": "Apa Itu Process Lag?", "id": "Kelambatan Perubahan Output Terhadap Input." },
    { "en": "Apa Itu Hysteresis Control?", "id": "Kontrol On-Off Dengan Batas Atas Bawah." },
    { "en": "Apa Itu Litz Wire?", "id": "Kawat Serabut Terisolasi Kurangi Skin-Effect." },
    { "en": "Apa Fungsi Litz Wire Trafo?", "id": "Efisiensi Tinggi Pada Frekuensi Tinggi." },
    { "en": "Apa Itu Planar Transformer?", "id": "Trafo Pipih Menggunakan Jalur PCB." },
    { "en": "Apa Itu Air Core Coil?", "id": "Induktor Tanpa Inti Besi (Udara)." },
    { "en": "Apa Itu Ferrite Core?", "id": "Inti Keramik Magnetik Frekuensi Tinggi." },
    { "en": "Apa Itu Powdered Iron Core?", "id": "Inti Serbuk Besi Tahan Saturasi." },
    { "en": "Apa Itu Permalloy Core?", "id": "Paduan Besi Nikel Permeabilitas Tinggi." },
    { "en": "Apa Itu Amorphous Core?", "id": "Logam Kaca Magnetik Rugi Rendah." },
    { "en": "Apa Itu Toroidal Core?", "id": "Inti Bentuk Cincin Efisiensi Terbaik." },
    { "en": "Apa Itu Bobbin Trafo?", "id": "Kerangka Plastik Tempat Gulung Kawat." },
    { "en": "Apa Itu Pirani Gauge?", "id": "Pengukur Vakum Konduktivitas Termal Kawat." },
    { "en": "Apa Itu Ionization Gauge?", "id": "Pengukur Vakum Tinggi Ionisasi Gas." },
    { "en": "Apa Itu McLeod Gauge?", "id": "Pengukur Vakum Referensi Air Raksa." },
    { "en": "Apa Itu Thermopile Sensor?", "id": "Gabungan Banyak Termokopel Deteksi Panas." },
    { "en": "Apa Itu Bolometer?", "id": "Sensor Radiasi Elektromagnetik Atau Panas." },
    { "en": "Apa Itu Pyroelectric Sensor?", "id": "Hasilkan Listrik Saat Suhu Berubah." },
    { "en": "Apa Itu Hall Effect Vane?", "id": "Sensor Posisi Celah Logam Berputar." },
    { "en": "Apa Itu Magnetostrictive Sensor?", "id": "Ukur Posisi Menggunakan Gelombang Ultrasonik." },
    { "en": "Apa Itu Fiber Bragg Grating?", "id": "Sensor Regangan Optik Dalam Fiber." },
    { "en": "Apa Itu Field Weakening Motor?", "id": "Kurangi Fluks Agar Putaran Naik." },
    { "en": "Apa Itu Constant Torque Region?", "id": "Torsi Tetap Hingga Kecepatan Dasar." },
    { "en": "Apa Itu Constant Power Region?", "id": "Daya Tetap Di Atas Kecepatan Dasar." },
    { "en": "Apa Itu Cogging Torque?", "id": "Gaya Magnetik Tarik Rotor Diam." },
    { "en": "Apa Itu Ripple Torque?", "id": "Fluktuasi Torsi Saat Motor Berputar." },
    { "en": "Apa Itu Regenerative Braking Unit?", "id": "Kembalikan Energi Rem Ke Jaringan." },
    { "en": "Apa Itu Dynamic Braking Unit?", "id": "Buang Energi Rem Ke Resistor." },
    { "en": "Apa Itu Resolver Motor?", "id": "Sensor Posisi Motor Tahan Panas." },
    { "en": "Apa Standar IEC 61131-3?", "id": "Standar Bahasa Pemrograman PLC Global." },
    { "en": "Apa Standar IEC 60068?", "id": "Standar Pengujian Lingkungan Produk Elektronik." },
    { "en": "Apa Standar IPC-A-610?", "id": "Standar Kualitas Perakitan PCB Elektronik." },
    { "en": "Apa Standar IPC-7711/7721?", "id": "Standar Perbaikan Dan Rework PCB." },
    { "en": "Apa Standar ISO 9001?", "id": "Sistem Manajemen Mutu Internasional." },
    { "en": "Apa Standar ISO 14001?", "id": "Sistem Manajemen Lingkungan Internasional." },
    { "en": "Apa Standar ISO 45001?", "id": "Sistem Manajemen Kesehatan Keselamatan Kerja." },
    { "en": "Apa Itu Six Sigma?", "id": "Metode Perbaikan Kualitas Kurangi Cacat." },
    { "en": "Apa Itu Kaizen?", "id": "Perbaikan Berkelanjutan Terus Menerus." },
    { "en": "Apa Itu Poka-Yoke?", "id": "Desain Anti Salah Pasang (Foolproof)." },
    { "en": "Apa Itu 5S (Seiri Seiton)?", "id": "Metode Penataan Tempat Kerja Efisien." },
    { "en": "Apa Itu Kanban System?", "id": "Sistem Jadwal Produksi Tepat Waktu." },
    { "en": "Apa Itu Just In Time (JIT)?", "id": "Produksi Sesuai Permintaan Tanpa Stok." },
    { "en": "Apa Itu BOM (Bill Of Materials)?", "id": "Daftar Komponen Dan Bahan Produk." },
    { "en": "Apa Itu MOQ (Minimum Order Quantity)?", "id": "Jumlah Minimal Pemesanan Barang." },
    { "en": "Apa Itu Lead Time?", "id": "Waktu Tunggu Pemesanan Hingga Datang." },
    { "en": "Apa Itu Cycle Time?", "id": "Waktu Proses Satu Unit Produk." },
    { "en": "Apa Itu Takt Time?", "id": "Kecepatan Produksi Sesuai Permintaan Pelanggan." },
    { "en": "Apa Itu Yield Rate?", "id": "Persentase Produk Bagus Tanpa Cacat." },
    { "en": "Apa Itu Defect Rate?", "id": "Persentase Produk Cacat Per Batch." },
    { "en": "Apa Itu Rework Station?", "id": "Meja Perbaikan Produk Gagal." },
    { "en": "Apa Itu Test Jig?", "id": "Alat Bantu Pengujian Produk Massal." },
    { "en": "Apa Itu Bed of Nails Fixture?", "id": "Alat Tes PCB Jarum Pegas." },
    { "en": "Apa Itu Pogo Pin?", "id": "Jarum Kontak Pegas Untuk Testing." },
    { "en": "Apa Itu Burn-in Chamber?", "id": "Oven Uji Ketahanan Panas Produk." },
    { "en": "Apa Itu Climatic Chamber?", "id": "Ruang Uji Suhu Dan Kelembapan." },
    { "en": "Apa Itu Vibration Shaker?", "id": "Mesin Uji Ketahanan Getaran Produk." },
    { "en": "Apa Itu Drop Test Machine?", "id": "Mesin Uji Jatuh Kemasan Produk." },
    { "en": "Apa Itu Salt Spray Chamber?", "id": "Ruang Uji Korosi Kabut Garam." },
    { "en": "Apa Itu Anechoic Chamber?", "id": "Ruang Kedap Suara Atau Gelombang." },
    { "en": "Apa Itu Faraday Cage?", "id": "Ruang Kedap Sinyal Elektromagnetik." },
    { "en": "Apa Itu Clean Room Class 100?", "id": "Ruang Sangat Bersih Produksi Chip." },
    { "en": "Apa Itu HEPA Filter?", "id": "Penyaring Udara Partikel Sangat Halus." },
    { "en": "Apa Itu Laminar Air Flow?", "id": "Aliran Udara Stabil Tanpa Turbulensi." },
    { "en": "Apa Itu Air Shower?", "id": "Penyemprot Debu Sebelum Masuk Cleanroom." },
    { "en": "Apa Itu Sticky Mat?", "id": "Karpet Lengket Penangkap Debu Sepatu." },
    { "en": "Apa Itu ESD Smock?", "id": "Baju Kerja Anti Listrik Statis." },
    { "en": "Apa Itu Finger Cots?", "id": "Sarung Jari Karet Pelindung Komponen." },
    { "en": "Apa Itu Wrist Strap Tester?", "id": "Alat Cek Gelang Grounding Berfungsi." },
    { "en": "Apa Itu Surface Resistivity Meter?", "id": "Ukur Tahanan Permukaan Meja ESD." },
    { "en": "Apa Itu Ionizer Blower?", "id": "Kipas Penghilang Muatan Listrik Statis." },
    { "en": "Apa Itu Nitrogen Reflow?", "id": "Solder Oven Dengan Gas Nitrogen." },
    { "en": "Apa Keuntungan Nitrogen Reflow?", "id": "Kurangi Oksidasi Hasil Solder Mengkilap." },
    { "en": "Apa Itu Vapor Phase Reflow?", "id": "Pemanasan Menggunakan Uap Cairan Kimia." },
    { "en": "Apa Itu Selective Soldering?", "id": "Mesin Solder Otomatis Titik Tertentu." },
    { "en": "Apa Itu Conformal Coating Spray?", "id": "Semprotan Pelindung PCB Anti Lembap." },
    { "en": "Apa Itu UV Curing?", "id": "Pengerasan Lem/Coating Sinar UV." },
    { "en": "Apa Itu X-Ray Inspection System?", "id": "Inspeksi Solderan BGA Tembus Pandang." },
    { "en": "Apa Itu GPU (Graphics Processing Unit)?", "id": "Unit Pemroses Grafis Komputer." },
    { "en": "Apa Itu TPU (Tensor Processing Unit)?", "id": "Prosesor Khusus Kecerdasan Buatan (AI)." },
    { "en": "Apa Beda DDR4 Dan DDR5?", "id": "DDR5 Lebih Cepat Bandwidth Besar." },
    { "en": "Apa Itu NVMe SSD?", "id": "Penyimpanan Cepat Lewat Jalur PCIe." },
    { "en": "Apa Itu ECC RAM?", "id": "Memori Dengan Koreksi Error Otomatis." },
    { "en": "Apa Itu Server Blade?", "id": "Server Tipis Hemat Ruang Rak." },
    { "en": "Apa Itu Hypervisor?", "id": "Software Pengelola Mesin Virtual." },
    { "en": "Apa Itu Docker Container?", "id": "Wadah Aplikasi Ringan Terisolasi." },
    { "en": "Apa Itu Kubernetes?", "id": "Platform Manajemen Kontainer Otomatis." },
    { "en": "Apa Itu Blockchain Technology?", "id": "Buku Besar Digital Terdesentralisasi." },
    { "en": "Apa Itu Smart Contract?", "id": "Kontrak Digital Otomatis Blockchain." },
    { "en": "Apa Itu Mining Bitcoin?", "id": "Proses Verifikasi Transaksi Kripto." },
    { "en": "Apa Itu NFT?", "id": "Token Digital Unik Tidak Tergantikan." },
    { "en": "Apa Itu Metaverse?", "id": "Dunia Virtual Tiga Dimensi." },
    { "en": "Apa Itu Augmented Reality (AR)?", "id": "Objek Digital Di Dunia Nyata." },
    { "en": "Apa Itu Virtual Reality (VR)?", "id": "Dunia Simulasi Komputer Penuh." },
    { "en": "Apa Itu Haptic Feedback?", "id": "Umpan Balik Getaran Sentuhan." },
    { "en": "Apa Itu LiDAR Scanner HP?", "id": "Pemindai Jarak Laser 3D." },
    { "en": "Apa Itu NFC Payment?", "id": "Pembayaran Nirkabel Jarak Dekat." },
    { "en": "Apa Itu Wireless Charging Qi?", "id": "Standar Cas Tanpa Kabel." },
    { "en": "Apa Itu Fast Charging PD?", "id": "Power Delivery Lewat USB-C." },
    { "en": "Apa Itu GaN Charger?", "id": "Adaptor Kecil Efisiensi Tinggi." },
    { "en": "Apa Itu Power Bank 20000mAh?", "id": "Penyimpan Daya Portable Kapasitas Besar." },
    { "en": "Apa Itu Lithium Polymer (LiPo)?", "id": "Baterai Gel Polimer Bentuk Bebas." },
    { "en": "Apa Itu Lithium Ion (Li-Ion)?", "id": "Baterai Cair Silinder Standar." },
    { "en": "Apa Itu 18650 Battery?", "id": "Ukuran Baterai Diameter 18mm." },
    { "en": "Apa Itu 21700 Battery?", "id": "Ukuran Baterai Diameter 21mm." },
    { "en": "Apa Itu BMS Battery?", "id": "Sistem Manajemen Pelindung Baterai." },
    { "en": "Apa Itu Active Balancer?", "id": "Penyeimbang Tegangan Transfer Energi." },
    { "en": "Apa Itu Passive Balancer?", "id": "Penyeimbang Tegangan Buang Panas." },
    { "en": "Apa Itu SoH Baterai?", "id": "State Of Health Kesehatan Baterai." },
    { "en": "Apa Itu SoC Baterai?", "id": "State Of Charge Sisa Daya." },
    { "en": "Apa Itu DoD Baterai?", "id": "Depth Of Discharge Kedalaman Kuras." },
    { "en": "Apa Itu Cycle Life?", "id": "Umur Baterai Berdasarkan Siklus Cas." },
    { "en": "Apa Itu Memory Effect Baterai?", "id": "Kapasitas Turun Jika Cas Tanggung." },
    { "en": "Apa Itu Self Discharge?", "id": "Baterai Kosong Sendiri Saat Disimpan." },
    { "en": "Apa Itu Trickle Charging?", "id": "Pengisian Arus Sangat Kecil." },
    { "en": "Apa Itu Float Charging?", "id": "Tegangan Jaga Agar Tetap Penuh." },
    { "en": "Apa Itu Equalization Charging?", "id": "Cas Tegangan Tinggi Ratakan Sel." },
    { "en": "Apa Itu Sulfasi Aki?", "id": "Kristal Putih Menutup Plat Timbal." },
    { "en": "Apa Itu CCA Aki?", "id": "Cold Cranking Amps Arus Start." },
    { "en": "Apa Itu Aki MF?", "id": "Maintenance Free Bebas Perawatan." },
    { "en": "Apa Itu Aki Basah?", "id": "Perlu Isi Ulang Air Zuur." },
    { "en": "Apa Itu Air Zuur?", "id": "Asam Sulfat Elektrolit Aki." },
    { "en": "Apa Itu Air Accu (Biru)?", "id": "Air Murni Untuk Menambah Aki." },
    { "en": "Apa Itu Specific Gravity Aki?", "id": "Kepadatan Cairan Elektrolit Aki." },
    { "en": "Apa Alat Ukur Air Aki?", "id": "Hydrometer." },
    { "en": "Apa Warna Indikator Aki Bagus?", "id": "Biasanya Berwarna Hijau." },
    { "en": "Apa Warna Indikator Aki Jelek?", "id": "Biasanya Berwarna Putih Atau Merah." },
    { "en": "Apa Fungsi Alternator Mobil?", "id": "Pembangkit Listrik Saat Mesin Hidup." },
    { "en": "Apa Fungsi Dinamo Starter?", "id": "Motor Listrik Pemutar Awal Mesin." },
    { "en": "Apa Fungsi Solenoid Starter?", "id": "Saklar Magnetik Pendorong Gigi Starter." },
    { "en": "Apa Fungsi Bendix Starter?", "id": "Gigi Searah Pemutar Flywheel." },
    { "en": "Apa Fungsi Carbon Brush Alternator?", "id": "Sikat Arus Rotor Berputar." },
    { "en": "Apa Fungsi Dioda Bridge Alternator?", "id": "Penyearah Arus AC Ke DC." },
    { "en": "Apa Fungsi IC Regulator Alternator?", "id": "Stabilkan Tegangan Output Pengisian." },
    { "en": "Apa Fungsi Fuse Utama Mobil?", "id": "Pengaman Seluruh Kelistrikan Mobil." },
    { "en": "Apa Itu FL (Fusible Link)?", "id": "Sekering Utama Bentuk Kabel/Kotak." },
    { "en": "Apa Itu Relay 4 Kaki?", "id": "Saklar Elektromagnetik Standar Mobil." },
    { "en": "Apa Itu Relay 5 Kaki?", "id": "Saklar Tukar SPDT Otomotif." },
    { "en": "Apa Kaki 30 Relay?", "id": "Sumber Arus Baterai Langsung." },
    { "en": "Apa Kaki 87 Relay?", "id": "Output Ke Beban (NO)." },
    { "en": "Apa Kaki 86 Relay?", "id": "Pemicu Koil Positif/Negatif." },
    { "en": "Apa Kaki 85 Relay?", "id": "Pemicu Koil Pasangan 86." },
    { "en": "Apa Itu Flasher Sein?", "id": "Relay Kedip Lampu Belok." },
    { "en": "Apa Itu LED Canbus?", "id": "LED Dengan Resistor Tipu ECU." },
    { "en": "Apa Itu HID Xenon?", "id": "Lampu Gas Discharge Tegangan Tinggi." },
    { "en": "Apa Fungsi Ballast HID?", "id": "Pembangkit Tegangan Tinggi Start HID." },
    { "en": "Apa Itu Projector Lamp?", "id": "Lensa Fokus Cahaya Lampu Depan." },
    { "en": "Apa Itu DRL (Daytime Running Light)?", "id": "Lampu Siang Hari Keselamatan." },
    { "en": "Apa Itu Fog Lamp?", "id": "Lampu Kabut Tembus Cuaca Buruk." },
    { "en": "Apa Itu High Mount Stop Lamp?", "id": "Lampu Rem Ketiga Di Atas." },
    { "en": "Apa Itu OBD-II Port?", "id": "Soket Diagnostik Komputer Mobil." },
    { "en": "Apa Itu ECU (Engine Control Unit)?", "id": "Otak Elektronik Pengatur Mesin." },
    { "en": "Apa Itu TCU (Transmission Control Unit)?", "id": "Otak Elektronik Transmisi Otomatis." },
    { "en": "Apa Itu ABS Module?", "id": "Pengatur Pengereman Anti Terkunci." },
    { "en": "Apa Itu Airbag Sensor?", "id": "Deteksi Benturan Pemicu Kantong Udara." },
    { "en": "Apa Itu Clockspring Setir?", "id": "Kabel Spiral Klakson Airbag Putar." },
    { "en": "Apa Itu EPS (Electric Power Steering)?", "id": "Setir Ringan Bantuan Motor Listrik." },
    { "en": "Apa Itu Immobilizer Key?", "id": "Kunci Chip Anti Maling." },
    { "en": "Apa Itu Keyless Entry?", "id": "Masuk Mobil Tanpa Anak Kunci." },
    { "en": "Apa Itu Start Stop Button?", "id": "Tombol Nyala Mesin Tanpa Kunci." },
    { "en": "Apa Itu Head Unit Android?", "id": "Audio Video Mobil Sistem Android." },
    { "en": "Apa Itu Power Audio Mobil?", "id": "Penguat Suara Speaker Mobil." },
    { "en": "Apa Itu Monoblock Amplifier?", "id": "Power Khusus Subwoofer Satu Kanal." },
    { "en": "Apa Itu Subwoofer Aktif?", "id": "Speaker Bass Dengan Amplifier Internal." },
    { "en": "Apa Itu Capacitor Bank Audio?", "id": "Penstabil Tegangan Bass Besar." },
    { "en": "Apa Itu RCA Cable Audio?", "id": "Kabel Sinyal Suara Analog." },
    { "en": "Apa Itu Kabel Grounding Audio?", "id": "Kabel Masa Mencegah Noise Mesin." },
    { "en": "Apa Itu Peredam Suara Mobil?", "id": "Aspal Tempel Kedap Suara." },
    { "en": "Apa Itu Kamera Mundur?", "id": "Kamera Bantu Parkir Belakang." },
    { "en": "Apa Itu Sensor Parkir?", "id": "Sensor Ultrasonik Jarak Mundur." },
    { "en": "Apa Itu Dashcam?", "id": "Kamera Perekam Perjalanan Depan." },
    { "en": "Apa Itu GPS Tracker Mobil?", "id": "Pelacak Posisi Mobil Satelit." },
    { "en": "Apa Itu Cut-off Engine GPS?", "id": "Fitur Matikan Mesin Jarak Jauh." },
    { "en": "Apa Itu Relay Cut-off?", "id": "Relay Pemutus Jalur Pompa Bensin." },
    { "en": "Apa Itu Sekering Tancap (Blade)?", "id": "Sekering Pipih Warna Warni." },
    { "en": "Apa Itu Sekering Tabung Kaca?", "id": "Sekering Silinder Transparan Lama." },
    { "en": "Apa Itu Tespen DC?", "id": "Obeng Cek Listrik 12 Volt." },
    { "en": "Apa Itu EFIS Pesawat?", "id": "Electronic Flight Instrument System (Layar Kaca)." },
    { "en": "Apa Itu PFD (Primary Flight Display)?", "id": "Layar Utama Instrumen Terbang Pilot." },
    { "en": "Apa Itu ND (Navigation Display)?", "id": "Layar Peta Dan Rute Penerbangan." },
    { "en": "Apa Itu EICAS Pesawat?", "id": "Sistem Monitor Mesin Dan Peringatan Kru." },
    { "en": "Apa Itu HUD (Head Up Display)?", "id": "Proyeksi Data Di Kaca Depan Pilot." },
    { "en": "Apa Itu VOR Navigasi?", "id": "VHF Omnidirectional Range (Radio Arah)." },
    { "en": "Apa Itu DME (Distance Measuring Equipment)?", "id": "Ukur Jarak Pesawat Ke Stasiun Darat." },
    { "en": "Apa Itu ADF (Automatic Direction Finder)?", "id": "Penunjuk Arah Sinyal Radio NDB." },
    { "en": "Apa Itu ILS (Instrument Landing System)?", "id": "Panduan Pendaratan Presisi Saat Cuaca Buruk." },
    { "en": "Apa Itu Glideslope ILS?", "id": "Panduan Sudut Turun Vertikal Pendaratan." },
    { "en": "Apa Itu Localizer ILS?", "id": "Panduan Lurus Kanan Kiri Landasan." },
    { "en": "Apa Itu GPWS Pesawat?", "id": "Ground Proximity Warning System (Anti Tabrak)." },
    { "en": "Apa Itu TCAS (Traffic Collision Avoidance)?", "id": "Sistem Hindari Tabrakan Antar Pesawat." },
    { "en": "Apa Itu Fly-By-Wire?", "id": "Kendali Pesawat Menggunakan Sinyal Listrik." },
    { "en": "Apa Itu CBTC Kereta Api?", "id": "Communication Based Train Control (Otomatis)." },
    { "en": "Apa Itu ETCS (European Train Control System)?", "id": "Standar Sinyal Kereta Api Eropa." },
    { "en": "Apa Itu ATP (Automatic Train Protection)?", "id": "Pengereman Otomatis Jika Melanggar Sinyal." },
    { "en": "Apa Itu ATO (Automatic Train Operation)?", "id": "Kereta Jalan Dan Berhenti Otomatis." },
    { "en": "Apa Itu Axle Counter?", "id": "Penghitung Gandar Roda Deteksi Kereta." },
    { "en": "Apa Itu Track Circuit?", "id": "Deteksi Kereta Lewat Hubungan Singkat Rel." },
    { "en": "Apa Itu Balise Kereta?", "id": "Transponder Di Antara Rel Kereta." },
    { "en": "Apa Itu Pantograf Kereta Listrik?", "id": "Alat Ambil Listrik Dari Kabel Atas." },
    { "en": "Apa Itu RT60 Akustik?", "id": "Waktu Dengung Turun 60 Desibel." },
    { "en": "Apa Itu Anechoic Chamber?", "id": "Ruangan Kedap Gema Sempurna." },
    { "en": "Apa Itu Standing Wave Akustik?", "id": "Gelombang Suara Pantul Tumpuk Di Ruangan." },
    { "en": "Apa Itu Bass Trap?", "id": "Peredam Akustik Khusus Nada Rendah." },
    { "en": "Apa Itu Diffuser Akustik?", "id": "Penyebar Pantulan Suara Agar Acak." },
    { "en": "Apa Itu Absorber Akustik?", "id": "Penyerap Suara Kurangi Gema." },
    { "en": "Apa Itu SPL Meter?", "id": "Sound Pressure Level Meter (Kebisingan)." },
    { "en": "Apa Itu White Noise Audio?", "id": "Energi Suara Sama Tiap Frekuensi." },
    { "en": "Apa Itu Pink Noise Audio?", "id": "Energi Suara Sama Tiap Oktaf." },
    { "en": "Apa Fungsi Pink Noise?", "id": "Kalibrasi Ekualiser Sistem Audio (RTA)." },
    { "en": "Apa Itu Firewall Stateful?", "id": "Firewall Pantau Status Koneksi Paket." },
    { "en": "Apa Itu IDS (Intrusion Detection System)?", "id": "Deteksi Serangan Jaringan Tanpa Blokir." },
    { "en": "Apa Itu IPS (Intrusion Prevention System)?", "id": "Deteksi Dan Blokir Serangan Otomatis." },
    { "en": "Apa Itu DDoS Attack?", "id": "Serangan Banjir Trafik Lumpuhkan Server." },
    { "en": "Apa Itu Phishing Email?", "id": "Email Palsu Curi Data Pengguna." },
    { "en": "Apa Itu Ransomware?", "id": "Malware Pengunci Data Minta Tebusan." },
    { "en": "Apa Itu Zero Day Exploit?", "id": "Serangan Celah Keamanan Belum Ditambal." },
    { "en": "Apa Itu Penetration Testing?", "id": "Simulasi Peretasan Untuk Uji Keamanan." },
    { "en": "Apa Itu SSL/TLS Encryption?", "id": "Pengamanan Data Web HTTPS." },
    { "en": "Apa Itu Public Key Cryptography?", "id": "Enkripsi Kunci Publik Dan Privat." },
    { "en": "Apa Itu Hash Function?", "id": "Ubah Data Jadi Kode Unik Tetap." },
    { "en": "Apa Itu Salt Dalam Password?", "id": "Data Acak Tambahan Perkuat Hash." },
    { "en": "Apa Itu MFA (Multi Factor Authentication)?", "id": "Verifikasi Ganda Selain Password." },
    { "en": "Apa Itu Biometric Authentication?", "id": "Login Pakai Sidik Jari Wajah." },
    { "en": "Apa Itu QR Code?", "id": "Kode Batang Dua Dimensi Matriks." },
    { "en": "Apa Itu Barcode 1D?", "id": "Kode Garis Garis Satu Dimensi." },
    { "en": "Apa Itu RFID Tag Pasif?", "id": "Tag Tanpa Baterai Ditenagai Reader." },
    { "en": "Apa Itu RFID Tag Aktif?", "id": "Tag Punya Baterai Jarak Jauh." },
    { "en": "Apa Itu NFC Card Emulation?", "id": "HP Bertindak Seperti Kartu Akses." },
    { "en": "Apa Itu BIOS Komputer?", "id": "Basic Input Output System." },
    { "en": "Apa Itu UEFI?", "id": "Unified Extensible Firmware Interface (Modern BIOS)." },
    { "en": "Apa Itu POST (Power On Self Test)?", "id": "Cek Hardware Saat Komputer Nyala." },
    { "en": "Apa Itu Overclocking CPU?", "id": "Paksa Prosesor Kerja Lebih Cepat." },
    { "en": "Apa Itu Throttling CPU?", "id": "Turunkan Kecepatan Karena Panas Berlebih." },
    { "en": "Apa Itu Benchmarking?", "id": "Uji Skor Performa Perangkat Keras." },
    { "en": "Apa Itu Thermal Design Power (TDP)?", "id": "Panas Maksimal Yang Dihasilkan Chip." },
    { "en": "Apa Itu Silicon Wafer?", "id": "Piringan Bahan Dasar Pembuatan Chip." },
    { "en": "Apa Itu Die (Chip)?", "id": "Potongan Kecil Wafer Berisi Sirkuit." },
    { "en": "Apa Itu Wire Bonding?", "id": "Kawat Emas Sambung Chip Ke Kaki." },
    { "en": "Apa Itu Encapsulation IC?", "id": "Bungkus Chip Dengan Plastik Hitam." },
    { "en": "Apa Itu Cleanroom Fabrikasi?", "id": "Ruang Produksi Bebas Debu Mikro." },
    { "en": "Apa Itu Lithography Chip?", "id": "Cetak Pola Sirkuit Pakai Cahaya." },
    { "en": "Apa Itu Moore's Law?", "id": "Transistor Chip Ganda Tiap 2 Tahun." },
    { "en": "Apa Itu Nanometer Process?", "id": "Ukuran Transistor Dalam Chip (Contoh 5nm)." },
    { "en": "Apa Itu FinFET Transistor?", "id": "Transistor 3D Sirip Efisiensi Tinggi." },
    { "en": "Apa Itu GAAFET (Gate-All-Around)?", "id": "Teknologi Transistor Penerus FinFET." },
    { "en": "Apa Itu SoC (System on Chip)?", "id": "Komputer Lengkap Dalam Satu Chip." },
    { "en": "Apa Itu Microcontroller (MCU)?", "id": "Komputer Kecil pengendali Tugas Khusus." },
    { "en": "Apa Itu Microprocessor (MPU)?", "id": "Otak Komputer Butuh Memori Luar." },
    { "en": "Apa Itu Heatsink Fan (HSF)?", "id": "Pendingin Logam Dengan Kipas." },
    { "en": "Apa Itu Water Cooling PC?", "id": "Pendingin Cairan Sirkulasi Radiator." },
    { "en": "Apa Itu AIO Cooler?", "id": "All In One Water Cooling Pabrikan." },
    { "en": "Apa Itu Custom Loop Cooling?", "id": "Water Cooling Rakitan Pipa Manual." },
    { "en": "Apa Itu Radiator PC?", "id": "Penukar Panas Cairan Ke Udara." },
    { "en": "Apa Itu Reservoir PC?", "id": "Tangki Penampung Cairan Pendingin." },
    { "en": "Apa Itu Pump PC?", "id": "Pompa Sirkulasi Cairan Pendingin." },
    { "en": "Apa Itu Fitting Compression?", "id": "Konektor Pipa Water Cooling Kencang." },
    { "en": "Apa Itu Tubing Hard Tube?", "id": "Pipa Akrilik Atau PETG Keras." },
    { "en": "Apa Itu Tubing Soft Tube?", "id": "Selang Karet Atau Silikon Lentur." },
    { "en": "Apa Itu MSDS Material?", "id": "Material Safety Data Sheet (Lembar Data)." },
    { "en": "Apa Itu Hazmat Suit?", "id": "Baju Pelindung Bahan Berbahaya." },
    { "en": "Apa Itu Respirator N95?", "id": "Masker Saring 95 Persen Partikel." },
    { "en": "Apa Itu Safety Goggles?", "id": "Kacamata Pelindung Rapat Anti Kimia." },
    { "en": "Apa Itu Ear Plugs?", "id": "Sumbat Telinga Busa Peredam Bising." },
    { "en": "Apa Itu Ear Muffs?", "id": "Penutup Telinga Besar Peredam Bising." },
    { "en": "Apa Itu Safety Shoes Toe Cap?", "id": "Ujung Baja Pelindung Jari Kaki." },
    { "en": "Apa Itu Safety Harness Double Hook?", "id": "Sabuk Panjat Dua Pengait Aman." },
    { "en": "Apa Itu Fall Arrest System?", "id": "Sistem Penahan Jatuh Pekerja Tinggi." },
    { "en": "Apa Itu Confined Space Tripod?", "id": "Alat Kerek Penyelamat Ruang Sempit." },
    { "en": "Apa Itu Gas Detector 4 Gas?", "id": "Deteksi O2, CO, H2S, LEL." },
    { "en": "Apa Itu LEL (Lower Explosive Limit)?", "id": "Batas Bawah Konsentrasi Gas Meledak." },
    { "en": "Apa Itu UEL (Upper Explosive Limit)?", "id": "Batas Atas Konsentrasi Gas Meledak." },
    { "en": "Apa Itu Lockout Station?", "id": "Tempat Simpan Gembok Dan Tag LOTO." },
    { "en": "Apa Itu Key Alike Padlock?", "id": "Satu Kunci Buka Banyak Gembok." },
    { "en": "Apa Itu Key Different Padlock?", "id": "Kunci Unik Tiap Gembok." },
    { "en": "Apa Itu Master Key System?", "id": "Kunci Dewa Buka Semua Gembok." },
    { "en": "Apa Itu Scaffolding Tag Hijau?", "id": "Perancah Aman Siap Digunakan." },
    { "en": "Apa Itu Scaffolding Tag Merah?", "id": "Perancah Tidak Aman Dilarang Naik." },
    { "en": "Apa Itu FLOPS Komputer?", "id": "Floating Point Operations Per Second." },
    { "en": "Apa Itu Teraflops?", "id": "Satu Triliun Operasi Per Detik." },
    { "en": "Apa Itu Petaflops?", "id": "Satu Kuadrillion Operasi Per Detik." },
    { "en": "Apa Itu Supercomputer Cluster?", "id": "Gabungan Ribuan Komputer Bekerja Bersama." },
    { "en": "Apa Itu Parallel Processing?", "id": "Proses Data Bersamaan Banyak Inti." },
    { "en": "Apa Itu CPU Core?", "id": "Unit Pemroses Inti Dalam Prosesor." },
    { "en": "Apa Itu CPU Thread?", "id": "Jalur Eksekusi Virtual Dalam Core." },
    { "en": "Apa Itu Hyper-Threading?", "id": "Satu Core Fisik Dua Thread." },
    { "en": "Apa Itu Cache L1?", "id": "Memori Tercepat Paling Dekat Prosesor." },
    { "en": "Apa Itu Cache L3?", "id": "Memori Bersama Semua Core Prosesor." },
    { "en": "Apa Itu TDP Processor?", "id": "Thermal Design Power (Panas Maksimal)." },
    { "en": "Apa Itu Overclocking?", "id": "Paksa Kecepatan Di Atas Standar." },
    { "en": "Apa Itu Undervolting?", "id": "Kurangi Tegangan Agar Lebih Dingin." },
    { "en": "Apa Itu Bottleneck PC?", "id": "Komponen Lambat Menghambat Kinerja Sistem." },
    { "en": "Apa Itu Thermal Throttling?", "id": "Turunkan Kecepatan Karena Suhu Panas." },
    { "en": "Apa Itu Water Cooling Loop?", "id": "Sirkuit Pendingin Cairan Tertutup." },
    { "en": "Apa Itu SEPIC Converter?", "id": "Regulator DC Naik Turun Non-inverting." },
    { "en": "Apa Itu Cuk Converter?", "id": "Regulator DC Output Polaritas Terbalik." },
    { "en": "Apa Itu Zeta Converter?", "id": "Mirip SEPIC Tapi Output Positif." },
    { "en": "Apa Itu Flyback Snubber?", "id": "Redam Spike Tegangan Pada MOSFET." },
    { "en": "Apa Itu LLC Resonant Converter?", "id": "Konverter Efisiensi Tinggi Zero Switching." },
    { "en": "Apa Itu Totem Pole PFC?", "id": "Koreksi Faktor Daya Tanpa Bridge." },
    { "en": "Apa Itu Interleaved Buck Converter?", "id": "Dua Buck Bekerja Bergantian Fasa." },
    { "en": "Apa Keuntungan Interleaved Converter?", "id": "Ripple Arus Output Lebih Kecil." },
    { "en": "Apa Itu Audio Embedder?", "id": "Masukkan Suara Ke Sinyal Video." },
    { "en": "Apa Itu Audio De-embedder?", "id": "Pisahkan Suara Dari Sinyal Video." },
    { "en": "Apa Itu HDMI Matrix Switch?", "id": "Banyak Input Ke Banyak Output." },
    { "en": "Apa Itu Video Wall Controller?", "id": "Gabung Banyak Layar Jadi Satu." },
    { "en": "Apa Itu SDI Cable?", "id": "Kabel Video Digital Jarak Jauh." },
    { "en": "Apa Impedansi Kabel SDI?", "id": "75 Ohm." },
    { "en": "Apa Itu 3G-SDI?", "id": "Video HD 1080p 60Hz." },
    { "en": "Apa Itu 12G-SDI?", "id": "Video 4K 60Hz Satu Kabel." },
    { "en": "Apa Itu Fiber Optic Extender?", "id": "Kirim Sinyal HDMI Lewat Fiber." },
    { "en": "Apa Itu HDBaseT?", "id": "Kirim HDMI Lewat Kabel LAN." },
    { "en": "Apa Itu EDID Emulator?", "id": "Alat Tipu Resolusi Layar Monitor." },
    { "en": "Apa Itu Chroma Key?", "id": "Teknik Layar Hijau (Green Screen)." },
    { "en": "Apa Itu Genlock Video?", "id": "Sinkronisasi Frame Antar Kamera." },
    { "en": "Apa Itu Timecode Audio?", "id": "Data Waktu Sinkronisasi Suara Gambar." },
    { "en": "Apa Itu Dark Fibre?", "id": "Kabel Optik Terpasang Belum Aktif." },
    { "en": "Apa Itu Lit Fibre?", "id": "Kabel Optik Aktif Membawa Data." },
    { "en": "Apa Itu Metro Ethernet?", "id": "Jaringan Ethernet Skala Kota Besar." },
    { "en": "Apa Itu MPLS Network?", "id": "Jaringan Label Switching Kecepatan Tinggi." },
    { "en": "Apa Itu SD-WAN?", "id": "Jaringan Luas Berbasis Software." },
    { "en": "Apa Itu VSAT C-Band?", "id": "Satelit Piringan Besar Tahan Hujan." },
    { "en": "Apa Itu VSAT Ku-Band?", "id": "Satelit Piringan Kecil Rawan Hujan." },
    { "en": "Apa Itu VSAT Ka-Band?", "id": "Satelit Internet Kecepatan Tinggi." },
    { "en": "Apa Itu LEO Satellite Internet?", "id": "Internet Satelit Orbit Rendah (Starlink)." },
    { "en": "Apa Keunggulan LEO Satelit?", "id": "Latensi (Ping) Sangat Rendah." },
    { "en": "Apa Itu Ground Station?", "id": "Stasiun Bumi Pengendali Satelit." },
    { "en": "Apa Itu Teleport Satelit?", "id": "Hub Komunikasi Satelit Dan Internet." },
    { "en": "Apa Itu Baterai Redox Flow?", "id": "Baterai Cairan Tangki Skala Besar." },
    { "en": "Apa Itu Battery Swapping Station?", "id": "Stasiun Tukar Baterai Motor Listrik." },
    { "en": "Apa Itu Second Life Battery?", "id": "Baterai Bekas EV Untuk PLTS." },
    { "en": "Apa Itu Sodium-Ion Battery?", "id": "Baterai Bahan Garam Murah." },
    { "en": "Apa Kelebihan Sodium-Ion?", "id": "Bahan Baku Melimpah Dan Murah." },
    { "en": "Apa Itu Solid State Electrolyte?", "id": "Elektrolit Padat Aman Tidak Bocor." },
    { "en": "Apa Itu Anoda Silikon?", "id": "Anoda Kapasitas Tinggi Baterai Baru." },
    { "en": "Apa Itu Wireless BMS?", "id": "Manajemen Baterai Tanpa Kabel Sense." },
    { "en": "Apa Itu PLC Redundancy?", "id": "Sistem PLC Ganda Saling Backup." },
    { "en": "Apa Itu Hot Standby PLC?", "id": "PLC Cadangan Siap Ambil Alih." },
    { "en": "Apa Itu Cold Standby PLC?", "id": "PLC Cadangan Mati Perlu Dinyalakan." },
    { "en": "Apa Itu Vote Logic (2oo3)?", "id": "Logika Suara 2 Dari 3." },
    { "en": "Apa Itu Fail Safe System?", "id": "Sistem Keadaan Aman Saat Rusak." },
    { "en": "Apa Itu SIL 3 PLC?", "id": "PLC Standar Keamanan Tinggi." },
    { "en": "Apa Itu Force Value PLC?", "id": "Paksa Nilai Input Output Software." },
    { "en": "Apa Itu Online Edit PLC?", "id": "Ubah Program Saat Mesin Jalan." },
    { "en": "Apa Itu Upload Program PLC?", "id": "Salin Program Dari PLC Ke PC." },
    { "en": "Apa Itu Download Program PLC?", "id": "Kirim Program Dari PC Ke PLC." },
    { "en": "Apa Itu Solenoid Push Type?", "id": "Batang Mendorong Saat Diberi Daya." },
    { "en": "Apa Itu Solenoid Pull Type?", "id": "Batang Menarik Saat Diberi Daya." },
    { "en": "Apa Itu Solenoid Bistable?", "id": "Kunci Posisi Dengan Magnet Permanen." },
    { "en": "Apa Itu Voice Coil Actuator?", "id": "Penggerak Linear Cepat Presisi Speaker." },
    { "en": "Apa Itu MEMS Microphone?", "id": "Mikrofon Chip Ukuran Sangat Kecil." },
    { "en": "Apa Itu PDM Output Mic?", "id": "Sinyal Digital Pulse Density Modulation." },
    { "en": "Apa Itu I2S Interface Audio?", "id": "Protokol Serial Data Audio Digital." },
    { "en": "Apa Itu DAC Audio 32-bit?", "id": "Konverter Audio Resolusi Sangat Tinggi." },
    { "en": "Apa Itu PCB Multilayer 4 Layer?", "id": "Empat Lapis Tembaga Satu Papan." },
    { "en": "Apa Itu Blind Via Layer 1-2?", "id": "Lubang Dari Top Ke Layer 2." },
    { "en": "Apa Itu Buried Via Layer 2-3?", "id": "Lubang Antar Layer Dalam Saja." },
    { "en": "Apa Itu Microvia Laser Drilled?", "id": "Lubang Via Sangat Kecil Laser." },
    { "en": "Apa Itu Rigid-Flex PCB?", "id": "Gabungan Papan Kaku Dan Lentur." },
    { "en": "Apa Itu PCB Aluminium Core?", "id": "PCB Pendingin Untuk Lampu LED." },
    { "en": "Apa Itu PCB Ceramic Base?", "id": "PCB Tahan Panas Frekuensi Tinggi." },
    { "en": "Apa Itu Signal Integrity?", "id": "Kualitas Sinyal Listrik Pada Jalur." },
    { "en": "Apa Itu Impedance Mismatch?", "id": "Beda Impedansi Sebabkan Pantulan Sinyal." },
    { "en": "Apa Itu Differential Pair Routing?", "id": "Dua Jalur Sejajar Sinyal Balans." },
    { "en": "Apa Itu Length Matching PCB?", "id": "Samakan Panjang Jalur Bus Data." },
    { "en": "Apa Itu Serpentine Trace?", "id": "Jalur Berkelok Tambah Panjang Kabel." },
    { "en": "Apa Itu Thermal Relief Pad?", "id": "Pad Ground Dengan Celah Panas." },
    { "en": "Apa Itu Teardrop Pad?", "id": "Penguat Sambungan Pad Dan Jalur." },
    { "en": "Apa Itu Mouse Bites PCB?", "id": "Lubang Kecil Pinggir Panel PCB." },
    { "en": "Apa Itu V-Cut Panel?", "id": "Garis Potong Pemisah Panel PCB." },
    { "en": "Apa Itu Fiducial Mark PCB?", "id": "Titik Referensi Mesin Pick Place." },
    { "en": "Apa Itu Stencil Solder Paste?", "id": "Plat Cetak Pasta Timah SMD." },
    { "en": "Apa Itu Pick And Place Machine?", "id": "Robot Pasang Komponen SMD Otomatis." },
    { "en": "Apa Itu Reflow Oven?", "id": "Pemanas Pasta Solder Komponen SMD." },
    { "en": "Apa Itu Wave Soldering?", "id": "Solder Celup Gelombang Komponen THT." },
    { "en": "Apa Itu Conformal Coating?", "id": "Lapisan Pelindung PCB Anti Lembap." },
    { "en": "Apa Itu PID Overshoot?", "id": "Lonjakan Nilai Melebihi Setpoint." },
    { "en": "Apa Itu PID Rise Time?", "id": "Waktu Sinyal Naik Ke Target." },
    { "en": "Apa Itu PID Settling Time?", "id": "Waktu Sinyal Menjadi Stabil." },
    { "en": "Apa Itu PID Steady State Error?", "id": "Selisih Nilai Akhir Dan Target." },
    { "en": "Apa Itu Tuning Zigler-Nichols?", "id": "Metode Cari Parameter PID Klasik." },
    { "en": "Apa Itu Tuning Cohen-Coon?", "id": "Metode PID Untuk Proses Lambat." },
    { "en": "Apa Itu Dead Time Proses?", "id": "Waktu Tunda Respon Sistem Fisik." },
    { "en": "Apa Itu Process Lag?", "id": "Kelambatan Perubahan Output Proses." },
    { "en": "Apa Itu Encoder Quadrature?", "id": "Output Dua Sinyal Beda Fasa." },
    { "en": "Apa Fungsi Resolver Motor?", "id": "Sensor Posisi Rotor Tahan Panas." },
    { "en": "Apa Itu Hall Effect Latch?", "id": "Sensor Magnet Dengan Memori Status." },
    { "en": "Apa Itu Load Cell Creep?", "id": "Perubahan Nilai Saat Beban Tetap." },
    { "en": "Apa Itu Hysteresis Error Sensor?", "id": "Beda Nilai Saat Naik Turun." },
    { "en": "Apa Itu Sensitivity Drift?", "id": "Perubahan Sensitivitas Akibat Suhu." },
    { "en": "Apa Itu Zero Drift Sensor?", "id": "Pergeseran Titik Nol Seiring Waktu." },
    { "en": "Apa Itu Span Error Sensor?", "id": "Kesalahan Pada Rentang Maksimum." },
    { "en": "Apa Itu Linearity Error?", "id": "Penyimpangan Dari Garis Lurus Ideal." },
    { "en": "Apa Itu Repeatability Error?", "id": "Variasi Hasil Ukur Berulang Kali." },
    { "en": "Apa Itu Cavitation Valve?", "id": "Gelembung Uap Merusak Dinding Katup." },
    { "en": "Apa Itu Flashing Valve?", "id": "Cairan Berubah Jadi Uap Permanen." },
    { "en": "Apa Itu Choked Flow?", "id": "Aliran Maksimal Tidak Bisa Bertambah." },
    { "en": "Apa Itu Valve Stiction?", "id": "Gesekan Statis Hambat Gerak Katup." },
    { "en": "Apa Itu Valve Deadband?", "id": "Rentang Sinyal Tanpa Gerakan Mekanis." },
    { "en": "Apa Itu Packet Sniffer?", "id": "Alat Sadap Paket Data Jaringan." },
    { "en": "Apa Fungsi Wireshark?", "id": "Software Analisis Protokol Jaringan Detail." },
    { "en": "Apa Itu Ping of Death?", "id": "Serangan Paket Ping Ukuran Besar." },
    { "en": "Apa Itu Man in the Middle?", "id": "Peretas Menyusup Diantara Dua Komunikasi." },
    { "en": "Apa Itu SQL Injection?", "id": "Serangan Kode Jahat Ke Database." },
    { "en": "Apa Itu Tap Changer OLTC?", "id": "Ubah Tap Trafo Saat Berbeban." },
    { "en": "Apa Itu Tap Changer DETC?", "id": "Ubah Tap Trafo Saat Mati." },
    { "en": "Apa Itu Vector Group Trafo?", "id": "Kode Hubungan Fasa Primer Sekunder." },
    { "en": "Apa Itu Impedansi Urutan Nol?", "id": "Impedansi Saat Terjadi Gangguan Tanah." },
    { "en": "Apa Itu Impedansi Urutan Positif?", "id": "Impedansi Sistem Saat Operasi Normal." },
    { "en": "Apa Itu Gerber File PCB?", "id": "File Desain Untuk Produksi PCB." },
    { "en": "Apa Itu Centroid File PCB?", "id": "Data Posisi Komponen Pick Place." },
    { "en": "Apa Itu BOM File?", "id": "Daftar Material Komponen Lengkap." },
    { "en": "Apa Itu PCB Panelization?", "id": "Gabung Banyak PCB Satu Papan." },
    { "en": "Apa Itu V-Score Cut PCB?", "id": "Potongan Parit Pemisah Panel PCB." },
    { "en": "Apa Itu Phantom Power 48V?", "id": "Daya Listrik Mikrofon Kondensor XLR." },
    { "en": "Apa Itu Audio Clipping?", "id": "Sinyal Terpotong Akibat Volume Berlebih." },
    { "en": "Apa Itu Headroom Audio?", "id": "Ruang Aman Sinyal Sebelum Distorsi." },
    { "en": "Apa Itu Noise Floor Audio?", "id": "Level Kebisingan Dasar Alat Elektronik." },
    { "en": "Apa Itu Dynamic Range Audio?", "id": "Selisih Suara Terpelan Dan Terkeras." },
    { "en": "Apa Itu Solar Irradiance?", "id": "Daya Matahari Per Meter Persegi." },
    { "en": "Apa Itu Solar Insolation?", "id": "Energi Matahari Terakumulasi Waktu." },
    { "en": "Apa Itu Air Mass (AM)?", "id": "Ketebalan Atmosfer Yang Dilewati Cahaya." },
    { "en": "Apa Fungsi Pyranometer?", "id": "Alat Ukur Radiasi Matahari Global." },
    { "en": "Apa Fungsi Pyrheliometer?", "id": "Ukur Radiasi Langsung Sinar Matahari." },
    { "en": "Apa Fungsi Oscilloscope Probe x1?", "id": "Ukur Sinyal Asli Tanpa Redaman." },
    { "en": "Apa Fungsi Oscilloscope Probe x10?", "id": "Redam Sinyal Masuk Sepuluh Kali." },
    { "en": "Apa Fungsi Logic Probe?", "id": "Cek Status Logika High Low." },
    { "en": "Apa Fungsi Signal Tracer?", "id": "Lacak Jalur Sinyal Audio Putus." },
    { "en": "Apa Fungsi Gauss Meter?", "id": "Ukur Kekuatan Medan Magnet Benda." },
    { "en": "Apa Itu Battery C-Rate?", "id": "Kecepatan Cas Atau Kuras Baterai." },
    { "en": "Apa Itu Peukert Effect?", "id": "Kapasitas Turun Saat Arus Besar." },
    { "en": "Apa Itu Self Discharge Baterai?", "id": "Baterai Kosong Sendiri Saat Disimpan." },
    { "en": "Apa Itu Cycle Life Baterai?", "id": "Jumlah Cas Ulang Hingga Rusak." },
    { "en": "Apa Itu Calendar Life Baterai?", "id": "Umur Baterai Berdasarkan Waktu Simpan." },
    { "en": "Apa Itu Slip Motor Induksi?", "id": "Selisih Kecepatan Rotor Dan Medan." },
    { "en": "Apa Itu Synchronous Speed Motor?", "id": "Kecepatan Putar Medan Magnet Stator." },
    { "en": "Apa Itu Locked Rotor Amps?", "id": "Arus Puncak Saat Rotor Diam." },
    { "en": "Apa Itu Full Load Amps?", "id": "Arus Nominal Saat Beban Penuh." },
    { "en": "Apa Itu Service Factor Motor?", "id": "Toleransi Beban Lebih Motor Aman." },
    { "en": "Apa Itu Kabel NYA?", "id": "Kabel Tembaga Tunggal Isolasi PVC." },
    { "en": "Apa Itu Kabel NYM?", "id": "Kabel Multi Core Isolasi Putih." },
    { "en": "Apa Itu Kabel NYY?", "id": "Kabel Multi Core Isolasi Hitam." },
    { "en": "Apa Itu Kabel NYFGBY?", "id": "Kabel Tanah Dengan Perisai Baja." },
    { "en": "Apa Itu Kabel Coaxial RG6?", "id": "Kabel Antena TV Digital Standar." },
    { "en": "Apa Satuan Lumen?", "id": "Total Cahaya Dari Sumber Lampu." },
    { "en": "Apa Satuan Lux?", "id": "Cahaya Yang Sampai Ke Permukaan." },
    { "en": "Apa Satuan Candela?", "id": "Intensitas Cahaya Ke Satu Arah." },
    { "en": "Apa Itu CCT Lighting?", "id": "Suhu Warna Cahaya (Kelvin)." },
    { "en": "Apa Itu CRI Lighting?", "id": "Akurasi Warna Objek Bawah Lampu." },
    { "en": "Apa Itu BIOS Legacy?", "id": "Sistem Dasar Komputer Model Lama." },
    { "en": "Apa Itu UEFI BIOS?", "id": "Sistem Dasar Komputer Modern Canggih." },
    { "en": "Apa Itu GPT Partition?", "id": "Tabel Partisi Harddisk Standar Baru." },
    { "en": "Apa Itu MBR Partition?", "id": "Tabel Partisi Harddisk Standar Lama." },
    { "en": "Apa Fungsi Secure Boot?", "id": "Cegah Software Jahat Saat Booting." },
    { "en": "Apa Kepanjangan SIM Card?", "id": "Subscriber Identity Module." },
    { "en": "Apa Itu eSIM?", "id": "Embedded Subscriber Identity Module." },
    { "en": "Apa Kepanjangan IMEI?", "id": "International Mobile Equipment Identity." },
    { "en": "Apa Kepanjangan IMSI?", "id": "International Mobile Subscriber Identity." },
    { "en": "Apa Itu APN HP?", "id": "Access Point Name Koneksi Internet." },
    { "en": "Apa Isi APAR Powder?", "id": "Serbuk Kimia Kering Pemadam Api." },
    { "en": "Apa Isi APAR CO2?", "id": "Gas Karbon Dioksida Dingin." },
    { "en": "Apa Fungsi Fire Blanket?", "id": "Selimut Tahan Api Tutup Kebakaran." },
    { "en": "Apa Fungsi Hydrant Pillar?", "id": "Titik Ambil Air Pemadam Luar." },
    { "en": "Apa Fungsi Smoke Detector?", "id": "Pendeteksi Asap Kebakaran Di Plafon." },
    { "en": "Apa Itu Dielectric Constant?", "id": "Kemampuan Bahan Simpan Medan Listrik." },
    { "en": "Apa Itu Permeability?", "id": "Kemampuan Bahan Alirkan Medan Magnet." },
    { "en": "Apa Itu Conductivity?", "id": "Kemampuan Bahan Hantarkan Arus Listrik." },
    { "en": "Apa Itu Resistivity?", "id": "Kemampuan Bahan Hambat Arus Listrik." },
    { "en": "Apa Itu Thermal Conductivity?", "id": "Kemampuan Bahan Hantarkan Panas." },
    { "en": "Apa Fungsi Resistor Shunt?", "id": "Resistor Ukur Arus Besar." },
    { "en": "Apa Fungsi Resistor Bleeder?", "id": "Buang Muatan Kapasitor Sisa." },
    { "en": "Apa Fungsi Resistor Pull-up?", "id": "Jaga Logika High Input Digital." },
    { "en": "Apa Fungsi Resistor Pull-down?", "id": "Jaga Logika Low Input Digital." },
    { "en": "Apa Itu Resistor Array?", "id": "Kumpulan Resistor Dalam Satu Paket." },
    { "en": "Apa Itu Truth Table?", "id": "Tabel Kebenaran Logika Digital." },
    { "en": "Apa Itu Boolean Algebra?", "id": "Matematika Logika Biner Digital." },
    { "en": "Apa Itu Karnaugh Map?", "id": "Metode Penyederhanaan Gerbang Logika." },
    { "en": "Apa Itu State Machine?", "id": "Model Logika Berbasis Status Keadaan." },
    { "en": "Apa Itu Flip-Flop JK?", "id": "Memori Satu Bit Universal." },
    { "en": "Apa Itu Analog Signal?", "id": "Sinyal Kontinyu Tanpa Putus." },
    { "en": "Apa Itu Digital Signal?", "id": "Sinyal Diskrit Nilai Nol Satu." },
    { "en": "Apa Itu Noise Floor?", "id": "Level Bawah Sinyal Gangguan." },
    { "en": "Apa Itu SNR Ratio?", "id": "Perbandingan Kekuatan Sinyal Dan Noise." },
    { "en": "Apa Itu Bandwidth Sinyal?", "id": "Lebar Pita Frekuensi Yang Digunakan." },
    { "en": "Apa Itu Cleanroom ISO 5?", "id": "Setara Class 100 Standar Lama." },
    { "en": "Apa Fungsi Photoresist Positif?", "id": "Bagian Terkena Cahaya Menjadi Larut." },
    { "en": "Apa Fungsi Photoresist Negatif?", "id": "Bagian Terkena Cahaya Menjadi Keras." },
    { "en": "Apa Itu Wet Etching?", "id": "Kikis Material Menggunakan Cairan Kimia." },
    { "en": "Apa Itu Dry Etching (Plasma)?", "id": "Kikis Material Menggunakan Gas Plasma." },
    { "en": "Apa Itu Wafer Dicing?", "id": "Potong Wafer Menjadi Chip Individu." },
    { "en": "Apa Itu Wire Bonding?", "id": "Sambung Chip Ke Kaki IC." },
    { "en": "Apa Bahan Kawat Wire Bonding?", "id": "Emas, Tembaga, Atau Aluminium." },
    { "en": "Apa Itu Flip Chip Technology?", "id": "Pasang Chip Terbalik Tanpa Kawat." },
    { "en": "Apa Itu AES/EBU Audio?", "id": "Standar Audio Digital Profesional Balanced." },
    { "en": "Berapa Impedansi Kabel AES/EBU?", "id": "110 Ohm." },
    { "en": "Apa Itu S/PDIF Coaxial?", "id": "Audio Digital Konsumen Konektor RCA." },
    { "en": "Apa Itu S/PDIF Optical?", "id": "Audio Digital Konsumen Konektor Toslink." },
    { "en": "Apa Fungsi Word Clock Audio?", "id": "Sinkronisasi Sampel Digital Antar Alat." },
    { "en": "Apa Itu MIDI Baud Rate?", "id": "31250 Bit Per Detik." },
    { "en": "Apa Itu DMX Terminator Value?", "id": "120 Ohm." },
    { "en": "Apa Fungsi Kabel Snake Audio?", "id": "Gabungan Banyak Kabel Audio Analog." },
    { "en": "Apa Itu DI Box Passive?", "id": "Penyesuai Impedansi Tanpa Baterai." },
    { "en": "Apa Itu DI Box Active?", "id": "Penyesuai Impedansi Perlu Baterai Phantom." },
    { "en": "Apa Itu IEC 60870-5-101?", "id": "Protokol Telecontrol Serial Standar IEC." },
    { "en": "Apa Itu IEC 60870-5-104?", "id": "Protokol Telecontrol Berbasis TCP/IP." },
    { "en": "Apa Fungsi PMU (Phasor Measurement Unit)?", "id": "Ukur Fasor Tegangan Arus Real-time." },
    { "en": "Apa Itu Synchrophasor?", "id": "Data Fasor Bersamaan Waktu GPS." },
    { "en": "Apa Fungsi STATCOM?", "id": "Kompensasi Daya Reaktif Cepat Inverter." },
    { "en": "Apa Fungsi SVC (Static Var Compensator)?", "id": "Kompensasi Reaktif Thyristor Cepat." },
    { "en": "Apa Itu HVDC LCC?", "id": "Line Commutated Converter (Thyristor)." },
    { "en": "Apa Itu HVDC VSC?", "id": "Voltage Source Converter (IGBT)." },
    { "en": "Apa Keunggulan HVDC VSC?", "id": "Bisa Black Start Dan Kontrol Aktif." },
    { "en": "Apa Itu Solder Paste Type 3?", "id": "Butiran Timah 25 Hingga 45 Mikron." },
    { "en": "Apa Itu Solder Paste Type 4?", "id": "Butiran Timah 20 Hingga 38 Mikron." },
    { "en": "Apa Itu Flux R (Rosin)?", "id": "Flux Getah Pinus Tidak Aktif." },
    { "en": "Apa Itu Flux RMA?", "id": "Rosin Mildly Activated Sedikit Aktif." },
    { "en": "Apa Itu Flux RA?", "id": "Rosin Activated Sangat Aktif Korosif." },
    { "en": "Apa Itu Reflow Zone Liquidus?", "id": "Fase Timah Mencair Di Oven." },
    { "en": "Apa Itu PCB Finish OSP?", "id": "Organic Solderability Preservative." },
    { "en": "Apa Itu PCB Finish ENIG?", "id": "Electroless Nickel Immersion Gold." },
    { "en": "Apa Itu PCB Finish HASL?", "id": "Hot Air Solder Leveling." },
    { "en": "Apa Itu MQTT Retain Message?", "id": "Pesan Disimpan Broker Untuk Pelanggan." },
    { "en": "Apa Itu MQTT Last Will?", "id": "Pesan Otomatis Saat Putus Koneksi." },
    { "en": "Apa Itu LoRaWAN Class B?", "id": "Terima Data Jadwal Ping Beacon." },
    { "en": "Apa Itu ESP32 Deep Sleep Wake?", "id": "Bangun Dari Tidur Lewat Timer GPIO." },
    { "en": "Apa Itu Arduino Bootloader Size?", "id": "Sekitar 0.5KB Hingga 2KB." },
    { "en": "Apa Itu Turndown Ratio Flowmeter?", "id": "Rentang Ukur Maksimum Banding Minimum." },
    { "en": "Apa Akurasi Pressure Gauge Class 1.0?", "id": "Error Maksimal 1 Persen Skala." },
    { "en": "Apa Akurasi Pressure Gauge Class 0.5?", "id": "Error Maksimal 0.5 Persen Skala." },
    { "en": "Apa Warna Kabel Thermocouple K ANSI?", "id": "Kuning Positif Dan Merah Negatif." },
    { "en": "Apa Warna Kabel Thermocouple J ANSI?", "id": "Putih Positif Dan Merah Negatif." },
    { "en": "Apa Warna Kabel Thermocouple T ANSI?", "id": "Biru Positif Dan Merah Negatif." },
    { "en": "Apa Itu EV Charging Level 1?", "id": "Cas Lambat 120V Standar US." },
    { "en": "Apa Itu EV Charging Level 2?", "id": "Cas Menengah 240V AC." },
    { "en": "Apa Itu EV Charging Level 3?", "id": "Cas Cepat DC (DCFC)." },
    { "en": "Apa Itu V2G (Vehicle to Grid)?", "id": "Mobil Suplai Listrik Ke Jaringan." },
    { "en": "Apa Itu Demand Response?", "id": "Konsumen Kurangi Beban Saat Puncak." },
    { "en": "Apa Itu Virtual Power Plant?", "id": "Gabungan Pembangkit Tersebar Jadi Satu." },
    { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Bisa Mandiri." },
    { "en": "Apa Itu Islanding Mode Microgrid?", "id": "Operasi Terpisah Dari Grid Utama." },
    { "en": "Apa Itu Black Start Capability?", "id": "Start Pembangkit Tanpa Listrik Luar." },
    { "en": "Apa Itu Load Shedding Scheme?", "id": "Skema Lepas Beban Cegah Blackout." },
    { "en": "Apa Itu Under Frequency Relay?", "id": "Trip Saat Frekuensi Turun Drastis." },
    { "en": "Apa Itu ROCOF Protection?", "id": "Rate of Change of Frequency." },
    { "en": "Apa Itu Vector Shift Protection?", "id": "Deteksi Perubahan Sudut Fasa Tegangan." },
    { "en": "Apa Itu Distance Protection Relay?", "id": "Proteksi Berdasar Impedansi Kabel." },
    { "en": "Apa Itu Differential Protection Relay?", "id": "Bandingkan Arus Masuk Dan Keluar." },
    { "en": "Apa Itu Restricted Earth Fault?", "id": "Proteksi Bocor Tanah Area Terbatas." },
    { "en": "Apa Itu Negative Sequence Relay?", "id": "Proteksi Ketidakseimbangan Beban Fasa." },
    { "en": "Apa Itu Thermal Overload Relay?", "id": "Proteksi Panas Akibat Arus Lebih." },
    { "en": "Apa Itu Buchholz Relay Trafo?", "id": "Deteksi Gas Akibat Fault Internal." },
    { "en": "Apa Itu Jansen Relay?", "id": "Proteksi Tekanan Tap Changer Trafo." },
    { "en": "Apa Itu Sudden Pressure Relay?", "id": "Deteksi Lonjakan Tekanan Tangki Trafo." },
    { "en": "Apa Itu PRV Trafo?", "id": "Pressure Relief Valve Buang Tekanan." },
    { "en": "Apa Itu Oil Level Indicator?", "id": "Penunjuk Tinggi Minyak Konservator." },
    { "en": "Apa Itu Winding Temperature Indicator?", "id": "Termometer Suhu Belitan Trafo." },
    { "en": "Apa Itu Oil Temperature Indicator?", "id": "Termometer Suhu Minyak Atas Trafo." },
    { "en": "Apa Itu Breather Trafo?", "id": "Penyaring Udara Pernapasan Trafo." },
    { "en": "Apa Isi Silica Gel Breather?", "id": "Butiran Penyerap Kelembapan Udara." },
    { "en": "Apa Warna Silica Gel Jenuh?", "id": "Merah Muda Atau Putih Pucat." },
    { "en": "Apa Itu Radiator Trafo?", "id": "Sirip Pendingin Sirkulasi Minyak." },
    { "en": "Apa Itu Fan Cooling Trafo?", "id": "Kipas Pendingin Radiator Trafo." },
    { "en": "Apa Itu Pump Cooling Trafo?", "id": "Pompa Sirkulasi Minyak Trafo." },
    { "en": "Apa Itu ONAN Cooling?", "id": "Oil Natural Air Natural." },
    { "en": "Apa Itu ONAF Cooling?", "id": "Oil Natural Air Forced Kipas." },
    { "en": "Apa Itu OFAF Cooling?", "id": "Oil Forced Pompa Air Forced." },
    { "en": "Apa Itu OFWF Cooling?", "id": "Oil Forced Water Forced Air." },
    { "en": "Apa Itu Dry Type Transformer?", "id": "Trafo Tanpa Minyak Cast Resin." },
    { "en": "Apa Kelebihan Trafo Kering?", "id": "Tahan Api Dan Beban Perawatan." },
    { "en": "Apa Itu Cast Resin Trafo?", "id": "Belitan Dicor Resin Epoksi Keras." },
    { "en": "Apa Itu Vector Group Dyn5?", "id": "Delta Star Geser 150 Derajat." },
    { "en": "Apa Itu Impedansi Trafo (Z%)?", "id": "Persen Tegangan Saat Hubung Singkat." },
    { "en": "Apa Efek Impedansi Trafo Tinggi?", "id": "Arus Hubung Singkat Lebih Kecil." },
    { "en": "Apa Efek Impedansi Trafo Rendah?", "id": "Regulasi Tegangan Lebih Baik." },
    { "en": "Apa Itu Excitation Current?", "id": "Arus Magnetisasi Inti Tanpa Beban." },
    { "en": "Apa Itu No Load Loss?", "id": "Rugi Besi Inti Trafo." },
    { "en": "Apa Itu Load Loss?", "id": "Rugi Tembaga I Kuadrat R." },
    { "en": "Apa Itu Efficiency Trafo?", "id": "Output Dibagi Output Tambah Rugi." },
    { "en": "Apa Itu Voltage Regulation Trafo?", "id": "Beda Tegangan Beban Penuh Kosong." },
    { "en": "Apa Itu Tap Changer?", "id": "Pengubah Rasio Lilitan Tegangan." },
    { "en": "Apa Itu OLTC Trafo?", "id": "On Load Tap Changer Berbeban." },
    { "en": "Apa Itu DETC Trafo?", "id": "De Energized Tap Changer Mati." },
    { "en": "Apa Itu Bushing Tegangan Tinggi?", "id": "Terminal Isolator Masuk Tangki." },
    { "en": "Apa Itu Spark Gap Bushing?", "id": "Celah Pelindung Surja Petir." },
    { "en": "Apa Itu Conservator Tank?", "id": "Tangki Ekspansi Minyak Trafo." },
    { "en": "Apa Itu Diaphragm Conservator?", "id": "Karet Pemisah Minyak Dan Udara." },
    { "en": "Apa Itu Nitrogen Cushion Trafo?", "id": "Gas Nitrogen Di Atas Minyak." },
    { "en": "Apa Itu Hermetically Sealed Trafo?", "id": "Trafo Tertutup Las Tanpa Konservator." },
    { "en": "Apa Itu Corrugated Tank?", "id": "Tangki Bersirip Gelombang Pendingin." },
    { "en": "Apa Fungsi Distance Relay Zone 1?", "id": "Proteksi Instan 80 Persen Saluran." },
    { "en": "Apa Fungsi Distance Relay Zone 2?", "id": "Proteksi Tunda 120 Persen Saluran." },
    { "en": "Apa Fungsi Distance Relay Zone 3?", "id": "Proteksi Cadangan Jarak Jauh." },
    { "en": "Apa Itu Power Swing Blocking?", "id": "Cegah Trip Saat Ayunan Daya." },
    { "en": "Apa Itu Switch On To Fault?", "id": "Trip Instan Saat Tutup Ke Gangguan." },
    { "en": "Apa Itu Auto Reclose Dead Time?", "id": "Waktu Tunggu Sebelum Tutup Balik." },
    { "en": "Apa Itu Single Pole Auto Reclose?", "id": "Tutup Balik Hanya Fasa Gangguan." },
    { "en": "Apa Itu Three Pole Auto Reclose?", "id": "Tutup Balik Tiga Fasa Serentak." },
    { "en": "Apa Itu Pilot Wire Protection?", "id": "Proteksi Diferensial Lewat Kabel Komunikasi." },
    { "en": "Apa Itu Line Current Differential?", "id": "Bandingkan Arus Ujung Ke Ujung." },
    { "en": "Apa Itu Directional Overcurrent Relay?", "id": "Proteksi Arus Lebih Arah Tertentu." },
    { "en": "Apa Itu Busbar Check Zone?", "id": "Zona Deteksi Gangguan Busbar Umum." },
    { "en": "Apa Itu Breaker Failure Protection?", "id": "Trip Breaker Lain Jika Gagal." },
    { "en": "Apa Itu Trip Circuit Supervision?", "id": "Monitor Kesiapan Rangkaian Trip Coil." },
    { "en": "Apa Itu DC Supply Supervision?", "id": "Monitor Tegangan Baterai Suplai Relay." },
    { "en": "Apa Itu VT Fuse Failure?", "id": "Deteksi Sekering Trafo Tegangan Putus." },
    { "en": "Apa Itu CT Supervision?", "id": "Deteksi Rangkaian Arus Terbuka." },
    { "en": "Apa Itu Event Recorder Relay?", "id": "Perekam Urutan Kejadian Gangguan." },
    { "en": "Apa Itu Disturbance Recorder?", "id": "Perekam Bentuk Gelombang Saat Gangguan." },
    { "en": "Apa Itu Integrated Services Digital Network?", "id": "Jaringan Telepon Digital (ISDN)." },
    { "en": "Apa Itu Asynchronous Transfer Mode?", "id": "Teknologi Jaringan Sel Tetap (ATM)." },
    { "en": "Apa Itu Frame Relay Network?", "id": "Protokol WAN Paket Data Lama." },
    { "en": "Apa Itu X.25 Protocol?", "id": "Protokol Paket Switching Sangat Tua." },
    { "en": "Apa Itu FDDI Network?", "id": "Fiber Distributed Data Interface Ring." },
    { "en": "Apa Itu Token Ring Network?", "id": "Akses Jaringan Estafet Token." },
    { "en": "Apa Itu Dial-Up Connection?", "id": "Internet Lewat Saluran Telepon Analog." },
    { "en": "Apa Itu ADSL Modem?", "id": "Asymmetric Digital Subscriber Line." },
    { "en": "Apa Itu VDSL Modem?", "id": "Very High Speed Digital Subscriber Line." },
    { "en": "Apa Itu DSLAM?", "id": "Digital Subscriber Line Access Multiplexer." },
    { "en": "Apa Itu GPON (Gigabit Passive Optical Network)?", "id": "Jaringan Fiber Optik Ke Rumah." },
    { "en": "Apa Itu OLT (Optical Line Terminal)?", "id": "Pusat Pengendali Jaringan GPON." },
    { "en": "Apa Itu ONU (Optical Network Unit)?", "id": "Modem Fiber Di Sisi User." },
    { "en": "Apa Itu Splitter Ratio 1:32?", "id": "Satu Fiber Bagi 32 Pelanggan." },
    { "en": "Apa Itu Biomass Gasification?", "id": "Ubah Biomassa Padat Jadi Gas." },
    { "en": "Apa Itu Syngas (Synthesis Gas)?", "id": "Gas Hasil Gasifikasi (CO H2)." },
    { "en": "Apa Itu Anaerobic Digestion?", "id": "Fermentasi Tanpa Udara Hasilkan Biogas." },
    { "en": "Apa Komponen Utama Biogas?", "id": "Gas Metana Dan Karbon Dioksida." },
    { "en": "Apa Itu Biodiesel B30?", "id": "Campuran 30% Nabati 70% Solar." },
    { "en": "Apa Itu Geothermal Binary Cycle?", "id": "Siklus Uap Fluida Kerja Sekunder." },
    { "en": "Apa Itu Dry Steam Geothermal?", "id": "Uap Kering Langsung Putar Turbin." },
    { "en": "Apa Itu Flash Steam Geothermal?", "id": "Air Panas Tekanan Tinggi Diuapkan." },
    { "en": "Apa Itu Tip Speed Ratio?", "id": "Rasio Kecepatan Ujung Baling Angin." },
    { "en": "Apa Itu Yaw Mechanism Turbin?", "id": "Putar Turbin Menghadap Arah Angin." },
    { "en": "Apa Itu Pitch Control Turbin?", "id": "Atur Sudut Baling Kendalikan Putaran." },
    { "en": "Apa Itu Cut-in Wind Speed?", "id": "Kecepatan Angin Mulai Hasilkan Listrik." },
    { "en": "Apa Itu Cut-out Wind Speed?", "id": "Kecepatan Angin Maksimal Turbin Berhenti." },
    { "en": "Apa Itu Vernier Caliper?", "id": "Jangka Sorong Ukur Dimensi." },
    { "en": "Apa Itu Micrometer Screw Gauge?", "id": "Alat Ukur Ketebalan Sangat Presisi." },
    { "en": "Apa Itu Dial Indicator Gauge?", "id": "Alat Ukur Kerataan Permukaan Benda." },
    { "en": "Apa Itu Gauge Block?", "id": "Balok Baja Standar Ukuran Presisi." },
    { "en": "Apa Itu Feeler Gauge?", "id": "Plat Tipis Ukur Celah Sempit." },
    { "en": "Apa Itu Thread Pitch Gauge?", "id": "Alat Ukur Jarak Ulir Baut." },
    { "en": "Apa Itu Torque Wrench Click?", "id": "Kunci Torsi Bunyi Klik." },
    { "en": "Apa Itu Torque Wrench Digital?", "id": "Kunci Torsi Layar Angka." },
    { "en": "Apa Itu Hardness Tester Rockwell?", "id": "Uji Kekerasan Logam Metode Indentasi." },
    { "en": "Apa Itu Surface Roughness Tester?", "id": "Ukur Kekasaran Permukaan Benda Kerja." },
    { "en": "Apa Itu Coordinate Measuring Machine (CMM)?", "id": "Mesin Ukur Dimensi 3D Presisi." },
    { "en": "Apa Itu Laser Distance Meter?", "id": "Meteran Jarak Sinar Laser Digital." },
    { "en": "Apa Itu FinFET Transistor?", "id": "Transistor Efisiensi Tinggi Sirip 3D." },
    { "en": "Apa Itu Gate Oxide Breakdown?", "id": "Kerusakan Isolasi Gate MOSFET." },
    { "en": "Apa Itu Electromigration Chip?", "id": "Perpindahan Atom Logam Akibat Arus." },
    { "en": "Apa Itu Dark Current Sensor?", "id": "Noise Sensor Kamera Saat Gelap." },
    { "en": "Apa Itu Hot Pixel Sensor?", "id": "Piksel Cacat Selalu Putih." },
    { "en": "Apa Itu Dead Pixel Sensor?", "id": "Piksel Cacat Selalu Hitam." },
    { "en": "Apa Itu Bayer Filter Array?", "id": "Filter Warna Sensor Kamera Digital." },
    { "en": "Apa Itu CCD Sensor?", "id": "Charge Coupled Device Sensor Gambar." },
    { "en": "Apa Itu CMOS Sensor?", "id": "Complementary Metal Oxide Semiconductor Sensor." },
    { "en": "Apa Keunggulan Sensor CMOS?", "id": "Hemat Daya Dan Baca Cepat." },
    { "en": "Apa Keunggulan Sensor CCD?", "id": "Kualitas Gambar Rendah Noise." },
    { "en": "Apa Itu Rolling Shutter Effect?", "id": "Distorsi Gambar Objek Bergerak Cepat." },
    { "en": "Apa Itu Global Shutter?", "id": "Tangkap Gambar Seluruh Piksel Bersamaan." },
    { "en": "Apa Itu ISO Sensitivity?", "id": "Tingkat Kepekaan Sensor Terhadap Cahaya." },
    { "en": "Apa Itu PWM Dimming LED?", "id": "Atur Terang Dengan Kedipan Cepat." },
    { "en": "Apa Itu Analog Dimming 0-10V?", "id": "Atur Terang Dengan Tegangan Analog." },
    { "en": "Apa Itu Triac Dimming?", "id": "Atur Terang Dengan Potong Fasa." },
    { "en": "Apa Itu DALI Dimming?", "id": "Atur Terang Dengan Protokol Digital." },
    { "en": "Apa Itu CRI 95+?", "id": "Akurasi Warna Sangat Tinggi." },
    { "en": "Apa Itu R9 Value LED?", "id": "Nilai Render Warna Merah Jenuh." },
    { "en": "Apa Itu MacAdam Ellipse?", "id": "Standar Konsistensi Warna Lampu LED." },
    { "en": "Apa Itu Binning LED?", "id": "Pengelompokan LED Berdasarkan Karakteristik." },
    { "en": "Apa Itu Lumen Maintenance L70?", "id": "Waktu Hingga Cahaya Sisa 70%." },
    { "en": "Apa Itu UGR (Unified Glare Rating)?", "id": "Indeks Kesilauan Cahaya Ruangan." },
    { "en": "Apa Itu IP65 Led Strip?", "id": "Lampu Pita Tahan Cipratan Air." },
    { "en": "Apa Itu IP68 Led Strip?", "id": "Lampu Pita Tahan Rendam Air." },
    { "en": "Apa Itu COB LED Strip?", "id": "Chip On Board Tanpa Bintik." },
    { "en": "Apa Itu NeoPixel LED?", "id": "LED RGB Addressable (WS2812B)." },
    { "en": "Apa Itu WLED Controller?", "id": "Kontroler LED Strip WiFi ESP." },
    { "en": "Apa Itu Zigbee Light Link?", "id": "Protokol Lampu Pintar Philips Hue." },
    { "en": "Apa Itu Home Assistant?", "id": "Server Otomasi Rumah Open Source." },
    { "en": "Apa Itu ESPHome Firmware?", "id": "Firmware Kustom Untuk Sensor ESP." },
    { "en": "Apa Itu Tasmota Firmware?", "id": "Firmware Alternatif Perangkat Sonoff." },
    { "en": "Apa Itu Smart Plug Power Monitoring?", "id": "Colokan Pintar Ukur Daya Listrik." },
    { "en": "Apa Itu Neutral Wire Switch?", "id": "Saklar Pintar Perlu Kabel Netral." },
    { "en": "Apa Itu No Neutral Switch?", "id": "Saklar Pintar Tanpa Kabel Netral." },
    { "en": "Apa Itu Capacitor Bypass Switch?", "id": "Cegah Lampu Kedip Saklar No-Neutral." },
    { "en": "Apa Itu Scene Controller?", "id": "Tombol Pemicu Aksi Otomasi Rumah." },
    { "en": "Apa Itu IR Blaster Smart?", "id": "Remote Universal WiFi Pengganti Remote." },
    { "en": "Apa Itu Door Window Sensor?", "id": "Sensor Magnet Pintu Jendela Zigbee." },
    { "en": "Apa Itu Kodek Video H.264?", "id": "Standar Kompresi Video AVC Umum." },
    { "en": "Apa Itu Kodek Video H.265 (HEVC)?", "id": "Kompresi Efisiensi Tinggi Separuh H.264." },
    { "en": "Apa Itu Kodek Video VP9?", "id": "Kompresi Video Open Source Google." },
    { "en": "Apa Itu Kodek Video AV1?", "id": "Kodek Bebas Royalti Penerus VP9." },
    { "en": "Apa Itu Bitrate Video?", "id": "Jumlah Data Video Per Detik." },
    { "en": "Apa Itu VBR (Variable Bitrate)?", "id": "Bitrate Berubah Sesuai Kompleksitas Gambar." },
    { "en": "Apa Itu CBR (Constant Bitrate)?", "id": "Bitrate Tetap Stabil Sepanjang Video." },
    { "en": "Apa Itu Keyframe Video?", "id": "Frame Gambar Penuh Tanpa Kompresi." },
    { "en": "Apa Itu Chroma Subsampling 4:2:0?", "id": "Kompresi Warna Hemat Bandwidth." },
    { "en": "Apa Itu Resolusi 8K UHD?", "id": "7680 Kali 4320 Piksel." },
    { "en": "Apa Itu Kabel HDMI Fiber Optik?", "id": "Kabel HDMI Jarak Jauh Hibrida." },
    { "en": "Apa Itu HDCP 2.3?", "id": "Proteksi Konten Digital Terbaru." },
    { "en": "Apa Itu EDID Handshake?", "id": "Komunikasi Kemampuan Layar Ke Sumber." },
    { "en": "Apa Itu Video Latency?", "id": "Jeda Waktu Gambar Tampil." },
    { "en": "Apa Itu Frame Rate 60fps?", "id": "Enam Puluh Gambar Per Detik." },
    { "en": "Apa Itu Refresh Rate 120Hz?", "id": "Layar Update 120 Kali Sedetik." },
    { "en": "Apa Itu G-Sync Monitor?", "id": "Sinkronisasi Layar Dengan GPU Nvidia." },
    { "en": "Apa Itu FreeSync Monitor?", "id": "Sinkronisasi Layar Dengan GPU AMD." },
    { "en": "Apa Itu Panel IPS?", "id": "Layar Sudut Pandang Luas Warna Akurat." },
    { "en": "Apa Itu Panel TN?", "id": "Layar Respon Cepat Sudut Sempit." },
    { "en": "Apa Itu Panel VA?", "id": "Layar Kontras Tinggi Hitam Pekat." },
    { "en": "Apa Itu Local Dimming?", "id": "Lampu Latar Mati Di Area Gelap." },
    { "en": "Apa Itu Mini LED?", "id": "Lampu Latar LED Ukuran Kecil." },
    { "en": "Apa Itu Micro LED?", "id": "Piksel LED Mikroskopis Tanpa Backlight." },
    { "en": "Apa Itu Firewall Deep Packet Inspection?", "id": "Analisis Isi Paket Data Mendalam." },
    { "en": "Apa Itu Zero Trust Security?", "id": "Tidak Percaya Siapapun Verifikasi Selalu." },
    { "en": "Apa Itu Air Gapped Network?", "id": "Jaringan Fisik Terpisah Dari Internet." },
    { "en": "Apa Itu Data Diode?", "id": "Alat Kirim Data Satu Arah Fisik." },
    { "en": "Apa Itu Honeypot Network?", "id": "Jebakan Palsu Untuk Menarik Peretas." },
    { "en": "Apa Itu IPSec VPN?", "id": "VPN Enkripsi Paket IP Level Network." },
    { "en": "Apa Itu SSL VPN?", "id": "VPN Akses Web Enkripsi Browser." },
    { "en": "Apa Itu SIEM System?", "id": "Manajemen Informasi Dan Event Keamanan." },
    { "en": "Apa Itu Penetration Test (Pentest)?", "id": "Simulasi Serangan Uji Celah Keamanan." },
    { "en": "Apa Itu Brute Force Attack?", "id": "Tebak Password Dengan Mencoba Semua." },
    { "en": "Apa Itu Dictionary Attack?", "id": "Tebak Password Pakai Daftar Kata." },
    { "en": "Apa Itu Social Engineering?", "id": "Manipulasi Psikologis Manusia Curi Data." },
    { "en": "Apa Itu Phishing Attack?", "id": "Email Palsu Pancing Klik Link." },
    { "en": "Apa Itu Malware Trojan?", "id": "Program Jahat Menyamar Jadi Baik." },
    { "en": "Apa Itu Malware Worm?", "id": "Program Jahat Menyebar Sendiri Jaringan." },
    { "en": "Apa Itu Rootkit?", "id": "Malware Sembunyi Di Level Sistem." },
    { "en": "Apa Itu Baterai Solid State?", "id": "Baterai Elektrolit Padat Aman Ledakan." },
    { "en": "Apa Itu Dendrite Lithium?", "id": "Kristal Tajam Tumbuh Tembus Separator." },
    { "en": "Apa Bahaya Dendrite Baterai?", "id": "Hubung Singkat Internal Dan Terbakar." },
    { "en": "Apa Itu Anoda Silikon?", "id": "Pengganti Grafit Kapasitas Lebih Besar." },
    { "en": "Apa Itu Baterai Sodium-Ion?", "id": "Baterai Bahan Garam Murah." },
    { "en": "Apa Itu Baterai Lithium-Sulfur?", "id": "Baterai Densitas Energi Sangat Tinggi." },
    { "en": "Apa Itu Gravimetric Energy Density?", "id": "Energi Per Satuan Berat (Wh/kg)." },
    { "en": "Apa Itu Volumetric Energy Density?", "id": "Energi Per Satuan Volume (Wh/L)." },
    { "en": "Apa Itu Thermal Management Baterai?", "id": "Sistem Pendingin Pemanas Pack Baterai." },
    { "en": "Apa Itu Liquid Cooling Baterai?", "id": "Pendingin Cairan Sirkulasi Antar Sel." },
    { "en": "Apa Itu Phase Change Material (PCM)?", "id": "Material Penyerap Panas Baterai Pasif." },
    { "en": "Apa Itu Battery Degassing?", "id": "Pelepasan Gas Saat Baterai Rusak." },
    { "en": "Apa Itu Battery Venting Valve?", "id": "Katup Buang Tekanan Gas Berlebih." },
    { "en": "Apa Itu Battery Module?", "id": "Kumpulan Sel Baterai Dalam Casing." },
    { "en": "Apa Itu Battery Pack?", "id": "Gabungan Modul Baterai Dan BMS." },
    { "en": "Apa Itu Kabel UTP Plenum (CMP)?", "id": "Kabel Tahan Api Ruang Plafon." },
    { "en": "Apa Itu Kabel UTP Riser (CMR)?", "id": "Kabel Jalur Vertikal Antar Lantai." },
    { "en": "Apa Itu Kabel LSZH (Low Smoke)?", "id": "Minim Asap Saat Terbakar." },
    { "en": "Apa Itu Kabel Outdoor UV Rated?", "id": "Kulit Kabel Tahan Sinar Matahari." },
    { "en": "Apa Itu Kabel Direct Burial?", "id": "Kabel Tanam Langsung Tanpa Pipa." },
    { "en": "Apa Itu Kabel Aerial Messenger?", "id": "Kabel Udara Dengan Kawat Penggantung." },
    { "en": "Apa Itu Patch Panel 24 Port?", "id": "Terminal Kabel Jaringan 24 Lubang." },
    { "en": "Apa Itu Cable Manager?", "id": "Rak Rapikan Kabel Di Server." },
    { "en": "Apa Itu Rack Server 19 Inch?", "id": "Lebar Standar Lemari Perangkat IT." },
    { "en": "Apa Itu Cage Nut (Mur Sangkar)?", "id": "Mur Kotak Untuk Rak Server." },
    { "en": "Apa Itu PDU Metered?", "id": "Stop Kontak Rak Dengan Layar Ampere." },
    { "en": "Apa Itu PDU Switched?", "id": "Stop Kontak Rak Bisa Remote On/Off." },
    { "en": "Apa Itu Raised Floor Data Center?", "id": "Lantai Panggung Ruang Kabel Bawah." },
    { "en": "Apa Itu Perforated Tile?", "id": "Ubin Berlubang Sirkulasi Udara Dingin." },
    { "en": "Apa Itu Hot Aisle Containment?", "id": "Lorong Panas Tertutup Pintu." },
    { "en": "Apa Itu In-Row Cooling?", "id": "AC Berdiri Di Antara Rak Server." },
    { "en": "Apa Itu Immersion Cooling Server?", "id": "Server Dicelup Cairan Dielektrik." },
    { "en": "Apa Itu Dry Cooler?", "id": "Pendingin Air Tertutup Kipas Udara." },
    { "en": "Apa Itu Free Cooling?", "id": "Manfaatkan Udara Luar Dingin Hemat Energi." },
    { "en": "Apa Itu PUE (Power Usage Effectiveness)?", "id": "Rasio Efisiensi Energi Data Center." },
    { "en": "Apa Nilai PUE 1.0?", "id": "Sangat Efisien Tanpa Rugi Pendinginan." },
    { "en": "Apa Itu Tier 1 Data Center?", "id": "Tanpa Redundansi (Jalur Tunggal)." },
    { "en": "Apa Itu Tier 4 Data Center?", "id": "Toleransi Kesalahan Penuh (2N+1)." },
    { "en": "Apa Itu Generator N+1?", "id": "Satu Genset Cadangan Siaga." },
    { "en": "Apa Itu UPS 2N Redundancy?", "id": "Dua Sistem UPS Terpisah Penuh." },
    { "en": "Apa Itu STS (Static Transfer Switch)?", "id": "Saklar Pindah Sumber Sangat Cepat." },
    { "en": "Apa Itu EPO (Emergency Power Off)?", "id": "Tombol Matikan Total Saat Darurat." },
    { "en": "Apa Itu VESDA System?", "id": "Deteksi Asap Aspirasi Sangat Dini." },
    { "en": "Apa Itu Gas FM200?", "id": "Gas Pemadam Api Ruang Server." },
    { "en": "Apa Itu Gas Novec 1230?", "id": "Cairan Pemadam Api Ramah Lingkungan." },
    { "en": "Apa Itu Water Mist System?", "id": "Pemadam Kabut Air Tekanan Tinggi." },
    { "en": "Apa Itu Hypoxic Air Fire Prevention?", "id": "Kurangi Oksigen Ruangan Cegah Api." },
    { "en": "Apa Itu Access Control Biometric?", "id": "Kunci Pintu Sidik Jari Wajah." },
    { "en": "Apa Itu Mantrap Door?", "id": "Ruang Antara Dua Pintu Keamanan." },
    { "en": "Apa Itu CCTV Fisheye?", "id": "Kamera Lensa Lebar 360 Derajat." },
    { "en": "Apa Itu CCTV PTZ?", "id": "Kamera Bisa Putar Zoom Gerak." },
    { "en": "Apa Itu Video Analytics?", "id": "Analisis Otomatis Rekaman Kamera CCTV." },
    { "en": "Apa Itu Face Recognition CCTV?", "id": "Kenali Wajah Orang Dari Video." },
    { "en": "Apa Itu LPR Camera?", "id": "License Plate Recognition (Baca Plat)." },
    { "en": "Apa Itu Fiber Optic ODF?", "id": "Optical Distribution Frame (Rak Fiber)." },
    { "en": "Apa Itu Splice Cassette?", "id": "Wadah Pelindung Sambungan Fiber ODF." },
    { "en": "Apa Itu Pigtail Fiber?", "id": "Kabel Pendek Ujung Konektor Splicing." },
    { "en": "Apa Itu Adapter Coupler Fiber?", "id": "Penyambung Dua Konektor Fiber." },
    { "en": "Apa Itu Attenuator Fiber LC?", "id": "Peredam Sinyal Konektor LC." },
    { "en": "Apa Itu Pipeline Hazard?", "id": "Konflik Data Saat Eksekusi Instruksi." },
    { "en": "Apa Itu Branch Prediction?", "id": "CPU Tebak Alur Program Berikutnya." },
    { "en": "Apa Itu Context Switching?", "id": "Simpan Status Saat Ganti Tugas." },
    { "en": "Apa Itu Atomic Operation?", "id": "Instruksi Tak Bisa Dipotong Interupsi." },
    { "en": "Apa Itu Memory Mapped I/O?", "id": "Alamat Hardware Sama Dengan Memori." },
    { "en": "Apa Itu Harvard Architecture?", "id": "Bus Data Dan Program Terpisah." },
    { "en": "Apa Itu Von Neumann Architecture?", "id": "Bus Data Dan Program Gabung." },
    { "en": "Apa Itu JTAG Boundary Scan?", "id": "Tes Kaki IC Tanpa Probe." },
    { "en": "Apa Itu Daisy Chain JTAG?", "id": "Seri Banyak Chip Satu Port." },
    { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Debug ARM Dua Kabel Saja." },
    { "en": "Apa Fungsi FFT Windowing?", "id": "Kurangi Kebocoran Spektral Sinyal." },
    { "en": "Apa Itu Hamming Window?", "id": "Jenis Jendela Filter Sinyal DSP." },
    { "en": "Apa Itu Hanning Window?", "id": "Jendela Filter Redam Sidelobe." },
    { "en": "Apa Itu Convolution DSP?", "id": "Operasi Gabung Dua Sinyal Input." },
    { "en": "Apa Itu Impulse Response?", "id": "Output Sistem Saat Input Impuls." },
    { "en": "Apa Kelebihan Filter FIR?", "id": "Fasa Linear Dan Selalu Stabil." },
    { "en": "Apa Kelebihan Filter IIR?", "id": "Efisien Memori Mirip Analog." },
    { "en": "Apa Itu Fixed Point DSP?", "id": "Hitungan Bilangan Bulat Cepat." },
    { "en": "Apa Itu Floating Point DSP?", "id": "Hitungan Desimal Presisi Tinggi." },
    { "en": "Apa Itu SNR (Signal to Noise)?", "id": "Rasio Kekuatan Sinyal Dan Noise." },
    { "en": "Apa Itu Eb/N0 (Energy per Bit)?", "id": "Ukuran Kualitas Sinyal Digital." },
    { "en": "Apa Itu Link Budget Satelit?", "id": "Hitungan Total Gain Dan Loss." },
    { "en": "Apa Itu EIRP Satelit?", "id": "Daya Pancar Efektif Terarah." },
    { "en": "Apa Itu G/T Ratio?", "id": "Kualitas Penerimaan Antena Satelit." },
    { "en": "Apa Itu Look Angle?", "id": "Sudut Antena Ke Satelit." },
    { "en": "Apa Itu Azimuth Angle?", "id": "Arah Horizontal Mata Angin." },
    { "en": "Apa Itu Elevation Angle?", "id": "Sudut Dongak Vertikal Ke Langit." },
    { "en": "Apa Itu Polarization Skew?", "id": "Miringnya Polarisasi Akibat Posisi Bumi." },
    { "en": "Apa Itu Rain Fade?", "id": "Sinyal Lemah Akibat Butiran Hujan." },
    { "en": "Apa Itu Sun Outage?", "id": "Gangguan Sinyal Akibat Posisi Matahari." },
    { "en": "Apa Itu Tin Whiskers?", "id": "Kristal Timah Tumbuh Sebabkan Short." },
    { "en": "Apa Penyebab Tin Whiskers?", "id": "Tegangan Mekanis Pada Timah Murni." },
    { "en": "Apa Itu Intermetallic Compound?", "id": "Paduan Logam Antara Timah Tembaga." },
    { "en": "Apa Itu Solder Wetting?", "id": "Timah Cair Menyebar Rata." },
    { "en": "Apa Itu Dewetting?", "id": "Timah Menarik Diri Dari Pad." },
    { "en": "Apa Itu Cold Solder Joint?", "id": "Solderan Kusam Tidak Menyatu." },
    { "en": "Apa Itu Solder Void?", "id": "Rongga Udara Dalam Sambungan Timah." },
    { "en": "Apa Itu Electromigration?", "id": "Perpindahan Atom Logam Akibat Arus." },
    { "en": "Apa Itu Dendrite Growth?", "id": "Pertumbuhan Kristal Logam Akibat Lembap." },
    { "en": "Apa Itu Conformal Coating Acrylic?", "id": "Pelapis PCB Mudah Diperbaiki." },
    { "en": "Apa Itu Conformal Coating Silicone?", "id": "Pelapis PCB Tahan Panas Tinggi." },
    { "en": "Apa Itu PCB Delamination?", "id": "Lapisan PCB Terkelupas Akibat Panas." },
    { "en": "Apa Itu CAF (Conductive Anodic Filament)?", "id": "Short Internal PCB Akibat Elektrokimia." },
    { "en": "Apa Itu Via Stub?", "id": "Sisa Lubang Via Mengganggu Sinyal." },
    { "en": "Apa Itu Backdrilling PCB?", "id": "Bor Buang Via Stub." },
    { "en": "Apa Itu Impedance Mismatch?", "id": "Penyebab Pantulan Sinyal Jalur." },
    { "en": "Apa Itu Reflection Coefficient?", "id": "Ukuran Sinyal Pantul Balik." },
    { "en": "Apa Itu Insertion Loss PCB?", "id": "Sinyal Hilang Sepanjang Jalur." },
    { "en": "Apa Itu Dielectric Loss?", "id": "Energi Serap Bahan PCB." },
    { "en": "Apa Itu Skin Effect Loss?", "id": "Hambatan Naik Di Frekuensi Tinggi." },
    { "en": "Apa Itu Bandgap Reference?", "id": "Sumber Tegangan Stabil Suhu." },
    { "en": "Apa Itu Thermal Runaway BJT?", "id": "Panas Naik Arus Naik Rusak." },
    { "en": "Apa Itu Secondary Breakdown?", "id": "Kerusakan Transistor Daya Tinggi." },
    { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Batas Aman Tegangan Arus Transistor." },
    { "en": "Apa Itu Miller Effect?", "id": "Kapasitansi Parasitik Penguat Inverting." },
    { "en": "Apa Itu Early Effect?", "id": "Lebar Basis Berubah Akibat Tegangan." },
    { "en": "Apa Itu Pinch-off Voltage JFET?", "id": "Tegangan Saat Arus Drain Nol." },
    { "en": "Apa Itu Threshold Voltage MOSFET?", "id": "Tegangan Gate Mulai Konduksi." },
    { "en": "Apa Itu Body Diode MOSFET?", "id": "Dioda Parasitik Bawaan Struktur." },
    { "en": "Apa Itu Reverse Recovery Dioda?", "id": "Waktu Dioda Berhenti Mengalir Balik." },
    { "en": "Apa Itu LiDAR Time of Flight?", "id": "Ukur Jarak Waktu Tempuh Cahaya." },
    { "en": "Apa Itu LiDAR Phase Shift?", "id": "Ukur Jarak Beda Fasa Gelombang." },
    { "en": "Apa Itu MEMS Accelerometer?", "id": "Sensor Percepatan Mekanis Mikro." },
    { "en": "Apa Itu MEMS Gyroscope?", "id": "Sensor Rotasi Mekanis Mikro." },
    { "en": "Apa Itu Coriolis Effect Gyro?", "id": "Prinsip Kerja Gyroscope MEMS." },
    { "en": "Apa Itu Piezoelectric Effect?", "id": "Tekanan Hasilkan Muatan Listrik." },
    { "en": "Apa Itu Piezoresistive Effect?", "id": "Tekanan Ubah Nilai Resistansi." },
    { "en": "Apa Itu Hall Effect Sensor?", "id": "Medan Magnet Hasilkan Tegangan." },
    { "en": "Apa Itu Seebeck Effect?", "id": "Beda Suhu Hasilkan Tegangan." },
    { "en": "Apa Itu Peltier Effect?", "id": "Arus Listrik Hasilkan Beda Suhu." },
    { "en": "Apa Itu Transformer Inrush?", "id": "Arus Kejut Saat Trafo Nyala." },
    { "en": "Apa Itu Sympathetic Inrush?", "id": "Arus Kejut Trafo Sebelah Nyala." },
    { "en": "Apa Itu Ferroresonance Trafo?", "id": "Osilasi Tegangan Lebih Non-linear." },
    { "en": "Apa Itu Tap Changer Arcing?", "id": "Percikan Api Saat Pindah Tap." },
    { "en": "Apa Itu Oil Sludge Trafo?", "id": "Endapan Lumpur Oksidasi Minyak." },
    { "en": "Apa Itu DGA (Dissolved Gas Analysis)?", "id": "Analisis Gas Terlarut Minyak Trafo." },
    { "en": "Apa Gas Asetilen Pada Trafo?", "id": "Indikasi Terjadi Busur Api (Arcing)." },
    { "en": "Apa Gas Hidrogen Pada Trafo?", "id": "Indikasi Terjadi Corona Discharge." },
    { "en": "Apa Gas Etilena Pada Trafo?", "id": "Indikasi Minyak Sangat Panas." },
    { "en": "Apa Itu Partial Discharge (PD)?", "id": "Peluahan Listrik Lokal Isolasi." },
    { "en": "Apa Itu Tan Delta Isolasi?", "id": "Ukuran Kualitas Rugi Dielektrik." },
    { "en": "Apa Itu Breakdown Voltage Minyak?", "id": "Tegangan Tembus Minyak Trafo." },
    { "en": "Apa Itu IEEE 802.11be?", "id": "Standar WiFi 7 Terbaru." },
    { "en": "Apa Itu IEEE 802.11ax?", "id": "Standar WiFi 6 Efisien." },
    { "en": "Apa Itu IEEE 802.3bt?", "id": "Standar PoE Daya Tinggi (100W)." },
    { "en": "Apa Itu IEEE 1588 PTP?", "id": "Protokol Sinkronisasi Waktu Presisi." },
    { "en": "Apa Itu Modbus RTU Framing?", "id": "Jeda Diam 3.5 Karakter." },
    { "en": "Apa Itu CRC Checksum?", "id": "Kode Deteksi Error Data." },
    { "en": "Apa Itu Parity Bit Serial?", "id": "Bit Cek Ganjil Genap Sederhana." },
    { "en": "Apa Itu Baud Rate Mismatch?", "id": "Kecepatan Data Tidak Sinkron." },
    { "en": "Apa Itu RS-485 Bias Resistor?", "id": "Jaga Level Tegangan Saat Idle." },
    { "en": "Apa Itu Ground Loop RS-485?", "id": "Arus Tanah Ganggu Sinyal Data." },
    { "en": "Apa Itu HART FSK?", "id": "Frequency Shift Keying Data." },
    { "en": "Apa Itu Fieldbus H1?", "id": "Jaringan Instrumen 31.25 kbps." },
    { "en": "Apa Itu Fieldbus Terminator?", "id": "Resistor 100 Ohm Dan Kapasitor." },
    { "en": "Apa Itu Manchester Encoding?", "id": "Kode Data Gabung Dengan Clock." },
    { "en": "Apa Itu NRZ Encoding?", "id": "Non Return To Zero." },
    { "en": "Apa Itu PWM Carrier Frequency?", "id": "Frekuensi Dasar Sinyal PWM." },
    { "en": "Apa Itu Dead Time PWM?", "id": "Jeda Aman Switching Transistor." },
    { "en": "Apa Itu EUV Lithography?", "id": "Cetak Chip Sinar Ultraviolet Ekstrem." },
    { "en": "Apa Itu Batas Hukum Moore?", "id": "Batas Fisik Ukuran Transistor." },
    { "en": "Apa Fungsi Wafer Stepper?", "id": "Mesin Proyeksi Pola Sirkuit Chip." },
    { "en": "Apa Fungsi Cleanroom Airlock?", "id": "Ruang Antara Cegah Kontaminasi Masuk." },
    { "en": "Apa Itu Through-Silicon Via?", "id": "Koneksi Vertikal Tembus Chip Silikon." },
    { "en": "Apa Itu System In Package?", "id": "Gabungan Banyak Chip Satu Kemasan." },
    { "en": "Apa Itu Die Attach Film?", "id": "Perekat Chip Ke Dudukan Substrat." },
    { "en": "Apa Itu Leadframe IC?", "id": "Rangka Logam Kaki-kaki Chip." },
    { "en": "Apa Itu Wire Bonding Emas?", "id": "Kawat Emas Sambung Chip Kaki." },
    { "en": "Apa Itu Flip Chip?", "id": "Pasang Chip Terbalik Tanpa Kawat." },
    { "en": "Apa Fungsi On-Board Charger EV?", "id": "Ubah AC Grid Jadi DC Baterai." },
    { "en": "Apa Fungsi Traction Inverter EV?", "id": "Ubah DC Baterai Jadi AC Motor." },
    { "en": "Apa Itu High Voltage Interlock?", "id": "Loop Pengaman Deteksi Kabel Putus." },
    { "en": "Apa Fungsi DC-DC Converter EV?", "id": "Turunkan Tegangan Tinggi Ke 12V." },
    { "en": "Apa Itu Battery Thermal Plate?", "id": "Plat Pendingin Bawah Modul Baterai." },
    { "en": "Apa Warna Kabel Tegangan Tinggi EV?", "id": "Oranye (Standar Internasional)." },
    { "en": "Apa Itu Regenerative Braking?", "id": "Pengereman Hasilkan Listrik Masuk Baterai." },
    { "en": "Apa Itu Siklus NEDC?", "id": "Standar Lama Uji Jarak Tempuh." },
    { "en": "Apa Itu Siklus WLTP?", "id": "Standar Baru Uji Jarak Tempuh." },
    { "en": "Apa Itu State Of Health Baterai?", "id": "Persentase Sisa Umur Pakai Baterai." },
    { "en": "Apa Fungsi Phasor Data Concentrator?", "id": "Kumpul Data Dari Banyak PMU." },
    { "en": "Apa Itu Wide Area Monitoring?", "id": "Pantau Grid Listrik Area Luas." },
    { "en": "Apa Itu Demand Side Management?", "id": "Atur Beban Konsumen Seimbangkan Grid." },
    { "en": "Apa Itu Peak Shaving?", "id": "Kurangi Beban Saat Puncak Pemakaian." },
    { "en": "Apa Itu Load Shifting?", "id": "Geser Pemakaian Ke Luar Puncak." },
    { "en": "Apa Itu Distributed Generation?", "id": "Pembangkit Listrik Dekat Titik Beban." },
    { "en": "Apa Fungsi Microgrid Controller?", "id": "Otak Pengatur Sumber Energi Mikrogrid." },
    { "en": "Apa Itu Grid Forming Inverter?", "id": "Inverter Penentu Tegangan Frekuensi Grid." },
    { "en": "Apa Itu Grid Following Inverter?", "id": "Inverter Mengikuti Gelombang Grid Ada." },
    { "en": "Apa Itu Dark Fiber Lease?", "id": "Sewa Kabel Optik Belum Aktif." },
    { "en": "Apa Itu Meet-Me Room?", "id": "Ruang Koneksi Antar Provider Internet." },
    { "en": "Apa Itu Cross Connect?", "id": "Kabel Penghubung Antar Perangkat Jaringan." },
    { "en": "Apa Itu Structured Cabling?", "id": "Standar Instalasi Kabel Gedung." },
    { "en": "Apa Itu Horizontal Cabling?", "id": "Kabel Dari Panel Ke Outlet." },
    { "en": "Apa Itu Backbone Cabling?", "id": "Kabel Utama Antar Ruang Server." },
    { "en": "Apa Itu Patch Cord Fiber?", "id": "Kabel Jumper Fiber Optik Pendek." },
    { "en": "Apa Itu Keystone Jack RJ45?", "id": "Lubang Konektor LAN Dinding." },
    { "en": "Apa Fungsi Cable Tray Mesh?", "id": "Rak Kabel Keranjang Kawat Terbuka." },
    { "en": "Apa Itu Rack Unit (U)?", "id": "Satuan Tinggi Perangkat Server Standard." },
    { "en": "Apa Fungsi Sensor Proximity Kapasitif?", "id": "Deteksi Benda Non-logam Jarak Dekat." },
    { "en": "Apa Fungsi Sensor Proximity Induktif?", "id": "Deteksi Benda Logam Jarak Dekat." },
    { "en": "Apa Itu Sensor Through-Beam?", "id": "Pemancar Dan Penerima Terpisah Jauh." },
    { "en": "Apa Itu Sensor Retro-Reflective?", "id": "Pemancar Penerima Satu Unit Cermin." },
    { "en": "Apa Itu Sensor Diffuse?", "id": "Deteksi Pantulan Langsung Dari Objek." },
    { "en": "Apa Fungsi Background Suppression?", "id": "Abaikan Objek Di Luar Jarak." },
    { "en": "Apa Itu Color Mark Sensor?", "id": "Deteksi Tanda Warna Kemasan Produk." },
    { "en": "Apa Itu Laser Distance Sensor?", "id": "Ukur Jarak Presisi Sinar Laser." },
    { "en": "Apa Itu Vision Sensor?", "id": "Kamera Inspeksi Produk Otomatis Sederhana." },
    { "en": "Apa Itu Barcode Scanner Tetap?", "id": "Pemindai Kode Batang Di Conveyor." },
    { "en": "Apa Itu Sarung Tangan Kelas 0?", "id": "Sarung Tangan Karet Tahan 1000V." },
    { "en": "Apa Itu Sarung Tangan Kelas 2?", "id": "Sarung Tangan Karet Tahan 17000V." },
    { "en": "Apa Fungsi Leather Protector Glove?", "id": "Pelindung Mekanis Sarung Tangan Karet." },
    { "en": "Apa Itu Arc Flash Suit?", "id": "Baju Tahan Panas Ledakan Listrik." },
    { "en": "Apa Satuan Energi Arc Flash?", "id": "Kalori Per Sentimeter Persegi." },
    { "en": "Apa Kategori HRC 4?", "id": "Risiko Bahaya Arc Flash Tertinggi." },
    { "en": "Apa Fungsi Lockout Hasp?", "id": "Gembok Banyak Lubang Satu Titik." },
    { "en": "Apa Itu Circuit Breaker Lockout?", "id": "Pengunci Tuas MCB Saat Perbaikan." },
    { "en": "Apa Itu Plug Lockout?", "id": "Pengunci Steker Kabel Agar Aman." },
    { "en": "Apa Fungsi Tagout Label?", "id": "Tanda Peringatan Jangan Dioperasikan." },
    { "en": "Apa Fungsi Bleeder Resistor?", "id": "Buang Muatan Kapasitor Demi Keselamatan." },
    { "en": "Apa Fungsi Pull-Up Resistor?", "id": "Jaga Logika High Saat Idle." },
    { "en": "Apa Fungsi Pull-Down Resistor?", "id": "Jaga Logika Low Saat Idle." },
    { "en": "Apa Fungsi Zero Ohm Resistor?", "id": "Jumper Atau Sekering Pada PCB." },
    { "en": "Apa Itu Potensiometer Logaritmik?", "id": "Perubahan Hambatan Melengkung (Audio)." },
    { "en": "Apa Itu Potensiometer Linear?", "id": "Perubahan Hambatan Lurus Proporsional." },
    { "en": "Apa Fungsi Trimmer Capacitor?", "id": "Kapasitor Variabel Untuk Kalibrasi RF." },
    { "en": "Apa Itu Supercapacitor?", "id": "Kapasitor Kapasitas Sangat Besar (Farad)." },
    { "en": "Apa Itu Varistor Metal Oxide?", "id": "Resistor Berubah Nilai Saat Overvoltage." },
    { "en": "Apa Itu NTC Thermistor?", "id": "Hambatan Turun Saat Suhu Naik." },
    { "en": "Apa Itu Kabel Audio Balanced?", "id": "Kabel Tiga Kawat Tahan Noise." },
    { "en": "Apa Itu Kabel Audio Unbalanced?", "id": "Kabel Dua Kawat Rentan Noise." },
    { "en": "Apa Konektor XLR Male?", "id": "Konektor Audio Jarum 3 Pin." },
    { "en": "Apa Konektor TRS 6.5mm?", "id": "Jack Stereo Besar Tiga Bagian." },
    { "en": "Apa Konektor TS 6.5mm?", "id": "Jack Mono Besar Dua Bagian." },
    { "en": "Apa Fungsi DI Box?", "id": "Ubah Sinyal Unbalanced Jadi Balanced." },
    { "en": "Apa Itu Impedansi Speaker?", "id": "Hambatan AC Koil Speaker (Ohm)." },
    { "en": "Apa Itu Line Level Signal?", "id": "Level Tegangan Standar Audio Profesional." },
    { "en": "Apa Itu Mic Level Signal?", "id": "Level Tegangan Kecil Dari Mikrofon." },
    { "en": "Apa Fungsi Mata Solder Pisau?", "id": "Solder Kaki IC Banyak Sekaligus." },
    { "en": "Apa Fungsi Flux Pasta?", "id": "Bersihkan Oksidasi Dan Ratakan Timah." },
    { "en": "Apa Fungsi Solder Sucker?", "id": "Sedot Timah Cair Saat Bongkar." },
    { "en": "Apa Fungsi Solder Wick?", "id": "Pita Tembaga Hisap Sisa Timah." },
    { "en": "Apa Fungsi Helping Hand?", "id": "Penjepit PCB Saat Proses Solder." },
    { "en": "Apa Fungsi Smoke Absorber?", "id": "Hisap Asap Beracun Saat Menyolder." },
    { "en": "Apa Fungsi PCB Vise?", "id": "Ragum Penjepit Papan Sirkuit Kuat." },
    { "en": "Apa Fungsi Lampu Magnifier?", "id": "Lampu Kaca Pembesar Cek Jalur." },
    { "en": "Apa Fungsi ESD Mat?", "id": "Alas Meja Konduktif Anti Statis." },
    { "en": "Apa Fungsi Wrist Strap?", "id": "Gelang Grounding Buang Listrik Tubuh." },
    { "en": "Apa Itu Kabel NYA 1.5mm?", "id": "Kabel Tunggal 1.5mm Instalasi Lampu." },
    { "en": "Apa Itu Kabel NYA 2.5mm?", "id": "Kabel Tunggal 2.5mm Instalasi Stopkontak." },
    { "en": "Apa Itu Kabel NYM 3x2.5mm?", "id": "Kabel Putih Isi 3 Kawat." },
    { "en": "Apa Itu Kabel NYY 4x10mm?", "id": "Kabel Tanah Hitam 4 Kawat." },
    { "en": "Apa Itu Kabel Twist 2x10mm?", "id": "Kabel Udara Aluminium Dua Pilin." },
    { "en": "Apa Itu Konektor Tap Konektor?", "id": "Sambung Kabel Twist Tanpa Kupas." },
    { "en": "Apa Itu Dead End Clamp?", "id": "Klem Tarik Kabel Twist Tiang." },
    { "en": "Apa Itu Suspension Clamp?", "id": "Klem Gantung Kabel Twist Lurus." },
    { "en": "Apa Itu Stainless Strap Tiang?", "id": "Pita Besi Pengikat Klem Tiang." },
    { "en": "Apa Itu Stopping Buckle?", "id": "Kunci Pengikat Pita Stainless Steel." },
    { "en": "Apa Itu Wafer Silicon?", "id": "Piringan Bahan Dasar Pembuatan Chip." },
    { "en": "Apa Itu Cleanroom ISO Class 1?", "id": "Ruang Produksi Chip Paling Bersih." },
    { "en": "Apa Itu Photolithography Mask?", "id": "Cetakan Pola Sirkuit Cahaya UV." },
    { "en": "Apa Itu Etching Process?", "id": "Pengikisan Lapisan Material Tidak Perlu." },
    { "en": "Apa Itu Doping Ion Implantation?", "id": "Penembakan Ion Ubah Sifat Listrik." },
    { "en": "Apa Itu Chemical Mechanical Polishing?", "id": "Poles Permukaan Wafer Hingga Rata." },
    { "en": "Apa Itu Wire Bonding Gold?", "id": "Kawat Emas Penghubung Chip Kaki." },
    { "en": "Apa Itu Die Attach Epoxy?", "id": "Lem Perekat Chip Ke Kemasan." },
    { "en": "Apa Itu Encapsulation Molding?", "id": "Bungkus Chip Dengan Plastik Hitam." },
    { "en": "Apa Itu Yield Rate Wafer?", "id": "Persentase Chip Bagus Per Wafer." },
    { "en": "Apa Itu Moore's Law Limit?", "id": "Batas Fisik Pengecilan Transistor Silikon." },
    { "en": "Apa Itu FinFET 3D Transistor?", "id": "Transistor Sirip Efisiensi Tinggi." },
    { "en": "Apa Itu Gate Oxide Thickness?", "id": "Tebal Isolator Gerbang Sangat Tipis." },
    { "en": "Apa Itu Leakage Current IC?", "id": "Arus Bocor Saat Chip Diam." },
    { "en": "Apa Itu Thermal Design Power?", "id": "Panas Maksimal Yang Dihasilkan Chip." },
    { "en": "Apa Itu SoC (System On Chip)?", "id": "Komputer Lengkap Dalam Satu Chip." },
    { "en": "Apa Itu FPGA Logic Cells?", "id": "Unit Dasar Pemrograman Logika FPGA." },
    { "en": "Apa Itu ASIC Design?", "id": "Desain Chip Khusus Satu Fungsi." },
    { "en": "Apa Itu MEMS Sensor?", "id": "Sensor Mekanik Ukuran Mikroskopis." },
    { "en": "Apa Itu Sensor Strain Gauge?", "id": "Ukur Regangan Benda Akibat Gaya." },
    { "en": "Apa Itu Wheatstone Bridge Circuit?", "id": "Rangkaian Ukur Resistansi Perubahan Kecil." },
    { "en": "Apa Itu Load Cell S-Type?", "id": "Sensor Berat Gantung Bentuk S." },
    { "en": "Apa Itu Load Cell Shear Beam?", "id": "Sensor Berat Lantai Batang Balok." },
    { "en": "Apa Itu LVDT Position Sensor?", "id": "Ukur Posisi Linear Sangat Presisi." },
    { "en": "Apa Itu Rotary Encoder Optical?", "id": "Sensor Putar Piringan Cahaya." },
    { "en": "Apa Itu Hall Effect Switch?", "id": "Saklar Aktif Oleh Medan Magnet." },
    { "en": "Apa Itu Proximity Inductive Shielded?", "id": "Sensor Logam Bisa Tanam Rata." },
    { "en": "Apa Itu Proximity Inductive Unshielded?", "id": "Sensor Logam Jarak Deteksi Jauh." },
    { "en": "Apa Itu Thermocouple Type K?", "id": "Sensor Suhu Umum Rentang Luas." },
    { "en": "Apa Itu RTD PT100 Class A?", "id": "Sensor Suhu Akurasi Sangat Tinggi." },
    { "en": "Apa Itu Thermowell Flange?", "id": "Pelindung Sensor Suhu Koneksi Baut." },
    { "en": "Apa Itu Pressure Transmitter 4-20mA?", "id": "Kirim Data Tekanan Lewat Arus." },
    { "en": "Apa Itu Differential Pressure Transmitter?", "id": "Ukur Beda Tekanan Dua Titik." },
    { "en": "Apa Itu Flowmeter Magnetic?", "id": "Ukur Aliran Cairan Konduktif." },
    { "en": "Apa Itu Flowmeter Coriolis?", "id": "Ukur Aliran Massa Secara Langsung." },
    { "en": "Apa Itu Level Radar Non-Contact?", "id": "Ukur Tinggi Cairan Tanpa Sentuh." },
    { "en": "Apa Itu Level Switch Float?", "id": "Saklar Pelampung Mekanis Sederhana." },
    { "en": "Apa Itu pH Sensor Probe?", "id": "Ukur Keasaman Cairan Elektroda Kaca." },
    { "en": "Apa Itu Conductivity Sensor?", "id": "Ukur Daya Hantar Listrik Air." },
    { "en": "Apa Itu Dissolved Oxygen Sensor?", "id": "Ukur Oksigen Terlarut Dalam Air." },
    { "en": "Apa Itu Turbidity Sensor?", "id": "Ukur Kekeruhan Air Lewat Cahaya." },
    { "en": "Apa Itu Gas Detector LEL?", "id": "Deteksi Ambang Ledak Gas." },
    { "en": "Apa Itu Gas Detector CO?", "id": "Deteksi Gas Karbon Monoksida Beracun." },
    { "en": "Apa Itu Flame Detector UV?", "id": "Deteksi Api Dari Sinar Ultraviolet." },
    { "en": "Apa Itu Vibration Sensor Piezo?", "id": "Deteksi Getaran Mesin Kristal Piezo." },
    { "en": "Apa Itu Accelerometer MEMS?", "id": "Sensor Percepatan Chip Digital." },
    { "en": "Apa Itu Gyroscope MEMS?", "id": "Sensor Rotasi Sudut Chip Digital." },
    { "en": "Apa Itu Magnetometer Compass?", "id": "Sensor Arah Mata Angin Digital." },
    { "en": "Apa Itu Barometric Pressure Sensor?", "id": "Ukur Tekanan Udara Dan Ketinggian." },
    { "en": "Apa Itu Kabel NYA 1.5mm?", "id": "Kabel Tunggal Kaku Instalasi Lampu." },
    { "en": "Apa Itu Kabel NYM 3x2.5mm?", "id": "Kabel Putih Isi Tiga Kawat." },
    { "en": "Apa Itu Kabel NYY 4x10mm?", "id": "Kabel Tanah Hitam Empat Kawat." },
    { "en": "Apa Itu Kabel NYFGBY?", "id": "Kabel Tanah Perisai Plat Baja." },
    { "en": "Apa Itu Kabel N2XSY?", "id": "Kabel Tegangan Menengah Isolasi XLPE." },
    { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Data Pilin Cegah Noise." },
    { "en": "Apa Itu Kabel Coaxial RG6?", "id": "Kabel Antena TV Impedansi 75 Ohm." },
    { "en": "Apa Itu Kabel Coaxial RG58?", "id": "Kabel Radio Komunikasi 50 Ohm." },
    { "en": "Apa Itu Kabel Fiber Single Mode?", "id": "Kabel Optik Inti Kecil Jauh." },
    { "en": "Apa Itu Kabel Fiber Multi Mode?", "id": "Kabel Optik Inti Besar Pendek." },
    { "en": "Apa Itu Kabel Thermocouple Extension?", "id": "Kabel Khusus Sambung Sensor Suhu." },
    { "en": "Apa Itu Kabel Audio Balanced?", "id": "Kabel Mic Tiga Kawat Low Noise." },
    { "en": "Apa Itu Kabel Speaker AWG 16?", "id": "Kabel Serabut Tebal Audio." },
    { "en": "Apa Itu Kabel Ribbon Pelangi?", "id": "Kabel Pita Koneksi Jalur Banyak." },
    { "en": "Apa Itu Kabel HDMI 2.1?", "id": "Kabel Video Digital Support 8K." },
    { "en": "Apa Itu Kabel USB Type-C?", "id": "Kabel Data Daya Bolak Balik." },
    { "en": "Apa Itu Kabel SATA Data?", "id": "Kabel Harddisk Komputer Pipih Merah." },
    { "en": "Apa Itu Busbar Tembaga?", "id": "Batang Penghantar Arus Panel Listrik." },
    { "en": "Apa Itu Heat Shrink Busbar?", "id": "Isolasi Warna Warni Rel Tembaga." },
    { "en": "Apa Itu Isolator Tumpu Busbar?", "id": "Dudukan Penyangga Rel Tembaga." },
    { "en": "Apa Itu Skun Kabel Ring?", "id": "Terminal Kabel Bulat Baut." },
    { "en": "Apa Itu Skun Kabel Garpu?", "id": "Terminal Kabel Bentuk Huruf Y." },
    { "en": "Apa Itu Skun Kabel Pin?", "id": "Terminal Kabel Masuk Lubang MCB." },
    { "en": "Apa Itu Ferrule End Sleeve?", "id": "Selongsong Ujung Kabel Serabut." },
    { "en": "Apa Itu Cable Gland PG?", "id": "Pengunci Kabel Masuk Box Panel." },
    { "en": "Apa Itu Cable Tray Ladder?", "id": "Rak Kabel Bentuk Tangga." },
    { "en": "Apa Itu Cable Tray Perforated?", "id": "Rak Kabel Plat Berlubang." },
    { "en": "Apa Itu Wiring Duct PVC?", "id": "Jalur Kabel Panel Berlubang Samping." },
    { "en": "Apa Itu Label Kabel Marker?", "id": "Penanda Nomor Identitas Kabel." },
    { "en": "Apa Itu Grid Tie Inverter?", "id": "Inverter Sinkron Jaringan PLN." },
    { "en": "Apa Itu Off Grid Inverter?", "id": "Inverter Mandiri Pakai Baterai." },
    { "en": "Apa Itu Hybrid Inverter Solar?", "id": "Inverter Gabungan Grid Dan Baterai." },
    { "en": "Apa Itu Solar Charge Controller?", "id": "Pengatur Pengisian Aki Dari Panel." },
    { "en": "Apa Itu MPPT Controller?", "id": "Charger Efisiensi Tinggi Lacak Daya." },
    { "en": "Apa Itu PWM Controller?", "id": "Charger Murah Pemotongan Pulsa." },
    { "en": "Apa Itu Deep Cycle Battery?", "id": "Aki Tahan Kuras Hingga Kosong." },
    { "en": "Apa Itu VRLA Gel Battery?", "id": "Aki Kering Elektrolit Gel." },
    { "en": "Apa Itu LiFePO4 Battery?", "id": "Baterai Lithium Besi Fosfat Awet." },
    { "en": "Apa Itu Active Balancer?", "id": "Penyeimbang Tegangan Sel Baterai Efisien." },
    { "en": "Apa Itu Inverter Pure Sine Wave?", "id": "Gelombang Output Halus Seperti PLN." },
    { "en": "Apa Itu Inverter Modified Sine Wave?", "id": "Gelombang Output Kotak Kasar." },
    { "en": "Apa Itu MC4 Connector?", "id": "Konektor Standar Kabel Panel Surya." },
    { "en": "Apa Itu Combiner Box PV?", "id": "Kotak Pertemuan Kabel Panel Surya." },
    { "en": "Apa Itu DC Fuse Solar?", "id": "Sekering Khusus Arus Searah." },
    { "en": "Apa Itu DC SPD Surge?", "id": "Anti Petir Khusus Jalur DC." },
    { "en": "Apa Itu FPGA?", "id": "Field Programmable Gate Array." },
    { "en": "Apa Itu ASIC?", "id": "Application Specific Integrated Circuit." },
    { "en": "Apa Fungsi Solder Mask?", "id": "Lindungi Tembaga Dari Oksidasi." },
    { "en": "Apa Itu Via PCB?", "id": "Lubang Penghubung Antar Layer." },
    { "en": "Apa Itu Reflow Oven?", "id": "Pemanas Pasta Solder SMD." },
    { "en": "Apa Itu Wave Soldering?", "id": "Solder Celup Komponen THT." },
    { "en": "Apa Itu Pick And Place?", "id": "Robot Pasang Komponen SMD." },
    { "en": "Apa Itu Gerber File?", "id": "Data Desain Produksi PCB." },
    { "en": "Apa Itu BOM (Bill Of Materials)?", "id": "Daftar Material Komponen Lengkap." },
    { "en": "Apa Itu Silkscreen PCB?", "id": "Tinta Teks Label Komponen." },
    { "en": "Apa Itu Impedance Matching?", "id": "Samakan Impedansi Sumber Beban." },
    { "en": "Apa Itu Crosstalk Sinyal?", "id": "Gangguan Sinyal Kabel Sebelah." },
    { "en": "Apa Itu EMI?", "id": "Electromagnetic Interference." },
    { "en": "Apa Itu EMC?", "id": "Electromagnetic Compatibility." },
    { "en": "Apa Itu Decoupling Capacitor?", "id": "Filter Noise Suplai Daya." },
    { "en": "Apa Fungsi Pull-up Resistor?", "id": "Jaga Logika High Input." },
    { "en": "Apa Fungsi Pull-down Resistor?", "id": "Jaga Logika Low Input." },
    { "en": "Apa Itu Open Collector?", "id": "Output Perlu Pull-up Eksternal." },
    { "en": "Apa Itu Tri-state Buffer?", "id": "Output High Low Hi-Z." },
    { "en": "Apa Itu Schmitt Trigger?", "id": "Input Histeresis Tahan Noise." },
    { "en": "Apa Itu ADC Resolution?", "id": "Jumlah Bit Konversi Analog." },
    { "en": "Apa Itu Sampling Rate?", "id": "Frekuensi Ambil Sampel Sinyal." },
    { "en": "Apa Itu Nyquist Theorem?", "id": "Sampling Minimal Dua Kali." },
    { "en": "Apa Itu Aliasing Signal?", "id": "Sinyal Palsu Sampling Rendah." },
    { "en": "Apa Itu PWM Duty Cycle?", "id": "Persen Waktu Sinyal On." },
    { "en": "Apa Itu PWM Frequency?", "id": "Jumlah Siklus Per Detik." },
    { "en": "Apa Itu H-Bridge Driver?", "id": "Pengendali Arah Motor DC." },
    { "en": "Apa Itu Stepper Motor?", "id": "Motor Gerak Langkah Presisi." },
    { "en": "Apa Itu Servo Motor?", "id": "Motor Gerak Sudut Terkendali." },
    { "en": "Apa Itu Brushless Motor?", "id": "Motor Tanpa Sikat Awet." },
    { "en": "Apa Fungsi Encoder Motor?", "id": "Sensor Posisi Putaran Poros." },
    { "en": "Apa Itu PID Control?", "id": "Proporsional Integral Derivatif." },
    { "en": "Apa Itu Setpoint Kontrol?", "id": "Nilai Target Yang Diinginkan." },
    { "en": "Apa Itu Process Variable?", "id": "Nilai Aktual Dari Sensor." },
    { "en": "Apa Itu Error Signal?", "id": "Selisih Setpoint Dan PV." },
    { "en": "Apa Itu SCADA?", "id": "Supervisory Control Data Acquisition." },
    { "en": "Apa Itu HMI?", "id": "Human Machine Interface." },
    { "en": "Apa Itu PLC?", "id": "Programmable Logic Controller." },
    { "en": "Apa Itu DCS?", "id": "Distributed Control System." },
    { "en": "Apa Itu RTU?", "id": "Remote Terminal Unit." },
    { "en": "Apa Itu Modbus Protocol?", "id": "Protokol Komunikasi Serial Industri." },
    { "en": "Apa Itu Profibus?", "id": "Process Field Bus." },
    { "en": "Apa Itu Ethernet/IP?", "id": "Ethernet Industrial Protocol." },
    { "en": "Apa Itu HART Protocol?", "id": "Highway Addressable Remote Transducer." },
    { "en": "Apa Itu 4-20mA Loop?", "id": "Standar Sinyal Arus Industri." },
    { "en": "Apa Itu Thermocouple?", "id": "Sensor Suhu Dua Logam." },
    { "en": "Apa Itu RTD PT100?", "id": "Sensor Suhu Platinum 100-Ohm." },
    { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat." },
    { "en": "Apa Itu Strain Gauge?", "id": "Sensor Ukur Regangan Benda." },
    { "en": "Apa Itu LVDT Sensor?", "id": "Linear Variable Differential Transformer." },
    { "en": "Apa Itu Proximity Sensor?", "id": "Deteksi Objek Tanpa Sentuh." },
    { "en": "Apa Itu Photoelectric Sensor?", "id": "Sensor Deteksi Pakai Cahaya." },
    { "en": "Apa Itu Ultrasonic Sensor?", "id": "Ukur Jarak Gelombang Suara." },
    { "en": "Apa Itu Li-Ion Battery?", "id": "Baterai Lithium Ion Cair." },
    { "en": "Apa Itu Li-Po Battery?", "id": "Baterai Lithium Polymer Gel." },
    { "en": "Apa Itu LiFePO4 Battery?", "id": "Baterai Lithium Besi Fosfat." },
    { "en": "Apa Itu BMS?", "id": "Battery Management System." },
    { "en": "Apa Itu C-Rate Baterai?", "id": "Laju Arus Charge Discharge." },
    { "en": "Apa Itu DoD Baterai?", "id": "Depth Of Discharge." },
    { "en": "Apa Itu SoC Baterai?", "id": "State Of Charge." },
    { "en": "Apa Itu SoH Baterai?", "id": "State Of Health." },
    { "en": "Apa Itu Inverter Pure Sine?", "id": "Gelombang Output Sinus Murni." },
    { "en": "Apa Itu Inverter Modified Sine?", "id": "Gelombang Output Kotak Modifikasi." },
    { "en": "Apa Itu MPPT Solar?", "id": "Maximum Power Point Tracking." },
    { "en": "Apa Itu PWM Solar Charger?", "id": "Pulse Width Modulation Charger." },
    { "en": "Apa Itu Grid-Tie Inverter?", "id": "Inverter Sinkron Jaringan PLN." },
    { "en": "Apa Itu Off-Grid Inverter?", "id": "Inverter Mandiri Pakai Baterai." },
    { "en": "Apa Itu Hybrid Inverter?", "id": "Gabungan Grid Dan Baterai." },
    { "en": "Apa Itu Solar Panel Monocrystalline?", "id": "Sel Surya Hitam Efisien." },
    { "en": "Apa Itu Solar Panel Polycrystalline?", "id": "Sel Surya Biru Ekonomis." },
    { "en": "Apa Itu Circuit Breaker?", "id": "Pemutus Arus Beban Lebih." },
    { "en": "Apa Kepanjangan MCB?", "id": "Miniature Circuit Breaker." },
    { "en": "Apa Kepanjangan MCCB?", "id": "Molded Case Circuit Breaker." },
    { "en": "Apa Kepanjangan ACB?", "id": "Air Circuit Breaker." },
    { "en": "Apa Kepanjangan VCB?", "id": "Vacuum Circuit Breaker." },
    { "en": "Apa Itu SF6 Breaker?", "id": "Breaker Gas Sulfur Hexafluoride." },
    { "en": "Apa Kepanjangan ELCB?", "id": "Earth Leakage Circuit Breaker." },
    { "en": "Apa Itu RCBO?", "id": "Gabungan MCB Dan ELCB." },
    { "en": "Apa Fungsi Surge Arrester?", "id": "Pelindung Lonjakan Tegangan Petir." },
    { "en": "Apa Fungsi Lightning Rod?", "id": "Batang Penangkal Petir Atas." },
    { "en": "Apa Fungsi Grounding Rod?", "id": "Batang Tembaga Tanam Tanah." },
    { "en": "Apa Fungsi Busbar?", "id": "Batang Tembaga Distribusi Arus." },
    { "en": "Apa Kepanjangan CT?", "id": "Current Transformer." },
    { "en": "Apa Kepanjangan PT?", "id": "Potential Transformer." },
    { "en": "Apa Itu Relay Proteksi?", "id": "Alat Deteksi Gangguan Listrik." },
    { "en": "Apa Itu Buchholz Relay?", "id": "Deteksi Gas Trafo Minyak." },
    { "en": "Apa Itu Thermal Overload?", "id": "Proteksi Panas Motor Listrik." },
    { "en": "Apa Itu Contact Resistance?", "id": "Hambatan Sambungan Kontak Listrik." },
    { "en": "Apa Itu Insulation Resistance?", "id": "Hambatan Isolasi Kabel Trafo." },
    { "en": "Apa Itu Earth Resistance?", "id": "Hambatan Pembumian Ke Tanah." },
    { "en": "Apa Itu Power Factor?", "id": "Rasio Daya Nyata Semu." },
    { "en": "Apa Itu Harmonics?", "id": "Distorsi Gelombang Frekuensi Kelipatan." },
    { "en": "Apa Itu THD Listrik?", "id": "Total Harmonic Distortion." },
    { "en": "Apa Itu Voltage Sag?", "id": "Tegangan Turun Sesaat." },
    { "en": "Apa Itu Voltage Swell?", "id": "Tegangan Naik Sesaat." },
    { "en": "Apa Itu Transient Voltage?", "id": "Lonjakan Tegangan Sangat Singkat." },
    { "en": "Apa Itu Voltage Flicker?", "id": "Kedipan Cahaya Akibat Tegangan." },
    { "en": "Apa Kepanjangan UPS?", "id": "Uninterruptible Power Supply." },
    { "en": "Apa Kepanjangan Genset?", "id": "Generator Set Diesel Bensin." },
    { "en": "Apa Kepanjangan ATS?", "id": "Automatic Transfer Switch." },
    { "en": "Apa Itu Protokol DNP3?", "id": "Standar Telemetri Gardu Induk." },
    { "en": "Apa Itu Protokol IEC 61850?", "id": "Komunikasi Digital Substation Automation." },
    { "en": "Apa Itu GOOSE Message?", "id": "Pesan Cepat Antar IED." },
    { "en": "Apa Itu IED Protection?", "id": "Intelligent Electronic Device Proteksi." },
    { "en": "Apa Itu Merging Unit?", "id": "Antarmuka CT PT Digital." },
    { "en": "Apa Itu Time Synchronization PTP?", "id": "Sinkronisasi Waktu Presisi Ethernet." },
    { "en": "Apa Itu NTP Server?", "id": "Network Time Protocol Server." },
    { "en": "Apa Itu GPS Clock Reference?", "id": "Sumber Waktu Satelit Akurat." },
    { "en": "Apa Itu IRIG-B Signal?", "id": "Format Kode Waktu Standar." },
    { "en": "Apa Itu RTU Panel?", "id": "Panel Remote Terminal Unit." },
    { "en": "Apa Itu Marshalling Kiosk?", "id": "Lemari Terminal Kabel Kontrol." },
    { "en": "Apa Itu Junction Box JB?", "id": "Kotak Sambung Kabel Lapangan." },
    { "en": "Apa Itu Cable Trench?", "id": "Parit Kabel Beton Bawah." },
    { "en": "Apa Itu Cable Gland Ex?", "id": "Pengunci Kabel Area Ledakan." },
    { "en": "Apa Itu Fire Stop Mortar?", "id": "Semen Penahan Api Lubang." },
    { "en": "Apa Itu Fire Stop Pillow?", "id": "Bantal Penahan Api Lubang." },
    { "en": "Apa Itu Busduct Alumunium?", "id": "Rel Daya Batang Alumunium." },
    { "en": "Apa Itu Busduct Tembaga?", "id": "Rel Daya Batang Tembaga." },
    { "en": "Apa Itu IP Busduct?", "id": "Ingress Protection Pelindung Busduct." },
    { "en": "Apa Itu GIS Switchgear?", "id": "Gas Insulated Switchgear SF6." },
    { "en": "Apa Itu AIS Switchgear?", "id": "Air Insulated Switchgear Terbuka." },
    { "en": "Apa Itu RMU (Ring Main Unit)?", "id": "Panel Distribusi Tegangan Menengah." },
    { "en": "Apa Itu LBS (Load Break Switch)?", "id": "Saklar Pemutus Beban TM." },
    { "en": "Apa Itu FCO (Fuse Cut Out)?", "id": "Sekering Tegangan Menengah Tiang." },
    { "en": "Apa Itu Recloser Otomatis?", "id": "Pemutus Balik Otomatis Jaringan." },
    { "en": "Apa Itu Sectionalizer?", "id": "Pemisah Seksi Jaringan Gangguan." },
    { "en": "Apa Itu SSO (Saklar Seksi Otomatis)?", "id": "Nama Lain Dari Sectionalizer." },
    { "en": "Apa Itu AVR Generator?", "id": "Automatic Voltage Regulator Genset." },
    { "en": "Apa Itu Governor Genset?", "id": "Pengatur Putaran Mesin Diesel." },
    { "en": "Apa Itu Exciter Generator?", "id": "Penguat Medan Magnet Rotor." },
    { "en": "Apa Itu PMG Exciter?", "id": "Permanent Magnet Generator Exciter." },
    { "en": "Apa Itu Droop Kit?", "id": "Modul Berbagi Beban Paralel." },
    { "en": "Apa Itu Load Sharing Module?", "id": "Pembagi Beban Otomatis Genset." },
    { "en": "Apa Itu Sync Check Relay?", "id": "Cek Kesamaan Fasa Tegangan." },
    { "en": "Apa Itu Reverse Power Relay?", "id": "Cegah Arus Balik Generator." },
    { "en": "Apa Itu Earth Fault Relay?", "id": "Proteksi Gangguan Ke Tanah." },
    { "en": "Apa Itu Restricted Earth Fault?", "id": "Proteksi Tanah Zona Terbatas." },
    { "en": "Apa Itu Differential Relay Trafo?", "id": "Bandingkan Arus Masuk Keluar." },
    { "en": "Apa Itu Buchholz Relay Trafo?", "id": "Deteksi Gas Internal Trafo." },
    { "en": "Apa Itu Jansen Relay?", "id": "Proteksi Tap Changer Trafo." },
    { "en": "Apa Itu Sudden Pressure Relay?", "id": "Deteksi Lonjakan Tekanan Tangki." },
    { "en": "Apa Itu Oil Level Indicator?", "id": "Penunjuk Tinggi Minyak Trafo." },
    { "en": "Apa Itu Winding Temp Indicator?", "id": "Penunjuk Suhu Belitan Trafo." },
    { "en": "Apa Itu Oil Temp Indicator?", "id": "Penunjuk Suhu Minyak Trafo." },
    { "en": "Apa Itu Breather Silica Gel?", "id": "Pernapasan Trafo Penyerap Lembap." },
    { "en": "Apa Itu Radiator Trafo?", "id": "Sirip Pendingin Minyak Trafo." },
    { "en": "Apa Itu On Load Tap Changer?", "id": "Pengubah Tap Saat Berbeban." },
    { "en": "Apa Itu Off Load Tap Changer?", "id": "Pengubah Tap Saat Mati." },
    { "en": "Apa Itu Vector Group Dyn5?", "id": "Delta Star Geser 150." },
    { "en": "Apa Itu Vector Group Yyn0?", "id": "Star Star Fasa Sama." },
    { "en": "Apa Itu Impedansi Trafo Uk?", "id": "Persen Tegangan Hubung Singkat." },
    { "en": "Apa Itu Inrush Current Trafo?", "id": "Arus Kejut Saat Energize." },
    { "en": "Apa Itu Excitation Current?", "id": "Arus Magnetisasi Inti Besi." },
    { "en": "Apa Itu No Load Loss?", "id": "Rugi Daya Inti Besi." },
    { "en": "Apa Itu Load Loss Trafo?", "id": "Rugi Tembaga Saat Berbeban." },
    { "en": "Apa Itu Efficiency Trafo?", "id": "Rasio Daya Output Input." },
    { "en": "Apa Itu Regulation Voltage?", "id": "Jatuh Tegangan Saat Beban." },
    { "en": "Apa Itu K-Factor Trafo?", "id": "Ketahanan Terhadap Arus Harmonisa." },
    { "en": "Apa Itu Dry Type Trafo?", "id": "Trafo Isolasi Resin Kering." },
    { "en": "Apa Itu Cast Resin Trafo?", "id": "Kumparan Dicor Resin Epoksi." },
    { "en": "Apa Itu Oil Immersed Trafo?", "id": "Kumparan Rendam Minyak Isolasi." },
    { "en": "Apa Itu Hermetically Sealed Trafo?", "id": "Trafo Tertutup Tanpa Konservator." },
    { "en": "Apa Itu Conservator Tank?", "id": "Tangki Pemuaian Minyak Trafo." },
    { "en": "Apa Itu Nitrogen Cushion?", "id": "Bantal Gas Nitrogen Pelindung." },
    { "en": "Apa Itu Bushing HV?", "id": "Isolator Terminal Tegangan Tinggi." },
    { "en": "Apa Itu Bushing LV?", "id": "Isolator Terminal Tegangan Rendah." },
    { "en": "Apa Itu Arcing Horn Bushing?", "id": "Tanduk Api Proteksi Petir." },
    { "en": "Apa Itu Spark Gap?", "id": "Celah Udara Buang Surja." },
    { "en": "Apa Itu Lightning Arrester ZnO?", "id": "Metal Oxide Varistor Surge." },
    { "en": "Apa Itu Discharge Counter?", "id": "Penghitung Sambaran Petir Arrester." },
    { "en": "Apa Itu Grounding Mesh?", "id": "Jaring Kawat Tanah Gardu." },
    { "en": "Apa Itu Step Voltage Danger?", "id": "Bahaya Tegangan Langkah Kaki." },
    { "en": "Apa Itu Touch Voltage Danger?", "id": "Bahaya Tegangan Sentuh Tangan." },
    { "en": "Apa Itu GPR (Ground Potential Rise)?", "id": "Kenaikan Tegangan Tanah Gangguan." },
    { "en": "Apa Itu Soil Resistivity Test?", "id": "Uji Tahanan Jenis Tanah." },
    { "en": "Apa Itu Wenner 4 Point Method?", "id": "Metode Ukur Tahanan Tanah." },
    { "en": "Apa Itu Earth Tester Digital?", "id": "Alat Ukur Grounding Otomatis." },
    { "en": "Apa Itu Clamp On Earth Tester?", "id": "Ukur Grounding Tanpa Pasak." },
    { "en": "Apa Itu Insulation Resistance Test?", "id": "Uji Tahanan Isolasi Megger." },
    { "en": "Apa Itu Polarization Index Test?", "id": "Rasio Megger 10 Menit." },
    { "en": "Apa Itu Dielectric Absorption Ratio?", "id": "Rasio Megger 60 Detik." },
    { "en": "Apa Itu Tan Delta Test?", "id": "Uji Faktor Daya Isolasi." },
    { "en": "Apa Itu Breakdown Voltage Oil?", "id": "Tegangan Tembus Minyak Trafo." },
    { "en": "Apa Itu DGA Oil Test?", "id": "Analisis Gas Terlarut Minyak." },
    { "en": "Apa Itu Furan Analysis?", "id": "Cek Kondisi Kertas Isolasi." },
    { "en": "Apa Itu Corrosive Sulfur Test?", "id": "Cek Sulfur Korosif Minyak." },
    { "en": "Apa Itu SFRA Test?", "id": "Analisis Respon Frekuensi Sapuan." },
    { "en": "Apa Itu TTR Test?", "id": "Uji Rasio Belitan Trafo." },
    { "en": "Apa Itu Winding Resistance Test?", "id": "Uji Tahanan DC Belitan." },
    { "en": "Apa Itu Contact Resistance Test?", "id": "Uji Tahanan Kontak Breaker." },
    { "en": "Apa Itu Circuit Breaker Analyzer?", "id": "Uji Waktu Buka Tutup." },
    { "en": "Apa Itu Relay Test Set?", "id": "Alat Uji Relay Proteksi." },
    { "en": "Apa Itu Primary Injection Test?", "id": "Uji Arus Besar Langsung." },
    { "en": "Apa Itu Secondary Injection Test?", "id": "Uji Sinyal Ke Relay." },
    { "en": "Apa Itu Partial Discharge Test?", "id": "Deteksi Peluahan Listrik Kecil." },
    { "en": "Apa Itu Thermography Inspection?", "id": "Inspeksi Panas Kamera Inframerah." },
    { "en": "Apa Itu Ultrasound Inspection?", "id": "Deteksi Suara Bocor Arus." },
    { "en": "Apa Itu Differential Relay Stability?", "id": "Tahan Trip Saat Gangguan Luar." },
    { "en": "Apa Itu Through Fault Current?", "id": "Arus Gangguan Lewat Zona Proteksi." },
    { "en": "Apa Itu CT Saturation Voltage?", "id": "Tegangan Saat Inti CT Jenuh." },
    { "en": "Apa Itu Knee Point Voltage?", "id": "Batas Linearitas Kurva Eksitasi CT." },
    { "en": "Apa Itu Remanence Flux CT?", "id": "Magnet Sisa Inti Trafo Arus." },
    { "en": "Apa Itu Transient Response CT?", "id": "Respon CT Terhadap Komponen DC." },
    { "en": "Apa Itu Burden Resistor CT?", "id": "Beban Resistif Terminal Sekunder CT." },
    { "en": "Apa Itu Interposing CT?", "id": "CT Penyesuai Rasio Atau Fasa." },
    { "en": "Apa Itu Summation CT?", "id": "Jumlahkan Arus Beberapa Feeder." },
    { "en": "Apa Itu Zero Sequence CT?", "id": "Sensor Arus Bocor Tanah (ZCT)." },
    { "en": "Apa Itu Neutral Grounding Resistor?", "id": "Batasi Arus Gangguan Tanah Trafo." },
    { "en": "Apa Itu Liquid Resistor NGR?", "id": "Resistor Cair Elektrolit Untuk NGR." },
    { "en": "Apa Itu Zigzag Transformer Grounding?", "id": "Buat Titik Netral Sistem Delta." },
    { "en": "Apa Itu Earthing Transformer?", "id": "Trafo Khusus Pembumian Titik Netral." },
    { "en": "Apa Itu Grounding Grid Mesh?", "id": "Jaring Kawat Tembaga Bawah Tanah." },
    { "en": "Apa Itu Step Voltage Danger?", "id": "Tegangan Langkah Kaki Saat Fault." },
    { "en": "Apa Itu Touch Voltage Danger?", "id": "Tegangan Sentuh Tangan Saat Fault." },
    { "en": "Apa Itu Wenner 4-Point Method?", "id": "Metode Ukur Resistivitas Tanah Standar." },
    { "en": "Apa Itu Grounding Improvement Material?", "id": "Semen Konduktif Penurun Tahanan Tanah." },
    { "en": "Apa Itu Partial Discharge Activity?", "id": "Aktivitas Peluahan Listrik Dalam Isolasi." },
    { "en": "Apa Itu Corona Camera (UV)?", "id": "Kamera Deteksi Sinar Ultraviolet Korona." },
    { "en": "Apa Itu Ultrasonic Leak Detector?", "id": "Deteksi Suara Bocor Arus/Gas." },
    { "en": "Apa Itu SF6 Gas Analyzer?", "id": "Analisis Kualitas Gas Isolasi SF6." },
    { "en": "Apa Itu Dew Point SF6?", "id": "Titik Embun Kelembapan Gas SF6." },
    { "en": "Apa Itu Circuit Breaker Timing Test?", "id": "Uji Kecepatan Buka Tutup Kontak." },
    { "en": "Apa Itu Contact Resistance Measurement?", "id": "Ukur Hambatan Kontak Breaker (Micro-ohm)." },
    { "en": "Apa Itu Vacuum Bottle Tester?", "id": "Uji Kevakuman Tabung VCB." },
    { "en": "Apa Itu GIS Partial Discharge?", "id": "PD Dalam Gas Insulated Switchgear." },
    { "en": "Apa Itu UHF PD Sensor?", "id": "Sensor PD Frekuensi Ultra Tinggi." },
    { "en": "Apa Itu TEV Sensor PD?", "id": "Transient Earth Voltage Partial Discharge." },
    { "en": "Apa Itu HFCT Sensor PD?", "id": "High Frequency Current Transformer PD." },
    { "en": "Apa Itu Acoustic PD Sensor?", "id": "Sensor Suara Peluahan Listrik." },
    { "en": "Apa Itu Dissolved Gas Analysis?", "id": "Analisis Gas Terlarut Minyak Trafo." },
    { "en": "Apa Itu Duval Triangle DGA?", "id": "Metode Diagnosa Jenis Fault Trafo." },
    { "en": "Apa Itu Rogers Ratio DGA?", "id": "Metode Rasio Gas Diagnosa Trafo." },
    { "en": "Apa Itu Key Gas Method?", "id": "Diagnosa Berdasarkan Gas Dominan." },
    { "en": "Apa Itu Furan Analysis Trafo?", "id": "Estimasi Sisa Umur Kertas Isolasi." },
    { "en": "Apa Itu Degree Of Polymerization?", "id": "Ukuran Kekuatan Mekanis Kertas Trafo." },
    { "en": "Apa Itu Moisture In Oil PPM?", "id": "Kadar Air Dalam Minyak (Part-per-million)." },
    { "en": "Apa Itu Breakdown Voltage Oil?", "id": "Tegangan Tembus Minyak Isolasi Trafo." },
    { "en": "Apa Itu Interfacial Tension Oil?", "id": "Tegangan Permukaan Minyak Dan Air." },
    { "en": "Apa Itu Acid Number Oil?", "id": "Tingkat Keasaman Minyak Trafo." },
    { "en": "Apa Itu Corrosive Sulfur Oil?", "id": "Sulfur Reaktif Perusak Tembaga Trafo." },
    { "en": "Apa Itu Passivator Minyak Trafo?", "id": "Zat Pencegah Korosi Tembaga." },
    { "en": "Apa Itu Oil Regeneration Process?", "id": "Daur Ulang Minyak Trafo Bekas." },
    { "en": "Apa Itu Vacuum Drying Trafo?", "id": "Pengeringan Isolasi Trafo Ruang Hampa." },
    { "en": "Apa Itu Hot Oil Circulation?", "id": "Sirkulasi Minyak Panas Bersihkan Trafo." },
    { "en": "Apa Itu Sweep Frequency Response?", "id": "Analisis Pergeseran Mekanis Belitan Trafo." },
    { "en": "Apa Itu Dielectric Frequency Response?", "id": "Analisis Kadar Air Isolasi Kertas." },
    { "en": "Apa Itu Tan Delta Capacitance?", "id": "Uji Faktor Daya Isolasi Trafo." },
    { "en": "Apa Itu Excitation Current Test?", "id": "Uji Kondisi Inti Besi Trafo." },
    { "en": "Apa Itu Leakage Reactance Test?", "id": "Uji Reaktansi Bocor Trafo Daya." },
    { "en": "Apa Itu Induced Voltage Test?", "id": "Uji Ketahanan Tegangan Antar Lilitan." },
    { "en": "Apa Itu Applied Voltage Test?", "id": "Uji Ketahanan Tegangan Ke Tanah." },
    { "en": "Apa Itu Impulse Voltage Test?", "id": "Uji Ketahanan Terhadap Sambaran Petir." },
    { "en": "Apa Itu Temperature Rise Test?", "id": "Uji Kenaikan Suhu Beban Penuh." },
    { "en": "Apa Itu Short Circuit Withstand?", "id": "Uji Ketahanan Arus Hubung Singkat." },
    { "en": "Apa Itu Sound Level Test?", "id": "Uji Kebisingan Suara Trafo." },
    { "en": "Apa Itu No-Load Loss Test?", "id": "Uji Rugi Besi Inti Trafo." },
    { "en": "Apa Itu Load Loss Test?", "id": "Uji Rugi Tembaga Belitan Trafo." },
    { "en": "Apa Itu Zero Sequence Impedance?", "id": "Impedansi Trafo Urutan Nol." },
    { "en": "Apa Itu Vector Group Verification?", "id": "Verifikasi Hubungan Fasa Belitan Trafo." },
    { "en": "Apa Itu Ratio Test TTR?", "id": "Uji Perbandingan Lilitan Trafo." },
    { "en": "Apa Itu Winding Resistance Test?", "id": "Uji Tahanan DC Belitan Trafo." },
    { "en": "Apa Itu Tap Changer Test?", "id": "Uji Fungsi Pengubah Sadapan Tegangan." },
    { "en": "Apa Itu Dynamic Resistance DRM?", "id": "Rekam Arus Saat Pindah Tap." },
    { "en": "Apa Itu Motor Vibration Analysis?", "id": "Analisis Spektrum Getaran Motor." },
    { "en": "Apa Itu Motor Current Signature?", "id": "Analisis Spektrum Arus Motor (MCSA)." },
    { "en": "Apa Itu Rotor Bar Fault?", "id": "Kerusakan Batang Rotor Sangkar Tupai." },
    { "en": "Apa Itu Stator Winding Fault?", "id": "Hubung Singkat Antar Lilitan Stator." },
    { "en": "Apa Itu Bearing Fault Frequency?", "id": "Frekuensi Getaran Kerusakan Bantalan." },
    { "en": "Apa Itu Unbalance Magnetic Pull?", "id": "Tarikan Magnet Tidak Rata Rotor." },
    { "en": "Apa Itu Soft Foot Motor?", "id": "Kaki Motor Tidak Duduk Rata." },
    { "en": "Apa Itu Shaft Misalignment?", "id": "Poros Motor Dan Beban Miring." },
    { "en": "Apa Itu Laser Alignment Tool?", "id": "Alat Luruskan Poros Presisi Tinggi." },
    { "en": "Apa Itu Belt Tension Meter?", "id": "Alat Ukur Kekencangan Sabuk Motor." },
    { "en": "Apa Itu Thermography Motor Inspection?", "id": "Cek Panas Berlebih Body Motor." },
    { "en": "Apa Itu Motor Surge Test?", "id": "Uji Isolasi Antar Lilitan Motor." },
    { "en": "Apa Itu Motor Hi-Pot Test?", "id": "Uji Isolasi Ke Ground Tegangan Tinggi." },
    { "en": "Apa Itu Motor Megger Test?", "id": "Uji Tahanan Isolasi Standar." },
    { "en": "Apa Itu Polarization Index Motor?", "id": "Rasio Megger 10 Menit Semenit." },
    { "en": "Apa Itu Step Voltage Test?", "id": "Uji Tegangan Bertahap Isolasi Motor." },
    { "en": "Apa Itu Ramp Voltage Test?", "id": "Uji Tegangan Naik Linear Isolasi." },
    { "en": "Apa Itu Partial Discharge Motor?", "id": "Deteksi Peluahan Listrik Isolasi HV." },
    { "en": "Apa Itu VLF Cable Test?", "id": "Very Low Frequency Hipot Kabel." },
    { "en": "Apa Itu Damped AC Test?", "id": "Uji Kabel Tegangan Osilasi Teredam." },
    { "en": "Apa Itu Cable Sheath Test?", "id": "Uji Kebocoran Kulit Luar Kabel." },
    { "en": "Apa Itu Cable Fault Location?", "id": "Titik Lokasi Gangguan Kabel Tanah." },
    { "en": "Apa Itu TDR Cable Fault?", "id": "Time Domain Reflectometry Radar Kabel." },
    { "en": "Apa Itu Thumper Cable Fault?", "id": "Generator Impuls Suara Ledakan Kabel." },
    { "en": "Apa Itu Pinpointing Cable Fault?", "id": "Menentukan Titik Tepat Galian Kabel." },
    { "en": "Apa Itu Cable Identifier Tool?", "id": "Alat Identifikasi Kabel Dalam Bundel." },
    { "en": "Apa Itu Phase Identification Tool?", "id": "Alat Identifikasi Fasa Kabel TM." },
    { "en": "Apa Itu Live Line Indicator?", "id": "Indikator Tegangan Kabel Beroperasi." },
    { "en": "Apa Itu Voltage Detector Stick?", "id": "Tongkat Deteksi Tegangan Tinggi." },
    { "en": "Apa Itu Phasing Stick HV?", "id": "Tongkat Cek Kesamaan Fasa HV." },
    { "en": "Apa Itu Grounding Cluster Set?", "id": "Kabel Pembumian Sementara Saat Kerja." },
    { "en": "Apa Itu Discharge Stick HV?", "id": "Tongkat Pembuang Muatan Sisa Kapasitif." },
    { "en": "Apa Itu Insulating Mat HV?", "id": "Karpet Isolasi Lantai Panel TM." },
    { "en": "Apa Itu HVDC Monopolar?", "id": "Transmisi DC Satu Kawat Return Tanah." },
    { "en": "Apa Itu HVDC Bipolar?", "id": "Transmisi DC Dua Kawat Positif Negatif." },
    { "en": "Apa Itu HVDC Back-to-Back?", "id": "Hubungkan Dua Grid Beda Frekuensi." },
    { "en": "Apa Itu Thyristor Valve HVDC?", "id": "Saklar Daya Utama Konverter LCC." },
    { "en": "Apa Itu Smoothing Reactor?", "id": "Haluskan Riak Arus DC Link." },
    { "en": "Apa Itu Commutation Failure?", "id": "Gagal Padamkan Thyristor Pada LCC." },
    { "en": "Apa Itu FACTS (Flexible AC Transmission)?", "id": "Alat Elektronik Pengendali Parameter Transmisi." },
    { "en": "Apa Itu SVC (Static Var Compensator)?", "id": "Kompensator Reaktif Berbasis Thyristor." },
    { "en": "Apa Itu TCR (Thyristor Controlled Reactor)?", "id": "Induktor Variabel Kendali Sudut Fasa." },
    { "en": "Apa Itu TSC (Thyristor Switched Capacitor)?", "id": "Kapasitor Bank Saklar Thyristor Cepat." },
    { "en": "Apa Itu STATCOM?", "id": "Kompensator Reaktif Berbasis VSC Inverter." },
    { "en": "Apa Itu UPFC (Unified Power Flow Controller)?", "id": "Pengendali Aliran Daya Paling Lengkap." },
    { "en": "Apa Itu Series Compensation?", "id": "Kapasitor Seri Kurangi Impedansi Saluran." },
    { "en": "Apa Itu Shunt Compensation?", "id": "Kapasitor Atau Reaktor Paralel Jaringan." },
    { "en": "Apa Itu Subsynchronous Resonance?", "id": "Interaksi Bahaya Turbin Dan Kapasitor Seri." },
    { "en": "Apa Itu Black Start Capability?", "id": "Pembangkit Start Tanpa Listrik Grid." },
    { "en": "Apa Itu Load Frequency Control?", "id": "Jaga Frekuensi Dengan Atur Governor." },
    { "en": "Apa Itu AVR (Automatic Voltage Regulator)?", "id": "Jaga Tegangan Generator Tetap Stabil." },
    { "en": "Apa Itu PSS (Power System Stabilizer)?", "id": "Redam Osilasi Daya Pada Generator." },
    { "en": "Apa Itu Infinite Bus?", "id": "Sistem Tegangan Frekuensi Tetap Konstan." },
    { "en": "Apa Itu Swing Curve?", "id": "Grafik Sudut Rotor Terhadap Waktu." },
    { "en": "Apa Itu Critical Clearing Time?", "id": "Waktu Maksimum Hapus Fault Stabil." },
    { "en": "Apa Itu Millman's Theorem?", "id": "Tegangan Ujung Generator Paralel." },
    { "en": "Apa Itu Tellegen's Theorem?", "id": "Jumlah Daya Dalam Rangkaian Nol." },
    { "en": "Apa Itu Reciprocity Theorem?", "id": "Tukar Posisi Sumber Dan Amperemeter." },
    { "en": "Apa Itu Superposition Theorem?", "id": "Analisis Rangkaian Banyak Sumber." },
    { "en": "Apa Itu Norton's Equivalent?", "id": "Sumber Arus Paralel Dengan Resistor." },
    { "en": "Apa Itu Thevenin's Equivalent?", "id": "Sumber Tegangan Seri Dengan Resistor." },
    { "en": "Apa Itu Maximum Power Transfer?", "id": "Impedansi Beban Sama Dengan Sumber." },
    { "en": "Apa Itu Q Factor (Quality Factor)?", "id": "Perbandingan Energi Simpan Dan Buang." },
    { "en": "Apa Itu Bandwidth Rangkaian Resonansi?", "id": "Frekuensi Resonansi Dibagi Q Factor." },
    { "en": "Apa Itu Selectivity Rangkaian?", "id": "Kemampuan Memilih Frekuensi Tertentu." },
    { "en": "Apa Itu Damping Ratio?", "id": "Ukuran Redaman Osilasi Sistem." },
    { "en": "Apa Itu Overdamped System?", "id": "Tidak Ada Osilasi Respon Lambat." },
    { "en": "Apa Itu Underdamped System?", "id": "Ada Osilasi Sebelum Stabil." },
    { "en": "Apa Itu Critically Damped?", "id": "Respon Tercepat Tanpa Osilasi." },
    { "en": "Apa Itu Hazardous Gas Group I?", "id": "Gas Metana Pertambangan Bawah Tanah." },
    { "en": "Apa Itu Hazardous Gas Group IIA?", "id": "Gas Propana (Risiko Rendah)." },
    { "en": "Apa Itu Hazardous Gas Group IIB?", "id": "Gas Etilena (Risiko Sedang)." },
    { "en": "Apa Itu Hazardous Gas Group IIC?", "id": "Gas Hidrogen Asetilen (Risiko Tinggi)." },
    { "en": "Apa Itu Temperature Class T1?", "id": "Suhu Permukaan Maksimal 450 Celcius." },
    { "en": "Apa Itu Temperature Class T6?", "id": "Suhu Permukaan Maksimal 85 Celcius." },
    { "en": "Apa Itu Dust Group IIIA?", "id": "Serat Terbang Yang Mudah Terbakar." },
    { "en": "Apa Itu Dust Group IIIB?", "id": "Debu Non-Konduktif." },
    { "en": "Apa Itu Dust Group IIIC?", "id": "Debu Konduktif (Paling Berbahaya)." },
    { "en": "Apa Itu Equipment Protection Level (EPL)?", "id": "Tingkat Keamanan Alat (Ga, Gb, Gc)." },
    { "en": "Apa Itu Flameproof Ex d?", "id": "Box Tahan Ledakan Internal." },
    { "en": "Apa Itu Increased Safety Ex e?", "id": "Cegah Percikan Pada Terminal." },
    { "en": "Apa Itu Intrinsic Safety Ex ia?", "id": "Aman Meski Dua Kesalahan Terjadi." },
    { "en": "Apa Itu Intrinsic Safety Ex ib?", "id": "Aman Meski Satu Kesalahan Terjadi." },
    { "en": "Apa Itu Purged Pressurized Ex p?", "id": "Gas Inert Tekanan Positif." },
    { "en": "Apa Itu Encapsulation Ex m?", "id": "Komponen Dicor Resin Penuh." },
    { "en": "Apa Itu Oil Immersion Ex o?", "id": "Komponen Direndam Minyak." },
    { "en": "Apa Itu Non-Sparking Ex n?", "id": "Peralatan Normal Tidak Memercik." },
    { "en": "Apa Itu Cable Gland Ex d?", "id": "Gland Tahan Ledakan Dan Api." },
    { "en": "Apa Itu Barrier Gland?", "id": "Gland Diisi Compound Resin." },
    { "en": "Apa Itu HTLS Conductor?", "id": "High Temperature Low Sag." },
    { "en": "Apa Itu ACSS Conductor?", "id": "Aluminium Conductor Steel Supported." },
    { "en": "Apa Itu Gap Type Conductor?", "id": "Celah Antara Inti Baja Aluminium." },
    { "en": "Apa Itu Invar Core Conductor?", "id": "Inti Paduan Besi Nikel Stabil." },
    { "en": "Apa Itu Carbon Composite Core?", "id": "Inti Serat Karbon Ringan Kuat." },
    { "en": "Apa Itu Superconducting Cable?", "id": "Kabel Hambatan Nol Suhu Dingin." },
    { "en": "Apa Itu HTS Cable (High Temp)?", "id": "Superkonduktor Nitrogen Cair." },
    { "en": "Apa Itu GIL (Gas Insulated Line)?", "id": "Pipa Konduktor Isolasi Gas SF6." },
    { "en": "Apa Itu XLPE Insulation Treeing?", "id": "Kerusakan Isolasi Bentuk Pohon." },
    { "en": "Apa Itu Water Treeing Kabel?", "id": "Treeing Akibat Kelembapan." },
    { "en": "Apa Itu Electrical Treeing Kabel?", "id": "Treeing Akibat Peluahan Listrik." },
    { "en": "Apa Itu Semiconductive Screen?", "id": "Ratakan Medan Listrik Isolasi Kabel." },
    { "en": "Apa Itu Metallic Screen Kabel?", "id": "Pita Tembaga Return Path Ground." },
    { "en": "Apa Itu Cross Bonding Kabel?", "id": "Silangkan Shield Kurangi Arus Sirkulasi." },
    { "en": "Apa Itu Link Box Kabel?", "id": "Kotak Sambung Shielding Kabel Tanah." },
    { "en": "Apa Itu SVL (Sheath Voltage Limiter)?", "id": "Arrester Pelindung Jaket Kabel." },
    { "en": "Apa Itu Distributed Temperature Sensing?", "id": "Ukur Suhu Sepanjang Kabel Fiber." },
    { "en": "Apa Itu Power Line Communication?", "id": "Kirim Data Lewat Kabel Listrik." },
    { "en": "Apa Itu Ripple Control System?", "id": "Sinyal Frekuensi Audio Kontrol Beban." },
    { "en": "Apa Itu Interharmonic?", "id": "Frekuensi Bukan Kelipatan Bulat Dasar." },
    { "en": "Apa Itu Subharmonic?", "id": "Frekuensi Di Bawah Frekuensi Dasar." },
    { "en": "Apa Itu DC Offset AC?", "id": "Komponen Arus Searah Di AC." },
    { "en": "Apa Itu Voltage Notch?", "id": "Cacat Tukik Akibat Komutasi SCR." },
    { "en": "Apa Itu Voltage Imbalance NEMA?", "id": "Deviasi Maksimum Dari Rata-rata." },
    { "en": "Apa Itu Voltage Imbalance IEC?", "id": "Komponen Urutan Negatif Bagi Positif." },
    { "en": "Apa Itu Short Circuit Ratio?", "id": "Kekuatan Grid Di Titik Sambung." },
    { "en": "Apa Itu Weak Grid?", "id": "Grid Impedansi Tinggi Mudah Goyang." },
    { "en": "Apa Itu Stiff Grid?", "id": "Grid Kuat Impedansi Rendah Stabil." },
    { "en": "Apa Itu Fault Ride Through?", "id": "Pembangkit Bertahan Saat Gangguan." },
    { "en": "Apa Itu Grid Code?", "id": "Aturan Teknis Koneksi Ke Jaringan." },
    { "en": "Apa Itu Ancillary Services?", "id": "Layanan Pendukung Stabilitas Grid." },
    { "en": "Apa Itu Frequency Containment Reserve?", "id": "Cadangan Daya Respon Cepat." },
    { "en": "Apa Itu Frequency Restoration Reserve?", "id": "Cadangan Kembalikan Frekuensi Normal." },
    { "en": "Apa Itu Replacement Reserve?", "id": "Cadangan Gantikan Pembangkit Trip." },
    { "en": "Apa Itu Automatic Generation Control?", "id": "Sistem Pusat Atur Output Pembangkit." },
    { "en": "Apa Itu Economic Dispatch?", "id": "Bagi Beban Pembangkit Termurah." },
    { "en": "Apa Itu Unit Commitment?", "id": "Jadwal Nyala Mati Pembangkit." },
    { "en": "Apa Itu State Estimation?", "id": "Perkiraan Kondisi Sistem Dari Data." },
    { "en": "Apa Itu Contingency Analysis?", "id": "Simulasi Gangguan N-1." },
    { "en": "Apa Itu N-1 Criterion?", "id": "Sistem Aman Jika Satu Alat Lepas." },
    { "en": "Apa Itu SCADA Front End Processor?", "id": "Komputer Komunikasi Ke RTU." },
    { "en": "Apa Itu Inter-Control Center Protocol?", "id": "Komunikasi Antar Pusat Kontrol (ICCP)." },
    { "en": "Apa Itu Proses Czochralski?", "id": "Metode Penumbuhan Kristal Silikon Tunggal." },
    { "en": "Apa Itu Silicon Ingot?", "id": "Batang Silikon Murni Hasil Kristalisasi." },
    { "en": "Apa Itu Wafer Slicing?", "id": "Pemotongan Batang Silikon Menjadi Kepingan." },
    { "en": "Apa Itu Epitaxial Layer?", "id": "Lapisan Kristal Murni Diatas Wafer." },
    { "en": "Apa Itu Doping Ion Implantation?", "id": "Tembak Ion Ubah Sifat Listrik." },
    { "en": "Apa Itu Cleanroom ISO Class 1?", "id": "Ruang Paling Bersih Industri Chip." },
    { "en": "Apa Fungsi HEPA Filter?", "id": "Saring 99.97 Persen Partikel Debu." },
    { "en": "Apa Fungsi ULPA Filter?", "id": "Saring 99.999 Persen Partikel Debu." },
    { "en": "Apa Itu Laminar Airflow Cabinet?", "id": "Meja Kerja Steril Aliran Udara." },
    { "en": "Apa Itu Harmonic Drive Gear?", "id": "Gearbox Robot Presisi Tanpa Celah." },
    { "en": "Apa Itu Cycloidal Gearbox?", "id": "Gearbox Robot Kuat Tahan Guncangan." },
    { "en": "Apa Itu Backlash Pada Gear?", "id": "Jarak Longgar Antara Gigi Roda." },
    { "en": "Apa Itu Zero Backlash?", "id": "Tidak Ada Jarak Longgar Gigi." },
    { "en": "Apa Itu Robot End Effector?", "id": "Alat Kerja Di Ujung Robot." },
    { "en": "Apa Itu Vacuum Gripper?", "id": "Pencapit Menggunakan Hisapan Udara." },
    { "en": "Apa Itu Soft Robotics?", "id": "Robot Dari Bahan Fleksibel Kenyal." },
    { "en": "Apa Itu Inverse Kinematics?", "id": "Hitung Sudut Sendi Dari Posisi." },
    { "en": "Apa Itu Forward Kinematics?", "id": "Hitung Posisi Dari Sudut Sendi." },
    { "en": "Apa Itu Singularity Point?", "id": "Posisi Robot Kehilangan Kebebasan Gerak." },
    { "en": "Apa Itu Standar Definisi Volt?", "id": "Berdasarkan Efek Josephson Junction." },
    { "en": "Apa Itu Standar Definisi Ohm?", "id": "Berdasarkan Efek Hall Kuantum." },
    { "en": "Apa Itu Standar Definisi Ampere?", "id": "Berdasarkan Aliran Muatan Elementer." },
    { "en": "Apa Itu Jam Atom Cesium?", "id": "Standar Waktu Detik Paling Akurat." },
    { "en": "Apa Itu NIST Traceability?", "id": "Standar Kalibrasi Mengacu Ke Amerika." },
    { "en": "Apa Itu Calibration Uncertainty?", "id": "Rentang Keraguan Hasil Pengukuran." },
    { "en": "Apa Itu Test Uncertainty Ratio?", "id": "Rasio Akurasi Alat Dan Standar." },
    { "en": "Apa Itu Enkripsi Simetris?", "id": "Kunci Enkripsi Dan Dekripsi Sama." },
    { "en": "Apa Itu Enkripsi Asimetris?", "id": "Kunci Publik Dan Privat Berbeda." },
    { "en": "Apa Itu Algoritma AES-256?", "id": "Standar Enkripsi Simetris Sangat Kuat." },
    { "en": "Apa Itu Algoritma RSA?", "id": "Standar Enkripsi Asimetris Kunci Publik." },
    { "en": "Apa Itu Elliptic Curve Cryptography?", "id": "Enkripsi Kuat Dengan Kunci Pendek." },
    { "en": "Apa Itu Hashing SHA-256?", "id": "Fungsi Hash Satu Arah Unik." },
    { "en": "Apa Itu Digital Signature?", "id": "Tanda Tangan Elektronik Validasi Data." },
    { "en": "Apa Itu Blockchain Node?", "id": "Komputer Penyimpan Salinan Buku Besar." },
    { "en": "Apa Itu Proof Of Work?", "id": "Validasi Transaksi Dengan Komputasi Berat." },
    { "en": "Apa Itu Proof Of Stake?", "id": "Validasi Transaksi Dengan Kepemilikan Koin." },
    { "en": "Apa Itu Smart Contract?", "id": "Program Otomatis Di Atas Blockchain." },
    { "en": "Apa Itu Quantum Key Distribution?", "id": "Distribusi Kunci Sandi Fisika Kuantum." },
    { "en": "Apa Itu Post-Quantum Cryptography?", "id": "Algoritma Tahan Serangan Komputer Kuantum." },
    { "en": "Apa Itu Homomorphic Encryption?", "id": "Olah Data Dalam Keadaan Terenkripsi." },
    { "en": "Apa Itu HVDC Valve Hall?", "id": "Ruang Isolasi Saklar Thyristor Besar." },
    { "en": "Apa Itu Wall Bushing HVDC?", "id": "Isolator Tembus Dinding Gedung Konverter." },
    { "en": "Apa Itu Smoothing Reactor HVDC?", "id": "Induktor Perata Arus DC Besar." },
    { "en": "Apa Itu Earth Return Path?", "id": "Arus Balik Melalui Tanah Laut." },
    { "en": "Apa Itu Electrode Station?", "id": "Stasiun Grounding Arus Balik HVDC." },
    { "en": "Apa Itu Metallic Return?", "id": "Arus Balik Lewat Kabel Khusus." },
    { "en": "Apa Itu Commutation Failure?", "id": "Kegagalan Padam Thyristor Inverter." },
    { "en": "Apa Itu Voltage Source Converter?", "id": "Konverter HVDC Menggunakan IGBT." },
    { "en": "Apa Itu Black Start HVDC?", "id": "Hidupkan Grid Mati Lewat DC." },
    { "en": "Apa Itu Submarine Cable HVDC?", "id": "Kabel Laut Tegangan Tinggi DC." },
    { "en": "Apa Itu Mass Impregnated Cable?", "id": "Kabel Kertas Minyak Kental Laut." },
    { "en": "Apa Itu XLPE DC Cable?", "id": "Kabel Plastik Tahan Tegangan DC." },
    { "en": "Apa Itu IGCT Thyristor?", "id": "Integrated Gate Commutated Thyristor." },
    { "en": "Apa Itu GTO Thyristor?", "id": "Gate Turn Off Thyristor." },
    { "en": "Apa Itu ETO Thyristor?", "id": "Emitter Turn Off Thyristor." },
    { "en": "Apa Itu MCT Thyristor?", "id": "MOS Controlled Thyristor." },
    { "en": "Apa Itu SIDAC?", "id": "Dioda Pemicu Tegangan Tinggi." },
    { "en": "Apa Itu SBS (Silicon Switch)?", "id": "Saklar Pemicu Dua Arah." },
    { "en": "Apa Itu SUS (Silicon Switch)?", "id": "Saklar Pemicu Satu Arah." },
    { "en": "Apa Itu Tunnel Diode Oscillator?", "id": "Pembangkit Sinyal Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Gunn Diode Oscillator?", "id": "Pembangkit Gelombang Mikro Radar." },
    { "en": "Apa Itu IMPATT Diode?", "id": "Dioda Daya Tinggi Gelombang Mikro." },
    { "en": "Apa Itu TRAPATT Diode?", "id": "Dioda Efisiensi Tinggi Gelombang Mikro." },
    { "en": "Apa Itu BARITT Diode?", "id": "Barrier Injection Transit Time Diode." },
    { "en": "Apa Itu Heat Pump EV?", "id": "Pemanas Kabin Efisien Hemat Baterai." },
    { "en": "Apa Itu PTC Heater EV?", "id": "Pemanas Resistif Kabin Mobil Listrik." },
    { "en": "Apa Itu V2L Adapter?", "id": "Alat Ubah Port Cas Stopkontak." },
    { "en": "Apa Itu Slippery Road Alert?", "id": "Komunikasi Bahaya Antar Kendaraan (V2V)." },
    { "en": "Apa Itu Emergency Lane Keep?", "id": "Setir Otomatis Hindari Keluar Jalur." },
    { "en": "Apa Itu Adaptive Cruise Control?", "id": "Jaga Jarak Otomatis Dengan Depan." },
    { "en": "Apa Itu Radar Millimeter Wave?", "id": "Sensor Jarak Mobil Anti Tabrak." },
    { "en": "Apa Itu Ultrasonic Parking Sensor?", "id": "Sensor Parkir Bunyi Beep." },
    { "en": "Apa Itu Surround View Camera?", "id": "Kamera 360 Derajat Lihat Sekeliling." },
    { "en": "Apa Itu Telematics Control Unit?", "id": "Modul Komunikasi Data Mobil Internet." },
    { "en": "Apa Itu eCall System?", "id": "Panggilan Darurat Otomatis Saat Kecelakaan." },
    { "en": "Apa Itu FOTA Update?", "id": "Firmware Over The Air Mobil." },
    { "en": "Apa Itu Battery Pre-conditioning?", "id": "Panaskan Baterai Sebelum Charging Cepat." },
    { "en": "Apa Itu Thermal Runaway Propagation?", "id": "Api Menyebar Antar Sel Baterai." },
    { "en": "Apa Itu Pyroswitch Fuse?", "id": "Sekering Ledak Putus Arus Darurat." },
    { "en": "Apa Itu High Voltage Interlock?", "id": "Loop Deteksi Konektor Lepas." },
    { "en": "Apa Itu Resolver Sensor?", "id": "Sensor Posisi Motor Tahan Banting." },
    { "en": "Apa Itu Flux Weakening?", "id": "Teknik Putar Motor Kecepatan Tinggi." },
    { "en": "Apa Itu Regenerative Efficiency?", "id": "Persen Energi Kembali Saat Rem." },
    { "en": "Apa Itu Rolling Resistance Ban?", "id": "Gaya Hambat Gelinding Ban Mobil." },
    { "en": "Apa Itu Drag Coefficient (Cd)?", "id": "Ukuran Hambatan Angin Bodi Mobil." },
    { "en": "Apa Itu Aerodynamic Wheel?", "id": "Velg Desain Khusus Belah Angin." },
    { "en": "Apa Itu Frunk (Front Trunk)?", "id": "Bagasi Depan Mobil Listrik." },
    { "en": "Apa Itu Skateboard Platform?", "id": "Sasis Baterai Rata Mobil Listrik." },
    { "en": "Apa Itu Gigacasting?", "id": "Cetak Rangka Aluminium Satu Bagian." },
    { "en": "Apa Itu Structural Battery Pack?", "id": "Baterai Jadi Bagian Rangka Mobil." },
    { "en": "Apa Itu Cell-to-Pack (CTP)?", "id": "Sel Langsung Ke Pack Tanpa Modul." },
    { "en": "Apa Itu Cell-to-Chassis (CTC)?", "id": "Sel Baterai Langsung Ke Sasis." },
    { "en": "Apa Itu Wireless BMS?", "id": "Manajemen Baterai Komunikasi Nirkabel." },
    { "en": "Apa Itu Battery Swapping?", "id": "Ganti Baterai Kosong Dengan Penuh." },
    { "en": "Apa Itu Supercharger V3?", "id": "Charger Tesla Kecepatan 250 kW." },
    { "en": "Apa Itu Megawatt Charging System?", "id": "Standar Charger Truk Listrik Besar." },
    { "en": "Apa Itu Pantograph Charging?", "id": "Cas Bus Listrik Lewat Atap." },
    { "en": "Apa Itu Wireless EV Charging?", "id": "Cas Mobil Parkir Diatas Pad." },
    { "en": "Apa Itu Dynamic Wireless Charging?", "id": "Cas Mobil Sambil Jalan (Jalan Tol)." },
    { "en": "Apa Itu Green Hydrogen?", "id": "Hidrogen Dari Energi Terbarukan." },
    { "en": "Apa Itu Blue Hydrogen?", "id": "Hidrogen Gas Alam Dengan Karbon Capture." },
    { "en": "Apa Itu Grey Hydrogen?", "id": "Hidrogen Dari Gas Alam Tanpa Capture." },
    { "en": "Apa Fungsi Electrolyzer?", "id": "Pemisah Air Menjadi Hidrogen Oksigen." },
    { "en": "Apa Itu Fuel Cell PEMFC?", "id": "Proton Exchange Membrane Fuel Cell." },
    { "en": "Apa Itu Fuel Cell SOFC?", "id": "Solid Oxide Fuel Cell Suhu Tinggi." },
    { "en": "Apa Produk Buangan Fuel Cell?", "id": "Hanya Air Murni Dan Panas." },
    { "en": "Apa Itu Perovskite Solar Cell?", "id": "Sel Surya Kristal Efisiensi Tinggi." },
    { "en": "Apa Itu Tandem Solar Cell?", "id": "Tumpukan Dua Jenis Sel Surya." },
    { "en": "Apa Itu Bifacial Gain?", "id": "Energi Tambahan Dari Belakang Panel." },
    { "en": "Apa Itu Superkonduktor Tipe I?", "id": "Menolak Medan Magnet Sepenuhnya." },
    { "en": "Apa Itu Superkonduktor Tipe II?", "id": "Ijinkan Fluks Magnet Tembus Sebagian." },
    { "en": "Apa Itu Efek Josephson?", "id": "Arus Terowongan Tanpa Beda Tegangan." },
    { "en": "Apa Itu Quantum Dot?", "id": "Nanokristal Semikonduktor Pemancar Cahaya." },
    { "en": "Apa Itu Graphene Sheet?", "id": "Lembaran Karbon Setebal Satu Atom." },
    { "en": "Apa Itu Carbon Nanotube?", "id": "Tabung Karbon Kekuatan Sangat Tinggi." },
    { "en": "Apa Itu Aerogel Insulation?", "id": "Isolator Padat Paling Ringan Dunia." },
    { "en": "Apa Itu Shape Memory Alloy?", "id": "Logam Kembali Bentuk Asal Saat Panas." },
    { "en": "Apa Itu Piezoelectric Floor?", "id": "Lantai Penghasil Listrik Dari Langkah." },
    { "en": "Apa Itu Thermoelectric Generator (TEG)?", "id": "Pembangkit Listrik Beda Suhu Pasif." },
    { "en": "Apa Itu Digital Substation?", "id": "Gardu Induk Berbasis Komunikasi Fiber." },
    { "en": "Apa Fungsi Merging Unit?", "id": "Ubah Sinyal CT PT Ke Digital." },
    { "en": "Apa Itu Process Bus IEC 61850?", "id": "Jaringan Data Mentah Arus Tegangan." },
    { "en": "Apa Itu Station Bus IEC 61850?", "id": "Jaringan Data Kontrol Dan Monitoring." },
    { "en": "Apa Itu PTP (Precision Time Protocol)?", "id": "Sinkronisasi Waktu Akurasi Nanodetik." },
    { "en": "Apa Itu IED (Intelligent Electronic Device)?", "id": "Relay Proteksi Berbasis Mikroprosesor." },
    { "en": "Apa Itu GOOSE Communication?", "id": "Pesan Cepat Antar IED Proteksi." },
    { "en": "Apa Itu Sampled Values (SV)?", "id": "Data Gelombang Listrik Digital Real-time." },
    { "en": "Apa Itu CID File IEC 61850?", "id": "File Konfigurasi IED Spesifik." },
    { "en": "Apa Itu SCD File IEC 61850?", "id": "File Konfigurasi Seluruh Gardu Induk." },
    { "en": "Apa Itu Profibus PA Profile?", "id": "Profil Alat Proses Intrinsik Aman." },
    { "en": "Apa Itu Profinet Real-Time (RT)?", "id": "Komunikasi Data Standar Otomasi." },
    { "en": "Apa Itu Profinet Isochronous (IRT)?", "id": "Komunikasi Motion Control Sangat Cepat." },
    { "en": "Apa Itu EtherNet/IP CIP?", "id": "Common Industrial Protocol Over Ethernet." },
    { "en": "Apa Itu Device Level Ring (DLR)?", "id": "Topologi Cincin Ethernet Tahan Putus." },
    { "en": "Apa Itu TSN (Time Sensitive Networking)?", "id": "Standar Ethernet Untuk Waktu Pasti." },
    { "en": "Apa Itu MQTT Topic Wildcard?", "id": "Karakter Pagar Untuk Baca Semua." },
    { "en": "Apa Itu JSON Payload?", "id": "Isi Pesan Format Teks Terstruktur." },
    { "en": "Apa Itu Node-RED?", "id": "Pemrograman Alur Visual Untuk IoT." },
    { "en": "Apa Itu InfluxDB?", "id": "Database Khusus Data Deret Waktu." },
    { "en": "Apa Itu Grafana Dashboard?", "id": "Visualisasi Data Grafik Real-time." },
    { "en": "Apa Itu LoRaWAN Class B Beacon?", "id": "Sinyal Waktu Sinkronisasi Dari Gateway." },
    { "en": "Apa Itu LoRaWAN ADR?", "id": "Pengaturan Kecepatan Data Otomatis." },
    { "en": "Apa Itu Sigfox Ultra Narrowband?", "id": "Modulasi Sinyal Sangat Sempit Hemat." },
    { "en": "Apa Itu NB-IoT Guard Band?", "id": "Spektrum Sela Antar Kanal LTE." },
    { "en": "Apa Itu LTE-M Power Saving?", "id": "Fitur Tidur Panjang Hemat Baterai." },
    { "en": "Apa Itu 5G Massive MTC?", "id": "Koneksi Jutaan Perangkat Per Km2." },
    { "en": "Apa Itu 5G URLLC Latency?", "id": "Latensi Di Bawah 1 Milidetik." },
    { "en": "Apa Itu WiFi 6 OFDMA?", "id": "Bagi Kanal Jadi Sub-carrier Kecil." },
    { "en": "Apa Itu WiFi 6 TWT?", "id": "Target Wake Time Hemat Baterai." },
    { "en": "Apa Itu Bluetooth Mesh?", "id": "Jaringan Lompat Banyak Ke Banyak." },
    { "en": "Apa Itu Zigbee Green Power?", "id": "Protokol Untuk Perangkat Tanpa Baterai." },
    { "en": "Apa Itu Thread Border Router?", "id": "Penghubung Jaringan Thread Ke WiFi." },
    { "en": "Apa Itu Matter Multi-Admin?", "id": "Kontrol Dari Banyak Ekosistem Sekaligus." },
    { "en": "Apa Itu NFC Tag Type 4?", "id": "Tag Memori Besar Keamanan Tinggi." },
    { "en": "Apa Itu RFID Anti-Collision?", "id": "Baca Banyak Tag Bersamaan." },
    { "en": "Apa Itu UWB (Ultra Wide Band)?", "id": "Komunikasi Jarak Dekat Sangat Presisi." },
    { "en": "Apa Fungsi UWB Di HP?", "id": "Kunci Mobil Digital Dan Lokasi." },
    { "en": "Apa Itu LiDAR Solid State?", "id": "LiDAR Tanpa Bagian Bergerak." },
    { "en": "Apa Itu Radar mmWave?", "id": "Radar Gelombang Milimeter Presisi." },
    { "en": "Apa Itu Sensor pH Gel?", "id": "Elektroda Isi Gel Minim Perawatan." },
    { "en": "Apa Itu Sensor DO Optik?", "id": "Ukur Oksigen Pendaran Cahaya." },
    { "en": "Apa Itu Sensor Gas Katalitik?", "id": "Deteksi Gas Mudah Terbakar." },
    { "en": "Apa Itu Sensor Gas Elektrokimia?", "id": "Deteksi Gas Beracun Presisi." },
    { "en": "Apa Itu Sensor Gas NDIR?", "id": "Deteksi CO2 Menggunakan Inframerah." },
    { "en": "Apa Itu Load Cell Creep Error?", "id": "Perubahan Nilai Saat Beban Tetap." },
    { "en": "Apa Itu Load Cell 6 Kabel?", "id": "Kompensasi Tegangan Kabel Panjang." },
    { "en": "Apa Itu Strain Gauge Rosette?", "id": "Tiga Gauge Sudut Berbeda." },
    { "en": "Apa Itu LVDT Stroke Length?", "id": "Jarak Maksimum Pergeseran Inti." },
    { "en": "Apa Itu Rotary Encoder Quadrature?", "id": "Dua Sinyal Beda Fasa 90." },
    { "en": "Apa Itu Absolute Encoder Grey Code?", "id": "Kode Biner Berubah Satu Bit." },
    { "en": "Apa Itu Resolver Excitation?", "id": "Sinyal AC Masuk Ke Rotor." },
    { "en": "Apa Itu Hall Effect Latch?", "id": "Ingat Status Magnet Terakhir." },
    { "en": "Apa Itu Motor BLDC Hub?", "id": "Motor Menyatu Dengan Roda Kendaraan." },
    { "en": "Apa Itu Motor Mid Drive?", "id": "Motor Di Tengah Rangka Sepeda." },
    { "en": "Apa Itu PAS Sensor E-Bike?", "id": "Pedal Assist Sensor Deteksi Kayuhan." },
    { "en": "Apa Itu Throttle Hall Effect?", "id": "Gas Tangan Motor Listrik." },
    { "en": "Apa Itu Controller Sine Wave?", "id": "Driver Motor Suara Hening." },
    { "en": "Apa Itu Controller Square Wave?", "id": "Driver Motor Suara Kasar." },
    { "en": "Apa Itu FOC Controller?", "id": "Field Oriented Control Efisiensi Tinggi." },
    { "en": "Apa Itu Regenerative Braking Level?", "id": "Kekuatan Pengereman Pengisian Baterai." },
    { "en": "Apa Itu E-ABS Brake?", "id": "Anti Lock Brake Elektronik Motor." },
    { "en": "Apa Itu Battery Sag?", "id": "Tegangan Drop Saat Beban Puncak." },
    { "en": "Apa Itu Battery Peukert Exponent?", "id": "Angka Efisiensi Discharge Baterai." },
    { "en": "Apa Itu Internal Resistance Meter?", "id": "Alat Ukur Kesehatan Sel Baterai." },
    { "en": "Apa Itu Nickel Strip Murni?", "id": "Plat Nikel Tahanan Rendah." },
    { "en": "Apa Itu Nickel Plated Steel?", "id": "Besi Lapis Nikel Murah." },
    { "en": "Apa Cara Cek Nickel Murni?", "id": "Gores Dan Rendam Air Garam." },
    { "en": "Apa Itu Spot Welder Pulse?", "id": "Durasi Arus Las Titik." },
    { "en": "Apa Itu Kapton Tape Polyimide?", "id": "Isolasi Kuning Tahan Panas Tinggi." },
    { "en": "Apa Itu Barley Paper Isolator?", "id": "Kertas Pelindung Kutub Positif Baterai." },
    { "en": "Apa Itu Heat Shrink PVC Battery?", "id": "Plastik Pembungkus Pack Baterai." },
    { "en": "Apa Itu XT90 Anti-Spark?", "id": "Konektor Ada Resistor Peredam Percikan." },
    { "en": "Apa Itu Anderson Powerpole?", "id": "Konektor Modular Arus DC Besar." },
    { "en": "Apa Itu Busbar Flexible?", "id": "Tumpukan Plat Tembaga Tipis." },
    { "en": "Apa Itu Terminal Lug Bimetal?", "id": "Konektor Kabel Aluminium Ke Tembaga." },
    { "en": "Apa Itu Grease Contact?", "id": "Pelumas Konduktif Sambungan Listrik." },
    { "en": "Apa Itu Torque Marker Paint?", "id": "Cat Penanda Baut Sudah Kencang." },
    { "en": "Apa Itu Cable Tie Stainless?", "id": "Pengikat Kabel Tahan Cuaca Ekstrem." }



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
