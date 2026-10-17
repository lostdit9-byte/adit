# 02 · Konten & Balancing

| | |
|---|---|
| Versi | 0.1 (nilai awal untuk playtest) |
| Dokumen terkait | [Game Design Document](01-game-design-document.md) · [Technical Design](03-technical-design.md) |

Semua angka di sini adalah **nilai awal** yang akan disetel lewat playtest. Saat implementasi, data ini dipindahkan ke `src/data/*.ts` (lihat Technical Design §7). Bila angka berubah di kode, perbarui juga dokumen ini di commit yang sama.

---

## 1. Konvensi

- **ID konten** memakai `snake_case` bahasa Inggris, misalnya `stone_axe`. Nama yang tampil ke pemain diambil dari i18n (`item.stone_axe.name`).
- **Satuan waktu:** *jam* = jam dalam game (50 detik nyata); *hari* = hari dalam game (20 menit nyata); *dtk* = detik nyata.
- **Jarak** dalam meter; 1 unit three.js = 1 m.
- **Porsi air:** 1 porsi = +15 Haus.
- **Tingkat kesulitan:** semua nilai untuk **Normal**, kecuali disebutkan lain (lihat §6).
- **Kelangkaan:** Umum · Tidak umum · Langka · Legendaris.

---

## 2. Waktu

| Parameter | Nilai |
|---|---|
| Panjang 1 hari | 20 menit nyata (opsi 15/20/30) |
| 1 jam game | 50 dtk |
| Siang / malam | 06:00–18:00 / 18:00–06:00 |
| Hari 1 dimulai | 09:00 |
| Barang hanyut muncul | Setiap pukul 06:00 |
| Tidur paling awal | 18:00, atau kapan saja saat Energi < 30 |
| Bangun | 06:00 |

---

## 3. Pergerakan

| Gerak | Nilai |
|---|---|
| Jalan | 2,5 m/dtk |
| Lari | 5,5 m/dtk |
| Jongkok | 1,4 m/dtk |
| Mengarungi air dangkal (0,5–1,1 m) | ×0,7 kecepatan darat |
| Renang | 1,8 m/dtk |
| Renang cepat | 3,2 m/dtk |
| Mengapung telentang (stamina habis) | 0,8 m/dtk |
| Menyelam | 1,6 m/dtk |
| Tinggi lompat | 0,9 m |
| Jatuh aman | ≤ 4 m; lebih dari itu −10 Kesehatan per meter tambahan |

---

## 4. Stat Survival

### 4.1 Laju Dasar

| Stat | Maks | Nilai awal (Hari 1) | Laju | Catatan |
|---|---|---|---|---|
| Kesehatan | 100 | 80 | Lihat §4.2 | |
| Lapar | 100 | 60 | −3/jam (±33 jam dari penuh ke 0) | ×0,5 saat tidur |
| Haus | 100 | 50 | −4,5/jam (±22 jam) | ×1,25 saat siang cerah 10:00–15:00 tanpa caping; ×0,5 saat tidur |
| Energi | 100 | 70 | −4/jam selama terjaga | Lihat §4.3 |
| Stamina | 100 | 100 | Lihat §4.4 | Maksimal 60 saat *Lelah* |
| Oksigen | 30 dtk | — | −1/dtk saat menyelam | +10 dtk dengan Kantong Udara; pulih penuh setelah 3 dtk di permukaan |
| Semangat | 100 | 60 | −0,5/jam selama terjaga | Lihat §4.5 |

### 4.2 Kesehatan

| Kondisi | Efek |
|---|---|
| Lapar ≥ 40, Haus ≥ 40, dan tidak Sakit Perut/Keracunan/Luka | +2/jam |
| Buff *Kenyang* | +4/jam (menggantikan +2) |
| Tidur di ranjang atau hammock | +5/jam |
| Lapar = 0 | −4/jam |
| Haus = 0 | −6/jam |
| Oksigen habis | −10/dtk |
| Diseruduk babi hutan | −20, dan 30% peluang *Luka* |
| Tersengat ubur-ubur | −5 |
| Berdiri di api | −5/dtk |

### 4.3 Tidur & Energi

| Tempat tidur | Energi/jam | Kesehatan/jam | Semangat saat bangun |
|---|---|---|---|
| Tanah | +6 | 0 | −5 |
| Bivak / kasur daun | +9 | +2 | 0 |
| Ranjang bambu / hammock | +12,5 | +5 | +2 × Kenyamanan |

Energi 0 → Bayu tertidur di tempat sampai pagi dan Semangat −10.

### 4.4 Stamina

| Aksi | Biaya |
|---|---|
| Lari | −10/dtk |
| Lompat | −8 |
| Renang | −1/dtk |
| Renang cepat | −8/dtk |
| Diam di air | 0 (pulih +5/dtk) |
| Mengayun kapak, beliung, atau cangkul | −6 per ayunan |
| Tusuk tombak / lempar tombak | −8 / −12 |
| Menarik busur | −4/dtk selama ditahan |
| Menggulung reel | −3/dtk |
| Memanjat pohon kelapa | −5/dtk |
| Pemulihan di darat | +20/dtk setelah jeda 1 dtk |

Stamina 0 di darat: tidak bisa lari atau mengayun alat selama 2 dtk. Di air: Bayu mengapung telentang sampai stamina pulih ke 30.

### 4.5 Semangat

| Rentang | Efek |
|---|---|
| ≥ 80, *Bersemangat* | Stamina pulih +25%; mengayun dan meracik 10% lebih cepat |
| 26–79 | Normal |
| ≤ 25, *Murung* | Stamina pulih −25%; buff *Segar* tidak didapat |

| Menambah Semangat | Nilai | Mengurangi Semangat | Nilai |
|---|---|---|---|
| Makan makanan matang | +2 | Makan makanan mentah | −2 |
| Masakan khas | +5 s/d +15 | Tidur di tanah | −5 |
| 3 jenis makanan berbeda dalam sehari | +5 | *Basah* di malam hari | −1/jam |
| Bangun di tempat nyaman | +2 × Kenyamanan | Sakit Perut / Keracunan | −2/jam |
| Duduk menonton matahari terbenam (1×/hari) | +10 | Seharian tanpa makanan matang | −5 |
| Entri Ensiklopedia baru | +2 | Pingsan | −15 |
| Melepas ikan dilindungi | +10 | | |
| Bab baru | +10 | | |
| Kontak radio / suara Ibu | +30 / +10 | | |
| Menyelamatkan sarang penyu | +15 | | |

### 4.6 Kenyamanan
Dihitung dalam radius 8 m dari tempat tidur. Setiap kategori maksimal menyumbang poin sekali.

| Kategori | Contoh | Poin |
|---|---|---|
| Tempat tidur | Kasur daun / hammock / ranjang bambu | 1 / 2 / 3 |
| Beratap | Ada atap di atas tempat tidur | 1 |
| Berdinding | Ruangan tertutup dinding | 1 |
| Penerangan | Api unggun, pelita, obor dinding | 1 |
| Alas | Tikar pandan | 1 |
| Tempat duduk | Bangku bambu | 1 |
| Meja | Meja bambu | 1 |
| Tanaman | Pot bunga | 1 |
| Suara | Lonceng angin kerang | 1 |
| Hiasan | Hiasan kerang, rak koleksi | 1 |

Nilai maksimal 12. Durasi buff *Segar* = 2 jam + 1 jam × Kenyamanan.

---

## 5. Status

| Status | Pemicu | Efek | Durasi | Penyembuh |
|---|---|---|---|---|
| Segar | Tidur ≥ 6 jam di kasur atau tempat tidur lebih baik | Stamina pulih +50%; Energi turun 25% lebih lambat | 2 jam + 1 jam × Kenyamanan | — |
| Kenyang | Masakan khas | Kesehatan +4/jam | 4–6 jam (sesuai masakan) | — |
| Basah | Berenang, hujan tanpa atap | Malam hari: Energi −1/jam dan Semangat −1/jam tambahan | Sampai kering | Dekat api 1 jam, di bawah matahari 3 jam |
| Sakit Perut | Makanan mentah, air mentah, beri merah | Muntah tiap 1–2 jam (Lapar −10, Haus −15); stamina maksimal −30 | 6–12 jam | Teh jahe (langsung sembuh), tidur |
| Keracunan | Jamur bintik merah, singkong mentah | Kesehatan −3/jam; pandangan bergoyang | 4 jam | Teh jahe, makan arang |
| Tersengat | Ubur-ubur | Kesehatan −1/jam; gerak −10% | 3 jam | Lidah buaya |
| Luka | Diseruduk babi hutan, jatuh > 4 m | Kesehatan tidak pulih | Sampai diobati | Perban, perban herbal |
| Lelah | Energi < 20 | Stamina maksimal 60; gerak −10% | Sampai Energi ≥ 20 | Tidur |

---

## 6. Tingkat Kesulitan

| Parameter | Santai | Normal | Survival |
|---|---|---|---|
| Laju Lapar/Haus/Energi | ×0,5 | ×1 | ×1,4 |
| Kerusakan saat Lapar/Haus = 0 | Tidak ada | ×1 | ×1,5 |
| Makanan basi | Tidak | Ya | Ya, ×1,3 lebih cepat |
| Babi hutan | Tidak pernah menyerang | Menyeruduk jika diserang atau didekati saat bersama anaknya | Menyeruduk bila didekati < 6 m |
| Hiu karang | Tidak ada | Tidak ada | Ada di luar karang |
| Pingsan | Tanpa kehilangan | Makanan mentah hilang; Lapar, Haus, Semangat −25 | Seperti Normal, dengan opsi permadeath |
| Durabilitas alat | ×2 | ×1 | ×0,75 |
| Bahan kembali saat dibongkar | 100% | 75% | 50% |

---

## 7. Item

### 7.1 Bekal Awal & Puing Perahu

| ID | Nama | Jumlah | Catatan |
|---|---|---|---|
| `knife` | Pisau Lipat | 1 | Durabilitas 500; kerusakan 8; diasah dengan Batu Asah (+50% durabilitas) |
| `fishing_rod` | Alat Pancing (joran ayah) | 1 | Durabilitas 200 tangkapan; zona hijau minigame 30% |
| `lighter` | Korek Gas | 1 | 30 kali nyala |
| `biscuit` | Biskuit | 3 | |
| `canned_sardine` | Sarden Kaleng | 1 | Setelah dimakan → `empty_can` |
| `plastic_bottle` | Botol Air Mineral | 2 | Masing-masing berisi 3 porsi air bersih |
| `tarp` | Terpal Robek | 1 | Puing di pantai |
| `rope` | Tali | 2 | Puing di pantai |
| `plank` | Papan | 3 | Puing di pantai |
| `jerrycan` | Jeriken | 1 | Puing di pantai; wadah 8 porsi |
| `broken_radio` | Radio Rusak | 1 | Puing di pantai; kunci cerita Bab 4 |

Inventaris dasar: **16 slot + 8 slot hotbar**. Tas Anyaman +6 slot; Tas Kulit +12 slot (menggantikan tas anyaman).

### 7.2 Bahan

| ID | Nama | Tumpuk | Sumber | Kegunaan utama |
|---|---|---|---|---|
| `stick` | Ranting | 30 | Tanah hutan, semak, pohon | Alat, bahan bakar, bangunan darurat |
| `driftwood` | Kayu Apung | 20 | Pantai (barang hanyut) | Bahan bakar; bisa dipotong jadi 2 ranting |
| `log` | Kayu Gelondong | 10 | Menebang pohon (kapak) | Meja kerja, papan, menara api |
| `plank` | Papan | 20 | Puing, meja kerja | Bangunan tier 2, peti |
| `bamboo` | Bambu | 20 | Rumpun bambu (pisau/parang) | Bangunan tier 1, alat, perabot |
| `rattan` | Rotan | 20 | Hutan (pisau/parang) | Pengikat, bubu, perabot |
| `fiber` | Serat | 30 | Rumput tinggi, kulit pohon waru | Tali |
| `rope` | Tali | 20 | Racik dari serat/sabut, puing | Hampir semua resep |
| `palm_leaf` | Daun Kelapa | 20 | Pohon kelapa | Bivak, atap rumbia, kasur daun |
| `thatch_panel` | Atap Rumbia | 10 | Racik | Atap tier 1 |
| `pandan_leaf` | Daun Pandan | 20 | Pandan di pantai | Tikar, tas anyaman, topi caping |
| `banana_leaf` | Daun Pisang | 20 | Pohon pisang | Pepes, penampung hujan |
| `coconut_husk` | Sabut Kelapa | 20 | Membuka kelapa | Tali, bahan bakar, obor |
| `coconut_shell` | Batok Kelapa | 20 | Membuka kelapa | Wadah air, pelita, orang-orangan sawah |
| `stone` | Batu | 30 | Di tanah, batu besar (beliung) | Alat, api unggun, tungku |
| `flint` | Batu Api | 10 | Bukit, batu besar (20%) | Pemantik, tombak batu, mata panah |
| `clay` | Tanah Liat | 20 | Tepi sungai dan mata air (cangkul) | Tungku, periuk, bata |
| `sand` | Pasir | 20 | Pantai (cangkul) | Batu asah |
| `shell` | Kerang | 30 | Pantai, bawah laut | Dekorasi, lonceng angin |
| `pearl` | Mutiara | 10 | Kerang mutiara di karang | Koleksi, dekorasi |
| `scrap_metal` | Besi Rongsok | 10 | Barang hanyut, bangkai perahu | Parang, gergaji, tombak besi |
| `nails` | Paku | 50 | Kotak perkakas, peti hanyut | Bangunan tier 2 |
| `cloth` | Kain | 10 | Barang hanyut | Perban, obor, hammock |
| `rubber_strip` | Karet Ban | 5 | Barang hanyut | Ketapel |
| `fishing_line` | Senar Pancing | 5 | Barang hanyut, sampah pancingan | Joran bambu, perbaikan joran |
| `leather` | Kulit | 10 | Babi hutan | Tas kulit |
| `bone` | Tulang | 20 | Babi hutan | Kail, mata panah |
| `feather` | Bulu | 30 | Ayam hutan | Panah |
| `resin` | Getah | 20 | Pohon damar (disadap dengan pisau) | Obor, menara api |
| `charcoal` | Arang | 20 | Tungku tanah liat, sisa api unggun | Bahan bakar, obat keracunan |
| `salt` | Garam | 20 | Tambak garam | Pengawetan |
| `coconut_oil` | Minyak Kelapa | 10 | Memasak daging kelapa | Pelita, tumis |
| `compost` | Kompos | 20 | Bak kompos | Pupuk |
| `brick` | Bata | 30 | Tungku tanah liat | Bangunan tier 3 |
| `empty_can` | Kaleng Kosong | 5 | Sarden kaleng | Panci darurat (1 porsi), penyuling surya |
| `copper_wire` | Kawat Tembaga | 3 | Bangkai haluan (Bab 4, ×2) | Antena, radio |
| `boat_battery` | Aki Kapal | 1 | Buritan perahu di karang luar | Radio |
| `coconut_sprout` | Tunas Kelapa | 5 | Kelapa yang dibiarkan 5 hari di tanah | Bibit pohon kelapa |
| `sago_flour` | Tepung Sagu | 10 | Pohon sagu (*Should*) | Papeda |

### 7.3 Makanan & Minuman

| ID | Nama | Lapar | Haus | Efek lain | Basi setelah |
|---|---|---|---|---|---|
| `biscuit` | Biskuit | +12 | −2 | | Tidak basi |
| `canned_sardine` | Sarden Kaleng | +30 | −5 | → Kaleng Kosong | Tidak basi |
| — | Air bersih (1 porsi) | | +15 | | — |
| — | Air mentah (1 porsi) | | +15 | 25% Sakit Perut (air mata air 10%) | — |
| — | Air laut (1 porsi) | | −10 | Kesehatan −2 | — |
| `coconut` | Kelapa (dibuka) | +3 | +25 | → Daging Kelapa ×1, Batok ×2, Sabut ×1 | 6 hari |
| `young_coconut` | Kelapa Muda | +8 | +35 | Hanya dari memanjat | 4 hari |
| `coconut_meat` | Daging Kelapa | +10 | | | 2 hari |
| `banana` | Pisang | +12 | +2 | | 4 hari |
| `papaya` | Pepaya | +18 | +6 | | 3 hari |
| `mango` | Mangga | +15 | +5 | Semangat +2 | 4 hari |
| `berry_purple` | Beri Ungu | +5 | +1 | | 2 hari |
| `berry_red` | Beri Merah Pahit | +3 | | 60% Sakit Perut | 2 hari |
| `mushroom_oyster` | Jamur Tiram | +4 | | Dimasak +10 | 2 hari |
| `mushroom_redspot` | Jamur Bintik Merah | +2 | | Keracunan, Kesehatan −15 | 2 hari |
| `cassava` | Singkong (mentah) | +5 | | Keracunan | 6 hari |
| `taro` | Talas (mentah) | +4 | | Kesehatan −5 (gatal) | 6 hari |
| `sweet_potato` | Ubi Jalar (mentah) | +6 | | | 6 hari |
| `water_spinach` | Kangkung | +4 | +2 | | 2 hari |
| `chili` | Cabai Rawit | +1 | −3 | Bumbu | 4 hari |
| `ginger` / `turmeric` | Jahe / Kunyit | +1 | | Bumbu & obat | 6 hari |
| `aloe` | Lidah Buaya | | | Menyembuhkan *Tersengat* | 4 hari |
| `seaweed` | Rumput Laut | +6 | −1 | | 2 hari |
| `egg` | Telur | +5 | | 20% Sakit Perut bila mentah | 3 hari |
| — | Ikan mentah (S/M/L) | +6/+10/+14 | | 30% Sakit Perut | 1 hari |
| `meat_raw` | Daging Mentah | +8 | | 50% Sakit Perut | 1,5 hari |
| `poultry_raw` | Daging Ayam Mentah | +6 | | 50% Sakit Perut | 1 hari |
| `crab_raw` | Kepiting Mentah | +5 | | 30% Sakit Perut | 1 hari |
| `grilled_fish_s/m/l` | Ikan Bakar (S/M/L) | +18/+30/+42 | | Semangat +2 | 3 hari |
| `grilled_meat` | Daging Bakar | +40 | | Semangat +2 | 3 hari |
| `grilled_poultry` | Ayam Bakar | +28 | | Semangat +2 | 3 hari |
| `grilled_crab` | Kepiting Bakar | +20 | | | 3 hari |
| `roasted_cassava` | Singkong Bakar | +30 | | | 3 hari |
| `roasted_taro` | Talas Bakar | +25 | | | 3 hari |
| `roasted_sweet_potato` | Ubi Bakar | +25 | | | 3 hari |
| `roasted_banana` | Pisang Bakar | +16 | | Semangat +3 | 2 hari |
| `boiled_egg` | Telur Rebus | +15 | | | 3 hari |
| `ginger_tea` | Teh Jahe | | +12 | Menyembuhkan Sakit Perut & Keracunan | — |
| `fish_soup` | Sup Ikan Kuah Kuning | +45 | +20 | Kenyang 6 jam, Semangat +10 | 2 hari |
| `pepes_fish` | Pepes Ikan | +40 | | Kenyang 4 jam, Semangat +8 | 2 hari |
| `stirfry_spinach` | Tumis Kangkung | +22 | | Semangat +6 | 2 hari |
| `papeda` | Papeda (*Should*) | +35 | +5 | Semangat +10 ekstra bila dimakan bersama Sup Ikan | 2 hari |
| `dried_fish` | Ikan Kering | +16 | −3 | | Tidak basi |
| `salted_fish` | Ikan Asin | +18 | −6 | | Tidak basi |
| `jerky` | Dendeng | +30 | −4 | | Tidak basi |
| `dried_seaweed` | Rumput Laut Kering | +7 | −2 | | Tidak basi |
| — | Makanan Gosong | +3 | | Terlalu lama di api (2× waktu masak) | 6 hari |

**Kesegaran:** turun dari 100% ke 0% sepanjang waktu basi. Di bawah 30%, nilai gizi −30%. Pada 0%, makanan menjadi *Makanan Basi* yang hanya berguna untuk kompos.

### 7.4 Alat & Senjata

| ID | Nama | Fungsi | Durabilitas | Kerusakan | Sumber |
|---|---|---|---|---|---|
| `knife` | Pisau Lipat | Memotong, membuka kelapa, menguliti, meracik | 500 | 8 | Bekal |
| `fishing_rod` | Alat Pancing | Memancing (zona hijau 30%) | 200 tangkapan | — | Bekal |
| `lighter` | Korek Gas | Menyalakan api (pasti berhasil) | 30 nyala | — | Bekal |
| `bow_drill` | Bor Api | Menyalakan api (minigame 3–6 dtk) | 15 | — | Racik tangan |
| `flint_striker` | Pemantik Batu Api | Menyalakan api (90% berhasil; durabilitas pisau −1) | 40 | — | Racik tangan |
| `stone_axe` | Kapak Batu | Menebang pohon | 120 | 12 | Racik tangan |
| `stone_pickaxe` | Beliung Batu | Memecah batu besar | 120 | 10 | Racik tangan |
| `stone_hammer` | Palu Batu | Membangun, memperbaiki | 150 | 8 | Racik tangan |
| `hammer_steel` | Palu Besi | Membangun 30% lebih cepat | 600 | 10 | Kotak perkakas |
| `stone_hoe` | Cangkul Batu | Mencangkul petak; menggali tanah liat, pasir, cacing | 120 | 6 | Racik tangan |
| `machete` | Parang | Bambu dan rotan ×1,5 hasil; senjata | 250 | 20 | Meja kerja |
| `saw` | Gergaji | 1 gelondong → 4 papan | 200 | — | Meja kerja |
| `wooden_spear` | Tombak Kayu | Tusuk 15 / lempar 20 | 40 | 15 | Racik tangan |
| `stone_spear` | Tombak Batu | Tusuk 22 / lempar 28 | 80 | 22 | Racik tangan |
| `iron_spear` | Tombak Besi | Tusuk 30 / lempar 36; tombak ikan terbaik | 200 | 30 | Meja kerja |
| `bamboo_bow` | Busur Bambu | Menembakkan panah (30 per panah) | 150 | 30 | Meja kerja |
| `arrow` | Panah | Amunisi; 70% bisa dipungut lagi | — | — | Meja kerja |
| `slingshot` | Ketapel | Membuat burung dan ayam hutan pingsan | 100 | 8 | Meja kerja |
| `bamboo_rod` | Joran Bambu | Memancing (zona hijau 22%) | 80 tangkapan | — | Meja kerja |
| `watering_can` | Penyiram Botol | Menyiram 3 petak per isi | 100 | — | Racik tangan |
| `torch` | Obor | Penerangan | 2 jam | 4 | Racik tangan |
| `whetstone` | Batu Asah | Mengasah pisau (+50% durabilitas) | 5 kali | — | Racik tangan |
| `caping` | Topi Caping | Menghapus tambahan laju Haus saat siang terik (§4.1) | — | — | Racik tangan |
| `woven_bag` | Tas Anyaman | +6 slot | — | — | Racik tangan |
| `leather_bag` | Tas Kulit | +12 slot | — | — | Meja kerja |
| `diving_goggles` | Kacamata Selam | Pandangan bawah air jernih | — | — | Kotak perkakas |
| `air_bladder` | Kantong Udara | +10 dtk Oksigen; diisi ulang di permukaan | 30 kali | — | Racik tangan |
| `bandage` | Perban | Menyembuhkan *Luka* | Habis pakai | — | Racik tangan |
| `herbal_bandage` | Perban Herbal | Menyembuhkan *Luka* + Kesehatan +15 | Habis pakai | — | Racik tangan |
| `snare` | Jerat | Menangkap ayam hutan/kepiting (40% per malam) | 5 tangkapan | — | Racik tangan |
| `fish_trap` | Bubu | 1–2 tangkapan per hari (ikan kecil, udang, kepiting bakau) | 20 hari | — | Meja kerja |
| `bait_worm` | Umpan Cacing | Waktu tunggu ×0,7 | Habis pakai | — | Mencangkul tanah lembap |
| `bait_fish` | Umpan Ikan | Waktu tunggu ×0,7; peluang ikan Tidak umum & Langka ×1,5 | Habis pakai | — | Racik tangan |

### 7.5 Wadah Air
Setiap wadah menyimpan satu jenis cairan: bersih, mentah, atau laut.

| ID | Nama | Kapasitas | Bisa merebus? |
|---|---|---|---|
| `coconut_shell` | Batok Kelapa | 1 porsi | Tidak |
| `bamboo_tube` | Tabung Bambu | 2 porsi | Tidak |
| `plastic_bottle` | Botol Plastik | 3 porsi | Tidak |
| `clay_jug` | Kendi | 5 porsi | Tidak |
| `jerrycan` | Jeriken | 8 porsi | Tidak |
| `empty_can` | Kaleng Kosong | 1 porsi | Ya |
| `clay_pot` | Periuk Tanah Liat | 3 porsi | Ya, juga untuk sup |

### 7.6 Bahan Bakar

| Bahan | Lama menyala (menit game) |
|---|---|
| Daun kelapa | 10 |
| Ranting | 20 |
| Sabut kelapa | 30 |
| Kayu apung | 40 |
| Kayu gelondong | 90 |
| Arang | 150 |

Api unggun menampung maksimal 4 jam bahan bakar. Tungku Masak 50% lebih hemat bahan bakar.

---

## 8. Resep

### 8.1 Racik Tangan

| Hasil | Bahan | Waktu | Terbuka saat |
|---|---|---|---|
| Tali ×1 | Serat ×3 | 2 dtk | Mengambil serat |
| Tali ×1 | Sabut Kelapa ×2 | 2 dtk | Mengambil sabut |
| Kapak Batu | Ranting ×2, Batu ×2, Tali ×1 | 4 dtk | Memegang ranting dan batu |
| Beliung Batu | Ranting ×2, Batu ×3, Tali ×1 | 4 dtk | Memegang batu |
| Palu Batu | Ranting ×1, Batu ×2, Tali ×1 | 4 dtk | Memegang papan atau bambu |
| Cangkul Batu | Ranting ×2, Batu ×2, Tali ×1 | 4 dtk | Menemukan pondok Pak Daeng |
| Tombak Kayu | Ranting ×2 (pakai pisau, durabilitas −2) | 3 dtk | Sejak awal |
| Tombak Batu | Ranting ×2, Batu Api ×1, Tali ×1 | 4 dtk | Memegang batu api |
| Obor | Ranting ×1 + (Kain ×1 atau Sabut ×1 + Getah ×1) | 2 dtk | Memegang kain atau getah |
| Bor Api | Ranting ×3, Tali ×1 | 3 dtk | Jurnal Pak Daeng hal. 2 |
| Pemantik Batu Api | Batu Api ×1 (pakai pisau) | 2 dtk | Memegang batu api |
| Batu Asah | Batu ×2, Pasir ×2 | 3 dtk | Memegang pasir |
| Perban | Kain ×1 | 2 dtk | Memegang kain |
| Perban Herbal | Kain ×1, Lidah Buaya ×1 | 2 dtk | Mengenali lidah buaya |
| Jerat | Tali ×2, Ranting ×2 | 3 dtk | Melihat ayam hutan |
| Tas Anyaman | Daun Pandan ×8, Tali ×2 | 6 dtk | Memegang daun pandan |
| Topi Caping | Daun Pandan ×6, Bambu ×1 | 5 dtk | Memegang daun pandan dan bambu |
| Atap Rumbia | Daun Kelapa ×4, Tali ×1 | 3 dtk | Memegang daun kelapa |
| Penyiram Botol | Botol Plastik kosong ×1 (pakai pisau) | 2 dtk | Menanam bibit pertama |
| Tabung Bambu | Bambu ×1 (pakai pisau) | 2 dtk | Memegang bambu |
| Umpan Ikan ×3 | Ikan mentah S ×1 (pakai pisau) | 2 dtk | Menangkap ikan pertama |
| Kantong Udara | Botol Plastik ×1, Tali ×1 | 3 dtk | Jurnal Pak Daeng (Bab 4) |
| Ranting ×2 | Kayu Apung ×1 (pakai pisau) | 2 dtk | Memegang kayu apung |

### 8.2 Meja Kerja

| Hasil | Bahan | Waktu | Syarat |
|---|---|---|---|
| Papan ×2 (×4 bila punya gergaji) | Kayu Gelondong ×1 | 4 dtk | |
| Parang | Besi Rongsok ×2, Ranting ×1, Tali ×1 | 8 dtk | Punya palu |
| Gergaji | Besi Rongsok ×3, Papan ×1 | 8 dtk | Punya palu |
| Tombak Besi | Besi Rongsok ×1, Bambu ×1, Tali ×1 | 6 dtk | |
| Busur Bambu | Bambu ×2, Tali ×2 | 6 dtk | |
| Panah ×4 | Bambu ×1, Bulu ×2, Batu Api ×1 (atau Tulang ×1) | 4 dtk | |
| Ketapel | Ranting ×1, Karet Ban ×1 | 4 dtk | |
| Kail Tulang ×2 | Tulang ×1 | 3 dtk | |
| Joran Bambu | Bambu ×1, Senar Pancing ×1 (atau Tali ×1: zona hijau −4%), Kail Tulang ×1 | 6 dtk | |
| Bubu | Bambu ×4, Rotan ×2 | 8 dtk | |
| Tas Kulit | Kulit ×3, Tali ×2 | 8 dtk | |
| Perbaikan alat | 50% bahan resep asli | 4 dtk | |

### 8.3 Memasak
Api Unggun punya 1 slot, Tungku Masak 3 slot. Resep rebusan butuh **Kaleng Kosong** (1 porsi) atau **Periuk Tanah Liat**. Makanan yang dibiarkan di api hingga 2× waktu masak menjadi *Gosong*.

| Hasil | Bahan | Stasiun / wadah | Waktu |
|---|---|---|---|
| Ikan Bakar S/M/L | Ikan mentah S/M/L ×1 | Api | 20 / 30 / 40 dtk |
| Daging Bakar | Daging Mentah ×1 | Api | 40 dtk |
| Ayam Bakar | Daging Ayam Mentah ×1 | Api | 30 dtk |
| Kepiting Bakar | Kepiting Mentah ×1 | Api | 20 dtk |
| Singkong / Talas / Ubi Bakar | Umbinya ×1 | Api | 30 dtk |
| Pisang Bakar | Pisang ×1 | Api | 15 dtk |
| Jamur Bakar | Jamur Tiram ×1 | Api | 15 dtk |
| Air Matang | Air mentah 1 porsi | Kaleng / Periuk | 20 dtk |
| Telur Rebus | Telur ×1, air 1 porsi | Kaleng / Periuk | 25 dtk |
| Teh Jahe | Jahe ×1, air 1 porsi | Kaleng / Periuk | 20 dtk |
| Pepes Ikan | Ikan mentah M ×1, Daun Pisang ×1, Cabai Rawit ×1 | Api | 45 dtk |
| Sup Ikan Kuah Kuning | Ikan mentah ×1, Kunyit ×1, air 2 porsi | Periuk + Tungku Masak | 60 dtk |
| Tumis Kangkung | Kangkung ×2, Minyak Kelapa ×1, Cabai Rawit ×1 | Periuk + Tungku Masak | 30 dtk |
| Minyak Kelapa | Daging Kelapa ×4 | Periuk | 1 jam game |
| Papeda (*Should*) | Tepung Sagu ×1, air 2 porsi | Periuk + Tungku Masak | 40 dtk |
| Radio Kering | Radio Rusak ×1 (diletakkan di dekat api) | Api | 8 jam game |

### 8.4 Rak Pengering
4 slot. Butuh 12 jam game cahaya siang; hujan menghentikan proses.

| Hasil | Bahan |
|---|---|
| Ikan Kering | Ikan mentah ×1 |
| Ikan Asin | Ikan mentah ×1, Garam ×1 |
| Dendeng | Daging Mentah ×1, Garam ×1 |
| Rumput Laut Kering | Rumput Laut ×1 |

### 8.5 Tungku Tanah Liat
Butuh bahan bakar seperti api unggun.

| Hasil | Bahan | Waktu |
|---|---|---|
| Periuk Tanah Liat | Tanah Liat ×4 | 1 jam game |
| Kendi | Tanah Liat ×3 | 1 jam game |
| Bata ×4 | Tanah Liat ×2 | 1 jam game |
| Arang ×3 | Kayu Gelondong ×2 | 2 jam game |
| Pot | Tanah Liat ×2 | 1 jam game |

### 8.6 Stasiun Pasif

| Stasiun | Masukan | Waktu | Hasil |
|---|---|---|---|
| Tambak Garam | Air laut 4 porsi | 1 hari cerah | Garam ×2 |
| Penyuling Surya | Otomatis (air laut) | Per siang | Cerah: 3 porsi air bersih · berawan: 1 · hujan: 0 |
| Penampung Hujan | — | Saat hujan | +1 porsi/jam ke wadah yang dipasang |
| Bak Kompos | 3 bahan organik (makanan basi, sisa ikan, daun) | 2 hari | Kompos ×1 |

---

## 9. Node Sumber Daya

| Node | Zona | Alat | Usaha | Hasil | Tumbuh kembali |
|---|---|---|---|---|---|
| Ranting di tanah | Hutan, pantai | Tangan | Ambil | Ranting ×1 | 1 hari |
| Semak | Hutan | Tangan / pisau | 2 kali | Ranting ×2, Serat ×1 | 2 hari |
| Rumput tinggi | Hutan, bukit | Tangan / pisau | 1 kali | Serat ×2 (pisau ×3) | 1 hari |
| Pohon kelapa (goyang) | Pantai | Tangan | Tahan 2 dtk | Kelapa ×0–2 | Buah: 3 hari |
| Pohon kelapa (panjat) | Pantai | Tangan | Tahan E | Kelapa ×2, Kelapa Muda ×1, Daun Kelapa ×2 | Buah: 3 hari |
| Pohon kelapa (tebang) | Pantai | Kapak | 10 ayunan | Kayu Gelondong ×2, Daun Kelapa ×4 | Tidak; tanam Tunas Kelapa |
| Pohon hutan | Hutan | Kapak | 8 ayunan | Kayu Gelondong ×3, Ranting ×4 | Tunggul → pohon baru dalam 7 hari |
| Rumpun bambu | Hutan dekat sungai | Pisau / parang | 4 / 2 ayunan | Bambu ×2 / ×3 | 3 hari |
| Rotan | Hutan | Pisau / parang | 2 ayunan | Rotan ×2 / ×3 | 4 hari |
| Pohon waru | Pesisir | Pisau | 2 kali | Serat ×3 | 3 hari |
| Pohon damar | Hutan | Pisau | Sadap | Getah ×1 | 2 hari |
| Batu kecil | Di mana saja | Tangan | Ambil | Batu ×1 | 1 hari |
| Batu besar | Bukit, pantai | Beliung | 6 ayunan | Batu ×4, Batu Api 20% | 5 hari |
| Tanah liat | Tepi sungai, mata air | Cangkul | 3 ayunan | Tanah Liat ×2 | 1 hari |
| Pasir | Pantai | Cangkul | 2 ayunan | Pasir ×2 | Tak terbatas |
| Kerang | Pantai, bawah laut | Tangan | Ambil | Kerang ×1 | 5–10 muncul tiap hari |
| Kerang mutiara | Karang | Pisau (menyelam) | 1 kali | Mutiara 25%, Kerang ×1 | 7 hari |
| Rumput laut | Laguna | Tangan (menyelam) | Ambil | Rumput Laut ×2 | 2 hari |

---

## 10. Tanaman

### 10.1 Tanaman Liar

| Tanaman | Zona | Hasil | Catatan | Kembali berbuah |
|---|---|---|---|---|
| Pohon kelapa | Pantai | Lihat §9 | Kelapa muda hanya lewat memanjat | 3 hari |
| Pisang | Tepi hutan | Pisang ×4, Daun Pisang ×2, anakan (bibit) | Perlu pisau | 3 hari |
| Pepaya | Bukaan hutan | Pepaya ×1–2, biji | | 4 hari |
| Mangga | Bukit | Mangga ×2–3 | | 5 hari |
| Singkong | Kebun Pak Daeng | Singkong ×3, stek | Wajib dimasak | Tanam ulang stek |
| Ubi jalar | Kebun Pak Daeng | Ubi ×3, stek | | Tanam ulang stek |
| Talas | Dekat mata air | Talas ×2, bibit | Wajib dimasak | 5 hari |
| Kangkung | Mata air | Kangkung ×2, stek | | 2 hari |
| Cabai rawit | Bukaan hutan | Cabai ×3, biji | | 3 hari |
| Jahe & kunyit | Lantai hutan | Rimpang ×2 | Bumbu dan obat | 4 hari |
| Lidah buaya | Bebatuan pantai | Lidah Buaya ×1 | Obat | 3 hari |
| Pandan | Pantai | Daun Pandan ×3 | | 2 hari |
| Semak beri ungu | Hutan | Beri Ungu ×3 | Aman | 2 hari |
| Semak beri merah | Hutan | Beri Merah Pahit ×3 | **Beracun** | 2 hari |
| Jamur tiram | Kayu lapuk | Jamur ×2 | Aman | 2 hari |
| Jamur bintik merah | Hutan lembap | Jamur ×2 | **Beracun** | 2 hari |
| Kembang sepatu | Sekitar pondok Pak Daeng | Bibit bunga sepatu ×1 (stek) | Untuk Pot Bunga | 3 hari |
| Pohon sagu (*Should*) | Rawa bakau | Tepung Sagu ×4 (tebang lalu olah) | | Tidak |

### 10.2 Tanaman Kebun

| Tanaman | Bibit | Waktu tumbuh | Panen | Berbuah lagi |
|---|---|---|---|---|
| Kangkung | Stek | 2 hari | Kangkung ×3 | Tiap 2 hari |
| Cabai rawit | Biji | 4 hari | Cabai ×4 | Tiap 3 hari |
| Ubi jalar | Stek | 4 hari | Ubi ×3 + stek | — |
| Talas | Bibit | 5 hari | Talas ×2 + bibit | — |
| Jahe / kunyit | Rimpang | 5 hari | Rimpang ×3 | — |
| Singkong | Stek | 6 hari | Singkong ×4 + stek | — |
| Pepaya | Biji | 7 hari | Pepaya ×2 | Tiap 4 hari (pohon permanen) |
| Pisang | Anakan | 8 hari | Pisang ×4 | Tiap 3 hari (pohon permanen) |
| Kelapa | Tunas Kelapa | 10 hari | Menjadi pohon kelapa | Tiap 3 hari |

**Aturan kebun:**
- Siram 1× sehari; hujan otomatis menyiram.
- 1 hari tanpa air → layu (berhenti tumbuh). 3 hari → mati.
- Kompos: waktu tumbuh −25% dan hasil panen +1.
- Ayam hutan bisa memakan 1 tanaman per malam di kebun yang tidak berpagar; orang-orangan sawah melindungi radius 8 m.

---

## 11. Ikan

### 11.1 Daftar Ikan

| ID | Nama | Zona | Waktu | Cuaca | Kelangkaan | Kesulitan | Ukuran |
|---|---|---|---|---|---|---|---|
| `fish_mullet` | Belanak | Pantai, laguna | Kapan saja | — | Umum | ★ | S |
| `fish_rabbitfish` | Baronang | Laguna (padang lamun) | Pagi, siang | — | Umum | ★★ | S |
| `fish_minnow` | Wader | Sungai, mata air | Siang | — | Umum | ★ | S |
| `fish_parrotfish` | Kakatua | Tepi karang | Siang | — | Umum | ★★ | M |
| `fish_red_snapper` | Kakap Merah | Tepi karang | Sore, malam | — | Tidak umum | ★★★ | M |
| `fish_grouper` | Kerapu | Karang | Siang, sore | — | Tidak umum | ★★★ | M–L |
| `fish_eel` | Sidat | Sungai, bakau | Malam | Hujan: peluang ×2 | Tidak umum | ★★★ | M |
| `squid` | Cumi-cumi | Laut dalam | Malam | — | Tidak umum | ★★★ | S |
| `fish_mackerel_tuna` | Tongkol | Laut dalam (Batu Karang) | Pagi | Cerah | Tidak umum | ★★★★ | L |
| `fish_giant_trevally` | Kuwe (GT) | Laut dalam | Pagi, sore | — | Langka | ★★★★★ | L |
| `fish_clownfish` | Ikan Badut | Karang | Siang | — | Langka | ★★ | Hanya dilepas |
| `fish_napoleon` | Napoleon | Karang luar | Pagi | Cerah | Legendaris | ★★★★★ | Hanya dilepas (dilindungi) |
| `mud_crab` | Kepiting Bakau | Bakau | Hanya bubu | — | Tidak umum | — | Kepiting Mentah ×2 |
| `shrimp` | Udang | Sungai | Hanya bubu | — | Umum | — | Umpan, atau Lapar +6 setelah dibakar |

Ukuran ikan menentukan masakannya: S → Ikan Bakar S, M → Ikan Bakar M, L → Ikan Bakar L.

**Sampah pancingan:** Sandal Jepit, Kaleng Berkarat, Senar Kusut (→ Senar Pancing), Kayu Apung, dan **Pesan dalam Botol** (8 surat unik yang muncul berurutan).

### 11.2 Aturan Memancing

| Parameter | Nilai |
|---|---|
| Waktu tunggu dasar | 6–18 dtk (acak) |
| Pengali waktu tunggu | Umpan ×0,7 · hujan ×0,85 · jam favorit ikan ×0,85 |
| Jendela strike | 0,8 dtk (joran ayah) / 0,6 dtk (joran bambu) |
| Bobot kelangkaan | Umum 60 · Tidak umum 25 · Langka 8 · Legendaris 1 · Sampah 6. Disaring dulu berdasarkan zona, jam, dan cuaca, lalu dinormalisasi |
| Umpan ikan | Bobot Tidak umum dan Langka ×1,5 |
| Pesan dalam botol | Menggantikan 25% hasil sampah sampai 8 pesan terkumpul |
| Zona hijau minigame | 30% (joran ayah) · 22% (joran bambu) · −2% per bintang kesulitan di atas ★★ |
| Progres tarikan | +15%/dtk di zona hijau · −10%/dtk di luar zona |
| Senar putus | Tegangan 100% selama 1,0 dtk (joran ayah) / 0,6 dtk (joran bambu) |
| Durabilitas joran | −1 per tangkapan, −3 saat senar putus |
| Ukuran ikan | Acak dalam rentang spesies (cm dan kg); rekor tersimpan di Ensiklopedia |

---

## 12. Hewan

| Hewan | Zona | HP | Kecepatan | Jarak sadar (jongkok) | Perilaku | Cara menangkap | Hasil | Muncul kembali |
|---|---|---|---|---|---|---|---|---|
| Kepiting pantai | Pantai (lebih banyak malam) | 5 | 3 m/dtk | 4 m (2 m) | Kabur ke lubang | Disergap dengan tangan, tombak, jerat | Kepiting Mentah ×1 | Tiap malam, maks 8 |
| Kelomang | Pantai | — | 0,5 m/dtk | — | Lambat | Diambil, lalu dilepas atau jadi umpan | Koleksi | Tiap hari |
| Ayam hutan | Tepi hutan | 15 | 6 m/dtk (terbang pendek) | 10 m (5 m) | Kabur; bersarang di semak | Jerat, busur, ketapel, tombak lempar | Daging Ayam ×1, Bulu ×2 (Telur di sarang) | 2 hari, maks 6 |
| Babi hutan | Hutan | 80 | 4 m/dtk, seruduk 7 m/dtk | 12 m (6 m) | Netral; menyeruduk bila diserang atau didekati saat bersama anaknya | Tombak, busur | Daging Mentah ×3, Kulit ×1, Tulang ×2 | 4 hari, maks 3 |
| Penyu hijau | Laguna, pantai | — | — | — | Dilindungi | Tidak bisa diburu; bisa diajak berenang bersama | Semangat | — |
| Ubur-ubur | Tepi karang, laut lepas | — | Hanyut | — | Bahaya pasif | Hindari | — | — |
| Hiu karang (Survival) | Luar karang | 120 | 6 m/dtk | 15 m | Patroli; menyerang bila pemain terlalu lama di luar karang | Hindari | — | — |
| Hewan suasana | Seluruh pulau | — | — | — | Kupu-kupu, capung, kunang-kunang, burung, cicak di dinding rumah, kalong, lumba-lumba di kejauhan | — | — | — |

**Kerusakan senjata:** tangan 5 · pisau 8 · parang 20 · tombak kayu 15 (lempar 20) · tombak batu 22 (lempar 28) · tombak besi 30 (lempar 36) · panah 30 · ketapel 8.

---

## 13. Bangunan & Perabot

### 13.1 Struktur

| ID | Nama | Bahan | Tier | Catatan |
|---|---|---|---|---|
| `lean_to` | Bivak | Ranting ×10, Daun Kelapa ×6 (atau Terpal ×1, Ranting ×6) | 0 | Tempat tidur darurat |
| `bamboo_pillar` | Tiang Bambu | Bambu ×2, Tali ×1 | 1 | Bisa berdiri di air sedalam ≤ 1,5 m |
| `bamboo_floor` | Lantai Bambu (2×2 m) | Bambu ×4, Tali ×2 | 1 | |
| `bamboo_wall` | Dinding Anyaman Bambu | Bambu ×3, Rotan ×2 | 1 | |
| `bamboo_wall_window` | Dinding Bambu Berjendela | Bambu ×3, Rotan ×2 | 1 | |
| `bamboo_door` | Pintu Bambu | Bambu ×4, Rotan ×2, Tali ×1 | 1 | |
| `thatch_roof_slope` | Atap Rumbia Miring | Bambu ×2, Atap Rumbia ×3 | 1 | |
| `thatch_roof_ridge` | Bubungan Atap Rumbia | Bambu ×1, Atap Rumbia ×2 | 1 | |
| `thatch_roof_corner` | Sudut Atap Rumbia | Bambu ×2, Atap Rumbia ×3 | 1 | |
| `bamboo_stairs` | Tangga Bambu | Bambu ×4, Tali ×2 | 1 | |
| `bamboo_fence` | Pagar Bambu | Bambu ×2, Tali ×1 | 1 | Melindungi kebun |
| `plank_floor` | Lantai Papan | Papan ×4, Paku ×4 | 2 | |
| `plank_wall` | Dinding Papan | Papan ×4, Paku ×4 | 2 | |
| `plank_wall_window` | Dinding Papan Berjendela | Papan ×3, Paku ×4 | 2 | |
| `wooden_door` | Pintu Kayu | Papan ×4, Paku ×6 | 2 | |
| `stone_foundation` | Fondasi Batu | Batu ×8, Tanah Liat ×2 | 3 | *Should* |
| `brick_wall` | Dinding Bata | Bata ×6, Tanah Liat ×2 | 3 | *Should* |
| `well` | Sumur | Batu ×20, Bata ×10, Tali ×4 | 3 | *Should*; sumber air bersih tak terbatas |

### 13.2 Stasiun

| ID | Nama | Bahan | Fungsi |
|---|---|---|---|
| `campfire` | Api Unggun | Batu ×6, Ranting ×4 | Memasak (1 slot), merebus, penerangan, mengeringkan *Basah* |
| `workbench` | Meja Kerja | Kayu Gelondong ×4, Tali ×4, Batu ×4 | Resep meja kerja, perbaikan alat |
| `stone_stove` | Tungku Masak | Batu ×10, Tanah Liat ×4 | Memasak (3 slot), memasak dengan periuk, hemat bahan bakar |
| `drying_rack` | Rak Pengering | Bambu ×6, Tali ×3 | Pengawetan (4 slot) |
| `clay_kiln` | Tungku Tanah Liat | Batu ×12, Tanah Liat ×10 | Gerabah, bata, arang |
| `storage_chest` | Peti | Papan ×6, Tali ×2 (atau Bambu ×8, Rotan ×2) | 20 slot |
| `rain_collector` | Penampung Hujan | Terpal ×1 (atau Daun Pisang ×8), Bambu ×4 | Mengisi wadah yang dipasang |
| `solar_still` | Penyuling Surya | Terpal ×1, Botol Plastik ×1, Kaleng Kosong ×1, Batu ×4 | Air laut → air bersih |
| `salt_pan` | Tambak Garam | Batu ×10, Tanah Liat ×4 | Garam |
| `compost_bin` | Bak Kompos | Bambu ×6, Tali ×2 | Kompos |
| `garden_plot` | Petak Kebun | Dicangkul (tanpa bahan) | 1 tanaman |
| `scarecrow` | Orang-orangan Sawah | Bambu ×2, Kain ×1, Batok Kelapa ×1 | Melindungi kebun radius 8 m |
| `fish_trap` | Bubu (dipasang) | Item Bubu | Dipasang di air dangkal |
| `snare` | Jerat (dipasang) | Item Jerat | Dipasang di jalur hewan |

### 13.3 Perabot & Dekorasi

| ID | Nama | Bahan | Kenyamanan |
|---|---|---|---|
| `leaf_bed` | Kasur Daun | Daun Kelapa ×4, Serat ×4 | Tempat tidur 1 |
| `bamboo_bed` | Ranjang Bambu | Bambu ×6, Tali ×4, Tikar Pandan ×1 | Tempat tidur 3 |
| `hammock` | Hammock | Tali ×8, Kain ×2 | Tempat tidur 2; dipasang di antara 2 pohon atau tiang |
| `pandan_mat` | Tikar Pandan | Daun Pandan ×6 | Alas 1 |
| `bamboo_chair` | Bangku Bambu | Bambu ×4, Tali ×2 | Tempat duduk 1 |
| `bamboo_table` | Meja Bambu | Bambu ×6, Tali ×2 | Meja 1 |
| `oil_lamp` | Pelita | Batok Kelapa ×1, Minyak Kelapa ×1, Kain ×1 | Penerangan 1 |
| `wall_torch` | Obor Dinding | Obor ×1, Bambu ×1 | Penerangan 1 |
| `flower_pot` | Pot Bunga | Pot ×1, bibit bunga sepatu ×1 | Tanaman 1 |
| `wind_chime` | Lonceng Angin Kerang | Kerang ×6, Tali ×1, Bambu ×1 | Suara 1 |
| `shell_decor` | Hiasan Kerang | Kerang ×10, Tali ×1 | Hiasan 1 |
| `trophy_rack` | Rak Koleksi | Bambu ×4, Tali ×2 | Hiasan 1 (memajang ikan rekor dan mutiara) |

### 13.4 Bangunan Cerita

| ID | Nama | Bahan | Lokasi | Efek |
|---|---|---|---|---|
| `sos_sign` | Tanda SOS | Batu ×30 (atau Kayu Gelondong ×12) | Penanda di Pantai Karam | Harapan +10% |
| `signal_tower` | Menara Api Sinyal | Kayu Gelondong ×10, Bambu ×8, Tali ×6, Daun Kelapa ×10, Getah ×3 | Puncak bukit | Harapan +15% |
| `bamboo_antenna` | Antena Bambu | Bambu ×6, Kawat Tembaga ×1, Tali ×4 | Puncak bukit | Syarat radio |
| `radio_station` | Radio | Radio Kering ×1, Aki Kapal ×1, Kawat Tembaga ×1 | Dekat antena | Harapan +15% saat berfungsi |

---

## 14. Barang Hanyut

Setiap pukul 06:00 muncul 2–4 titik barang hanyut di sepanjang pantai, dengan maksimal 6 titik yang belum diambil.

| Kelangkaan | Peluang | Isi |
|---|---|---|
| Umum | 60% | Kayu Apung ×2–4 · Botol Plastik ×1 · Tali ×1 · Kain ×1 · Sandal Jepit (sampah) |
| Tidak umum | 30% | Besi Rongsok ×1–2 · Paku ×5–10 · Senar Pancing ×1 · Karet Ban ×1 · Makanan kaleng (Lapar +25) |
| Langka | 10% | Peti kayu: Paku ×20, Kain ×3, Besi Rongsok ×3, dan 1 benih acak |

**Terjadwal:** Hari 1 (puing perahu), Hari 3 (peti berisi Paku ×20 dan Kain ×3), Hari 7 (kotak P3K: Perban ×3 dan Jahe ×2).

---

## 15. Quest

| ID | Bab | Tujuan | Selesai saat | Hadiah |
|---|---|---|---|---|
| `q_prologue_look` | Prolog | Lihat sekeliling, ganti kamera | Mouse digerakkan dan V ditekan | — |
| `q_prologue_fish` | Prolog | Tangkap 2 ikan | 2 ikan tertangkap | Badai dimulai |
| `q_check_bag` | 1 | Periksa tas | Inventaris dibuka | — |
| `q_salvage` | 1 | Kumpulkan puing perahu | Terpal, Tali, Papan, Jeriken, dan Radio Rusak diambil | — |
| `q_thirst` | 1 | Minum air kelapa | Kelapa diminum | — |
| `q_campfire` | 1 | Buat dan nyalakan api unggun | Api Unggun menyala | — |
| `q_shelter` | 1 | Buat bivak sebelum gelap | Bivak berdiri | — |
| `q_first_night` | 1 | Tidur sampai pagi | Bangun pukul 06:00 | Harapan +5% |
| `q_fresh_water` | 2 | Cari sumber air tawar | Masuk area mata air | Harapan +5% |
| `q_stone_tools` | 2 | Buat Kapak Batu | Kapak Batu diracik | — |
| `q_lagoon_fish` | 2 | Tangkap ikan di laguna dan bakar | Ikan Bakar dimakan | — |
| `q_old_hut` | 2 | Ikuti jejak ke timur | Pondok Pak Daeng ditemukan | Harapan +5%, resep Cangkul, jurnal hal. 1–3 |
| `q_first_seed` | 2 | Tanam bibit pertama | 1 tanaman ditanam | — |
| `q_name_island` | 2 | Beri nama pulau (opsional) | Nama diisi | — |
| `q_workbench` | 3 | Bangun meja kerja | Meja Kerja berdiri | — |
| `q_toolbox` | 3 | Ambil kotak perkakas di bangkai laguna | Kotak perkakas diambil | Palu Besi, Paku ×20, Kacamata Selam |
| `q_house` | 3 | Bangun rumah panggung | Lantai ×4, dinding ×6, atap ×4, pintu ×1, dan kasur terpasang | Harapan +15% |
| `q_sos` | 3 | Buat tanda SOS | Tanda SOS selesai | Harapan +10% |
| `q_dry_radio` | 4 | Keringkan radio | Radio Kering didapat | — |
| `q_copper` | 4 | Ambil kawat tembaga dari bangkai haluan | Kawat Tembaga ×2 | — |
| `q_battery` | 4 | Ambil aki di buritan perahu (karang luar) | Aki Kapal diambil | — |
| `q_antenna` | 4 | Bangun antena di puncak bukit | Antena Bambu berdiri | — |
| `q_radio` | 4 | Pasang radio | Radio berfungsi | Harapan +15% |
| `q_contact` | 4 | Siarkan panggilan pukul 19:00 | Dialog kontak selesai | Harapan +15%, Semangat +30 |
| `q_tower` | 4 | Bangun menara api sinyal | Menara Api Sinyal berdiri | Harapan +15% |
| `q_storm_prep` | 5 | Bersiap menghadapi badai | Makanan matang ×6, air bersih 8 porsi, rumah beratap lengkap | — |
| `q_storm` | 5 | Bertahan melewati malam badai | Pagi tiba dan Bayu berada di dalam rumah | Harapan +15% (100%) |
| `q_rescue` | 5 | Nyalakan menara api saat helikopter datang | Menara menyala saat helikopter terlihat | Ending |
