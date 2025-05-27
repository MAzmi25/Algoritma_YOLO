<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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


  { "en": "Apa kepanjangan dari YOLO?", "id": "You Only Look Once." },
  { "en": "Apa tujuan utama algoritma YOLO?", "id": "Deteksi objek secara real-time." },
  { "en": "Bagaimana YOLO memproses gambar?", "id": "Melihat seluruh gambar sekali jalan." },
  { "en": "YOLO termasuk jenis detektor apa?", "id": "Detektor satu tahap (single-stage)." },
  { "en": "Apa yang diprediksi YOLO untuk objek?", "id": "Kotak batas dan kelas objek." },
  { "en": "Siapa pengembang asli YOLO?", "id": "Joseph Redmon dan timnya." },
  { "en": "Kapan YOLO pertama kali diperkenalkan?", "id": "Tahun 2016." },
  { "en": "Apa keunggulan utama dari YOLO?", "id": "Kecepatan deteksi sangat tinggi." },
  { "en": "Bagaimana YOLO membagi gambar input?", "id": "Menjadi beberapa sel grid (grid cells)." },
  { "en": "Apa fungsi setiap sel grid YOLO?", "id": "Mendeteksi objek dalam area sel." },
  { "en": "Apa itu skor kepercayaan (confidence score)?", "id": "Keyakinan ada objek dan akurasi kotak." },
  { "en": "Apakah YOLO menggunakan region proposal network?", "id": "Tidak, YOLO tidak menggunakannya." },
  { "en": "Apa perbedaan fundamental YOLO dan R-CNN?", "id": "YOLO lebih cepat dari R-CNN." },
  { "en": "Dapatkah YOLO mendeteksi berbagai objek sekaligus?", "id": "Ya, mampu deteksi banyak objek." },
  { "en": "Bagaimana YOLO mengatasi deteksi ganda?", "id": "Menggunakan Non-Maximum Suppression (NMS)." },
  { "en": "Apakah YOLO mempelajari fitur objek umum?", "id": "Ya, belajar representasi objek umum." },
  { "en": "Bagaimana kinerja YOLO pada objek kecil?", "id": "Versi awal kurang optimal." },
  { "en": "Apakah kode sumber YOLO tersedia bebas?", "id": "Ya, banyak versi open source." },
  { "en": "Framework apa yang digunakan YOLO awal?", "id": "Darknet." },
  { "en": "Apa itu bounding box dalam YOLO?", "id": "Kotak penanda lokasi objek terdeteksi." },
  { "en": "Parameter bounding box apa saja?", "id": "x, y, lebar, tinggi, kepercayaan." },
  { "en": "Apakah YOLO efektif untuk video?", "id": "Ya, sangat efektif untuk video." },
  { "en": "Mengapa YOLO disebut deteksi terpadu?", "id": "Deteksi sebagai masalah regresi tunggal." },
  { "en": "Apa output dari jaringan YOLO?", "id": "Tensor prediksi kotak dan kelas." },
  { "en": "Apa itu probabilitas kelas bersyarat?", "id": "Probabilitas objek kelas tertentu hadir." },
  { "en": "Versi YOLO mana yang paling pertama?", "id": "YOLO versi 1 (YOLOv1)." },
  { "en": "Peningkatan signifikan YOLOv2 apa?", "id": "Lebih baik, cepat, dan kuat." },
  { "en": "Apa nama populer dari YOLOv2?", "id": "YOLO9000." },
  { "en": "Fitur baru apa di YOLOv2?", "id": "Anchor boxes dan batch normalization." },
  { "en": "Berapa kelas bisa dideteksi YOLO9000?", "id": "Lebih dari 9000 kelas berbeda." },
  { "en": "Fokus utama pengembangan YOLOv3?", "id": "Akurasi lebih baik, deteksi objek kecil." },
  { "en": "Arsitektur backbone YOLOv3 apa namanya?", "id": "Darknet-53." },
  { "en": "Bagaimana YOLOv3 deteksi objek kecil?", "id": "Prediksi pada tiga skala berbeda." },
  { "en": "Siapa yang mengembangkan model YOLOv4?", "id": "Alexey Bochkovskiy dan rekannya." },
  { "en": "Apa itu 'Bag of Freebies' YOLOv4?", "id": "Teknik training tingkatkan akurasi gratis." },
  { "en": "Apa itu 'Bag of Specials' YOLOv4?", "id": "Modul tingkatkan akurasi sedikit biaya." },
  { "en": "Siapa yang merilis seri YOLOv5?", "id": "Perusahaan Ultralytics." },
  { "en": "Framework apa yang digunakan YOLOv5?", "id": "PyTorch." },
  { "en": "Keunggulan utama dari YOLOv5?", "id": "Mudah digunakan dan performa solid." },
  { "en": "Siapa pengembang utama YOLOv6?", "id": "Tim dari Meituan." },
  { "en": "Fokus utama pengembangan YOLOv6?", "id": "Efisiensi untuk aplikasi industri." },
  { "en": "Siapa pengembang utama YOLOv7?", "id": "WongKinYiu dan Alexey Bochkovskiy." },
  { "en": "Inovasi penting apa pada YOLOv7?", "id": "Extended ELAN dan model scaling." },
  { "en": "Siapa yang merilis seri YOLOv8?", "id": "Perusahaan Ultralytics." },
  { "en": "Kemampuan tambahan apa dimiliki YOLOv8?", "id": "Segmentasi, klasifikasi, dan estimasi pose." },
  { "en": "Apakah YOLOv8 masih menggunakan anchor boxes?", "id": "Tidak, YOLOv8 bersifat anchor-free." },
  { "en": "Apa kepanjangan dari YOLO-NAS?", "id": "YOLO Neural Architecture Search." },
  { "en": "Keunggulan utama dari YOLO-NAS?", "id": "Arsitektur optimal hasil pencarian otomatis." },
  { "en": "Apa fitur unik YOLO World?", "id": "Deteksi objek open-vocabulary canggih." },
  { "en": "Bagaimana YOLO World deteksi objek baru?", "id": "Menggunakan input deskripsi teks." },
  { "en": "Sebutkan tiga bagian arsitektur YOLO?", "id": "Backbone, Neck, dan Head." },
  { "en": "Apa fungsi dari Backbone pada YOLO?", "id": "Ekstraksi fitur dari gambar input." },
  { "en": "Contoh arsitektur Backbone YOLO?", "id": "Darknet, ResNet, CSPNet, EfficientNet." },
  { "en": "Apa fungsi dari Neck pada YOLO?", "id": "Menggabungkan fitur dari berbagai level." },
  { "en": "Contoh arsitektur Neck pada YOLO?", "id": "FPN, PANet, BiFPN, ELAN." },
  { "en": "Apa fungsi dari Head pada YOLO?", "id": "Melakukan prediksi akhir (kotak, kelas)." },
  { "en": "Apa itu anchor boxes di YOLO?", "id": "Kotak pra-definisi berbagai rasio aspek." },
  { "en": "Kapan anchor boxes pertama kali ada?", "id": "Pada versi YOLOv2." },
  { "en": "Apa guna utama anchor boxes?", "id": "Membantu prediksi kotak lebih akurat." },
  { "en": "Apakah semua versi YOLO pakai anchor?", "id": "Tidak, beberapa versi sudah anchor-free." },
  { "en": "Apa yang dimaksud grid cell YOLO?", "id": "Unit pembagian area pada gambar." },
  { "en": "Apa itu lapisan konvolusi (convolutional)?", "id": "Lapisan utama untuk ekstraksi fitur." },
  { "en": "Fungsi batch normalization di YOLO?", "id": "Menstabilkan dan mempercepat proses training." },
  { "en": "Contoh fungsi aktivasi di YOLO?", "id": "Leaky ReLU, Mish, SiLU, GeLU." },
  { "en": "Apa itu Darknet dalam konteks YOLO?", "id": "Framework dan arsitektur backbone awal." },
  { "en": "Apa itu CSPNet singkatan dari?", "id": "Cross Stage Partial Network." },
  { "en": "Fungsi FPN (Feature Pyramid Network)?", "id": "Menggabungkan fitur multi-skala efektif." },
  { "en": "Fungsi PANet (Path Aggregation Network)?", "id": "Meningkatkan alur informasi antar fitur." },
  { "en": "Apa itu decoupled head di YOLO?", "id": "Memisahkan prediksi kotak dan kelas." },
  { "en": "Fungsi Spatial Pyramid Pooling (SPP)?", "id": "Menangani berbagai ukuran input gambar." },
  { "en": "Apa syarat utama melatih YOLO?", "id": "Dataset gambar dengan anotasi objek." },
  { "en": "Apa itu anotasi objek dataset?", "id": "Label kelas dan koordinat kotak." },
  { "en": "Format anotasi umum YOLO apa?", "id": "File teks (kelas x y w h)." },
  { "en": "Apa itu loss function training?", "id": "Mengukur seberapa error prediksi model." },
  { "en": "Komponen loss function YOLO umumnya?", "id": "Lokalisasi, kepercayaan, dan klasifikasi." },
  { "en": "Apa itu localization loss (box loss)?", "id": "Error prediksi lokasi dan ukuran." },
  { "en": "Apa itu confidence loss (objectness loss)?", "id": "Error prediksi keberadaan suatu objek." },
  { "en": "Apa itu classification loss (class loss)?", "id": "Error prediksi label kelas objek." },
  { "en": "Optimizer apa sering dipakai YOLO?", "id": "Adam atau SGD." },
  { "en": "Apa itu learning rate training?", "id": "Ukuran langkah pembaruan bobot model." },
  { "en": "Apa yang dimaksud dengan epoch?", "id": "Satu siklus penuh training dataset." },
  { "en": "Apa itu batch size training?", "id": "Jumlah sampel per iterasi training." },
  { "en": "Apa itu data augmentation AI?", "id": "Memperbanyak data training secara artifisial." },
  { "en": "Contoh teknik data augmentation populer?", "id": "Rotasi, flip, scaling, mosaic, mixup." },
  { "en": "Apa itu mosaic data augmentation?", "id": "Menggabungkan empat gambar jadi satu." },
  { "en": "Manfaat utama mosaic data augmentation?", "id": "Meningkatkan deteksi objek ukuran kecil." },
  { "en": "Apa itu transfer learning AI?", "id": "Menggunakan bobot model pra-terlatih." },
  { "en": "Mengapa transfer learning itu penting?", "id": "Hemat waktu, butuh data sedikit." },
  { "en": "Dataset apa sering dipakai pra-latih?", "id": "COCO, ImageNet, Pascal VOC." },
  { "en": "Apa itu fine-tuning model AI?", "id": "Melatih ulang model pra-latih sedikit." },
  { "en": "Kapan fine-tuning model itu diperlukan?", "id": "Saat dataset target sangat berbeda." },
  { "en": "Perangkat keras ideal untuk training?", "id": "GPU dengan memori yang besar." },
  { "en": "Apa itu overfitting dalam training?", "id": "Model bagus di train, buruk di tes." },
  { "en": "Bagaimana cara mencegah overfitting model?", "id": "Augmentasi, regularisasi, dropout, early stopping." },
  { "en": "Apa itu teknik early stopping?", "id": "Hentikan training jika validasi memburuk." },
  { "en": "Apa itu multi-scale image training?", "id": "Training dengan berbagai resolusi gambar." },
  { "en": "Metrik evaluasi utama deteksi objek?", "id": "mAP (mean Average Precision)." },
  { "en": "Apa itu IoU singkatan dari?", "id": "Intersection over Union." },
  { "en": "Bagaimana IoU dihitung secara mudah?", "id": "Area irisan dibagi area gabungan." },
  { "en": "Fungsi IoU threshold dalam evaluasi?", "id": "Menentukan apakah deteksi itu benar." },
  { "en": "Apa itu True Positive (TP)?", "id": "Deteksi benar objek yang memang ada." },
  { "en": "Apa itu False Positive (FP)?", "id": "Deteksi salah, objek tidak ada." },
  { "en": "Apa itu False Negative (FN)?", "id": "Gagal deteksi objek yang ada." },
  { "en": "Apa itu Precision dalam evaluasi?", "id": "Proporsi deteksi benar dari total deteksi." },
  { "en": "Rumus dasar untuk Precision adalah?", "id": "TP dibagi (TP ditambah FP)." },
  { "en": "Apa itu Recall atau Sensitivity?", "id": "Proporsi objek terdeteksi dari total objek." },
  { "en": "Rumus dasar untuk Recall adalah?", "id": "TP dibagi (TP ditambah FN)." },
  { "en": "Apa itu kurva Precision-Recall?", "id": "Grafik hubungan antara presisi dan recall." },
  { "en": "Apa itu Average Precision (AP)?", "id": "Rata-rata presisi di berbagai recall." },
  { "en": "Bagaimana mAP (mean AP) dihitung?", "id": "Rata-rata AP untuk semua kelas." },
  { "en": "Nilai mAP yang baik itu bagaimana?", "id": "Semakin tinggi nilainya semakin baik." },
  { "en": "Selain mAP, metrik kecepatan apa penting?", "id": "FPS (Frames Per Second)." },
  { "en": "Apa arti notasi mAP@0.5?", "id": "mAP dengan IoU threshold 0.5." },
  { "en": "Apa arti mAP@[.5:.95] (COCO)?", "id": "Rata-rata mAP pada berbagai IoU." },
  { "en": "Apa itu confidence threshold inferensi?", "id": "Ambang batas skor kepercayaan minimum." },
  { "en": "Apa itu F1-Score dalam metrik?", "id": "Rata-rata harmonik presisi dan recall." },
  { "en": "Apa itu GFLOPS pada model AI?", "id": "Ukuran kompleksitas komputasi suatu model." },
  { "en": "Aplikasi YOLO pada mobil otonom?", "id": "Deteksi kendaraan, pejalan kaki, rambu." },
  { "en": "Aplikasi YOLO dalam bidang keamanan?", "id": "Pengawasan, deteksi penyusup, keamanan publik." },
  { "en": "Aplikasi YOLO di sektor ritel?", "id": "Analisis pelanggan, manajemen stok otomatis." },
  { "en": "Aplikasi YOLO di bidang pertanian?", "id": "Deteksi hama, pemantauan kondisi tanaman." },
  { "en": "Aplikasi YOLO di dunia medis?", "id": "Analisis citra medis, deteksi sel." },
  { "en": "Aplikasi YOLO konservasi satwa liar?", "id": "Pelacakan hewan, anti-perburuan liar." },
  { "en": "Aplikasi YOLO dalam analisis olahraga?", "id": "Pelacakan pemain dan bola otomatis." },
  { "en": "Aplikasi YOLO di sektor manufaktur?", "id": "Kontrol kualitas, deteksi cacat produk." },
  { "en": "Aplikasi YOLO untuk drone (UAV)?", "id": "Pemetaan udara, pengawasan area luas." },
  { "en": "Bisakah YOLO untuk deteksi wajah?", "id": "Ya, bisa dilatih untuk itu." },
  { "en": "Bisakah YOLO baca plat nomor?", "id": "Ya, sebagai bagian sistem ANPR." },
  { "en": "YOLO untuk navigasi robot cerdas?", "id": "Membantu robot mengenali lingkungan sekitar." },
  { "en": "YOLO untuk aplikasi augmented reality?", "id": "Penempatan objek virtual dunia nyata." },
  { "en": "YOLO dalam sistem parkir pintar?", "id": "Deteksi ketersediaan slot parkir." },
  { "en": "YOLO untuk manajemen lalu lintas?", "id": "Pemantauan kepadatan, deteksi pelanggaran." },
  { "en": "YOLO pada perangkat edge computing?", "id": "Ya, versi ringan bisa jalan." },
  { "en": "Manfaat YOLO pada perangkat edge?", "id": "Proses lokal, latensi sangat rendah." },
  { "en": "Tantangan YOLO pada perangkat edge?", "id": "Keterbatasan sumber daya komputasi." },
  { "en": "YOLO untuk analisis video keamanan?", "id": "Keunggulan utama YOLO adalah itu." },
  { "en": "YOLO deteksi objek citra satelit?", "id": "Ya, dengan dataset yang sesuai." },
  { "en": "Industri apa banyak gunakan YOLO?", "id": "Otomotif, keamanan, ritel, manufaktur." },
  { "en": "YOLO untuk sistem presensi otomatis?", "id": "Deteksi wajah untuk pencatatan kehadiran." },
  { "en": "YOLO pemantauan lingkungan hidup?", "id": "Deteksi polusi atau deforestasi." },
  { "en": "YOLO interaksi manusia-komputer (HCI)?", "id": "Pengenalan gestur tangan pengguna." },
  { "en": "YOLO untuk sistem sortir otomatis?", "id": "Memilah objek di lini produksi." },
  { "en": "YOLO inspeksi infrastruktur publik?", "id": "Deteksi retakan jembatan atau jalan." },
  { "en": "YOLO pemantauan kerumunan massa?", "id": "Estimasi jumlah, deteksi perilaku abnormal." },
  { "en": "YOLO untuk deteksi emosi wajah?", "id": "Bisa, jika dilatih data relevan." },
  { "en": "YOLO deteksi teks dalam gambar?", "id": "Bukan utama, tapi bisa dikombinasi." },
  { "en": "Sebutkan kelebihan utama algoritma YOLO?", "id": "Sangat cepat dalam melakukan deteksi." },
  { "en": "Sebutkan kelebihan lain dari YOLO?", "id": "Memproses gambar secara global." },
  { "en": "Apakah YOLO termasuk algoritma akurat?", "id": "Versi baru memiliki akurasi tinggi." },
  { "en": "Apakah YOLO mudah diimplementasikan?", "id": "Beberapa versi sangat mudah digunakan." },
  { "en": "Kekurangan awal YOLO (misal YOLOv1)?", "id": "Kurang akurat pada objek kecil." },
  { "en": "Kekurangan lain dari model YOLOv1?", "id": "Kesulitan objek berkelompok rapat." },
  { "en": "Apakah YOLO boros sumber daya?", "id": "Relatif efisien dibanding detektor dua-tahap." },
  { "en": "Tantangan YOLO objek sangat kecil?", "id": "Masih menjadi area riset aktif." },
  { "en": "Tantangan objek aspek rasio ekstrem?", "id": "Perlu anchor atau desain khusus." },
  { "en": "Apakah YOLO solusi terbaik selalu?", "id": "Tergantung kasus penggunaan spesifiknya." },
  { "en": "Kapan YOLO menjadi pilihan unggul?", "id": "Saat kecepatan adalah prioritas utama." },
  { "en": "Kapan metode lain mungkin lebih baik?", "id": "Jika akurasi tertinggi mutlak diperlukan." },
  { "en": "Bagaimana trade-off kecepatan dan akurasi?", "id": "Umumnya, model cepat kurang akurat." },
  { "en": "Apakah YOLO butuh banyak data?", "id": "Ya, seperti model deep learning." },
  { "en": "Kesulitan YOLO dengan objek baru?", "id": "Performa menurun tanpa fine-tuning." },
  { "en": "Masalah localization error pada YOLO?", "id": "Bisa terjadi, terutama versi awal." },
  { "en": "Apakah YOLO bisa salah klasifikasi?", "id": "Ya, tidak ada model sempurna." },
  { "en": "Ketergantungan pada kualitas data training?", "id": "Sangat bergantung pada kualitas data." },
  { "en": "Apakah YOLO sensitif variasi pencahayaan?", "id": "Augmentasi data dapat membantu." },
  { "en": "Apakah YOLO sulit untuk di-debug?", "id": "Bisa jadi, seperti model DL." },
  { "en": "Kompleksitas implementasi custom YOLO sendiri?", "id": "Membutuhkan pemahaman yang mendalam." },
  { "en": "Keterbatasan YOLO pada dataset kecil?", "id": "Rentan overfitting, transfer learning membantu." },
  { "en": "Apakah YOLO hasilkan banyak false positive?", "id": "Tergantung threshold dan kualitas training." },
  { "en": "Bagaimana mengatasi kekurangan-kekurangan YOLO?", "id": "Versi baru, teknik training, post-processing." },
  { "en": "Apakah biaya komputasi training tinggi?", "id": "Ya, GPU bertenaga biasanya diperlukan." },
  { "en": "Apakah hasil YOLO selalu konsisten?", "id": "Dipengaruhi banyak faktor, termasuk inisialisasi." },
  { "en": "Tantangan etika penggunaan teknologi YOLO?", "id": "Privasi, bias dalam data training." },
  { "en": "Apakah YOLO bisa deteksi objek teroklusi?", "id": "Kemampuan terbatas, tergantung tingkat oklusi." },
  { "en": "Performa YOLO kondisi cuaca buruk?", "id": "Bisa menurun, perlu data beragam." },
  { "en": "Apa itu Non-Maximum Suppression (NMS)?", "id": "Menghilangkan kotak batas prediksi redundan." },
  { "en": "Bagaimana cara kerja NMS umum?", "id": "Pilih skor tertinggi, hapus tumpang tindih." },
  { "en": "Parameter penting dalam NMS apa?", "id": "IoU threshold dan score threshold." },
  { "en": "Apa itu teknik soft-NMS?", "id": "Mengurangi skor kotak yang tumpang tindih." },
  { "en": "Apa itu Bag of Freebies (BoF)?", "id": "Teknik training tanpa biaya inferensi." },
  { "en": "Sebutkan contoh teknik BoF populer?", "id": "Augmentasi data (CutMix, Mosaic), DropBlock." },
  { "en": "Apa itu Bag of Specials (BoS)?", "id": "Modul tingkatkan akurasi, sedikit biaya." },
  { "en": "Sebutkan contoh modul BoS populer?", "id": "Mish activation, SPP, SAM, PAN." },
  { "en": "Apa itu label assignment training?", "id": "Menentukan grid/anchor mana bertanggung jawab." },
  { "en": "Sebutkan teknik label assignment canggih?", "id": "SimOTA, TAL, Hungarian algorithm." },
  { "en": "Apa itu Generalized IoU (GIoU) Loss?", "id": "Peningkatan IoU loss, atasi non-overlap." },
  { "en": "Apa itu Distance IoU (DIoU) Loss?", "id": "Mempertimbangkan jarak pusat antar kotak." },
  { "en": "Apa itu Complete IoU (CIoU) Loss?", "id": "Mempertimbangkan aspek rasio kotak juga." },
  { "en": "Apa itu Focus layer YOLOv5?", "id": "Mengurangi komputasi, pertahankan informasi penting." },
  { "en": "Apa itu SiLU activation function?", "id": "Fungsi aktivasi halus, non-monotonik." },
  { "en": "Apa itu Mish activation function?", "id": "Fungsi aktivasi canggih mirip SiLU." },
  { "en": "Apa itu RepVGG block arsitektur?", "id": "Konversi arsitektur training ke inferensi." },
  { "en": "Apa itu quantization model AI?", "id": "Mengurangi presisi bobot model AI." },
  { "en": "Manfaat utama dari quantization model?", "id": "Model lebih kecil, inferensi cepat." },
  { "en": "Apa itu pruning model AI?", "id": "Menghilangkan bobot/koneksi tidak penting." },
  { "en": "Manfaat utama dari pruning model?", "id": "Model lebih ringan dan efisien." },
  { "en": "Apa itu knowledge distillation AI?", "id": "Melatih model kecil dari model besar." },
  { "en": "Apa itu anchor-free object detection?", "id": "Deteksi tanpa anchor boxes pra-definisi." },
  { "en": "Keuntungan detektor model anchor-free?", "id": "Lebih sederhana, generalisasi sering baik." },
  { "en": "Contoh model YOLO anchor-free?", "id": "YOLOX, YOLOv8, beberapa varian." },
  { "en": "Apakah YOLO pakai arsitektur Transformer?", "id": "Beberapa varian baru mulai mengadopsi." },
  { "en": "Apa itu ELAN (EfficientNet-style Layer Aggregation)?", "id": "Blok arsitektur efisien di YOLOv7." },
  { "en": "Apa itu model scaling YOLO?", "id": "Menyesuaikan kedalaman/lebar model sistematis." },
  { "en": "Apa itu reparameterization dalam model?", "id": "Mengubah struktur blok saat inferensi." },
  { "en": "Apa itu NVIDIA TensorRT SDK?", "id": "Library optimasi inferensi deep learning." },
  { "en": "Bagaimana TensorRT percepat inferensi YOLO?", "id": "Optimasi layer, presisi rendah, fusi." },
  { "en": "Apa itu format model ONNX?", "id": "Format pertukaran model antar framework." },
  { "en": "Bisakah model YOLO diekspor ONNX?", "id": "Ya, banyak versi mendukung ONNX." },
  { "en": "Apa itu self-supervised learning YOLO?", "id": "Training tanpa label manusia ekstensif." },
  { "en": "Apa itu zero-shot object detection?", "id": "Deteksi kelas tak terlihat saat training." },
  { "en": "YOLO World dukung zero-shot detection?", "id": "Ya, melalui deskripsi teks input." },
  { "en": "Dataset populer deteksi objek apa?", "id": "COCO, Pascal VOC, OpenImages, ImageNet." },
  { "en": "Apa itu dataset COCO singkatan?", "id": "Common Objects in Context dataset." },
  { "en": "Berapa jumlah kelas dataset COCO?", "id": "80 kategori objek umum." },
  { "en": "Apa itu dataset Pascal VOC?", "id": "Visual Object Classes challenge dataset." },
  { "en": "Berapa jumlah kelas Pascal VOC?", "id": "20 kategori objek." },
  { "en": "Format label anotasi COCO apa?", "id": "JSON (JavaScript Object Notation)." },
  { "en": "Format label anotasi Pascal VOC?", "id": "XML (Extensible Markup Language)." },
  { "en": "Sebutkan alat anotasi objek?", "id": "LabelImg, CVAT, Roboflow, VGG VIA." },
  { "en": "Platform untuk mengelola dataset AI?", "id": "Roboflow, V7 Labs, Dataloop, Scale." },
  { "en": "Framework Darknet ditulis bahasa apa?", "id": "C dan CUDA." },
  { "en": "Kelebihan utama framework Darknet?", "id": "Cepat, fleksibel untuk riset YOLO." },
  { "en": "Kekurangan dari framework Darknet?", "id": "Kurang user-friendly dibanding framework lain." },
  { "en": "Implementasi YOLO PyTorch populer?", "id": "Ultralytics YOLOv5, YOLOv8, lainnya." },
  { "en": "Kelebihan implementasi YOLO PyTorch?", "id": "Komunitas besar, mudah, fleksibel." },
  { "en": "Implementasi YOLO TensorFlow populer?", "id": "Beberapa repositori GitHub tersedia." },
  { "en": "Bisakah YOLO jalan hanya CPU?", "id": "Ya, tapi inferensi akan lambat." },
  { "en": "Apakah GPU perlu inferensi YOLO?", "id": "Sangat dianjurkan untuk performa real-time." },
  { "en": "Jenis GPU cocok untuk YOLO?", "id": "NVIDIA GPU dengan dukungan CUDA." },
  { "en": "Apa itu Google Colaboratory (Colab)?", "id": "Platform Jupyter notebook gratis GPU." },
  { "en": "Bisakah YOLO dilatih Google Colab?", "id": "Ya, cocok untuk eksperimen awal." },
  { "en": "Apa itu Docker deployment YOLO?", "id": "Memudahkan deployment lingkungan konsisten." },
  { "en": "Perintah instalasi YOLOv5 Ultralytics?", "id": "pip install yolov5." },
  { "en": "Perintah instalasi YOLOv8 Ultralytics?", "id": "pip install ultralytics." },
  { "en": "Contoh perintah inferensi YOLOv8 CLI?", "id": "yolo predict model=yolov8n.pt source=img.jpg" },
  { "en": "Bisakah YOLO dengan library OpenCV?", "id": "Ya, OpenCV bisa muat model." },
  { "en": "Format bobot model Darknet YOLO?", "id": ".weights." },
  { "en": "Format bobot model PyTorch YOLO?", "id": ".pt atau .pth." },
  { "en": "Apa itu file .cfg Darknet?", "id": "Mendefinisikan arsitektur jaringan model." },
  { "en": "Apa itu file .yaml YOLOv5/v8?", "id": "Konfigurasi dataset, model, dan training." },
  { "en": "Bagaimana visualisasi hasil deteksi YOLO?", "id": "Gambar kotak batas pada gambar." },
  { "en": "Library Python visualisasi deteksi?", "id": "OpenCV, Matplotlib, Pillow." },
  { "en": "Apakah ada GUI untuk YOLO?", "id": "Beberapa proyek komunitas sediakan GUI." },
  { "en": "Bisakah YOLO integrasi dengan webcam?", "id": "Ya, sangat umum dilakukan pengguna." },
  { "en": "Format video input untuk YOLO?", "id": "Format umum (MP4, AVI, MOV)." },
  { "en": "Bagaimana simpan hasil deteksi video?", "id": "Simpan sebagai video baru beranotasi." },
  { "en": "Tantangan implementasi YOLO di mobile?", "id": "Ukuran model, konsumsi daya baterai." },
  { "en": "Framework untuk YOLO di mobile?", "id": "TensorFlow Lite, PyTorch Mobile, CoreML." },
  { "en": "Ada model YOLO pra-latih mobile?", "id": "Ya, versi ringan sering tersedia." },
  { "en": "Pentingnya memilih ukuran input gambar?", "id": "Mempengaruhi akurasi dan kecepatan inferensi." },
  { "en": "Ukuran input gambar umum YOLO?", "id": "416x416, 640x640, 1280x1280 piksel." },
  { "en": "Bagaimana tangani gambar ukuran beda?", "id": "Resize atau padding ukuran tetap." },
  { "en": "YOLO vs Faster R-CNN: Cepat mana?", "id": "YOLO jauh lebih cepat." },
  { "en": "YOLO vs Faster R-CNN: Akurat mana?", "id": "Versi baru YOLO sangat akurat." },
  { "en": "YOLO vs SSD: Perbedaan utama?", "id": "Cara prediksi kotak dan skala." },
  { "en": "Apakah SSD juga detektor satu-tahap?", "id": "Ya, SSD detektor satu-tahap." },
  { "en": "YOLO vs RetinaNet: Keunggulan RetinaNet?", "id": "Mengatasi class imbalance (Focal Loss)." },
  { "en": "YOLO vs EfficientDet: Fokus EfficientDet?", "id": "Skalabilitas model yang sangat efisien." },
  { "en": "YOLO vs DETR: Perbedaan utama?", "id": "DETR gunakan transformer, tanpa NMS." },
  { "en": "Apakah YOLO masih relevan sekarang?", "id": "Ya, sangat relevan terus berkembang." },
  { "en": "YOLO dibandingkan model segmentasi?", "id": "YOLO deteksi, segmentasi lebih detail." },
  { "en": "Bisakah YOLO dimodifikasi segmentasi?", "id": "Ya, ada varian seperti YOLACT." },
  { "en": "Apa itu instance segmentation task?", "id": "Deteksi dan segmentasi setiap instans." },
  { "en": "YOLO untuk estimasi pose manusia?", "id": "Ya, YOLOv8 dukung estimasi pose." },
  { "en": "Apa itu estimasi pose task?", "id": "Mendeteksi keypoints tubuh manusia/objek." },
  { "en": "Performa YOLO dataset benchmark?", "id": "Versi baru sering capai SOTA." },
  { "en": "Apa itu kompetisi COCO Detection?", "id": "Ajang bergengsi uji model deteksi." },
  { "en": "Apakah YOLO sering menang kompetisi?", "id": "Varian YOLO sering kinerja baik." },
  { "en": "Seberapa besar komunitas pengguna YOLO?", "id": "Sangat besar, aktif, dan suportif." },
  { "en": "Di mana cari bantuan YOLO?", "id": "GitHub issues, forum, Stack Overflow." },
  { "en": "Kontribusi utama Joseph Redmon AI?", "id": "Pencipta YOLO dan framework Darknet." },
  { "en": "Mengapa Redmon berhenti riset CV?", "id": "Alasan etika penggunaan militer." },
  { "en": "Siapa yang lanjutkan pengembangan YOLO?", "id": "Banyak peneliti dan berbagai organisasi." },
  { "en": "Peran Ultralytics ekosistem YOLO?", "id": "Kembangkan dan populerkan YOLOv5, YOLOv8." },
  { "en": "Peran Meituan ekosistem YOLO?", "id": "Mengembangkan seri YOLOv6 untuk industri." },
  { "en": "Dampak YOLO bidang computer vision?", "id": "Revolusi deteksi objek secara real-time." },
  { "en": "Apakah YOLO dipakai industri besar?", "id": "Ya, diadopsi luas berbagai industri." },
  { "en": "Bagaimana lisensi software untuk YOLO?", "id": "Bervariasi, banyak open source (GPL)." },
  { "en": "Apakah ada kursus online YOLO?", "id": "Ya, banyak tersedia berbagai platform." },
  { "en": "YOLO sulit dipelajari untuk pemula?", "id": "Konsep dasar mudah, implementasi usaha." },
  { "en": "Bagaimana masa depan algoritma YOLO?", "id": "Lebih akurat, cepat, efisien, fitur." },
  { "en": "Pendekatan YOLOv1 untuk deteksi objek?", "id": "Regresi tunggal kotak dan kelas." },
  { "en": "Berapa grid cell YOLOv1 pakai?", "id": "7x7 grid cells." },
  { "en": "Berapa bounding box per sel YOLOv1?", "id": "Dua bounding boxes." },
  { "en": "Backbone YOLOv1 inspirasi dari mana?", "id": "Arsitektur GoogLeNet." },
  { "en": "Apakah YOLOv1 gunakan anchor boxes?", "id": "Tidak, YOLOv1 tidak pakai anchor." },
  { "en": "Apa itu Fast YOLO (varian YOLOv1)?", "id": "Versi lebih kecil dan cepat." },
  { "en": "Teknik baru apa di YOLOv2?", "id": "Batch Norm, High Res, Anchors." },
  { "en": "Apa itu Dimension Clusters YOLOv2?", "id": "K-means pada anchor box priors." },
  { "en": "Apa itu Direct location prediction?", "id": "Prediksi offset relatif ke grid." },
  { "en": "Backbone YOLOv2 namanya apa?", "id": "Darknet-19." },
  { "en": "Bagaimana YOLO9000 deteksi banyak kelas?", "id": "Gabung dataset deteksi dan klasifikasi." },
  { "en": "Apa itu WordTree di YOLO9000?", "id": "Struktur hirarki untuk label kelas." },
  { "en": "Backbone YOLOv3 berapa lapisan konvolusi?", "id": "53 lapisan (Darknet-53)." },
  { "en": "YOLOv3 prediksi pada berapa skala?", "id": "Tiga skala berbeda (multi-scale)." },
  { "en": "Loss function klasifikasi YOLOv3?", "id": "Binary cross-entropy per kelas." },
  { "en": "Varian kecil YOLOv3 populer?", "id": "YOLOv3-tiny." },
  { "en": "Filosofi utama di balik YOLOv4?", "id": "Kecepatan dan akurasi optimal." },
  { "en": "Backbone utama digunakan YOLOv4?", "id": "CSPDarknet53." },
  { "en": "Neck digunakan pada arsitektur YOLOv4?", "id": "SPP, PANet." },
  { "en": "Contoh Bag of Freebies YOLOv4?", "id": "CutMix, Mosaic, DropBlock, Label Smoothing." },
  { "en": "Contoh Bag of Specials YOLOv4?", "id": "Mish, CSP, SAM, PAN." },
  { "en": "Keunggulan utama framework YOLOv5?", "id": "Mudah digunakan, training cepat, dokumentasi." },
  { "en": "Arsitektur backbone YOLOv5 berbasis apa?", "id": "Modifikasi CSPNet dan Focus Layer." },
  { "en": "Neck digunakan pada arsitektur YOLOv5?", "id": "PANet (Path Aggregation Network)." },
  { "en": "Sebutkan varian model YOLOv5?", "id": "n, s, m, l, x." },
  { "en": "Lisensi software digunakan YOLOv5?", "id": "GNU GPL v3.0." },
  { "en": "Fokus utama pengembangan YOLOv6?", "id": "Efisiensi untuk aplikasi industri." },
  { "en": "Desain backbone YOLOv6 pakai blok?", "id": "EfficientRep (berbasis blok RepVGG)." },
  { "en": "Desain neck YOLOv6 disebut apa?", "id": "RepBiFPAN." },
  { "en": "Apakah YOLOv6 pakai decoupled head?", "id": "Ya, versi awal menggunakannya." },
  { "en": "Lisensi software digunakan YOLOv6?", "id": "Lisensi permisif (misal Apache 2.0)." },
  { "en": "Inovasi arsitektur utama YOLOv7?", "id": "E-ELAN (Extended ELAN)." },
  { "en": "Bagaimana YOLOv7 lakukan model scaling?", "id": "Menyesuaikan model berbagai ukuran." },
  { "en": "Apa itu reparameterized convolution YOLOv7?", "id": "Teknik optimasi efisiensi inferensi." },
  { "en": "Apakah YOLOv7 pakai auxiliary head?", "id": "Ya, untuk membantu proses training." },
  { "en": "Fokus YOLOv7 selain akurasi/kecepatan?", "id": "Efisiensi proses training modelnya." },
  { "en": "Apakah YOLOv8 bersifat anchor-free?", "id": "Ya, YOLOv8 adalah anchor-free." },
  { "en": "Backbone YOLOv8 pakai modul apa?", "id": "Modifikasi CSPDarknet, dengan modul C2f." },
  { "en": "Head digunakan pada YOLOv8?", "id": "Decoupled head dan anchor-free." },
  { "en": "Tugas apa saja didukung YOLOv8?", "id": "Deteksi, segmentasi, klasifikasi, pose." },
  { "en": "Loss function YOLOv8 melibatkan apa?", "id": "TaskAlignedAssigner, CIoU, DFL." },
  { "en": "Apa kepanjangan dari YOLO-NAS?", "id": "YOLO Neural Architecture Search." },
  { "en": "Siapa yang kembangkan YOLO-NAS?", "id": "Perusahaan Deci AI." },
  { "en": "Apakah YOLO-NAS quantization-aware?", "id": "Ya, dirancang untuk proses kuantisasi." },
  { "en": "Mesin NAS apa YOLO-NAS pakai?", "id": "AutoNAC (milik Deci AI)." },
  { "en": "Blok baru arsitektur YOLO-NAS?", "id": "Quantization-friendly basic block inovatif." },
  { "en": "Kemampuan utama model YOLO-World?", "id": "Deteksi objek open-vocabulary." },
  { "en": "Siapa pengembang model YOLO-World?", "id": "Tim Tencent AI Lab." },
  { "en": "Bagaimana cara kerja YOLO-World?", "id": "Belajar asosiasi visual dan linguistik." },
  { "en": "Text encoder apa YOLO-World pakai?", "id": "Pre-trained CLIP text encoder." },
  { "en": "Vision backbone YOLO-World berbasis apa?", "id": "RepVL-PAN (berbasis YOLOv8)." },
  { "en": "Apa itu grid sensitivity problem?", "id": "Masalah objek batas grid cell." },
  { "en": "Apa itu focal loss function?", "id": "Mengatasi class imbalance objek sulit." },
  { "en": "Apa itu hard negative mining?", "id": "Fokus training sampel negatif sulit." },
  { "en": "Masalah: CUDA out of memory?", "id": "Kurangi batch size atau resolusi." },
  { "en": "Masalah: mAP sangat rendah sekali?", "id": "Periksa data, augmentasi, hyperparameter." },
  { "en": "Pentingnya normalisasi data input YOLO?", "id": "Ya, bantu konvergensi proses training." },
  { "en": "Apa itu vanishing gradient problem?", "id": "Gradien kecil, training lambat/stagnan." },
  { "en": "Apa itu exploding gradient problem?", "id": "Gradien besar, training tidak stabil." },
  { "en": "Apa itu dropout regularization?", "id": "Teknik regularisasi, nonaktifkan neuron acak." },
  { "en": "Apa itu label smoothing regularization?", "id": "Mencegah model terlalu percaya diri." },
  { "en": "Pentingnya bersihkan dataset training?", "id": "Sangat penting untuk hasil baik." },
  { "en": "Apa itu model checkpointing training?", "id": "Menyimpan bobot model secara berkala." },
  { "en": "Alat visualisasi metrik training?", "id": "TensorBoard atau Weights & Biases." },
  { "en": "Kenapa validasi mAP bisa nol?", "id": "Masalah anotasi, konfigurasi, atau training." },
  { "en": "Apa itu reproducibility deep learning?", "id": "Kemampuan menghasilkan ulang hasil eksperimen." },
  { "en": "Pengaruh random seed dalam training?", "id": "Kontrol inisialisasi acak untuk reproducibility." },
  { "en": "Apa itu data leakage AI?", "id": "Informasi data tes bocor training." },
  { "en": "Bagaimana pilih IoU threshold NMS?", "id": "Eksperimen, tergantung kepadatan objek." },
  { "en": "Bagaimana pilih confidence threshold inferensi?", "id": "Trade-off antara presisi dan recall." },
  { "en": "Apa itu few-shot object detection?", "id": "Deteksi dengan sedikit sampel data." },
  { "en": "Etika: Bias dalam dataset YOLO?", "id": "Bisa sebabkan performa tidak adil." },
  { "en": "Etika: Penggunaan YOLO untuk pengawasan?", "id": "Timbulkan kekhawatiran terkait privasi." },
  { "en": "Bagaimana kurangi bias model YOLO?", "id": "Dataset beragam, teknik fairness AI." },
  { "en": "Tren YOLO masa depan bagaimana?", "id": "Efisien, multi-task, self-supervised." },
  { "en": "YOLO dan Neural Architecture Search?", "id": "Tren untuk arsitektur lebih optimal." },
  { "en": "YOLO dan arsitektur model Transformer?", "id": "Integrasi fitur Transformer mungkin meningkat." },
  { "en": "YOLO dan self-supervised learning?", "id": "Kurangi ketergantungan label manual." },
  { "en": "YOLO video konsistensi temporal?", "id": "Area riset aktif dan berkembang." },
  { "en": "YOLO perangkat keras khusus (ASIC)?", "id": "Potensi efisiensi daya lebih tinggi." },
  { "en": "YOLO dengan Explainable AI (XAI)?", "id": "Penting pahami keputusan model AI." },
  { "en": "YOLO untuk deteksi objek 3D?", "id": "Varian YOLO untuk data 3D." },
  { "en": "YOLO dan continual learning (CL)?", "id": "Model belajar terus tanpa lupa." },
  { "en": "YOLO dengan on-device learning?", "id": "Model belajar langsung perangkat pengguna." },
  { "en": "YOLO lebih hemat energi (Green AI)?", "id": "Penting untuk mobile dan edge." },
  { "en": "Kombinasi YOLO dengan sensor lain?", "id": "Sensor fusion (LIDAR) lebih robust." },
  { "en": "YOLO untuk domain adaptation AI?", "id": "Model kerja baik domain beda." },
  { "en": "Tantangan terbesar YOLO ke depan?", "id": "Generalisasi, robustnes, efisiensi, etika." },
  { "en": "YOLO dan open-world object detection?", "id": "Deteksi objek tak dikenal." },
  { "en": "Apakah akan ada versi YOLOvNext?", "id": "Pengembangan akan terus berlanjut pesat." },
  { "en": "Peran komunitas evolusi YOLO?", "id": "Sangat besar, dorong inovasi berkelanjutan." },
  { "en": "YOLO algoritma untuk tugas apa?", "id": "Deteksi objek gambar/video." },
  { "en": "Arti dari 'You Only Look Once'?", "id": "Proses gambar satu kali jalan." },
  { "en": "YOLO bagi gambar input jadi?", "id": "Kumpulan sel grid (grid cells)." },
  { "en": "Output YOLO objek terdeteksi?", "id": "Kotak batas (bounding box) dan kelas." },
  { "en": "Metrik evaluasi performa utama YOLO?", "id": "mAP dan FPS." },
  { "en": "Fungsi Non-Maximum Suppression (NMS)?", "id": "Hapus prediksi kotak tumpang tindih." },
  { "en": "Bagian Backbone arsitektur YOLO berfungsi?", "id": "Ekstraksi fitur penting gambar." },
  { "en": "Bagian Neck arsitektur YOLO berfungsi?", "id": "Agregasi fitur berbagai level." },
  { "en": "Bagian Head arsitektur YOLO berfungsi?", "id": "Prediksi akhir (kotak, kelas)." },
  { "en": "Anchor boxes bantu prediksi objek?", "id": "Objek berbagai ukuran/rasio aspek." },
  { "en": "Tujuan utama data augmentation?", "id": "Tingkatkan variasi data set training." },
  { "en": "Manfaat utama transfer learning?", "id": "Percepat training, butuh data sedikit." },
  { "en": "COCO contoh dari apa?", "id": "Dataset standar deteksi objek." },
  { "en": "Darknet awalnya apa untuk YOLO?", "id": "Framework neural network asli YOLO." },
  { "en": "PyTorch konteks YOLO adalah apa?", "id": "Framework deep learning populer YOLO." },
  { "en": "FPS (Frames Per Second) ukur apa?", "id": "Kecepatan inferensi atau deteksi model." },
  { "en": "IoU singkatan dari apa itu?", "id": "Intersection over Union." },
  { "en": "Loss function training ukur apa?", "id": "Seberapa besar kesalahan prediksi model." },
  { "en": "Siapa pengembang utama seri YOLOv8?", "id": "Perusahaan teknologi Ultralytics." },
  { "en": "YOLO-World dirancang deteksi jenis apa?", "id": "Deteksi objek open-vocabulary." },
  { "en": "Apakah YOLO bisa image segmentation?", "id": "Ya, varian tertentu dukung segmentasi." },
  { "en": "Apakah YOLO bisa pose estimation?", "id": "Ya, varian tertentu dukung pose." },
  { "en": "Arti dari model 'anchor-free'?", "id": "Tidak gunakan anchor boxes pra-definisi." },
  { "en": "Apa itu deteksi objek real-time?", "id": "Deteksi sangat cepat, seolah instan." },
  { "en": "YOLO pertama dikenalkan tahun berapa?", "id": "Tahun 2016 oleh Joseph Redmon." },
  { "en": "YOLO selalu butuh hardware GPU?", "id": "Tidak selalu, tapi GPU percepat." },
  { "en": "Apa itu bounding box regression?", "id": "Prediksi koordinat (x,y,w,h) kotak." },
  { "en": "Apa itu class prediction YOLO?", "id": "Prediksi label kategori objek." },
  { "en": "Pengembangan YOLO masih aktifkah?", "id": "Ya, sangat aktif versi baru." },
  { "en": "Fungsi aktivasi Leaky ReLU apa?", "id": "Non-linearitas, hindari dying ReLU." },
  { "en": "Apa itu 'bottleneck' arsitektur CNN?", "id": "Lapisan yang kurangi jumlah fitur." },
  { "en": "Apa itu 'upsampling' YOLO neck?", "id": "Tingkatkan resolusi peta fitur." },
  { "en": "Apa itu 'strides' convolutional layer?", "id": "Langkah pergeseran filter konvolusi." },
  { "en": "Apa itu 'padding' convolutional layer?", "id": "Tambahkan piksel sekitar batas gambar." },
  { "en": "Manfaat 'padding=same' konvolusi?", "id": "Jaga ukuran output sama input." },
  { "en": "Apa itu 'feature map' CNN?", "id": "Representasi fitur hasil dari konvolusi." },
  { "en": "YOLOv1 prediksi multiple objects/cell?", "id": "Prediksi B boxes, pilih IoU." },
  { "en": "Batasan 'spatial constraint' YOLOv1?", "id": "Satu objek per grid cell." },
  { "en": "Apa itu 'passthrough layer' YOLOv2?", "id": "Bawa fitur resolusi tinggi akhir." },
  { "en": "Teknik 'joint training' YOLO9000?", "id": "Deteksi dan klasifikasi ribuan kelas." },
  { "en": "Apa itu 'logistic regression' objectness?", "id": "Prediksi skor keberadaan objek kotak." },
  { "en": "YOLOv3 gunakan berapa anchor/skala?", "id": "Tiga anchor box per skala." },
  { "en": "Apa itu 'residual block' Darknet-53?", "id": "Mungkinkan training jaringan sangat dalam." },
  { "en": "Bagaimana YOLOv4 optimasi proses training?", "id": "Gunakan banyak teknik BoF BoS." },
  { "en": "Apa itu 'CSP' connection arsitektur?", "id": "Kurangi komputasi, jaga akurasi model." },
  { "en": "Fitur auto-anchor YOLOv5 untuk apa?", "id": "Otomatis optimasi anchor boxes dataset." },
  { "en": "Apa itu 'quantization-aware training'?", "id": "Training model optimasi kuantisasi." },
  { "en": "YOLOv6 'hardware-friendly design' artinya?", "id": "Desain efisien berbagai perangkat keras." },
  { "en": "Teknik 'model re-parameterization' untuk apa?", "id": "Ubah arsitektur inferensi jadi cepat." },
  { "en": "Apa itu 'label assignment' dinamis?", "id": "Penentuan tanggung jawab prediksi adaptif." },
  { "en": "Apa itu 'auxiliary losses' training?", "id": "Loss tambahan bantu optimasi fitur." },
  { "en": "YOLOv8 gunakan 'C2f module', apa?", "id": "Modul CSP dua alur fitur." },
  { "en": "Apa itu 'Distribution Focal Loss'?", "id": "Loss regresi distribusi kotak halus." },
  { "en": "Apa 'prompt engineering' YOLO-World?", "id": "Buat deskripsi teks input efektif." },
  { "en": "YOLO tangani variasi skala objek?", "id": "Multi-scale features, FPN, anchor." },
  { "en": "YOLO bisa deteksi objek transparan?", "id": "Sangat sulit, tergantung kualitas data." },
  { "en": "YOLO bisa deteksi objek cepat?", "id": "Tergantung FPS kamera dan model." },
  { "en": "Bagaimana tingkatkan FPS YOLO CPU?", "id": "Model ringan, kuantisasi, optimasi." },
  { "en": "Apa itu 'model zoo' YOLO?", "id": "Koleksi model pra-latih siap." },
  { "en": "Bisakah YOLO dilatih satu kelas?", "id": "Ya, sangat bisa dan umum." },
  { "en": "Apa 'validation set' training AI?", "id": "Dataset evaluasi model selama training." },
  { "en": "Apa 'test set' machine learning?", "id": "Dataset final evaluasi performa model." },
  { "en": "Bisakah YOLO salah deteksi objek?", "id": "Ya, bisa hasilkan false positive." },
  { "en": "Pentingnya resolusi gambar input YOLO?", "id": "Resolusi tinggi deteksi objek kecil." },
  { "en": "Apakah YOLO bisa di Raspberry Pi?", "id": "Ya, model ringan dengan optimasi." },
  { "en": "Apa itu 'edge TPU' akselerasi AI?", "id": "ASIC Google inferensi edge AI." },
  { "en": "Bisakah YOLO gabung pelacakan objek?", "id": "Ya, sering kombinasi dengan DeepSORT." },
  { "en": "Apa 'Kalman Filter' dalam pelacakan?", "id": "Prediksi posisi objek masa depan." },
  { "en": "Apa 'Hungarian algorithm' pelacakan?", "id": "Cocokkan deteksi dengan track ada." },
  { "en": "Apakah YOLO bisa estimasi kedalaman?", "id": "Tidak langsung, butuh input stereo." },
  { "en": "Apa itu 'data-centric AI' konsep?", "id": "Fokus pada kualitas kuantitas data." },
  { "en": "Apa itu 'model-centric AI' konsep?", "id": "Fokus pada pengembangan arsitektur model." },
  { "en": "YOLO termasuk pendekatan model-centric?", "id": "Awalnya ya, kini data penting." },
  { "en": "Augmentasi offline lebih baik online?", "id": "Tergantung kasus, online lebih fleksibel." },
  { "en": "Apa 'synthetic data' untuk training?", "id": "Data buatan komputer untuk augmentasi." },
  { "en": "Bisakah YOLO deteksi objek sketsa?", "id": "Jika dilatih dataset sketsa." },
  { "en": "Apa 'active learning' untuk anotasi?", "id": "Model bantu pilih data informatif." },
  { "en": "Ada batasan jumlah kelas YOLO?", "id": "Praktis tidak, tapi performa turun." },
  { "en": "Apa 'attention mechanism' dalam CNN?", "id": "Fokus pada bagian penting fitur." },
  { "en": "YOLO gunakan attention mechanism?", "id": "Beberapa varian baru mulai gunakan." },
  { "en": "Bagaimana YOLO kurangi false negatives?", "id": "Threshold, augmentasi, model lebih baik." },
  { "en": "Apa itu 'model compression' AI?", "id": "Teknik kurangi ukuran model AI." },
  { "en": "Contoh teknik model compression?", "id": "Pruning, quantization, knowledge distillation." },
  { "en": "YOLO bisa untuk deteksi anomali?", "id": "Bisa, jika anomali visual jelas." },
  { "en": "Apa 'adversarial attack' pada YOLO?", "id": "Input dirancang menipu model YOLO." },
  { "en": "Bagaimana bertahan dari adversarial attack?", "id": "Adversarial training, input sanitization." },
  { "en": "Apa itu 'federated learning' AI?", "id": "Training model terdistribusi tanpa bagi." },
  { "en": "Bisakah YOLO dilatih federated learning?", "id": "Teoritis bisa, implementasi kompleks." },
  { "en": "Apa 'differential privacy' dalam AI?", "id": "Jaga privasi data individu training." },
  { "en": "YOLO versi terbaru selalu baik?", "id": "Umumnya ya, tapi ada trade-off." },
  { "en": "Bagaimana pilih versi YOLO tepat?", "id": "Sesuaikan kebutuhan kecepatan, akurasi, kemudahan." },
  { "en": "YOLO 'human-in-the-loop' anotasi?", "id": "Bisa percepat proses anotasi data." },
  { "en": "Apa 'global context' dilihat YOLO?", "id": "Informasi seluruh gambar, bukan patch." },
  { "en": "Apakah YOLO bisa deteksi suara?", "id": "Tidak, YOLO untuk data visual." },
  { "en": "Bagaimana pengaruh blur deteksi YOLO?", "id": "Bisa turunkan akurasi deteksi objek." },
  { "en": "Bagaimana pengaruh noise deteksi YOLO?", "id": "Juga bisa turunkan akurasi deteksi." },
  { "en": "Pentingnya dataset seimbang (balanced)?", "id": "Cegah bias model kelas mayoritas." },
  { "en": "Teknik atasi dataset tidak seimbang?", "id": "Oversampling, undersampling, focal loss, bobot." },
  { "en": "Apa 'heatmap' prediksi YOLO visualisasi?", "id": "Visualisasi area fokus deteksi model." },
  { "en": "Bisakah YOLO jalan di browser?", "id": "Ya, dengan TensorFlow.js atau ONNX.js." },
  { "en": "Apa 'pipeline' deteksi objek AI?", "id": "Serangkaian langkah input ke output." },
  { "en": "YOLO bagian dari pipeline apa?", "id": "Sistem cerdas, robotika, analisis video." },
  { "en": "Apa 'inference server' untuk YOLO?", "id": "Server layani permintaan deteksi model." },
  { "en": "Contoh inference server model AI?", "id": "NVIDIA Triton, TensorFlow Serving, TorchServe." },
  { "en": "Hasil prediksi YOLO deterministik?", "id": "Dengan seed tetap, bisa deterministik." },
  { "en": "Apa 'benchmark' untuk model AI?", "id": "Dataset dan metrik standar perbandingan." },
  { "en": "Seberapa sering versi YOLO baru?", "id": "Cukup sering, bidang berkembang pesat." }





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
            }, 5000);
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
