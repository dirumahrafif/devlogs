- Jadi gini dengan mengikuti tutorial ini, teman-teman akan paham VIM di VSCODE yang akan membuat proses coding menjadi lebih ngebut lagi  
## VIM
- Install vim
- Ada 3 mode
	- Normal : default, digunakan untuk menjalankan perintah
	- Insert : digunakan untuk melakukan insert data
	- Visual : digunakan untuk melakukan seleksi teks

## Perintah

- `h, j, k, l` untuk kiri, turun, naik, kanan
- `2j` turun 2 langkah, `2k` naik 2 langkah, `5l` kanan 5 huruf, `3h` kiri 5 huruf
- `_` menuju ke awal karakter dalam sebuah baris
- `$` menuju ke akhir karakter dalam sebuah baris
- `gg` menuju ke awal dokumen
- `GG` menuju ke akhir dokumen
- `10G` menuju ke line 10
- `/` untuk pencarian `n` untuk setelahnya, `N` untuk sebelumnya

## Copy Paste Delete
- `yy` untuk copy satu line, 
	- letakkan di lokasi yang diinginkan, misalnya klik `GG` untuk lokasi akhir, klik `$` klik huruf `p`
- `2yy` copy 2 line , letakkan di lokasi yang diinginkan klik huruf `p`
	- `GG` untuk menuju ke baris terakhir masukkan `$` untuk menuju ke cursor terakhir
- `yiw` copy untuk satu kata saja
	- `GG` untuk menuju ke baris terakhir masukkan `$` untuk menuju ke cursor terakhir
- copy banyak karakter, 
	- letakkan cursor di awal yang akan dicopy, masuk visual mode, arahkan cursor
	- klik huruf `y`
- Cut satu baris
	- pindhkan ke bagian akhir `GG`, masukkan `$`, kemudian klik `p`
- Cut, satu huruf `d`
	- pindahkan ke bagian terakhir di baris `$`, kemudian klik `p`
- Cut satu kata `diw`
	- pindahkan ke bagian terakhir di baris `$`, kemudian klik `p`
- Tidak suka? tidak apa, apa kita masih bisa gunakan pindah baris dengan `alt+arah`

## Ubah Ecape ke JJ menuju Ke Normal Mode
- File -> preference -> settings -> cari `vim insert Mode Key Binding`
```json
"vim.insertModeKeyBindings": [
	{
	  "before": ["j", "j"],
	  "after": ["<Esc>"]
	}
]
```
- Ubah untuk bagian visual mode
- File -> preference -> settings -> cari `vim Visual Mode Key Binding`
```php
"vim.visualModeKeyBindings": [
    {
      "before": ["j", "j"],
      "after": ["<Esc>"]
    }
  ]
```
## copy ke clipboard
- File -> preference -> setting -> cari `vim clipboard`