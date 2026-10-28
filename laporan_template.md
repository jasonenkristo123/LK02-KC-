# Laporan Mini Assignment — Universal Dependencies Bahasa Indonesia

**Nama / Kelompok:*Kelompok 12* 
**Kelas:*NLP*
**Tanggal:*10/28/2026*

## A. Ringkasan hasil

Dependency Tree Visualization membantu kita untuk memahami lebih dalam terkait pergantian hubungan antar dependency dengan lebih jelas. Untuk saat ini visualisasi difokuskan pada pemberian label kata kata, siapa yang berperan sebagai nsubj, obj, obl, obl:agent dan nsubj:pass 

## B. Perbandingan *active–passive voice*

| Semantic role (interpretasi manual) | Active: token + `DEPREL` | Passive: token + `DEPREL` | Apa yang berubah? |
|---|---|---|---|
| Agent | Siti = nsubj | siti = oblique:agent | perubahannnya ada di hubungan dependency nya dia, yang awalnya dia sebuah nominal subject berubah menjadi obl:agent, perubahan ini dilakukan karena Siti yang awalnya merupakan nsubj tersingkirkan menjadi kalimat keterangan (obl), namun karena ia sejatinya adalah nsubj yang tersingkirkan, maka ia mendapatkan gelar obl:agent |
| Patient | buku = obj | buku = nsubj:pass | sebaliknya kalau pada kasus buku dia berubah dari object menjadi nominal subject:pass, mengapa berubah? karena dia subjek yang tidak melakukan suatu aksi, dalam lingusitik sebuah subjek umumnya merupakan pelaku yang melakukan suatu aksi, namun karena disini buku merupakan "korban" dari suatu aksi maka haruslah diberikan keterangan "pass" |
| Location | perpustakaan = obl | perpustakaan = obl  | untuk perpustakaan tidak mengalami perubahan karena posisi dia sama di aktif dan pasif yaitu sebagai sebuah keterangan tempat (obl:loc) |

## C. Jawaban analisis (200–300 kata total)

**C1.** Mengapa `buku` berubah dari `obj` menjadi `nsubj:pass`?

Karena 

**C2.** Mengapa `Siti` berubah dari `nsubj` menjadi `obl:agent`?

...

**C3.** Mengapa `di` di “di perpustakaan” menjadi token `case`, tetapi `di-` pada “dibaca” tidak menjadi node sintaksis tersendiri?

...

**C4.** Bagaimana temuan Anda terhubung dengan minimal satu bagian paper (§2.1, §2.2, §2.5, §2.7)?

...

## D. Modifikasi kode

**Fitur yang ditambahkan:** Tiga fitur sesuai opsi Task D:

1. **Penyorotan khusus *case markers*** — `di` dan `oleh` mendapatkan warna hijau pada garis panah dan label `DEPREL`-nya, serta border hijau putus-putus pada node. Ketika di-klik di Token Inspector, muncul penjelasan eksplisit tentang perbedaan preposisi `di` vs prefiks `di-` pada `dibaca`.

2. **Toggle UPOS** — Checkbox "Tampilkan UPOS di node" memungkinkan pengguna menyembunyikan atau menampilkan label part of speech di bawah setiap node pada SVG.

3. **Kolom "Semantic role tetap?"** — Kolom keempat ditambahkan pada *comparison table*, menampilkan badge hijau "Tetap" beserta catatan yang menjelaskan bahwa semantic role (Agent, Patient, Location) tetap sama meskipun DEPRELnya berubah.

**File yang diubah:** `app.js`, `index.html`, `style.css`

**Potongan kode utama (fitur 1 — case marker highlighting):**

```js
const isCase = t.deprel === "case";
let edgeClass = "edge";
if (changed) edgeClass = "edge changed";
else if (isCase) edgeClass = "edge case-edge";

if (isCase) {
  const note = document.createElement("div");
  note.className = "case-note";
  note.textContent = caseExplain[token.form.toLowerCase()];
  target.appendChild(note);
}
```

```css
.edge.case-edge { stroke: #16835f; stroke-dasharray: 5 3; }
.edge-label.case-label { fill: #16835f; }
.case-note { background: #f0faf5; border-left: 3px solid #16835f; }
```

**Hasil pengujian:**
- Pada kalimat active: garis dan label `case` milik `di` berwarna hijau; node `di` berborder hijau.
- Pada kalimat passive: `di` dan `oleh` keduanya hijau; mengklik `oleh` memunculkan penjelasan §2.5 di inspector.
- Checkbox UPOS berfungsi — label UPOS hilang/muncul tanpa reload.
- Kolom keempat muncul di comparison table dengan badge "Tetap" untuk semua tiga role.

**Screenshot before/after:** (lihat `screenshot.png` yang disertakan)


## E. Refleksi

Hal yang paling menarik dari visualisasi ini adalah bagaimana **perubahan yang tampak sepele di permukaan kalimat ternyata melibatkan pergeseran besar pada struktur sintaksis**. Ketika kalimat diubah dari aktif ke pasif, bukan hanya urutan kata yang berubah, seluruh fungsi gramatikal token berganti peran: `buku` yang semula `obj` naik menjadi subjek sintaksis (`nsubj:pass`), sementara `Siti` yang tetap melakukan tindakan membaca justru "turun" menjadi oblique (`obl:agent`).

Visualisasi *dependency tree* membuat perubahan ini sangat konkret. Tanpa diagram, sulit membayangkan bahwa `membaca` dan `dibaca` memiliki FORM berbeda tetapi LEMMA yang sama (`baca`), dan bahwa prefiks `di-` tidak membuat node baru — ini langsung terlihat di Token Inspector. Hal ini mengajarkan bahwa **anotasi UD memisahkan level morfologi dan level sintaksis** dengan cara yang elegan, sesuai prinsip *lexicalism* yaitu satu kata utuh = satu node, terlepas dari seberapa banyak afiks yang terkandung di dalamnya.

## F. Referensi

- de Marneffe et al. (2015 revision). [*Universal Dependencies: A cross-linguistic typology*](https://nlp.stanford.edu/~manning/papers/USD_LREC14_UD_revision.pdf).
- [Universal Dependencies — Indonesian-GSD](https://universaldependencies.org/treebanks/id_gsd/index.html).
- Dokumentasi UD v2 lain yang Anda gunakan: ...
