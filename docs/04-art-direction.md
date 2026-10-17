# 04 · Art Direction

| | |
|---|---|
| Versi | 0.1 (draft untuk review) |
| Dokumen terkait | [Game Design Document](01-game-design-document.md) · [Technical Design](03-technical-design.md) · [Asset Pipeline](05-asset-pipeline.md) |

---

## 1. Visi Visual

> **Pulau tropis yang terasa seperti liburan, bukan bencana.**

Cozy, santai, bersih, dan hangat. **Air yang bening** adalah bintang utamanya, alam selalu bergerak lembut, dan langit biru cerah selalu bersih.

| Kami ingin terasa | Kami hindari |
|---|---|
| Hangat · lembut · cerah · bersih · hidup · khas Nusantara | Suram · kotor · realistis kasar · horor · neon · berantakan |

---

## 2. Pilar Visual

1. **Air sebening kaca.** Laguna toska yang dasar pasirnya terlihat, cahaya kaustik menari di dasar, buih putih lembut di garis pantai, dan kilau matahari di permukaan.
2. **Alam yang bernapas.** Tidak ada yang benar-benar diam: rumput dan daun bergoyang, pelepah kelapa berayun berat, awan melintas, kupu-kupu dan burung lewat. Gerakannya pelan dan menenangkan.
3. **Cahaya hangat.** Matahari keemasan, bayangan kebiruan (bukan hitam), kontras lembut, dan fajar serta senja yang dramatis tapi manis.
4. **Bersih dan terbaca.** Siluet sederhana, tekstur tidak ramai, dan objek penting mudah dikenali dari jauh. Mata pemain tidak lelah.

---

## 3. Gaya Rendering: *Stylized PBR*

- **Dasar PBR** (`MeshStandardMaterial`) supaya konsisten dengan tekstur PBR keluaran Meshy 7 dan tetap bereaksi alami terhadap siklus hari.
- **Stilisasi lewat konten, bukan shader keras:**
  - bentuk disederhanakan dengan tepi sedikit membulat (*bevel*),
  - tekstur bergaya lukisan dengan detail sedang (tidak fotografis),
  - normal map lembut, tidak tajam,
  - warna jenuh tapi tidak neon,
  - bayangan berwarna kebiruan dari hemisphere light,
  - *rim light* lembut pada karakter,
  - color grading hangat.
- **Bukan cel/toon shading** dan **tanpa outline hitam**. Alasannya: lebih sulit diselaraskan dengan aset AI, dan terasa kurang "bening" untuk air.
- Shader khusus hanya untuk **air, vegetasi (angin), terrain, dan langit** (lihat Technical Design §8).

---

## 4. Palet Warna

Nilai hex di bawah adalah titik awal. Palet final dikunci setelah style bible disetujui.

### 4.1 Langit & Air

| Nama | Hex | Pemakaian |
|---|---|---|
| Langit zenit (siang) | `#4FA9E8` | Puncak kubah langit |
| Langit horizon (siang) | `#BFE6FA` | Horizon, kabut jauh |
| Awan terang | `#FFFFFF` | Sisi awan yang terkena matahari |
| Bayangan awan | `#DCE8F2` | Sisi bawah awan |
| Air sangat dangkal | `#7FE3D6` | Pasir terlihat jelas di bawahnya |
| Air laguna | `#2CC3C3` | Toska khas laguna |
| Air tepi karang | `#1C9CC0` | Peralihan ke laut dalam |
| Air dalam | `#0E5C8A` | Laut lepas |
| Buih | `#F4FFFD` | Garis pantai, puncak gelombang |
| Kabut bawah air | `#3FB8B0` | Saat menyelam |

### 4.2 Daratan

| Nama | Hex | Pemakaian |
|---|---|---|
| Pasir putih | `#F3E6C4` | Pantai kering |
| Pasir basah | `#D9C79E` | Pita garis air |
| Rumput terang | `#8CCB5E` | Padang rumput, bukit |
| Daun kelapa | `#6DB54C` | Pelepah kelapa, semak |
| Hutan gelap | `#3E7D45` | Kanopi, bayangan hutan |
| Tanah | `#9C7653` | Jalan setapak, kebun |
| Batu | `#A8A39A` | Batu pantai, bukit |
| Batu gelap | `#6E6A66` | Tebing, karang di atas air |

### 4.3 Bangunan & Aksen

| Nama | Hex | Pemakaian |
|---|---|---|
| Bambu | `#D8C27A` | Tiang, lantai, dinding anyaman |
| Kayu | `#A87850` | Papan, perabot |
| Atap rumbia | `#C9A15F` | Atap daun kering |
| Karang koral | `#FF8B7B` | Terumbu, aksen bawah laut |
| Kembang sepatu | `#F25C5C` | Bunga, aksen dekorasi |
| Cahaya sore | `#FFC56E` | Matahari golden hour, api |

### 4.4 Malam

| Nama | Hex | Pemakaian |
|---|---|---|
| Langit malam (zenit) | `#132447` | Puncak langit |
| Langit malam (horizon) | `#2C4A78` | Horizon malam, tidak pernah hitam pekat |
| Cahaya bulan | `#A9C4FF` | Directional light malam |
| Plankton bercahaya | `#5FF3FF` | Ombak di malam hari |
| Api unggun | `#FF9A3C` | Cahaya hangat di malam hari |

### 4.5 UI

| Nama | Hex | Pemakaian |
|---|---|---|
| Kertas krem | `#FFF4E0` | Panel dan latar menu |
| Kertas gelap | `#F0DFC0` | Slot, kartu |
| Tinta coklat | `#4B3527` | Teks utama, garis ikon |
| Hijau aksi | `#5FAF6A` | Tombol utama, berhasil |
| Merah lembut | `#E0685E` | Peringatan |
| Biru info | `#4C9BD6` | Info, air, oksigen |
| Kuning harapan | `#F2B84B` | Meter Harapan, sorotan |

---

## 5. Color Script: Pencahayaan per Jam

Parameter ini di-*keyframe* dan diinterpolasi oleh sistem langit (Technical Design §8.3). Intensitas ditulis relatif terhadap siang (1,0).

| Waktu | Langit atas | Horizon | Matahari / bulan | Intensitas | Ambient (langit / tanah) | Mood |
|---|---|---|---|---|---|---|
| Fajar 05:00–06:30 | `#6C8FD0` | `#FFB89A` | `#FFD2A6` | 0,45 | `#9AA8C8` / `#8A7A6A` | Sunyi, penuh harapan |
| Pagi 06:30–10:00 | `#5AAFE8` | `#CDEBFA` | `#FFF0D8` | 0,85 | `#BFE3FF` / `#C9B48A` | Segar, ceria |
| Siang 10:00–15:30 | `#4FA9E8` | `#BFE6FA` | `#FFF8EC` | 1,0 | `#C6E6FF` / `#D2BF94` | Cerah, air paling bening |
| Golden hour 15:30–17:30 | `#6BA4E0` | `#FFD8A0` | `#FFC27A` | 0,8 | `#B8CFEA` / `#C9A77A` | Hangat, nostalgia |
| Senja 17:30–18:45 | `#4A4F8C` | `#FF8E6E` | `#FF9A5C` | 0,4 | `#8A86B8` / `#7A5E58` | Romantis, tenang |
| Malam 18:45–05:00 | `#132447` | `#2C4A78` | Bulan `#A9C4FF` | 0,25 | `#2E3E66` / `#1E2436` | Damai, berkilau (bintang, plankton) |

**Cuaca menggeser palet:** berawan menurunkan saturasi 15% dan intensitas matahari 30%; hujan membuat semuanya lebih abu-abu kebiruan dan basah mengilap; badai membuat langit `#4A5566` dengan kilat putih kebiruan.

**Malam harus tetap terbaca.** Pemain bisa melihat bentuk pulau tanpa obor. Kegelapan tidak dipakai untuk menakut-nakuti.

---

## 6. Bentuk & Proporsi
- **Bahasa bentuk:** alam dan rumah memakai bentuk membulat dan lembut (aman, nyaman); batu dan tebing sedikit bersudut; bahaya diberi isyarat bentuk tajam yang halus (taring babi hutan, tentakel ubur-ubur).
- **Proporsi sedikit dilebih-lebihkan:** daun lebih lebar, buah sedikit lebih besar, dan perabot sedikit lebih tebal, supaya mudah terbaca dari kamera TPV.
- **Siluet** objek yang bisa diinteraksi harus khas dan mudah dikenali dari jarak 10–20 m.
- **Kepadatan detail:** tinggi di area dekat jalur pemain dan POI, rendah di latar. Ada area "istirahat mata" (pasir lapang, laut terbuka).

---

## 7. Material & Tekstur
- **Dilukis, bukan difoto.** Bentuk besar terlihat jelas; noise kecil seminimal mungkin.
- **Kepadatan tekstur:** ±256 px/m untuk lingkungan dan ±512 px/m untuk karakter dan prop yang dipegang.
- **Albedo tanpa pencahayaan:** tidak ada bayangan atau highlight yang ter-*bake*. Nilai albedo tidak di bawah 30 (hitam pekat) atau di atas 240 (putih mutlak).
- **Roughness:** permukaan alam dominan kasar (0,6–0,9); air, daun basah, dan kerang lebih licin.
- **Normal map lembut.** Detail besar lebih baik dibentuk di geometri atau albedo.
- **Trim sheet bersama** untuk bambu, kayu, dan atap rumbia, supaya semua bangunan konsisten dan hemat draw call.

---

## 8. Air
| Aspek | Spesifikasi |
|---|---|
| Kejernihan | Dasar pasir terlihat jelas sampai kedalaman ±4–5 m di laguna saat siang cerah |
| Warna | Gradasi menurut kedalaman: sangat dangkal → laguna → tepi karang → dalam (§4.1) |
| Gelombang | Tenang: 0,05–0,3 m, panjang gelombang 4–20 m, gerak pelan. Badai: hingga 1,5 m |
| Buih | Pita putih lembut di garis pantai yang bergerak ke darat dan memudar; buih tipis di puncak gelombang |
| Kilau | Titik-titik kilau matahari yang lembut, terutama saat golden hour |
| Kaustik | Pola jaring cahaya yang bergerak pelan di pasir dan karang bawah air |
| Bawah air | Kabut toska, *god rays*, partikel melayang, warna merah memudar seiring kedalaman |
| Malam | Air biru tua memantulkan bulan, dan plankton cyan berkilau di buih ombak |

---

## 9. Vegetasi

| Tanaman | Catatan visual | Gerakan |
|---|---|---|
| Pohon kelapa | Batang melengkung ramping dengan cincin, pelepah lebar menjuntai, buah bergerombol | Ayunan berat lambat (periode 4–6 dtk) + getar ujung daun |
| Ketapang | Kanopi bertingkat seperti payung; sebagian daun merah | Ayunan sedang |
| Pohon hutan | Batang besar berbanir, kanopi lebat, sulur | Ayunan pelan, hanya di kanopi |
| Rumpun bambu | Batang hijau-kuning beruas, daun tipis | Melengkung lentur, daun bergetar cepat |
| Pisang | Daun lebar, sebagian sobek | Daun berkibar lebar |
| Semak & pakis | Bulat, padat | Bergoyang dan bergetar saat disentuh pemain |
| Rumput | Helai lembut dengan variasi hijau terang ke kekuningan | Gelombang hembusan angin menyapu padang tiap 6–12 dtk |
| Pandan | Daun panjang berduri, berbentuk spiral | Ayunan kecil |
| Bunga | Kembang sepatu merah, bunga liar kuning dan putih | Mengangguk pelan |
| Bawah air | Lamun, rumput laut, karang bercabang dan meja | Melambai mengikuti arus |

- Setiap instance mendapat variasi warna ±5% (hue dan value) serta variasi skala ±15% supaya tidak terlihat berulang.
- Daun yang membelakangi matahari tampak tembus cahaya (hijau-kuning terang).

---

## 10. Langit & Awan
- Langit siang biru cerah dengan gradien bersih ke horizon yang pucat.
- Awan kumulus empuk berbentuk sederhana, bergerak pelan. Siang cerah hanya ada sedikit awan.
- Senja jingga dan merah muda, dengan awan yang diterangi dari bawah.
- Malam bertabur bintang, bulan dengan halo lembut, dan Bimasakti samar.
- Tidak ada langit mendung pekat, kecuali saat hujan dan badai.

---

## 11. Karakter: Bayu

| Aspek | Deskripsi |
|---|---|
| Usia & tinggi | 21 tahun, 1,72 m |
| Tubuh | Ramping atletis khas nelayan, bahu sedikit lebar |
| Proporsi | Stylized ±6,5 kepala; tangan dan kaki sedikit lebih besar agar mudah terbaca |
| Wajah | Ramah, alis tebal, mata coklat, senyum mudah; ekspresi netral sedikit tersenyum |
| Kulit | Sawo matang hangat |
| Rambut | Hitam, pendek, sedikit berantakan |
| Pakaian | Kaos oblong teal pudar dengan sobekan kecil di ujung; celana pendek kargo krem; bertelanjang kaki (sandalnya hilang saat badai, dan sebelahnya bisa ditemukan sebagai sampah pancingan) |
| Aksesori | Sarung kotak-kotak hijau-merah yang diikat di bahu atau pinggang; gelang tali pemberian ibu; nanti topi caping dan tas anyaman (perlengkapan yang terlihat) |
| Palet | Teal `#3E9C9A`, krem `#E8D9B5`, sarung hijau `#4F8F5A` dan merah `#B5483E` |
| Siluet | Tetap terbaca dari belakang (kamera TPV): sarung dan rambut berantakan jadi ciri khas |

**Kebutuhan model:** turnaround depan, samping, dan belakang dalam **A-pose** (lengan ±45°), ekspresi netral, pencahayaan rata. Kepala dan rambut dipisah menjadi mesh sendiri agar bisa disembunyikan di FPV (lihat Asset Pipeline).

---

## 12. Hewan

| Hewan | Arahan |
|---|---|
| Kepiting pantai | Oranye kemerahan, mata bertangkai, gerak menyamping lucu |
| Kelomang | Cangkang beragam warna, malu-malu |
| Ayam hutan | Warna asli ayam hutan merah: merah, emas, dan ekor hijau mengilap. Gesit, bukan agresif |
| Babi hutan | Coklat tua berbulu kasar, sedikit gempal, taring kecil. Berwibawa tapi tidak menyeramkan |
| Penyu hijau | Cangkang hijau-coklat bermotif, gerakan anggun dan tenang |
| Ikan | Bentuk sederhana dengan warna cerah khas spesiesnya (kakatua biru-hijau, kakap merah, badut oranye-putih) |
| Ubur-ubur | Bening kebiruan dan sedikit bercahaya, sehingga mudah terlihat dan dihindari |
| Burung & serangga | Camar putih-abu, kupu-kupu kuning dan biru, kunang-kunang hijau-kuning, cicak di dinding rumah |

---

## 13. Arsitektur & Bangunan
- **Inspirasi:** arsitektur pesisir tradisional Nusantara. Rumah panggung (tiang), dinding anyaman bambu (*gedek* atau *bilik*), atap rumbia dari daun kelapa atau nipah, dan ikatan rotan yang terlihat jelas, sehingga terasa buatan tangan.
- **Buatan tangan:** sedikit tidak rata, tapi rapi dan kokoh, bukan reyot.
- **Modular:** grid 2 m. Siluet tiap bagian jelas: tiang, lantai, dinding, jendela, pintu, atap miring, bubungan.
- **Kondisi:** bangunan pemain bersih dan terawat. Reruntuhan pondok Pak Daeng berlumut dan ditumbuhi bunga, tetap cantik, tidak suram.
- **Interior hangat:** pelita dan api memberi cahaya jingga dari dalam rumah di malam hari, terlihat dari luar lewat jendela.

---

## 14. Props & Item
- Setiap item punya **ikon** dan, bila bisa diletakkan atau dijatuhkan, **model dunia**. Item kecil yang dijatuhkan memakai model **bundel atau kantong umum** agar hemat aset.
- Alat batu terlihat sederhana tapi meyakinkan (ikatan tali, batu yang diasah). Alat besi tampak hasil daur ulang (besi rongsok yang dibentuk).
- Wadah air menunjukkan isinya (botol tampak terisi atau kosong).

---

## 15. UI & Ikon
- **Gaya:** jurnal kertas krem dengan garis tinta tangan, sudut membulat, dan bayangan lembut. Terasa seperti buku catatan Bayu.
- **Ikon:** lukisan tangan sudut 3/4, garis luar tipis coklat tua untuk keterbacaan, cahaya dari kiri atas. Satu set ikon harus terlihat dibuat oleh "seniman yang sama".
- **Tipografi:** judul memakai **Baloo 2** (bulat dan ramah), isi memakai **Nunito** (jelas dan hangat). Keduanya berlisensi OFL dan di-*self-host*. Ukuran teks isi minimal 16 px pada 1080p dan bisa diperbesar.
- **Gerak UI:** muncul dengan *ease-out* 150–250 ms, sedikit membal. Tanpa kilatan keras.
- **Peta:** gambar tangan bergaya jurnal yang terisi sedikit demi sedikit saat pemain menjelajah.

---

## 16. VFX
| Efek | Arahan |
|---|---|
| Debu pasir, serpihan kayu, daun | Partikel kecil lembut dan cepat hilang |
| Cipratan & riak | Putih-toska, berbentuk bulat lembut |
| Api unggun | Lidah api bergaya (bukan simulasi realistis), percikan, dan cahaya jingga yang berdenyut pelan |
| Asap | Gumpalan lembut keabu-abuan yang terbawa angin |
| Hujan | Garis tipis, riak di genangan, kabut tipis di kejauhan |
| Kilat | Kilatan putih kebiruan singkat di langit, tidak menyilaukan, dengan opsi aksesibilitas untuk meredupkannya |
| Kunang-kunang & plankton | Titik cahaya berdenyut lembut |
| Highlight interaksi | Tepi bercahaya lembut, bukan panah atau tanda seru |

---

## 17. Do & Don't

| Lakukan | Hindari |
|---|---|
| Gradien lembut, warna jenuh tapi kalem | Warna neon, kontras keras |
| Bayangan berwarna (biru, ungu) | Bayangan hitam pekat |
| Siluet sederhana dan terbaca | Detail kecil berlebihan, tekstur ramai |
| Gerakan alami yang pelan di mana-mana | Dunia statis, atau gerakan cepat dan gelisah |
| Albedo bersih tanpa pencahayaan | Tekstur dengan bayangan dan highlight ter-*bake* |
| Material doff alami | Semua terlihat plastik mengilap |
| Malam biru yang terbaca | Malam gelap gulita |
| Bangunan buatan tangan yang rapi | Kesan kumuh atau rusak (kecuali reruntuhan yang tetap indah) |
| Nuansa Nusantara yang tulus | Stereotip atau ornamen tempelan |
| Berburu tanpa darah, dengan efek lembut | Gore, darah, kekerasan grafis |

---

## 18. Referensi Visual
- **Game:** Tchia (pulau tropis, air, budaya lokal), Sea of Thieves (warna dan gerak air), The Legend of Zelda: Breath of the Wild (cahaya lembut bergaya lukisan), A Short Hike (kehangatan), Summer in Mara (pulau cozy), Raft (suasana survival laut).
- **Film & ilustrasi:** palet latar Studio Ghibli (Ponyo, Arrietty).
- **Lokasi nyata untuk moodboard:** laguna Raja Ampat, Wakatobi, dan Derawan (air toska); pantai pasir putih Kepulauan Kei; rumah panggung Bajau dan Bugis; perahu nelayan kayu Indonesia timur.

Moodboard dan style bible yang disetujui disimpan di `art/style-bible/` (lihat Asset Pipeline).
