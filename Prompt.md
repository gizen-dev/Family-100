Buatkan sebuah aplikasi web interaktif mobile-first berupa game kuis kompetitif berbasis survei yang terinspirasi dari mekanisme acara kuis Family 100, tetapi JANGAN menggunakan nama, logo, aset visual, audio, atau identitas merek Family 100. Gunakan nama aplikasi sementara: “AI Survey Battle”.

Tujuan aplikasi:
Admin cukup memasukkan sebuah TOPIK, kemudian AI menggunakan Gemini untuk menghasilkan 20 pertanyaan permainan lengkap dengan daftar jawaban yang paling mungkin diberikan masyarakat, beserta distribusi poin simulasi. Setelah admin melakukan review dan konfirmasi, game siap dimainkan oleh 2 kelompok: Kelompok A dan Kelompok B.

PENTING:
Aplikasi harus benar-benar playable. Jangan membuat prototype statis, mockup, atau sekadar halaman UI. Semua tombol, state game, scoring, pergantian giliran, strike, rebutan, timer, dan progress harus berfungsi.

========================================

1. KONSEP PERMAINAN
   ========================================

Game terdiri dari 20 pertanyaan.

Setiap pertanyaan memiliki:

- teks pertanyaan
- 5 sampai 8 jawaban yang mungkin
- setiap jawaban memiliki poin
- total poin untuk satu pertanyaan harus 100
- jawaban diurutkan dari poin tertinggi ke terendah
- jawaban memiliki beberapa variasi/sinonim yang dianggap setara

Contoh:

Pertanyaan:
“Sebutkan sesuatu yang biasanya dibawa mahasiswa ke kampus.”

Jawaban:

1. Laptop — 35
2. Buku — 20
3. HP/Handphone — 18
4. Pulpen — 12
5. Tas — 8
6. Botol minum — 5
7. Dompet — 2

Total = 100

PENTING:
Bobot tersebut adalah SIMULASI berdasarkan perkiraan AI, bukan hasil survei manusia nyata.
Tampilkan secara eksplisit di halaman admin:
“Skor merupakan estimasi AI, bukan hasil survei responden nyata.”

Jangan mengklaim bahwa data berasal dari survei manusia jika memang tidak ada data survei.

========================================
2. FLOW ADMIN

Buat halaman Admin Dashboard dengan flow berikut:

STEP 1:
Admin memasukkan:

- Topik permainan
- jumlah pertanyaan, default 20
- bahasa, default Bahasa Indonesia

Contoh topik:
“Kehidupan Santri”
“Kehidupan Mahasiswa”
“Kebiasaan di Rumah”
“Makanan Indonesia”
“Teknologi”
“Dunia Kerja”
“Sekolah”

Tombol:
“Generate Game”

STEP 2:
Gemini menghasilkan draft 20 pertanyaan.

Tampilkan seluruh pertanyaan dalam daftar/card.

Setiap pertanyaan bisa:

- diedit
- dihapus
- regenerate
- melihat semua jawaban
- mengubah poin
- mengubah jawaban
- menambahkan jawaban
- menghapus jawaban

Admin dapat memperbaiki hasil AI sebelum game dikunci.

STEP 3:
Admin klik:
“Konfirmasi & Mulai Game”

Setelah dikonfirmasi:

- semua pertanyaan dikunci untuk sesi permainan
- game session dibuat
- skor Kelompok A = 0
- skor Kelompok B = 0
- pertanyaan aktif = 1
- status = READY

========================================
3. GAME SCREEN

Buat tampilan game yang terlihat seperti game show modern.

Layout harus responsive untuk:

- smartphone
- tablet
- desktop
- TV/projector

Tampilan utama:

Header:
AI SURVEY BATTLE

Menampilkan:
Kelompok A | Skor | Kelompok B

Area tengah:
Nomor pertanyaan
Pertanyaan aktif

Contoh:

PERTANYAAN 07

“Sebutkan sesuatu yang sering dilakukan orang ketika sedang bosan.”

Di bawahnya tampil papan jawaban yang masih tertutup.

Contoh:

1. █████████████
2. █████████████
3. █████████████
4. █████████████
5. █████████████
6. █████████████

Setiap jawaban yang benar akan membuka:

- jawaban
- poin

========================================
4. MEKANISME GILIRAN

Sediakan tombol:

“Mulai Ronde”

Kemudian admin/game operator menentukan kelompok yang bermain lebih dulu:

[ KELOMPOK A ]
[ KELOMPOK B ]

Setelah kelompok dipilih:

- kelompok aktif menjadi currentTeam
- input jawaban aktif
- timer mulai jika timer digunakan

Setelah peserta menjawab:
jawaban dimasukkan melalui input:

“Masukkan jawaban...”

Tombol:
[JAWAB]

========================================
5. SISTEM PENILAIAN JAWABAN

Pencocokan jawaban harus dilakukan bertahap.

Prioritas pertama:

1. normalisasi string
2. exact match
3. synonym/alias match
4. fuzzy matching
5. semantic matching menggunakan Gemini

Normalisasi harus menangani:

- huruf besar/kecil
- spasi
- tanda baca
- typo sederhana
- bentuk jamak/singular
- variasi umum Bahasa Indonesia
- singkatan

Contoh:

Target:
“Handphone”

Jawaban:
“HP”

harus dianggap cocok.

Target:
“Sepeda motor”

Jawaban:
“motor”

boleh dianggap cocok.

Target:
“Al-Qur'an”

Jawaban:
“alquran”

harus dianggap cocok.

Namun sistem tidak boleh terlalu longgar.

Contoh:

Target:
“Laptop”

Jawaban:
“Buku”

harus dianggap salah.

Jika semantic matching menggunakan Gemini:

- jangan langsung mempercayai model
- kirim daftar jawaban target dan jawaban peserta
- minta Gemini mengembalikan structured JSON
- gunakan confidence threshold
- contoh threshold minimal 0.80
- jika confidence di bawah threshold, anggap salah

========================================
6. STRIKE SYSTEM

Setiap kelompok memiliki strike.

Tampilan:

STRIKE
X
X
X

Jika jawaban benar:

- strike tidak bertambah
- jawaban terbuka
- poin masuk ke ROUND SCORE

Jika salah:

- strike bertambah
- tampil animasi sederhana
- berikan feedback “Jawaban belum cocok”

Maksimum:
3 strike

Setelah kelompok aktif mendapatkan 3 strike:

- ronde memasuki fase REBUTAN

========================================
7. REBUTAN

Setelah kelompok aktif mendapatkan 3 strike:

Tampilkan:
“KELOMPOK B MENDAPAT KESEMPATAN REBUTAN”

Kelompok lawan hanya mendapat 1 kesempatan menjawab.

Jika jawaban benar:

- kelompok lawan mendapatkan seluruh ROUND SCORE

Jika jawaban salah:

- kelompok aktif mempertahankan ROUND SCORE

Setelah fase rebutan selesai:

- ronde selesai
- ROUND SCORE diberikan sesuai hasil
- semua jawaban dibuka

Tombol:
“PERTANYAAN BERIKUTNYA”

========================================
8. ROUND SCORE

Jangan langsung memasukkan semua poin ke skor total.

Buat state:

roundScore

Setiap jawaban benar menambah poin ke roundScore.

Contoh:
Jawaban 1 = 35
Jawaban 3 = 18
Jawaban 5 = 8

roundScore = 61

Ketika ronde selesai:
roundScore dipindahkan ke totalScore tim pemenang ronde.

Contoh:

teamAScore = 150
teamBScore = 120
roundScore = 61

Jika A menang ronde:
teamAScore = 211

Kemudian:
roundScore = 0

========================================
9. AKHIR GAME

Setelah semua 20 pertanyaan selesai:

Tampilkan halaman RESULT.

Contoh:

GAME SELESAI

KELOMPOK A
845 POIN

KELOMPOK B
790 POIN

Tampilkan:

- total skor
- jumlah jawaban benar
- jumlah strike
- jumlah ronde dimenangkan
- jumlah rebutan berhasil
- jumlah rebutan gagal

Berikan animasi kemenangan yang sederhana dan profesional.

Tampilkan:
“GAME RESULTS”

Berikan tombol:
[ MAIN LAGI ]
[ KEMBALI KE ADMIN ]

Jangan menggunakan efek berlebihan.

========================================
10. DATA STRUCTURE

Gunakan TypeScript interface yang jelas.

Contoh:

interface AnswerOption {
id: string;
answer: string;
aliases: string[];
points: number;
revealed: boolean;
}

interface Question {
id: string;
questionNumber: number;
question: string;
answers: AnswerOption[];
}

interface TeamState {
name: string;
score: number;
strikes: number;
roundScore: number;
roundsWon: number;
correctAnswers: number;
stealSuccess: number;
stealFailure: number;
}

interface GameState {
gameId: string;
topic: string;
questions: Question[];
currentQuestionIndex: number;
currentTeam: "A" | "B" | null;
phase:
| "ADMIN"
| "READY"
| "PLAYING"
| "STEAL"
| "ROUND_END"
| "GAME_END";
teamA: TeamState;
teamB: TeamState;
roundScore: number;
timerSeconds: number;
}

Gunakan state machine sederhana supaya transisi game jelas dan tidak menghasilkan state yang kacau.

========================================
11. GEMINI GENERATION

Gemini digunakan untuk menghasilkan pertanyaan.

Jangan biarkan Gemini mengembalikan teks bebas.

Gunakan structured JSON.

Format yang diharapkan:

{
"topic": "Kehidupan Santri",
"questions": [
{
"questionNumber": 1,
"question": "Sebutkan sesuatu yang biasa dilakukan santri setelah Subuh.",
"answers": [
{
"answer": "Mengaji",
"aliases": ["ngaji", "membaca Al-Quran", "baca Quran"],
"points": 32
},
{
"answer": "Tidur",
"aliases": ["tidur lagi", "rebahan"],
"points": 24
}
]
}
]
}

Pastikan:

- 20 pertanyaan
- setiap pertanyaan 5-8 jawaban
- poin setiap pertanyaan total 100
- tidak ada poin negatif
- tidak ada duplikasi jawaban
- jawaban masuk akal
- pertanyaan tidak ambigu
- semua pertanyaan relevan dengan topik
- Bahasa Indonesia natural

========================================
12. PROMPT INTERNAL UNTUK GEMINI

Gunakan instruksi internal berikut saat generate:

“Kamu adalah generator kuis survey-based yang menghasilkan pertanyaan permainan berdasarkan pola jawaban masyarakat Indonesia.

Buat pertanyaan yang:

- mudah dipahami
- memiliki banyak kemungkinan jawaban
- memungkinkan muncul jawaban populer
- tidak terlalu spesifik
- tidak membutuhkan pengetahuan akademik mendalam
- cocok dimainkan oleh dua kelompok
- relevan dengan topik yang diberikan

Untuk setiap pertanyaan:

- buat 5-8 kemungkinan jawaban
- urutkan berdasarkan perkiraan popularitas
- berikan poin total 100
- buat alias untuk variasi jawaban
- hindari jawaban yang terlalu mirip satu sama lain
- hindari pertanyaan yang hanya memiliki satu jawaban masuk akal

Poin adalah SIMULASI ESTIMASI AI, bukan data survei nyata.”

========================================
13. QUALITY CONTROL AI

Setelah Gemini menghasilkan soal:
jalankan validasi otomatis.

Periksa:

- jumlah pertanyaan = 20
- setiap pertanyaan memiliki 5-8 jawaban
- total poin = 100
- tidak ada duplicate answer
- tidak ada duplicate question
- poin terbesar berada di jawaban paling populer
- semua field valid
- JSON valid

Jika invalid:

- otomatis perbaiki atau regenerate bagian yang invalid
- jangan membuat user harus memperbaiki JSON manual

========================================
14. ADMIN EDITOR

Admin harus dapat mengubah:

Pertanyaan:
[ textarea ]

Jawaban:
[ text ]

Alias:
[ text ]

Poin:
[ number ]

Tambahkan:
[ + Tambah Jawaban ]

Hapus:
[ Hapus ]

Setelah poin diubah:
tampilkan total:

TOTAL POIN: 100

Jika total bukan 100:
beri warning dan tombol:
“Normalisasi Poin”

Saat tombol ditekan, sistem menyesuaikan poin secara proporsional agar total kembali 100.

========================================
15. TIMER

Tambahkan timer opsional.

Default:
20 detik per jawaban.

Admin dapat mengaktifkan / menonaktifkan timer.

Jika aktif:
tampilkan countdown besar.

10
9
8
7
...

Saat mencapai 0:
otomatis dianggap salah atau masuk kondisi timeout.

Jangan membuat timer bergantung pada AI.
Timer harus dikontrol oleh JavaScript state.

========================================
16. SOUND EFFECT

Tambahkan sound effect sederhana menggunakan Web Audio API atau audio lokal yang bebas digunakan.

Gunakan efek untuk:

- jawaban benar
- jawaban salah
- strike
- rebutan
- ronde selesai
- game selesai

Jangan menggunakan audio berhak cipta dari acara TV.

Berikan tombol:
🔊 SOUND ON/OFF

========================================
17. DESAIN UI

Gaya visual:

- modern
- premium
- game show
- clean
- high contrast
- cocok untuk proyektor
- tombol besar
- typography tegas
- tidak terlalu ramai

Gunakan:

- React
- TypeScript
- Tailwind CSS
- component architecture yang rapi

Buat layout:

- mobile-first
- responsive
- touch-friendly

Gunakan animasi secukupnya:

- answer reveal
- strike
- score update
- transition antar pertanyaan

Jangan membuat UI terlalu berat.

========================================
18. STORAGE

Untuk MVP tidak wajib menggunakan database eksternal.

Gunakan localStorage untuk menyimpan:

- generated game
- pertanyaan
- jawaban
- konfigurasi
- game state
- history game

Berikan menu:
“Riwayat Game”

Admin dapat melihat:

- tanggal
- topik
- skor A
- skor B
- status selesai/belum

Admin dapat:

- buka kembali
- hapus history

========================================
19. ARSITEKTUR KODE

Pisahkan kode menjadi komponen yang jelas.

Contoh:

src/
components/
AdminDashboard
QuestionGenerator
QuestionEditor
GameBoard
ScoreBoard
AnswerBoard
TeamPanel
StrikeDisplay
Timer
ResultScreen
GameHistory

services/
geminiService
answerMatcher
scoringService
gameService
storageService

types/
game.ts

utils/
normalizeAnswer
validateQuestions
normalizePoints

Jangan menaruh seluruh aplikasi dalam satu file besar.

========================================
20. GAME ENGINE

Game engine harus deterministik.

AI TIDAK boleh menentukan:

- skor akhir
- siapa menang
- berapa strike
- siapa mendapat ronde
- pergantian turn
- timer
- apakah ronde selesai

Semua keputusan tersebut harus berasal dari state dan logic TypeScript.

AI hanya digunakan untuk:

- generate content
- semantic answer matching jika diperlukan

========================================
21. ERROR HANDLING

Jika Gemini gagal:
tampilkan pesan:

“AI gagal menghasilkan soal. Silakan coba lagi.”

Sediakan:
[ Coba Lagi ]

Jika response Gemini tidak valid JSON:

- parse
- repair bila memungkinkan
- kalau gagal, regenerate

Jika API key bermasalah:
tampilkan pesan error yang jelas tanpa mengekspos secret.

Jangan hardcode API key.

Gunakan environment configuration yang sesuai dengan environment Google AI Studio.

========================================
22. UX ADMIN

Admin harus bisa:
Generate → Review → Edit → Confirm → Play

Jangan mengharuskan admin memahami coding, JSON, API, atau prompt.

Semua proses AI harus terjadi di belakang layar.

Gunakan bahasa UI:

- Bahasa Indonesia
- sederhana
- jelas

========================================
23. UX PEMAIN

Pemain tidak boleh melihat:

- jawaban yang belum terbuka
- poin jawaban yang belum ditemukan
- alias jawaban
- metadata AI

Yang terlihat hanya:

- pertanyaan
- jawaban yang sudah terbuka
- poin jawaban yang sudah terbuka
- skor kedua kelompok
- strike
- timer

========================================
24. FITUR RESET / KONTROL GAME

Game operator/admin mempunyai kontrol:

[ Jeda Game ]
[ Lanjutkan ]
[ Reset Ronde ]
[ Reset Game ]
[ Pertanyaan Berikutnya ]
[ Akhiri Game ]

Reset game harus meminta konfirmasi.

Contoh:
“Yakin ingin mereset game? Semua skor sesi saat ini akan hilang.”

========================================
25. FITUR DEMO

Saat aplikasi pertama kali dibuka dan Gemini belum digunakan:
sediakan tombol:

“Load Demo Game”

Gunakan satu dataset demo statis agar aplikasi bisa langsung dites tanpa API call.

Topik demo:
“Kebiasaan Orang Indonesia di Rumah”

Minimal 5 pertanyaan demo.

========================================
26. REQUIREMENT OUTPUT

Jangan hanya menjelaskan cara membuat aplikasi.

Bangun aplikasi secara langsung.

Setelah selesai:

- pastikan aplikasi bisa dijalankan
- pastikan tidak ada error TypeScript
- pastikan semua tombol penting bekerja
- pastikan game flow dari awal sampai akhir dapat dimainkan
- pastikan state tidak reset secara tidak sengaja
- pastikan scoring berjalan benar
- pastikan strike berjalan benar
- pastikan steal/rebutan berjalan benar
- pastikan timer berjalan benar
- pastikan localStorage berjalan benar
- pastikan layout responsive

Sebelum menganggap pekerjaan selesai, lakukan simulasi satu game lengkap dari pertanyaan 1 sampai pertanyaan 20.

Prioritas pembangunan:

PRIORITAS 1:
Game engine dan scoring harus benar.

PRIORITAS 2:
AI generation dan answer matching harus benar.

PRIORITAS 3:
Admin workflow harus benar.

PRIORITAS 4:
UI/animation/sound.

Jangan mengorbankan logic game demi tampilan.

========================================
27. HASIL AKHIR YANG DIHARAPKAN

Aplikasi final harus memiliki tiga area utama:

1. ADMIN
   Input topik → Generate → Review → Confirm

2. GAME
   Pertanyaan → Jawaban → Match → Reveal → Strike → Rebutan → Scoring

3. RESULT
   Total skor → statistik → hasil pertandingan → history

Gunakan nama aplikasi:
“AI Survey Battle”

Tambahkan subtitle:
“Game kuis berbasis simulasi jawaban populer oleh AI.”

Tampilan harus terasa seperti sebuah produk yang siap dipakai untuk acara:

- pondok
- sekolah
- kampus
- gathering
- seminar
- komunitas
- keluarga
- perusahaan

JANGAN membuat clone identitas visual acara TV tertentu.

Bangun aplikasi ini secara end-to-end dan pastikan setiap bagian benar-benar terhubung.
