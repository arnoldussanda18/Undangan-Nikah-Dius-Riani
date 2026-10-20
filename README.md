[README.md](https://github.com/user-attachments/files/33728724/README.md)
# Undangan Pernikahan Digital

Satu file: `index.html`. Tidak perlu install apa pun.

## Cara pakai
1. Buka `index.html`, ubah bagian `CONFIG` di dalam `<script>` (nama, tanggal, acara, ayat, foto, musik).
2. Foto mempelai: taruh file di folder yang sama, lalu isi `photo: "raka.jpg"`.
3. Musik: taruh `musik.mp3` di folder yang sama, lalu isi `music: "musik.mp3"`.
4. Upload ke hosting gratis (Netlify Drop, GitHub Pages, atau Cloudflare Pages).

## Nama tamu di cover
Tambahkan `?to=` di akhir link, spasi ditulis `%20`:

    https://alamat-anda.netlify.app/?to=Bapak%20Asis%20dan%20Pasangan

## Ucapan yang terlihat semua tamu
Tanpa server, ucapan hanya tersimpan di HP tamu itu sendiri. Agar semua orang melihat ucapan yang sama, pakai Google Sheets gratis:

1. Buat Google Sheet dengan header di baris 1: `name | status | events | pax | msg | at`
2. Extensions > Apps Script, tempel kode ini, lalu Deploy > New deployment > Web app (Who has access: Anyone):

```js
function doPost(e) {
  var s = SpreadsheetApp.getActiveSheet();
  var d = JSON.parse(e.postData.contents);
  s.appendRow([d.name, d.status, d.events, d.pax, d.msg, d.at]);
  return ContentService.createTextOutput("ok");
}
function doGet() {
  var s = SpreadsheetApp.getActiveSheet();
  var rows = s.getDataRange().getValues();
  var head = rows.shift();
  var out = rows.map(function (r) {
    var o = {}; head.forEach(function (h, i) { o[h] = r[i]; }); return o;
  });
  return ContentService.createTextOutput(JSON.stringify(out)).setMimeType(ContentService.MimeType.JSON);
}
```

3. Salin URL web app ke `scriptURL` di `CONFIG`.
