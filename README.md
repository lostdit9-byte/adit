# Terdampar

> **Game 3D cozy survival di pulau tropis, dibuat dengan Three.js.**
> *Terdampar* adalah judul kerja (alternatif: *Pulau Harapan*, *Ombak Pulang*).

Bayu, nelayan muda dari sebuah desa pesisir di Indonesia timur, sedang memancing sendirian ketika badai datang tiba-tiba. Perahunya terombang-ambing, rusak, lalu karam. Ia terbangun di pantai sebuah pulau yang tidak ada di peta.

Bekalnya hanya yang sempat terbawa dari perahu: **pisau lipat, alat pancing, korek gas, serta sedikit makanan dan minuman**. Dengan itu Bayu harus bertahan hidup: mencari makanan, berkebun, berburu, memancing, berenang, dan membangun rumah dari bahan yang ia cari sendiri, sambil mengirim sinyal sampai bantuan datang.

## Sekilas

| | |
|---|---|
| **Genre** | Cozy survival · crafting · eksplorasi |
| **Engine** | Three.js (WebGL2) · TypeScript · Vite · Rapier (fisika) |
| **Platform** | Browser desktop (Chrome, Edge, Firefox, Safari); versi desktop app opsional nanti |
| **Kamera** | Third-person (TPV) dan first-person (FPV), bisa diganti kapan saja |
| **Mode** | Single-player |
| **Panjang cerita** | ±16 hari dalam game (±5–8 jam), lalu bebas lanjut bermain |
| **Gaya visual** | Cozy dan santai: air laut bening, daun bergoyang lembut, langit biru cerah yang bersih |
| **Aset** | Dibuat lewat Higgsfield: gambar dan tekstur dengan GPT Image 2.5, model 3D dengan Meshy 7 |
| **Status** | **Fase 0: Pra-produksi** (dokumen dan perencanaan) |

## Yang Dilakukan Pemain

- **Mencari tanaman untuk dimakan:** kelapa, pisang, pepaya, umbi, jamur, herbal. Hati-hati, ada yang beracun.
- **Berkebun:** ambil bibit dari alam dan kebun tua di pulau, lalu tanam, siram, dan panen di dekat rumah.
- **Berburu:** kepiting, ayam hutan, babi hutan, dengan tombak, busur, atau jerat.
- **Memancing:** pakai alat pancing bawaan dari perahu. Ada minigame tarik-ulur, belasan jenis ikan, bubu, dan tombak ikan.
- **Berenang & menyelam:** jelajahi laguna dan karang, lalu selami bangkai perahu untuk mencari perkakas dan suku cadang.
- **Membangun:** dari bivak darurat sampai rumah panggung bambu beratap rumbia. Semua alat dan bahan harus dicari atau dibuat sendiri.
- **Menunggu bantuan:** naikkan meter *Harapan* lewat tanda SOS, menara api, dan radio yang diperbaiki, sampai tim SAR datang.

## Dokumen

| # | Dokumen | Isi |
|---|---|---|
| 01 | [Game Design Document](docs/01-game-design-document.md) | Cerita, dunia, loop gameplay, semua mekanik, progresi & sistem penyelamatan, kamera, kontrol, UI, audio |
| 02 | [Konten & Balancing](docs/02-content-and-balancing.md) | Database item, resep, tanaman, ikan, hewan, bangunan, quest, dan semua angka balancing |
| 03 | [Technical Design](docs/03-technical-design.md) | Tech stack, keputusan arsitektur (ADR), rendering air/vegetasi/langit, kamera TPV/FPV, fisika, save, performa, testing |
| 04 | [Art Direction](docs/04-art-direction.md) | Pilar visual, palet warna, pencahayaan per jam, desain karakter, arsitektur, UI |
| 05 | [Asset Pipeline](docs/05-asset-pipeline.md) | Alur Higgsfield → GPT Image 2.5 / Meshy 7 → Blender → glTF, template prompt, spesifikasi teknis, daftar aset |
| 06 | [Roadmap](docs/06-roadmap.md) | Fase pengembangan, milestone, tugas per fase, risiko, keputusan terbuka |

Panduan untuk asisten AI (Claude Code) ada di [CLAUDE.md](CLAUDE.md).

## Tech Stack (rencana)

| Area | Pilihan |
|---|---|
| Bahasa & build | TypeScript (strict) + Vite |
| Rendering | three.js r186, `WebGLRenderer` (WebGL2), shader GLSL custom untuk air, vegetasi, terrain, dan langit |
| Post-processing | `postprocessing` (pmndrs): bloom, tone mapping, color grading LUT, SMAA |
| Fisika | Rapier (`@dimforge/rapier3d-compat`): character controller, terrain heightfield |
| UI | Preact + Signals sebagai overlay DOM |
| Save | IndexedDB (`idb-keyval`) |
| Aset | glTF Binary (`.glb`) + tekstur KTX2 + kompresi Meshopt (`@gltf-transform/cli`) |
| Tes | Vitest (logika) + Playwright (smoke test di browser) |
| Hosting | Vercel (preview per branch), itch.io untuk rilis |

Alasan setiap pilihan ada di bagian *Keputusan Arsitektur (ADR)* pada [Technical Design](docs/03-technical-design.md).

## Struktur Repo (rencana)

```
.
├── docs/            # dokumen desain (sumber kebenaran)
├── art/             # style bible, template prompt, catatan asal-usul aset
├── public/assets/   # aset runtime yang sudah dioptimasi (glb, ktx2, audio)
├── src/             # kode game (TypeScript)
├── tools/           # script pipeline: optimasi aset, bake dunia, manifest
└── tests/           # unit test (Vitest) & e2e (Playwright)
```

Saat ini repo baru berisi dokumen. Penulisan kode dimulai di Fase 1 (lihat [Roadmap](docs/06-roadmap.md)).

## Keputusan yang Perlu Dikonfirmasi

Beberapa hal sudah diasumsikan supaya rencana bisa berjalan. Rinciannya ada di [Roadmap §6](docs/06-roadmap.md):

1. Judul final dan nama tokoh (sementara *Terdampar* dan *Bayu*).
2. Gaya visual: stylized-cozy (rekomendasi) atau semi-realistis.
3. Platform: browser desktop dulu, lalu desktop app atau mobile?
4. Bahasa: Indonesia sebagai bahasa utama, Inggris sebagai bahasa kedua.
5. Langganan tool AI: Higgsfield berbayar (tanpa watermark), dan Meshy Pro bila Meshy 7 dipakai langsung di meshy.ai.

## Langkah Berikutnya

1. Review dokumen dan konfirmasi keputusan di atas.
2. Buat *style bible* (8–12 gambar konsep) dengan GPT Image 2.5 di Higgsfield.
3. Uji pipeline aset end-to-end: 1 tekstur, 1 prop, dan karakter Bayu (rig + animasi jalan).
4. Mulai Fase 1: setup project dan graybox pulau dengan kamera TPV/FPV.
