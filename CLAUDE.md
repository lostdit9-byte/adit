# CLAUDE.md

Panduan untuk Claude Code (dan kontributor lain) saat bekerja di repo ini.

## Proyek
*Terdampar*: game 3D cozy survival di pulau tropis yang berjalan di browser dengan three.js. Sumber kebenaran desain ada di `docs/`:

- `docs/01-game-design-document.md`: gameplay, cerita, kamera, kontrol, UI
- `docs/02-content-and-balancing.md`: semua data konten dan angka; ikuti dokumen ini saat menulis `src/data/`
- `docs/03-technical-design.md`: arsitektur, ADR, rendering, konvensi kode, anggaran performa
- `docs/04-art-direction.md` dan `docs/05-asset-pipeline.md`: gaya visual dan cara membuat aset
- `docs/06-roadmap.md`: fase dan tugas; cek fase aktif sebelum menambah fitur

## Status
Fase 0 (pra-produksi). Belum ada kode. Setelah setup project di Fase 1, isi bagian **Perintah** di bawah.

## Aturan Kerja
- Ikuti ADR di `docs/03-technical-design.md` §3. Jangan mengganti renderer, library fisika, atau framework UI tanpa ADR baru.
- Konten bersifat data-driven di `src/data/`. ID memakai `snake_case` bahasa Inggris, dan angkanya diambil dari `docs/02`.
- Kode dan komentar ditulis dalam bahasa Inggris. Teks yang tampil ke pemain selalu lewat i18n (`src/i18n/id.json` sebagai default, `en.json`).
- Logika gameplay murni tidak meng-import three.js, supaya bisa dites dengan Vitest.
- Jangan membuat objek baru di loop per frame atau per tick; reuse `Vector3` dan sejenisnya.
- Patuhi anggaran performa (`docs/03` §20). Fitur yang terlihat harus punya preset kualitas.
- Aset di `public/assets/` hanya hasil optimasi (glb + KTX2 + Meshopt). File mentah tidak di-commit (`docs/05` §15).
- Bila desain atau angka berubah, perbarui dokumen terkait di commit yang sama.
- Commit memakai Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`).

## Perintah
Diisi saat Fase 1 (dev server, build, lint, typecheck, test, e2e).
