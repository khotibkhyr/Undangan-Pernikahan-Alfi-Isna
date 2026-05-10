# Setup Google Sheets Integration untuk RSVP

## Langkah-langkah Setup:

### 1. Buat Google Spreadsheet
- Buka [Google Sheets](https://sheets.google.com)
- Buat spreadsheet baru dengan nama "RSVP Pernikahan Alfi & Isna"
- Rename sheet pertama menjadi "Responses"
- Pada row pertama, tambahkan header columns:
  - A1: `Tanggal`
  - B1: `Nama Lengkap`
  - C1: `Konfirmasi Kehadiran`
  - D1: `Jumlah Orang`
  - E1: `Pesan`

### 2. Buat Google Apps Script
- Buka Google Sheets yang sudah dibuat
- Klik `Extensions` → `Apps Script`
- Hapus semua code di editor
- Copy-paste code berikut:

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      data.timestamp,
      data.fullName,
      data.attendance,
      data.numPeople,
      data.message
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({
      result: "success",
      message: "Data tersimpan"
    })).setMimeType(ContentService.MimeType.JSON);
    
  } catch(err) {
    return ContentService.createTextOutput(JSON.stringify({
      result: "error",
      message: err.toString()
    })).setMimeType(ContentService.MimeType.JSON);
  }
}
```

- Klik `Deploy` → `New deployment`
- Pilih type: `Web app`
- Execute as: (Pilih email Anda)
- Who has access: `Anyone`
- Klik `Deploy`
- Copy URL yang ditampilkan (contoh: `https://script.google.com/macros/d/xxxxx/userweb...`)

### 3. Update URL di index.html
- Buka file `index.html`
- Cari baris: `const SCRIPT_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';`
- Ganti dengan URL yang Anda copy dari Apps Script
- Contoh: `const SCRIPT_URL = 'https://script.google.com/macros/d/xxxxx/userweb...';`

### 4. Test Form
- Buka `index.html` di browser
- Scroll ke bawah ke section RSVP
- Isi form dengan data test
- Klik tombol "Konfirmasi"
- Periksa Google Sheets Anda - data seharusnya muncul

## Troubleshooting

### Error: "URL Google Apps Script belum dikonfigurasi"
- Pastikan Anda sudah mengganti `YOUR_GOOGLE_APPS_SCRIPT_URL_HERE` dengan URL yang benar

### Data tidak tersimpan
- Pastikan Anda sudah click Deploy setelah mengedit Apps Script code
- Pastikan izin akses Google Sheets sudah "Anyone" di deployment settings

### CORS Error
- Gunakan `mode: 'no-cors'` dalam fetch (sudah ada di kode)

## Tips Keamanan
- Jangan share URL Google Apps Script secara publik jika Anda ingin membatasi akses
- Pertimbangkan untuk membuat form validation yang lebih ketat di Apps Script side
- Backup Google Sheets secara berkala
