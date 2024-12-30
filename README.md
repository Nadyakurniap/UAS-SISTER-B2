# UAS-SISTER-B2
KALKULATOR PENGAJUAN KREDIT PEMILIKAN RUMAH (KPR)

# 1. Deskripsi Program
Kredit Pemilikan Rumah (KPR) adalah solusi pembiayaan rumah yang banyak digunakan oleh pegawai BUMN, PNS, dan karyawan swasta. Masing-masing kategori pekerjaan ini memiliki ketentuan dan fasilitas berbeda terkait bunga, tenor, serta persyaratan. Namun, calon peminjam sering kesulitan menghitung kemampuan finansial dan memilih skema KPR yang sesuai.

Kalkulator pengajuan KPR ini dirancang untuk mempermudah pegawai BUMN, PNS, dan swasta menghitung estimasi cicilan berdasarkan status pekerjaan, penghasilan, uang muka, dan tenor, sehingga proses perencanaan dan pengambilan keputusan menjadi lebih efektif dan akurat.

# 2. Logika Program
1. Ambil dan Validasi Data Input
Data yang diambil dari pengguna meliputi:
- Gaji Bulanan (salary)
- Jumlah Pinjaman (principal)
- Status Kepegawaian (employeeType) (PNS, BUMN, atau Swasta)
- Suku Bunga Tahunan (annualInterestRate)
- Jangka Waktu (termInYears)
- Uang Muka (downPayment)

  Validasi:
- Pastikan nilai input valid (positif, tidak kosong).
- Validasi aturan berdasarkan status kepegawaian:
  - Jangka waktu: Tidak boleh melebihi batas maksimal aturan status kepegawaian.
  - Suku bunga: Harus berada dalam rentang aturan.
  - Uang muka: Persentase uang muka harus memenuhi minimal aturan.

  Jika ada pelanggaran aturan, program menampilkan pesan error dan menghentikan perhitungan.
  
  double salary = parseFormattedInput(salaryField.getText());
  double principal = parseFormattedInput(principalField.getText());
  double annualInterestRate = Double.parseDouble(annualInterestRateField.getText().replace(".", ""));
  int termInYears = Integer.parseInt(termInYearsField.getText().replace(".", ""));
  double downPayment = parseFormattedInput(downPaymentField.getText());

# 3. Fungsi Perhitungan
![image](https://github.com/user-attachments/assets/1a4eb7a5-d8fe-4711-b5c9-7081aa6eee64)




