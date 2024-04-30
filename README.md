# PA-B23-KELOMPOK11
*Tugas PA DBMS x ASD Semester 2*
# 🐟✨ [Sea Threat & Damage  Control : Pengawasan ancaman dan Kerusakan terhadap Ekosistem Laut]
Anggota Kelompok :
* Nindya Pramudita (2309116053)
* Fajar Syafatoni Raihanadif (2309116060)
* Ahmad Nuralfansyah (2309116090)
* Muhammad Zakky Atsal Ferlian Ramdanie (2309116096)
# 📝Deskripsi Program
![image](https://github.com/PA-KELOMPOK-11-ASD-X-DBMS/PA-B23-KELOMPOK11/assets/98721112/b598f319-0975-4c5d-b05f-121acb3c3c57)

* Berikut ini adalah project program kelompok kami dengan tema ***Life Below Water*** dari **SDGs _(Sustainable Development Goals)_** atau bisa kita sebut **TPB _(Tujuan Pembangunan Berkelanjutan)_**. Tujuan dari program kami yaitu untuk mewujudkan salah satu tujuan dari program SDGs poin ke-14 itu sendiri, yaitu untuk menjaga kesehatan ekosistem laut,baik dari masalah yang disebabkan oleh manusia maupun alam itu sendiri. Lebih jelasnya, program ini dibuat untuk mengontrol dan mengawasi ancaman dan kerusakan terhadap ekosistem laut setempat.

*  Program kami ini dapat dipakai Multiuser dan terdapat 2 user dengan privelege yang berbeda. Pertama, user masyarakat. Masyarakat dapat melaporkan kondisi atau masalah terkait ekosistem laut disekitarnya serta dapat melihat data data kerusakan. Kedua, user admin. Admin pendataan bertanggung jawab menganalisis dan menyajikan data kerusakan yang terjadi di lingkungan laut seperti jumlah terumbu karang yang rusak, rumput laut yang rusak, letak laut yang rusak dan dapat menyajikan hasil statistik kepada masyarakat yang menggunakan aplikasi ini.

*  Di dalam struktur database MySQLnya, program ini menggunakan database dengan nama **sea_threat_damage_monitor** yang terdiri dari **4** tabel/entitas, yaitu 
entitas **Masyarakat, Admin, Aduan, dan Data_kerusakan**. Semua 4 entitas tersebut menggunakan atribut **"ID"** sebagai **primary key** *( ex : ID_Aduan, ID_Admin, ID_Masyarakat, ID_Data )*.
Selain primary key, di entitas **"Masyarakat"** terdapat atribut ***"nama_lengkap"***, ***"alamat_rumah"***, ***"no_hp"***. Sedangkan di entitas **"Admin"**, terdapat atribut ***"nama_lengkap"***, ***"no_hp_admin***, dan ***"email"***. Selanjutnya, di entitas **"Aduan"** terdapat atribut ***"lokasi"***, ***"tanggal"*** , ***"keterangan_aduan"***, dan ada atribut ***"ID_Masyarakat"*** yang dihubungkan dengan *foreign key*. Terakhir, yaitu entitas **"Data_kerusakan"**. **"Data_kerusakan"** memiliki atribut ***"lokasi"***, ***"tanggal"*** , ***"jenis_kerusakan"***, ***"deskripsi"***, ***"jumlah_kerusakan"***, dan atribut ***"ID_Admin"*** dari entitas **Admin** yang dihubungkan dengan *foreign key*.



# 📝📌 Struktur Project

![Sea Threat Damage drawio (1)](https://github.com/PA-KELOMPOK-11-ASD-X-DBMS/PA-B23-KELOMPOK11/assets/144713730/4eaf01df-9075-4517-a196-f1e65cf55454)
