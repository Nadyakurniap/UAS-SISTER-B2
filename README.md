# UAS-SISTER-B2
KALKULATOR PENGAJUAN KREDIT PEMILIKAN RUMAH (KPR)

# A. Deskripsi Program
Kredit Pemilikan Rumah (KPR) adalah solusi pembiayaan rumah yang banyak digunakan oleh pegawai BUMN, PNS, dan karyawan swasta. Masing-masing kategori pekerjaan ini memiliki ketentuan dan fasilitas berbeda terkait bunga, tenor, serta persyaratan. Namun, calon peminjam sering kesulitan menghitung kemampuan finansial dan memilih skema KPR yang sesuai.

Kalkulator pengajuan KPR ini dirancang untuk mempermudah pegawai BUMN, PNS, dan swasta menghitung estimasi cicilan berdasarkan status pekerjaan, penghasilan, uang muka, dan tenor, sehingga proses perencanaan dan pengambilan keputusan menjadi lebih efektif dan akurat.

# B. Logika Program
### 1. Ambil dan Validasi Data Input
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
  
`double salary = parseFormattedInput(salaryField.getText());
double principal = parseFormattedInput(principalField.getText());
double annualInterestRate = Double.parseDouble(annualInterestRateField.getText().replace(".", ""));
int termInYears = Integer.parseInt(termInYearsField.getText().replace(".", ""));
double downPayment = parseFormattedInput(downPaymentField.getText());`

### 2. Hitung Besaran Kredit
`double loanAmount = principal - downPayment;`

### 3. Konversi Suku Bunga ke Bulanan
`double monthlyInterestRate = (annualInterestRate / 100) / BULAN_DALAM_SETAHUN;`

### 4. Hitung Cicilan Bulanan
`int numberOfPayments = termInYears * BULAN_DALAM_SETAHUN;
double monthlyPayment = loanAmount * (
    (monthlyInterestRate * Math.pow(1 + monthlyInterestRate, numberOfPayments)) / 
    (Math.pow(1 + monthlyInterestRate, numberOfPayments) - 1)
);`

### 5. Hitung Total Pembayaran
`double totalPayment = (monthlyPayment * numberOfPayments) + downPayment;`

### 6. Hitung Maksimal Pinjaman
`double maxMonthlyPayment = salary * MAKS_CICILAN_GAJI;
double maxLoanPossible = maxMonthlyPayment * (
    (Math.pow(1 + monthlyInterestRate, numberOfPayments) - 1) /
    (monthlyInterestRate * Math.pow(1 + monthlyInterestRate, numberOfPayments))
);`

### 7. Hitung Persentase Uang Muka
`double downPaymentPercent = (downPayment / principal) * 100;`

### 8. Evaluasi Kelayakan
`boolean isEligible = monthlyPayment <= maxMonthlyPayment;
String statusText = isEligible ? 
    "Status Kelayakan: LAYAK (Cicilan di bawah 30% gaji)" :
    "Status Kelayakan: TIDAK LAYAK (Cicilan melebihi 30% gaji)";
statusLabel.setText(statusText);
statusLabel.setForeground(isEligible ? new Color(0, 150, 0) : Color.RED);`

### 9. Format dan Tampilan Hasil
`NumberFormat currencyFormat = NumberFormat.getCurrencyInstance(new Locale("id", "ID"));
NumberFormat percentFormat = NumberFormat.getNumberInstance(new Locale("id", "ID"));
percentFormat.setMaximumFractionDigits(2);`
`monthlyPaymentLabel.setText("Cicilan Bulanan: " + currencyFormat.format(monthlyPayment));
totalPaymentLabel.setText("Total Pembayaran: " + currencyFormat.format(totalPayment));
maxLoanLabel.setText("Maksimal Pinjaman: " + currencyFormat.format(maxLoanPossible));
downPaymentAmountLabel.setText("Jumlah Uang Muka: " + currencyFormat.format(downPayment));
downPaymentPercentLabel.setText("Persentase Uang Muka: " + percentFormat.format(downPaymentPercent) + "%");`

### 10. Fungsi Reset
`resetButton.addActionListener(e -> {
    salaryField.setText("");
    principalField.setText("");
    ...
    statusLabel.setText("Status Kelayakan: ");
});`

### 11. Data Aturan 
  Array dari objek EmployeeRules yang menyimpan aturan untuk berbagai status kepegawaian:
  - PNS: Risiko rendah, bunga lebih rendah, tenor lebih panjang, uang muka minimal.
  - BUMN: Risiko menengah, aturan sedikit lebih ketat dari PNS.
  - Swasta: Risiko tinggi, bunga lebih besar, uang muka lebih tinggi.

`private static final EmployeeRules[] EMPLOYEE_RULES = {
    new EmployeeRules("PNS", 7.0, 9.0, 20, 5.0, "Rendah"),
    new EmployeeRules("Pegawai BUMN", 9.0, 11.0, 15, 10.0, "Rendah-Menengah"),
    new EmployeeRules("Pegawai Swasta", 12.0, 15.0, 15, 15.0, "Menengah-Tinggi")
};`


# 3. Fungsi Perhitungan
- Cicilan Bulanan
![image](https://github.com/user-attachments/assets/1a4eb7a5-d8fe-4711-b5c9-7081aa6eee64)
  dengan :
  - P : Jumlah Pinjaman
  - r : Suku Bunga Bulanan
  - n : Jumlah Bulan
    
- Maksimal Pinjaman
  ![image](https://github.com/user-attachments/assets/1d01e675-afb0-4c6c-8be4-1be56489583d)

- Total Pembayaran
  ![image](https://github.com/user-attachments/assets/368a6745-0e8e-46d6-ac9f-f5410c9ee5a8)

- Penentuan Kelayakan
  Cicilan bulanan dibandingkan dengan 30% gaji:
    - LAYAK: Jika cicilan ≤ 30% gaji.
    - TIDAK LAYAK: Jika cicilan > 30% gaji.


# KESIMPULAN
Kode ini dirancang untuk mempermudah simulasi KPR bagi pegawai dengan GUI yang intuitif. Tiap bagian dirancang modular sehingga mudah dimodifikasi sesuai kebutuhan, seperti menambahkan status baru atau mengubah aturan kredit.


