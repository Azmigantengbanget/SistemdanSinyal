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


  { "en": "Apa definisi sinyal?", "id": "Representasi informasi fenomena fisik." },
  { "en": "Apa definisi sistem?", "id": "Entitas pemroses sinyal input." },
  { "en": "Variabel independen sinyal?", "id": "Umumnya adalah waktu." },
  { "en": "Variabel dependen sinyal?", "id": "Amplitudo atau nilai sinyal." },
  { "en": "Apa itu sinyal waktu-kontinyu?", "id": "Terdefinisi di setiap waktu." },
  { "en": "Apa itu sinyal waktu-diskrit?", "id": "Terdefinisi di waktu tertentu." },
  { "en": "Contoh sinyal waktu-kontinyu?", "id": "Sinyal suara analog." },
  { "en": "Contoh sinyal waktu-diskrit?", "id": "Data sampel digital." },
  { "en": "Apa itu sinyal analog?", "id": "Amplitudo dapat bernilai apapun." },
  { "en": "Apa itu sinyal digital?", "id": "Amplitudo memiliki nilai terbatas." },
  { "en": "Sinyal periodik adalah?", "id": "Sinyal yang berulang secara periodik." },
  { "en": "Sinyal aperiodik adalah?", "id": "Sinyal yang tidak berulang." },
  { "en": "Apa simbol periode fundamental?", "id": "Simbol T untuk waktu-kontinyu." },
  { "en": "Sebutkan dua klasifikasi sinyal.", "id": "Deterministik dan acak." },
  { "en": "Apa itu sinyal deterministik?", "id": "Dapat dinyatakan dalam fungsi matematis." },
  { "en": "Apa itu sinyal acak?", "id": "Tidak bisa diprediksi secara pasti." },
  { "en": "Apa itu sinyal genap?", "id": "Simetris terhadap sumbu vertikal." },
  { "en": "Apa itu sinyal ganjil?", "id": "Simetris terhadap titik asal." },
  { "en": "Hasil perkalian sinyal genap?", "id": "Sinyal genap." },
  { "en": "Hasil perkalian genap-ganjil?", "id": "Sinyal ganjil." },
  { "en": "Apa itu sinyal energi?", "id": "Energi terbatas, daya nol." },
  { "en": "Apa itu sinyal daya?", "id": "Daya terbatas, energi tak terbatas." },
  { "en": "Operasi dasar pada sinyal?", "id": "Pergeseran, penskalaan, pembalikan." },
  { "en": "Apa itu pergeseran waktu?", "id": "Menggeser sinyal di sumbu waktu." },
  { "en": "Apa itu penskalaan waktu?", "id": "Mengompres atau meregangkan sinyal." },
  { "en": "Apa itu pembalikan waktu?", "id": "Mencerminkan sinyal terhadap sumbu y." },
  { "en": "Fungsi impuls satuan disebut?", "id": "Fungsi delta Dirac." },
  { "en": "Nilai delta Dirac saat t=0?", "id": "Tak terhingga." },
  { "en": "Luas area impuls satuan?", "id": "Satu." },
  { "en": "Fungsi undak satuan disebut?", "id": "Fungsi Heaviside." },
  { "en": "Nilai undak satuan untuk t>0?", "id": "Satu." },
  { "en": "Nilai undak satuan untuk t<0?", "id": "Nol." },
  { "en": "Hubungan impuls dan undak satuan?", "id": "Impuls adalah turunan undak." },
  { "en": "Apa itu fungsi ramp?", "id": "Sinyal yang naik secara linear." },
  { "en": "Apa itu sinyal eksponensial?", "id": "Sinyal berbentuk pangkat eksponen." },
  { "en": "Apa itu sinyal sinusoidal?", "id": "Berbentuk fungsi sinus atau kosinus." },
  { "en": "Klasifikasi sistem berdasarkan output?", "id": "SISO dan MIMO." },
  { "en": "Singkatan SISO (Single Input Single Output)?", "id": "Single Input Single Output." },
  { "en": "Singkatan MIMO (Multiple Input Multiple Output)?", "id": "Multiple Input Multiple Output." },
  { "en": "Sistem statis artinya?", "id": "Output hanya bergantung input saat ini." },
  { "en": "Sistem dinamis artinya?", "id": "Output bergantung pada input masa lalu." },
  { "en": "Nama lain sistem statis?", "id": "Sistem tanpa memori." },
  { "en": "Nama lain sistem dinamis?", "id": "Sistem dengan memori." },
  { "en": "Apa itu sistem kausal?", "id": "Output tidak mendahului input." },
  { "en": "Apa itu sistem non-kausal?", "id": "Output bergantung input masa depan." },
  { "en": "Sistem di dunia nyata bersifat?", "id": "Selalu kausal." },
  { "en": "Apa itu sistem linear?", "id": "Memenuhi prinsip superposisi." },
  { "en": "Prinsip superposisi terdiri dari?", "id": "Aditivitas dan homogenitas." },
  { "en": "Apa itu aditivitas?", "id": "Respon jumlah input sama jumlah respon." },
  { "en": "Apa itu homogenitas?", "id": "Respon input terskala sama respon terskala." },
  { "en": "Apa itu sistem non-linear?", "id": "Tidak memenuhi prinsip superposisi." },
  { "en": "Apa itu sistem time-invariant?", "id": "Karakteristiknya tidak berubah oleh waktu." },
  { "en": "Nama lain time-invariant?", "id": "Invarian waktu atau LTI." },
  { "en": "Uji sistem time-invariant?", "id": "Pergeseran input menggeser output sama." },
  { "en": "Apa itu sistem time-variant?", "id": "Karakteristiknya berubah oleh waktu." },
  { "en": "Singkatan LTI (Linear Time-Invariant)?", "id": "Linear Time-Invariant." },
  { "en": "Sistem LTI adalah?", "id": "Sistem linear dan time-invariant." },
  { "en": "Mengapa sistem LTI penting?", "id": "Analisisnya lebih mudah dilakukan." },
  { "en": "Respon sistem LTI ditentukan oleh?", "id": "Respon impuls sistem." },
  { "en": "Apa itu respon impuls?", "id": "Output sistem saat input impuls." },
  { "en": "Operasi untuk mencari output LTI?", "id": "Konvolusi." },
  { "en": "Simbol operasi konvolusi?", "id": "Tanda bintang atau asterisk (*)." },
  { "en": "Apa itu integral konvolusi?", "id": "Konvolusi untuk sinyal waktu-kontinyu." },
  { "en": "Apa itu penjumlahan konvolusi?", "id": "Konvolusi untuk sinyal waktu-diskrit." },
  { "en": "Apa properti komutatif konvolusi?", "id": "Urutan sinyal tidak penting." },
  { "en": "Apa properti distributif konvolusi?", "id": "Berlaku pada penjumlahan sinyal." },
  { "en": "Apa properti asosiatif konvolusi?", "id": "Pengelompokan sinyal tidak penting." },
  { "en": "Apa itu sistem stabil?", "id": "Input terbatas hasilkan output terbatas." },
  { "en": "Nama lain sistem stabil?", "id": "Sistem stabil BIBO." },
  { "en": "Singkatan BIBO (Bounded-Input Bounded-Output)?", "id": "Bounded-Input Bounded-Output." },
  { "en": "Syarat stabilitas sistem LTI?", "id": "Respon impuls absolutnya terintegralkan." },
  { "en": "Apa itu sistem invertibel?", "id": "Input bisa didapat dari output." },
  { "en": "Apa syarat sistem invertibel?", "id": "Terdapat sistem invers yang unik." },
  { "en": "Contoh sistem dengan memori?", "id": "Integrator atau kapasitor." },
  { "en": "Contoh sistem tanpa memori?", "id": "Resistor ideal." },
  { "en": "Sistem kausal bergantung pada?", "id": "Input masa kini dan lalu." },
  { "en": "Apa sinyal identitas konvolusi?", "id": "Fungsi impuls satuan." },
  { "en": "Apa itu dekomposisi sinyal?", "id": "Memecah sinyal menjadi komponen sederhana." },
  { "en": "Sinyal bisa diurai menjadi?", "id": "Komponen genap dan ganjil." },
  { "en": "Representasi sinyal diskrit?", "id": "Urutan angka atau sekuens." },
  { "en": "Apa itu periode sampling?", "id": "Interval waktu antar sampel." },
  { "en": "Apa itu frekuensi sampling?", "id": "Jumlah sampel per detik." },
  { "en": "Hubungan frekuensi dan periode sampling?", "id": "Saling berbanding terbalik." },
  { "en": "Apa itu sinyal real?", "id": "Nilai amplitudonya bilangan real." },
  { "en": "Apa itu sinyal kompleks?", "id": "Nilai amplitudonya bilangan kompleks." },
  { "en": "Bagian dari sinyal kompleks?", "id": "Bagian real dan imajiner." },
  { "en": "Apa itu energi sinyal diskrit?", "id": "Jumlah kuadrat nilai absolutnya." },
  { "en": "Apa itu daya sinyal diskrit?", "id": "Rata-rata energi per sampel." },
  { "en": "Sistem seri adalah?", "id": "Output sistem satu jadi input lain." },
  { "en": "Sistem paralel adalah?", "id": "Input sama untuk beberapa sistem." },
  { "en": "Analisis domain waktu adalah?", "id": "Analisis sinyal terhadap waktu." },
  { "en": "Analisis domain frekuensi adalah?", "id": "Analisis sinyal terhadap frekuensi." },
  { "en": "Transformasi ke domain frekuensi?", "id": "Transformasi Fourier." },
  { "en": "Sistem apa yang paling umum?", "id": "Sistem LTI." },
  { "en": "Apa itu respon undak?", "id": "Output sistem saat input undak." },
  { "en": "Hubungan respon undak dan impuls?", "id": "Respon undak adalah integral respon impuls." },
  { "en": "Sistem LTI (Linear Time-Invariant) kausal bergantung pada?", "id": "Nilai masa kini dan lalu." },
  { "en": "Syarat kausalitas respon impuls?", "id": "h(t) nol untuk t < 0." },
  { "en": "Sistem LTI (Linear Time-Invariant) stabil jika?", "id": "Respon impulsnya terintegralkan mutlak." },
  { "en": "Respon impuls sistem seri?", "id": "Konvolusi dari tiap respon impuls." },
  { "en": "Respon impuls sistem paralel?", "id": "Penjumlahan dari tiap respon impuls." },
  { "en": "Sistem umpan balik adalah?", "id": "Output mempengaruhi input kembali." },
  { "en": "Apa itu dekonvolusi?", "id": "Proses untuk membalikkan konvolusi." },
  { "en": "Respon frekuensi sistem LTI (Linear Time-Invariant)?", "id": "Transformasi Fourier dari respon impuls." },
  { "en": "Tujuan analisis domain frekuensi?", "id": "Melihat karakteristik sinyal vs frekuensi." },
  { "en": "Metode analisis sinyal periodik?", "id": "Deret Fourier." },
  { "en": "Siapa penemu Deret Fourier?", "id": "Jean-Baptiste Joseph Fourier." },
  { "en": "Deret Fourier merepresentasikan sinyal sebagai?", "id": "Penjumlahan tak hingga gelombang sinusoida." },
  { "en": "Komponen dasar Deret Fourier?", "id": "Sinusoida yang berhubungan secara harmonik." },
  { "en": "Apa itu harmonik?", "id": "Kelipatan bilangan bulat frekuensi fundamental." },
  { "en": "Apa itu frekuensi fundamental?", "id": "Kebalikan dari periode sinyal." },
  { "en": "Apa itu koefisien Deret Fourier?", "id": "Amplitudo dan fasa setiap harmonik." },
  { "en": "Bentuk Deret Fourier ada berapa?", "id": "Tiga: trigonometri, polar, eksponensial." },
  { "en": "Apa syarat sinyal punya Deret Fourier?", "id": "Memenuhi kondisi Dirichlet." },
  { "en": "Sebutkan satu kondisi Dirichlet.", "id": "Harus bernilai tunggal." },
  { "en": "Sebutkan kondisi Dirichlet kedua.", "id": "Jumlah maksima/minima terbatas." },
  { "en": "Sebutkan kondisi Dirichlet ketiga.", "id": "Harus terintegralkan secara absolut." },
  { "en": "Singkatan CTFS (Continuous-Time Fourier Series)?", "id": "Continuous-Time Fourier Series." },
  { "en": "Koefisien a0 pada deret merepresentasikan?", "id": "Komponen DC atau nilai rata-rata." },
  { "en": "Koefisien an pada deret?", "id": "Amplitudo komponen kosinus." },
  { "en": "Koefisien bn pada deret?", "id": "Amplitudo komponen sinus." },
  { "en": "Koefisien deret sinyal genap?", "id": "Hanya komponen kosinus (an)." },
  { "en": "Koefisien deret sinyal ganjil?", "id": "Hanya komponen sinus (bn)." },
  { "en": "Apa itu spektrum frekuensi?", "id": "Plot koefisien Fourier vs frekuensi." },
  { "en": "Apa itu spektrum amplitudo?", "id": "Plot magnitudo koefisien vs frekuensi." },
  { "en": "Apa itu spektrum fasa?", "id": "Plot fasa koefisien vs frekuensi." },
  { "en": "Spektrum amplitudo sinyal real?", "id": "Selalu fungsi genap." },
  { "en": "Spektrum fasa sinyal real?", "id": "Selalu fungsi ganjil." },
  { "en": "Apa itu Teorema Parseval?", "id": "Daya sinyal di domain waktu & frekuensi." },
  { "en": "Daya total sinyal periodik?", "id": "Jumlah kuadrat magnitudo koefisien." },
  { "en": "Apa itu fenomena Gibbs?", "id": "Overshoot pada titik diskontinuitas." },
  { "en": "Bagaimana mengurangi fenomena Gibbs?", "id": "Dengan fungsi windowing." },
  { "en": "Sifat linearitas deret Fourier?", "id": "Deret dari jumlahan sinyal." },
  { "en": "Sifat pergeseran waktu deret Fourier?", "id": "Mengubah fasa koefisien." },
  { "en": "Sifat penskalaan waktu deret Fourier?", "id": "Mengubah periode fundamental." },
  { "en": "Deret Fourier dari sinyal turunan?", "id": "Koefisiennya dikalikan jnω₀." },
  { "en": "Deret Fourier dari sinyal integral?", "id": "Koefisiennya dibagi jnω₀." },
  { "en": "Apa itu sinyal ortogonal?", "id": "Integral perkaliannya nol." },
  { "en": "Fungsi basis deret Fourier?", "id": "Himpunan fungsi sinusoidal ortogonal." },
  { "en": "Metode analisis sinyal aperiodik?", "id": "Transformasi Fourier." },
  { "en": "Singkatan CTFT (Continuous-Time Fourier Transform)?", "id": "Continuous-Time Fourier Transform." },
  { "en": "CTFT (Continuous-Time Fourier Transform) mengubah sinyal dari?", "id": "Domain waktu ke domain frekuensi." },
  { "en": "Hasil dari Transformasi Fourier?", "id": "Spektrum frekuensi yang kontinyu." },
  { "en": "Syarat eksistensi Transformasi Fourier?", "id": "Memenuhi kondisi Dirichlet." },
  { "en": "Apa itu pasangan Transformasi Fourier?", "id": "Sinyal dan hasil transformasinya." },
  { "en": "Transformasi Fourier sinyal impuls?", "id": "Satu." },
  { "en": "Transformasi Fourier sinyal kotak?", "id": "Fungsi sinc." },
  { "en": "Apa itu fungsi sinc?", "id": "Sinus(x) dibagi x." },
  { "en": "Transformasi Fourier sinyal undak?", "id": "Tidak ada (secara matematis murni)." },
  { "en": "Apa itu dualitas?", "id": "Bentuk sinyal di satu domain." },
  { "en": "Apa properti dualitas Fourier?", "id": "Bentuk sinyal dan spektrumnya bisa ditukar." },
  { "en": "Konvolusi di domain waktu menjadi?", "id": "Perkalian di domain frekuensi." },
  { "en": "Perkalian di domain waktu menjadi?", "id": "Konvolusi di domain frekuensi." },
  { "en": "Transformasi Fourier sinyal periodik?", "id": "Deretan impuls di domain frekuensi." },
  { "en": "Apa itu bandwidth sinyal?", "id": "Lebar pita frekuensi sinyal." },
  { "en": "Singkatan DFT (Discrete Fourier Transform)?", "id": "Discrete Fourier Transform." },
  { "en": "DFT (Discrete Fourier Transform) digunakan untuk sinyal?", "id": "Sinyal diskrit dan terbatas." },
  { "en": "Singkatan FFT (Fast Fourier Transform)?", "id": "Fast Fourier Transform." },
  { "en": "FFT (Fast Fourier Transform) adalah?", "id": "Algoritma efisien untuk menghitung DFT." },
  { "en": "Apa fungsi Transformasi Laplace?", "id": "Menganalisis sistem LTI lebih luas." },
  { "en": "Domain Transformasi Laplace?", "id": "Domain frekuensi kompleks (s-plane)." },
  { "en": "Variabel kompleks s terdiri dari?", "id": "Bagian real (σ) dan imajiner (jω)." },
  { "en": "Transformasi Laplace disebut juga?", "id": "Generalisasi dari Transformasi Fourier." },
  { "en": "Apa itu ROC (Region of Convergence)?", "id": "Daerah konvergensi Transformasi Laplace." },
  { "en": "ROC (Region of Convergence) menentukan apa?", "id": "Sifat kausalitas dan stabilitas sistem." },
  { "en": "Bentuk ROC (Region of Convergence) di s-plane?", "id": "Strip atau bidang." },
  { "en": "ROC (Region of Convergence) untuk sinyal sisi kanan?", "id": "Bidang di sebelah kanan pole." },
  { "en": "ROC (Region of Convergence) untuk sinyal sisi kiri?", "id": "Bidang di sebelah kiri pole." },
  { "en": "ROC (Region of Convergence) untuk sinyal durasi terbatas?", "id": "Seluruh s-plane." },
  { "en": "Sistem LTI (Linear Time-Invariant) kausal jika ROC (Region of Convergence)?", "id": "Di sebelah kanan pole paling kanan." },
  { "en": "Sistem LTI (Linear Time-Invariant) stabil jika ROC (Region of Convergence)?", "id": "Mencakup sumbu jω." },
  { "en": "Apa itu pole?", "id": "Nilai s yang membuat transfer function tak hingga." },
  { "en": "Apa itu zero?", "id": "Nilai s yang membuat transfer function nol." },
  { "en": "Apa itu fungsi transfer sistem?", "id": "Transformasi Laplace dari respon impuls." },
  { "en": "Fungsi transfer menghubungkan?", "id": "Output dan input di domain s." },
  { "en": "Analisis sistem menggunakan pole-zero?", "id": "Melihat respon frekuensi dan stabilitas." },
  { "en": "Transformasi Laplace dari turunan?", "id": "Dikalikan dengan s." },
  { "en": "Transformasi Laplace dari integral?", "id": "Dibagi dengan s." },
  { "en": "Apa itu Teorema Nilai Awal?", "id": "Mencari nilai awal sinyal dari domain s." },
  { "en": "Apa itu Teorema Nilai Akhir?", "id": "Mencari nilai akhir sinyal dari domain s." },
  { "en": "Syarat Teorema Nilai Akhir?", "id": "Semua pole berada di LHP." },
  { "en": "Singkatan LHP (Left-Half Plane)?", "id": "Left-Half Plane." },
  { "en": "Singkatan RHP (Right-Half Plane)?", "id": "Right-Half Plane." },
  { "en": "Pole di RHP (Right-Half Plane) menandakan?", "id": "Sistem tidak stabil." },
  { "en": "Pole di sumbu jω menandakan?", "id": "Sistem marginal stabil atau osilator." },
  { "en": "Apa itu invers Transformasi Laplace?", "id": "Mengembalikan sinyal ke domain waktu." },
  { "en": "Metode invers Transformasi Laplace?", "id": "Ekspansi pecahan parsial." },
  { "en": "Apa itu sinyal waktu-diskrit?", "id": "Sinyal yang terdefinisi pada integer." },
  { "en": "Notasi sinyal waktu-diskrit?", "id": "Menggunakan kurung siku x[n]." },
  { "en": "Apa itu sekuens?", "id": "Representasi dari sinyal waktu-diskrit." },
  { "en": "Apa itu impuls satuan diskrit?", "id": "Bernilai satu hanya di n=0." },
  { "en": "Notasi impuls satuan diskrit?", "id": "Delta[n] atau δ[n]." },
  { "en": "Apa itu undak satuan diskrit?", "id": "Bernilai satu untuk n >= 0." },
  { "en": "Notasi undak satuan diskrit?", "id": "u[n]." },
  { "en": "Hubungan impuls dan undak diskrit?", "id": "Impuls adalah selisih undak pertama." },
  { "en": "Apa itu sistem waktu-diskrit?", "id": "Sistem yang memproses sinyal diskrit." },
  { "en": "Output sistem LTI (Linear Time-Invariant) diskrit?", "id": "Hasil dari penjumlahan konvolusi." },
  { "en": "Respon impuls h[n] adalah?", "id": "Output sistem saat input impuls diskrit." },
  { "en": "Sistem diskrit kausal jika?", "id": "h[n] = 0 untuk n < 0." },
  { "en": "Sistem diskrit stabil jika?", "id": "Respon impulsnya terjumlahkan mutlak." },
  { "en": "Apa itu persamaan beda?", "id": "Deskripsi matematis sistem diskrit." },
  { "en": "Orde dari persamaan beda?", "id": "Perbedaan maksimum antara indeks n." },
  { "en": "Apa itu sistem rekursif?", "id": "Output bergantung pada output sebelumnya." },
  { "en": "Apa itu sistem non-rekursif?", "id": "Output hanya bergantung pada input." },
  { "en": "Sistem FIR (Finite Impulse Response) adalah?", "id": "Sistem non-rekursif." },
  { "en": "Singkatan FIR (Finite Impulse Response)?", "id": "Finite Impulse Response." },
  { "en": "Sistem IIR (Infinite Impulse Response) adalah?", "id": "Sistem rekursif." },
  { "en": "Singkatan IIR (Infinite Impulse Response)?", "id": "Infinite Impulse Response." },
  { "en": "Analisis frekuensi sinyal diskrit?", "id": "Menggunakan Transformasi Fourier Waktu-Diskrit." },
  { "en": "Singkatan DTFT (Discrete-Time Fourier Transform)?", "id": "Discrete-Time Fourier Transform." },
  { "en": "Hasil dari DTFT (Discrete-Time Fourier Transform)?", "id": "Spektrum frekuensi kontinyu dan periodik." },
  { "en": "Spektrum DTFT (Discrete-Time Fourier Transform) periodik dengan?", "id": "Periode 2π." },
  { "en": "DTFT (Discrete-Time Fourier Transform) mengubah sekuens dari?", "id": "Domain n ke domain frekuensi." },
  { "en": "Variabel frekuensi pada DTFT (Discrete-Time Fourier Transform)?", "id": "Omega (ω)." },
  { "en": "DTFT (Discrete-Time Fourier Transform) dari impuls diskrit?", "id": "Satu." },
  { "en": "DTFT (Discrete-Time Fourier Transform) dari sekuens undak?", "id": "Kombinasi deret impuls dan fungsi lain." },
  { "en": "Sifat konvolusi pada DTFT (Discrete-Time Fourier Transform)?", "id": "Konvolusi menjadi perkalian." },
  { "en": "Sifat perkalian pada DTFT (Discrete-Time Fourier Transform)?", "id": "Perkalian menjadi konvolusi periodik." },
  { "en": "Apa itu Transformasi-Z?", "id": "Generalisasi dari DTFT." },
  { "en": "Transformasi-Z adalah counterpart dari?", "id": "Transformasi Laplace." },
  { "en": "Domain dari Transformasi-Z?", "id": "Bidang-z (z-plane) yang kompleks." },
  { "en": "Variabel pada Transformasi-Z?", "id": "Variabel kompleks z." },
  { "en": "Hubungan z dengan DTFT (Discrete-Time Fourier Transform)?", "id": "Saat z berada di unit circle." },
  { "en": "Hubungan variabel z dan s?", "id": "z = e pangkat sT." },
  { "en": "Apa itu ROC (Region of Convergence) Transformasi-Z?", "id": "Daerah konvergensi di bidang-z." },
  { "en": "Bentuk ROC (Region of Convergence) pada bidang-z?", "id": "Berbentuk cincin atau disk." },
  { "en": "ROC (Region of Convergence) tidak pernah mengandung?", "id": "Pole." },
  { "en": "ROC (Region of Convergence) sekuens durasi terbatas?", "id": "Seluruh bidang-z, kecuali z=0 atau tak hingga." },
  { "en": "ROC (Region of Convergence) sekuens sisi kanan?", "id": "Di luar lingkaran terluar." },
  { "en": "ROC (Region of Convergence) sekuens sisi kiri?", "id": "Di dalam lingkaran terdalam." },
  { "en": "ROC (Region of Convergence) sekuens dua sisi?", "id": "Berbentuk cincin (annulus)." },
  { "en": "Fungsi transfer sistem diskrit?", "id": "Transformasi-Z dari respon impuls h[n]." },
  { "en": "Notasi fungsi transfer diskrit?", "id": "H(z)." },
  { "en": "Pole pada bidang-z?", "id": "Nilai z yang membuat H(z) tak hingga." },
  { "en": "Zero pada bidang-z?", "id": "Nilai z yang membuat H(z) nol." },
  { "en": "Apa itu unit circle?", "id": "Lingkaran di bidang-z dengan radius satu." },
  { "en": "Unit circle berhubungan dengan?", "id": "Stabilitas sistem." },
  { "en": "Sistem LTI (Linear Time-Invariant) diskrit kausal jika?", "id": "ROC-nya di luar pole terluar." },
  { "en": "Sistem LTI (Linear Time-Invariant) diskrit stabil jika?", "id": "ROC-nya mencakup unit circle." },
  { "en": "Sistem kausal dan stabil jika?", "id": "Semua pole di dalam unit circle." },
  { "en": "Pole di luar unit circle?", "id": "Menyebabkan sistem tidak stabil." },
  { "en": "Pole di atas unit circle?", "id": "Sistem marginal stabil atau osilator." },
  { "en": "Apa itu plot pole-zero?", "id": "Visualisasi pole dan zero di bidang-z." },
  { "en": "Sifat pergeseran waktu Transformasi-Z?", "id": "Dikalikan dengan z pangkat -n₀." },
  { "en": "Sifat penskalaan di domain-z?", "id": "Mengubah posisi pole dan zero." },
  { "en": "Sifat konvolusi pada Transformasi-Z?", "id": "Menjadi perkalian sederhana." },
  { "en": "Apa itu Invers Transformasi-Z?", "id": "Mengembalikan sekuens ke domain waktu." },
  { "en": "Metode Invers Transformasi-Z?", "id": "Ekspansi pecahan parsial atau pembagian." },
  { "en": "Apa itu sampling?", "id": "Konversi sinyal kontinyu ke diskrit." },
  { "en": "Proses sampling menghasilkan?", "id": "Sekuens dari sampel-sampel." },
  { "en": "Apa itu Teorema Sampling?", "id": "Menentukan laju sampling minimum." },
  { "en": "Nama lain Teorema Sampling?", "id": "Teorema Nyquist-Shannon." },
  { "en": "Frekuensi sampling harus?", "id": "Lebih dari dua kali frekuensi maksimum." },
  { "en": "Apa itu frekuensi Nyquist?", "id": "Dua kali frekuensi sinyal maksimum." },
  { "en": "Apa itu laju Nyquist?", "id": "Laju sampling minimum (frekuensi Nyquist)." },
  { "en": "Apa akibat laju sampling terlalu rendah?", "id": "Terjadinya aliasing." },
  { "en": "Apa itu aliasing?", "id": "Frekuensi tinggi menyamar jadi frekuensi rendah." },
  { "en": "Bagaimana menghindari aliasing?", "id": "Sampling di atas laju Nyquist." },
  { "en": "Peran filter sebelum sampling?", "id": "Anti-aliasing filter." },
  { "en": "Apa itu filter anti-aliasing?", "id": "Filter low-pass untuk membatasi bandwidth." },
  { "en": "Apa itu rekonstruksi sinyal?", "id": "Proses mengembalikan sinyal kontinyu." },
  { "en": "Rekonstruksi ideal menggunakan?", "id": "Filter low-pass ideal." },
  { "en": "Nama lain proses rekonstruksi?", "id": "Interpolasi." },
  { "en": "Apa itu zero-order hold?", "id": "Metode rekonstruksi praktis." },
  { "en": "Apa itu downsampling atau decimation?", "id": "Mengurangi laju sampling sinyal." },
  { "en": "Apa itu upsampling atau interpolation?", "id": "Meningkatkan laju sampling sinyal." },
  { "en": "Singkatan ADC (Analog-to-Digital Converter)?", "id": "Analog-to-Digital Converter." },
  { "en": "Proses di dalam ADC (Analog-to-Digital Converter)?", "id": "Sampling dan kuantisasi." },
  { "en": "Apa itu kuantisasi?", "id": "Pembulatan amplitudo ke level diskrit." },
  { "en": "Apa itu error kuantisasi?", "id": "Selisih sinyal asli dan terkuantisasi." },
  { "en": "Singkatan DAC (Digital-to-Analog Converter)?", "id": "Digital-to-Analog Converter." },
  { "en": "Fungsi DAC (Digital-to-Analog Converter)?", "id": "Mengubah sinyal digital ke analog." },
  { "en": "Apa itu sistem kausal FIR (Finite Impulse Response)?", "id": "Selalu stabil." },
  { "en": "Apa itu sistem kausal IIR (Infinite Impulse Response)?", "id": "Bisa stabil atau tidak stabil." },
  { "en": "Stabilitas IIR (Infinite Impulse Response) bergantung pada?", "id": "Posisi pole-polenya." },
  { "en": "Respons frekuensi sistem FIR (Finite Impulse Response)?", "id": "Dihitung dengan DFT dari h[n]." },
  { "en": "Respons frekuensi sistem IIR (Infinite Impulse Response)?", "id": "Evaluasi H(z) pada unit circle." },
  { "en": "Zero di atas unit circle?", "id": "Menekan frekuensi tertentu." },
  { "en": "Pole dekat unit circle?", "id": "Memperkuat frekuensi tertentu (resonansi)." },
  { "en": "Apa itu filter digital?", "id": "Sistem untuk memodifikasi spektrum sinyal." },
  { "en": "Jenis filter digital dasar?", "id": "Low-pass, high-pass, band-pass, band-stop." },
  { "en": "Filter low-pass (LPF) melakukan?", "id": "Melewatkan frekuensi rendah." },
  { "en": "Filter high-pass (HPF) melakukan?", "id": "Melewatkan frekuensi tinggi." },
  { "en": "Apa itu filter band-pass?", "id": "Melewatkan rentang frekuensi tertentu." },
  { "en": "Apa itu filter band-stop?", "id": "Menolak rentang frekuensi tertentu." },
  { "en": "Nama lain filter band-stop?", "id": "Filter notch atau band-reject." },
  { "en": "Apa itu passband?", "id": "Pita frekuensi yang dilewatkan filter." },
  { "en": "Apa itu stopband?", "id": "Pita frekuensi yang dilemahkan filter." },
  { "en": "Apa itu cut-off frequency?", "id": "Frekuensi batas antara passband dan stopband." },
  { "en": "Apa itu transition band?", "id": "Daerah transisi antara passband-stopband." },
  { "en": "Transition band yang ideal?", "id": "Sangat sempit atau nol." },
  { "en": "Apa itu passband ripple?", "id": "Fluktuasi magnitudo di dalam passband." },
  { "en": "Apa itu stopband attenuation?", "id": "Tingkat pelemahan sinyal di stopband." },
  { "en": "Filter ideal memiliki?", "id": "Respon magnitudo berbentuk kotak." },
  { "en": "Mengapa filter ideal tidak realisabel?", "id": "Karena bersifat non-kausal." },
  { "en": "Jenis aproksimasi filter analog?", "id": "Butterworth, Chebyshev, Elliptic." },
  { "en": "Karakteristik filter Butterworth?", "id": "Respon passband flat maksimal." },
  { "en": "Karakteristik filter Chebyshev Tipe I?", "id": "Memiliki ripple di passband." },
  { "en": "Karakteristik filter Chebyshev Tipe II?", "id": "Memiliki ripple di stopband." },
  { "en": "Karakteristik filter Elliptic?", "id": "Memiliki ripple di passband dan stopband." },
  { "en": "Filter mana yang transisinya paling tajam?", "id": "Filter Elliptic." },
  { "en": "Apa itu respons fasa filter?", "id": "Pergeseran fasa terhadap frekuensi." },
  { "en": "Apa itu fasa linear?", "id": "Pergeseran fasa linear terhadap frekuensi." },
  { "en": "Akibat dari fasa linear?", "id": "Tidak ada distorsi fasa." },
  { "en": "Apa itu group delay?", "id": "Ukuran rata-rata delay sinyal." },
  { "en": "Group delay pada fasa linear?", "id": "Konstan." },
  { "en": "Filter FIR (Finite Impulse Response) bisa memiliki?", "id": "Fasa linear sempurna." },
  { "en": "Filter IIR (Infinite Impulse Response) memiliki?", "id": "Fasa non-linear." },
  { "en": "Apa itu diagram blok sistem?", "id": "Representasi grafis dari persamaan sistem." },
  { "en": "Elemen dasar diagram blok?", "id": "Penjumlah, pengali, blok tunda." },
  { "en": "Apa itu realisasi sistem?", "id": "Struktur implementasi dari fungsi transfer." },
  { "en": "Apa itu realisasi Direct Form I?", "id": "Implementasi langsung dari persamaan beda." },
  { "en": "Apa itu realisasi Direct Form II?", "id": "Bentuk kanonik, butuh memori minimum." },
  { "en": "Apa itu realisasi cascade?", "id": "Menghubungkan sistem orde rendah seri." },
  { "en": "Apa itu realisasi paralel?", "id": "Menjumlahkan output sistem orde rendah." },
  { "en": "Apa itu realisasi transposed form?", "id": "Membalik semua arah panah." },
  { "en": "Struktur FIR (Finite Impulse Response) disebut juga?", "id": "Struktur tapped delay line." },
  { "en": "Apa itu korelasi sinyal?", "id": "Ukuran kemiripan antara dua sinyal." },
  { "en": "Apa itu cross-correlation?", "id": "Korelasi antara dua sinyal berbeda." },
  { "en": "Apa itu autocorrelation?", "id": "Korelasi sinyal dengan dirinya sendiri." },
  { "en": "Aplikasi cross-correlation?", "id": "Deteksi sinyal atau radar." },
  { "en": "Aplikasi autocorrelation?", "id": "Deteksi periodisitas dalam sinyal." },
  { "en": "Nilai maksimum autokorelasi terjadi saat?", "id": "Pergeseran waktu (lag) nol." },
  { "en": "Apa itu PSD (Power Spectral Density)?", "id": "Distribusi daya sinyal per frekuensi." },
  { "en": "Singkatan PSD (Power Spectral Density)?", "id": "Power Spectral Density." },
  { "en": "PSD (Power Spectral Density) adalah Transformasi Fourier dari?", "id": "Fungsi autokorelasi." },
  { "en": "Hubungan ini disebut teorema?", "id": "Teorema Wiener-Khinchin." },
  { "en": "Apa itu sinyal white noise?", "id": "Sinyal acak dengan spektrum flat." },
  { "en": "Autokorelasi dari white noise?", "id": "Fungsi impuls." },
  { "en": "Apa itu representasi state-space?", "id": "Model sistem menggunakan variabel state." },
  { "en": "State-space cocok untuk sistem?", "id": "Sistem MIMO dan non-linear." },
  { "en": "Apa itu variabel state?", "id": "Set variabel terkecil mendeskripsikan sistem." },
  { "en": "Apa itu vektor state?", "id": "Vektor yang berisi variabel state." },
  { "en": "Apa itu persamaan state?", "id": "Mendeskripsikan evolusi state dari waktu." },
  { "en": "Apa itu persamaan output?", "id": "Menghubungkan output dengan state." },
  { "en": "Berapa banyak variabel state dibutuhkan?", "id": "Sama dengan orde sistem." },
  { "en": "Apa itu matriks transisi state?", "id": "Matriks yang memetakan state awal." },
  { "en": "Apa itu observability?", "id": "Kemampuan mengamati state dari output." },
  { "en": "Apa itu controllability?", "id": "Kemampuan mengontrol state dari input." },
  { "en": "Sifat diferensiasi waktu Transformasi Laplace?", "id": "Perkalian dengan s di domain-s." },
  { "en": "Sifat integrasi waktu Transformasi Laplace?", "id": "Pembagian dengan s di domain-s." },
  { "en": "Sifat pergeseran frekuensi Transformasi Laplace?", "id": "Perkalian eksponensial di domain waktu." },
  { "en": "Sifat konvolusi pada Transformasi Laplace?", "id": "Konvolusi di waktu jadi perkalian di s." },
  { "en": "Sifat diferensiasi waktu Transformasi-Z?", "id": "Tidak sesederhana Laplace." },
  { "en": "Sifat konvolusi pada Transformasi-Z?", "id": "Konvolusi di waktu jadi perkalian di z." },
  { "en": "Teorema nilai awal Transformasi-Z?", "id": "Menentukan x[0] dari X(z)." },
  { "en": "Teorema nilai akhir Transformasi-Z?", "id": "Menentukan nilai steady-state x[n]." },
  { "en": "Perkalian dengan n di domain-diskrit?", "id": "Diferensiasi X(z) terhadap z." },
  { "en": "Perkalian dengan a^n di domain-diskrit?", "id": "Penskalaan di domain-z." },
  { "en": "Apa itu sistem all-pass?", "id": "Sistem dengan magnitudo respon konstan." },
  { "en": "Fungsi sistem all-pass?", "id": "Hanya mengubah fasa sinyal." },
  { "en": "Apa itu sistem fasa minimum?", "id": "Sistem stabil dan inversnya stabil." },
  { "en": "Pole dan zero sistem fasa minimum?", "id": "Semua di dalam unit circle." },
  { "en": "Respon frekuensi adalah evaluasi H(s) di?", "id": "Sumbu jω (s = jω)." },
  { "en": "Respon frekuensi adalah evaluasi H(z) di?", "id": "Unit circle (z = e^jω)." },
  { "en": "Apa itu aliasing domain waktu?", "id": "Overlap dalam sinyal waktu diskrit." },
  { "en": "Sinyal kausal memiliki transformasi?", "id": "Transformasi Laplace unilateral." },
  { "en": "Sinyal non-kausal memiliki transformasi?", "id": "Transformasi Laplace bilateral." },
  { "en": "Stabilitas sistem diskrit ditentukan oleh?", "id": "Lokasi pole di bidang-z." },
  { "en": "Stabilitas sistem kontinyu ditentukan oleh?", "id": "Lokasi pole di bidang-s." },
  { "en": "Apa itu sinyal pita terbatas (bandlimited)?", "id": "Spektrumnya nol di luar rentang frekuensi." },
  { "en": "Zero pada sumbu jω (bidang-s) menyebabkan?", "id": "Pelemahan total pada frekuensi itu." },
  { "en": "Zero pada unit circle (bidang-z) menyebabkan?", "id": "Pelemahan total pada frekuensi itu." },
  { "en": "Sinyal undak diskrit bisa disebut?", "id": "Sekuens Heaviside." },
  { "en": "Sinyal impuls diskrit bisa disebut?", "id": "Sekuens Kronecker delta." },
  { "en": "Apa itu kuantisasi uniform?", "id": "Setiap level kuantisasi berjarak sama." },
  { "en": "Apa itu kuantisasi non-uniform?", "id": "Jarak level kuantisasi tidak sama." },
  { "en": "Sistem linier jika memenuhi?", "id": "Aditivitas dan homogenitas." },
  { "en": "Sistem time-invariant jika?", "id": "Respon tidak bergantung waktu." },
  { "en": "Sinyal periodik memiliki spektrum?", "id": "Spektrum garis (diskrit)." },
  { "en": "Sinyal aperiodik memiliki spektrum?", "id": "Spektrum kontinyu." },
  { "en": "Transformasi Fourier adalah kasus khusus dari?", "id": "Transformasi Laplace." },
  { "en": "DTFT (Discrete-Time Fourier Transform) kasus khusus dari?", "id": "Transformasi-Z." },
  { "en": "Daya sinyal periodik terdistribusi di?", "id": "Frekuensi harmoniknya." },
  { "en": "Energi sinyal aperiodik terdistribusi di?", "id": "Seluruh spektrum frekuensi." },
  { "en": "Daya rata-rata sinyal energi?", "id": "Nol." },
  { "en": "Energi total sinyal daya?", "id": "Tak hingga." },
  { "en": "Apa itu modulasi?", "id": "Menumpangkan sinyal informasi pada pembawa." },
  { "en": "Apa itu sinyal baseband?", "id": "Sinyal informasi asli frekuensi rendah." },
  { "en": "Apa itu sinyal passband?", "id": "Sinyal termodulasi di frekuensi tinggi." },
  { "en": "Apa itu sinyal pembawa (carrier)?", "id": "Gelombang frekuensi tinggi untuk modulasi." },
  { "en": "Mengapa modulasi diperlukan?", "id": "Untuk transmisi nirkabel yang efisien." },
  { "en": "Sebutkan dua jenis modulasi analog.", "id": "AM dan FM." },
  { "en": "Singkatan AM (Amplitude Modulation)?", "id": "Amplitude Modulation." },
  { "en": "Parameter apa yang diubah pada AM (Amplitude Modulation)?", "id": "Amplitudo sinyal pembawa." },
  { "en": "Singkatan FM (Frequency Modulation)?", "id": "Frequency Modulation." },
  { "en": "Parameter apa yang diubah pada FM (Frequency Modulation)?", "id": "Frekuensi sinyal pembawa." },
  { "en": "Apa itu indeks modulasi AM (Amplitude Modulation)?", "id": "Ukuran kedalaman modulasi." },
  { "en": "Apa itu demodulasi?", "id": "Proses ekstraksi sinyal informasi kembali." },
  { "en": "Nama lain demodulasi?", "id": "Deteksi." },
  { "en": "Apa itu proses stokastik?", "id": "Model matematis untuk sinyal acak." },
  { "en": "Nama lain proses stokastik?", "id": "Proses acak." },
  { "en": "Apa itu ansambel (ensemble)?", "id": "Kumpulan semua kemungkinan realisasi sinyal." },
  { "en": "Apa itu proses stasioner?", "id": "Sifat statistiknya tidak berubah waktu." },
  { "en": "Apa itu proses ergodik?", "id": "Rata-rata waktu sama dengan rata-rata ansambel." },
  { "en": "Mean dari sinyal acak?", "id": "Nilai harapan atau rata-rata statistik." },
  { "en": "Varians dari sinyal acak?", "id": "Ukuran penyebaran dari nilai mean." },
  { "en": "Autokorelasi sinyal stasioner bergantung pada?", "id": "Hanya pada selisih waktu." },
  { "en": "Cross-korelasi dua proses acak mengukur?", "id": "Keterkaitan statistik antara keduanya." },
  { "en": "PSD (Power Spectral Density) sinyal acak?", "id": "Distribusi daya rata-rata per frekuensi." },
  { "en": "Input white noise ke sistem LTI (Linear Time-Invariant)?", "id": "Outputnya adalah sinyal acak berwarna." },
  { "en": "DFT (Discrete Fourier Transform) adalah versi sampel dari?", "id": "DTFT (Discrete-Time Fourier Transform)." },
  { "en": "Input dan output DFT (Discrete Fourier Transform)?", "id": "Keduanya sekuens diskrit dan terbatas." },
  { "en": "Panjang sekuens DFT (Discrete Fourier Transform) dinotasikan?", "id": "N." },
  { "en": "DFT (Discrete Fourier Transform) digunakan untuk?", "id": "Analisis spektral numerik di komputer." },
  { "en": "Apa itu zero-padding?", "id": "Menambahkan sampel nol pada sinyal." },
  { "en": "Fungsi zero-padding pada DFT (Discrete Fourier Transform)?", "id": "Meningkatkan resolusi tampilan spektrum." },
  { "en": "Apa itu spectral leakage?", "id": "Energi sinyal bocor ke frekuensi lain." },
  { "en": "Penyebab utama spectral leakage?", "id": "Jendela pengamatan sinyal yang terbatas." },
  { "en": "Cara mengurangi spectral leakage?", "id": "Menggunakan fungsi window." },
  { "en": "Apa itu fungsi window?", "id": "Fungsi untuk memperhalus tepi sinyal." },
  { "en": "Contoh fungsi window?", "id": "Hamming, Hanning, Blackman, Bartlett." },
  { "en": "Window rektangular menyebabkan leakage?", "id": "Paling besar." },
  { "en": "Apa itu scalloping loss?", "id": "Kesalahan amplitudo karena frekuensi sinyal." },
  { "en": "Kompleksitas komputasi DFT (Discrete Fourier Transform)?", "id": "Proporsional terhadap N kuadrat." },
  { "en": "Kompleksitas komputasi FFT (Fast Fourier Transform)?", "id": "Proporsional terhadap N log N." },
  { "en": "Prinsip dasar FFT (Fast Fourier Transform)?", "id": "Divide and conquer (pecah dan taklukkan)." },
  { "en": "Jenis algoritma FFT (Fast Fourier Transform)?", "id": "Decimation-in-time dan decimation-in-frequency." },
  { "en": "Apa itu konvolusi sirkular?", "id": "Konvolusi yang periodik." },
  { "en": "Perkalian DFT (Discrete Fourier Transform) setara dengan?", "id": "Konvolusi sirkular di domain waktu." },
  { "en": "Bagaimana menghitung konvolusi linear dengan FFT (Fast Fourier Transform)?", "id": "Menggunakan zero-padding yang cukup." },
  { "en": "Metode ini disebut?", "id": "Metode overlap-add atau overlap-save." },
  { "en": "Apa itu sistem fasa campuran?", "id": "Memiliki zero di dalam dan luar." },
  { "en": "Apa itu Hilbert Transform?", "id": "Transformasi yang menggeser fasa -90 derajat." },
  { "en": "Sinyal analitik adalah?", "id": "Sinyal kompleks dengan spektrum satu sisi." },
  { "en": "Bagian imajiner sinyal analitik?", "id": "Hilbert Transform dari bagian real." },
  { "en": "Apa itu instantaneous frequency?", "id": "Turunan dari fasa sesaat." },
  { "en": "Sistem LTI (Linear Time-Invariant) kontinyu dijelaskan oleh?", "id": "Persamaan diferensial." },
  { "en": "Sistem LTI (Linear Time-Invariant) diskrit dijelaskan oleh?", "id": "Persamaan beda." },
  { "en": "Respon natural sistem?", "id": "Respon sistem tanpa input." },
  { "en": "Respon paksa (forced) sistem?", "id": "Respon sistem akibat adanya input." },
  { "en": "Respon total sistem?", "id": "Jumlah dari respon natural dan paksa." },
  { "en": "Apa itu mode sistem?", "id": "Komponen respon natural sistem." },
  { "en": "Mode sistem ditentukan oleh?", "id": "Pole dari fungsi transfer." },
  { "en": "Pole real di LHP (Left-Half Plane)?", "id": "Mode eksponensial meluruh." },
  { "en": "Pole kompleks di LHP (Left-Half Plane)?", "id": "Mode sinusoidal teredam." },
  { "en": "Pole real di RHP (Right-Half Plane)?", "id": "Mode eksponensial tumbuh." },
  { "en": "Pole kompleks di RHP (Right-Half Plane)?", "id": "Mode sinusoidal tumbuh." },
  { "en": "Pole di sumbu imajiner?", "id": "Mode osilasi tak teredam." },
  { "en": "Pole di titik asal (s=0)?", "id": "Mode integrator." },
  { "en": "Pole di dalam unit circle?", "id": "Mode diskrit meluruh." },
  { "en": "Pole di luar unit circle?", "id": "Mode diskrit tumbuh." },
  { "en": "Pole di atas unit circle?", "id": "Mode osilasi diskrit." },
  { "en": "Apa itu diagram Bode?", "id": "Plot respon frekuensi (magnitudo dan fasa)." },
  { "en": "Sumbu horizontal diagram Bode?", "id": "Frekuensi dalam skala logaritmik." },
  { "en": "Sumbu vertikal magnitudo Bode?", "id": "Desibel (dB)." },
  { "en": "Sumbu vertikal fasa Bode?", "id": "Derajat atau radian." },
  { "en": "Apa itu gain margin?", "id": "Ukuran stabilitas relatif sistem." },
  { "en": "Apa itu phase margin?", "id": "Ukuran stabilitas relatif lainnya." },
  { "en": "Apa itu zero-phase filter?", "id": "Filter yang tidak menggeser fasa." },
  { "en": "Filter zero-phase bersifat?", "id": "Non-kausal." },
  { "en": "Apa itu sinyal kausal?", "id": "Bernilai nol untuk waktu negatif." },
  { "en": "Apa itu sinyal anti-kausal?", "id": "Bernilai nol untuk waktu positif." },
  { "en": "Apa itu kestabilan marginal?", "id": "Sistem tidak stabil tapi output terbatas." },
  { "en": "Contoh sistem marginal stabil?", "id": "Osilator sinusoidal ideal." },
  { "en": "Transformasi Fourier sinyal kosinus?", "id": "Dua impuls di domain frekuensi." },
  { "en": "Transformasi Fourier sinyal sinus?", "id": "Dua impuls imajiner." },
  { "en": "Sistem LTI (Linear Time-Invariant) diskrit disebut juga?", "id": "Filter digital." },
  { "en": "Sistem LTI (Linear Time-Invariant) kontinyu disebut juga?", "id": "Filter analog." },
  { "en": "Apa itu impulse train?", "id": "Deretan impuls satuan periodik." },
  { "en": "Transformasi Fourier impulse train?", "id": "Juga sebuah impulse train." },
  { "en": "Fungsi impulse train dalam sampling?", "id": "Model matematis untuk proses sampling." },
  { "en": "Apa itu aliasing frekuensi?", "id": "Sinonim untuk aliasing." },
  { "en": "Sinyal periodik memiliki daya?", "id": "Terbatas dan tidak nol." },
  { "en": "Sinyal aperiodik memiliki daya?", "id": "Nol." },
  { "en": "Sinyal undak satuan adalah sinyal?", "id": "Sinyal daya." },
  { "en": "Sinyal kotak adalah sinyal?", "id": "Sinyal energi." },
  { "en": "Apa itu sistem time-scaling?", "id": "Mempercepat atau memperlambat sinyal." },
  { "en": "Apa itu sistem integrator?", "id": "Output adalah integral dari input." },
  { "en": "Apa itu sistem differentiator?", "id": "Output adalah turunan dari input." },
  { "en": "Respon frekuensi integrator ideal?", "id": "Satu per j omega." },
  { "en": "Respon frekuensi differentiator ideal?", "id": "j omega." },
  { "en": "Sumbu stabilitas untuk sistem kontinyu?", "id": "Sumbu imajiner (jω) di bidang-s." },
  { "en": "Sumbu stabilitas untuk sistem diskrit?", "id": "Lingkaran satuan (unit circle) di bidang-z." },
  { "en": "Transformasi Fourier adalah evaluasi Laplace di?", "id": "Sumbu jω (s = jω)." },
  { "en": "DTFT (Discrete-Time Fourier Transform) adalah evaluasi Z-Transform di?", "id": "Unit circle (z = e^jω)." },
  { "en": "Kausalitas di bidang-s ditentukan ROC (Region of Convergence) di?", "id": "Sebelah kanan pole paling kanan." },
  { "en": "Kausalitas di bidang-z ditentukan ROC (Region of Convergence) di?", "id": "Di luar lingkaran terluar." },
  { "en": "Apa itu Energy Spectral Density?", "id": "Distribusi energi sinyal per frekuensi." },
  { "en": "Singkatan ESD (Energy Spectral Density)?", "id": "Energy Spectral Density." },
  { "en": "ESD (Energy Spectral Density) adalah kuadrat magnitudo dari?", "id": "Transformasi Fourier." },
  { "en": "ROC (Region of Convergence) dari penjumlahan dua sinyal?", "id": "Irisan dari kedua ROC." },
  { "en": "ROC (Region of Convergence) dari konvolusi dua sinyal?", "id": "Juga irisan dari kedua ROC." },
  { "en": "Zero di z=0 menandakan apa?", "id": "Pergeseran waktu ke masa depan." },
  { "en": "Pole di z=0 menandakan apa?", "id": "Pergeseran waktu ke masa lalu." },
  { "en": "Apa itu phase delay?", "id": "Delay waktu dari fasa." },
  { "en": "Apa itu group delay?", "id": "Delay waktu dari amplop sinyal." },
  { "en": "Filter fasa linear memiliki group delay?", "id": "Konstan." },
  { "en": "Apa itu metode impulse invariance?", "id": "Metode desain filter IIR." },
  { "en": "Masalah utama metode impulse invariance?", "id": "Terjadinya aliasing pada respon frekuensi." },
  { "en": "Apa itu metode bilinear transformation?", "id": "Metode desain filter IIR lain." },
  { "en": "Kelebihan utama bilinear transformation?", "id": "Menghindari aliasing sepenuhnya." },
  { "en": "Kelemahan bilinear transformation?", "id": "Menyebabkan frequency warping." },
  { "en": "Apa itu frequency warping?", "id": "Distorsi non-linear pada sumbu frekuensi." },
  { "en": "Apa itu pre-warping?", "id": "Kompensasi untuk efek frequency warping." },
  { "en": "Desain filter FIR (Finite Impulse Response) menggunakan metode?", "id": "Windowing atau optimisasi." },
  { "en": "Apa itu ekuivalensi sistem diferensial/beda?", "id": "Mengubah sistem kontinyu ke diskrit." },
  { "en": "Contoh sistem LTI (Linear Time-Invariant) kontinyu?", "id": "Sirkuit RLC (Resistor-Inductor-Capacitor)." },
  { "en": "Contoh sistem LTI (Linear Time-Invariant) diskrit?", "id": "Sistem bunga bank majemuk." },
  { "en": "Menghilangkan noise frekuensi tinggi menggunakan?", "id": "Filter low-pass." },
  { "en": "Menghilangkan dengung 50/60 Hz menggunakan?", "id": "Filter band-stop atau notch." },
  { "en": "Operasi 'sharpening' pada gambar adalah?", "id": "Operasi high-pass filtering." },
  { "en": "Operasi 'blurring' pada gambar adalah?", "id": "Operasi low-pass filtering." },
  { "en": "Transformasi Laplace dari cos(ω₀t)u(t)?", "id": "s / (s² + ω₀²)." },
  { "en": "Transformasi Laplace dari sin(ω₀t)u(t)?", "id": "ω₀ / (s² + ω₀²)." },
  { "en": "Transformasi-Z dari a^n cos(ω₀n)u[n]?", "id": "Memiliki bentuk pecahan rasional." },
  { "en": "Transformasi-Z dari a^n sin(ω₀n)u[n]?", "id": "Juga bentuk pecahan rasional." },
  { "en": "Apa itu sinyal chirp?", "id": "Sinyal yang frekuensinya berubah waktu." },
  { "en": "Apa itu spectrogram?", "id": "Visualisasi frekuensi sinyal terhadap waktu." },
  { "en": "Spectrogram dihitung menggunakan?", "id": "Short-Time Fourier Transform (STFT)." },
  { "en": "Singkatan STFT (Short-Time Fourier Transform)?", "id": "Short-Time Fourier Transform." },
  { "en": "Apa itu wavelet transform?", "id": "Analisis sinyal di skala berbeda." },
  { "en": "Kelebihan wavelet dibanding STFT (Short-Time Fourier Transform)?", "id": "Resolusi waktu-frekuensi lebih baik." },
  { "en": "Apa itu sinyal diskrit periodik?", "id": "Sekuens yang berulang setelah N sampel." },
  { "en": "Analisis sinyal diskrit periodik?", "id": "Menggunakan Discrete Fourier Series (DFS)." },
  { "en": "Singkatan DFS (Discrete Fourier Series)?", "id": "Discrete Fourier Series." },
  { "en": "Hasil dari DFS (Discrete Fourier Series)?", "id": "Sekuens koefisien yang juga periodik." },
  { "en": "Periode koefisien DFS (Discrete Fourier Series)?", "id": "Sama dengan periode sinyal (N)." },
  { "en": "Hubungan DFS (Discrete Fourier Series) dan DFT (Discrete Fourier Transform)?", "id": "DFT adalah satu periode dari DFS." },
  { "en": "Apa itu sinyal pita-dasar (baseband)?", "id": "Spektrumnya terpusat di sekitar nol." },
  { "en": "Apa itu sinyal pita-lolos (passband)?", "id": "Spektrumnya terpusat di frekuensi pembawa." },
  { "en": "Proses mengubah baseband ke passband?", "id": "Modulasi." },
  { "en": "Proses mengubah passband ke baseband?", "id": "Demodulasi." },
  { "en": "Apa itu single-sideband (SSB) modulation?", "id": "Modulasi AM yang lebih efisien." },
  { "en": "Singkatan SSB (Single-Sideband)?", "id": "Single-Sideband." },
  { "en": "Kelebihan SSB (Single-Sideband) dibanding AM (Amplitude Modulation)?", "id": "Membutuhkan bandwidth setengahnya." },
  { "en": "Apa itu frequency division multiplexing (FDM)?", "id": "Membagi kanal berdasarkan frekuensi." },
  { "en": "Singkatan FDM (Frequency Division Multiplexing)?", "id": "Frequency Division Multiplexing." },
  { "en": "Apa itu time division multiplexing (TDM)?", "id": "Membagi kanal berdasarkan waktu." },
  { "en": "Singkatan TDM (Time Division Multiplexing)?", "id": "Time Division Multiplexing." },
  { "en": "Apa itu kuantisasi skalar?", "id": "Mengkuantisasi setiap sampel secara independen." },
  { "en": "Apa itu kuantisasi vektor?", "id": "Mengkuantisasi blok sampel secara bersamaan." },
  { "en": "Apa itu companding?", "id": "Proses kompresi-ekspansi sinyal." },
  { "en": "Tujuan companding?", "id": "Meningkatkan signal-to-noise ratio (SNR)." },
  { "en": "Singkatan SNR (Signal-to-Noise Ratio)?", "id": "Signal-to-Noise Ratio." },
  { "en": "Hukum companding yang umum?", "id": "A-law dan μ-law." },
  { "en": "Respon impuls sistem identitas?", "id": "Impuls satuan." },
  { "en": "Fungsi transfer sistem identitas?", "id": "Satu." },
  { "en": "Respon impuls sistem tunda?", "id": "Impuls satuan yang digeser." },
  { "en": "Fungsi transfer sistem tunda?", "id": "Eksponensial kompleks." },
  { "en": "Apa itu sistem non-minimum phase?", "id": "Memiliki zero di luar unit circle." },
  { "en": "Ciri sistem non-minimum phase?", "id": "Inversnya tidak stabil atau non-kausal." },
  { "en": "Dua sistem dengan magnitudo sama?", "id": "Bisa berbeda fasa." },
  { "en": "Apa itu equalizer?", "id": "Filter untuk mengkompensasi distorsi kanal." },
  { "en": "Apa itu kanal (channel)?", "id": "Medium transmisi sinyal." },
  { "en": "Contoh kanal transmisi?", "id": "Kabel tembaga, udara, serat optik." },
  { "en": "Apa itu noise aditif?", "id": "Noise yang ditambahkan pada sinyal." },
  { "en": "Model noise yang paling umum?", "id": "AWGN (Additive White Gaussian Noise)." },
  { "en": "Singkatan AWGN (Additive White Gaussian Noise)?", "id": "Additive White Gaussian Noise." },
  { "en": "Apa itu matched filter?", "id": "Filter optimal untuk deteksi sinyal." },
  { "en": "Fungsi matched filter?", "id": "Memaksimalkan SNR pada output." },
  { "en": "Respon impuls matched filter?", "id": "Versi terbalik dan tergeser sinyal." },
  { "en": "Apa itu sistem diskrit stabil marginal?", "id": "Pole simpel di atas unit circle." },
  { "en": "Apa itu sistem kontinyu stabil marginal?", "id": "Pole simpel di sumbu jω." },
  { "en": "Sistem FIR (Finite Impulse Response) selalu?", "id": "Stabil." },
  { "en": "Sistem IIR (Infinite Impulse Response) bisa?", "id": "Stabil atau tidak stabil." },
  { "en": "Apa itu cepstrum?", "id": "Transformasi Fourier dari log spektrum." },
  { "en": "Aplikasi cepstrum?", "id": "Analisis suara dan deteksi pitch." },
  { "en": "Apa itu homomorphic filtering?", "id": "Filtering pada domain cepstral." },
  { "en": "Apa itu autokovariansi?", "id": "Autokorelasi dari sinyal nol-mean." },
  { "en": "Apa itu cross-kovariansi?", "id": "Cross-korelasi dari sinyal nol-mean." },
  { "en": "Apa itu sinyal cyclostationary?", "id": "Sinyal dengan statistik periodik." },
  { "en": "Contoh sinyal cyclostationary?", "id": "Sinyal komunikasi digital termodulasi." },
  { "en": "Sinyal energi memiliki ESD (Energy Spectral Density) yang?", "id": "Terbatas." },
  { "en": "Sinyal daya memiliki PSD (Power Spectral Density) yang?", "id": "Terbatas." },
  { "en": "Daya total sinyal adalah?", "id": "Integral dari PSD-nya." },
  { "en": "Energi total sinyal adalah?", "id": "Integral dari ESD-nya." },
  { "en": "Dapatkah sinyal menjadi sinyal energi dan daya?", "id": "Tidak, kecuali sinyal nol." },
  { "en": "Apa itu filter adaptif?", "id": "Filter yang koefisiennya bisa berubah." },
  { "en": "Tujuan filter adaptif?", "id": "Beradaptasi dengan perubahan karakteristik sinyal." },
  { "en": "Struktur dasar filter adaptif?", "id": "Filter digital dan algoritma adaptif." },
  { "en": "Apa itu sinyal error adaptif?", "id": "Selisih antara sinyal output dan referensi." },
  { "en": "Algoritma adaptif paling populer?", "id": "Algoritma LMS (Least Mean Squares)." },
  { "en": "Singkatan LMS (Least Mean Squares)?", "id": "Least Mean Squares." },
  { "en": "Tujuan algoritma LMS (Least Mean Squares)?", "id": "Meminimalkan kuadrat dari sinyal error." },
  { "en": "Aplikasi utama filter adaptif?", "id": "Peredaman gema dan noise." },
  { "en": "Apa itu peredaman gema (echo cancellation)?", "id": "Menghilangkan gema pada komunikasi suara." },
  { "en": "Apa itu peredaman noise (noise cancellation)?", "id": "Menghilangkan noise yang tidak diinginkan." },
  { "en": "Apa itu identifikasi sistem?", "id": "Membuat model matematis sistem tak dikenal." },
  { "en": "Apa itu multirate signal processing?", "id": "Pemrosesan sinyal pada laju sampling berbeda." },
  { "en": "Apa itu decimator?", "id": "Sistem untuk mengurangi laju sampling." },
  { "en": "Komponen decimator?", "id": "Filter anti-aliasing dan downsampler." },
  { "en": "Apa itu interpolator?", "id": "Sistem untuk meningkatkan laju sampling." },
  { "en": "Komponen interpolator?", "id": "Upsampler dan filter anti-imaging." },
  { "en": "Fungsi upsampler?", "id": "Menyisipkan sampel nol di antara sampel." },
  { "en": "Fungsi downsampler?", "id": "Membuang sampel secara periodik." },
  { "en": "Filter anti-imaging adalah?", "id": "Filter low-pass untuk menghaluskan." },
  { "en": "Apa itu filter bank?", "id": "Kumpulan filter untuk memisahkan frekuensi." },
  { "en": "Jenis filter bank?", "id": "Analysis bank dan synthesis bank." },
  { "en": "Apa itu analysis filter bank?", "id": "Memecah sinyal menjadi beberapa sub-band." },
  { "en": "Apa itu synthesis filter bank?", "id": "Menggabungkan sinyal sub-band kembali." },
  { "en": "Singkatan QMF (Quadrature Mirror Filter)?", "id": "Quadrature Mirror Filter." },
  { "en": "Aplikasi QMF (Quadrature Mirror Filter)?", "id": "Kompresi audio dan gambar." },
  { "en": "Apa itu dekomposisi polyphase?", "id": "Teknik efisien implementasi sistem multirate." },
  { "en": "Apa itu efek kuantisasi?", "id": "Error akibat representasi digital terbatas." },
  { "en": "Apa itu error kuantisasi koefisien?", "id": "Ketidakakuratan nilai koefisien filter." },
  { "en": "Efek kuantisasi koefisien pada sistem?", "id": "Mengubah lokasi pole dan zero." },
  { "en": "Efek ini lebih signifikan pada?", "id": "Filter IIR (Infinite Impulse Response)." },
  { "en": "Apa itu limit cycle?", "id": "Osilasi kecil pada output filter IIR." },
  { "en": "Penyebab limit cycle?", "id": "Non-linearitas dari proses kuantisasi." },
  { "en": "Jenis limit cycle?", "id": "Granular dan overflow." },
  { "en": "Apa itu overflow oscillation?", "id": "Limit cycle amplitudo besar." },
  { "en": "Cara menghindari overflow?", "id": "Penskalaan sinyal atau aritmatika saturasi." },
  { "en": "Apa itu struktur filter lattice?", "id": "Struktur implementasi filter digital." },
  { "en": "Keuntungan utama struktur lattice?", "id": "Sensitivitas rendah terhadap kuantisasi." },
  { "en": "Stabilitas filter lattice ditentukan oleh?", "id": "Koefisien refleksi." },
  { "en": "Syarat stabilitas filter lattice?", "id": "Magnitudo koefisien refleksi kurang dari satu." },
  { "en": "Apa itu sinyal pita-lebar (wideband)?", "id": "Sinyal dengan bandwidth yang lebar." },
  { "en": "Apa itu sinyal pita-sempit (narrowband)?", "id": "Sinyal dengan bandwidth yang sempit." },
  { "en": "Teorema Parseval untuk DTFT (Discrete-Time Fourier Transform)?", "id": "Menghubungkan energi di domain waktu-frekuensi." },
  { "en": "Berapa persen overshoot fenomena Gibbs?", "id": "Sekitar 9 persen dari diskontinuitas." },
  { "en": "Respon impuls sistem stabil harus?", "id": "Meluruh menuju nol." },
  { "en": "Respon impuls sistem tidak stabil akan?", "id": "Tumbuh tak terbatas." },
  { "en": "Respon impuls sistem marginal stabil?", "id": "Tidak meluruh dan tidak tumbuh." },
  { "en": "Apa itu phase unwrapping?", "id": "Proses menghilangkan lompatan 2π fasa." },
  { "en": "Apa itu filter notch?", "id": "Filter yang menolak frekuensi tunggal." },
  { "en": "Apa itu filter comb?", "id": "Filter dengan respon seperti sisir." },
  { "en": "Aplikasi filter comb?", "id": "Efek audio seperti flanging." },
  { "en": "Apa itu autokorelasi sinyal periodik?", "id": "Juga periodik dengan periode sama." },
  { "en": "Apa itu resolusi frekuensi?", "id": "Kemampuan membedakan dua frekuensi berdekatan." },
  { "en": "Resolusi frekuensi DFT (Discrete Fourier Transform) bergantung pada?", "id": "Panjang jendela pengamatan (N)." },
  { "en": "Apa itu pemadanan impedansi (impedance matching)?", "id": "Menyesuaikan impedansi untuk transfer daya." },
  { "en": "Kondisi transfer daya maksimum?", "id": "Impedansi beban adalah konjugat kompleks." },
  { "en": "Apa itu decibel (dB)?", "id": "Satuan logaritmik untuk rasio daya." },
  { "en": "3 dB point menandakan?", "id": "Daya berkurang setengahnya." },
  { "en": "Apa itu oktaf (octave)?", "id": "Interval frekuensi dengan rasio dua." },
  { "en": "Apa itu dekade (decade)?", "id": "Interval frekuensi dengan rasio sepuluh." },
  { "en": "Slope pada diagram Bode diukur dalam?", "id": "dB per oktaf atau dekade." },
  { "en": "Apa itu sistem linear phase FIR (Finite Impulse Response)?", "id": "Respon impulsnya simetris." },
  { "en": "Tipe simetri FIR (Finite Impulse Response) fasa linear?", "id": "Simetri genap dan ganjil." },
  { "en": "Apa itu model AR (Autoregressive)?", "id": "Model sinyal acak." },
  { "en": "Singkatan AR (Autoregressive)?", "id": "Autoregressive." },
  { "en": "Model AR (Autoregressive) menghasilkan sinyal dari?", "id": "Outputnya sendiri dan white noise." },
  { "en": "Apa itu model MA (Moving Average)?", "id": "Model sinyal acak lain." },
  { "en": "Singkatan MA (Moving Average)?", "id": "Moving Average." },
  { "en": "Model MA (Moving Average) menghasilkan sinyal dari?", "id": "White noise masa lalu dan kini." },
  { "en": "Apa itu model ARMA (Autoregressive Moving Average)?", "id": "Kombinasi model AR dan MA." },
  { "en": "Singkatan ARMA (Autoregressive Moving Average)?", "id": "Autoregressive Moving Average." },
  { "en": "Apa itu bank osilator?", "id": "Implementasi DFT (Discrete Fourier Transform) menggunakan filter." },
  { "en": "Apa itu sinyal pita suara (voiceband)?", "id": "Rentang frekuensi suara manusia." },
  { "en": "Rentang frekuensi sinyal voiceband?", "id": "Sekitar 300 Hz hingga 3400 Hz." },
  { "en": "Sampling rate untuk sinyal telepon?", "id": "8000 sampel per detik." },
  { "en": "Sampling rate untuk audio CD?", "id": "44100 sampel per detik." },
  { "en": "Teorema sampling berlaku untuk sinyal?", "id": "Sinyal bandlimited." },
  { "en": "Transformasi Laplace dari fungsi ramp?", "id": "1 dibagi s kuadrat." },
  { "en": "Transformasi Z dari sekuens ramp?", "id": "z / (z-1) kuadrat." },
  { "en": "Apa itu feedback?", "id": "Mengumpankan sebagian output kembali ke input." },
  { "en": "Jenis feedback?", "id": "Positif dan negatif." },
  { "en": "Feedback negatif cenderung?", "id": "Menstabilkan sistem." },
  { "en": "Feedback positif cenderung?", "id": "Membuat sistem tidak stabil." },
  { "en": "Apa itu osilator?", "id": "Sistem yang menghasilkan sinyal periodik." },
  { "en": "Syarat osilasi Barkhausen?", "id": "Loop gain satu, fasa 360 derajat." },
  { "en": "Apa itu pemrosesan sinyal real-time?", "id": "Pemrosesan selesai sebelum sampel baru datang." },
  { "en": "Apa itu DSP (Digital Signal Processor)?", "id": "Mikroprosesor khusus pemrosesan sinyal." },
  { "en": "Singkatan DSP (Digital Signal Processor)?", "id": "Digital Signal Processor." },
  { "en": "Arsitektur DSP (Digital Signal Processor) yang umum?", "id": "Arsitektur Harvard." },
  { "en": "Ciri arsitektur Harvard?", "id": "Memisahkan memori program dan data." },
  { "en": "Apa itu MAC (Multiply-Accumulate) unit?", "id": "Unit hardware penting dalam DSP." },
  { "en": "Singkatan MAC (Multiply-Accumulate)?", "id": "Multiply-Accumulate." },
  { "en": "Fungsi unit MAC (Multiply-Accumulate)?", "id": "Melakukan operasi perkalian-akumulasi cepat." },
  { "en": "Apa itu circular shift?", "id": "Pergeseran periodik sekuens." },
  { "en": "Sifat pergeseran sirkular DFT (Discrete Fourier Transform)?", "id": "Perkalian dengan eksponensial kompleks." },
  { "en": "Apa itu sinyal waktu-nyata?", "id": "Sinyal yang terjadi di dunia nyata." },
  { "en": "Dapatkah sistem non-kausal ada di dunia nyata?", "id": "Tidak, untuk pemrosesan real-time." },
  { "en": "Apa itu modulasi digital?", "id": "Representasi data digital pada sinyal analog." },
  { "en": "Singkatan ASK (Amplitude Shift Keying)?", "id": "Amplitude Shift Keying." },
  { "en": "Pada ASK (Amplitude Shift Keying) data direpresentasikan oleh?", "id": "Perubahan amplitudo sinyal pembawa." },
  { "en": "Singkatan FSK (Frequency Shift Keying)?", "id": "Frequency Shift Keying." },
  { "en": "Pada FSK (Frequency Shift Keying) data direpresentasikan oleh?", "id": "Perubahan frekuensi sinyal pembawa." },
  { "en": "Singkatan PSK (Phase Shift Keying)?", "id": "Phase Shift Keying." },
  { "en": "Pada PSK (Phase Shift Keying) data direpresentasikan oleh?", "id": "Perubahan fasa sinyal pembawa." },
  { "en": "Contoh PSK (Phase Shift Keying) paling sederhana?", "id": "BPSK (Binary Phase Shift Keying)." },
  { "en": "Singkatan BPSK (Binary Phase Shift Keying)?", "id": "Binary Phase Shift Keying." },
  { "en": "Apa itu diagram konstelasi?", "id": "Visualisasi sinyal modulasi digital." },
  { "en": "Singkatan QAM (Quadrature Amplitude Modulation)?", "id": "Quadrature Amplitude Modulation." },
  { "en": "QAM (Quadrature Amplitude Modulation) memodulasi apa?", "id": "Amplitudo dan fasa secara bersamaan." },
  { "en": "Kelebihan QAM (Quadrature Amplitude Modulation)?", "id": "Efisiensi spektral yang tinggi." },
  { "en": "Apa itu bit rate?", "id": "Jumlah bit per detik." },
  { "en": "Apa itu baud rate?", "id": "Jumlah simbol per detik." },
  { "en": "Apa itu simbol dalam modulasi?", "id": "Satu keadaan sinyal representasi data." },
  { "en": "Singkatan ISI (Intersymbol Interference)?", "id": "Intersymbol Interference." },
  { "en": "Apa itu ISI (Intersymbol Interference)?", "id": "Interferensi antar simbol yang berdekatan." },
  { "en": "Penyebab ISI (Intersymbol Interference)?", "id": "Bandwidth kanal yang terbatas." },
  { "en": "Apa itu kriteria Nyquist untuk zero ISI (Intersymbol Interference)?", "id": "Syarat respon pulsa untuk nol ISI." },
  { "en": "Apa itu filter raised-cosine?", "id": "Filter praktis untuk mengurangi ISI." },
  { "en": "Apa itu diagram mata (eye diagram)?", "id": "Alat visualisasi performa sinyal digital." },
  { "en": "Bukaan mata yang lebar menandakan?", "id": "Kualitas sinyal baik, ISI rendah." },
  { "en": "Bukaan mata yang sempit menandakan?", "id": "Kualitas sinyal buruk, ISI tinggi." },
  { "en": "Apa itu kriteria stabilitas Routh-Hurwitz?", "id": "Metode stabilitas untuk sistem kontinyu." },
  { "en": "Kriteria Routh-Hurwitz menganalisis?", "id": "Koefisien polinomial karakteristik." },
  { "en": "Kondisi perlu untuk stabilitas Routh-Hurwitz?", "id": "Semua koefisien harus ada dan positif." },
  { "en": "Apa itu Jury stability test?", "id": "Metode stabilitas untuk sistem diskrit." },
  { "en": "Pole dominan adalah?", "id": "Pole yang paling mempengaruhi respon." },
  { "en": "Di mana letak pole dominan?", "id": "Paling dekat sumbu stabilitas." },
  { "en": "Pole lebih kiri di bidang-s?", "id": "Respon sistem meluruh lebih cepat." },
  { "en": "Pole lebih dekat origin bidang-z?", "id": "Respon diskrit meluruh lebih cepat." },
  { "en": "Jika ROC (Region of Convergence) kosong?", "id": "Maka transformasi tidak konvergen." },
  { "en": "Jika sistem linear tapi time-variant?", "id": "Respon impulsnya berubah terhadap waktu." },
  { "en": "Apa itu sinyal chirp linear?", "id": "Frekuensi sesaatnya berubah secara linear." },
  { "en": "Sifat pembalikan waktu Transformasi Fourier?", "id": "Spektrumnya juga mengalami pembalikan waktu." },
  { "en": "Sifat diferensiasi frekuensi Fourier?", "id": "Perkalian dengan -jt di domain waktu." },
  { "en": "Sifat integrasi frekuensi Fourier?", "id": "Tidak ada bentuk sederhana." },
  { "en": "Sinyal real dan genap memiliki spektrum?", "id": "Real dan genap." },
  { "en": "Sinyal real dan ganjil memiliki spektrum?", "id": "Imajiner murni dan ganjil." },
  { "en": "Apa itu bandwidth absolut?", "id": "Lebar pita di mana spektrum nol." },
  { "en": "Apa itu bandwidth 3dB?", "id": "Lebar pita di titik half-power." },
  { "en": "Apa itu sampling ideal?", "id": "Perkalian sinyal dengan impulse train." },
  { "en": "Spektrum sinyal tersampel ideal?", "id": "Salinan periodik dari spektrum asli." },
  { "en": "Apa itu normalisasi frekuensi?", "id": "Membagi frekuensi dengan frekuensi sampling." },
  { "en": "Apa itu orde filter?", "id": "Derajat tertinggi polinomial fungsi transfer." },
  { "en": "Orde filter lebih tinggi menghasilkan?", "id": "Roll-off yang lebih tajam." },
  { "en": "Apa itu roll-off?", "id": "Tingkat penurunan pada transition band." },
  { "en": "Apa itu sinyal pita-lebar (broadband)?", "id": "Sinonim untuk sinyal wideband." },
  { "en": "Apa itu sinyal UWB (Ultra-Wideband)?", "id": "Sinyal dengan bandwidth sangat lebar." },
  { "en": "Singkatan UWB (Ultra-Wideband)?", "id": "Ultra-Wideband." },
  { "en": "Apa itu orthogonal frequency-division multiplexing (OFDM)?", "id": "Teknik modulasi multi-carrier." },
  { "en": "Singkatan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Orthogonal Frequency-Division Multiplexing." },
  { "en": "Prinsip OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Sub-carrier saling ortogonal." },
  { "en": "Kelebihan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Tahan terhadap multipath fading." },
  { "en": "Apa itu bit error rate (BER)?", "id": "Ukuran error dalam transmisi digital." },
  { "en": "Singkatan BER (Bit Error Rate)?", "id": "Bit Error Rate." },
  { "en": "Korelasi antara dua sinyal ortogonal?", "id": "Nol." },
  { "en": "Apa itu white noise Gaussian?", "id": "White noise dengan distribusi Gaussian." },
  { "en": "Sinyal undak satuan memiliki diskontinuitas di?", "id": "t = 0." },
  { "en": "Energi total dari impuls satuan?", "id": "Tak terdefinisi (tak hingga)." },
  { "en": "Daya total dari impuls satuan?", "id": "Tak terdefinisi." },
  { "en": "Apa itu sistem lossless?", "id": "Sistem yang tidak menghilangkan energi." },
  { "en": "Apa itu sistem lossy?", "id": "Sistem yang menghilangkan (disipasi) energi." },
  { "en": "Resistor adalah contoh sistem?", "id": "Lossy." },
  { "en": "Kapasitor dan induktor ideal adalah?", "id": "Sistem lossless." },
  { "en": "Apa itu impedansi karakteristik?", "id": "Impedansi saluran transmisi tak hingga." },
  { "en": "Apa itu standing wave ratio (SWR)?", "id": "Ukuran ketidakcocokan impedansi." },
  { "en": "Singkatan SWR (Standing Wave Ratio)?", "id": "Standing Wave Ratio." },
  { "en": "Nilai SWR (Standing Wave Ratio) ideal?", "id": "Satu." },
  { "en": "Transformasi Fourier waktu-diskrit (DTFT) dari sekuens terbatas?", "id": "Polinomial dalam e pangkat -jω." },
  { "en": "Apa itu linear prediction?", "id": "Memprediksi sampel sinyal dari sampel sebelumnya." },
  { "en": "Singkatan LPC (Linear Predictive Coding)?", "id": "Linear Predictive Coding." },
  { "en": "Aplikasi LPC (Linear Predictive Coding)?", "id": "Kompresi suara." },
  { "en": "Apa itu differential pulse-code modulation (DPCM)?", "id": "Teknik kuantisasi sinyal." },
  { "en": "Singkatan DPCM (Differential Pulse-Code Modulation)?", "id": "Differential Pulse-Code Modulation." },
  { "en": "Prinsip DPCM (Differential Pulse-Code Modulation)?", "id": "Mengkuantisasi selisih antar sampel." },
  { "en": "Apa itu delta modulation?", "id": "Versi sederhana dari DPCM." },
  { "en": "Singkatan PCM (Pulse-Code Modulation)?", "id": "Pulse-Code Modulation." },
  { "en": "PCM (Pulse-Code Modulation) adalah dasar dari?", "id": "Audio digital." },
  { "en": "Apa itu aliasing spasial?", "id": "Aliasing pada sinyal gambar (spasial)." },
  { "en": "Contoh aliasing spasial?", "id": "Pola moiré." },
  { "en": "Transformasi Fourier 2D digunakan untuk?", "id": "Analisis frekuensi gambar." },
  { "en": "Sinyal deterministik memiliki PSD (Power Spectral Density)?", "id": "Tidak, hanya sinyal daya acak." },
  { "en": "Respons sistem terhadap sinusoidal?", "id": "Sinusoidal dengan amplitudo dan fasa baru." },
  { "en": "Gain sistem adalah?", "id": "Magnitudo dari respon frekuensi." },
  { "en": "Pergeseran fasa sistem adalah?", "id": "Sudut dari respon frekuensi." },
  { "en": "Jika input LTI (Linear Time-Invariant) periodik?", "id": "Outputnya juga periodik." },
  { "en": "Apa itu sistem eigenfunction?", "id": "Sinyal yang bentuknya tidak berubah." },
  { "en": "Eigenfunction dari sistem LTI (Linear Time-Invariant)?", "id": "Eksponensial kompleks." },
  { "en": "Eigenvalue dari sistem LTI (Linear Time-Invariant)?", "id": "Respon frekuensi sistem." },
  { "en": "Apa itu intermodulation distortion?", "id": "Munculnya frekuensi baru yang tidak diinginkan." },
  { "en": "Penyebab intermodulation distortion?", "id": "Non-linearitas sistem." },
  { "en": "Apa itu harmonic distortion?", "id": "Munculnya frekuensi harmonik." },
  { "en": "Penyebab harmonic distortion?", "id": "Juga karena non-linearitas sistem." },
  { "en": "Apa itu zero-input response?", "id": "Respon sistem tanpa adanya input." },
  { "en": "Zero-input response disebabkan oleh?", "id": "Kondisi awal sistem (initial state)." },
  { "en": "Apa itu zero-state response?", "id": "Respon sistem dengan kondisi awal nol." },
  { "en": "Zero-state response disebabkan oleh?", "id": "Hanya oleh sinyal input." },
  { "en": "Respon total sistem adalah jumlahan?", "id": "Zero-input dan zero-state response." },
  { "en": "Formula Euler menghubungkan eksponensial kompleks dengan?", "id": "Fungsi sinus dan kosinus." },
  { "en": "Sistem LTI (Linear Time-Invariant) dikarakterisasi penuh oleh?", "id": "Respon impulsnya." },
  { "en": "Mengapa aliasing terjadi secara spektral?", "id": "Karena replika spektrum saling tumpang tindih." },
  { "en": "Mengapa fasa linear penting dalam audio?", "id": "Menjaga bentuk gelombang suara." },
  { "en": "Mengapa ROC (Region of Convergence) penting dalam Z-Transform?", "id": "Menentukan keunikan dan sifat sekuens." },
  { "en": "Dapatkah dua sekuens berbeda memiliki Z-Transform sama?", "id": "Ya, tetapi dengan ROC berbeda." },
  { "en": "Apa itu filter Kalman?", "id": "Estimator state rekursif optimal." },
  { "en": "Tujuan utama filter Kalman?", "id": "Mengestimasi state dari pengukuran ber-noise." },
  { "en": "Apa itu proses inovasi?", "id": "Selisih antara pengukuran dan prediksi." },
  { "en": "Filter Kalman cocok untuk sistem?", "id": "Sistem linear dengan noise Gaussian." },
  { "en": "Apa itu Extended Kalman Filter (EKF)?", "id": "Versi non-linear dari filter Kalman." },
  { "en": "Singkatan EKF (Extended Kalman Filter)?", "id": "Extended Kalman Filter." },
  { "en": "Apa itu metode overlap-add?", "id": "Metode konvolusi panjang menggunakan FFT." },
  { "en": "Apa itu metode overlap-save?", "id": "Metode konvolusi panjang lain via FFT." },
  { "en": "Kedua metode tersebut digunakan untuk?", "id": "Memproses sinyal yang sangat panjang." },
  { "en": "Karakteristik utama window Hamming?", "id": "Mengurangi sidelobe pertama secara signifikan." },
  { "en": "Karakteristik utama window Blackman?", "id": "Penekanan sidelobe sangat baik." },
  { "en": "Karakteristik utama window rektangular?", "id": "Mainlobe paling sempit, sidelobe tinggi." },
  { "en": "Apa itu Carson's rule?", "id": "Mengestimasi bandwidth sinyal FM." },
  { "en": "Apa itu filter Bessel?", "id": "Filter dengan fasa linear terbaik." },
  { "en": "Kelemahan filter Bessel?", "id": "Roll-off yang landai." },
  { "en": "Aplikasi filter Bessel?", "id": "Sistem audio dan video." },
  { "en": "Apa itu filter Linkwitz-Riley?", "id": "Filter yang populer di audio crossover." },
  { "en": "Apa itu audio crossover?", "id": "Memisahkan frekuensi untuk speaker berbeda." },
  { "en": "Keluaran dari matched filter adalah?", "id": "Autokorelasi dari sinyal input." },
  { "en": "Apa itu koherensi (coherence)?", "id": "Ukuran korelasi frekuensi dua sinyal." },
  { "en": "Nilai koherensi berkisar antara?", "id": "Nol dan satu." },
  { "en": "Koherensi satu berarti?", "id": "Kedua sinyal berhubungan linear sempurna." },
  { "en": "Apa itu time-bandwidth product?", "id": "Ukuran hubungan durasi dan bandwidth." },
  { "en": "Prinsip ketidakpastian menyatakan?", "id": "Sinyal tidak bisa sempit di kedua domain." },
  { "en": "Sinyal apa yang mencapai batas ketidakpastian?", "id": "Sinyal Gaussian." },
  { "en": "Apa itu sinyal analitik?", "id": "Sinyal bernilai kompleks tanpa frekuensi negatif." },
  { "en": "Cara membuat sinyal analitik?", "id": "Menjumlahkan sinyal asli dengan Hilbert transform-nya." },
  { "en": "Apa itu instantaneous amplitude?", "id": "Magnitudo dari sinyal analitik." },
  { "en": "Instantaneous amplitude disebut juga?", "id": "Amplop (envelope) sinyal." },
  { "en": "Apa itu instantaneous phase?", "id": "Sudut dari sinyal analitik." },
  { "en": "Apa itu instantaneous frequency?", "id": "Turunan dari instantaneous phase." },
  { "en": "Dapatkah instantaneous frequency bernilai negatif?", "id": "Ya, bisa." },
  { "en": "Apa itu sistem pasif?", "id": "Sistem yang tidak menghasilkan energi." },
  { "en": "Contoh komponen pasif?", "id": "Resistor, kapasitor, induktor." },
  { "en": "Apa itu sistem aktif?", "id": "Sistem yang dapat menghasilkan energi." },
  { "en": "Contoh komponen aktif?", "id": "Op-amp, transistor." },
  { "en": "Apa itu decimation-in-time?", "id": "Salah satu jenis algoritma FFT." },
  { "en": "Apa itu decimation-in-frequency?", "id": "Jenis algoritma FFT yang lain." },
  { "en": "Apa itu butterfly operation?", "id": "Operasi dasar dalam algoritma FFT." },
  { "en": "Mengapa disebut butterfly?", "id": "Karena bentuk diagram alir datanya." },
  { "en": "Apa itu bit-reversal?", "id": "Pengurutan ulang input/output FFT." },
  { "en": "Apa itu twiddle factor?", "id": "Koefisien eksponensial kompleks dalam FFT." },
  { "en": "Apa itu sinyal chirp Z-transform?", "id": "Generalisasi dari DFT." },
  { "en": "Apa itu grup?", "id": "Struktur aljabar dalam matematika." },
  { "en": "Apa itu ring?", "id": "Struktur aljabar lain." },
  { "en": "Apa itu field?", "id": "Struktur aljabar seperti bilangan real." },
  { "en": "Operasi sinyal dapat dilihat sebagai?", "id": "Operasi dalam ruang vektor." },
  { "en": "Apa itu basis ortonormal?", "id": "Set fungsi basis ortogonal ternormalisasi." },
  { "en": "Deret Fourier menggunakan basis?", "id": "Basis sinusoidal ortonormal." },
  { "en": "Apa itu proyeksi sinyal?", "id": "Memproyeksikan satu sinyal ke sinyal lain." },
  { "en": "Koefisien Fourier dapat diartikan sebagai?", "id": "Proyeksi sinyal ke fungsi basis." },
  { "en": "Apa itu Gram-Schmidt orthogonalization?", "id": "Proses membuat set basis ortogonal." },
  { "en": "Apa itu FIR (Finite Impulse Response) equiripple filter?", "id": "Filter FIR optimal." },
  { "en": "Algoritma untuk mendesain filter equiripple?", "id": "Algoritma Parks-McClellan." },
  { "en": "Apa itu sinyal pita-lewat (passband)?", "id": "Sinonim untuk sinyal passband." },
  { "en": "Apa itu downconversion?", "id": "Mengubah sinyal passband ke baseband." },
  { "en": "Apa itu upconversion?", "id": "Mengubah sinyal baseband ke passband." },
  { "en": "Downconversion dilakukan dengan?", "id": "Mengalikan dengan sinusoidal dan filtering." },
  { "en": "Apa itu quadrature demodulation?", "id": "Demodulasi menggunakan komponen I dan Q." },
  { "en": "Apa itu komponen I (in-phase)?", "id": "Komponen yang sefasa dengan carrier." },
  { "en": "Apa itu komponen Q (quadrature)?", "id": "Komponen yang beda fasa 90 derajat." },
  { "en": "Representasi sinyal passband menggunakan?", "id": "Komponen in-phase dan quadrature." },
  { "en": "Apa itu dithering?", "id": "Menambahkan noise untuk mengurangi error kuantisasi." },
  { "en": "Tujuan dithering?", "id": "Mengacak pola error kuantisasi." },
  { "en": "Apa itu noise shaping?", "id": "Memindahkan noise kuantisasi ke frekuensi lain." },
  { "en": "Noise shaping digunakan dalam?", "id": "ADC dan DAC modern." },
  { "en": "Apa itu delta-sigma modulation?", "id": "Teknik ADC yang menggunakan noise shaping." },
  { "en": "Kelebihan delta-sigma ADC (Analog-to-Digital Converter)?", "id": "Resolusi tinggi dengan komponen sederhana." },
  { "en": "Apa itu oversampling?", "id": "Sampling jauh di atas laju Nyquist." },
  { "en": "Keuntungan oversampling?", "id": "Meningkatkan SNR dan menyederhanakan filter." },
  { "en": "Apa itu sinyal diskrit kompleks?", "id": "Sekuens dengan nilai bilangan kompleks." },
  { "en": "Spektrum sinyal diskrit real?", "id": "Memiliki simetri konjugat." },
  { "en": "Spektrum sinyal diskrit kompleks?", "id": "Tidak harus memiliki simetri." },
  { "en": "Apa itu periodogram?", "id": "Estimasi PSD dari suatu sinyal." },
  { "en": "Kelemahan periodogram?", "id": "Varians estimasi yang tinggi." },
  { "en": "Metode untuk mengurangi varians periodogram?", "id": "Averaging (metode Welch) atau smoothing." },
  { "en": "Apa itu cyclic prefix dalam OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Salinan akhir simbol di bagian awal." },
  { "en": "Tujuan cyclic prefix?", "id": "Mengatasi ISI dan menjaga ortogonalitas." },
  { "en": "Apa itu sistem LPTV (Linear Periodically Time-Varying)?", "id": "Sistem linear yang berubah periodik." },
  { "en": "Singkatan LPTV (Linear Periodically Time-Varying)?", "id": "Linear Periodically Time-Varying." },
  { "en": "Contoh sistem LPTV (Linear Periodically Time-Varying)?", "id": "Mixer atau modulator." },
  { "en": "Apakah sampling proses LTI (Linear Time-Invariant)?", "id": "Bukan, itu adalah proses LPTV." },
  { "en": "Apa itu warping?", "id": "Transformasi non-linear pada sumbu." },
  { "en": "Contoh warping?", "id": "Frequency warping pada bilinear transform." },
  { "en": "Prinsip superposisi adalah dasar dari?", "id": "Sifat linearitas sistem." },
  { "en": "Fungsi sinc adalah transformasi Fourier dari?", "id": "Sinyal pulsa kotak (rectangular)." },
  { "en": "Deretan impuls adalah transformasi Fourier dari?", "id": "Sinyal periodik." },
  { "en": "Konvolusi di domain frekuensi adalah hasil dari?", "id": "Perkalian di domain waktu." },
  { "en": "Sistem yang respon impulsnya non-nol untuk t<0?", "id": "Sistem non-kausal." },
  { "en": "Apa itu orde-nol hold (ZOH)?", "id": "Metode praktis rekonstruksi sinyal." },
  { "en": "Singkatan ZOH (Zero-Order Hold)?", "id": "Zero-Order Hold." },
  { "en": "ZOH (Zero-Order Hold) membuat sinyal terlihat?", "id": "Seperti tangga (staircase)." },
  { "en": "Respon impuls ZOH (Zero-Order Hold)?", "id": "Pulsa kotak." },
  { "en": "Apa itu orde-pertama hold (FOH)?", "id": "Rekonstruksi dengan interpolasi linear." },
  { "en": "Singkatan FOH (First-Order Hold)?", "id": "First-Order Hold." },
  { "en": "Bentuk rektangular bilangan kompleks?", "id": "a + jb." },
  { "en": "Bentuk polar bilangan kompleks?", "id": "r dikali e pangkat jθ." },
  { "en": "Magnitudo dari bilangan kompleks a+jb?", "id": "Akar dari (a² + b²)." },
  { "en": "Fasa dari bilangan kompleks a+jb?", "id": "Arctan dari (b/a)." },
  { "en": "Apa itu eigenvalue dari matriks state A?", "id": "Pole dari sistem." },
  { "en": "Apa itu konjugat kompleks?", "id": "Membalik tanda bagian imajiner." },
  { "en": "Pole dan zero sistem real selalu?", "id": "Berpasangan konjugat kompleks." },
  { "en": "Headphone noise-cancelling menggunakan prinsip?", "id": "Superposisi dan interferensi destruktif." },
  { "en": "Audio equalizer pada dasarnya adalah?", "id": "Bank filter band-pass paralel." },
  { "en": "Kompresi JPEG menggunakan transformasi?", "id": "DCT (Discrete Cosine Transform)." },
  { "en": "Singkatan DCT (Discrete Cosine Transform)?", "id": "Discrete Cosine Transform." },
  { "en": "DCT (Discrete Cosine Transform) adalah varian dari?", "id": "DFT untuk sinyal real." },
  { "en": "Kelebihan DCT (Discrete Cosine Transform) untuk kompresi?", "id": "Kompaksi energi yang sangat baik." },
  { "en": "Apa itu kompresi lossy?", "id": "Kompresi yang membuang sebagian informasi." },
  { "en": "Apa itu kompresi lossless?", "id": "Kompresi tanpa kehilangan informasi." },
  { "en": "FFT (Fast Fourier Transform) radix-2 bekerja pada sekuens?", "id": "Yang panjangnya pangkat dari dua." },
  { "en": "Apa itu sistem all-zero?", "id": "Sistem FIR, hanya memiliki zero." },
  { "en": "Apa itu sistem all-pole?", "id": "Sistem IIR, hanya memiliki pole." },
  { "en": "Apa itu normalisasi?", "id": "Mengubah skala sinyal ke rentang tertentu." },
  { "en": "Rentang normalisasi yang umum?", "id": "Antara -1 dan 1." },
  { "en": "Apa itu moving average filter?", "id": "Filter FIR sederhana untuk smoothing." },
  { "en": "Koefisien moving average filter?", "id": "Semua bernilai sama (konstan)." },
  { "en": "Apa itu windowing dalam analisis spektral?", "id": "Mengalikan sinyal dengan fungsi window." },
  { "en": "Tujuan windowing?", "id": "Mengurangi spectral leakage." },
  { "en": "Apa itu main lobe?", "id": "Puncak utama pada respon frekuensi window." },
  { "en": "Apa itu side lobe?", "id": "Puncak-puncak kecil di samping main lobe." },
  { "en": "Window yang baik memiliki side lobe?", "id": "Rendah." },
  { "en": "Apa itu sinyal pita-terbatas (band-limited)?", "id": "Spektrum nol di luar frekuensi tertentu." },
  { "en": "Dapatkah sinyal terbatas-waktu menjadi pita-terbatas?", "id": "Tidak, secara teoritis tidak mungkin." },
  { "en": "Sinyal Gaussian di domain waktu memiliki bentuk?", "id": "Gaussian juga di domain frekuensi." },
  { "en": "Apa itu sinyal ergodik?", "id": "Proses acak yang properti ansambelnya." },
  { "en": "Sinyal deterministik apakah ergodik?", "id": "Ya, secara trivial." },
  { "en": "Apa itu cross-spectral density?", "id": "Transformasi Fourier dari cross-korelasi." },
  { "en": "Apa itu settling time?", "id": "Waktu sistem mencapai kondisi tunak." },
  { "en": "Apa itu rise time?", "id": "Waktu respon naik dari 10% ke 90%." },
  { "en": "Apa itu overshoot?", "id": "Respon sistem melebihi nilai akhirnya." },
  { "en": "Overshoot sering terjadi pada sistem?", "id": "Sistem orde dua underdamped." },
  { "en": "Apa itu faktor redaman (damping factor)?", "id": "Parameter yang menentukan osilasi respon." },
  { "en": "Faktor redaman nol berarti?", "id": "Osilasi tak teredam." },
  { "en": "Faktor redaman satu berarti?", "id": "Respon critically damped." },
  { "en": "Faktor redaman lebih dari satu?", "id": "Respon overdamped (lambat)." },
  { "en": "Apa itu frekuensi natural tak teredam?", "id": "Frekuensi osilasi tanpa redaman." },
  { "en": "Apa itu sinyal polifase?", "id": "Representasi sinyal dalam beberapa komponen." },
  { "en": "Sinyal polifase digunakan dalam?", "id": "Implementasi efisien bank filter." },
  { "en": "Apa itu sinyal chirp?", "id": "Sinusoida yang frekuensinya berubah-ubah." },
  { "en": "Apa itu 'pumping' dalam audio?", "id": "Efek samping kompresor audio dinamis." },
  { "en": "Apa itu 'aliasing' temporal?", "id": "Aliasing dalam domain waktu." },
  { "en": "Contoh aliasing temporal?", "id": "Roda tampak berputar mundur di film." },
  { "en": "Apa itu filter IIR (Infinite Impulse Response) notch?", "id": "Memiliki zero di atas unit circle." },
  { "en": "Apa itu FIR (Finite Impulse Response) comb filter?", "id": "Memiliki zero yang tersebar merata." },
  { "en": "Respons fasa dari sistem kausal LTI (Linear Time-Invariant)?", "id": "Ditentukan oleh Teorema Paley-Wiener." },
  { "en": "Apa itu bank filter uniform DFT (Discrete Fourier Transform)?", "id": "Bank filter dengan spasi frekuensi sama." },
  { "en": "Apa itu 'carrier frequency' dalam modulasi?", "id": "Frekuensi sinyal pembawa." },
  { "en": "Apa itu 'envelope detector'?", "id": "Sirkuit untuk demodulasi AM." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sirkuit untuk demodulasi FM." },
  { "en": "Singkatan PLL (Phase-Locked Loop)?", "id": "Phase-Locked Loop." },
  { "en": "ROC (Region of Convergence) dari sekuens kausal?", "id": "Bagian luar dari sebuah lingkaran." },
  { "en": "ROC (Region of Convergence) dari sekuens anti-kausal?", "id": "Bagian dalam dari sebuah lingkaran." },
  { "en": "ROC (Region of Convergence) dari sistem stabil?", "id": "Harus mencakup sumbu stabilitas." },
  { "en": "Apa itu 'kernel' dalam transformasi integral?", "id": "Fungsi basis dari transformasi." },
  { "en": "Kernel dari Transformasi Fourier?", "id": "Fungsi eksponensial kompleks." },
  { "en": "Kernel dari Transformasi Laplace?", "id": "Fungsi eksponensial kompleks teredam." },
  { "en": "Apa itu 'white noise'?", "id": "Sinyal acak dengan spektrum daya flat." },
  { "en": "Apa itu 'pink noise'?", "id": "Spektrum dayanya menurun 3 dB per oktaf." },
  { "en": "Apa itu 'brown noise'?", "id": "Spektrum dayanya menurun 6 dB per oktaf." },
  { "en": "Sifat time-reversal pada Z-Transform?", "id": "z digantikan dengan 1/z." },
  { "en": "Transformasi-Z dari u[n]?", "id": "z / (z-1)." },
  { "en": "Transformasi-Z dari a^n u[n]?", "id": "z / (z-a)." },
  { "en": "ROC (Region of Convergence) dari a^n u[n]?", "id": "|z| > |a|." },
  { "en": "Apa itu 'padding' dalam konvolusi?", "id": "Menambahkan nol untuk menghindari wrap-around." },
  { "en": "Apa itu 'circular convolution'?", "id": "Konvolusi yang hasilnya periodik." },
  { "en": "Perkalian dalam domain DFT (Discrete Fourier Transform) menghasilkan?", "id": "Konvolusi sirkular." },
  { "en": "Syarat konvolusi linear via DFT (Discrete Fourier Transform)?", "id": "Zero-padding yang cukup pada kedua sinyal." },
  { "en": "Panjang hasil konvolusi linear N1 dan N2?", "id": "N1 + N2 - 1." },
  { "en": "Apa itu 'latency' sistem?", "id": "Total waktu tunda dari input ke output." },
  { "en": "Apa itu 'throughput' sistem?", "id": "Laju data yang bisa diproses." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi waktu tunda sinyal." },
  { "en": "Apa itu 'sampling clock'?", "id": "Sinyal periodik pemicu proses sampling." },
  { "en": "Apa itu 'aperture jitter'?", "id": "Ketidakpastian pada timing sampling clock." },
  { "en": "Efek 'aperture jitter'?", "id": "Menimbulkan noise pada sinyal tersampel." },
  { "en": "Apa itu 'quantization noise'?", "id": "Error yang diperkenalkan oleh kuantisasi." },
  { "en": "Model 'quantization noise' seringkali?", "id": "Dianggap sebagai white noise uniform." },
  { "en": "Daya 'quantization noise' bergantung pada?", "id": "Jumlah bit kuantisasi." },
  { "en": "Setiap penambahan satu bit mengurangi noise sebesar?", "id": "Sekitar 6 dB." },
  { "en": "Apa itu sistem waktu-nyata (real-time)?", "id": "Sistem dengan batasan waktu komputasi." },
  { "en": "Apa itu hard real-time?", "id": "Deadline yang tidak boleh terlewat sama sekali." },
  { "en": "Apa itu soft real-time?", "id": "Kelewatan deadline dapat ditoleransi." },
  { "en": "Contoh sistem hard real-time?", "id": "Sistem kontrol airbag mobil." },
  { "en": "Contoh sistem soft real-time?", "id": "Streaming video online." },
  { "en": "Apa itu sinyal multidimensi?", "id": "Sinyal dengan lebih dari satu variabel independen." },
  { "en": "Contoh sinyal dua dimensi (2D)?", "id": "Gambar digital." },
  { "en": "Variabel independen sinyal gambar?", "id": "Koordinat spasial (x, y)." },
  { "en": "Contoh sinyal tiga dimensi (3D)?", "id": "Data video atau citra medis." },
  { "en": "Transformasi Fourier untuk sinyal 2D?", "id": "Transformasi Fourier 2D." },
  { "en": "Apa itu frekuensi spasial?", "id": "Ukuran perubahan intensitas per jarak." },
  { "en": "Frekuensi spasial rendah merepresentasikan?", "id": "Area gambar yang halus." },
  { "en": "Frekuensi spasial tinggi merepresentasikan?", "id": "Tepi (edge) dan detail gambar." },
  { "en": "Konvolusi 2D digunakan untuk?", "id": "Operasi filtering pada gambar." },
  { "en": "Apa itu kernel konvolusi gambar?", "id": "Matriks kecil yang digunakan dalam filtering." },
  { "en": "Apa itu median filter?", "id": "Filter non-linear untuk menghilangkan noise." },
  { "en": "Kelebihan median filter?", "id": "Menghilangkan noise salt-and-pepper dengan baik." },
  { "en": "Apa itu 'picket-fence effect'?", "id": "Efek 'melihat' spektrum lewat celah." },
  { "en": "Penyebab 'picket-fence effect' pada DFT (Discrete Fourier Transform)?", "id": "Karena DFT hanya mensampel DTFT." },
  { "en": "Bagaimana mengurangi 'picket-fence effect'?", "id": "Dengan zero-padding." },
  { "en": "Apa itu 'leakage' dalam analisis spektral?", "id": "Sinonim untuk spectral leakage." },
  { "en": "Apa itu 'sidelobe' pada spektrum?", "id": "Puncak-puncak minor di spektrum." },
  { "en": "Apa itu 'mainlobe' pada spektrum?", "id": "Puncak utama energi di spektrum." },
  { "en": "Tujuan utama desain window?", "id": "Menekan level sidelobe." },
  { "en": "Apa itu 'scalloping loss'?", "id": "Kesalahan amplitudo akibat frekuensi sinyal." },
  { "en": "Scalloping loss terjadi saat frekuensi?", "id": "Di antara titik-titik sampel DFT." },
  { "en": "Apa itu 'orthogonal projection'?", "id": "Proyeksi ke subruang ortogonal." },
  { "en": "Contoh penggunaan 'orthogonal projection'?", "id": "Aproksimasi sinyal dengan basis." },
  { "en": "Apa itu 'least squares error'?", "id": "Error kuadrat terkecil." },
  { "en": "Metode 'least squares' digunakan untuk?", "id": "Aproksimasi atau desain filter." },
  { "en": "Apa itu 'condition number' matriks?", "id": "Ukuran sensitivitas matriks terhadap error." },
  { "en": "Condition number tinggi berarti?", "id": "Sistem numerik tidak stabil." },
  { "en": "Apa itu 'singular value decomposition' (SVD)?", "id": "Dekomposisi matriks yang kuat." },
  { "en": "Singkatan SVD (Singular Value Decomposition)?", "id": "Singular Value Decomposition." },
  { "en": "Apa itu 'pseudoinverse'?", "id": "Generalisasi dari invers matriks." },
  { "en": "Apa itu 'down-conversion'?", "id": "Menggeser spektrum sinyal ke frekuensi rendah." },
  { "en": "Apa itu 'up-conversion'?", "id": "Menggeser spektrum sinyal ke frekuensi tinggi." },
  { "en": "Proses 'down-conversion' juga disebut?", "id": "Heterodyning." },
  { "en": "Apa itu 'image frequency' dalam superheterodyne?", "id": "Frekuensi yang tidak diinginkan." },
  { "en": "Bagaimana menghilangkan 'image frequency'?", "id": "Menggunakan filter RF di awal." },
  { "en": "Apa itu 'sampling rate conversion'?", "id": "Mengubah laju sampling sinyal." },
  { "en": "Konversi dengan faktor non-integer?", "id": "Menggunakan interpolasi diikuti desimasi." },
  { "en": "Apa itu 'aliasing' domain waktu?", "id": "Overlap pada sinyal waktu diskrit." },
  { "en": "Penyebab 'aliasing' domain waktu?", "id": "Downsampling tanpa filtering yang cukup." },
  { "en": "Apa itu 'noble identities'?", "id": "Identitas untuk menyederhanakan sistem multirate." },
  { "en": "Apa itu 'Nyquist filter'?", "id": "Filter yang memenuhi kriteria Nyquist ISI." },
  { "en": "Contoh 'Nyquist filter'?", "id": "Filter Sinc dan Raised Cosine." },
  { "en": "Apa itu 'roll-off factor'?", "id": "Parameter pada filter raised-cosine." },
  { "en": "Roll-off factor nol berarti?", "id": "Filter sinc ideal." },
  { "en": "Roll-off factor satu berarti?", "id": "Bandwidth dua kali minimum." },
  { "en": "Apa itu 'carrier recovery'?", "id": "Proses mengekstrak frekuensi pembawa." },
  { "en": "Apa itu 'timing recovery'?", "id": "Proses menyinkronkan clock sampling." },
  { "en": "Apa itu 'phase ambiguity' pada PSK (Phase Shift Keying)?", "id": "Ketidakpastian fasa referensi." },
  { "en": "Cara mengatasi 'phase ambiguity'?", "id": "Menggunakan differential encoding (DPSK)." },
  { "en": "Singkatan DPSK (Differential Phase Shift Keying)?", "id": "Differential Phase Shift Keying." },
  { "en": "Apa itu 'trellis coding'?", "id": "Teknik modulasi dan coding gabungan." },
  { "en": "Apa itu 'Viterbi algorithm'?", "id": "Algoritma untuk decoding konvolusional." },
  { "en": "Apa itu 'channel capacity'?", "id": "Laju data maksimum tanpa error." },
  { "en": "Teorema yang mendefinisikan 'channel capacity'?", "id": "Teorema Shannon-Hartley." },
  { "en": "Kapasitas kanal bergantung pada?", "id": "Bandwidth dan Signal-to-Noise Ratio (SNR)." },
  { "en": "Apa itu 'spectral efficiency'?", "id": "Ukuran efisiensi penggunaan bandwidth." },
  { "en": "Satuan 'spectral efficiency'?", "id": "Bit per detik per Hertz." },
  { "en": "Apa itu 'Gaussian process'?", "id": "Proses stokastik dengan distribusi Gaussian." },
  { "en": "Apa itu 'Markov process'?", "id": "Proses stokastik tanpa memori." },
  { "en": "Sifat 'Markov process'?", "id": "Masa depan hanya bergantung masa kini." },
  { "en": "Apa itu 'Poisson process'?", "id": "Model untuk kedatangan acak." },
  { "en": "Apa itu 'convolutional code'?", "id": "Jenis kode koreksi error." },
  { "en": "Apa itu 'block code'?", "id": "Jenis kode koreksi error lain." },
  { "en": "Apa itu 'Hamming distance'?", "id": "Jumlah posisi bit yang berbeda." },
  { "en": "Apa itu 'parity bit'?", "id": "Bit tambahan untuk deteksi error." },
  { "en": "Apa itu 'Cyclic Redundancy Check' (CRC)?", "id": "Metode deteksi error yang kuat." },
  { "en": "Singkatan CRC (Cyclic Redundancy Check)?", "id": "Cyclic Redundancy Check." },
  { "en": "Apa itu 'finite field' (Galois Field)?", "id": "Struktur aljabar dalam coding theory." },
  { "en": "Apa itu 'Fourier optics'?", "id": "Aplikasi analisis Fourier pada optik." },
  { "en": "Apa itu 'diffraction'?", "id": "Penyebaran gelombang saat melewati celah." },
  { "en": "Apa itu 'interference'?", "id": "Superposisi dari dua atau lebih gelombang." },
  { "en": "Apa itu 'standing wave'?", "id": "Gelombang yang tampak diam." },
  { "en": "Terbentuknya 'standing wave'?", "id": "Interferensi gelombang dan pantulannya." },
  { "en": "Apa itu 'Doppler effect'?", "id": "Perubahan frekuensi akibat gerakan relatif." },
  { "en": "Apa itu 'redshift'?", "id": "Efek Doppler untuk sumber menjauh." },
  { "en": "Apa itu 'blueshift'?", "id": "Efek Doppler untuk sumber mendekat." },
  { "en": "Apa itu 'group velocity'?", "id": "Kecepatan propagasi amplop gelombang." },
  { "en": "Apa itu 'phase velocity'?", "id": "Kecepatan propagasi fasa gelombang." },
  { "en": "Kapan 'group velocity' dan 'phase velocity' berbeda?", "id": "Pada medium dispersif." },
  { "en": "Apa itu 'dispersion'?", "id": "Penyebaran gelombang akibat kecepatan fasa." },
  { "en": "Apa itu 'anechoic chamber'?", "id": "Ruangan yang dirancang menyerap suara." },
  { "en": "Apa itu 'reverberation'?", "id": "Gema yang bertahan setelah sumber suara berhenti." },
  { "en": "Apa itu 'impulse response' akustik?", "id": "Karakteristik gema suatu ruangan." },
  { "en": "Apa itu 'convolution reverb'?", "id": "Efek gema buatan menggunakan konvolusi." },
  { "en": "Apa itu 'autocorrelation matrix'?", "id": "Matriks yang elemennya nilai autokorelasi." },
  { "en": "Apa itu 'Toeplitz matrix'?", "id": "Matriks dengan diagonal konstan." },
  { "en": "Matriks autokorelasi sinyal stasioner adalah?", "id": "Matriks Toeplitz." },
  { "en": "Apa itu 'eigen-decomposition'?", "id": "Dekomposisi matriks menjadi eigenvector-eigenvalue." },
  { "en": "Aplikasi 'eigen-decomposition' dalam sinyal?", "id": "Principal Component Analysis (PCA)." },
  { "en": "Singkatan PCA (Principal Component Analysis)?", "id": "Principal Component Analysis." },
  { "en": "Tujuan PCA (Principal Component Analysis)?", "id": "Reduksi dimensi data." },
  { "en": "Apa itu 'Fourier slice theorem'?", "id": "Menghubungkan proyeksi dengan Transformasi Fourier." },
  { "en": "Aplikasi 'Fourier slice theorem'?", "id": "Tomografi terkomputasi (CT scans)." },
  { "en": "Singkatan CT (Computed Tomography)?", "id": "Computed Tomography." },
  { "en": "Apa itu 'back-projection'?", "id": "Metode rekonstruksi gambar dari proyeksi." },
  { "en": "Apa itu 'Radon transform'?", "id": "Transformasi matematis dasar CT scan." },
  { "en": "Apa itu 'phantom' dalam pencitraan?", "id": "Objek standar untuk menguji pemindai." },
  { "en": "Apa itu 'beamforming'?", "id": "Teknik memfokuskan sinyal antena." },
  { "en": "Aplikasi 'beamforming'?", "id": "Radar, sonar, dan komunikasi nirkabel." },
  { "en": "Apa itu 'array' antena?", "id": "Kumpulan elemen antena." },
  { "en": "Apa itu 'steering vector'?", "id": "Vektor fasa untuk mengarahkan beam." },
  { "en": "Apa itu 'null steering'?", "id": "Mengarahkan nol radiasi untuk menolak interferensi." },
  { "en": "Apa itu 'sampling oscilloscope'?", "id": "Osiloskop untuk melihat sinyal periodik berfrekuensi sangat tinggi." },
  { "en": "Apa itu 'spectrum analyzer'?", "id": "Instrumen untuk melihat spektrum sinyal." },
  { "en": "Apa itu 'vector signal analyzer' (VSA)?", "id": "Menganalisis sinyal termodulasi kompleks." },
  { "en": "Singkatan VSA (Vector Signal Analyzer)?", "id": "Vector Signal Analyzer." },
  { "en": "Apa itu 'arbitrary waveform generator' (AWG)?", "id": "Pembangkit sinyal dengan bentuk gelombang custom." },
  { "en": "Singkatan AWG (Arbitrary Waveform Generator)?", "id": "Arbitrary Waveform Generator." },
  { "en": "Apa itu 'bit synchronization'?", "id": "Menyelaraskan waktu bit pada penerima." },
  { "en": "Apa itu 'frame synchronization'?", "id": "Menemukan awal dan akhir frame data." },
  { "en": "Apa itu 'carrier synchronization'?", "id": "Menyelaraskan osilator lokal dengan carrier." },
  { "en": "Apa itu ' costas loop'?", "id": "Sirkuit untuk carrier recovery BPSK." },
  { "en": "Apa itu 'early-late gate' synchronizer?", "id": "Metode untuk timing recovery." },
  { "en": "Apa itu 'kalman gain'?", "id": "Faktor penimbang dalam filter Kalman." },
  { "en": "Apa itu 'covariance matrix'?", "id": "Matriks yang berisi kovariansi elemen." },
  { "en": "Apa itu 'time-frequency analysis'?", "id": "Analisis sinyal di kedua domain." },
  { "en": "Contoh 'time-frequency distribution'?", "id": "Spectrogram, Wigner-Ville Distribution." },
  { "en": "Apa itu 'Wigner-Ville distribution' (WVD)?", "id": "Representasi waktu-frekuensi resolusi tinggi." },
  { "en": "Singkatan WVD (Wigner-Ville Distribution)?", "id": "Wigner-Ville Distribution." },
  { "en": "Kelemahan WVD (Wigner-Ville Distribution)?", "id": "Adanya 'cross-terms' atau interferensi." },
  { "en": "Apa itu 'ambiguity function'?", "id": "Alat analisis sinyal radar." },
  { "en": "Apa itu 'chirp rate'?", "id": "Laju perubahan frekuensi sinyal chirp." },
  { "en": "Apa itu 'pulse compression'?", "id": "Teknik untuk meningkatkan resolusi radar." },
  { "en": "Apa itu 'Fourier projection-slice theorem'?", "id": "Sinonim untuk Fourier slice theorem." },
  { "en": "Apa itu 'image registration'?", "id": "Proses menyelaraskan dua atau lebih gambar." },
  { "en": "Apa itu 'image segmentation'?", "id": "Proses mempartisi gambar menjadi segmen." },
  { "en": "Apa itu 'edge detection'?", "id": "Proses menemukan batas objek di gambar." },
  { "en": "Contoh operator 'edge detection'?", "id": "Sobel, Canny, Prewitt." },
  { "en": "Apa itu 'thresholding' dalam gambar?", "id": "Mengubah gambar grayscale menjadi biner." },
  { "en": "Apa itu 'histogram' gambar?", "id": "Distribusi tingkat keabuan piksel." },
  { "en": "Apa itu 'histogram equalization'?", "id": "Metode untuk meningkatkan kontras gambar." },
  { "en": "Apa itu 'convolutional neural network' (CNN)?", "id": "Model deep learning untuk data grid." },
  { "en": "Singkatan CNN (Convolutional Neural Network)?", "id": "Convolutional Neural Network." },
  { "en": "Operasi dasar CNN (Convolutional Neural Network)?", "id": "Konvolusi." },
  { "en": "Apa itu 'pooling layer' dalam CNN (Convolutional Neural Network)?", "id": "Layer untuk downsampling (subsampling)." },
  { "en": "Apa itu 'activation function'?", "id": "Fungsi non-linear dalam jaringan saraf." },
  { "en": "Contoh 'activation function'?", "id": "ReLU, Sigmoid, Tanh." },
  { "en": "Apa itu 'backpropagation'?", "id": "Algoritma untuk melatih jaringan saraf." },
  { "en": "Apa itu 'gradient descent'?", "id": "Algoritma optimisasi untuk mencari minimum." },
  { "en": "Apa itu 'learning rate'?", "id": "Ukuran langkah dalam gradient descent." },
  { "en": "Apa itu 'overfitting'?", "id": "Model terlalu cocok dengan data training." },
  { "en": "Apa itu 'underfitting'?", "id": "Model terlalu sederhana untuk data." },
  { "en": "Apa itu 'regularization'?", "id": "Teknik untuk mencegah overfitting." },
  { "en": "Contoh 'regularization'?", "id": "L1, L2, Dropout." },
  { "en": "Apa itu 'Fourier-Bessel series'?", "id": "Deret Fourier untuk koordinat silindris." },
  { "en": "Apa itu 'spherical harmonics'?", "id": "Basis ortogonal pada permukaan bola." },
  { "en": "Aplikasi 'spherical harmonics'?", "id": "Grafik komputer dan geofisika." },
  { "en": "Apa itu 'Gibbs sampling'?", "id": "Algoritma MCMC untuk sampling." },
  { "en": "Singkatan MCMC (Markov Chain Monte Carlo)?", "id": "Markov Chain Monte Carlo." },
  { "en": "Apa itu 'Metropolis-Hastings algorithm'?", "id": "Algoritma MCMC lain." },
  { "en": "Apa itu 'hidden Markov model' (HMM)?", "id": "Model statistik Markov dengan state tersembunyi." },
  { "en": "Singkatan HMM (Hidden Markov Model)?", "id": "Hidden Markov Model." },
  { "en": "Aplikasi HMM (Hidden Markov Model)?", "id": "Pengenalan suara dan bioinformatika." },
  { "en": "Apa itu 'forward-backward algorithm'?", "id": "Algoritma untuk inferensi HMM." },
  { "en": "Apa itu 'transfer learning'?", "id": "Menggunakan model terlatih untuk tugas baru." },
  { "en": "Apa itu 'fine-tuning'?", "id": "Menyesuaikan sedikit parameter model terlatih." },
  { "en": "Apa itu 'feature extraction'?", "id": "Proses mengekstrak informasi relevan." },
  { "en": "Apa itu 'dimensionality reduction'?", "id": "Mengurangi jumlah variabel (fitur)." },
  { "en": "Apa itu 'manifold learning'?", "id": "Teknik reduksi dimensi non-linear." },
  { "en": "Apa itu 'linear discriminant analysis' (LDA)?", "id": "Teknik reduksi dimensi ter-supervisi." },
  { "en": "Singkatan LDA (Linear Discriminant Analysis)?", "id": "Linear Discriminant Analysis." },
  { "en": "Apa itu 'clustering'?", "id": "Mengelompokkan data tanpa label." },
  { "en": "Contoh algoritma 'clustering'?", "id": "K-Means, DBSCAN." },
  { "en": "Apa itu 'classification'?", "id": "Memprediksi label kategori dari data." },
  { "en": "Contoh algoritma 'classification'?", "id": "SVM, Random Forest." },
  { "en": "Singkatan SVM (Support Vector Machine)?", "id": "Support Vector Machine." },
  { "en": "Apa itu 'regression'?", "id": "Memprediksi nilai kontinyu dari data." },
  { "en": "Contoh algoritma 'regression'?", "id": "Linear Regression, Gradient Boosting." },
  { "en": "Apa itu 'cross-validation'?", "id": "Teknik untuk mengevaluasi performa model." },
  { "en": "Apa itu 'confusion matrix'?", "id": "Tabel untuk mengevaluasi performa klasifikasi." },
  { "en": "Metrik dalam 'confusion matrix'?", "id": "Akurasi, presisi, recall, F1-score." },
  { "en": "Apa itu 'ROC (Receiver Operating Characteristic) curve'?", "id": "Grafik performa klasifikasi biner." },
  { "en": "Singkatan ROC (Receiver Operating Characteristic)?", "id": "Receiver Operating Characteristic." },
  { "en": "Sumbu x pada kurva ROC (Receiver Operating Characteristic)?", "id": "False Positive Rate." },
  { "en": "Sumbu y pada kurva ROC (Receiver Operating Characteristic)?", "id": "True Positive Rate." },
  { "en": "Apa itu 'Area Under the Curve' (AUC)?", "id": "Ukuran performa klasifikasi." },
  { "en": "Singkatan AUC (Area Under the Curve)?", "id": "Area Under the Curve." },
  { "en": "Nilai AUC (Area Under the Curve) yang sempurna?", "id": "Satu." },
  { "en": "Nilai AUC (Area Under the Curve) tebakan acak?", "id": "Setengah (0.5)." },
  { "en": "Apa itu 'bias-variance tradeoff'?", "id": "Konsep fundamental dalam machine learning." },
  { "en": "Model dengan 'high bias'?", "id": "Cenderung underfitting." },
  { "en": "Model dengan 'high variance'?", "id": "Cenderung overfitting." },
  { "en": "Apa itu 'ensemble learning'?", "id": "Menggabungkan beberapa model untuk prediksi." },
  { "en": "Contoh 'ensemble learning'?", "id": "Bagging, Boosting, Stacking." },
  { "en": "Apa itu 'random forest'?", "id": "Ensemble dari decision trees menggunakan bagging." },
  { "en": "Apa itu 'gradient boosting'?", "id": "Ensemble yang membangun model secara sekuensial." },
  { "en": "Apa itu 'transient response'?", "id": "Respon sistem sebelum mencapai kondisi tunak." },
  { "en": "Apa itu 'steady-state response'?", "id": "Respon sistem setelah waktu yang lama." },
  { "en": "'Transient response' ditentukan oleh?", "id": "Letak pole sistem." },
  { "en": "'Steady-state response' ditentukan oleh?", "id": "Input dan gain DC sistem." },
  { "en": "Singkatan ECG (Electrocardiogram)?", "id": "Electrocardiogram." },
  { "en": "Sinyal ECG (Electrocardiogram) merekam?", "id": "Aktivitas kelistrikan jantung." },
  { "en": "Apa itu gelombang P pada ECG (Electrocardiogram)?", "id": "Depolarisasi atrium." },
  { "en": "Apa itu kompleks QRS pada ECG (Electrocardiogram)?", "id": "Depolarisasi ventrikel." },
  { "en": "Apa itu gelombang T pada ECG (Electrocardiogram)?", "id": "Repolarisasi ventrikel." },
  { "en": "Singkatan EEG (Electroencephalogram)?", "id": "Electroencephalogram." },
  { "en": "Sinyal EEG (Electroencephalogram) merekam?", "id": "Aktivitas kelistrikan otak." },
  { "en": "Contoh ritme gelombang otak?", "id": "Alpha, Beta, Delta, Theta, Gamma." },
  { "en": "Ritme Alpha berhubungan dengan?", "id": "Kondisi rileks dan tenang." },
  { "en": "Ritme Beta berhubungan dengan?", "id": "Kondisi aktif dan waspada." },
  { "en": "Singkatan EMG (Electromyogram)?", "id": "Electromyogram." },
  { "en": "Sinyal EMG (Electromyogram) merekam?", "id": "Aktivitas kelistrikan otot." },
  { "en": "Noise umum pada sinyal biomedis?", "id": "Interferensi jala-jala listrik." },
  { "en": "Frekuensi noise jala-jala di Indonesia?", "id": "50 Hz." },
  { "en": "Filter untuk menghilangkan noise jala-jala?", "id": "Filter notch." },
  { "en": "Apa itu 'baseline wander'?", "id": "Fluktuasi frekuensi rendah pada sinyal." },
  { "en": "Cara menghilangkan 'baseline wander'?", "id": "Menggunakan filter high-pass." },
  { "en": "Apa itu 'phoneme'?", "id": "Unit suara terkecil dalam bahasa." },
  { "en": "Apa itu 'formant'?", "id": "Puncak resonansi pada spektrum suara." },
  { "en": "Forman digunakan untuk mengidentifikasi?", "id": "Bunyi vokal." },
  { "en": "Apa itu 'pitch' dalam suara?", "id": "Persepsi frekuensi fundamental." },
  { "en": "Singkatan MFCC (Mel-Frequency Cepstral Coefficients)?", "id": "Mel-Frequency Cepstral Coefficients." },
  { "en": "MFCC (Mel-Frequency Cepstral Coefficients) digunakan untuk?", "id": "Ekstraksi fitur dalam pengenalan suara." },
  { "en": "Apa itu 'prosody'?", "id": "Irama, stres, dan intonasi ucapan." },
  { "en": "Apa itu 'speech synthesis'?", "id": "Proses menghasilkan ucapan buatan." },
  { "en": "Apa itu 'speech recognition'?", "id": "Proses mengubah ucapan menjadi teks." },
  { "en": "Apa itu 'pole placement'?", "id": "Teknik desain kontroler state-space." },
  { "en": "Tujuan 'pole placement'?", "id": "Menempatkan pole sistem loop tertutup." },
  { "en": "Apa itu 'observer' dalam sistem kontrol?", "id": "Mengestimasi state internal sistem." },
  { "en": "Nama lain 'observer'?", "id": "State estimator." },
  { "en": "Contoh 'observer' yang terkenal?", "id": "Luenberger observer." },
  { "en": "Singkatan PID (Proportional-Integral-Derivative)?", "id": "Proportional-Integral-Derivative." },
  { "en": "Kontroler PID (Proportional-Integral-Derivative) adalah contoh?", "id": "Sistem umpan balik (feedback)." },
  { "en": "Fungsi aksi P (Proportional)?", "id": "Memberi respon proporsional terhadap error." },
  { "en": "Fungsi aksi I (Integral)?", "id": "Menghilangkan error kondisi tunak." },
  { "en": "Fungsi aksi D (Derivative)?", "id": "Memberi respon terhadap laju perubahan error." },
  { "en": "Apa itu 'controller tuning'?", "id": "Proses menentukan parameter kontroler." },
  { "en": "Metode tuning PID (Proportional-Integral-Derivative) yang populer?", "id": "Metode Ziegler-Nichols." },
  { "en": "Apa itu 'actuator'?", "id": "Komponen yang menggerakkan sistem fisik." },
  { "en": "Apa itu 'sensor'?", "id": "Komponen yang mengukur variabel fisik." },
  { "en": "Apa itu 'open-loop control'?", "id": "Kontrol tanpa menggunakan umpan balik." },
  { "en": "Apa itu 'closed-loop control'?", "id": "Kontrol yang menggunakan umpan balik." },
  { "en": "Kelebihan 'closed-loop control'?", "id": "Lebih akurat dan tahan gangguan." },
  { "en": "Apa itu 'root locus'?", "id": "Plot lintasan pole loop tertutup." },
  { "en": "Root locus digunakan untuk?", "id": "Analisis stabilitas dan transien." },
  { "en": "Apa itu 'gain' dalam sistem kontrol?", "id": "Faktor penguatan sinyal." },
  { "en": "Apa itu 'entropy' dalam teori informasi?", "id": "Ukuran rata-rata ketidakpastian." },
  { "en": "Satuan 'entropy'?", "id": "Bit." },
  { "en": "Entropi maksimum terjadi saat?", "id": "Semua hasil memiliki probabilitas sama." },
  { "en": "Apa itu 'mutual information'?", "id": "Ukuran informasi bersama dua variabel." },
  { "en": "Tujuan 'source coding'?", "id": "Kompresi data dengan menghilangkan redundansi." },
  { "en": "Apa itu 'Huffman coding'?", "id": "Algoritma 'source coding' populer." },
  { "en": "Prinsip 'Huffman coding'?", "id": "Simbol sering diberi kode pendek." },
  { "en": "Apa itu 'channel coding'?", "id": "Menambah redundansi untuk koreksi error." },
  { "en": "Apa itu 'code rate'?", "id": "Rasio bit informasi terhadap bit total." },
  { "en": "Apa itu 'Hamming code'?", "id": "Jenis 'channel code' linear sederhana." },
  { "en": "Apa itu 'convolutional encoder'?", "id": "Encoder yang menggunakan memori." },
  { "en": "Apa itu 'Nyquist plot'?", "id": "Plot respon frekuensi dalam koordinat polar." },
  { "en": "Nyquist plot digunakan untuk?", "id": "Menentukan stabilitas sistem umpan balik." },
  { "en": "Apa itu 'Nyquist stability criterion'?", "id": "Kriteria stabilitas berdasarkan Nyquist plot." },
  { "en": "Apa itu 'gain margin'?", "id": "Ukuran seberapa jauh dari ketidakstabilan." },
  { "en": "Apa itu 'phase margin'?", "id": "Ukuran lain dari stabilitas relatif." },
  { "en": "Sistem yang baik memiliki margin?", "id": "Gain dan phase margin yang cukup." },
  { "en": "Apa itu 'dead time' dalam sistem?", "id": "Tunda waktu murni tanpa dinamika." },
  { "en": "Apa itu 'bode plot'?", "id": "Plot magnitudo dan fasa vs frekuensi." },
  { "en": "Apa itu 'nichols chart'?", "id": "Plot log-magnitudo vs fasa." },
  { "en": "Apa itu 'system identification'?", "id": "Membangun model dari data input-output." },
  { "en": "Model 'system identification' bisa berupa?", "id": "Fungsi transfer atau state-space." },
  { "en": "Apa itu sinyal 'pseudo-random binary sequence' (PRBS)?", "id": "Sinyal biner deterministik mirip noise." },
  { "en": "Singkatan PRBS (Pseudo-Random Binary Sequence)?", "id": "Pseudo-Random Binary Sequence." },
  { "en": "PRBS (Pseudo-Random Binary Sequence) digunakan untuk?", "id": "Menguji dan mengidentifikasi sistem." },
  { "en": "Apa itu 'impulse response' sistem akustik?", "id": "Representasi gema suatu ruangan." },
  { "en": "Apa itu 'anechoic'?", "id": "Tanpa gema." },
  { "en": "Apa itu 'reverberant'?", "id": "Penuh dengan gema." },
  { "en": "Apa itu 'reverberation time' (RT60)?", "id": "Waktu gema meluruh 60 dB." },
  { "en": "Singkatan RT60 (Reverberation Time 60dB)?", "id": "Reverberation Time 60dB." },
  { "en": "Apa itu 'direct sound'?", "id": "Suara yang tiba langsung dari sumber." },
  { "en": "Apa itu 'early reflections'?", "id": "Pantulan suara pertama yang tiba." },
  { "en": "Apa itu 'late reverberation'?", "id": "Kumpulan pantulan suara yang kompleks." },
  { "en": "Apa itu 'head-related transfer function' (HRTF)?", "id": "Fungsi transfer dari sumber ke telinga." },
  { "en": "Singkatan HRTF (Head-Related Transfer Function)?", "id": "Head-Related Transfer Function." },
  { "en": "HRTF (Head-Related Transfer Function) digunakan untuk?", "id": "Menciptakan audio 3D (binaural)." },
  { "en": "Apa itu 'binaural audio'?", "id": "Rekaman audio yang meniru pendengaran manusia." },
  { "en": "Apa itu 'equal-loudness contour'?", "id": "Kurva sensitivitas pendengaran manusia." },
  { "en": "Nama lain 'equal-loudness contour'?", "id": "Kurva Fletcher-Munson." },
  { "en": "Telinga manusia paling sensitif pada frekuensi?", "id": "Sekitar 1-5 kHz." },
  { "en": "Apa itu 'masking' dalam psikoakustik?", "id": "Satu suara menutupi suara lain." },
  { "en": "Jenis 'masking'?", "id": "Frequency masking dan temporal masking." },
  { "en": "Prinsip 'masking' digunakan dalam?", "id": "Kompresi audio seperti MP3." },
  { "en": "Apa itu 'critical bands'?", "id": "Pita-pita frekuensi pendengaran manusia." },
  { "en": "Apa perbedaan sinyal energi dan daya?", "id": "Energi terbatas versus daya terbatas." },
  { "en": "Apakah sistem tanpa memori selalu kausal?", "id": "Ya, selalu bersifat kausal." },
  { "en": "Apakah sistem kausal selalu punya memori?", "id": "Tidak, bisa tanpa memori." },
  { "en": "Dapatkah sinyal periodik menjadi sinyal energi?", "id": "Tidak, energinya selalu tak hingga." },
  { "en": "ROC (Region of Convergence) sekuens terbalik waktu?", "id": "Inversi dari ROC (Region of Convergence) aslinya." },
  { "en": "Model 'Black-Scholes' dalam keuangan menggunakan?", "id": "Proses stokastik (gerak Brown)." },
  { "en": "Analisis sinyal seismik digunakan untuk?", "id": "Eksplorasi minyak dan gas bumi." },
  { "en": "Apa itu 'channel equalization'?", "id": "Mengkompensasi distorsi pada kanal komunikasi." },
  { "en": "Apa itu 'adaptive equalization'?", "id": "Equalization yang beradaptasi dengan kanal." },
  { "en": "Apa itu 'training sequence'?", "id": "Sinyal yang diketahui untuk melatih equalizer." },
  { "en": "Apa itu 'space-time coding'?", "id": "Teknik coding untuk sistem MIMO." },
  { "en": "Singkatan MIMO (Multiple-Input Multiple-Output)?", "id": "Multiple-Input Multiple-Output." },
  { "en": "Apa itu 'beamforming' adaptif?", "id": "Beamforming yang beradaptasi dengan lingkungan." },
  { "en": "Apa itu 'spread spectrum'?", "id": "Teknik menyebarkan sinyal di spektrum lebar." },
  { "en": "Tujuan 'spread spectrum'?", "id": "Tahan terhadap interferensi dan penyadapan." },
  { "en": "Jenis 'spread spectrum'?", "id": "DSSS dan FHSS." },
  { "en": "Singkatan DSSS (Direct-Sequence Spread Spectrum)?", "id": "Direct-Sequence Spread Spectrum." },
  { "en": "Singkatan FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Frequency-Hopping Spread Spectrum." },
  { "en": "Apa itu 'processing gain'?", "id": "Ukuran performa sistem spread spectrum." },
  { "en": "Apa itu 'gold codes'?", "id": "Sekuens pseudo-noise dengan korelasi baik." },
  { "en": "Apa itu 'matched filter'?", "id": "Filter optimal untuk memaksimalkan SNR." },
  { "en": "Apa itu konvergensi 'pointwise'?", "id": "Konvergensi di setiap titik individual." },
  { "en": "Apa itu konvergensi 'mean-square'?", "id": "Konvergensi dalam arti energi error." },
  { "en": "Deret Fourier konvergen dalam arti?", "id": "Mean-square (energi)." },
  { "en": "Apa itu fungsi 'signum' (sgn)?", "id": "Fungsi yang memberikan tanda dari angka." },
  { "en": "Fungsi 'signum' adalah fungsi?", "id": "Fungsi ganjil." },
  { "en": "Transformasi Fourier dari fungsi 'signum'?", "id": "2 dibagi j omega." },
  { "en": "Apa itu 'fixed-point arithmetic'?", "id": "Aritmetika menggunakan representasi integer." },
  { "en": "Apa itu 'floating-point arithmetic'?", "id": "Aritmetika menggunakan representasi real." },
  { "en": "Kelebihan DSP (Digital Signal Processor) 'fixed-point'?", "id": "Lebih cepat, murah, daya rendah." },
  { "en": "Kekurangan DSP (Digital Signal Processor) 'fixed-point'?", "id": "Rentang dinamis terbatas, butuh scaling." },
  { "en": "Kelebihan DSP (Digital Signal Processor) 'floating-point'?", "id": "Rentang dinamis besar, mudah diprogram." },
  { "en": "Kekurangan DSP (Digital Signal Processor) 'floating-point'?", "id": "Lebih lambat, mahal, daya tinggi." },
  { "en": "Apa itu 'pipelining' dalam arsitektur prosesor?", "id": "Mengeksekusi instruksi secara tumpang tindih." },
  { "en": "Tujuan 'pipelining'?", "id": "Meningkatkan throughput instruksi." },
  { "en": "Apa itu 'circular buffer'?", "id": "Struktur data untuk streaming sinyal." },
  { "en": "Aplikasi 'circular buffer'?", "id": "Implementasi filter FIR efisien." },
  { "en": "Siapa penemu filter Kalman?", "id": "Rudolf E. Kálmán." },
  { "en": "Teorema sampling sering juga disebut teorema?", "id": "Nyquist-Shannon-Kotelnikov." },
  { "en": "Apa itu 'delta-sigma' ADC (Analog-to-Digital Converter)?", "id": "Converter yang menggunakan oversampling dan noise shaping." },
  { "en": "Apa itu 'flash' ADC (Analog-to-Digital Converter)?", "id": "Jenis ADC paralel yang sangat cepat." },
  { "en": "Kelemahan 'flash' ADC (Analog-to-Digital Converter)?", "id": "Membutuhkan banyak komparator." },
  { "en": "Apa itu 'successive approximation' (SAR) ADC?", "id": "Jenis ADC dengan keseimbangan kecepatan-resolusi." },
  { "en": "Singkatan SAR (Successive-Approximation Register)?", "id": "Successive-Approximation Register." },
  { "en": "Apa itu 'R-2R ladder'?", "id": "Jaringan resistor untuk DAC." },
  { "en": "Apa itu 'glitch' pada output DAC?", "id": "Spike transien saat input berubah." },
  { "en": "Apa itu 'monotonicity' DAC (Digital-to-Analog Converter)?", "id": "Output selalu naik saat input naik." },
  { "en": "Apa itu 'differential nonlinearity' (DNL)?", "id": "Ukuran error antar level output DAC." },
  { "en": "Singkatan DNL (Differential Nonlinearity)?", "id": "Differential Nonlinearity." },
  { "en": "Apa itu 'integral nonlinearity' (INL)?", "id": "Ukuran deviasi dari garis lurus ideal." },
  { "en": "Singkatan INL (Integral Nonlinearity)?", "id": "Integral Nonlinearity." },
  { "en": "Apa itu 'spurious-free dynamic range' (SFDR)?", "id": "Rentang sinyal bebas dari komponen palsu." },
  { "en": "Singkatan SFDR (Spurious-Free Dynamic Range)?", "id": "Spurious-Free Dynamic Range." },
  { "en": "Apa itu 'total harmonic distortion' (THD)?", "id": "Ukuran distorsi harmonik." },
  { "en": "Singkatan THD (Total Harmonic Distortion)?", "id": "Total Harmonic Distortion." },
  { "en": "Apa itu 'phase noise'?", "id": "Fluktuasi acak pada fasa osilator." },
  { "en": "Efek 'phase noise' pada komunikasi?", "id": "Membatasi performa sistem." },
  { "en": "Apa itu 'reciprocal mixing'?", "id": "Efek phase noise pada mixer." },
  { "en": "Apa itu 'I/Q imbalance'?", "id": "Ketidakcocokan antara kanal I dan Q." },
  { "en": "Efek 'I/Q imbalance'?", "id": "Menurunkan performa modulasi QAM." },
  { "en": "Apa itu 'DC offset'?", "id": "Komponen DC yang tidak diinginkan." },
  { "en": "Penyebab 'DC offset' pada receiver?", "id": "Self-mixing pada osilator lokal." },
  { "en": "Apa itu 'software-defined radio' (SDR)?", "id": "Sistem radio yang komponennya diimplementasikan software." },
  { "en": "Singkatan SDR (Software-Defined Radio)?", "id": "Software-Defined Radio." },
  { "en": "Kelebihan SDR (Software-Defined Radio)?", "id": "Fleksibel dan dapat dikonfigurasi ulang." },
  { "en": "Apa itu 'cognitive radio'?", "id": "Radio cerdas yang bisa beradaptasi." },
  { "en": "Fungsi 'cognitive radio'?", "id": "Mendeteksi dan menggunakan spektrum kosong." },
  { "en": "Apa itu 'spectral sensing'?", "id": "Proses mendeteksi spektrum yang tidak digunakan." },
  { "en": "Apa itu 'orthogonal basis'?", "id": "Set vektor/fungsi yang saling tegak lurus." },
  { "en": "Apa itu 'orthonormal basis'?", "id": "Basis ortogonal dengan norma satu." },
  { "en": "Apa itu 'signal space'?", "id": "Ruang vektor yang ditempati oleh sinyal." },
  { "en": "Dimensi 'signal space' ditentukan oleh?", "id": "Jumlah fungsi basis." },
  { "en": "Diagram konstelasi adalah representasi sinyal di?", "id": "Signal space." },
  { "en": "Jarak antar titik konstelasi berhubungan dengan?", "id": "Probabilitas error." },
  { "en": "Apa itu 'decision boundary'?", "id": "Batas pemisah antar simbol di signal space." },
  { "en": "Apa itu 'maximum likelihood' (ML) detection?", "id": "Aturan keputusan optimal di penerima." },
  { "en": "Singkatan ML (Maximum Likelihood)?", "id": "Maximum Likelihood." },
  { "en": "Apa itu 'Bayesian inference'?", "id": "Metode statistik berdasarkan Teorema Bayes." },
  { "en": "Apa itu 'prior probability'?", "id": "Probabilitas sebelum observasi." },
  { "en": "Apa itu 'posterior probability'?", "id": "Probabilitas setelah observasi." },
  { "en": "Apa itu 'likelihood function'?", "id": "Probabilitas observasi data." },
  { "en": "Apa itu 'Cramer-Rao lower bound' (CRLB)?", "id": "Batas bawah teoritis pada varians estimator." },
  { "en": "Singkatan CRLB (Cramér–Rao Lower Bound)?", "id": "Cramér–Rao Lower Bound." },
  { "en": "Estimator yang mencapai CRLB (Cramér–Rao Lower Bound) disebut?", "id": "Estimator efisien." },
  { "en": "Apa itu 'Fisher information matrix'?", "id": "Matriks dalam perhitungan CRLB." },
  { "en": "Apa itu 'sufficient statistic'?", "id": "Statistik yang mengandung semua informasi." },
  { "en": "Apa itu 'law of large numbers'?", "id": "Rata-rata sampel konvergen ke mean." },
  { "en": "Apa itu 'central limit theorem'?", "id": "Jumlah variabel acak menuju distribusi normal." },
  { "en": "Distribusi Gaussian juga disebut?", "id": "Distribusi normal." },
  { "en": "Distribusi Rayleigh digunakan untuk memodelkan?", "id": "Amplop sinyal fading." },
  { "en": "Distribusi Rician digunakan untuk memodelkan?", "id": "Fading dengan komponen line-of-sight." },
  { "en": "Singkatan LOS (Line-of-Sight)?", "id": "Line-of-Sight." },
  { "en": "Apa itu 'multipath fading'?", "id": "Fluktuasi sinyal akibat pantulan ganda." },
  { "en": "Apa itu 'delay spread'?", "id": "Sebaran waktu tunda pada kanal multipath." },
  { "en": "Apa itu 'coherence bandwidth'?", "id": "Ukuran rentang frekuensi kanal flat." },
  { "en": "Kanal 'flat fading' terjadi jika?", "id": "Bandwidth sinyal lebih kecil dari coherence." },
  { "en": "Kanal 'frequency-selective fading' terjadi jika?", "id": "Bandwidth sinyal lebih besar dari coherence." },
  { "en": "Apa itu 'Doppler spread'?", "id": "Sebaran frekuensi akibat pergerakan." },
  { "en": "Apa itu 'coherence time'?", "id": "Ukuran durasi kanal tetap statis." },
  { "en": "Apa itu 'convolution theorem'?", "id": "Teorema yang menghubungkan konvolusi dan perkalian." },
  { "en": "Apa itu 'Parseval's theorem'?", "id": "Teorema tentang konservasi energi/daya." },
  { "en": "Apa itu 'Plancherel's theorem'?", "id": "Generalisasi dari Teorema Parseval." },
  { "en": "Apa itu 'Hilbert space'?", "id": "Ruang vektor dengan produk dalam." },
  { "en": "Sinyal dapat dianggap sebagai vektor di?", "id": "Ruang Hilbert." },
  { "en": "Apa itu 'norm' dari sinyal?", "id": "Ukuran panjang atau energi sinyal." },
  { "en": "Norm-2 (L2 norm) dari sinyal berhubungan dengan?", "id": "Energi sinyal." },
  { "en": "Apa itu 'inner product' dari dua sinyal?", "id": "Generalisasi dari perkalian titik." },
  { "en": "Inner product dari dua sinyal ortogonal?", "id": "Nol." },
  { "en": "Apa itu 'Haar wavelet'?", "id": "Wavelet paling sederhana, berbentuk kotak." },
  { "en": "Apa itu 'Daubechies wavelet'?", "id": "Keluarga wavelet ortogonal yang populer." },
  { "en": "Apa itu 'Mexican hat wavelet'?", "id": "Wavelet yang berasal dari fungsi Gaussian." },
  { "en": "Apa itu 'continuous wavelet transform' (CWT)?", "id": "Transformasi wavelet untuk sinyal kontinyu." },
  { "en": "Singkatan CWT (Continuous Wavelet Transform)?", "id": "Continuous Wavelet Transform." },
  { "en": "Apa itu 'discrete wavelet transform' (DWT)?", "id": "Versi diskrit dari transformasi wavelet." },
  { "en": "Singkatan DWT (Discrete Wavelet Transform)?", "id": "Discrete Wavelet Transform." },
  { "en": "Aplikasi DWT (Discrete Wavelet Transform)?", "id": "Kompresi gambar JPEG 2000." },
  { "en": "Apa itu 'multiresolution analysis' (MRA)?", "id": "Analisis sinyal pada resolusi berbeda." },
  { "en": "Singkatan MRA (Multiresolution Analysis)?", "id": "Multiresolution Analysis." },
  { "en": "Apa itu 'scaling function' dalam wavelet?", "id": "Fungsi dasar untuk aproksimasi." },
  { "en": "Apa itu 'wavelet function'?", "id": "Fungsi dasar untuk detail." },
  { "en": "Apa itu 'Trellis-Coded Modulation' (TCM)?", "id": "Skema modulasi dan coding terintegrasi." },
  { "en": "Singkatan TCM (Trellis-Coded Modulation)?", "id": "Trellis-Coded Modulation." },
  { "en": "Tujuan TCM (Trellis-Coded Modulation)?", "id": "Mendapatkan coding gain tanpa menambah bandwidth." },
  { "en": "Apa itu 'Turbo codes'?", "id": "Kode koreksi error berkinerja tinggi." },
  { "en": "Kinerja 'Turbo codes' mendekati?", "id": "Batas Shannon." },
  { "en": "Apa itu 'Low-Density Parity-Check' (LDPC) codes?", "id": "Kode koreksi error modern yang efisien." },
  { "en": "Singkatan LDPC (Low-Density Parity-Check)?", "id": "Low-Density Parity-Check." },
  { "en": "Apa itu 'polar codes'?", "id": "Kode yang terbukti mencapai kapasitas kanal." },
  { "en": "Apa itu 'channel state information' (CSI)?", "id": "Informasi mengenai kondisi properti kanal." },
  { "en": "Singkatan CSI (Channel State Information)?", "id": "Channel State Information." },
  { "en": "Apa itu 'fading channel'?", "id": "Kanal komunikasi yang sinyalnya berfluktuasi." },
  { "en": "Apa itu 'diversity' dalam komunikasi?", "id": "Teknik melawan fading." },
  { "en": "Jenis 'diversity'?", "id": "Waktu, frekuensi, ruang." },
  { "en": "Apa itu 'spatial diversity'?", "id": "Menggunakan beberapa antena." },
  { "en": "Apa itu 'RAKE receiver'?", "id": "Penerima untuk sistem DSSS multipath." },
  { "en": "Apa itu 'Lyapunov stability'?", "id": "Metode analisis stabilitas sistem non-linear." },
  { "en": "Apa itu 'Lyapunov function'?", "id": "Fungsi skalar untuk membuktikan stabilitas." },
  { "en": "Apa itu 'controllability matrix'?", "id": "Matriks untuk menguji keterkontrolan sistem." },
  { "en": "Apa itu 'observability matrix'?", "id": "Matriks untuk menguji keteramatan sistem." },
  { "en": "Sistem 'controllable' jika matriksnya?", "id": "Memiliki rank penuh (full rank)." },
  { "en": "Sistem 'observable' jika matriksnya?", "id": "Juga memiliki rank penuh." },
  { "en": "Apa itu 'optimal control'?", "id": "Desain kontroler untuk optimalkan 'cost function'." },
  { "en": "Apa itu 'cost function' (fungsi biaya)?", "id": "Fungsi yang ingin diminimalkan." },
  { "en": "Apa itu 'Linear-Quadratic Regulator' (LQR)?", "id": "Jenis kontroler optimal yang populer." },
  { "en": "Singkatan LQR (Linear-Quadratic Regulator)?", "id": "Linear-Quadratic Regulator." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Versi optimal dari Luenberger observer." },
  { "en": "Apa itu 'robust control'?", "id": "Desain kontroler yang tahan ketidakpastian." },
  { "en": "Apa itu 'adaptive control'?", "id": "Kontroler yang parameternya beradaptasi." },
  { "en": "Hubungan antara Z-transform dan deret Laurent?", "id": "Z-transform adalah deret Laurent." },
  { "en": "Apa itu 'analytic signal'?", "id": "Sinonim untuk sinyal analitik." },
  { "en": "Filter FIR (Finite Impulse Response) apakah selalu kausal?", "id": "Tidak, tapi bisa dibuat kausal." },
  { "en": "Sistem FIR (Finite Impulse Response) non-kausal bisa diimplementasikan?", "id": "Ya, dengan offline processing." },
  { "en": "Apa itu 'group delay' negatif?", "id": "Terjadi pada sistem fasa non-minimum." },
  { "en": "Apakah 'group delay' negatif melanggar kausalitas?", "id": "Tidak." },
  { "en": "Apa itu 'Wiener filter'?", "id": "Filter optimal untuk estimasi sinyal." },
  { "en": "Tujuan 'Wiener filter'?", "id": "Meminimalkan mean square error (MSE)." },
  { "en": "Singkatan MSE (Mean Square Error)?", "id": "Mean Square Error." },
  { "en": "Apa itu 'spectral factorization'?", "id": "Dekomposisi spektrum daya." },
  { "en": "Apa itu 'power spectrum'?", "id": "Sinonim untuk Power Spectral Density (PSD)." },
  { "en": "Apa itu 'ergodicity'?", "id": "Sifat proses acak." },
  { "en": "Apa itu 'cyclostationarity'?", "id": "Sifat proses acak lain." },
  { "en": "Apa itu 'sampling jitter'?", "id": "Sinonim untuk aperture jitter." },
  { "en": "Apa itu 'aliasing' dalam konteks gambar?", "id": "Munculnya pola bergerigi (jaggies)." },
  { "en": "Metode 'anti-aliasing' pada grafik komputer?", "id": "Supersampling atau multisampling." },
  { "en": "Apa itu 'texture mapping'?", "id": "Menerapkan gambar (tekstur) ke permukaan 3D." },
  { "en": "Apa itu 'bump mapping'?", "id": "Teknik memberi ilusi detail permukaan." },
  { "en": "Apa itu 'ray tracing'?", "id": "Teknik rendering grafik yang realistis." },
  { "en": "Prinsip 'ray tracing'?", "id": "Mensimulasikan jalur cahaya (sinar)." },
  { "en": "Apa itu 'photon mapping'?", "id": "Teknik rendering lain untuk iluminasi global." },
  { "en": "Apa itu 'finite difference method'?", "id": "Metode numerik untuk solusi persamaan diferensial." },
  { "en": "Apa itu 'finite element method' (FEM)?", "id": "Metode numerik lain yang kuat." },
  { "en": "Singkatan FEM (Finite Element Method)?", "id": "Finite Element Method." },
  { "en": "Aplikasi FEM (Finite Element Method)?", "id": "Analisis struktur dan elektromagnetik." },
  { "en": "Apa itu 'boundary conditions'?", "id": "Kondisi yang harus dipenuhi di batas." },
  { "en": "Apa itu 'initial conditions'?", "id": "Kondisi sistem pada waktu awal." },
  { "en": "Apa itu 'Fourier number'?", "id": "Bilangan tak berdimensi dalam konduksi panas." },
  { "en": "Apa itu 'Biot number'?", "id": "Bilangan tak berdimensi lain dalam transfer panas." },
  { "en": "Hubungan antara sinyal dan transfer panas?", "id": "Keduanya dijelaskan oleh persamaan diferensial." },
  { "en": "Apa itu 'Green's function'?", "id": "Respon impuls dari operator diferensial." },
  { "en": "Apa itu 'Dirac comb'?", "id": "Sinonim untuk impulse train." },
  { "en": "Transformasi Fourier dari 'Dirac comb'?", "id": "Juga sebuah 'Dirac comb'." },
  { "en": "Apa itu 'uncertainty principle'?", "id": "Prinsip ketidakpastian Heisenberg." },
  { "en": "Aplikasi 'uncertainty principle' pada sinyal?", "id": "Sinyal tak bisa sempit di waktu dan frekuensi." },
  { "en": "Apa itu 'time-bandwidth product'?", "id": "Ukuran kuantitatif dari prinsip ketidakpastian." },
  { "en": "Nilai minimum 'time-bandwidth product' untuk sinyal?", "id": "Dicapai oleh pulsa Gaussian." },
  { "en": "Apa itu 'analytic continuation'?", "id": "Teknik memperluas domain fungsi analitik." },
  { "en": "Hubungan Laplace dan Fourier via 'analytic continuation'?", "id": "Transformasi Fourier adalah evaluasi di sumbu." },
  { "en": "Apa itu 'Paley-Wiener theorem'?", "id": "Menghubungkan kausalitas dengan respon frekuensi." },
  { "en": "Apa itu 'Kramers-Kronig relations'?", "id": "Menghubungkan bagian real dan imajiner." },
  { "en": "Kondisi untuk relasi 'Kramers-Kronig'?", "id": "Sistem harus kausal." },
  { "en": "Apa itu 'white noise'?", "id": "Sinyal acak dengan spektrum daya rata." },
  { "en": "Autokorelasi dari 'white noise'?", "id": "Fungsi impuls di titik nol." },
  { "en": "Apa itu 'shot noise'?", "id": "Noise acak akibat sifat diskrit muatan." },
  { "en": "Apa itu 'thermal noise' (Johnson-Nyquist noise)?", "id": "Noise akibat agitasi termal elektron." },
  { "en": "Transformasi mana yang menghubungkan domain-s dan domain-z?", "id": "Bilinear transformation." },
  { "en": "Teori sampling menjembatani sinyal apa?", "id": "Sinyal waktu-kontinyu dan waktu-diskrit." },
  { "en": "Stabilitas BIBO (Bounded-Input Bounded-Output) mensyaratkan apa pada respon impuls?", "id": "Terintegralkan atau terjumlahkan absolut." },
  { "en": "Fenomena overshoot dekat diskontinuitas disebut?", "id": "Fenomena Gibbs." },
  { "en": "Representasi sinyal sebagai jumlahan sinusoida disebut?", "id": "Deret Fourier." },
  { "en": "Bahasa pemrograman populer untuk DSP (Digital Signal Processing)?", "id": "MATLAB, Python, C++." },
  { "en": "Apa itu Simulink?", "id": "Lingkungan simulasi grafis berbasis blok." },
  { "en": "Apa itu LabVIEW?", "id": "Platform pengembangan sistem pemrograman grafis." },
  { "en": "Apa itu 'GNU Radio'?", "id": "Toolkit software gratis untuk SDR." },
  { "en": "Apa itu 'bispectrum'?", "id": "Analisis spektral orde tinggi." },
  { "en": "Tujuan analisis 'bispectrum'?", "id": "Mendeteksi interaksi fasa non-linear." },
  { "en": "Apa itu 'kurtosis'?", "id": "Ukuran 'tailedness' dari distribusi probabilitas." },
  { "en": "Apa itu 'skewness'?", "id": "Ukuran asimetri distribusi probabilitas." },
  { "en": "Distribusi normal memiliki 'skewness'?", "id": "Nol." },
  { "en": "Apa itu 'Gardner algorithm'?", "id": "Algoritma untuk timing recovery (sinkronisasi)." },
  { "en": "Apa itu 'Mueller and Müller algorithm'?", "id": "Algoritma timing recovery lain." },
  { "en": "Apa itu 'Farrow structure'?", "id": "Struktur efisien untuk filter fraksional delay." },
  { "en": "Aplikasi 'Farrow structure'?", "id": "Konversi laju sampling arbitrer." },
  { "en": "Siapa yang mempopulerkan algoritma FFT (Fast Fourier Transform)?", "id": "Cooley dan Tukey." },
  { "en": "Kontroler PID (Proportional-Integral-Derivative) pertama kali dikembangkan untuk?", "id": "Sistem kemudi kapal otomatis." },
  { "en": "Apa itu 'fractional Fourier transform' (FrFT)?", "id": "Generalisasi dari Transformasi Fourier." },
  { "en": "Singkatan FrFT (Fractional Fourier Transform)?", "id": "Fractional Fourier Transform." },
  { "en": "Apa itu 'linear canonical transform' (LCT)?", "id": "Generalisasi dari banyak transformasi." },
  { "en": "Singkatan LCT (Linear Canonical Transform)?", "id": "Linear Canonical Transform." },
  { "en": "Apa itu 'WDF (Wave Digital Filter)'?", "id": "Filter digital yang meniru sirkuit analog." },
  { "en": "Singkatan WDF (Wave Digital Filter)?", "id": "Wave Digital Filter." },
  { "en": "Apa itu 'Goertzel algorithm'?", "id": "Algoritma efisien deteksi satu frekuensi." },
  { "en": "Aplikasi 'Goertzel algorithm'?", "id": "Deteksi nada DTMF pada telepon." },
  { "en": "Singkatan DTMF (Dual-Tone Multi-Frequency)?", "id": "Dual-Tone Multi-Frequency." },
  { "en": "Apa itu 'chirplet transform'?", "id": "Representasi sinyal menggunakan sinyal chirp." },
  { "en": "Apa itu 'sparse signal'?", "id": "Sinyal yang sebagian besar nilainya nol." },
  { "en": "Apa itu 'compressed sensing' (CS)?", "id": "Teknik sampling di bawah laju Nyquist." },
  { "en": "Singkatan CS (Compressed Sensing)?", "id": "Compressed Sensing." },
  { "en": "Syarat 'compressed sensing' (CS)?", "id": "Sinyal harus sparse di domain lain." },
  { "en": "Apa itu 'basis pursuit'?", "id": "Algoritma rekonstruksi untuk compressed sensing." },
  { "en": "Apa itu 'independent component analysis' (ICA)?", "id": "Memisahkan sinyal menjadi komponen independen." },
  { "en": "Singkatan ICA (Independent Component Analysis)?", "id": "Independent Component Analysis." },
  { "en": "Aplikasi ICA (Independent Component Analysis)?", "id": "Blind source separation (BSS)." },
  { "en": "Singkatan BSS (Blind Source Separation)?", "id": "Blind Source Separation." },
  { "en": "Contoh masalah BSS (Blind Source Separation)?", "id": "Cocktail party problem." },
  { "en": "Apa itu 'Volterra series'?", "id": "Ekspansi untuk sistem non-linear." },
  { "en": "Apa itu 'Wiener series'?", "id": "Ekspansi ortogonal untuk sistem non-linear." },
  { "en": "Apa itu 'sampling oscilloscope'?", "id": "Osiloskop untuk sinyal repetitif frekuensi tinggi." },
  { "en": "Apa itu 'logic analyzer'?", "id": "Instrumen untuk menganalisis sinyal digital." },
  { "en": "Apa itu 'network analyzer'?", "id": "Instrumen untuk mengukur parameter jaringan." },
  { "en": "Parameter yang diukur network analyzer?", "id": "S-parameters." },
  { "en": "Apa itu 'scattering parameters' (S-parameters)?", "id": "Mendeskripsikan perilaku jaringan RF." },
  { "en": "Apa itu 'Smith chart'?", "id": "Diagram grafis untuk analisis impedansi." },
  { "en": "Apa itu 'Constellation diagram'?", "id": "Sinonim untuk diagram konstelasi." },
  { "en": "Apa itu 'error vector magnitude' (EVM)?", "id": "Ukuran kualitas sinyal modulasi digital." },
  { "en": "Singkatan EVM (Error Vector Magnitude)?", "id": "Error Vector Magnitude." },
  { "en": "EVM (Error Vector Magnitude) yang rendah menandakan?", "id": "Kualitas sinyal yang baik." },
  { "en": "Apa itu 'papr (peak-to-average power ratio)'?", "id": "Rasio daya puncak ke rata-rata." },
  { "en": "Singkatan PAPR (Peak-to-Average Power Ratio)?", "id": "Peak-to-Average Power Ratio." },
  { "en": "Sinyal OFDM (Orthogonal Frequency-Division Multiplexing) memiliki PAPR (Peak-to-Average Power Ratio)?", "id": "Tinggi." },
  { "en": "Masalah akibat PAPR (Peak-to-Average Power Ratio) tinggi?", "id": "Membutuhkan power amplifier yang linear." },
  { "en": "Apa itu 'crest factor'?", "id": "Ukuran lain dari rasio puncak sinyal." },
  { "en": "Apa itu 'form factor'?", "id": "Rasio nilai RMS terhadap nilai rata-rata." },
  { "en": "Nilai RMS (Root Mean Square) dari sinusoida?", "id": "Amplitudo dibagi akar dua." },
  { "en": "Singkatan RMS (Root Mean Square)?", "id": "Root Mean Square." },
  { "en": "Apa itu 'Gaussian mixture model' (GMM)?", "id": "Model probabilistik menggunakan campuran Gaussian." },
  { "en": "Singkatan GMM (Gaussian Mixture Model)?", "id": "Gaussian Mixture Model." },
  { "en": "Apa itu 'expectation-maximization' (EM) algorithm?", "id": "Algoritma untuk estimasi parameter GMM." },
  { "en": "Singkatan EM (Expectation-Maximization)?", "id": "Expectation-Maximization." },
  { "en": "Apa itu 'singular spectrum analysis' (SSA)?", "id": "Metode analisis time series." },
  { "en": "Singkatan SSA (Singular Spectrum Analysis)?", "id": "Singular Spectrum Analysis." },
  { "en": "Apa itu 'Karhunen-Loève transform' (KLT)?", "id": "Transformasi optimal untuk kompresi." },
  { "en": "Singkatan KLT (Karhunen-Loève Transform)?", "id": "Karhunen-Loève Transform." },
  { "en": "KLT (Karhunen-Loève Transform) sama dengan?", "id": "PCA untuk sinyal zero-mean." },
  { "en": "Apa itu 'Walsh-Hadamard transform'?", "id": "Transformasi non-sinusoidal ortogonal." },
  { "en": "Elemen matriks Hadamard?", "id": "+1 dan -1." },
  { "en": "Apa itu 'Slant transform'?", "id": "Transformasi lain untuk kompresi gambar." },
  { "en": "Apa itu 'Haar transform'?", "id": "Transformasi berbasis wavelet Haar." },
  { "en": "Apa itu 'entropy coding'?", "id": "Jenis kompresi data lossless." },
  { "en": "Contoh 'entropy coding'?", "id": "Huffman coding, Arithmetic coding." },
  { "en": "Apa itu 'run-length encoding' (RLE)?", "id": "Metode kompresi sederhana." },
  { "en": "Singkatan RLE (Run-Length Encoding)?", "id": "Run-Length Encoding." },
  { "en": "Prinsip RLE (Run-Length Encoding)?", "id": "Menyimpan data berulang sebagai hitungan." },
  { "en": "Apa itu 'Lempel-Ziv-Welch' (LZW) algorithm?", "id": "Algoritma kompresi lossless." },
  { "en": "Singkatan LZW (Lempel-Ziv-Welch)?", "id": "Lempel-Ziv-Welch." },
  { "en": "Aplikasi LZW (Lempel-Ziv-Welch)?", "id": "Format gambar GIF." },
  { "en": "Apa itu 'quantizer'?", "id": "Sistem yang melakukan kuantisasi." },
  { "en": "Jenis 'quantizer'?", "id": "Scalar dan vector." },
  { "en": "Apa itu 'dither'?", "id": "Noise intensitas rendah yang ditambahkan." },
  { "en": "Tujuan 'dither'?", "id": "Mengurangi distorsi akibat kuantisasi." },
  { "en": "Apa itu 'Nyquist frequency'?", "id": "Setengah dari frekuensi sampling." },
  { "en": "Frekuensi di atas 'Nyquist frequency' akan?", "id": "Mengalami aliasing." },
  { "en": "Apa itu 'passband ripple'?", "id": "Riak di pita lolos filter." },
  { "en": "Nama lain 'passband ripple'?", "id": "Chebyshev ripple." },
  { "en": "Apa itu 'stopband ripple'?", "id": "Riak di pita henti filter." },
  { "en": "Apa itu 'zero-forcing equalizer'?", "id": "Equalizer yang mencoba membalik kanal." },
  { "en": "Kelemahan 'zero-forcing equalizer'?", "id": "Memperkuat noise secara berlebihan." },
  { "en": "Apa itu 'minimum mean-square error' (MMSE) equalizer?", "id": "Equalizer yang meminimalkan error kuadrat." },
  { "en": "Singkatan MMSE (Minimum Mean-Square Error)?", "id": "Minimum Mean-Square Error." },
  { "en": "Apa itu 'decision feedback equalizer' (DFE)?", "id": "Equalizer non-linear yang menggunakan keputusan." },
  { "en": "Singkatan DFE (Decision Feedback Equalizer)?", "id": "Decision Feedback Equalizer." },
  { "en": "Apa itu 'carrier sense multiple access' (CSMA)?", "id": "Protokol akses media." },
  { "en": "Singkatan CSMA (Carrier Sense Multiple Access)?", "id": "Carrier Sense Multiple Access." },
  { "en": "Aplikasi CSMA (Carrier Sense Multiple Access)?", "id": "Wi-Fi (802.11)." }


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
