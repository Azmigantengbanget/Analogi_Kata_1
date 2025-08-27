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


  { "en": "Mata : Melihat = Telinga : .........", "id": "Mendengar" },
  { "en": "Panas : Api = Dingin : .........", "id": "Es" },
  { "en": "Pisau : Memotong = Pulpen : .........", "id": "Menulis" },
  { "en": "Guru : Sekolah = Dokter : .........", "id": "Rumah Sakit" },
  { "en": "Jari : Tangan = Daun : .........", "id": "Ranting" },
  { "en": "Siang : Matahari = Malam : .........", "id": "Bulan" },
  { "en": "Lapar : Makan = Haus : .........", "id": "Minum" },
  { "en": "Mobil : Roda = Perahu : .........", "id": "Dayung" },
  { "en": "Pilot : Pesawat = Masinis : .........", "id": "Kereta Api" },
  { "en": "Benar : Salah = Baik : .........", "id": "Buruk" },
  { "en": "Kucing : Mengeong = Anjing : .........", "id": "Menggonggong" },
  { "en": "Buku : Kertas = Tembok : .........", "id": "Batu Bata" },
  { "en": "Indonesia : Jakarta = Jepang : .........", "id": "Tokyo" },
  { "en": "Nasi : Beras = Roti : .........", "id": "Tepung" },
  { "en": "Pelukis : Kuas = Penulis : .........", "id": "Pena" },
  { "en": "Air : Cair = Udara : .........", "id": "Gas" },
  { "en": "Senang : Tertawa = Sedih : .........", "id": "Menangis" },
  { "en": "Gula : Manis = Garam : .........", "id": "Asin" },
  { "en": "Ayah : Pria = Ibu : .........", "id": "Wanita" },
  { "en": "Kepala : Helm = Kaki : .........", "id": "Sepatu" },
  { "en": "Padi : Petani = Ikan : .........", "id": "Nelayan" },
  { "en": "Detik : Menit = Menit : .........", "id": "Jam" },
  { "en": "Sapi : Susu = Lebah : .........", "id": "Madu" },
  { "en": "Merah : Mawar = Kuning : .........", "id": "Pisang" },
  { "en": "Jarum : Menjahit = Gunting : .........", "id": "Memotong" },
  { "en": "Semut : Kecil = Gajah : .........", "id": "Besar" },
  { "en": "Terbang : Burung = Berenang : .........", "id": "Ikan" },
  { "en": "Atap : Rumah = Mahkota : .........", "id": "Raja" },
  { "en": "Kayu : Pohon = Emas : .........", "id": "Tambang" },
  { "en": "Belajar : Pandai = Malas : .........", "id": "Bodoh" },
  { "en": "Kompor : Memasak = Kulkas : .........", "id": "Mendinginkan" },
  { "en": "Siswa : Belajar = Guru : .........", "id": "Mengajar" },
  { "en": "Gigi : Mengunyah = Lambung : .........", "id": "Mencerna" },
  { "en": "Pagi : Sarapan = Malam : .........", "id": "Makan Malam" },
  { "en": "Uang : Dompet = Pakaian : .........", "id": "Lemari" },
  { "en": "Obat : Apoteker = Makanan : .........", "id": "Koki" },
  { "en": "Pintu : Kunci = Surat : .........", "id": "Perangko" },
  { "en": "Telur : Cangkang = Otak : .........", "id": "Tengkorak" },
  { "en": "Sutradara : Film = Komponis : .........", "id": "Lagu" },
  { "en": "Rambut : Sisir = Lantai : .........", "id": "Sapu" },
  { "en": "Darah : Merah = Daun : .........", "id": "Hijau" },
  { "en": "Tidur : Mengantuk = Bangun : .........", "id": "Segar" },
  { "en": "Bensin : Mobil = Listrik : .........", "id": "Lampu" },
  { "en": "Mata Uang : Rupiah = Negara : .........", "id": "Indonesia" },
  { "en": "Hangat : Panas = Sejuk : .........", "id": "Dingin" },
  { "en": "Buta : Penglihatan = Tuli : .........", "id": "Pendengaran" },
  { "en": "Mikroskop : Mikroba = Teleskop : .........", "id": "Bintang" },
  { "en": "Sakit : Dokter = Rusak : .........", "id": "Montir" },
  { "en": "Penjara : Napi = Sekolah : .........", "id": "Siswa" },
  { "en": "Bus : Terminal = Kapal : .........", "id": "Pelabuhan" },
  { "en": "Aktor : Panggung = Atlet : .........", "id": "Stadion" },
  { "en": "Tajam : Pisau = Tumpul : .........", "id": "Batu" },
  { "en": "Bulu : Unggas = Sisik : .........", "id": "Ikan" },
  { "en": "Laut : Asin = Sungai : .........", "id": "Tawar" },
  { "en": "Palu : Paku = Obeng : .........", "id": "Sekrup" },
  { "en": "Kering : Lembab = Terang : .........", "id": "Redup" },
  { "en": "Tesis : Sarjana = Disertasi : .........", "id": "Doktor" },
  { "en": "Ulat : Kepompong = Larva : .........", "id": "Pupa" },
  { "en": "Kamera : Foto = Perekam : .........", "id": "Suara" },
  { "en": "Kuda : Berlari = Ular : .........", "id": "Melata" },
  { "en": "Semenanjung : Daratan = Teluk : .........", "id": "Lautan" },
  { "en": "Jantung : Memompa = Paru-paru : .........", "id": "Bernapas" },
  { "en": "Kertas : Menulis = Kanvas : .........", "id": "Melukis" },
  { "en": "Abjad : Huruf = Bilangan : .........", "id": "Angka" },
  { "en": "Berhasil : Gagal = Optimis : .........", "id": "Pesimis" },
  { "en": "Jam : Waktu = Termometer : .........", "id": "Suhu" },
  { "en": "Koran : Editor = Film : .........", "id": "Sutradara" },
  { "en": "Nakhoda : Kapal = Kusir : .........", "id": "Delman" },
  { "en": "Bawang : Umbi = Wortel : .........", "id": "Akar" },
  { "en": "Perusahaan : Karyawan = Sekolah : .........", "id": "Staf" },
  { "en": "Pohon : Hutan = Air : .........", "id": "Danau" },
  { "en": "Lingkaran : Bola = Persegi : .........", "id": "Kubus" },
  { "en": "Cepat : Lambat = Tinggi : .........", "id": "Rendah" },
  { "en": "Pramugari : Pesawat = Kasir : .........", "id": "Toko" },
  { "en": "Jahitan : Benang = Anyaman : .........", "id": "Rotan" },
  { "en": "Keringat : Olahraga = Asap : .........", "id": "Api" },
  { "en": "Kopi : Kafein = Teh : .........", "id": "Tein" },
  { "en": "Suara : Audio = Gambar : .........", "id": "Visual" },
  { "en": "Sarung Tangan : Tangan = Kaus Kaki : .........", "id": "Kaki" },
  { "en": "Prolog : Awal = Epilog : .........", "id": "Akhir" },
  { "en": "Gergaji : Tukang Kayu = Stetoskop : .........", "id": "Dokter" },
  { "en": "Jembatan : Sungai = Terowongan : .........", "id": "Gunung" },
  { "en": "Petunjuk : Teka-teki = Peta : .........", "id": "Lokasi" },
  { "en": "Awan : Hujan = Matahari : .........", "id": "Cahaya" },
  { "en": "Wisuda : Kelulusan = Pernikahan : .........", "id": "Perkawinan" },
  { "en": "Kuman : Penyakit = Belajar : .........", "id": "Ilmu" },
  { "en": "Gelap : Cahaya = Hampa : .........", "id": "Udara" },
  { "en": "Manusia : Oksigen = Tumbuhan : .........", "id": "Karbondioksida" },
  { "en": "Berbicara : Mulut = Berjalan : .........", "id": "Kaki" },
  { "en": "Singa : Karnivora = Kambing : .........", "id": "Herbivora" },
  { "en": "Kamar : Hotel = Gerbong : .........", "id": "Kereta" },
  { "en": "Desibel : Suara = Volt : .........", "id": "Listrik" },
  { "en": "Puisi : Penyair = Novel : .........", "id": "Novelis" },
  { "en": "Bab : Buku = Bait : .........", "id": "Puisi" },
  { "en": "Indonesia : Merah Putih = Malaysia : .........", "id": "Jalur Gemilang" },
  { "en": "Sayap : Terbang = Sirip : .........", "id": "Berenang" },
  { "en": "Api : Membakar = Air : .........", "id": "Membasahi" },
  { "en": "Fakta : Nyata = Fiksi : .........", "id": "Khayal" },
  { "en": "Hidung : Mencium = Lidah : .........", "id": "Mengecap" },
  { "en": "Apoteker : Obat = Desainer : .........", "id": "Pakaian" },
  { "en": "Beras : Nasi Goreng = Daging : .........", "id": "Sate" },
  { "en": "Kelopak : Bunga = Halaman : .........", "id": "Buku" },
  { "en": "Oven : Memanggang = Blender : .........", "id": "Menghaluskan" },
  { "en": "Sedan : Mobil = Brokoli : .........", "id": "Sayuran" },
  { "en": "Lelah : Istirahat = Gatal : .........", "id": "Menggaruk" },
  { "en": "Petir : Guntur = Kilat : .........", "id": "Cahaya" },
  { "en": "Sepeda : Pedal = Motor : .........", "id": "Gas" },
  { "en": "Rajin : Prestasi = Tekun : .........", "id": "Keberhasilan" },
  { "en": "Herbivora : Tumbuhan = Insektivora : .........", "id": "Serangga" },
  { "en": "Jenderal : Pasukan = Rektor : .........", "id": "Universitas" },
  { "en": "Thailand : Baht = Vietnam : .........", "id": "Dong" },
  { "en": "Amnesia : Ingatan = Lumpuh : .........", "id": "Gerakan" },
  { "en": "Keringat : Pori-pori = Air Mata : .........", "id": "Kelenjar" },
  { "en": "Ornitologi : Burung = Entomologi : .........", "id": "Serangga" },
  { "en": "Marah : Merah = Duka : .........", "id": "Hitam" },
  { "en": "Stadion : Penonton = Restoran : .........", "id": "Pelanggan" },
  { "en": "Ransel : Membawa = Jala : .........", "id": "Menangkap" },
  { "en": "Prajurit : Tentara = Bhayangkara : .........", "id": "Polisi" },
  { "en": "Kaki : Berjalan = Tangan : .........", "id": "Memegang" },
  { "en": "Sendok : Makan = Sumpit : .........", "id": "Makan" },
  { "en": "Mawar : Berduri = Kaktus : .........", "id": "Berduri" },
  { "en": "Hadir : Absen = Hidup : .........", "id": "Mati" },
  { "en": "Paus : Mamalia = Komodo : .........", "id": "Reptil" },
  { "en": "Barat : Timur = Utara : .........", "id": "Selatan" },
  { "en": "Semen : Membangun = Lem : .........", "id": "Merekatkan" },
  { "en": "Gerimis : Hujan = Percikan : .........", "id": "Banjir" },
  { "en": "Bata : Tanah Liat = Kaca : .........", "id": "Pasir" },
  { "en": "Hukum : Hakim = Pertandingan : .........", "id": "Wasit" },
  { "en": "Kulit : Gatal = Perut : .........", "id": "Mulas" },
  { "en": "Palu : Tukang = Jarum Suntik : .........", "id": "Suster" },
  { "en": "Jam Tangan : Pergelangan = Cincin : .........", "id": "Jari" },
  { "en": "Rumput : Lapangan = Pasir : .........", "id": "Pantai" },
  { "en": "Rem : Berhenti = Gas : .........", "id": "Maju" },
  { "en": "Olimpiade : Atlet = Oscar : .........", "id": "Aktor" },
  { "en": "Arsitek : Bangunan = Koreografer : .........", "id": "Tarian" },
  { "en": "Tembakau : Rokok = Gandum : .........", "id": "Roti" },
  { "en": "Cinta : Sayang = Benci : .........", "id": "Dengki" },
  { "en": "Pramuka : Tenda = Petani : .........", "id": "Gubuk" },
  { "en": "Fosil : Purbakala = Bintang : .........", "id": "Astronomi" },
  { "en": "Bibir : Gincu = Kuku : .........", "id": "Kuteks" },
  { "en": "Anting : Telinga = Gelang : .........", "id": "Tangan" },
  { "en": "Kuda : Istal = Harimau : .........", "id": "Hutan" },
  { "en": "Berudu : Katak = Belatung : .........", "id": "Lalat" },
  { "en": "Jus : Buah = Sup : .........", "id": "Sayur" },
  { "en": "Lampu Merah : Berhenti = Lampu Hijau : .........", "id": "Jalan" },
  { "en": "Perak : Cincin = Gading : .........", "id": "Pipa" },
  { "en": "Kepala : Pusing = Perut : .........", "id": "Sakit" },
  { "en": "Kuman : Steril = Racun : .........", "id": "Penawar" },
  { "en": "Sopir : Mobil = Delman : .........", "id": "Kusir" },
  { "en": "Mahasiswa : Skripsi = Siswa : .........", "id": "Tugas Akhir" },
  { "en": "Fisika : Energi = Biologi : .........", "id": "Organisme" },
  { "en": "Pesta : Gembira = Pemakaman : .........", "id": "Sedih" },
  { "en": "Mendaki : Gunung = Berlayar : .........", "id": "Lautan" },
  { "en": "Diameter : Lingkaran = Diagonal : .........", "id": "Persegi" },
  { "en": "Merpati : Perdamaian = Timbangan : .........", "id": "Keadilan" },
  { "en": "Penyair : Puisi = Kartunis : .........", "id": "Kartun" },
  { "en": "Riak : Gelombang = Bukit : .........", "id": "Gunung" },
  { "en": "Gemetar : Takut = Berkeringat : .........", "id": "Gugup" },
  { "en": "Paras : Wajah = Figur : .........", "id": "Bentuk" },
  { "en": "Mawar : Merah = Melati : .........", "id": "Putih" },
  { "en": "Tesis : Argumen = Cerita : .........", "id": "Plot" },
  { "en": "Album : Foto = Buku Harian : .........", "id": "Catatan" },
  { "en": "Riak : Air = Angin : .........", "id": "Pohon" },
  { "en": "Koki : Dapur = Petani : .........", "id": "Sawah" },
  { "en": "Es : Air = Air : .........", "id": "Uap" },
  { "en": "Murid : Ujian = Petinju : .........", "id": "Pertandingan" },
  { "en": "Tepuk Tangan : Apresiasi = Siulan : .........", "id": "Ejikan" },
  { "en": "Kertas : Daur Ulang = Sampah Organik : .........", "id": "Kompos" },
  { "en": "Tidur : Mendengkur = Tertawa : .........", "id": "Terbahak" },
  { "en": "Kopi : Pahit = Cokelat : .........", "id": "Manis" },
  { "en": "Piramida : Mesir = Eiffel : .........", "id": "Prancis" },
  { "en": "Cahaya : Fotosintesis = Oksigen : .........", "id": "Respirasi" },
  { "en": "Tato : Kulit = Lukisan : .........", "id": "Dinding" },
  { "en": "Biji : Tunas = Telur : .........", "id": "Embrio" },
  { "en": "Tikus : Kucing = Kelinci : .........", "id": "Rubah" },
  { "en": "Banjir : Air = Badai : .........", "id": "Angin" },
  { "en": "Utara : Kompas = Waktu : .........", "id": "Arloji" },
  { "en": "Kata : Kalimat = Not : .........", "id": "Melodi" },
  { "en": "Manusia : Rumah = Beruang : .........", "id": "Gua" },
  { "en": "Tepung : Kue = Pasir : .........", "id": "Beton" },
  { "en": "Elang : Paruh = Lebah : .........", "id": "Sengat" },
  { "en": "Drama : Babak = Pertunjukan : .........", "id": "Adegan" },
  { "en": "Bintang : Galaksi = Pulau : .........", "id": "Kepulauan" },
  { "en": "Jual : Beli = Ekspor : .........", "id": "Impor" },
  { "en": "Jahil : Usil = Cerdas : .........", "id": "Pandai" },
  { "en": "Api : Padam = Banjir : .........", "id": "Surut" },
  { "en": "Sutra : Ulat = Wol : .........", "id": "Domba" },
  { "en": "Penjaga Gawang : Gawang = Wasit : .........", "id": "Lapangan" },
  { "en": "Kaki : Tendang = Tangan : .........", "id": "Pukul" },
  { "en": "Bantal : Kepala = Selimut : .........", "id": "Tubuh" },
  { "en": "Tuna : Ikan = Catur : .........", "id": "Olahraga" },
  { "en": "Diskon : Potongan Harga = Bonus : .........", "id": "Tambahan" },
  { "en": "Gurun : Kering = Hutan Hujan : .........", "id": "Lembab" },
  { "en": "Kamera : Fotografer = Palet : .........", "id": "Pelukis" },
  { "en": "Akar : Tanah = Jangkar : .........", "id": "Dasar Laut" },
  { "en": "Amnesti : Hukuman = Vaksin : .........", "id": "Penyakit" },
  { "en": "Senar : Gitar = Tuts : .........", "id": "Piano" },
  { "en": "Sel : Jaringan = Atom : .........", "id": "Molekul" },
  { "en": "Bor : Melubangi = Sekop : .........", "id": "Menggali" },
  { "en": "Beras : Petani = Garam : .........", "id": "Petambak" },
  { "en": "Kata : Kamus = Peta : .........", "id": "Atlas" },
  { "en": "Kaca : Rapuh = Baja : .........", "id": "Kuat" },
  { "en": "Konduktor : Tembaga = Isolator : .........", "id": "Karet" },
  { "en": "Mayor : Jenderal = Letnan : .........", "id": "Kapten" },
  { "en": "Tesis : Antitesis = Sintesis : .........", "id": "Kesimpulan" },
  { "en": "Abstrak : Konkret = Acak : .........", "id": "Teratur" },
  { "en": "Medan : Sumatra Utara = Surabaya : .........", "id": "Jawa Timur" },
  { "en": "Presiden : Negara = Gubernur : .........", "id": "Provinsi" },
  { "en": "Iklim : Klimatologi = Cuaca : .........", "id": "Meteorologi" },
  { "en": "Nomaden : Menetap = Sementara : .........", "id": "Permanen" },
  { "en": "Fakta : Data = Opini : .........", "id": "Asumsi" },
  { "en": "Teori : Hipotesis = Hukum : .........", "id": "Pasal" },
  { "en": "Spora : Jamur = Biji : .........", "id": "Pohon" },
  { "en": "Kuda Nil : Herbivora = Buaya : .........", "id": "Karnivora" },
  { "en": "Menara : Tinggi = Sumur : .........", "id": "Dalam" },
  { "en": "Lidah : Pahit = Hidung : .........", "id": "Bau" },
  { "en": "Pandai Besi : Pedang = Penjahit : .........", "id": "Baju" },
  { "en": "Padi : Tikus = Bangkai : .........", "id": "Belatung" },
  { "en": "Fakultas : Universitas = Kelas : .........", "id": "Sekolah" },
  { "en": "Volume : Liter = Massa : .........", "id": "Kilogram" },
  { "en": "Bulu Tangkis : Raket = Tenis Meja : .........", "id": "Bet" },
  { "en": "Banjir : Air Bah = Angin : .........", "id": "Topan" },
  { "en": "Kalender : Tanggal = Peta : .........", "id": "Skala" },
  { "en": "Batu : Keras = Kapas : .........", "id": "Lunak" },
  { "en": "Mufakat : Musyawarah = Keputusan : .........", "id": "Rapat" },
  { "en": "Tawa : Gembira = Dahi Berkerut : .........", "id": "Berpikir" },
  { "en": "Celsius : Suhu = Hektar : .........", "id": "Luas" },
  { "en": "Astronot : Luar Angkasa = Penyelam : .........", "id": "Dasar Laut" },
  { "en": "Stasiun : Kereta = Halte : .........", "id": "Bus" },
  { "en": "Karnivora : Daging = Frugivora : .........", "id": "Buah" },
  { "en": "Ayat : Al-Quran = Pasal : .........", "id": "Undang-Undang" },
  { "en": "Air : Haus = Udara : .........", "id": "Sesak" },
  { "en": "Tato : Seniman Tato = Resep : .........", "id": "Koki" },
  { "en": "Anemia : Darah = Rabun : .........", "id": "Mata" },
  { "en": "Bohlam : Thomas Edison = Telepon : .........", "id": "Alexander Graham Bell" },
  { "en": "Kaya : Miskin = Subur : .........", "id": "Tandus" },
  { "en": "Lezat : Enak = Indah : .........", "id": "Elok" },
  { "en": "Serigala : Auman = Kuda : .........", "id": "Ringkikan" },
  { "en": "Tinta : Pulpen = Cat : .........", "id": "Kuas" },
  { "en": "Batawi : Jakarta = Sunda : .........", "id": "Jawa Barat" },
  { "en": "Kaki Gunung : Puncak = Dasar Laut : .........", "id": "Permukaan" },
  { "en": "Ular : Bisa = Kalajengking : .........", "id": "Racun" },
  { "en": "Oksigen : Bernapas = Makanan : .........", "id": "Energi" },
  { "en": "Sertifikat : Tanah = Ijazah : .........", "id": "Sekolah" },
  { "en": "Komandan : Pasukan = Dirigen : .........", "id": "Orkestra" },
  { "en": "Virus : Penyakit = Pupuk : .........", "id": "Kesuburan" },
  { "en": "Dosen : Mengajar = Mahasiswa : .........", "id": "Belajar" },
  { "en": "Sutera : Halus = Amplas : .........", "id": "Kasar" },
  { "en": "Gletser : Es = Awan : .........", "id": "Uap Air" },
  { "en": "Kurus : Gemuk = Rendah : .........", "id": "Tinggi" },
  { "en": "Raja : Istana = Lebah : .........", "id": "Sarang" },
  { "en": "Perunggu : Logam = Solar : .........", "id": "Bahan Bakar" },
  { "en": "Drama : Aktor = Konser : .........", "id": "Musisi" },
  { "en": "Bunga : Taman = Rempah : .........", "id": "Dapur" },
  { "en": "Lagu : Nada = Skenario : .........", "id": "Dialog" },
  { "en": "Macet : Kendaraan = Antre : .........", "id": "Orang" },
  { "en": "Mendengkur : Tidur = Menguap : .........", "id": "Mengantuk" },
  { "en": "Jari Kaki : Kaki = Jari Tangan : .........", "id": "Tangan" },
  { "en": "Proposal : Proyek = Rencana : .........", "id": "Tindakan" },
  { "en": "Sekam : Padi = Kulit : .........", "id": "Buah" },
  { "en": "Koral : Laut = Kaktus : .........", "id": "Gurun" },
  { "en": "Gergaji : Membelah = Palu : .........", "id": "Memukul" },
  { "en": "Angin : Kincir = Arus : .........", "id": "Turbin" },
  { "en": "Pedas : Cabai = Pahit : .........", "id": "Pare" },
  { "en": "Gelar : Nama = Pangkat : .........", "id": "Jabatan" },
  { "en": "Jangkrik : Serangga = Salmon : .........", "id": "Ikan" },
  { "en": "Jarak : Kilometer = Berat : .........", "id": "Ton" },
  { "en": "Kambing : Mengembik = Katak : .........", "id": "Mendengkung" },
  { "en": "Ritsleting : Celana = Tali : .........", "id": "Sepatu" },
  { "en": "Perpustakaan : Pustakawan = Museum : .........", "id": "Kurator" },
  { "en": "Saham : Investor = Properti : .........", "id": "Pemilik" },
  { "en": "Teka-teki : Misteri = Pertanyaan : .........", "id": "Jawaban" },
  { "en": "Uang Muka : Pembelian = Pendahuluan : .........", "id": "Buku" },
  { "en": "Raut : Wajah = Postur : .........", "id": "Tubuh" },
  { "en": "Dahaga : Air = Kelaparan : .........", "id": "Pangan" },
  { "en": "Kambium : Pohon = Sumsum : .........", "id": "Tulang" },
  { "en": "Elastis : Karet = Kaku : .........", "id": "Besi" },
  { "en": "Rontgen : Sinar-X = USG : .........", "id": "Suara Ultrasonik" },
  { "en": "Vertikal : Tegak = Horizontal : .........", "id": "Mendatar" },
  { "en": "Antibiotik : Bakteri = Pestisida : .........", "id": "Hama" },
  { "en": "Berlian : Intan = Permata : .........", "id": "Batu Mulia" },
  { "en": "Bunglon : Mimikri = Paus : .........", "id": "Ekolokasi" },
  { "en": "Sarjana : Universitas = Hafiz : .........", "id": "Pesantren" },
  { "en": "Candu : Ketergantungan = Vitamin : .........", "id": "Kesehatan" },
  { "en": "Maju : Mundur = Datang : .........", "id": "Pergi" },
  { "en": "Penyu : Tempurung = Siput : .........", "id": "Cangkang" },
  { "en": "Skor : Pertandingan = Nilai : .........", "id": "Ujian" },
  { "en": "Kepulauan : Pulau = Gugusan : .........", "id": "Bintang" },
  { "en": "Insang : Ikan = Trakea : .........", "id": "Serangga" },
  { "en": "Pintu : Engsel = Lengan : .........", "id": "Siku" },
  { "en": "Individual : Kelompok = Pribadi : .........", "id": "Publik" },
  { "en": "Medali : Pemenang = Piala : .........", "id": "Juara" },
  { "en": "Tuna : Laut = Mujair : .........", "id": "Air Tawar" },
  { "en": "Matahari : Pusat = Planet : .........", "id": "Mengorbit" },
  { "en": "Kawah : Gunung Api = Palung : .........", "id": "Lautan" },
  { "en": "Debit : Air = Kecepatan : .........", "id": "Kendaraan" },
  { "en": "Watt : Daya = Ohm : .........", "id": "Hambatan" },
  { "en": "Legislatif : DPR = Eksekutif : .........", "id": "Presiden" },
  { "en": "Arkeolog : Artefak = Filatelis : .........", "id": "Perangko" },
  { "en": "Unta : Punuk = Jerapah : .........", "id": "Leher Panjang" },
  { "en": "Latihan : Keterampilan = Investasi : .........", "id": "Keuntungan" },
  { "en": "Filipina : Manila = Kanada : .........", "id": "Ottawa" },
  { "en": "Sporadis : Jarang = Rutin : .........", "id": "Teratur" },
  { "en": "Skeptis : Ragu = Optimis : .........", "id": "Yakin" },
  { "en": "Mikser : Mengaduk = Setrika : .........", "id": "Melicinkan" },
  { "en": "Benang : Kain = Kata : .........", "id": "Paragraf" },
  { "en": "Protagonis : Tokoh Utama = Antagonis : .........", "id": "Tokoh Lawan" },
  { "en": "Rima : Puisi = Ritme : .........", "id": "Musik" },
  { "en": "Fertilisasi : Embrio = Evaporasi : .........", "id": "Uap" },
  { "en": "Menabung : Hemat = Boros : .........", "id": "Menghamburkan" },
  { "en": "Anggur : Kebun = Arsip : .........", "id": "Kantor" },
  { "en": "Nokturnal : Malam = Diurnal : .........", "id": "Siang" },
  { "en": "Mendengarkan : Suara = Menyaksikan : .........", "id": "Peristiwa" },
  { "en": "Kepompong : Kupu-kupu = Benih : .........", "id": "Tanaman" },
  { "en": "Apotek : Obat = Perpustakaan : .........", "id": "Buku" },
  { "en": "Air : Bendungan = Uang : .........", "id": "Bank" },
  { "en": "Piramida : Segitiga = Kubus : .........", "id": "Persegi" },
  { "en": "Platina : Logam = Anggrek : .........", "id": "Bunga" },
  { "en": "Insomnia : Tidur = Afasia : .........", "id": "Berbicara" },
  { "en": "Gempa Bumi : Seismograf = Suhu Tubuh : .........", "id": "Termometer" },
  { "en": "Kuda : Merumput = Elang : .........", "id": "Memangsa" },
  { "en": "Sandal : Kaki = Topi : .........", "id": "Kepala" },
  { "en": "Statis : Diam = Dinamis : .........", "id": "Bergerak" },
  { "en": "Kurator : Seni = Manajer : .........", "id": "Tim" },
  { "en": "Petinju : Ring = Pembalap : .........", "id": "Sirkuit" },
  { "en": "Gandum : Roti = Kedelai : .........", "id": "Tahu" },
  { "en": "Tuna : Sirip = Kelelawar : .........", "id": "Sayap" },
  { "en": "Perkenalan : Nama = Pembukaan : .........", "id": "Pidato" },
  { "en": "Dahulu : Sekarang = Masa Lalu : .........", "id": "Masa Kini" },
  { "en": "Seri : Imbang = Menang : .........", "id": "Kalah" },
  { "en": "Berasal : Asli = Tiruan : .........", "id": "Palsu" },
  { "en": "Mata : Retina = Otak : .........", "id": "Korteks" },
  { "en": "Kering : Gurun = Lembab : .........", "id": "Rawa" },
  { "en": "Anjing Laut : Amfibi = Manusia : .........", "id": "Mamalia" },
  { "en": "Judul : Buku = Tajuk : .........", "id": "Artikel" },
  { "en": "Baja : Keras = Spons : .........", "id": "Lunak" },
  { "en": "Jari : Kuku = Mata : .........", "id": "Bulu Mata" },
  { "en": "Laba : Penjualan = Gaji : .........", "id": "Pekerjaan" },
  { "en": "Pena : Tinta = Kuas : .........", "id": "Cat" },
  { "en": "Akselerasi : Percepatan = Deselerasi : .........", "id": "Perlambatan" },
  { "en": "Bintang : Astronomi = Batuan : .........", "id": "Geologi" },
  { "en": "Pintu : Masuk = Jendela : .........", "id": "Ventilasi" },
  { "en": "Otoriter : Diktator = Demokratis : .........", "id": "Presiden" },
  { "en": "Putih : Bersih = Hitam : .........", "id": "Kotor" },
  { "en": "Sarang : Burung = Kandang : .........", "id": "Ternak" },
  { "en": "Busur : Panah = Senapan : .........", "id": "Peluru" },
  { "en": "Kolonel : Letnan Kolonel = Sersan Mayor : .........", "id": "Sersan Kepala" },
  { "en": "Kertas : Sobek = Kaca : .........", "id": "Pecah" },
  { "en": "Televisi : Menonton = Radio : .........", "id": "Mendengar" },
  { "en": "Legal : Sah = Ilegal : .........", "id": "Haram" },
  { "en": "Mawar : Wangi = Durian : .........", "id": "Menyengat" },
  { "en": "Air : Mengalir = Api : .........", "id": "Menjalar" },
  { "en": "Tawa : Lucu = Teriakan : .........", "id": "Takut" },
  { "en": "Kawah : Vulkanik = Delta : .........", "id": "Sungai" },
  { "en": "Pialang : Saham = Notaris : .........", "id": "Akta" },
  { "en": "Rel : Kereta Api = Aspal : .........", "id": "Mobil" },
  { "en": "Telinga : Anting = Leher : .........", "id": "Kalung" },
  { "en": "Landak : Duri = Badak : .........", "id": "Cula" },
  { "en": "Domba : Wol = Kepompong : .........", "id": "Sutra" },
  { "en": "Kuman : Antiseptik = Api : .........", "id": "Air" },
  { "en": "Kaki : Sepak Bola = Tangan : .........", "id": "Bola Basket" },
  { "en": "Hutan : Pohon = Lautan : .........", "id": "Air" },
  { "en": "Kopi : Gula = Sup : .........", "id": "Garam" },
  { "en": "Kecap : Manis = Cuka : .........", "id": "Asam" },
  { "en": "Kuliah : Dosen = Sekolah Dasar : .........", "id": "Guru" },
  { "en": "Sutra : Ulat Sutra = Mutiara : .........", "id": "Tiram" },
  { "en": "Dataran Rendah : Pantai = Dataran Tinggi : .........", "id": "Pegunungan" },
  { "en": "Lensa : Kamera = Layar : .........", "id": "Ponsel" },
  { "en": "Tawon : Sengat = Nyamuk : .........", "id": "Gigit" },
  { "en": "Pondasi : Bangunan = Akar : .........", "id": "Pohon" },
  { "en": "Perpustakaan : Membaca = Laboratorium : .........", "id": "Meneliti" },
  { "en": "Jeda : Istirahat = Lanjut : .........", "id": "Meneruskan" },
  { "en": "Biji : Buah = Embrio : .........", "id": "Janin" },
  { "en": "Lirik : Lagu = Resep : .........", "id": "Masakan" },
  { "en": "Garis : Bidang = Huruf : .........", "id": "Kata" },
  { "en": "Kamera : Gambar = Mikrofon : .........", "id": "Suara" },
  { "en": "Kucing : Mamalia = Katak : .........", "id": "Amfibi" },
  { "en": "Maju : Progres = Mundur : .........", "id": "Regres" },
  { "en": "Satu : Tunggal = Dua : .........", "id": "Ganda" },
  { "en": "Berbisik : Berbicara = Berjalan : .........", "id": "Berlari" },
  { "en": "Yudikatif : Mahkamah Agung = Moneter : .........", "id": "Bank Sentral" },
  { "en": "Kontraktor : Proyek = Produser : .........", "id": "Film" },
  { "en": "Bingkai : Lukisan = Sampul : .........", "id": "Buku" },
  { "en": "Bumi : Geotermal = Matahari : .........", "id": "Surya" },
  { "en": "Klub : Anggota = Federasi : .........", "id": "Negara" },
  { "en": "Paragraf : Esai = Adegan : .........", "id": "Film" },
  { "en": "Gajah : Belalai = Kanguru : .........", "id": "Kantong" },
  { "en": "Pagi : Fajar = Malam : .........", "id": "Senja" },
  { "en": "Cair : Menguap = Padat : .........", "id": "Mencair" },
  { "en": "Izin : Masuk = Tiket : .........", "id": "Menonton" },
  { "en": "Pohon : Tumbang = Tembok : .........", "id": "Runtuh" },
  { "en": "Penjara : Kejahatan = Sekolah : .........", "id": "Pendidikan" },
  { "en": "Apresiasi : Pujian = Kritik : .........", "id": "Celaan" },
  { "en": "Konstitusi : Negara = Anggaran Dasar : .........", "id": "Organisasi" },
  { "en": "Paspor : Negara = KTP : .........", "id": "Warga Negara" },
  { "en": "Mawar : Flora = Ayam : .........", "id": "Fauna" },
  { "en": "Kardiolog : Jantung = Nefrolog : .........", "id": "Ginjal" },
  { "en": "Grasi : Pengampunan = Amnesti : .........", "id": "Penghapusan Hukuman" },
  { "en": "Inflasi : Harga Naik = Resesi : .........", "id": "Ekonomi Lesu" },
  { "en": "Onomatope : Tiruan Bunyi = Personifikasi : .........", "id": "Penginsanan" },
  { "en": "Skala Richter : Gempa = Skala Mohs : .........", "id": "Kekerasan Mineral" },
  { "en": "Fermentasi : Ragi = Fotosintesis : .........", "id": "Klorofil" },
  { "en": "Kawanan : Gajah = Armada : .........", "id": "Kapal" },
  { "en": "Berlayar : Badai = Belajar : .........", "id": "Gangguan" },
  { "en": "DNA : Heliks Ganda = Bumi : .........", "id": "Bulat Pepat" },
  { "en": "Proklamasi : Kemerdekaan = Pemilu : .........", "id": "Wakil Rakyat" },
  { "en": "Merebus : Air = Menggoreng : .........", "id": "Minyak" },
  { "en": "Belanda : Kincir Angin = Swiss : .........", "id": "Pegunungan Alpen" },
  { "en": "Patah Tulang : Gips = Luka : .........", "id": "Perban" },
  { "en": "Suap : Korupsi = Fitnah : .........", "id": "Pencemaran Nama Baik" },
  { "en": "Valuta : Mata Uang = Bursa : .........", "id": "Saham" },
  { "en": "Aliterasi : Konsonan = Asonansi : .........", "id": "Vokal" },
  { "en": "Barometer : Tekanan Udara = Higrometer : .........", "id": "Kelembaban" },
  { "en": "Distilasi : Penyulingan = Oksidasi : .........", "id": "Pengkaratan" },
  { "en": "Gerombolan : Pencuri = Komplotan : .........", "id": "Penjahat" },
  { "en": "Mendaki : Jurang = Mengetik : .........", "id": "Salah Eja" },
  { "en": "Sel : Membran = Telur : .........", "id": "Cangkang" },
  { "en": "Resolusi : Konflik = Negosiasi : .........", "id": "Kesepakatan" },
  { "en": "Memanggang : Api = Menjemur : .........", "id": "Matahari" },
  { "en": "Australia : Kanguru = Tiongkok : .........", "id": "Panda" },
  { "en": "Dermatolog : Kulit = Psikiater : .........", "id": "Jiwa" },
  { "en": "Kasasi : Mahkamah Agung = Banding : .........", "id": "Pengadilan Tinggi" },
  { "en": "Ekspor : Keluar = Impor : .........", "id": "Masuk" },
  { "en": "Metafora : Kiasan = Hiperbola : .........", "id": "Berlebihan" },
  { "en": "Anemometer : Kecepatan Angin = Spektrometer : .........", "id": "Panjang Gelombang" },
  { "en": "Transpirasi : Tumbuhan = Evaporasi : .........", "id": "Air Laut" },
  { "en": "Resimen : Tentara = Korps : .........", "id": "Mahasiswa" },
  { "en": "Berpikir : Ragu = Bertindak : .........", "id": "Risiko" },
  { "en": "Jantung : Atrium = Otak : .........", "id": "Serebrum" },
  { "en": "Vonis : Pengadilan = Ijazah : .........", "id": "Lembaga Pendidikan" },
  { "en": "Memahat : Patung = Menganyam : .........", "id": "Keranjang" },
  { "en": "India : Sungai Gangga = Mesir : .........", "id": "Sungai Nil" },
  { "en": "Obat : Resep = Hidangan : .........", "id": "Menu" },
  { "en": "Plagiarisme : Karya Tulis = Pembajakan : .........", "id": "Perangkat Lunak" },
  { "en": "Dividen : Laba = Bunga : .........", "id": "Simpanan" },
  { "en": "Sinopsis : Cerita = Abstrak : .........", "id": "Penelitian" },
  { "en": "Galon : Volume = Knot : .........", "id": "Kecepatan Laut" },
  { "en": "Elektrolisis : Listrik = Pelapukan : .........", "id": "Cuaca" },
  { "en": "Grup : Penyanyi = Tim : .........", "id": "Pemain" },
  { "en": "Terbang : Gravitasi = Berenang : .........", "id": "Arus" },
  { "en": "Arteri : Darah Bersih = Vena : .........", "id": "Darah Kotor" },
  { "en": "Reformasi : Perubahan = Kudeta : .........", "id": "Penggulingan Kekuasaan" },
  { "en": "Menyulam : Benang = Membatik : .........", "id": "Lilin" },
  { "en": "Spanyol : Matador = Jepang : .........", "id": "Sumo" },
  { "en": "Antibodi : Imun = Hormon : .........", "id": "Pertumbuhan" },
  { "en": "Terdakwa : Pengacara = Pasien : .........", "id": "Dokter" },
  { "en": "Deflasi : Harga Turun = Devaluasi : .........", "id": "Nilai Mata Uang Turun" },
  { "en": "Glosarium : Istilah = Indeks : .........", "id": "Kata Kunci" },
  { "en": "Joule : Energi = Newton : .........", "id": "Gaya" },
  { "en": "Reduksi : Pengurangan = Oksidasi : .........", "id": "Penambahan Oksigen" },
  { "en": "Satu Lusin : 12 = Satu Kodi : .........", "id": "20" },
  { "en": "Berlari : Cedera = Makan : .........", "id": "Tersedak" },
  { "en": "Neuron : Saraf = Kapiler : .........", "id": "Darah" },
  { "en": "UU : Disahkan = Perjanjian : .........", "id": "Diratifikasi" },
  { "en": "Mencetak : Printer = Memindai : .........", "id": "Scanner" },
  { "en": "Brasil : Kopi = Malaysia : .........", "id": "Kelapa Sawit" },
  { "en": "Katarak : Mata = Osteoporosis : .........", "id": "Tulang" },
  { "en": "Saksi : Fakta = Ahli : .........", "id": "Opini" },
  { "en": "Pajak : Negara = Iuran : .........", "id": "Organisasi" },
  { "en": "Bibliografi : Referensi = Catatan Kaki : .........", "id": "Sumber Kutipan" },
  { "en": "Pascal : Tekanan = Hertz : .........", "id": "Frekuensi" },
  { "en": "Abrasi : Ombak = Erosi : .........", "id": "Angin" },
  { "en": "Satu Rim : 500 = Satu Gross : .........", "id": "144" },
  { "en": "Berdebat : Argumen = Bertanding : .........", "id": "Strategi" },
  { "en": "Enzim : Katalis = Adrenalin : .........", "id": "Stimulan" },
  { "en": "Embargo : Perdagangan = Boikot : .........", "id": "Partisipasi" },
  { "en": "Mengetik : Keyboard = Menggambar : .........", "id": "Pensil" },
  { "en": "Finlandia : Nokia = Korea Selatan : .........", "id": "Samsung" },
  { "en": "Apendisitis : Usus Buntu = Gastritis : .........", "id": "Lambung" },
  { "en": "Terdakwa : Jaksa = Atlet : .........", "id": "Lawan" },
  { "en": "Subsidi : Bantuan = Proteksi : .........", "id": "Perlindungan" },
  { "en": "Premis : Kesimpulan = Sebab : .........", "id": "Akibat" },
  { "en": "Lumen : Intensitas Cahaya = Candela : .........", "id": "Kekuatan Cahaya" },
  { "en": "Kondensasi : Embun = Presipitasi : .........", "id": "Hujan" },
  { "en": "Keluarga : Ayah = Kerajaan : .........", "id": "Raja" },
  { "en": "Berbisnis : Rugi = Berperang : .........", "id": "Kalah" },
  { "en": "Kromosom : Gen = Buku : .........", "id": "Informasi" },
  { "en": "Investigasi : Penyelidik = Audit : .........", "id": "Auditor" },
  { "en": "Memutar : Kunci = Menggesek : .........", "id": "Kartu" },
  { "en": "Arab Saudi : Minyak Bumi = Jepang : .........", "id": "Teknologi" },
  { "en": "Diabetes : Gula = Hipertensi : .........", "id": "Garam" },
  { "en": "Tergugat : Penggugat = Pembeli : .........", "id": "Penjual" },
  { "en": "Barter : Barang = Jual Beli : .........", "id": "Uang" },
  { "en": "Hipotesis : Penelitian = Draf : .........", "id": "Naskah" },
  { "en": "Kalori : Energi = Ampere : .........", "id": "Arus Listrik" },
  { "en": "Meander : Sungai = Bukit Pasir : .........", "id": "Gurun" },
  { "en": "Parlemen : Anggota = Kabinet : .........", "id": "Menteri" },
  { "en": "Memuji : Sanjungan = Menghina : .........", "id": "Cacian" },
  { "en": "Vaksin : Kekebalan = Nutrisi : .........", "id": "Kesehatan" },
  { "en": "Mosi : Parlemen = Amendemen : .........", "id": "Konstitusi" },
  { "en": "Mosaik : Kepingan = Kolase : .........", "id": "Potongan" },
  { "en": "Italia : Pizza = Meksiko : .........", "id": "Taco" },
  { "en": "Gajah Mada : Majapahit = Soekarno : .........", "id": "Indonesia" },
  { "en": "Biola : Gesek = Trompet : .........", "id": "Tiup" },
  { "en": "Helium : Ringan = Timbal : .........", "id": "Berat" },
  { "en": "Kolom : Menopang = Atap : .........", "id": "Melindungi" },
  { "en": "Kuda : Anak Kuda = Domba : .........", "id": "Anak Domba" },
  { "en": "Bertapa : Gua = Bertani : .........", "id": "Ladang" },
  { "en": "Bel : Berdering = Guntur : .........", "id": "Menggelegar" },
  { "en": "Kakek : Cucu = Paman : .........", "id": "Keponakan" },
  { "en": "Penjumlahan : Total = Pengurangan : .........", "id": "Selisih" },
  { "en": "Epik : Pahlawan = Fabel : .........", "id": "Hewan" },
  { "en": "Putih : Suci = Emas : .........", "id": "Kemewahan" },
  { "en": "Alis : Mengerut = Bibir : .........", "id": "Tersenyum" },
  { "en": "Revolusi : Perubahan Cepat = Evolusi : .........", "id": "Perubahan Lambat" },
  { "en": "Kuintet : Lima = Trio : .........", "id": "Tiga" },
  { "en": "Besi : Berkarat = Roti : .........", "id": "Berjamur" },
  { "en": "Balkon : Bangunan = Dek : .........", "id": "Kapal" },
  { "en": "Angsa : Berkelompok = Macan Tutul : .........", "id": "Soliter" },
  { "en": "Beribadah : Masjid = Berjudi : .........", "id": "Kasino" },
  { "en": "Sirene : Bahaya = Peluit : .........", "id": "Peringatan" },
  { "en": "Majikan : Bawahan = Jenderal : .........", "id": "Prajurit" },
  { "en": "Perkalian : Hasil Kali = Pembagian : .........", "id": "Hasil Bagi" },
  { "en": "Mitos : Dewa = Legenda : .........", "id": "Tokoh Sejarah" },
  { "en": "Ungu : Bangsawan = Merah Jambu : .........", "id": "Feminin" },
  { "en": "Mata : Berkedip = Jantung : .........", "id": "Berdetak" },
  { "en": "Pemberontakan : Melawan = Pengabdian : .........", "id": "Melayani" },
  { "en": "Piano : Ditekan = Drum : .........", "id": "Dipukul" },
  { "en": "Natrium : Garam = Kalsium : .........", "id": "Susu" },
  { "en": "Tangga : Lantai = Eskalator : .........", "id": "Tingkat" },
  { "en": "Ayam : Bertelur = Sapi : .........", "id": "Beranak" },
  { "en": "Observasi : Laboratorium = Ekskavasi : .........", "id": "Situs Arkeologi" },
  { "en": "Gong : Dipukul = Angklung : .........", "id": "Digoyangkan" },
  { "en": "Suami : Istri = Kakak : .........", "id": "Adik" },
  { "en": "Sudut : Derajat = Frekuensi : .........", "id": "Hertz" },
  { "en": "Balada : Kisah Sedih = Komedi : .........", "id": "Kisah Lucu" },
  { "en": "Oranye : Semangat = Biru : .........", "id": "Ketenangan" },
  { "en": "Dahi : Mengerut = Pipi : .........", "id": "Merona" },
  { "en": "Modernisasi : Baru = Tradisi : .........", "id": "Lama" },
  { "en": "Harpa : Dipetik = Harmonika : .........", "id": "Ditiup" },
  { "en": "Hidrogen : Air = Karbon : .........", "id": "Gula" },
  { "en": "Jalan Raya : Mobil = Rel : .........", "id": "Lokomotif" },
  { "en": "Singa : Anak Singa = Ikan : .........", "id": "Benih" },
  { "en": "Rapat : Ruang Rapat = Sidang : .........", "id": "Ruang Sidang" },
  { "en": "Klakson : Mobil = Lonceng : .........", "id": "Gereja" },
  { "en": "Tuan Rumah : Tamu = Produsen : .........", "id": "Konsumen" },
  { "en": "Faktor : Perkalian = Suku : .........", "id": "Deret" },
  { "en": "Horor : Menakutkan = Romansa : .........", "id": "Cinta" },
  { "en": "Cokelat : Energi = Abu-abu : .........", "id": "Kemonotonan" },
  { "en": "Bahu : Mengedik = Hidung : .........", "id": "Mengendus" },
  { "en": "Kolonialisme : Penjajahan = Nasionalisme : .........", "id": "Kebangsaan" },
  { "en": "Orkestra : Konduktor = Paduan Suara : .........", "id": "Dirigen" },
  { "en": "Emas : Konduktif = Plastik : .........", "id": "Isolator" },
  { "en": "Panggung : Aktor = Mimbar : .........", "id": "Penceramah" },
  { "en": "Buaya : Melata = Kanguru : .........", "id": "Melompat" },
  { "en": "Turnamen : Olahraga = Resital : .........", "id": "Musik" },
  { "en": "Jam : Berdetik = Hujan : .........", "id": "Turun" },
  { "en": "Dosen : Mahasiswa = Pelatih : .........", "id": "Atlet" },
  { "en": "Akar : Kuadrat = Turunan : .........", "id": "Integral" },
  { "en": "Sains Fiksi : Masa Depan = Sejarah : .........", "id": "Masa Lalu" },
  { "en": "Perak : Elegan = Hijau : .........", "id": "Alami" },
  { "en": "Lutut : Menekuk = Leher : .........", "id": "Menoleh" },
  { "en": "Penemuan : Ilmuwan = Inovasi : .........", "id": "Inovator" },
  { "en": "Gamelan : Dipukul = Kecapi : .........", "id": "Dipetik" },
  { "en": "Yodium : Tiroid = Zat Besi : .........", "id": "Hemoglobin" },
  { "en": "Auditorium : Pertunjukan = Galeri : .........", "id": "Pameran" },
  { "en": "Bebek : Berjalan = Ular : .........", "id": "Merayap" },
  { "en": "Audisi : Peran = Wawancara : .........", "id": "Pekerjaan" },
  { "en": "Telepon : Berdering = Alarm : .........", "id": "Berbunyi" },
  { "en": "Penjual : Pembeli = Guru : .........", "id": "Murid" },
  { "en": "Variabel : Persamaan = Kata Sifat : .........", "id": "Frasa" },
  { "en": "Misteri : Detektif = Petualangan : .........", "id": "Penjelajah" },
  { "en": "Perunggu : Kemenangan = Zaitun : .........", "id": "Perdamaian" },
  { "en": "Bibir : Tersenyum = Tangan : .........", "id": "Melambai" },
  { "en": "Urbanisasi : Kota = Transmigrasi : .........", "id": "Daerah Lain" },
  { "en": "Band : Vokalis = Tim Sepak Bola : .........", "id": "Kapten" },
  { "en": "Nitrogen : Udara = Silikon : .........", "id": "Kerak Bumi" },
  { "en": "Kokpit : Pilot = Ruang Kontrol : .........", "id": "Operator" },
  { "en": "Kucing : Berjalan Kaki = Elang : .........", "id": "Terbang" },
  { "en": "Lelang : Penawaran = Kompetisi : .........", "id": "Persaingan" },
  { "en": "Angin : Meniup = Ombak : .........", "id": "Menghempas" },
  { "en": "Hakim : Terdakwa = Dokter : .........", "id": "Pasien" },
  { "en": "Pembilang : Penyebut = Lintang : .........", "id": "Bujur" },
  { "en": "Parodi : Mengejek = Satir : .........", "id": "Menyindir" },
  { "en": "Merah Putih : Berani Suci = Kuning : .........", "id": "Hati-hati" },
  { "en": "Jempol : Menyetujui = Telunjuk : .........", "id": "Menunjuk" },
  { "en": "Globalisasi : Dunia = Lokalisasi : .........", "id": "Daerah" },
  { "en": "Solo : Satu = Duet : .........", "id": "Dua" },
  { "en": "Raksa : Cair = Berlian : .........", "id": "Padat" },
  { "en": "Hanggar : Pesawat = Dok : .........", "id": "Kapal" },
  { "en": "Katak : Melompat = Monyet : .........", "id": "Memanjat" },
  { "en": "Debut : Penampilan Pertama = Final : .........", "id": "Penampilan Terakhir" },
  { "en": "Api : Berkobar = Air : .........", "id": "Mendidih" },
  { "en": "Editor : Naskah = Kurator : .........", "id": "Karya Seni" },
  { "en": "Garis Lurus : Terdekat = Kurva : .........", "id": "Melengkung" },
  { "en": "Biografi : Riwayat Hidup = Otobiografi : .........", "id": "Riwayat Hidup Sendiri" },
  { "en": "Hitam : Duka = Jingga : .........", "id": "Keceriaan" },
  { "en": "Kepala : Mengangguk = Tangan : .........", "id": "Berjabat" },
  { "en": "Thor : Palu = Zeus : .........", "id": "Petir" },
  { "en": "Xilem : Mengangkut Air = Floem : .........", "id": "Mengangkut Makanan" },
  { "en": "Magma : Gunung Berapi = Air Tanah : .........", "id": "Mata Air" },
  { "en": "RAM : Memori Sementara = Hardisk : .........", "id": "Memori Permanen" },
  { "en": "Merek : Identitas = Slogan : .........", "id": "Tagline" },
  { "en": "Barista : Kopi = Sommelier : .........", "id": "Anggur" },
  { "en": "Iris : Mengatur Pupil = Diafragma : .........", "id": "Mengatur Pernapasan" },
  { "en": "Alibi : Bukti Ketidakhadiran = Motif : .........", "id": "Alasan Tindakan" },
  { "en": "Gemirisik : Daun Kering = Gemericik : .........", "id": "Air Mengalir" },
  { "en": "Fasad : Depan Bangunan = Buritan : .........", "id": "Belakang Kapal" },
  { "en": "Membaca : Penglihatan = Mencicipi : .........", "id": "Pengecapan" },
  { "en": "Simile : Bagaikan = Metafora : .........", "id": "Kiasan Langsung" },
  { "en": "Nyi Roro Kidul : Laut Selatan = Dewa Ruci : .........", "id": "Samudra" },
  { "en": "Stomata : Pertukaran Gas = Alveolus : .........", "id": "Pertukaran Udara" },
  { "en": "Stalaktit : Langit-langit Gua = Stalagmit : .........", "id": "Lantai Gua" },
  { "en": "CPU : Otak Komputer = Motherboard : .........", "id": "Tulang Punggung" },
  { "en": "Pemasaran : Penjualan = Humas : .........", "id": "Citra Perusahaan" },
  { "en": "Koki : Pisau = Bartender : .........", "id": "Shaker" },
  { "en": "Tendon : Otot ke Tulang = Ligamen : .........", "id": "Tulang ke Tulang" },
  { "en": "Precedent : Keputusan Terdahulu = Yurisprudensi : .........", "id": "Ilmu Hukum" },
  { "en": "Denting : Logam = Derit : .........", "id": "Kayu" },
  { "en": "Kubah : Atap Melengkung = Arkade : .........", "id": "Lorong Beratap" },
  { "en": "Meraba : Perabaan = Menghirup : .........", "id": "Penciuman" },
  { "en": "Ironi : Bertentangan = Sarkasme : .........", "id": "Sindiran Tajam" },
  { "en": "Poseidon : Laut = Hades : .........", "id": "Dunia Bawah" },
  { "en": "Kloroplas : Fotosintesis = Mitokondria : .........", "id": "Respirasi Sel" },
  { "en": "Delta : Muara Sungai = Estuari : .........", "id": "Percampuran Air Tawar dan Asin" },
  { "en": "Perangkat Lunak : Aplikasi = Perangkat Keras : .........", "id": "Monitor" },
  { "en": "Akuisisi : Pengambilalihan = Merger : .........", "id": "Penggabungan" },
  { "en": "Sake : Jepang = Soju : .........", "id": "Korea" },
  { "en": "Kornea : Melindungi Mata = Sklera : .........", "id": "Bagian Putih Mata" },
  { "en": "Juri : Putusan = Arbiter : .........", "id": "Arbitrase" },
  { "en": "Gemerincing : Lonceng = Desis : .........", "id": "Ular" },
  { "en": "Aqueduct : Saluran Air = Viaduct : .........", "id": "Jembatan di Atas Lembah" },
  { "en": "Bertepuk : Pendengaran = Mengelus : .........", "id": "Perasaan" },
  { "en": "Eufemisme : Penghalusan Kata = Pleonasme : .........", "id": "Berlebihan" },
  { "en": "Anubis : Kematian = Ra : .........", "id": "Matahari" },
  { "en": "Vakuola : Cadangan Makanan = Ribosom : .........", "id": "Sintesis Protein" },
  { "en": "Garis Lintang : Horizontal = Garis Bujur : .........", "id": "Vertikal" },
  { "en": "Piksel : Gambar Digital = Titik : .........", "id": "Garis" },
  { "en": "Waralaba : Bisnis = Lisensi : .........", "id": "Hak Penggunaan" },
  { "en": "Kimchi : Makanan = Teh Hijau : .........", "id": "Minuman" },
  { "en": "Retina : Menangkap Cahaya = Gendang Telinga : .........", "id": "Menangkap Getaran" },
  { "en": "Gugatan : Perdata = Tuntutan : .........", "id": "Pidana" },
  { "en": "Letupan : Balon = Dentuman : .........", "id": "Meriam" },
  { "en": "Obelisk : Monumen Tinggi = Pagoda : .........", "id": "Kuil Bertingkat" },
  { "en": "Tersenyum : Bibir = Mengernyit : .........", "id": "Dahi" },
  { "en": "Klimaks : Puncak Cerita = Antiklimaks : .........", "id": "Penurunan Cerita" },
  { "en": "Ganesha : Ilmu Pengetahuan = Wisnu : .........", "id": "Pemelihara" },
  { "en": "Autotrof : Membuat Makanan Sendiri = Heterotrof : .........", "id": "Mendapatkan Makanan dari Luar" },
  { "en": "Ekuator : Tengah Bumi = Kutub : .........", "id": "Ujung Bumi" },
  { "en": "URL : Alamat Web = IP Address : .........", "id": "Alamat Jaringan" },
  { "en": "Retail : Eceran = Grosir : .........", "id": "Partai Besar" },
  { "en": "Paella : Spanyol = Risotto : .........", "id": "Italia" },
  { "en": "Koklea : Rumah Siput Telinga = Vestibulum : .........", "id": "Keseimbangan" },
  { "en": "Advokat : Membela = Mediator : .........", "id": "Menengahi" },
  { "en": "Deru : Mesin = Kicau : .........", "id": "Burung" },
  { "en": "Menara : Pengawas = Mercusuar : .........", "id": "Pemandu Kapal" },
  { "en": "Melotot : Mata = Menggertak : .........", "id": "Gigi" },
  { "en": "Paradoks : Kontradiktif = Oksimoron : .........", "id": "Dua Kata Berlawanan" },
  { "en": "Arjuna : Panah = Bima : .........", "id": "Gada" },
  { "en": "Simbiosis : Interaksi Makhluk Hidup = Mutualisme : .........", "id": "Saling Menguntungkan" },
  { "en": "Tornado : Darat = Topan : .........", "id": "Laut" },
  { "en": "Firewall : Keamanan Jaringan = Antivirus : .........", "id": "Keamanan Komputer" },
  { "en": "Aset : Harta = Liabilitas : .........", "id": "Utang" },
  { "en": "Cuka : Asam = Soda Kue : .........", "id": "Basa" },
  { "en": "Pankreas : Insulin = Kelenjar Adrenal : .........", "id": "Adrenalin" },
  { "en": "Terdakwa : Tergugat = Jaksa : .........", "id": "Penggugat" },
  { "en": "Mengaum : Singa = Melenguh : .........", "id": "Sapi" },
  { "en": "Benteng : Pertahanan = Jembatan : .........", "id": "Penyeberangan" },
  { "en": "Menggigil : Dingin = Pingsan : .........", "id": "Syok" },
  { "en": "Alegori : Simbol = Parabel : .........", "id": "Pelajaran Moral" },
  { "en": "Srikandi : Pemanah = Gatotkaca : .........", "id": "Terbang" },
  { "en": "Parasitisme : Merugikan Sebelah = Komensalisme : .........", "id": "Satu Untung, Satu Tidak Rugi" },
  { "en": "Musim : Iklim = Fase : .........", "id": "Bulan" },
  { "en": "Cache : Mempercepat Akses = Cookie : .........", "id": "Menyimpan Informasi Pengguna" },
  { "en": "Ekuitas : Modal Sendiri = Obligasi : .........", "id": "Surat Utang" },
  { "en": "Sushi : Ikan Mentah = Steak : .........", "id": "Daging Panggang" },
  { "en": "Hipofisis : Kelenjar Master = Tiroid : .........", "id": "Metabolisme" },
  { "en": "Remisi : Pengurangan Hukuman = Abolisi : .........", "id": "Penghapusan Tuntutan" },
  { "en": "Berdecak : Kagum = Mendengus : .........", "id": "Menghina" },
  { "en": "Tiang : Bendera = Gantungan : .........", "id": "Baju" },
  { "en": "Berteriak : Marah = Berbisik : .........", "id": "Rahasia" },
  { "en": "Repetisi : Pengulangan = Gradasi : .........", "id": "Peningkatan Bertahap" },
  { "en": "Afrodit : Cinta = Ares : .........", "id": "Perang" },
  { "en": "Rantai Makanan : Aliran Energi = Jaring Makanan : .........", "id": "Gabungan Rantai Makanan" },
  { "en": "Atmosfer : Selimut Bumi = Ozon : .........", "id": "Pelindung Sinar UV" },
  { "en": "Kode Sumber : Programmer = Cetak Biru : .........", "id": "Arsitek" },
  { "en": "Portofolio : Investasi = Neraca : .........", "id": "Keuangan Perusahaan" },
  { "en": "Anggur : Fermentasi = Susu : .........", "id": "Pasteurisasi" },
  { "en": "Sinapsis : Hubungan Antar Saraf = Reseptor : .........", "id": "Penerima Rangsang" },
  { "en": "Legislator : Pembuat UU = Konstituen : .........", "id": "Pemilih" },
  { "en": "Mendesis : Uap Panas = Merintih : .........", "id": "Kesakitan" },
  { "en": "Amfiteater : Pertunjukan Terbuka = Observatorium : .........", "id": "Pengamatan Bintang" },
  { "en": "Fresko : Dinding Basah = Mozaik : .........", "id": "Pecahan Kaca" },
  { "en": "Katalis : Mempercepat Reaksi = Inhibitor : .........", "id": "Memperlambat Reaksi" },
  { "en": "Fjord : Teluk dari Gletser = Atol : .........", "id": "Pulau Karang Cincin" },
  { "en": "Hibernasi : Tidur Musim Dingin = Estivasi : .........", "id": "Tidur Musim Panas" },
  { "en": "Fobia : Ketakutan Irrasional = Euforia : .........", "id": "Kegembiraan Berlebihan" },
  { "en": "Blanching : Merebus Sebentar = Sauté : .........", "id": "Menumis Cepat" },
  { "en": "Otoskop : Telinga = Oftalmoskop : .........", "id": "Mata" },
  { "en": "Deduksi : Umum ke Khusus = Induksi : .........", "id": "Khusus ke Umum" },
  { "en": "Kres : Naik Setengah Nada = Mol : .........", "id": "Turun Setengah Nada" },
  { "en": "Bipartisan : Dua Partai = Multipartai : .........", "id": "Banyak Partai" },
  { "en": "Gabus : Mengapung = Besi : .........", "id": "Tenggelam" },
  { "en": "Membeku : Suhu Dingin = Menguap : .........", "id": "Suhu Panas" },
  { "en": "Soneta : 14 Baris = Haiku : .........", "id": "3 Baris" },
  { "en": "Isotop : Nomor Atom Sama = Isobar : .........", "id": "Nomor Massa Sama" },
  { "en": "Archipelago : Gugusan Pulau = Semenanjung : .........", "id": "Daratan menjorok ke Laut" },
  { "en": "Mimikri : Meniru Lingkungan = Autotomi : .........", "id": "Memutuskan Ekor" },
  { "en": "Introvert : Energi dari Kesendirian = Ekstrovert : .........", "id": "Energi dari Interaksi Sosial" },
  { "en": "Braising : Memasak Perlahan = Grilling : .........", "id": "Memanggang di Atas Api" },
  { "en": "Sfigmomanometer : Tekanan Darah = Glukometer : .........", "id": "Gula Darah" },
  { "en": "Silogisme : Penalaran Logis = Anagram : .........", "id": "Susun Ulang Huruf" },
  { "en": "Forte : Keras = Piano : .........", "id": "Lembut" },
  { "en": "Monarki : Raja = Republik : .........", "id": "Presiden" },
  { "en": "Isolator : Menghambat Panas = Konduktor : .........", "id": "Menghantarkan Panas" },
  { "en": "Mencair : Padat ke Cair = Menyublim : .........", "id": "Padat ke Gas" },
  { "en": "Gurindam : Dua Baris = Seloka : .........", "id": "Bentuk Puisi" },
  { "en": "Polimer : Monomer = Protein : .........", "id": "Asam Amino" },
  { "en": "Gurun : Oasis = Laut : .........", "id": "Pulau" },
  { "en": "Feromon : Komunikasi Serangga = Hormon : .........", "id": "Komunikasi Tubuh" },
  { "en": "Empati : Merasakan Emosi Orang Lain = Simpati : .........", "id": "Merasa Kasihan" },
  { "en": "Poaching : Merebus Tanpa Cangkang = Steaming : .........", "id": "Mengukus" },
  { "en": "Endoskop : Organ Dalam = Kolonoskop : .........", "id": "Usus Besar" },
  { "en": "Palindrom : Dibaca Bolak-balik Sama = Akronim : .........", "id": "Singkatan dari Frasa" },
  { "en": "Adagio : Lambat = Allegro : .........", "id": "Cepat" },
  { "en": "Federal : Pembagian Kekuasaan = Unitaris : .........", "id": "Pemusatan Kekuasaan" },
  { "en": "Kenyal : Karet = Getas : .........", "id": "Kerupuk" },
  { "en": "Deposisi : Gas ke Padat = Kondensasi : .........", "id": "Gas ke Cair" },
  { "en": "Pantun : Sampiran dan Isi = Syair : .........", "id": "Semua Baris Isi" },
  { "en": "Karbohidrat : Gula = Lemak : .........", "id": "Asam Lemak" },
  { "en": "Tundra : Lumut = Sabana : .........", "id": "Padang Rumput dan Pohon" },
  { "en": "Ekolokasi : Kelelawar = Bioluminesensi : .........", "id": "Kunang-kunang" },
  { "en": "Delusi : Keyakinan Palsu = Halusinasi : .........", "id": "Persepsi Palsu" },
  { "en": "Marinating : Merendam Bumbu = Glazing : .........", "id": "Melapisi Gula" },
  { "en": "Stetoskop : Detak Jantung = Palu Refleks : .........", "id": "Respon Saraf" },
  { "en": "Kiasan : Bahasa Figuratif = Denotasi : .........", "id": "Arti Sebenarnya" },
  { "en": "Staccato : Terputus-putus = Legato : .........", "id": "Menyambung" },
  { "en": "Oligarki : Dikuasai Sedikit Orang = Demokrasi : .........", "id": "Dikuasai Rakyat" },
  { "en": "Transparan : Tembus Pandang = Opak : .........", "id": "Tidak Tembus Pandang" },
  { "en": "Basa : Pahit = Asam : .........", "id": "Kecut" },
  { "en": "Epigram : Puisi Pendek Jenaka = Elegi : .........", "id": "Puisi Kesedihan" },
  { "en": "Enzim : Biokatalisator = Antibodi : .........", "id": "Sistem Imun" },
  { "en": "Taiga : Hutan Pinus = Stepa : .........", "id": "Padang Rumput Luas" },
  { "en": "Regenerasi : Bintang Laut = Metamorfosis : .........", "id": "Katak" },
  { "en": "Narsisme : Cinta Diri Berlebihan = Altruisme : .........", "id": "Mementingkan Orang Lain" },
  { "en": "Caramelizing : Menggosongkan Gula = Whipping : .........", "id": "Mengocok Krim" },
  { "en": "Spuit : Suntikan = Kateter : .........", "id": "Saluran Urin" },
  { "en": "Konotasi : Arti Kiasan = Ambiguitas : .........", "id": "Makna Ganda" },
  { "en": "Ritme : Pola Waktu = Harmoni : .........", "id": "Kombinasi Nada" },
  { "en": "Aristokrasi : Kaum Bangsawan = Plutokrasi : .........", "id": "Kaum Kaya" },
  { "en": "Poros : Berputar = Sumbu : .........", "id": "Simetri" },
  { "en": "Lakmus Merah : Basa = Lakmus Biru : .........", "id": "Asam" },
  { "en": "Ode : Puisi Pujian = Himne : .........", "id": "Lagu Pujian" },
  { "en": "Vitamin : Mikronutrien = Protein : .........", "id": "Makronutrien" },
  { "en": "Monsoon : Angin Musiman = Pasat : .........", "id": "Angin Sepanjang Tahun" },
  { "en": "Partenogenesis : Reproduksi Tanpa Jantan = Ovipar : .........", "id": "Bertelur" },
  { "en": "Apatis : Tidak Peduli = Antusias : .........", "id": "Sangat Berminat" },
  { "en": "Deglazing : Melarutkan Sisa Masakan = Kneading : .........", "id": "Menguleni Adonan" },
  { "en": "Pinset : Mencabut = Skalpel : .........", "id": "Membedah" },
  { "en": "Prosa : Narasi = Puisi : .........", "id": "Liris" },
  { "en": "Crescendo : Makin Keras = Decrescendo : .........", "id": "Makin Lembut" },
  { "en": "Tirani : Kekuasaan Mutlak = Anarki : .........", "id": "Tanpa Pemerintahan" },
  { "en": "Elastisitas : Kembali ke Bentuk Semula = Plastisitas : .........", "id": "Berubah Bentuk Permanen" },
  { "en": "Larutan : Homogen = Suspensi : .........", "id": "Heterogen" },
  { "en": "Prolog : Pengantar = Monolog : .........", "id": "Percakapan Sendiri" },
  { "en": "Aorta : Arteri Terbesar = Vena Kava : .........", "id": "Vena Terbesar" },
  { "en": "Gletser : Es Bergerak = Moraine : .........", "id": "Endapan Gletser" },
  { "en": "Vivipar : Melahirkan = Ovovivipar : .........", "id": "Bertelur dan Melahirkan" },
  { "en": "Hedonisme : Kenikmatan = Stoikisme : .........", "id": "Pengendalian Diri" },
  { "en": "Folding : Melipat Adonan = Mincing : .........", "id": "Mencincang Halus" },
  { "en": "Forsep : Menjepit = Bor : .........", "id": "Melubangi Tulang" },
  { "en": "Kosa Kata : Leksikon = Tata Bahasa : .........", "id": "Sintaksis" },
  { "en": "Akselerando : Mempercepat Tempo = Ritardando : .........", "id": "Memperlambat Tempo" },
  { "en": "Embargo : Larangan Ekspor Impor = Protektorat : .........", "id": "Negara di Bawah Perlindungan" },
  { "en": "Viskositas : Kekentalan Cairan = Densitas : .........", "id": "Kepadatan Massa" },
  { "en": "pH : Keasaman = pOH : .........", "id": "Kebasaan" },
  { "en": "Dialog : Antar Tokoh = Solilokui : .........", "id": "Mengungkapkan Pikiran Sendiri" },
  { "en": "Nukleus : Inti Sel = Sitoplasma : .........", "id": "Cairan Sel" },
  { "en": "Sesar : Patahan Lempeng = Palung : .........", "id": "Dasar Laut Dalam" },
  { "en": "Amfibi : Hidup di Dua Alam = Arboreal : .........", "id": "Hidup di Pohon" },
  { "en": "Rasional : Logis = Empiris : .........", "id": "Berdasarkan Pengalaman" },
  { "en": "Dicing : Memotong Dadu = Julienne : .........", "id": "Memotong Korek Api" },
  { "en": "Termokopel : Pengukur Suhu = Piezometer : .........", "id": "Pengukur Tekanan Cairan" },
  { "en": "Fonem : Satuan Bunyi Terkecil = Morfem : .........", "id": "Satuan Makna Terkecil" },
  { "en": "Oktaf : Delapan Nada = Interval : .........", "id": "Jarak Antar Nada" },
  { "en": "Referendum : Pemungutan Suara Rakyat = Mosi Tidak Percaya : .........", "id": "Pernyataan Parlemen" },
  { "en": "Kingdom : Filum = Kelas : .........", "id": "Ordo" },
  { "en": "Renaisans : Kelahiran Kembali = Abad Pertengahan : .........", "id": "Zaman Kegelapan" },
  { "en": "Kata Benda : Nomina = Kata Kerja : .........", "id": "Verba" },
  { "en": "Monopoli : Satu Penjual = Monopsoni : .........", "id": "Satu Pembeli" },
  { "en": "Troposfer : Tempat Terjadinya Cuaca = Stratosfer : .........", "id": "Lapisan Ozon" },
  { "en": "Dendrit : Menerima Impuls = Akson : .........", "id": "Mengirim Impuls" },
  { "en": "Alusi : Rujukan Hal Terkenal = Antitesis : .........", "id": "Pertentangan Ide" },
  { "en": "Pleidoi : Pembelaan Terdakwa = Replik : .........", "id": "Jawaban Jaksa atas Pleidoi" },
  { "en": "Terasering : Mencegah Erosi = Irigasi : .........", "id": "Mengairi Sawah" },
  { "en": "Epistemologi : Teori Pengetahuan = Aksiologi : .........", "id": "Teori Nilai" },
  { "en": "Kulit : Tas = Getah Karet : .........", "id": "Ban" },
  { "en": "Menebang Hutan : Longsor = Membuang Sampah Sembarangan : .........", "id": "Banjir" },
  { "en": "Teodolit : Survei Tanah = Seismograf : .........", "id": "Gempa Bumi" },
  { "en": "Alpha : Awal = Omega : .........", "id": "Akhir" },
  { "en": "Familia : Genus = Spesies : .........", "id": "Individu" },
  { "en": "Barok : Ornamen Berlebih = Minimalisme : .........", "id": "Sederhana" },
  { "en": "Kata Sifat : Adjektiva = Kata Keterangan : .........", "id": "Adverbia" },
  { "en": "Oligopoli : Beberapa Penjual = Duopoli : .........", "id": "Dua Penjual" },
  { "en": "Mesosfer : Melindungi dari Meteor = Termosfer : .........", "id": "Lapisan Panas" },
  { "en": "Badan Sel : Mengatur Sel Saraf = Selubung Mielin : .........", "id": "Melindungi Akson" },
  { "en": "Klimaks : Puncak Ketegangan = Resolusi : .........", "id": "Penyelesaian Masalah" },
  { "en": "Duplik : Jawaban Terdakwa atas Replik = Vonis : .........", "id": "Putusan Hakim" },
  { "en": "Rotasi Tanaman : Menjaga Kesuburan Tanah = Tumpang Sari : .........", "id": "Menanam Lebih dari Satu Jenis" },
  { "en": "Ontologi : Hakikat Keberadaan = Etika : .........", "id": "Filsafat Moral" },
  { "en": "Biji Kopi : Minuman = Biji Kakao : .........", "id": "Cokelat" },
  { "en": "Polusi Udara : Penyakit Pernapasan = Polusi Air : .........", "id": "Penyakit Pencernaan" },
  { "en": "Galvanometer : Mengukur Arus Lemah = Voltmeter : .........", "id": "Mengukur Tegangan" },
  { "en": "Merah : Berhenti = Hijau : .........", "id": "Jalan" },
  { "en": "Vertebrata : Bertulang Belakang = Invertebrata : .........", "id": "Tidak Bertulang Belakang" },
  { "en": "Gotik : Lengkungan Runcing = Klasik : .........", "id": "Pilar dan Simetri" },
  { "en": "Pronomina : Kata Ganti = Konjungsi : .........", "id": "Kata Hubung" },
  { "en": "Persaingan Sempurna : Banyak Penjual Pembeli = Persaingan Monopolistik : .........", "id": "Produk Terdiferensiasi" },
  { "en": "Eksosfer : Lapisan Terluar = Ionosfer : .........", "id": "Memantulkan Gelombang Radio" },
  { "en": "Sinapsis : Celah Antar Neuron = Neurotransmitter : .........", "id": "Zat Penghantar Impuls" },
  { "en": "Eksposisi : Pengenalan Tokoh = Komplikasi : .........", "id": "Munculnya Konflik" },
  { "en": "Saksi Ahli : Memberi Keterangan Keahlian = Saksi Fakta : .........", "id": "Melihat atau Mendengar Peristiwa" },
  { "en": "Reboisasi : Menanam Kembali Hutan = Diversifikasi Pertanian : .........", "id": "Penganekaragaman Tanaman" },
  { "en": "Logika : Penalaran = Estetika : .........", "id": "Keindahan" },
  { "en": "Serat Kapas : Benang = Bambu : .........", "id": "Kertas" },
  { "en": "Efek Rumah Kaca : Pemanasan Global = Hujan Asam : .........", "id": "Kerusakan Bangunan" },
  { "en": "Amperemeter : Mengukur Arus Listrik = Ohmmeter : .........", "id": "Mengukur Hambatan" },
  { "en": "Salib : Kekristenan = Bintang Daud : .........", "id": "Yudaisme" },
  { "en": "Poikiloterm : Berdarah Dingin = Homoiterm : .........", "id": "Berdarah Panas" },
  { "en": "Impresionisme : Kesan Sesaat = Surealisme : .........", "id": "Dunia Bawah Sadar" },
  { "en": "Preposisi : Kata Depan = Interjeksi : .........", "id": "Kata Seru" },
  { "en": "Kartel : Kolusi Harga = Trust : .........", "id": "Gabungan Perusahaan" },
  { "en": "Angin Darat : Malam Hari = Angin Laut : .........", "id": "Siang Hari" },
  { "en": "Sel Saraf Sensorik : Ke Pusat Saraf = Sel Saraf Motorik : .........", "id": "Dari Pusat Saraf" },
  { "en": "Sudut Pandang Orang Pertama : Aku = Sudut Pandang Orang Ketiga : .........", "id": "Dia/Nama Tokoh" },
  { "en": "Locus Delicti : Tempat Kejadian = Tempus Delicti : .........", "id": "Waktu Kejadian" },
  { "en": "Monokultur : Satu Jenis Tanaman = Polikultur : .........", "id": "Banyak Jenis Tanaman" },
  { "en": "Rasionalisme : Akal = Empirisme : .........", "id": "Pengalaman Indrawi" },
  { "en": "Minyak Mentah : Bensin = Bijih Besi : .........", "id": "Baja" },
  { "en": "Penipisan Ozon : Sinar UV Berlebih = Penebangan Bakau : .........", "id": "Abrasi Pantai" },
  { "en": "Manometer : Mengukur Tekanan Gas Tertutup = Dinamometer : .........", "id": "Mengukur Gaya" },
  { "en": "Bulan Sabit : Islam = Roda Dharma : .........", "id": "Buddhisme" },
  { "en": "Angiospermae : Biji Tertutup = Gymnospermae : .........", "id": "Biji Terbuka" },
  { "en": "Kubisme : Bentuk Geometris = Naturalisme : .........", "id": "Sesuai Alam" },
  { "en": "Frasa : Gabungan Kata = Klausa : .........", "id": "Memiliki Subjek dan Predikat" },
  { "en": "Holding Company : Perusahaan Induk = Anak Perusahaan : .........", "id": "Perusahaan yang Dimiliki" },
  { "en": "Siklon : Tekanan Rendah = Antisiklon : .........", "id": "Tekanan Tinggi" },
  { "en": "Sistem Saraf Pusat : Otak dan Sumsum Tulang Belakang = Sistem Saraf Tepi : .........", "id": "Saraf di Seluruh Tubuh" },
  { "en": "Latar : Tempat dan Waktu = Amanat : .........", "id": "Pesan Moral" },
  { "en": "Tersangka : Diduga Bersalah = Terpidana : .........", "id": "Telah Dihukum" },
  { "en": "Panca Usaha Tani : Lima Upaya Pertanian = Sapta Pesona : .........", "id": "Tujuh Unsur Pariwisata" },
  { "en": "Idealisme : Ide = Materialisme : .........", "id": "Materi" },
  { "en": "Lateks : Karet = Nira : .........", "id": "Gula Aren" },
  { "en": "Intrusif : Aktivitas Vulkanik = Tumpahan Minyak : .........", "id": "Pencemaran Laut" },
  { "en": "Higrometer : Kelembaban Relatif = Psikrometer : .........", "id": "Kelembaban dan Suhu" },
  { "en": "Yin Yang : Taoisme = Om : .........", "id": "Hinduisme" },
  { "en": "Dikotil : Biji Berkeping Dua = Monokotil : .........", "id": "Biji Berkeping Satu" },
  { "en": "Realisme : Apa Adanya = Romantisme : .........", "id": "Imajinasi dan Emosi" },
  { "en": "Sintaksis : Struktur Kalimat = Semantik : .........", "id": "Makna Kata" },
  { "en": "Joint Venture : Usaha Patungan = Konsorsium : .........", "id": "Kerja Sama Proyek" },
  { "en": "Arus Konveksi : Pergerakan Udara/Cairan = Arus Konduksi : .........", "id": "Perambatan Panas pada Padatan" },
  { "en": "Saraf Simpatik : Mempercepat Detak Jantung = Saraf Parasimpatik : .........", "id": "Memperlambat Detak Jantung" },
  { "en": "Alur Maju : Progresif = Alur Mundur : .........", "id": "Regresif" },
  { "en": "Bukti : Meyakinkan Hakim = Argumen : .........", "id": "Meyakinkan Lawan Debat" },
  { "en": "Ekstensifikasi : Memperluas Lahan = Intensifikasi : .........", "id": "Meningkatkan Hasil per Lahan" },
  { "en": "Eksistensialisme : Kebebasan Individu = Nihilisme : .........", "id": "Ketiadaan Makna" },
  { "en": "Tembaga : Kabel = Silikon : .........", "id": "Chip Komputer" },
  { "en": "Sedimentasi : Pengendapan = Abrasi : .........", "id": "Pengikisan Pantai" },
  { "en": "Anemometer : Mengukur Angin = Barograf : .........", "id": "Merekam Tekanan Udara" },
  { "en": "Menorah : Yudaisme = Khanda : .........", "id": "Sikhisme" },
  { "en": "Bryophyta : Tumbuhan Lumut = Pteridophyta : .........", "id": "Tumbuhan Paku" },
  { "en": "Dadaisme : Anti-Seni = Pop Art : .........", "id": "Budaya Populer" },
  { "en": "Pragmatik : Konteks Bahasa = Morfologi : .........", "id": "Bentuk Kata" },
  { "en": "Ceteris Paribus : Faktor Lain Dianggap Konstan = Post Hoc : .........", "id": "Sesat Pikir Sebab-Akibat" },
  { "en": "Kabut : Awan Rendah = Embun : .........", "id": "Uap Air Mengembun" },
  { "en": "Lobus Frontal : Berpikir = Lobus Oksipital : .........", "id": "Melihat" },
  { "en": "Perwatakan : Karakter Tokoh = Tema : .........", "id": "Gagasan Utama Cerita" },
  { "en": "Grasi : Hak Presiden = Mosi : .........", "id": "Hak Parlemen" },
  { "en": "Mekanisasi : Menggunakan Mesin = Otomatisasi : .........", "id": "Sistem Bekerja Sendiri" },
  { "en": "Utilitarianisme : Kebahagiaan Terbanyak = Deontologi : .........", "id": "Kewajiban Moral" },
  { "en": "Cantilever : Jembatan Gantung = Buttress : .........", "id": "Dinding Katedral" },
  { "en": "Arpeggio : Akor Terurai = Vibrato : .........", "id": "Getaran Nada" },
  { "en": "Foreshadowing : Petunjuk Masa Depan = Flashback : .........", "id": "Kilas Balik" },
  { "en": "Trilobita : Era Paleozoikum = Amonit : .........", "id": "Era Mesozoikum" },
  { "en": "Putik : Organ Betina Bunga = Benang Sari : .........", "id": "Organ Jantan Bunga" },
  { "en": "Ad Hominem : Menyerang Pribadi = Red Herring : .........", "id": "Pengalihan Isu" },
  { "en": "Firaun : Mesir Kuno = Tsar : .........", "id": "Rusia" },
  { "en": "Biner : Dua Simbol = Heksadesimal : .........", "id": "Enam Belas Simbol" },
  { "en": "Ikatan Ionik : Serah Terima Elektron = Ikatan Kovalen : .........", "id": "Pemakaian Bersama Elektron" },
  { "en": "Kumulus : Awan Menggumpal = Stratus : .........", "id": "Awan Berlapis" },
  { "en": "Onkologi : Kanker = Geriatri : .........", "id": "Lanjut Usia" },
  { "en": "Pahat : Mengukir Kayu = Cetakan : .........", "id": "Membentuk Logam" },
  { "en": "Tidur Siang : Siesta = Ziarah Keagamaan : .........", "id": "Haji" },
  { "en": "Kubah : Atap = Fondasi : .........", "id": "Dasar Bangunan" },
  { "en": "Glissando : Meluncur di Antara Nada = Pizzicato : .........", "id": "Memetik Senar" },
  { "en": "Cameo : Penampilan Singkat = Leitmotif : .........", "id": "Tema Musik Berulang" },
  { "en": "Dinosaurus : Era Mesozoikum = Mamut : .........", "id": "Zaman Es" },
  { "en": "Sepal : Melindungi Kuncup = Petal : .........", "id": "Menarik Penyerbuk" },
  { "en": "Straw Man : Memutarbalikkan Argumen = Slippery Slope : .........", "id": "Rangkaian Konsekuensi Buruk" },
  { "en": "Sultan : Kesultanan = Kaisar : .........", "id": "Kekaisaran" },
  { "en": "Algoritma : Langkah-langkah Logis = Heuristik : .........", "id": "Pendekatan Coba-coba" },
  { "en": "Reaksi Endoterm : Menyerap Panas = Reaksi Eksoterm : .........", "id": "Melepaskan Panas" },
  { "en": "Sirus : Awan Tipis dan Tinggi = Nimbus : .........", "id": "Awan Hujan" },
  { "en": "Pediatri : Anak-anak = Obstetri : .........", "id": "Kebidanan" },
  { "en": "Amplas : Menghaluskan = Gerinda : .........", "id": "Mengikis" },
  { "en": "Perjalanan Jauh : Eksodus = Pengasingan Paksa : .........", "id": "Deportasi" },
  { "en": "Gargoyle : Hiasan Saluran Air = Fresko : .........", "id": "Lukisan Dinding" },
  { "en": "Timbre : Warna Suara = Dinamika : .........", "id": "Keras Lembutnya Nada" },
  { "en": "Montage : Rangkaian Gambar Cepat = Jump Cut : .........", "id": "Loncatan Waktu" },
  { "en": "Pangea : Superbenua = Panthalassa : .........", "id": "Superlautan" },
  { "en": "Akar Tunggang : Dikotil = Akar Serabut : .........", "id": "Monokotil" },
  { "en": "Circular Reasoning : Argumen Berputar = False Dichotomy : .........", "id": "Pilihan Hitam-Putih" },
  { "en": "Syah : Persia = Mikado : .........", "id": "Jepang (Kuno)" },
  { "en": "Kompilasi : Menerjemahkan Kode = Debugging : .........", "id": "Mencari Kesalahan" },
  { "en": "Zat Pelarut : Solven = Zat Terlarut : .........", "id": "Solut" },
  { "en": "Aurora : Kutub = Pelangi : .........", "id": "Setelah Hujan" },
  { "en": "Neurologi : Saraf = Pulmonologi : .........", "id": "Paru-paru" },
  { "en": "Solder : Menyambung Logam = Las : .........", "id": "Melelehkan dan Menyambung" },
  { "en": "Tepuk Tangan : Ovasi = Hukuman Mati : .........", "id": "Eksekusi" },
  { "en": "Dorik : Pilar Sederhana = Korintus : .........", "id": "Pilar Rumit" },
  { "en": "A Capella : Tanpa Instrumen = Instrumental : .........", "id": "Tanpa Vokal" },
  { "en": "Deus Ex Machina : Solusi Tiba-tiba = MacGuffin : .........", "id": "Objek Pemicu Plot" },
  { "en": "Zaman Kapur : Kepunahan Dinosaurus = Zaman Es : .........", "id": "Kepunahan Mamut" },
  { "en": "Gutasi : Tetesan Air di Tepi Daun = Transpirasi : .........", "id": "Penguapan dari Daun" },
  { "en": "Hasty Generalization : Generalisasi Terburu-buru = Appeal to Authority : .........", "id": "Menggunakan Otoritas Palsu" },
  { "en": "Raja : Monarki = Perdana Menteri : .........", "id": "Parlementer" },
  { "en": "Loop : Pengulangan = Kondisional : .........", "id": "Jika-Maka" },
  { "en": "Emulsi : Campuran Cairan Tak Larut = Koloid : .........", "id": "Partikel Tersuspensi" },
  { "en": "Front Dingin : Hujan Deras = Front Panas : .........", "id": "Hujan Ringan" },
  { "en": "Endokrinologi : Hormon = Hematologi : .........", "id": "Darah" },
  { "en": "Dongkrak : Mengangkat = Katrol : .........", "id": "Menarik Beban" },
  { "en": "Upacara : Ritual = Keyakinan : .........", "id": "Dogma" },
  { "en": "Denah : Tata Ruang = Skema : .........", "id": "Diagram Sistem" },
  { "en": "Melodi : Rangkaian Nada = Kontrapung : .........", "id": "Gabungan Beberapa Melodi" },
  { "en": "Diegesis : Dunia dalam Cerita = Non-diegetik : .........", "id": "Elemen di Luar Cerita (Musik Latar)" },
  { "en": "Hominid : Kera Besar = Canid : .........", "id": "Anjing" },
  { "en": "Eutrofikasi : Ledakan Alga = Deforestasi : .........", "id": "Hilangnya Hutan" },
  { "en": "Non Sequitur : Tidak Nyambung = Post Hoc : .........", "id": "Menyimpulkan Sebab Akibat yang Salah" },
  { "en": "Khan : Mongol = Emir : .........", "id": "Arab" },
  { "en": "Pseudocode : Kerangka Logika = Flowchart : .........", "id": "Diagram Alir" },
  { "en": "Titrasi : Menentukan Konsentrasi = Kromatografi : .........", "id": "Memisahkan Campuran" },
  { "en": "Isobar : Garis Tekanan Sama = Isoterm : .........", "id": "Garis Suhu Sama" },
  { "en": "Hepatologi : Hati = Reumatologi : .........", "id": "Sendi dan Jaringan Ikat" },
  { "en": "Ragum : Menjepit = Bor Tangan : .........", "id": "Membuat Lubang" },
  { "en": "Sumpah : Janji Suci = Nazar : .........", "id": "Janji Bersyarat" },
  { "en": "Relief : Pahatan Timbul = Patung : .........", "id": "Karya Tiga Dimensi" },
  { "en": "Koda : Bagian Penutup Musik = Overture : .........", "id": "Bagian Pembuka Musik" },
  { "en": "Breaking the Fourth Wall : Berbicara pada Penonton = Chekhov's Gun : .........", "id": "Elemen yang Harus Penting di Akhir" },
  { "en": "Felid : Kucing = Bovine : .........", "id": "Sapi" },
  { "en": "Suksesi Ekologis : Perubahan Komunitas = Iklim Mikro : .........", "id": "Iklim di Area Terbatas" },
  { "en": "Ad Ignorantiam : Argumen dari Ketidaktahuan = Tu Quoque : .........", "id": "Menuduh Lawan Melakukan Hal Sama" },
  { "en": "Dogma : Doktrin = Eretis : .........", "id": "Bid'ah" },
  { "en": "Firewall : Memfilter Lalu Lintas Jaringan = Enkripsi : .........", "id": "Mengamankan Data" },
  { "en": "Isomer : Rumus Kimia Sama, Struktur Beda = Alotrop : .........", "id": "Bentuk Fisik Unsur yang Beda" },
  { "en": "Kecepatan Angin : Anemometer = Arah Angin : .........", "id": "Wind Vane" },
  { "en": "Toksikologi : Racun = Virologi : .........", "id": "Virus" },
  { "en": "Jangka Sorong : Mengukur Diameter = Mikrometer Sekrup : .........", "id": "Mengukur Ketebalan" },
  { "en": "Biksu : Vihara = Pastor : .........", "id": "Gereja" },
  { "en": "Stuko : Plester Dinding = Mozaik : .........", "id": "Hiasan dari Kepingan" },
  { "en": "Subplot : Alur Cerita Sampingan = Plot Twist : .........", "id": "Kejutan dalam Alur" },
  { "en": "Amphibian : Katak = Marsupial : .........", "id": "Kanguru" },
  { "en": "Biomassa : Total Berat Makhluk Hidup = Rantai Makanan : .........", "id": "Urutan Makan-Dimakan" },
  { "en": "Ekuivokasi : Penggunaan Kata Bermakna Ganda = Amfiboli : .........", "id": "Kalimat Bermakna Ganda" },
  { "en": "Teokrasi : Diperintah Pemuka Agama = Meritokrasi : .........", "id": "Diperintah Berdasar Kemampuan" },
  { "en": "API (Application Programming Interface) : Penghubung Antar Perangkat Lunak = GUI (Graphical User Interface) : .........", "id": "Antarmuka Visual Pengguna" },
  { "en": "Asam Amino : Blok Pembangun Protein = Nukleotida : .........", "id": "Blok Pembangun DNA" },
  { "en": "Titik Embun : Suhu Pengembunan = Kelembaban Absolut : .........", "id": "Massa Uap Air per Volume Udara" },
  { "en": "Sitologi : Sel = Histologi : .........", "id": "Jaringan" },
  { "en": "Obeng : Memutar Sekrup = Kunci Inggris : .........", "id": "Memutar Baut" },
  { "en": "Baptis : Inisiasi Kristen = Tahallul : .........", "id": "Akhir Ibadah Haji" },
  { "en": "Arsitektur : Seni Merancang Bangunan = Sinematografi : .........", "id": "Seni Merekam Gambar Bergerak" },
  { "en": "Protagonis : Hero = Antagonis : .........", "id": "Villain" },
  { "en": "Cephalopod : Gurita = Gastropod : .........", "id": "Siput" },
  { "en": "Niche : Peran Ekologis Spesies = Habitat : .........", "id": "Tempat Tinggal Spesies" },
  { "en": "Etimologi : Asal Usul Kata = Paleografi : .........", "id": "Tulisan Kuno" },
  { "en": "Supernova : Ledakan Bintang = Lubang Hitam : .........", "id": "Gravitasi Ekstrem" },
  { "en": "Stoikisme : Pengendalian Diri = Epikureanisme : .........", "id": "Kesenangan Sederhana" },
  { "en": "Transkripsi : DNA ke RNA = Translasi : .........", "id": "RNA ke Protein" },
  { "en": "Kamuflase : Penyamaran Visual = Mimikri : .........", "id": "Peniruan Spesies Lain" },
  { "en": "Kubisme : Picasso = Impresionisme : .........", "id": "Monet" },
  { "en": "Sonata : Instrumen Solo = Concerto : .........", "id": "Instrumen Solo dan Orkestra" },
  { "en": "Batuan Beku : Pendinginan Magma = Batuan Sedimen : .........", "id": "Pengendapan Material" },
  { "en": "Teorema : Perlu Dibuktikan = Aksioma : .........", "id": "Diterima Tanpa Bukti" },
  { "en": "Sistem Limfatik : Kekebalan Tubuh = Sistem Integumen : .........", "id": "Kulit dan Turunannya" },
  { "en": "Demografi : Studi Populasi = Etnografi : .........", "id": "Studi Budaya" },
  { "en": "Saham : Bukti Kepemilikan = Obligasi : .........", "id": "Surat Perjanjian Utang" },
  { "en": "Griffin : Tubuh Singa Kepala Elang = Centaur : .........", "id": "Tubuh Kuda Badan Manusia" },
  { "en": "Semiotika : Ilmu Tanda = Fonologi : .........", "id": "Ilmu Bunyi Bahasa" },
  { "en": "Pulsar : Bintang Neutron Berputar = Quasar : .........", "id": "Inti Galaksi Aktif yang Sangat Terang" },
  { "en": "Rasionalisme : Pengetahuan dari Akal = Empirisme : .........", "id": "Pengetahuan dari Pengalaman" },
  { "en": "Mitosis : Pembelahan Sel Tubuh = Meiosis : .........", "id": "Pembelahan Sel Kelamin" },
  { "en": "Perang Gerilya : Taktik Kecil dan Cepat = Perang Parit : .........", "id": "Pertahanan Statis" },
  { "en": "Surealisme : Salvador Dali = Pop Art : .........", "id": "Andy Warhol" },
  { "en": "Simfoni : Orkestra Besar = Kuartet Gesek : .........", "id": "Empat Pemain Alat Musik Gesek" },
  { "en": "Batuan Metamorf : Perubahan Akibat Panas dan Tekanan = Batuan Piroklastik : .........", "id": "Letusan Gunung Berapi" },
  { "en": "Postulat : Asumsi Dasar = Korolarium : .........", "id": "Konsekuensi Langsung dari Teorema" },
  { "en": "Sistem Saraf : Transmisi Sinyal = Sistem Ekskresi : .........", "id": "Pembuangan Limbah" },
  { "en": "Sosiologi : Masyarakat = Antropologi : .........", "id": "Manusia dan Perilakunya" },
  { "en": "Opsi : Hak Beli atau Jual = Reksadana : .........", "id": "Kumpulan Dana Investor" },
  { "en": "Minotaur : Manusia Berkepala Banteng = Siren : .........", "id": "Wanita Bertubuh Burung" },
  { "en": "Morfologi : Struktur Kata = Sintaksis : .........", "id": "Struktur Kalimat" },
  { "en": "Nebula : Awan Gas Pembentuk Bintang = Gugus Bintang : .........", "id": "Kumpulan Bintang" },
  { "en": "Absolutisme : Kekuasaan Tak Terbatas = Konstitusionalisme : .........", "id": "Kekuasaan Dibatasi Konstitusi" },
  { "en": "Genotipe : Sifat Genetik = Fenotipe : .........", "id": "Sifat Fisik yang Terlihat" },
  { "en": "Invasi : Serangan Masuk Wilayah = Blokade : .........", "id": "Pengepungan Pelabuhan" },
  { "en": "Abstrak Ekspresionisme : Jackson Pollock = Renaisans : .........", "id": "Leonardo da Vinci" },
  { "en": "Fugue : Komposisi Polifonik = Aria : .........", "id": "Lagu Solo dalam Opera" },
  { "en": "Erosi Angin : Deflasi = Erosi Air : .........", "id": "Ablasi" },
  { "en": "Lema : Bentuk Dasar Kata = Glos : .........", "id": "Catatan Penjelasan" },
  { "en": "Sistem Kardiovaskular : Jantung dan Pembuluh Darah = Sistem Pencernaan : .........", "id": "Lambung dan Usus" },
  { "en": "Kriminologi : Kejahatan = Viktimologi : .........", "id": "Korban Kejahatan" },
  { "en": "Pasar Modal : Instrumen Jangka Panjang = Pasar Uang : .........", "id": "Instrumen Jangka Pendek" },
  { "en": "Harpy : Wanita Bersayap = Satyr : .........", "id": "Manusia Berkaki Kambing" },
  { "en": "Dialek : Variasi Bahasa Regional = Idiolek : .........", "id": "Gaya Bahasa Individu" },
  { "en": "Komet : Ekor Gas dan Debu = Asteroid : .........", "id": "Batuan Angkasa" },
  { "en": "Feodalisme : Tuan Tanah dan Petani = Kapitalisme : .........", "id": "Pemilik Modal dan Pekerja" },
  { "en": "Alel : Varian Gen = Locus : .........", "id": "Posisi Gen pada Kromosom" },
  { "en": "Spionase : Mata-mata = Sabotase : .........", "id": "Perusakan" },
  { "en": "Barok : Rembrandt = Romantisme : .........", "id": "Delacroix" },
  { "en": "Kanon : Aturan Komposisi = Improvisasi : .........", "id": "Penciptaan Spontan" },
  { "en": "Diagenesis : Proses Pembentukan Batuan Sedimen = Metamorfisme : .........", "id": "Proses Perubahan Batuan" },
  { "en": "Dalil : Proposisi yang Terbukti = Hipotesis : .........", "id": "Dugaan yang Perlu Diuji" },
  { "en": "Sistem Respirasi : Pernapasan = Sistem Reproduksi : .........", "id": "Perkembangbiakan" },
  { "en": "Gerontologi : Proses Penuaan = Psikologi : .........", "id": "Proses Mental" },
  { "en": "Inflasi : Kenaikan Harga Umum = Stagflasi : .........", "id": "Inflasi dan Stagnasi Ekonomi" },
  { "en": "Phoenix : Burung Api Abadi = Hydra : .........", "id": "Ular Berkepala Banyak" },
  { "en": "Filologi : Naskah Kuno = Leksikografi : .........", "id": "Penyusunan Kamus" },
  { "en": "Lubang Cacing : Jalan Pintas Ruang-Waktu = Materi Gelap : .........", "id": "Materi Tak Terlihat" },
  { "en": "Merkantilisme : Akumulasi Emas = Liberalisme : .........", "id": "Perdagangan Bebas" },
  { "en": "Kromosom : Struktur Pembawa Gen = DNA : .........", "id": "Materi Genetik" },
  { "en": "Embargo : Larangan Perdagangan = Sanksi : .........", "id": "Hukuman Ekonomi" },
  { "en": "Rococo : Gaya Dekoratif Ringan = Neoklasikisme : .........", "id": "Inspirasi dari Seni Klasik" },
  { "en": "Resitatif : Gaya Bernyanyi seperti Berbicara = Libretto : .........", "id": "Naskah Opera" },
  { "en": "Fosil Jejak : Aktivitas Organisme = Fosil Tubuh : .........", "id": "Sisa Tubuh Organisme" },
  { "en": "Logaritma : Pangkat = Trigonometri : .........", "id": "Sudut dan Sisi Segitiga" },
  { "en": "Sistem Otot : Pergerakan = Sistem Rangka : .........", "id": "Penopang Tubuh" },
  { "en": "Futurologi : Masa Depan = Sejarah : .........", "id": "Masa Lalu" },
  { "en": "Derivatif : Kontrak Finansial = Komoditas : .........", "id": "Barang Dagangan" },
  { "en": "Pegasus : Kuda Bersayap = Cerberus : .........", "id": "Anjing Berkepala Tiga" },
  { "en": "Toponimi : Nama Tempat = Antroponimi : .........", "id": "Nama Orang" },
  { "en": "Cahaya Merah : Panjang Gelombang Panjang = Cahaya Biru : .........", "id": "Panjang Gelombang Pendek" },
  { "en": "Anarkisme : Tanpa Negara = Komunisme : .........", "id": "Masyarakat Tanpa Kelas" },
  { "en": "Gamet : Sel Kelamin = Zigot : .........", "id": "Sel Hasil Fertilisasi" },
  { "en": "Aturan Keterlibatan : Militer = Protokol : .........", "id": "Diplomasi" },
  { "en": "Pointilisme : Seurat = Fauvisme : .........", "id": "Matisse" },
  { "en": "Oratorio : Bertema Religius = Opera : .........", "id": "Bertema Drama" },
  { "en": "Dataran Banjir : Endapan Aluvial = Delta : .........", "id": "Endapan di Muara Sungai" },
  { "en": "Kalkulus : Perubahan = Aljabar : .........", "id": "Simbol dan Aturan" },
  { "en": "Hipotalamus : Mengatur Suhu Tubuh = Serebelum : .........", "id": "Mengatur Keseimbangan" },
  { "en": "Polemik : Debat Publik = Diskursus : .........", "id": "Wacana" },
  { "en": "BEI : Bursa Efek Indonesia = SEC : .........", "id": "Komisi Sekuritas dan Bursa AS" },
  { "en": "Golem : Makhluk Tanah Liat = Basilisk : .........", "id": "Ular Raja Pembunuh" },
  { "en": "Retorika : Seni Berbicara = Dialektika : .........", "id": "Seni Berdebat" },
  { "en": "Singularitas : Titik Kepadatan Tak Terhingga = Horizon Peristiwa : .........", "id": "Batas Lubang Hitam" },
  { "en": "Sosialisme : Kepemilikan Kolektif = Fasisme : .........", "id": "Nasionalisme Otoriter" },
  { "en": "Kariotipe : Peta Kromosom = Genom : .........", "id": "Seluruh Materi Genetik" },
  { "en": "Gencatan Senjata : Penghentian Sementara = Perjanjian Damai : .........", "id": "Penghentian Permanen" },
  { "en": "Art Nouveau : Ornamen Organik = Art Deco : .........", "id": "Bentuk Geometris" },
  { "en": "Counterpoint : Harmoni Melodi Independen = Homofoni : .........", "id": "Melodi Utama dengan Iringan" },
  { "en": "Lapili : Kerikil Vulkanik = Tefra : .........", "id": "Material Letusan Vulkanik" },
  { "en": "Geometri : Bentuk dan Ruang = Aritmetika : .........", "id": "Operasi Angka" },
  { "en": "Medula Oblongata : Mengatur Fungsi Vital = Korteks Serebral : .........", "id": "Pusat Pemikiran Tinggi" },
  { "en": "Hegemoni : Dominasi = Subordinasi : .........", "id": "Penaklukan" },
  { "en": "Hedging : Lindung Nilai = Arbitrase : .........", "id": "Mencari Keuntungan dari Perbedaan Harga" },
  { "en": "Sphinx : Teka-teki = Chimera : .........", "id": "Gabungan Banyak Hewan" },
  { "en": "Akronim : Singkatan Dibaca sebagai Kata = Inisialisme : .........", "id": "Singkatan Dibaca Huruf per Huruf" },
  { "en": "Bintang Biner : Dua Bintang Mengorbit = Galaksi Satelit : .........", "id": "Galaksi Kecil Mengorbit Galaksi Besar" },
  { "en": "Plutokrasi : Pemerintahan oleh Orang Kaya = Demokrasi : .........", "id": "Pemerintahan oleh Rakyat" },
  { "en": "Lisosom : Pencernaan Sel = Peroksisom : .........", "id": "Detoksifikasi Racun" },
  { "en": "Esker : Punggungan Pasir Gletser = Drumlin : .........", "id": "Bukit Gletser" },
  { "en": "TCP : Koneksi Andal = UDP : .........", "id": "Koneksi Cepat Tanpa Jaminan" },
  { "en": "Plosif : Hentian Udara (p, t, k) = Frikatif : .........", "id": "Gesekan Udara (f, s, v)" },
  { "en": "Opportunity Cost : Biaya Peluang = Sunk Cost : .........", "id": "Biaya Hangus" },
  { "en": "Alkana : Ikatan Tunggal = Alkena : .........", "id": "Ikatan Rangkap Dua" },
  { "en": "Aperture : Jumlah Cahaya Masuk = Shutter Speed : .........", "id": "Durasi Cahaya Masuk" },
  { "en": "Hukum Pidana : Kepentingan Publik = Hukum Perdata : .........", "id": "Kepentingan Pribadi" },
  { "en": "Perjanjian Versailles : Akhir Perang Dunia I = Runtuhnya Tembok Berlin : .........", "id": "Akhir Perang Dingin" },
  { "en": "Virga : Hujan yang Menguap Sebelum Sampai Tanah = Derecho : .........", "id": "Badai Angin Lurus yang Merusak" },
  { "en": "Begging the Question : Argumen Berputar = Genetic Fallacy : .........", "id": "Menilai Sesuatu dari Asal-usulnya" },
  { "en": "Stridulasi : Suara Gesekan (Jangkrik) = Vokalisasi : .........", "id": "Suara dari Pita Suara (Burung)" },
  { "en": "Endositosis : Memasukkan Materi ke Sel = Eksositosis : .........", "id": "Mengeluarkan Materi dari Sel" },
  { "en": "Cirque : Cekungan Lereng Gunung Akibat Gletser = Moraine : .........", "id": "Tumpukan puing Gletser" },
  { "en": "HTTP : Protokol Web = FTP : .........", "id": "Protokol Transfer File" },
  { "en": "Nasal : Bunyi Lewat Hidung (m, n) = Lateral : .........", "id": "Udara Lewat Sisi Lidah (l)" },
  { "en": "Moral Hazard : Risiko dari Perilaku Buruk = Adverse Selection : .........", "id": "Risiko dari Informasi Asimetris" },
  { "en": "Alkuna : Ikatan Rangkap Tiga = Senyawa Aromatik : .........", "id": "Cincin Benzena" },
  { "en": "Chiaroscuro : Kontras Terang-Gelap = Sfumato : .........", "id": "Transisi Warna Halus" },
  { "en": "Testimoni : Keterangan Saksi = Bukti Fisik : .........", "id": "Benda Terkait Kasus" },
  { "en": "Renaisans : Kebangkitan Seni dan Ilmu = Reformasi : .........", "id": "Perubahan Keagamaan" },
  { "en": "Fatwa : Hukum Islam = Dekret : .........", "id": "Perintah Resmi Penguasa" },
  { "en": "Slippery Slope : Rangkaian Akibat Buruk = Appeal to Tradition : .........", "id": "Menggunakan Tradisi sebagai Pembenaran" },
  { "en": "Taksis : Gerakan Menuju Rangsang = Tropisme : .........", "id": "Gerakan Pertumbuhan Menuju Rangsang" },
  { "en": "Fagositosis : Memakan Partikel Padat = Pinositosis : .........", "id": "Meminum Tetesan Cairan" },
  { "en": "Stalaktit : Dari Atas ke Bawah = Stalagmit : .........", "id": "Dari Bawah ke Atas" },
  { "en": "IP Address : Alamat Unik Perangkat = MAC Address : .........", "id": "Alamat Fisik Perangkat Keras" },
  { "en": "Diftong : Dua Vokal dalam Satu Suku Kata = Monoftong : .........", "id": "Vokal Tunggal" },
  { "en": "Giffen Good : Harga Naik Permintaan Naik = Veblen Good : .........", "id": "Barang Mewah (Status)" },
  { "en": "Hidrokarbon : Hidrogen dan Karbon = Alkohol : .........", "id": "Memiliki Gugus -OH" },
  { "en": "ISO : Sensitivitas Sensor Cahaya = White Balance : .........", "id": "Koreksi Warna Cahaya" },
  { "en": "Hukum Kontinental : Berbasis Kodifikasi = Hukum Anglo-Saxon : .........", "id": "Berbasis Yurisprudensi" },
  { "en": "Perang Salib : Perebutan Tanah Suci = Perang Opium : .........", "id": "Konflik Perdagangan" },
  { "en": "Halo : Cincin Cahaya di Sekitar Matahari/Bulan = Aurora : .........", "id": "Cahaya di Langit Kutub" },
  { "en": "Ad Populum : Argumen Berdasarkan Popularitas = Bandwagon Fallacy : .........", "id": "Ikut-ikutan karena Populer" },
  { "en": "Kinesis : Gerak Acak karena Rangsang = Nasti : .........", "id": "Gerak Tumbuhan Tanpa Arah Rangsang" },
  { "en": "Aparatus Golgi : Memodifikasi Protein = Retikulum Endoplasma : .........", "id": "Sintesis Protein dan Lipid" },
  { "en": "Delta : Segitiga Endapan di Muara = Meander : .........", "id": "Lekukan Sungai" },
  { "en": "Domain Name : Nama Situs Web = Server : .........", "id": "Komputer Penyimpan Situs Web" },
  { "en": "Fonetik : Cara Bunyi Dihasilkan = Fonemik : .........", "id": "Cara Bunyi Membedakan Makna" },
  { "en": "Elastisitas Permintaan : Responsivitas Permintaan = Elastisitas Penawaran : .........", "id": "Responsivitas Penawaran" },
  { "en": "Polimerisasi : Pembentukan Polimer = Depolimerisasi : .........", "id": "Penguraian Polimer" },
  { "en": "Depth of Field : Rentang Fokus Tajam = Bokeh : .........", "id": "Kualitas Blur Latar Belakang" },
  { "en": "Yurisdiksi : Wewenang Hukum = Imunitas : .........", "id": "Kekebalan Hukum" },
  { "en": "Revolusi Industri : Mekanisasi = Revolusi Digital : .........", "id": "Komputerisasi dan Internet" },
  { "en": "Lentikular : Awan Berbentuk Lensa = Mammatus : .........", "id": "Awan Berbentuk Kantung" },
  { "en": "Guilt by Association : Salah karena Asosiasi = No True Scotsman : .........", "id": "Mengubah Definisi untuk Menang Argumen" },
  { "en": "Diapause : Penundaan Perkembangan (Serangga) = Dormansi : .........", "id": "Masa Istirahat (Tumbuhan)" },
  { "en": "Sentriol : Pembelahan Sel Hewan = Dinding Sel : .........", "id": "Pelindung Sel Tumbuhan" },
  { "en": "Tombolo : Gundukan Pasir Penghubung Pulau = Spit : .........", "id": "Gundukan Pasir di Sepanjang Pantai" },
  { "en": "Router : Menghubungkan Jaringan = Switch : .........", "id": "Menghubungkan Perangkat dalam Satu Jaringan" },
  { "en": "Suku Kata : Unit Bunyi = Leksikon : .........", "id": "Perbendaharaan Kata" },
  { "en": "Kurva Phillips : Hubungan Inflasi dan Pengangguran = Kurva Laffer : .........", "id": "Hubungan Tarif Pajak dan Pendapatan" },
  { "en": "Esterifikasi : Pembentukan Ester = Saponifikasi : .........", "id": "Pembuatan Sabun" },
  { "en": "Golden Hour : Cahaya Pagi/Sore = Blue Hour : .........", "id": "Cahaya Sebelum Matahari Terbit/Setelah Terbenam" },
  { "en": "Subpoena : Panggilan Resmi Pengadilan = Waran : .........", "id": "Perintah Penangkapan/Penggeledahan" },
  { "en": "Pax Romana : Periode Damai Romawi = Belle Époque : .........", "id": "Periode Damai dan Sejahtera di Eropa" },
  { "en": "Anomali Air : Pemuaian Saat Mendingin = Tegangan Permukaan : .........", "id": "Kohesi Molekul Air" },
  { "en": "Texas Sharpshooter : Mencari Pola Setelah Fakta = Cherry Picking : .........", "id": "Memilih Data yang Mendukung Saja" },
  { "en": "Seleksi Alam : Bertahan Hidup yang Kuat = Hanyutan Genetik : .........", "id": "Perubahan Frekuensi Gen Acak" },
  { "en": "Klorofil : Pigmen Hijau = Karotenoid : .........", "id": "Pigmen Oranye/Kuning" },
  { "en": "Teras Laut : Bekas Garis Pantai = Dataran Aluvial : .........", "id": "Dataran Endapan Sungai" },
  { "en": "Cache : Penyimpanan Data Cepat = RAM : .........", "id": "Penyimpanan Kerja Sementara" },
  { "en": "Sintagma : Gabungan Kata Beraturan = Paradigma : .........", "id": "Hubungan Antar Kata yang Bisa Saling Menggantikan" },
  { "en": "Utilitas Marjinal : Kepuasan Tambahan = Biaya Marjinal : .........", "id": "Biaya Tambahan" },
  { "en": "Isotop : Jumlah Proton Sama = Isoton : .........", "id": "Jumlah Neutron Sama" },
  { "en": "Long Exposure : Merekam Gerakan = High-Speed Photography : .........", "id": "Membekukan Gerakan" },
  { "en": "Amicus Curiae : Sahabat Pengadilan = Pro Bono : .........", "id": "Bantuan Hukum Gratis" },
  { "en": "Zaman Pencerahan : Penekanan pada Akal = Zaman Romantisme : .........", "id": "Penekanan pada Emosi" },
  { "en": "Efek Coriolis : Pembelokan Angin = Arus Teluk : .........", "id": "Arus Hangat di Atlantik" },
  { "en": "Argument from Silence : Menyimpulkan dari Ketiadaan Bukti = Burden of Proof Fallacy : .........", "id": "Melempar Beban Pembuktian" },
  { "en": "Endemik : Khas Satu Wilayah = Kosmopolitan : .........", "id": "Tersebar di Seluruh Dunia" },
  { "en": "Sitokinesis : Pembelahan Sitoplasma = Kariokinesis : .........", "id": "Pembelahan Inti Sel" },
  { "en": "Kaldera : Kawah Vulkanik Besar = Dike : .........", "id": "Intrusi Batuan Beku Vertikal" },
  { "en": "Proxy Server : Perantara Jaringan = VPN : .........", "id": "Jaringan Pribadi Virtual" },
  { "en": "Alofon : Variasi Pengucapan Fonem = Alomorf : .........", "id": "Variasi Bentuk Morfem" },
  { "en": "Indeks Harga Konsumen : Mengukur Inflasi = PDB : .........", "id": "Mengukur Pertumbuhan Ekonomi" },
  { "en": "Gugus Fungsional : Penentu Sifat Kimia = Rantai Karbon : .........", "id": "Kerangka Molekul Organik" },
  { "en": "Rule of Thirds : Komposisi Foto = Leading Lines : .........", "id": "Garis Pemandu Mata" },
  { "en": "Preseden : Putusan Terdahulu sebagai Acuan = Stare Decisis : .........", "id": "Prinsip Mengikuti Preseden" },
  { "en": "Jalan Sutra : Jalur Perdagangan Kuno = Segitiga Emas : .........", "id": "Wilayah Produksi Opium" },
  { "en": "El Niño : Pemanasan Permukaan Laut Pasifik = La Niña : .........", "id": "Pendinginan Permukaan Laut Pasifik" },
  { "en": "Fallacy of Composition : Sifat Bagian Dianggap Sifat Keseluruhan = Fallacy of Division : .........", "id": "Sifat Keseluruhan Dianggap Sifat Bagian" },
  { "en": "Spesiasi : Pembentukan Spesies Baru = Kepunahan : .........", "id": "Hilangnya Spesies" },
  { "en": "Ribosom : Pabrik Protein = Mitokondria : .........", "id": "Pembangkit Energi Sel" },
  { "en": "Gorge : Ngarai Sempit = Canyon : .........", "id": "Ngarai Lebar" },
  { "en": "Bandwidth : Kapasitas Transfer Data = Latency : .........", "id": "Waktu Tunda Transfer Data" },
  { "en": "Homograf : Tulisan Sama, Bunyi dan Makna Beda = Homofon : .........", "id": "Bunyi Sama, Tulisan dan Makna Beda" },
  { "en": "Depresiasi : Penurunan Nilai Aset = Amortisasi : .........", "id": "Penghapusan Nilai Aset Tak Berwujud" },
  { "en": "Hidrolisis : Penguraian oleh Air = Fotolisis : .........", "id": "Penguraian oleh Cahaya" },
  { "en": "Panning : Mengikuti Objek Bergerak = Tilting : .........", "id": "Menggerakkan Kamera Vertikal" },
  { "en": "Fiat Justitia Ruat Caelum : Keadilan Harus Ditegakkan Walau Langit Runtuh = Ultimatum : .........", "id": "Tuntutan Terakhir" },
  { "en": "Perang Dingin : AS vs Uni Soviet = Perang Peloponnesia : .........", "id": "Athena vs Sparta" },
  { "en": "Angin Fohn : Angin Kering di Lereng Gunung = Angin Monsun : .........", "id": "Angin yang Berubah Arah Musiman" },
  { "en": "Affirming the Consequent : Kesalahan Logika Kondisional = Denying the Antecedent : .........", "id": "Kesalahan Logika 'Jika-Maka'" },
  { "en": "Kladistika : Klasifikasi Berdasarkan Leluhur = Taksonomi : .........", "id": "Ilmu Klasifikasi Makhluk Hidup" },
  { "en": "Nukleolus : Membuat Ribosom = Kromatin : .........", "id": "Kompleks DNA dan Protein" },
  { "en": "Sill : Intrusi Batuan Beku Horizontal = Lakolit : .........", "id": "Intrusi Berbentuk Kubah" },
  { "en": "Mencangkok : Menggabungkan Jaringan = Setek : .........", "id": "Memotong dan Menanam Bagian Tanaman" },
  { "en": "Quark : Komponen Proton = Elektron : .........", "id": "Partikel Lepton" },
  { "en": "Batu Rosetta : Kunci Hieroglif Mesir = Kode Hammurabi : .........", "id": "Hukum Babilonia Kuno" },
  { "en": "Jaringan Epitel : Pelapis Permukaan = Jaringan Ikat : .........", "id": "Penghubung dan Penopang" },
  { "en": "Cadenza : Solo Virtuoso di Akhir Konserto = Coda : .........", "id": "Bagian Penutup Komposisi" },
  { "en": "Confirmation Bias : Mencari Bukti yang Mendukung = Anchoring Bias : .........", "id": "Terpaku pada Informasi Pertama" },
  { "en": "Sentrifugasi : Memisahkan Berdasarkan Berat Jenis = Elektroforesis : .........", "id": "Memisahkan Berdasarkan Muatan Listrik" },
  { "en": "Distopia : Masyarakat Masa Depan yang Buruk = Utopia : .........", "id": "Masyarakat Ideal" },
  { "en": "Asimilasi : Bunyi Menjadi Mirip = Disimilasi : .........", "id": "Bunyi Menjadi Berbeda" },
  { "en": "ALU (Arithmetic Logic Unit) : Melakukan Perhitungan = Control Unit : .........", "id": "Mengatur Operasi Prosesor" },
  { "en": "Mens Rea : Niat Jahat = Actus Reus : .........", "id": "Tindakan Jahat" },
  { "en": "Persaingan Sempurna : Produk Homogen = Persaingan Monopolistik : .........", "id": "Produk Terdiferensiasi" },
  { "en": "Stolon : Batang Menjalar di Atas Tanah = Rizoma : .........", "id": "Batang Menjalar di Bawah Tanah" },
  { "en": "Foton : Partikel Cahaya = Fonon : .........", "id": "Kuantum Getaran dalam Kisi Kristal" },
  { "en": "Gulungan Laut Mati : Naskah Alkitab Ibrani = Tentara Terakota : .........", "id": "Patung Penjaga Makam Kaisar Qin" },
  { "en": "Jaringan Saraf : Menghantar Impuls = Jaringan Otot : .........", "id": "Kontraksi dan Gerakan" },
  { "en": "Da Capo : Kembali dari Awal = Dal Segno : .........", "id": "Kembali dari Tanda" },
  { "en": "Dunning-Kruger Effect : Merasa Lebih Kompeten dari Sebenarnya = Impostor Syndrome : .........", "id": "Merasa Tidak Kompeten Meski Sukses" },
  { "en": "Titrasi : Mengetahui Konsentrasi = Spektroskopi : .........", "id": "Menganalisis Interaksi Cahaya dan Materi" },
  { "en": "Satir : Sindiran Sosial = Parabel : .........", "id": "Cerita dengan Pelajaran Moral" },
  { "en": "Metatesis : Pertukaran Posisi Bunyi = Apokop : .........", "id": "Penghilangan Bunyi di Akhir Kata" },
  { "en": "Register : Penyimpanan Cepat di CPU = Cache : .........", "id": "Memori Perantara CPU dan RAM" },
  { "en": "Corpus Delicti : Bukti Terjadinya Kejahatan = Habeas Corpus : .........", "id": "Hak untuk Diadili" },
  { "en": "Elastisitas Silang : Hubungan Dua Barang = Pendapatan Nasional Bruto : .........", "id": "Total Pendapatan Negara" },
  { "en": "Okulasi : Penempelan Mata Tunas = Kultur Jaringan : .........", "id": "Perbanyakan Sel/Jaringan di Laboratorium" },
  { "en": "Boson : Partikel Pembawa Gaya = Fermion : .........", "id": "Partikel Materi" },
  { "en": "Zaman Pencerahan : Logika dan Sains = Zaman Romantik : .........", "id": "Emosi dan Alam" },
  { "en": "Otot Polos : Bekerja Tanpa Sadar = Otot Lurik : .........", "id": "Bekerja Secara Sadar" },
  { "en": "Fermata : Menahan Nada = Legato : .........", "id": "Memainkan Nada Tersambung" },
  { "en": "Hindsight Bias : 'Sudah Kuduga' Setelah Kejadian = Availability Heuristic : .........", "id": "Mengandalkan Informasi yang Mudah Diingat" },
  { "en": "Kromatografi : Pemisahan Berdasarkan Kecepatan Gerak = Distilasi : .........", "id": "Pemisahan Berdasarkan Titik Didih" },
  { "en": "Asonansi : Pengulangan Vokal = Konsonansi : .........", "id": "Pengulangan Konsonan" },
  { "en": "Sinkop : Penghilangan Bunyi di Tengah Kata = Protesis : .........", "id": "Penambahan Bunyi di Awal Kata" },
  { "en": "BIOS : Program Inisialisasi Perangkat Keras = Sistem Operasi : .........", "id": "Manajer Sumber Daya Komputer" },
  { "en": "Stare Decisis : Mengikuti Putusan Sebelumnya = Ratio Decidendi : .........", "id": "Alasan Hukum di Balik Putusan" },
  { "en": "Depresi Besar : Krisis Ekonomi 1929 = Krisis Subprime Mortgage : .........", "id": "Krisis Finansial 2008" },
  { "en": "Tumbuhan Xerofit : Adaptasi di Lingkungan Kering = Tumbuhan Hidrofit : .........", "id": "Adaptasi di Lingkungan Air" },
  { "en": "Hadron : Terdiri dari Quark = Lepton : .........", "id": "Partikel Fundamental (cth: Elektron)" },
  { "en": "Revolusi Neolitik : Lahirnya Pertanian = Revolusi Industri : .........", "id": "Lahirnya Manufaktur" },
  { "en": "Osteoblas : Sel Pembentuk Tulang = Osteoklas : .........", "id": "Sel Perusak Tulang" },
  { "en": "Atonalitas : Tanpa Kunci Nada = Polifoni : .........", "id": "Banyak Suara Independen" },
  { "en": "Survivorship Bias : Fokus pada yang 'Selamat' = Self-Serving Bias : .........", "id": "Menyalahkan Eksternal, Mengklaim Sukses Internal" },
  { "en": "Sublimasi : Perubahan Padat ke Gas = Deposisi : .........", "id": "Perubahan Gas ke Padat" },
  { "en": "Ironi Dramatis : Penonton Tahu, Tokoh Tidak = Ironi Verbal : .........", "id": "Ucapan Berlawanan Makna" },
  { "en": "Epentesis : Penyisipan Bunyi di Tengah Kata = Paragoge : .........", "id": "Penambahan Bunyi di Akhir Kata" },
  { "en": "Kernel : Inti Sistem Operasi = Shell : .........", "id": "Antarmuka Pengguna ke Kernel" },
  { "en": "Obiter Dicta : Komentar Sampingan Hakim = Ad Hoc : .........", "id": "Untuk Kasus Tertentu" },
  { "en": "Perdagangan Barter : Pertukaran Barang = Ekonomi Pasar : .........", "id": "Mekanisme Harga" },
  { "en": "Tumbuhan Higrofit : Adaptasi di Lingkungan Lembab = Tumbuhan Halofit : .........", "id": "Adaptasi di Lingkungan Asin" },
  { "en": "Gluon : Pembawa Gaya Kuat = Graviton : .........", "id": "Partikel Hipotetis Pembawa Gravitasi" },
  { "en": "Perang Dingin : Kompetisi Ideologi = Perang Dunia : .........", "id": "Konflik Militer Global" },
  { "en": "Kondrosit : Sel Tulang Rawan = Miosit : .........", "id": "Sel Otot" },
  { "en": "Homofoni : Melodi Utama dengan Iringan = Monofoni : .........", "id": "Satu Melodi Tanpa Iringan" },
  { "en": "Fundamental Attribution Error : Menekankan Kepribadian, Mengabaikan Situasi = Actor-Observer Bias : .........", "id": "Menyalahkan Situasi (Diri), Kepribadian (Orang Lain)" },
  { "en": "Elektrolisis : Mengurai Senyawa dengan Listrik = Galvanisasi : .........", "id": "Melapisi Logam dengan Logam Lain" },
  { "en": "Solilokui : Berbicara pada Diri Sendiri = Aside : .........", "id": "Komentar Singkat ke Penonton" },
  { "en": "Sandhi : Perubahan Bunyi Akibat Gabungan Kata = Vokal Harmoni : .........", "id": "Kesesuaian Vokal dalam Kata" },
  { "en": "Driver : Perangkat Lunak untuk Perangkat Keras = Firmware : .........", "id": "Perangkat Lunak Tertanam di Perangkat Keras" },
  { "en": "Force Majeure : Keadaan Kahar = Pacta Sunt Servanda : .........", "id": "Perjanjian Mengikat sebagai Undang-Undang" },
  { "en": "Proteksionisme : Melindungi Industri Dalam Negeri = Liberalisasi : .........", "id": "Membuka Perdagangan" },
  { "en": "Etiolasi : Pertumbuhan di Tempat Gelap = Fototropisme : .........", "id": "Pertumbuhan Mengarah ke Cahaya" },
  { "en": "Antimateri : Muatan Berlawanan = Materi Gelap : .........", "id": "Materi Tak Berinteraksi dengan Cahaya" },
  { "en": "Jalan Sutra : Perdagangan Darat = Jalur Rempah : .........", "id": "Perdagangan Laut" },
  { "en": "Neuron : Sel Saraf = Glia : .........", "id": "Sel Pendukung Saraf" },
  { "en": "Konsonan : Bunyi Udara Terhambat = Vokal : .........", "id": "Bunyi Udara Tak Terhambat" },
  { "en": "Status Quo Bias : Memilih yang Sudah Ada = Bandwagon Effect : .........", "id": "Mengikuti Pilihan Mayoritas" },
  { "en": "Anoda : Elektroda Positif = Katoda : .........", "id": "Elektroda Negatif" },
  { "en": "Apostrof : Menandai Penghilangan Huruf = Elipsis : .........", "id": "Menandai Penghilangan Kata" },
  { "en": "Haplologi : Penghilangan Suku Kata yang Mirip = Metatesis Kuantitatif : .........", "id": "Pertukaran Panjang Vokal" },
  { "en": "Open Source : Kode Sumber Terbuka = Proprietary : .........", "id": "Perangkat Lunak Berpemilik" },
  { "en": "In Absentia : Tanpa Kehadiran Terdakwa = Ex Parte : .........", "id": "Atas Nama Satu Pihak" },
  { "en": "Invisible Hand : Mekanisme Pasar Bebas = Laissez-Faire : .........", "id": "Tanpa Campur Tangan Pemerintah" },
  { "en": "Geotropisme : Pertumbuhan Menuju Gravitasi = Tigmotropisme : .........", "id": "Pertumbuhan Merespon Sentuhan" },
  { "en": "Dualisme Gelombang-Partikel : Sifat Ganda Cahaya = Prinsip Ketidakpastian Heisenberg : .........", "id": "Batas Pengukuran Posisi dan Momentum" },
  { "en": "Perang Seratus Tahun : Inggris vs Perancis = Perang Candu : .........", "id": "Inggris vs Tiongkok" },
  { "en": "Eritrosit : Sel Darah Merah = Leukosit : .........", "id": "Sel Darah Putih" },
  { "en": "Orkestrasi : Penataan Instrumen dalam Orkestra = Aransemen : .........", "id": "Adaptasi Komposisi Musik" },
  { "en": "Zero-Sum Game : Kemenangan Satu Pihak adalah Kekalahan Pihak Lain = Positive-Sum Game : .........", "id": "Semua Pihak Bisa Menang" },
  { "en": "Redoks : Reaksi Reduksi dan Oksidasi = Asam-Basa : .........", "id": "Reaksi Transfer Proton" },
  { "en": "Arketipe : Pola Dasar Karakter = Stereotipe : .........", "id": "Generalisasi Berlebihan" },
  { "en": "Disimilasi : Bunyi yang Berdekatan Menjadi Beda = Rhotasisme : .........", "id": "Perubahan Bunyi Menjadi 'r'" },
  { "en": "GUI (Graphical User Interface) : Interaksi Visual = CLI (Command-Line Interface) : .........", "id": "Interaksi Berbasis Teks" },
  { "en": "Lex Talionis : Hukuman Setimpal (Mata ganti Mata) = Presumption of Innocence : .........", "id": "Asas Praduga Tak Bersalah" },
  { "en": "Tragedi Komun : Kerusakan Sumber Daya Bersama = Eksternalitas : .........", "id": "Dampak Samping Ekonomi" },
  { "en": "Vernalisasi : Perangsangan Bunga oleh Suhu Rendah = Fotoperiodisme : .........", "id": "Respon Tumbuhan terhadap Panjang Hari" },
  { "en": "Superposisi Kuantum : Berada di Banyak Keadaan Sekaligus = Keterkaitan Kuantum : .........", "id": "Keterhubungan Antar Partikel Jarak Jauh" },
  { "en": "Reformasi Protestan : Martin Luther = Kontra-Reformasi : .........", "id": "Respon Gereja Katolik" },
  { "en": "Trombosit : Keping Darah untuk Pembekuan = Plasma : .........", "id": "Cairan Darah" },
  { "en": "Sonoritas : Tingkat Kenyaringan Bunyi = Intonasi : .........", "id": "Nada Kalimat" },
  { "en": "Framing Effect : Pengaruh dari Cara Informasi Disajikan = Loss Aversion : .........", "id": "Kecenderungan Menghindari Kerugian" },
  { "en": "Korosi : Kerusakan Logam akibat Reaksi Kimia = Elektrolit : .........", "id": "Zat yang Menghantarkan Listrik dalam Larutan" },
  { "en": "Kiasmus : Struktur Kalimat Terbalik (A-B-B-A) = Zeugma : .........", "id": "Satu Kata untuk Dua Frasa dengan Makna Beda" },
  { "en": "Sandhi Vokal : Peleburan Vokal = Sandhi Konsonan : .........", "id": "Peleburan Konsonan" },
  { "en": "Malware : Perangkat Lunak Berbahaya = Ransomware : .........", "id": "Malware yang Meminta Tebusan" },
  { "en": "Etiologi : Studi Penyebab Penyakit = Patogenesis : .........", "id": "Studi Mekanisme Perkembangan Penyakit" },
  { "en": "Entropi : Ukuran Ketidakteraturan = Entalpi : .........", "id": "Ukuran Kandungan Panas Total Sistem" },
  { "en": "Restorasi Meiji : Modernisasi Jepang = Revolusi Bolshevik : .........", "id": "Komunisme di Rusia" },
  { "en": "Taksonomi Linnaeus : Berbasis Morfologi = Kladistika : .........", "id": "Berbasis Hubungan Evolusioner" },
  { "en": "Skala Mayor : Terdengar Ceria = Skala Minor : .........", "id": "Terdengar Sedih" },
  { "en": "Tahap Sensorimotor Piaget : Belajar Lewat Indra = Tahap Operasional Formal : .........", "id": "Berpikir Abstrak dan Logis" },
  { "en": "Hibridisasi : Percampuran Orbital Atom = Geometri Molekul : .........", "id": "Susunan Tiga Dimensi Atom" },
  { "en": "Limerick : Puisi Lima Baris Jenaka = Villanelle : .........", "id": "Puisi 19 Baris dengan Pola Pengulangan" },
  { "en": "Bahasa Austronesia : Bahasa Indonesia = Bahasa Indo-Eropa : .........", "id": "Bahasa Inggris" },
  { "en": "Model Waterfall : Pengembangan Perangkat Lunak Sekuensial = Model Agile : .........", "id": "Pengembangan Iteratif dan Inkremental" },
  { "en": "Ganti Rugi Kompensasi : Mengganti Kerugian Nyata = Ganti Rugi Punitive : .........", "id": "Hukuman untuk Memberi Efek Jera" },
  { "en": "Indeks Gini : Ukuran Ketimpangan Pendapatan = PDB per Kapita : .........", "id": "Ukuran Kemakmuran Rata-rata" },
  { "en": "Morfologi : Studi Bentuk = Fisiologi : .........", "id": "Studi Fungsi" },
  { "en": "Hukum Termodinamika Pertama : Kekekalan Energi = Hukum Termodinamika Kedua : .........", "id": "Peningkatan Entropi" },
  { "en": "Revolusi Prancis : Kejatuhan Monarki = Revolusi Amerika : .........", "id": "Kemerdekaan dari Kolonialisme" },
  { "en": "Domen : Tingkat Taksonomi Tertinggi = Kingdom : .........", "id": "Tingkat di Bawah Domen" },
  { "en": "Modus Ionian : Sama dengan Skala Mayor = Modus Dorian : .........", "id": "Skala Minor dengan Nada Keenam Naik" },
  { "en": "Teori Erikson : Krisis Psikososial = Teori Freud : .........", "id": "Tahap Psikoseksual" },
  { "en": "Ikatan Sigma : Tumpang Tindih Orbital Langsung = Ikatan Pi : .........", "id": "Tumpang Tindih Orbital Samping" },
  { "en": "Sestina : Puisi dengan Enam Stanza Enam Baris = Soneta : .........", "id": "Puisi 14 Baris" },
  { "en": "Bahasa Roman : Bahasa Prancis = Bahasa Slavia : .........", "id": "Bahasa Rusia" },
  { "en": "Scrum : Kerangka Kerja Agile = Sprint : .........", "id": "Siklus Kerja dalam Scrum" },
  { "en": "Jus Cogens : Norma Hukum Internasional yang Mengikat = Perjanjian : .........", "id": "Kesepakatan Antar Negara" },
  { "en": "Kurva Penawaran : Hubungan Harga dan Kuantitas Ditawarkan = Kurva Permintaan : .........", "id": "Hubungan Harga dan Kuantitas Diminta" },
  { "en": "Histologi : Jaringan = Sitologi : .........", "id": "Sel" },
  { "en": "Gerak Brown : Gerak Acak Partikel = Osmosis : .........", "id": "Perpindahan Pelarut Lewat Membran Semipermeabel" },
  { "en": "Zaman Helenistik : Penyebaran Budaya Yunani = Zaman Pencerahan : .........", "id": "Dominasi Akal dan Ilmu Pengetahuan" },
  { "en": "Spesies Kunci : Berdampak Besar pada Ekosistem = Spesies Invasif : .........", "id": "Spesies Asing yang Merusak Ekosistem" },
  { "en": "Skala Pentatonik : Lima Nada = Skala Diatonik : .........", "id": "Tujuh Nada" },
  { "en": "Teori Kohlberg : Perkembangan Moral = Teori Vygotsky : .........", "id": "Peran Interaksi Sosial dalam Pembelajaran" },
  { "en": "Elektronegativitas : Kemampuan Menarik Elektron = Afinitas Elektron : .........", "id": "Energi yang Dilepaskan Saat Menerima Elektron" },
  { "en": "Haiku : Pola Suku Kata 5-7-5 = Tanka : .........", "id": "Pola Suku Kata 5-7-5-7-7" },
  { "en": "Bahasa Jermanik : Bahasa Belanda = Bahasa Keltik : .........", "id": "Bahasa Irlandia" },
  { "en": "Kanban : Visualisasi Alur Kerja = Lean : .........", "id": "Metodologi Mengurangi Pemborosan" },
  { "en": "Ekstradisi : Penyerahan Pelaku Kejahatan = Suaka : .........", "id": "Perlindungan Politik" },
  { "en": "Surplus Konsumen : Keuntungan Pembeli = Surplus Produsen : .........", "id": "Keuntungan Penjual" },
  { "en": "Embriologi : Perkembangan Embrio = Teratologi : .........", "id": "Studi Kelainan Perkembangan" },
  { "en": "Efek Doppler : Perubahan Frekuensi Gelombang = Dentuman Sonik : .........", "id": "Gelombang Kejut dari Objek Melebihi Kecepatan Suara" },
  { "en": "Gerakan Non-Blok : Tidak Memihak Blok Barat/Timur = Pakta Warsawa : .........", "id": "Aliansi Militer Blok Timur" },
  { "en": "Relung Ekologis : Peran Fungsional Spesies = Habitat : .........", "id": "Alamat Fisik Spesies" },
  { "en": "Temperamen : Dasar Biologis Kepribadian = Karakter : .........", "id": "Kepribadian yang Terbentuk dari Pengalaman" },
  { "en": "Bilangan Oksidasi : Muatan Hipotetis Atom = Valensi : .........", "id": "Elektron untuk Berikatan" },
  { "en": "Pantun : Berpola a-b-a-b = Gurindam : .........", "id": "Berpola a-a" },
  { "en": "Bahasa Isolat : Tidak Terkait Bahasa Lain (cth: Basque) = Rumpun Bahasa : .........", "id": "Kelompok Bahasa dari Nenek Moyang Bersama" },
  { "en": "Refactoring : Memperbaiki Struktur Kode = Komentari : .........", "id": "Memberi Penjelasan pada Kode" },
  { "en": "Persona Non Grata : Orang yang Tidak Diinginkan = Duta Besar : .........", "id": "Perwakilan Diplomatik Tertinggi" },
  { "en": "Deadweight Loss : Hilangnya Efisiensi Ekonomi = Efisiensi Pareto : .........", "id": "Kondisi Optimal Tanpa Merugikan Pihak Lain" },
  { "en": "Anatomi Komparatif : Perbandingan Struktur Tubuh = Biogeografi : .........", "id": "Studi Distribusi Spesies" },
  { "en": "Radiasi Benda Hitam : Emisi Termal Ideal = Efek Fotolistrik : .........", "id": "Pelepasan Elektron Akibat Cahaya" },
  { "en": "Kebijakan Apartheid : Segregasi Ras di Afrika Selatan = Gerakan Hak Sipil : .........", "id": "Perjuangan Kesetaraan Ras di AS" },
  { "en": "Simbiosis : Interaksi Jangka Panjang = Kompetisi : .........", "id": "Perebutan Sumber Daya" },
  { "en": "Kognisi : Proses Berpikir = Metakognisi : .........", "id": "Berpikir Tentang Proses Berpikir Sendiri" },
  { "en": "Kiralitas : Molekul Bayangan Cermin Tak Superimposabel = Isomer : .........", "id": "Molekul dengan Rumus Sama, Struktur Beda" },
  { "en": "Ode : Puisi untuk Menghormati Sesuatu = Epigram : .........", "id": "Pernyataan Puitis Singkat dan Cerdas" },
  { "en": "Kreol : Bahasa Campuran yang Jadi Bahasa Ibu = Pijin : .........", "id": "Bahasa Campuran Sederhana (Bukan Bahasa Ibu)" },
  { "en": "DevOps : Integrasi Pengembangan dan Operasi = QA (Quality Assurance) : .........", "id": "Penjaminan Kualitas Perangkat Lunak" },
  { "en": "Ratifikasi : Pengesahan Perjanjian Internasional = Protokol : .........", "id": "Tambahan atau Amandemen Perjanjian" },
  { "en": "Oligopoli : Pasar Dikuasai Beberapa Perusahaan = Duopoli : .........", "id": "Pasar Dikuasai Dua Perusahaan" },
  { "en": "Paleontologi : Fosil = Arkeologi : .........", "id": "Artefak" },
  { "en": "Prinsip Ekuivalensi : Gravitasi dan Akselerasi = Relativitas Khusus : .........", "id": "Hukum Fisika Sama untuk Semua Pengamat Inersial" },
  { "en": "Perestroika : Restrukturisasi Ekonomi Soviet = Glasnost : .........", "id": "Keterbukaan Politik Soviet" },
  { "en": "Suksesi Primer : Dimulai dari Lingkungan Tanpa Kehidupan = Suksesi Sekunder : .........", "id": "Dimulai dari Lingkungan yang Pernah Ada Kehidupan" },
  { "en": "Zona Perkembangan Proksimal (Vygotsky) : Jarak antara Belajar Mandiri dan dengan Bantuan = Scaffolding : .........", "id": "Bantuan yang Diberikan Guru/Teman" },
  { "en": "Entalpi Pembentukan : Energi untuk Membentuk Senyawa = Entalpi Penguraian : .........", "id": "Energi untuk Mengurai Senyawa" },
  { "en": "Balada : Puisi Naratif tentang Cinta atau Tragedi = Elegi : .........", "id": "Puisi tentang Kesedihan atau Kematian" },
  { "en": "Lingua Franca : Bahasa Komunikasi Antar Kelompok Berbeda = Bahasa Resmi : .........", "id": "Bahasa yang Digunakan dalam Pemerintahan" },
  { "en": "User Story : Deskripsi Fitur dari Perspektif Pengguna = Acceptance Criteria : .........", "id": "Syarat yang Harus Dipenuhi Fitur" },
  { "en": "Jus Soli : Kewarganegaraan Berdasarkan Tempat Lahir = Jus Sanguinis : .........", "id": "Kewarganegaraan Berdasarkan Keturunan" },
  { "en": "Efek Substitusi : Beralih ke Barang Lebih Murah = Efek Pendapatan : .........", "id": "Perubahan Permintaan Akibat Perubahan Daya Beli" },
  { "en": "Taksonomi : Klasifikasi = Filogeni : .........", "id": "Sejarah Evolusi" },
  { "en": "Difraksi : Pembelokan Gelombang di Sekitar Penghalang = Interferensi : .........", "id": "Penggabungan Gelombang" },
  { "en": "Doktrin Truman : Pembendungan Komunisme = Rencana Marshall : .........", "id": "Bantuan Ekonomi AS untuk Eropa" },
  { "en": "Komensalisme : Satu Untung, Lainnya Tidak Terpengaruh = Amensalisme : .........", "id": "Satu Rugi, Lainnya Tidak Terpengaruh" },
  { "en": "Kecerdasan Cair : Kemampuan Memecahkan Masalah Baru = Kecerdasan Kristal : .........", "id": "Pengetahuan dan Keterampilan dari Pengalaman" },
  { "en": "Hukum Hess : Perubahan Entalpi Konstan = Aturan Hund : .........", "id": "Pengisian Orbital dengan Spin Paralel" },
  { "en": "Alegori : Cerita dengan Makna Simbolis = Mitos : .........", "id": "Cerita Tradisional tentang Dewa atau Pahlawan" },
  { "en": "Diglosia : Penggunaan Dua Varietas Bahasa dalam Konteks Berbeda = Bilingualisme : .........", "id": "Kemampuan Menggunakan Dua Bahasa" },
  { "en": "Unit Testing : Menguji Bagian Kecil Kode = Integration Testing : .........", "id": "Menguji Gabungan Beberapa Bagian" },
  { "en": "Non-refoulement : Larangan Mengembalikan Pengungsi ke Negara Berbahaya = Suaka Politik : .........", "id": "Perlindungan dari Penindasan Politik" },
  { "en": "Asymmetric Information : Salah Satu Pihak Punya Informasi Lebih = Perfect Information : .........", "id": "Semua Pihak Punya Informasi Lengkap" },
  { "en": "Homologi : Struktur Sama, Fungsi Beda (Leluhur Sama) = Analogi : .........", "id": "Struktur Beda, Fungsi Sama (Leluhur Beda)" },
  { "en": "Polarisasi : Orientasi Getaran Gelombang = Refraksi : .........", "id": "Pembiasan Gelombang" },
  { "en": "Detente : Meredanya Ketegangan (Perang Dingin) = Brinkmanship : .........", "id": "Politik di Ambang Perang" },
  { "en": "Biomagnifikasi : Peningkatan Konsentrasi Racun di Rantai Makanan = Bioakumulasi : .........", "id": "Akumulasi Racun dalam Satu Organisme" },
  { "en": "Nature vs Nurture : Perdebatan Faktor Genetik vs Lingkungan = Free Will vs Determinism : .........", "id": "Perdebatan Kehendak Bebas vs Takdir" },
  { "en": "Aturan Oktet : Kecenderungan Atom Memiliki 8 Elektron Valensi = Ikatan Hidrogen : .........", "id": "Interaksi Antarmolekul Melibatkan Hidrogen" },
  { "en": "Epilog : Bagian Penutup setelah Cerita Selesai = Prolog : .........", "id": "Bagian Pembuka Sebelum Cerita Dimulai" },
  { "en": "Isoglos : Batas Geografis Fitur Bahasa = Rumpun Bahasa : .........", "id": "Kelompok Bahasa dengan Asal yang Sama" },
  { "en": "End-to-End Testing : Menguji Seluruh Alur Aplikasi = System Testing : .........", "id": "Menguji Sistem Terintegrasi secara Keseluruhan" },
  { "en": "Dwi Kewarganegaraan : Memiliki Dua Kewarganegaraan = Apatride : .........", "id": "Tidak Memiliki Kewarganegaraan" },
  { "en": "Utilitas : Ukuran Kepuasan = Profit : .........", "id": "Ukuran Keuntungan Finansial" },
  { "en": "Vestigial : Struktur Sisa Evolusi = Atavisme : .........", "id": "Munculnya Kembali Sifat Leluhur yang Hilang" },
  { "en": "Superkonduktivitas : Hambatan Listrik Nol = Superfluiditas : .........", "id": "Viskositas Nol" },
  { "en": "Konferensi Yalta : Pembagian Eropa Pasca-PD II = Konferensi Potsdam : .........", "id": "Administrasi Jerman Pasca-Perang" },
  { "en": "Piramida Energi : Aliran Energi Antar Tingkat Trofik = Jaring-jaring Makanan : .........", "id": "Interaksi Makan-Dimakan yang Kompleks" },
  { "en": "Classical Conditioning (Pavlov) : Asosiasi Stimulus = Operant Conditioning (Skinner) : .........", "id": "Asosiasi Perilaku dan Konsekuensi" },
  { "en": "Gaya van der Waals : Interaksi Lemah Antarmolekul = Ikatan Kovalen : .........", "id": "Ikatan Kuat Dalam Molekul" },
  { "en": "MRI (Magnetic Resonance Imaging) : Menggunakan Medan Magnet = CT Scan (Computed Tomography) : .........", "id": "Menggunakan Sinar-X" },
  { "en": "Gaya Elektromagnetik : Diperantarai Foton = Gaya Nuklir Kuat : .........", "id": "Diperantarai Gluon" },
  { "en": "Magna Carta : Pembatasan Kekuasaan Raja Inggris = Deklarasi Hak Asasi Manusia : .........", "id": "Pernyataan Universal Hak Individu" },
  { "en": "Difusi : Gerak dari Konsentrasi Tinggi ke Rendah = Transpor Aktif : .........", "id": "Gerak Melawan Gradien Konsentrasi (Butuh Energi)" },
  { "en": "Polifoni : Banyak Melodi Independen Bersamaan = Monodi : .........", "id": "Satu Melodi Dominan dengan Iringan Sederhana" },
  { "en": "Memori Episodik : Ingatan tentang Pengalaman Pribadi = Memori Semantik : .........", "id": "Ingatan tentang Fakta Umum" },
  { "en": "Titik Tripel : Tiga Fase Materi Bersamaan = Titik Kritis : .........", "id": "Batas di mana Fase Cair dan Gas Tak Terbedakan" },
  { "en": "Narator Terpercaya : Menceritakan Secara Objektif = Narator Tidak Terpercaya : .........", "id": "Menceritakan Secara Subjektif atau Menyesatkan" },
  { "en": "Hipotesis Periode Kritis : Batas Usia Belajar Bahasa = Universal Grammar (Chomsky) : .........", "id": "Kemampuan Tata Bahasa Bawaan" },
  { "en": "Singleton Pattern : Menjamin Satu Instansi Objek = Factory Pattern : .........", "id": "Membuat Objek Tanpa Menentukan Kelasnya" },
  { "en": "Paten : Melindungi Invensi = Hak Cipta : .........", "id": "Melindungi Karya Seni dan Sastra" },
  { "en": "Siklus Bisnis : Pola Ekspansi dan Kontraksi Ekonomi = Resesi : .........", "id": "Fase Penurunan dalam Siklus Bisnis" },
  { "en": "PET Scan (Positron Emission Tomography) : Menggunakan Pelacak Radioaktif = USG (Ultrasonografi) : .........", "id": "Menggunakan Gelombang Suara" },
  { "en": "Gaya Nuklir Lemah : Peluruhan Radioaktif = Gaya Gravitasi : .........", "id": "Tarik Menarik Antar Massa" },
  { "en": "Perjanjian Westphalia : Konsep Kedaulatan Negara = Kongres Wina : .........", "id": "Menata Ulang Peta Politik Eropa Pasca-Napoleon" },
  { "en": "Kanal Ion : Memfasilitasi Difusi Ion = Pompa Natrium-Kalium : .........", "id": "Contoh Transpor Aktif" },
  { "en": "Musik Program : Musik yang Menceritakan Sesuatu = Musik Absolut : .........", "id": "Musik Tanpa Referensi Eksternal" },
  { "en": "Memori Prosedural : Ingatan Keterampilan ('Bagaimana') = Memori Deklaratif : .........", "id": "Ingatan Fakta ('Apa')" },
  { "en": "Plasma : Keadaan Materi Keempat (Gas Terionisasi) = Kondensat Bose-Einstein : .........", "id": "Keadaan Materi Kelima (Suhu Sangat Rendah)" },
  { "en": "Stream of Consciousness : Aliran Pikiran Tanpa Filter = Solilokui : .........", "id": "Tokoh Berbicara pada Diri Sendiri di Panggung" },
  { "en": "Pemerolehan Bahasa : Proses Belajar Bahasa Pertama = Pembelajaran Bahasa : .........", "id": "Proses Belajar Bahasa Kedua" },
  { "en": "Observer Pattern : Notifikasi Otomatis pada Objek Lain = Strategy Pattern : .........", "id": "Memungkinkan Pergantian Algoritma saat Runtime" },
  { "en": "Merek Dagang : Melindungi Nama atau Logo = Rahasia Dagang : .........", "id": "Melindungi Informasi Bisnis Rahasia" },
  { "en": "Stagflasi : Inflasi Tinggi, Pertumbuhan Rendah = Hiperinflasi : .........", "id": "Inflasi yang Sangat Cepat dan Tak Terkendali" },
  { "en": "Biopsi : Pengambilan Sampel Jaringan = Endoskopi : .........", "id": "Pemeriksaan Visual Organ Dalam" },
  { "en": "Momentum Sudut : Ukuran Rotasi = Momentum Linear : .........", "id": "Ukuran Gerak Lurus" },
  { "en": "Reconquista : Perebutan Kembali Semenanjung Iberia oleh Kristen = Perang Seratus Tahun : .........", "id": "Konflik antara Inggris dan Prancis" },
  { "en": "Akuaporin : Kanal Khusus untuk Air = Osmoregulasi : .........", "id": "Pengaturan Keseimbangan Air" },
  { "en": "Leitmotif : Tema Musik yang Mewakili Karakter/Ide = Ostinato : .........", "id": "Pola Musik yang Diulang Terus-menerus" },
  { "en": "Priming : Pengaruh Stimulus Sebelumnya pada Respon = Efek Serial Posisi : .........", "id": "Kecenderungan Mengingat Awal dan Akhir Daftar" },
  { "en": "Cairan Superkritis : Properti antara Cair dan Gas = Gas Ideal : .........", "id": "Gas Hipotetis yang Mengikuti Hukum Gas Sempurna" },
  { "en": "Suspense : Ketegangan Menunggu Sesuatu Terjadi = Ironi : .........", "id": "Kontradiksi antara Harapan dan Kenyataan" },
  { "en": "Pidgin : Bahasa Kontak Sederhana = Kreol : .........", "id": "Pidgin yang Telah Menjadi Bahasa Ibu" },
  { "en": "MVC (Model-View-Controller) : Pola Arsitektur Perangkat Lunak = API (Application Programming Interface) : .........", "id": "Antarmuka untuk Interaksi Antar Perangkat Lunak" },
  { "en": "Indikasi Geografis : Melindungi Produk Khas Daerah = Desain Industri : .........", "id": "Melindungi Tampilan Estetis Produk" },
  { "en": "Depresi : Penurunan Ekonomi Jangka Panjang = Resesi : .........", "id": "Penurunan Ekonomi Jangka Pendek (Dua Kuartal)" },
  { "en": "Kultur Darah : Mendeteksi Bakteri dalam Darah = Tes ELISA : .........", "id": "Mendeteksi Antibodi atau Antigen" },
  { "en": "Inersia : Kecenderungan Benda Menolak Perubahan Gerak = Torsi : .........", "id": "Gaya yang Menyebabkan Rotasi" },
  { "en": "Sistem Feodal : Berbasis Kepemilikan Tanah = Sistem Merkantilisme : .........", "id": "Berbasis Perdagangan dan Akumulasi Logam Mulia" },
  { "en": "Potensial Aksi : Sinyal Listrik di Neuron = Potensial Istirahat : .........", "id": "Kondisi Neuron Saat Tidak Aktif" },
  { "en": "Kromatikisme : Penggunaan Nada di Luar Skala = Diatonik : .........", "id": "Penggunaan Nada dalam Skala Mayor/Minor" },
  { "en": "Efek Plasebo : Perbaikan Kondisi Akibat Sugesti = Efek Nocebo : .........", "id": "Perburukan Kondisi Akibat Sugesti Negatif" },
  { "en": "Fluida Newtonia : Viskositas Konstan = Fluida Non-Newtonia : .........", "id": "Viskositas Berubah Tergantung Tekanan" },
  { "en": "Verisimilitude : Kesan Realistis dalam Fiksi = Deus Ex Machina : .........", "id": "Penyelesaian Masalah yang Tiba-tiba dan Tidak Realistis" },
  { "en": "Neurolinguistik : Hubungan Bahasa dan Otak = Psikolinguistik : .........", "id": "Proses Mental dalam Berbahasa" },
  { "en": "Refactoring : Membersihkan Kode Tanpa Mengubah Fungsi = Debugging : .........", "id": "Mencari dan Memperbaiki Kesalahan dalam Kode" },
  { "en": "Gugatan Class Action : Gugatan oleh Kelompok = Litigasi : .........", "id": "Proses Penyelesaian Sengketa di Pengadilan" },
  { "en": "PDB Riil : Disesuaikan dengan Inflasi = PDB Nominal : .........", "id": "Tidak Disesuaikan dengan Inflasi" },
  { "en": "Lumbal Pungsi : Pengambilan Cairan Serebrospinal = Kateterisasi Jantung : .........", "id": "Pemeriksaan Pembuluh Darah Jantung" },
  { "en": "Frekuensi Resonansi : Frekuensi Getaran Alami = Redaman : .........", "id": "Pengurangan Amplitudo Getaran" },
  { "en": "Diplomasi Kapal Perang : Demonstrasi Kekuatan Militer = Diplomasi Publik : .........", "id": "Mempengaruhi Opini Publik Asing" },
  { "en": "Plasmolisis : Lepasnya Membran Sel dari Dinding Sel Tumbuhan = Turgor : .........", "id": "Tekanan Cairan di Dalam Sel Tumbuhan" },
  { "en": "Counter-melody : Melodi Sekunder yang Melengkapi Melodi Utama = Bassline : .........", "id": "Garis Melodi Nada Rendah" },
  { "en": "Chunking : Mengelompokkan Informasi untuk Diingat = Mnemonic : .........", "id": "Jembatan Keledai atau Teknik Memori Lainnya" },
  { "en": "Tegangan Permukaan : Kecenderungan Cairan untuk Mengerut = Kapilaritas : .........", "id": "Kemampuan Cairan Naik dalam Ruang Sempit" },
  { "en": "Setting : Latar Waktu dan Tempat Cerita = Tone : .........", "id": "Sikap Penulis terhadap Subjek Cerita" },
  { "en": "Pragmatisme : Fokus pada Hasil Praktis = Idealisme : .........", "id": "Fokus pada Ide dan Pikiran" },
  { "en": "Version Control (e.g., Git) : Mengelola Perubahan Kode = Continuous Integration : .........", "id": "Menggabungkan dan Menguji Kode secara Otomatis" },
  { "en": "Hak Tanggungan : Jaminan atas Tanah = Fidusia : .........", "id": "Jaminan atas Benda Bergerak" },
  { "en": "Teori Kuantitas Uang : Hubungan Jumlah Uang dan Tingkat Harga = Paritas Daya Beli : .........", "id": "Teori Nilai Tukar Jangka Panjang" },
  { "en": "EKG (Elektrokardiogram) : Merekam Aktivitas Listrik Jantung = EEG (Elektroensefalogram) : .........", "id": "Merekam Aktivitas Listrik Otak" },
  { "en": "Kopling : Menghubungkan/Memutuskan Tenaga Mesin = Diferensial : .........", "id": "Memungkinkan Roda Berputar dengan Kecepatan Berbeda" },
  { "en": "Perang Bubat : Majapahit vs Sunda = Pertempuran Waterloo : .........", "id": "Kekalahan Terakhir Napoleon" },
  { "en": "Mielin : Isolator Akson Saraf = Nodus Ranvier : .........", "id": "Celah pada Selubung Mielin" },
  { "en": "Modulasi : Perubahan Kunci Nada dalam Lagu = Transposisi : .........", "id": "Memindahkan Seluruh Lagu ke Kunci Nada Baru" },
  { "en": "Disosiasi Kognitif : Ketidaknyamanan Akibat Ide Bertentangan = Rasionalisasi : .........", "id": "Mencari Alasan untuk Membenarkan Perilaku" },
  { "en": "Efek Tyndall : Penghamburan Cahaya oleh Koloid = Gerak Brown : .........", "id": "Gerak Acak Partikel Koloid" },
  { "en": "Motif : Unit Terkecil dalam Narasi = Arketipe : .........", "id": "Simbol atau Karakter Universal" },
  { "en": "Sosiolinguistik : Hubungan Bahasa dan Masyarakat = Etnolinguistik : .........", "id": "Hubungan Bahasa dan Budaya" },
  { "en": "Containerization (e.g., Docker) : Mengisolasi Aplikasi = Virtualization (e.g., VMware) : .........", "id": "Mensimulasikan Sistem Perangkat Keras" },
  { "en": "Somasi : Peringatan Hukum Sebelum Tuntutan = Mediasi : .........", "id": "Penyelesaian Sengketa dengan Bantuan Pihak Ketiga" },
  { "en": "Lembah Silikon : Pusat Teknologi di AS = Bangalore : .........", "id": "Pusat Teknologi di India" },
  { "en": "Prognosis : Prediksi Perkembangan Penyakit = Diagnosis : .........", "id": "Identifikasi Penyakit" },
  { "en": "Bilangan Mach : Rasio Kecepatan Objek terhadap Kecepatan Suara = Angka Reynolds : .........", "id": "Memprediksi Pola Aliran Fluida" },
  { "en": "Pax Britannica : Periode Dominasi dan Perdamaian Inggris = Abad Pencerahan : .........", "id": "Periode Dominasi Akal dan Sains di Eropa" },
  { "en": "Homeostasis : Menjaga Keseimbangan Internal Tubuh = Umpan Balik Negatif : .........", "id": "Mekanisme untuk Mencapai Homeostasis" },
  { "en": "Sinkopasi : Penekanan pada Ketukan Lemah = Poliritme : .........", "id": "Penggunaan Dua atau Lebih Ritme Berbeda Bersamaan" },
  { "en": "Heuristik : Jalan Pintas Mental = Algoritma : .........", "id": "Prosedur Langkah-demi-Langkah yang Pasti" },
  { "en": "Bilangan Avogadro : Jumlah Partikel dalam Satu Mol = Tetapan Planck : .........", "id": "Kuantum Aksi dalam Fisika Kuantum" },
  { "en": "Foil : Karakter yang Menjadi Kontras bagi Karakter Utama = Antagonis : .........", "id": "Karakter yang Melawan Karakter Utama" },
  { "en": "Hiponim : Kata dengan Makna Spesifik (Mawar) = Hipernim : .........", "id": "Kata dengan Makna Umum (Bunga)" },
  { "en": "Load Balancer : Mendistribusikan Trafik Jaringan = Firewall : .........", "id": "Melindungi Jaringan dari Akses Tidak Sah" },
  { "en": "Arbitrase : Penyelesaian Sengketa oleh Arbiter = Ajudikasi : .........", "id": "Penyelesaian Sengketa oleh Hakim" },
  { "en": "Revolusi Hijau : Peningkatan Produksi Pangan = Revolusi Industri 4.0 : .........", "id": "Otomatisasi dan Pertukaran Data" },
  { "en": "Auskultasi : Mendengarkan Suara Tubuh (dengan Stetoskop) = Perkusi : .........", "id": "Mengetuk Permukaan Tubuh" },
  { "en": "Entanglement Kuantum : Keterkaitan Aneh Partikel Jarak Jauh = Terowongan Kuantum : .........", "id": "Partikel Menembus Penghalang Energi" },
  { "en": "Sistem Kasta : Stratifikasi Sosial Tertutup di India = Sistem Feodal : .........", "id": "Struktur Sosial Berbasis Tanah di Eropa" },
  { "en": "Apoptosis : Kematian Sel Terprogram = Nekrosis : .........", "id": "Kematian Sel Akibat Cedera" },
  { "en": "Atonal : Tanpa Pusat Nada = Serialisme : .........", "id": "Musik Berdasarkan Rangkaian Nada yang Telah Ditentukan" },
  { "en": "Learned Helplessness : Perasaan Pasrah Setelah Kegagalan Berulang = Self-Efficacy : .........", "id": "Keyakinan pada Kemampuan Diri" },
  { "en": "Hukum Raoult : Tekanan Uap Larutan Ideal = Hukum Henry : .........", "id": "Kelarutan Gas dalam Cairan" },
  { "en": "Personifikasi : Memberi Sifat Manusia pada Benda Mati = Apostrof : .........", "id": "Berbicara pada Benda Mati atau Orang yang Absen" },
  { "en": "Kohesi : Tarikan Antar Molekul Sejenis = Adhesi : .........", "id": "Tarikan Antar Molekul Tak Sejenis" },
  { "en": "SDK (Software Development Kit) : Kumpulan Alat untuk Membuat Aplikasi = Framework : .........", "id": "Kerangka Kerja yang Menyediakan Struktur Aplikasi" },
  { "en": "Ultra Vires : Tindakan di Luar Wewenang = Intra Vires : .........", "id": "Tindakan di Dalam Wewenang" },
  { "en": "Globalisasi : Peningkatan Keterhubungan Global = Proteksionisme : .........", "id": "Kebijakan Melindungi Ekonomi Domestik" },
  { "en": "Anaphora : Pengulangan Kata di Awal Kalimat = Epistrophe : .........", "id": "Pengulangan Kata di Akhir Kalimat" },
  { "en": "Inflasi Kosmik : Ekspansi Eksponensial Awal Alam Semesta = Radiasi Latar Gelombang Mikro Kosmik : .........", "id": "Sisa Cahaya dari Big Bang" },
  { "en": "Zaman Victoria : Moralitas Konservatif = Roaring Twenties : .........", "id": "Kemakmuran dan Perubahan Sosial Pasca-PD I" },
  { "en": "Metilasi DNA : Umumnya Menekan Ekspresi Gen = Asetilasi Histon : .........", "id": "Umumnya Meningkatkan Ekspresi Gen" },
  { "en": "Tritone : Interval Sangat Disonan = Perfect Fifth (Kuint) : .........", "id": "Interval Sangat Konsonan" },
  { "en": "Psikoanalisis (Freud) : Analisis Bawah Sadar = Terapi Kognitif Perilaku (CBT) : .........", "id": "Mengubah Pola Pikir dan Perilaku Negatif" },
  { "en": "Enansiomer : Isomer Optik (Bayangan Cermin) = Diastereomer : .........", "id": "Isomer Ruang (Bukan Bayangan Cermin)" },
  { "en": "Solipsisme : Hanya Pikiran Sendiri yang Pasti Ada = Determinisme : .........", "id": "Semua Peristiwa Telah Ditentukan Sebelumnya" },
  { "en": "Hukum Grimm : Pergeseran Bunyi Konsonan di Bahasa Jermanik = Hukum Verner : .........", "id": "Pengecualian pada Hukum Grimm" },
  { "en": "Stack (Tumpukan) : LIFO (Last-In, First-Out) = Queue (Antrean) : .........", "id": "FIFO (First-In, First-Out)" },
  { "en": "Kelalaian (Tort) : Kegagalan Bertindak Hati-hati = Pencemaran Nama Baik : .........", "id": "Pernyataan Palsu yang Merusak Reputasi" },
  { "en": "Dilema Tahanan : Konflik antara Kepentingan Individu dan Kelompok = Ekuilibrium Nash : .........", "id": "Pilihan Optimal Pemain, Mengingat Pilihan Lawan" },
  { "en": "Litotes : Merendah untuk Meninggikan = Eufemisme : .........", "id": "Menggunakan Ungkapan yang Lebih Halus" },
  { "en": "Lensa Gravitasi : Pembelokan Cahaya oleh Massa = Pergeseran Merah Gravitasi : .........", "id": "Cahaya Kehilangan Energi Saat Keluar dari Medan Gravitasi" },
  { "en": "Gerakan Bawah Tanah (Underground Railroad) : Jaringan Pembebasan Budak = Prohibisi : .........", "id": "Larangan Alkohol di AS" },
  { "en": "RNA Polymerase : Mensintesis RNA dari DNA = Ribosom : .........", "id": "Mensintesis Protein dari RNA" },
  { "en": "Skala Kardashev : Mengukur Kemajuan Teknologi Peradaban = Paradoks Fermi : .........", "id": "Kontradiksi antara Peluang Tinggi dan Ketiadaan Bukti Kehidupan Asing" },
  { "en": "Terapi Humanistik (Rogers) : Fokus pada Aktualisasi Diri = Terapi Gestalt : .........", "id": "Fokus pada Kesadaran 'Di Sini dan Sekarang'" },
  { "en": "Gugus Karbonil : Atom Karbon Rangkap Dua dengan Oksigen = Gugus Hidroksil : .........", "id": "Gugus -OH" },
  { "en": "Tabula Rasa (Locke) : Pikiran Lahir sebagai Lembaran Kosong = Innatism (Descartes) : .........", "id": "Ide Bawaan Sejak Lahir" },
  { "en": "Rekonstruksi Proto-Bahasa : Membangun Kembali Bahasa Nenek Moyang = Glotokronologi : .........", "id": "Menentukan Umur Bahasa" },
  { "en": "Pohon Biner : Struktur Data dengan Maksimal Dua Anak = Linked List : .........", "id": "Struktur Data Linier dengan Pointer" },
  { "en": "Fitnah (Slander) : Pencemaran Nama Baik Lisan = Fitnah (Libel) : .........", "id": "Pencemaran Nama Baik Tertulis" },
  { "en": "Tragedi Bersama (Tragedy of the Commons) : Eksploitasi Sumber Daya Publik = Eksternalitas Negatif : .........", "id": "Biaya yang Ditanggung Pihak Ketiga" },
  { "en": "Asindeton : Menghilangkan Kata Hubung = Polisindeton : .........", "id": "Menggunakan Banyak Kata Hubung" },
  { "en": "Prinsip Antropik : Alam Semesta Teramati Karena Kita Ada = Teori Multiverse : .........", "id": "Hipotesis Banyaknya Alam Semesta" },
  { "en": "Depresi Hebat (Great Depression) : Kemerosotan Ekonomi Global 1930-an = New Deal : .........", "id": "Program Pemulihan Ekonomi AS oleh Roosevelt" },
  { "en": "Telomer : Ujung Pelindung Kromosom = Sentromer : .........", "id": "Pusat Kromosom" },
  { "en": "Disonansi Kognitif : Ketidaknyamanan Psikologis Akibat Konflik Keyakinan = Homeostasis : .........", "id": "Kecenderungan Tubuh Menjaga Keseimbangan" },
  { "en": "Zwitterion : Molekul dengan Muatan Positif dan Negatif = Radikal Bebas : .........", "id": "Atom atau Molekul dengan Elektron Tak Berpasangan" },
  { "en": "Imperatif Kategoris (Kant) : Kewajiban Moral Universal = Utilitarianisme (Bentham) : .........", "id": "Tindakan yang Memaksimalkan Kebahagiaan" },
  { "en": "Sabir : Pijin Mediterania = Tok Pisin : .........", "id": "Bahasa Kreol di Papua Nugini" },
  { "en": "Hash Table : Struktur Data Menggunakan Fungsi Hash = Array : .........", "id": "Kumpulan Elemen dengan Indeks" },
  { "en": "Ganti Rugi Nominal : Kerugian Kecil atau Simbolis = Ganti Rugi Likuidasi : .........", "id": "Jumlah yang Disepakati dalam Kontrak" },
  { "en": "Seleksi Kerabat (Kin Selection) : Perilaku Altruistik pada Kerabat = Altruisme Resiprokal : .........", "id": "Menolong dengan Harapan Ditolong Kembali" },
  { "en": "Metonimia : Mengganti Nama dengan Atribut Terkait = Sinekdoke : .........", "id": "Menggunakan Sebagian untuk Mewakili Keseluruhan" },
  { "en": "Dekomposisi Spektral : Memisahkan Cahaya menjadi Warna = Efek Zeeman : .........", "id": "Pemisahan Garis Spektrum dalam Medan Magnet" },
  { "en": "Gerakan Hak Pilih Perempuan (Suffrage) : Perjuangan Hak Pilih Wanita = Gerakan Abolisionis : .........", "id": "Gerakan Penghapusan Perbudakan" },
  { "en": "Prion : Protein Menular Penyebab Penyakit = Viroid : .........", "id": "RNA Menular pada Tumbuhan" },
  { "en": "Aransemen : Mengadaptasi Ulang Komposisi = Orkestrasi : .........", "id": "Menentukan Instrumen mana yang Memainkan Bagian Tertentu" },
  { "en": "Atavisme : Kemunculan Kembali Sifat Leluhur = Sifat Vestigial : .........", "id": "Sisa Struktur dari Leluhur yang Tak Lagi Berfungsi" },
  { "en": "Reaksi Fisi : Pembelahan Inti Berat = Reaksi Fusi : .........", "id": "Penggabungan Inti Ringan" },
  { "en": "Absurdisme (Camus) : Mencari Makna dalam Dunia Tanpa Makna = Eksistensialisme (Sartre) : .........", "id": "Kebebasan dan Tanggung Jawab Individu Menciptakan Makna" },
  { "en": "Bahasa Analitik : Mengandalkan Urutan Kata (cth: Inggris) = Bahasa Sintetik : .........", "id": "Mengandalkan Imbuhan (cth: Latin)" },
  { "en": "Big O Notation : Notasi Kompleksitas Algoritma = Pseudocode : .........", "id": "Deskripsi Informal Algoritma" },
  { "en": "Doktrin Monroe : Peringatan AS terhadap Kolonialisme Eropa di Amerika = Takdir Manifest (Manifest Destiny) : .........", "id": "Keyakinan Ekspansi AS di Seluruh Amerika Utara" },
  { "en": "Kondensasi : Uap menjadi Cair = Rebus : .........", "id": "Cair menjadi Uap" },
  { "en": "Tautologi : Pernyataan yang Selalu Benar (Hujan itu basah) = Kontradiksi : .........", "id": "Pernyataan yang Selalu Salah (Bujangan beristri)" },
  { "en": "Radiasi Adaptif : Evolusi Cepat Beragam Spesies dari Satu Nenek Moyang = Evolusi Konvergen : .........", "id": "Spesies Berbeda Mengembangkan Sifat Mirip Secara Independen" },
  { "en": "Alegro : Tempo Cepat = Lento : .........", "id": "Tempo Lambat" },
  { "en": "Nihilisme : Penolakan terhadap Makna dan Nilai = Humanisme Sekuler : .........", "id": "Etika dan Makna Berdasarkan Akal Manusia, Bukan Ilahi" },
  { "en": "Isoglos : Garis Peta yang Menandai Batas Fitur Bahasa = Kontur : .........", "id": "Garis Peta yang Menandai Ketinggian yang Sama" },
  { "en": "Polimorfisme : Objek dapat Memiliki Banyak Bentuk = Enkapsulasi : .........", "id": "Menyembunyikan Detail Implementasi Internal" },
  { "en": "Preseden : Keputusan Pengadilan Terdahulu = Statuta : .........", "id": "Hukum yang Dibuat oleh Legislatif" },
  { "en": "Keseimbangan Punctuated (Gould) : Evolusi Cepat Diselingi Stasis = Gradualisme (Darwin) : .........", "id": "Evolusi Perlahan dan Konstan" },
  { "en": "Klimaks : Titik Puncak Narasi = Denouement : .........", "id": "Penyelesaian Akhir Narasi" },
  { "en": "Peluruhan Alfa : Emisi Inti Helium = Peluruhan Beta : .........", "id": "Emisi Elektron atau Positron" },
  { "en": "Perjanjian Tordesillas : Pembagian Dunia Baru antara Spanyol dan Portugal = Konferensi Berlin : .........", "id": "Pembagian Afrika oleh Kekuatan Eropa" },
  { "en": "Epigenetika : Perubahan Ekspresi Gen Tanpa Mengubah DNA = Genetika : .........", "id": "Studi Gen dan Keturunan" },
  { "en": "Teori Belajar Sosial (Bandura) : Belajar Melalui Observasi = Behaviorisme (Skinner) : .........", "id": "Belajar Melalui Penguatan dan Hukuman" },
  { "en": "Tegangan : Gaya per Satuan Luas = Regangan : .........", "id": "Perubahan Bentuk Relatif" },
  { "en": "Kontrak Sosial (Rousseau) : Kesepakatan untuk Membentuk Masyarakat = Leviathan (Hobbes) : .........", "id": "Negara Absolut untuk Mencegah Kekacauan" },
  { "en": "Bahasa Aglutinatif : Banyak Imbuhan digabung (cth: Turki) = Bahasa Inflektif : .........", "id": "Imbuhan Mengubah Bentuk Kata (cth: Latin)" },
  { "en": "Rekursi : Fungsi yang Memanggil Dirinya Sendiri = Iterasi : .........", "id": "Pengulangan Menggunakan Loop" },
  { "en": "Hukum Perkawinan : Mengatur Pernikahan = Hukum Waris : .........", "id": "Mengatur Pewarisan Harta" },
  { "en": "Spesies Cincin : Rangkaian Populasi yang Dapat Kawin Silang, Kecuali di Ujungnya = Spesiasi Alopatrik : .........", "id": "Pembentukan Spesies Akibat Isolasi Geografis" },
  { "en": "Oksimoron : Dua Kata Bertentangan Bersandingan = Paradoks : .........", "id": "Pernyataan yang Tampak Kontradiktif tapi Mungkin Benar" },
  { "en": "Waktu Paruh : Waktu untuk Separuh Zat Meluruh = Peluruhan Eksponensial : .........", "id": "Proses Peluruhan dengan Laju Proporsional" },
  { "en": "Zaman Pencerahan : Akal dan Individualisme = Zaman Kontra-Pencerahan : .........", "id": "Reaksi Melawan Rasionalisme Pencerahan" },
  { "en": "RNA Duta (mRNA) : Membawa Kode Genetik = RNA Transfer (tRNA) : .........", "id": "Membawa Asam Amino ke Ribosom" },
  { "en": "Arketipe Jung : Pola Universal dalam Bawah Sadar Kolektif = Id, Ego, Superego (Freud) : .........", "id": "Struktur Kepribadian Psikoanalitik" },
  { "en": "Modulus Young : Ukuran Kekakuan Benda Padat = Viskositas : .........", "id": "Ukuran Ketahanan Aliran Fluida" },
  { "en": "Imperatif Hipotetis (Kant) : 'Jika ingin X, lakukan Y' = Imperatif Kategoris : .........", "id": "'Lakukan Y' (Kewajiban Mutlak)" },
  { "en": "Rumpun Bahasa Afro-Asiatik : Bahasa Arab = Rumpun Bahasa Sino-Tibetan : .........", "id": "Bahasa Mandarin" },
  { "en": "Garbage Collection : Manajemen Memori Otomatis = Pointer : .........", "id": "Variabel yang Menyimpan Alamat Memori" },
  { "en": "Preskriptif : Menetapkan Aturan (Bagaimana Seharusnya) = Deskriptif : .........", "id": "Menggambarkan Keadaan (Bagaimana Adanya)" },
  { "en": "Efek Coattail : Kandidat Populer Mendongkrak Kandidat Lain di Partai yang Sama = Gerrymandering : .........", "id": "Manipulasi Batas Daerah Pemilihan" },
  { "en": "Allelopathy : Tumbuhan Menghambat Pertumbuhan Tumbuhan Lain dengan Kimia = Kompetisi : .........", "id": "Perebutan Sumber Daya Terbatas" },
  { "en": "Antitesis : Menempatkan Dua Ide Berlawanan Berdampingan = Kiasmus : .........", "id": "Membalik Struktur Gramatikal pada Frasa Berikutnya" },
  { "en": "Isotop Stabil : Tidak Meluruh = Radioisotop : .........", "id": "Isotop Tidak Stabil yang Meluruh" },
  { "en": "Perang Tiga Puluh Tahun : Konflik Agama dan Politik di Eropa Tengah = Perdamaian Westphalia : .........", "id": "Akhir dari Perang Tiga Puluh Tahun" },
  { "en": "Splicing RNA : Penghapusan Intron dari mRNA = Polimerase : .........", "id": "Enzim yang Mensintesis Polimer" },
  { "en": "Stroop Effect : Interferensi antara Nama Warna dan Tinta Warna = Cocktail Party Effect : .........", "id": "Kemampuan Fokus pada Satu Percakapan di Tengah Kebisingan" },
  { "en": "Termokopel : Mengubah Panas menjadi Tegangan Listrik = Efek Pizoelektrik : .........", "id": "Mengubah Tekanan Mekanis menjadi Tegangan Listrik" },
  { "en": "Tabula Rasa : Pikiran adalah Papan Kosong = Nativisme : .........", "id": "Kemampuan atau Pengetahuan adalah Bawaan" },
  { "en": "Bahasa Fusional : Imbuhan Membawa Banyak Makna (cth: Spanyol) = Bahasa Polisintetik : .........", "id": "Banyak Morfem digabung menjadi Satu Kata Panjang (cth: Eskimo)" },
  { "en": "Inheritance (Pewarisan) : Kelas Mewarisi Sifat dari Kelas Lain = Abstraksi : .........", "id": "Menyembunyikan Kompleksitas dan Menampilkan Fungsi Penting" },
  { "en": "Checks and Balances : Sistem Saling Mengawasi Antar Cabang Pemerintahan = Separasi Kekuasaan : .........", "id": "Pemisahan Wewenang (Legislatif, Eksekutif, Yudikatif)" },
  { "en": "Keseimbangan Hardy-Weinberg : Frekuensi Alel Konstan dalam Populasi = Pergeseran Genetik : .........", "id": "Perubahan Acak Frekuensi Alel" },
  { "en": "Hipernim : Kata Umum (Kendaraan) = Hiponim : .........", "id": "Kata Khusus (Mobil)" },
  { "en": "Efek Hall : Timbulnya Tegangan Melintang Akibat Medan Magnet = Efek Seebeck : .........", "id": "Timbulnya Tegangan Akibat Perbedaan Suhu" },
  { "en": "Renaisans Karoling : Kebangkitan Budaya di Bawah Charlemagne = Renaisans Italia : .........", "id": "Kebangkitan Seni dan Sastra di Italia" },
  { "en": "Endosimbiosis : Teori Asal Usul Mitokondria dan Kloroplas = Panspermia : .........", "id": "Hipotesis Kehidupan Berasal dari Luar Angkasa" },
  { "en": "Disonansi Kognitif : Ketidaknyamanan Akibat Keyakinan yang Bertentangan = Konformitas : .........", "id": "Mengubah Perilaku agar Sesuai dengan Kelompok" }




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
