## 📌Fitur dan Fungsionalitas
-  mengimport module dan library

1. mysql.connector: Modul ini adalah penghubung antara Python dan database MySQL. Dengan menggunakan modul ini, pengguna dapat terhubung, mengirim, dan menerima data dari database MySQL menggunakan Python.
2. PrettyTable: PrettyTable adalah sebuah modul Python yang memungkinkan untuk membuat tabel yang rapi dan mudah dibaca dari data dalam bentuk list atau dictionary.
3. matplotlib.pyplot: Matplotlib adalah library plotting Python yang populer. pyplot adalah modul yang menyediakan fungsi untuk membuat berbagai jenis plot, seperti garis, histogram, scatter plot, dan sebagainya. Dengan menggunakan matplotlib.pyplot, pengguna dapat membuat visualisasi data dari hasil query database atau data lainnya.
4. datetime: Modul ini menyediakan kelas untuk memanipulasi tanggal dan waktu dalam Python. Dengan menggunakan datetime, dapat melakukan manipulasi tanggal dan waktu, seperti mengambil waktu saat ini, memformat tanggal, atau melakukan operasi matematika pada tanggal.
5. os: Modul ini menyediakan fungsi-fungsi untuk berinteraksi dengan sistem operasi. Dengan menggunakan os, dapat melakukan berbagai operasi terkait sistem operasi, seperti mengakses file, mengubah direktori kerja, atau menjalankan perintah sistem.

```
import mysql.connector 
from prettytable import PrettyTable 
import matplotlib.pyplot as plt 
from datetime import datetime 
import os
```

Fungsi os.system('cls') membersihkan layar pada Windows. Ini berguna untuk menghapus teks yang ada di layar sebelum menampilkan output baru.
```
os.system('cls')
```


Fungsi buat_koneksi() digunakan untuk membuat koneksi ke database MySQL. Dalam fungsi ini, kita menggunakan mysql.connector.connect() untuk membuat koneksi dengan parameter host, user, password, dan database yang sesuai. Autocommit disetel sebagai True untuk memastikan bahwa setiap perubahan yang dilakukan langsung di-commit ke database.
```
def buat_koneksi():  
    return mysql.connector.connect(  
        host="localhost",  
        user="root",
        password="",
        database="sea_threat_damage_monitor",  
        autocommit=True  
    )
```

Fungsi get_input(prompt) bertujuan untuk mendapatkan input dari pengguna dengan menampilkan pesan prompt. Saat pengguna memberikan input, fungsi akan mengembalikan nilai input tersebut. Jika terjadi KeyboardInterrupt (biasanya karena pengguna menekan Ctrl+C), pesan akan dicetak, dan proses input akan dibatalkan.
```
def get_input(prompt):
    while True:
        try:
            user_input = input(prompt)
            # Lakukan validasi atau pemrosesan tambahan di sini jika diperlukan
            return user_input
        except KeyboardInterrupt:
            print("\nKeyboardInterrupt terdeteksi. Masukkan dibatalkan.")
```


Di kelas User, terdapat method __init__() yang digunakan sebagai constructor. Saat sebuah objek User dibuat, method ini akan dipanggil untuk menginisialisasi atribut-atribut objek, seperti nama_lengkap dan no_hp.
```
class User:  
    def __init__(self, nama_lengkap, no_hp,):  
        self.nama_lengkap = nama_lengkap  
        self.no_hp = no_hp
```

- USER SEBAGAI ADMIN

Di kelas Admin, terdapat method __init__() yang mewarisi method __init__() dari kelas User menggunakan fungsi super(). Selain atribut yang diwarisi dari kelas User, kelas Admin memiliki atribut tambahan yaitu ID_Admin.
```
class Admin(User):  
    def __init__(self, ID_Admin, nama_lengkap, no_hp):  
        super().__init__(nama_lengkap, no_hp)  
        self.nama_lengkap = nama_lengkap
        self.ID_Admin = ID_Admin
```

Metode merge adalah metode statis yang menggabungkan dua list yang diurutkan berdasarkan nilai kunci tertentu. Parameter yang diterima adalah left (list pertama), right (list kedua), key (kunci untuk pengurutan), dan reverse (opsional, bila True, urutannya akan dibalik). Metode ini melakukan iterasi melalui kedua list, membandingkan nilai kunci, dan menambahkan elemen ke result sesuai dengan kondisi pengurutan, kemudian mengembalikan result yang telah digabungkan.
```
@staticmethod  
    def merge(left, right, key, reverse=False):  
        result = []  
        i = j = 0  
        while i < len(left) and j < len(right):  
            if not reverse:  
                if left[i][key] < right[j][key]:  
                    result.append(left[i])  
                    i += 1  
                else:
                    result.append(right[j])  
                    j += 1  
            else:  
                if left[i][key] > right[j][key]:  
                    result.append(left[i])  
                    i += 1  
                else:
                    result.append(right[j])  
                    j += 1  
        result.extend(left[i:])  
        result.extend(right[j:])  
        return result
```

Metode merge_sort adalah metode statis yang menggunakan algoritma merge sort untuk mengurutkan list arr berdasarkan nilai kunci tertentu. Parameter yang diterima adalah arr (list yang akan diurutkan), key (kunci untuk pengurutan), dan reverse (opsional, bila True, urutannya akan dibalik).
```
@staticmethod  
    def merge_sort(arr, key, reverse=False):  
        if len(arr) <= 1:  
            return arr  
        mid = len(arr) // 2  
        left = Admin.merge_sort(arr[:mid], key, reverse)  
        right = Admin.merge_sort(arr[mid:], key, reverse)  
        return Admin.merge(left, right, key, reverse)
```


Fungsi sort_merge_data_kerusakan mengambil data kerusakan dari database, mengurutkannya menggunakan merge sort, dan menampilkan dalam tabel menggunakan PrettyTable.
```
def sort_merge_data_kerusakan(self, conn, key, reverse=False):  
        cursor = conn.cursor()  
        try:  
            cursor.execute(
                "SELECT ID_data, lokasi, DATE_FORMAT(Tanggal, '%d %M %Y'), jenis_kerusakan, deskripsi, jumlah_kerusakan FROM data_kerusakan"  
            )
            data_kerusakan = cursor.fetchall()  

            sorted_data = self.merge_sort(data_kerusakan, key=key, reverse=reverse)  


            table = PrettyTable()  
            table.field_names = ["ID_data", "Lokasi", "Tanggal", "Jenis Kerusakan", "Deskripsi", "Jumlah Kerusakan"]  


            for data in sorted_data:  
                table.add_row(data)  

            print(f"\nData Kerusakan:")  
            table.max_width["Deskripsi"] = 80  
            print(table)  

        except mysql.connector.Error as err:  
            print("Gagal mengambil data kerusakan:", err)  
        finally:  
            cursor.close()
```

Fungsi menu_sorting_data_kerusakan meminta pengguna untuk memilih kriteria pengurutan data kerusakan, yaitu berdasarkan lokasi atau tanggal. Kemudian, fungsi ini menggunakan metode sort_merge_data_kerusakan untuk melakukan pengurutan data sesuai dengan pilihan pengguna.
```
 def menu_sorting_data_kerusakan(self):  
        choice = input("\nIngin Sorting Berdasarkan Apa? (Lokasi/Tanggal): ").lower()  
        conn = buat_koneksi()  
        if choice == "lokasi":  
            self.sort_merge_data_kerusakan(conn, key=1)  
        elif choice == "tanggal":  
            self.sort_merge_data_kerusakan(conn, key=2)  
        else:
            print("Pilihan Tidak Ada!")  
        conn.close()
```

Fungsi sort_merge_aduan digunakan untuk mengambil data aduan dari database, mengurutkannya berdasarkan kriteria tertentu menggunakan merge sort, dan menampilkannya dalam bentuk tabel menggunakan PrettyTable.
```
def sort_merge_aduan(self, conn, key, reverse=False):  
        cursor = conn.cursor()   
        try:  
            cursor.execute("SELECT * FROM aduan")  
            aduan = cursor.fetchall()   

            sorted_data = self.merge_sort(aduan, key=key, reverse=reverse)  


            table = PrettyTable()  
            table.field_names = ["ID Aduan", "ID Masyarakat", "Lokasi", "Tanggal", "Keterangan"]  


            for data in sorted_data:  
                table.add_row(data)  


            print(f"\nData Aduan Masyarakat (Diurutkan berdasarkan {key}):")  
            table.max_width["Keterangan"] = 80  
            print(table)  

        except mysql.connector.Error as err:  
            print("Gagal mengambil data aduan masyarakat:", err)
        finally:  
            cursor.close()  
            conn.close()  
```

Fungsi menu_sorting_aduan meminta pengguna untuk memilih kriteria pengurutan data aduan, yaitu berdasarkan lokasi atau tanggal. Kemudian, fungsi ini menggunakan metode sort_merge_aduan untuk melakukan pengurutan data sesuai dengan pilihan pengguna.
```
def menu_sorting_aduan(self):  
        choice = input("\nIngin Sorting Berdasarkan Apa? (Lokasi/Tanggal): ").lower()  
        conn = buat_koneksi()  
        if  choice == "lokasi":   
            self.sort_merge_aduan(conn, key=2)  
        elif choice == "tanggal":
            self.sort_merge_aduan(conn, key=3)  
        else:
            print("Pilihan Tidak Ada!")  
```


Fungsi fibonacci_search adalah implementasi algoritma pencarian Fibonacci dalam Python untuk mencari elemen x dalam list arr berdasarkan kunci tertentu key.
```
 def fibonacci_search(self, arr, x, key):  
        fibMMm2 = 0   
        fibMMm1 = 1   
        fibM = fibMMm2 + fibMMm1   


        while (fibM < len(arr)):  
            fibMMm2 = fibMMm1  
            fibMMm1 = fibM  
            fibM = fibMMm2 + fibMMm1  

        offset = -1  

        while (fibM > 1):  
            i = min(offset + fibMMm2, len(arr) - 1)  


            if (arr[i][key] < x):  
                fibM = fibMMm1  
                fibMMm1 = fibMMm2
                fibMMm2 = fibM - fibMMm1  
                offset = i  


            elif (arr[i][key] > x):  
                fibM = fibMMm2  
                fibMMm1 = fibMMm1 - fibMMm2 
                fibMMm2 = fibM - fibMMm1


            else:
                return i  


        if (fibMMm1 and arr[offset + 1][key] == x):  
            return offset + 1


        return -1  
```

Fungsi menu_search_data_kerusakan digunakan untuk melakukan pencarian data kerusakan berdasarkan kriteria tertentu seperti ID, lokasi, atau tanggal. Parameter yang diterima adalah key (kriteria pencarian) dan value (nilai yang dicari).
```
def menu_search_data_kerusakan(self, key, value):  
        conn = buat_koneksi()  
        cursor = conn.cursor() 
        try:  
            cursor.execute(
                "SELECT ID_data, lokasi, DATE_FORMAT(Tanggal, '%d %M %Y'), jenis_kerusakan, deskripsi, jumlah_kerusakan FROM data_kerusakan"
            ) 
            data_kerusakan = cursor.fetchall() 


            search_table = PrettyTable()  
            search_table.field_names = ["ID Data", "Lokasi", "Tanggal", "Jenis Kerusakan", "Deskripsi", "Jumlah Kerusakan"]  


            if key == "ID_data":  

                found = False  
                for data in data_kerusakan:  
                    if str(data[0]) == value:  
                        search_table.add_row(data)  
                        found = True  
                        break  
                if not found:  
                    print(f"Tidak ada data dengan ID {value}.")  
                    return  
            elif key == "lokasi":  

                found = False   
                for data in data_kerusakan:  
                    if data[1].strip().lower() == value.lower():   
                        search_table.add_row(data)  
                        found = True  
                        break    
                if not found:  
                    print(f"Tidak ada data dengan lokasi {value}.")  
                    return  
            elif key == "tanggal":  

                value = datetime.strptime(value, '%d-%m-%Y').strftime('%d %B %Y')  


                found = False 
                for data in data_kerusakan:  
                    if data[2] == value: 
                        search_table.add_row(data) 
                        found = True  
                if not found:   
                    print(f"Tidak ada data pada tanggal {value}.")  
                    return  
            else:
                print("Key tidak valid. Silakan coba lagi.")  
                return


            search_table.max_width["Deskripsi"] = 80  


            print("Pencarian ditemukan!")
            print(f"\nHasil Pencarian (Berdasarkan {key}):")
            print(search_table)  

        except mysql.connector.Error as err:  
            print("Gagal melakukan pencarian data kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```


Fungsi menu_search_aduan digunakan untuk melakukan pencarian data aduan masyarakat berdasarkan kriteria tertentu seperti ID, lokasi, atau tanggal. Parameter yang diterima adalah key (kriteria pencarian) dan value (nilai yang dicari).
```
def menu_search_aduan(self, key, value):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT * FROM aduan")  
            aduan_masyarakat = cursor.fetchall()  


            search_table = PrettyTable()  
            search_table.field_names = ["IDAduan", "ID Masyarakat", "Lokasi", "Tanggal", "Keterangan"]  


            if key == "ID Aduan":  

                found = False  
                for aduan in aduan_masyarakat:  
                    if str(aduan[0]) == value:  
                        search_table.add_row(aduan)  
                        found = True  
                        break  
                if not found:   
                    print(f"Tidak ada aduan dengan ID {value}.")  
                    return  
            elif key == "lokasi":  

                found = False  
                for aduan in aduan_masyarakat:
                    if aduan[2].strip().lower() == value.strip().lower():   
                        search_table.add_row(aduan)  
                        found = True  
                        break   
                if not found:  
                    print(f"Tidak ada aduan dengan lokasi {value}.")   
                    return  
            elif key == "tanggal":   

                found = False  
                for aduan in aduan_masyarakat:   
                    if aduan[3].strftime('%Y-%m-%d') == value:  
                        search_table.add_row(aduan)  
                        found = True  
                if not found:  
                    print(f"Tidak ada aduan pada tanggal {value}.")   
                    return  
            else:
                print("Key tidak valid. Silakan coba lagi.")  
                return

            search_table.max_width["Keterangan"] = 80  


            print(f"\nHasil Pencarian (Berdasarkan {key}):")
            print(search_table)   

        except mysql.connector.Error as err:  
            print("Gagal melakukan pencarian aduan masyarakat:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi menu_search_masyarakat digunakan untuk mencari informasi akun masyarakat berdasarkan ID masyarakat. Fungsi ini juga menampilkan daftar aduan yang dibuat oleh masyarakat yang bersangkutan. Parameter yang diterima adalah field (kolom yang digunakan untuk pencarian, dalam hal ini ID_Masyarakat) dan value (nilai yang dicari).
```
def menu_search_masyarakat(self, field, value):  
        conn = buat_koneksi()  
        cursor = conn.cursor() 
        try:  
            cursor.execute(f"SELECT * FROM masyarakat WHERE `ID_Masyarakat` = %s", (value,))  
            masyarakat = cursor.fetchone()  

            if masyarakat:  
                print("\nInformasi Akun Masyarakat:")
                print("ID Masyarakat:", masyarakat[0])  
                print("Nama:", masyarakat[1])  
                print("Alamat:", masyarakat[2])  
                print("Nomor Telepon:", masyarakat[3])  


                cursor.execute("SELECT * FROM aduan WHERE ID_Masyarakat = %s", (value,))   
                aduan_masyarakat = cursor.fetchall()  

                if aduan_masyarakat: 

                    table_aduan = PrettyTable()  
                    table_aduan.field_names = ["ID Aduan", "ID_Masyarakat", "Lokasi", "Tanggal", "Keterangan"]  


                    for aduan in aduan_masyarakat:  
                        table_aduan.add_row(aduan[0:5])   


                    table_aduan.max_width["Keterangan"] = 80  
                    print("\nDaftar Aduan Masyarakat Yang  Dibuat:")
                    print(table_aduan)   
                else:
                    print("\nMasyarakat ini belum membuat aduan!")

            else:
                print("Masyarakat dengan ID tersebut tidak ditemukan.")

        except mysql.connector.Error as err:  
            print("Gagal mencari akun masyarakat:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi lihat_data_kerusakan digunakan untuk menampilkan semua data kerusakan yang tersimpan dalam database. Fungsi ini mengambil data dari tabel data_kerusakan, memformatnya, dan menampilkannya dalam bentuk tabel menggunakan PrettyTable.
```
def lihat_data_kerusakan(self):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT ID_data, lokasi, DATE_FORMAT(Tanggal, '%d %M %Y'), jenis_kerusakan, deskripsi, jumlah_kerusakan FROM data_kerusakan")  
            data_kerusakan = cursor.fetchall()  


            table = PrettyTable()  
            table.field_names = ["ID Data", "Lokasi", "Tanggal", "Jenis Kerusakan", "Deskripsi", "Jumlah Kerusakan"]  


            for data in data_kerusakan:  
                table.add_row(data)  


            table.max_width["Deskripsi"] = 80  


            print("\nData Kerusakan:")
            print(table) 

        except mysql.connector.Error as err:  
            print("Gagal mengambil data kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi hapus_data_kerusakan digunakan untuk menghapus data kerusakan dari database berdasarkan ID data yang diberikan. Fungsi ini terlebih dahulu memeriksa apakah data dengan ID yang diberikan ada dalam database. Jika ada, maka data tersebut dihapus dari tabel data_kerusakan. Setelah penghapusan, fungsi ini memanggil kembali fungsi lihat_data_kerusakan untuk menampilkan data kerusakan yang tersisa.
```
def hapus_data_kerusakan(self, id_data):
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  

            cursor.execute("SELECT ID_data FROM data_kerusakan WHERE ID_data = %s", (id_data,))  
            result = cursor.fetchone()  

            if result:

                query = "DELETE FROM data_kerusakan WHERE ID_data = %s"  
                cursor.execute(query, (id_data,))  
                conn.commit()  
                print("Data kerusakan berhasil dihapus.")
            else:

                print(f"Tidak ada data kerusakan dengan ID {id_data}.")


            self.lihat_data_kerusakan()  

        except mysql.connector.Error as err:  
            print("Gagal menghapus data kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi pindahkan_ke_data_kerusakan bertanggung jawab untuk memindahkan aduan yang ada ke data kerusakan. Proses ini melibatkan input dari pengguna untuk memilih jenis kerusakan dan mengisi deskripsi serta jumlah kerusakan. Setelah proses ini selesai, aduan dipindahkan ke data kerusakan dan dihapus dari tabel aduan.
```
def pindahkan_ke_data_kerusakan(self, cursor, id_aduan, id_admin):  
        try:  

            query = """
                SELECT aduan.lokasi, aduan.Tanggal, aduan.keterangan_aduan, data_kerusakan.jenis_kerusakan
                FROM aduan
                LEFT JOIN data_kerusakan ON aduan.ID_Aduan = data_kerusakan.ID_Aduan
                WHERE aduan.ID_Aduan = %s 
            """  

            cursor.execute(query, (id_aduan,))  
            aduan_info = cursor.fetchone()  

            if aduan_info:  
                lokasi, tanggal, keterangan_aduan, jenis_kerusakan = aduan_info   
                print(f"\n[Informasi Aduan!]\nLokasi: {lokasi}\nTanggal: {tanggal}\nKeterangan: {keterangan_aduan}")




                print("Pilih jenis kerusakan:")
                print("1. Hewan laut")
                print("2. Terumbu karang")
                print("3. Limbah laut")
                print("4. Kapal laut")
                print("5. Lainnya")
                jenis_kerusakan_input = input("Silahkan Pilih jenis kerusakan: ") 
                if jenis_kerusakan_input == "1":  
                    jenis_kerusakan = "Hewan laut" 
                elif jenis_kerusakan_input == "2":
                    jenis_kerusakan = "Terumbu karang"
                elif jenis_kerusakan_input == "3":
                    jenis_kerusakan = "Limbah laut"
                elif jenis_kerusakan_input == "4":
                    jenis_kerusakan = "Kapal laut"
                elif jenis_kerusakan_input == "5":
                    jenis_kerusakan = "Lainnya"
                else:
                    print("Pilihan kategori tidak valid.")

                if jenis_kerusakan_input in {"1", "2", "3", "4", "5"}:  

                    deskripsi_kerusakan = input("Perbarui Keterangan: ")  


                    while True:  
                        jumlah_kerusakan = input("Masukan Jumlah Kerusakan: ")  
                        if jumlah_kerusakan.isdigit():   
                            jumlah_kerusakan = int(jumlah_kerusakan)   
                            break
                        else:
                            print("Masukan harus berupa angka. Silakan coba lagi.")


                query_insert = """
                    INSERT INTO data_kerusakan (ID_Admin, lokasi, tanggal, jenis_kerusakan, deskripsi, jumlah_kerusakan, ID_Aduan)
                    VALUES (%s, %s, %s, %s, %s, %s, %s)
                """  
                cursor.execute(query_insert, (id_admin, lokasi, tanggal, jenis_kerusakan, deskripsi_kerusakan, jumlah_kerusakan, id_aduan))  

                print("Aduan berhasil dipindahkan ke data kerusakan.")
                query_delete = "DELETE FROM aduan WHERE ID_Aduan = %s"  
                cursor.execute(query_delete, (id_aduan,))  
            else:
                print("ID Aduan tidak ditemukan. ID harus berupa angka!")

        except mysql.connector.Error as err:  
            print("Gagal memindahkan aduan ke data kerusakan:", err)
```

Fungsi update_data_kerusakan memungkinkan admin untuk memperbarui data kerusakan dengan memindahkan aduan tertentu ke data kerusakan. Admin dapat memilih aduan yang ingin dipindahkan dengan memasukkan ID aduan. Setelah pemilihan, aduan akan diproses dan dipindahkan ke data kerusakan.
```
def update_data_kerusakan(self):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT ID_Aduan, ID_Masyarakat, lokasi, Tanggal, keterangan_aduan FROM aduan")  
            aduan_masyarakat = cursor.fetchall()  


            table = PrettyTable()  
            table.field_names = ["ID Aduan", "ID Masyarakat", "Lokasi", "Tanggal", "Keterangan"]  


            for aduan in aduan_masyarakat:  
                table.add_row(aduan)   


            print("\nDaftar Aduan Masyarakat:")
            table.max_width["Keterangan"] = 80  
            print(table)

            id_aduan = input("\nPilih ID_aduan aduan --> data kerusakan(kosongkan untuk kembali): ")
            if id_aduan:

                result = self.pindahkan_ke_data_kerusakan(cursor, id_aduan, self.ID_Admin)

        except mysql.connector.Error as err:  
            print("Gagal mengambil daftar aduan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi lihat_aduan_masyarakat digunakan untuk menampilkan daftar aduan yang telah dibuat oleh masyarakat. Fungsi ini mengambil data dari tabel aduan, memformatnya, dan menampilkannya dalam bentuk tabel menggunakan PrettyTable.
```
def lihat_aduan_masyarakat(self):
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT * FROM aduan")  
            aduan_masyarakat = cursor.fetchall()  


            table = PrettyTable()   
            table.field_names = ["ID Aduan", "ID Masyarakat", "Lokasi", "Tanggal", "Keterangan"]  


            for aduan in aduan_masyarakat:  
                table.add_row(aduan)  


            print("\nDaftar Aduan Masyarakat:")

            table.max_width["Keterangan"] = 80  
            print(table)  

        except mysql.connector.Error as err:  
            print("Gagal mengambil daftar aduan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi hapus_aduan digunakan untuk menghapus aduan dari database berdasarkan ID aduan yang diberikan. Fungsi ini memeriksa terlebih dahulu apakah ID aduan yang diberikan valid (berupa angka). Jika valid, maka aduan akan dihapus dari tabel aduan.
```
def hapus_aduan(self, id_aduan):

        if not id_aduan.isdigit():  
            print("ID aduan harus berupa angka.")
            return
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  

            cursor.execute("SELECT ID_Aduan FROM aduan WHERE ID_Aduan = %s", (id_aduan,))  
            result = cursor.fetchone()  

            if result:  

                query = "DELETE FROM aduan WHERE ID_Aduan = %s"  
                cursor.execute(query, (id_aduan,))  
                conn.commit()  

                print("Aduan berhasil dihapus.")
            else:

                print(f"Tidak ada aduan dengan ID {id_aduan}.")

        except mysql.connector.Error as err:  
            print("Gagal menghapus aduan:", err)
        finally:
            cursor.close()  
            conn.close()  
```


Fungsi lihat_akun_masyarakat digunakan untuk menampilkan daftar akun masyarakat beserta jumlah aduan yang telah dibuat oleh setiap masyarakat. Fungsi ini mengambil data dari tabel masyarakat dan aduan, melakukan penggabungan data dengan menggunakan operasi LEFT JOIN, mengelompokkan data berdasarkan ID masyarakat, dan menghitung jumlah aduan yang dibuat oleh setiap masyarakat. Hasilnya ditampilkan dalam bentuk tabel menggunakan PrettyTable.
```
def lihat_akun_masyarakat(self):
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("""
                SELECT m.ID_Masyarakat, m.Nama_Lengkap, m.Alamat_Rumah, m.No_HP, 
                    COUNT(a.ID_Aduan) AS jumlah_aduan
                FROM masyarakat m
                LEFT JOIN aduan a ON m.ID_Masyarakat = a.ID_Masyarakat 
                GROUP BY m.ID_Masyarakat
            """)  


            akun_masyarakat = cursor.fetchall()  


            table = PrettyTable()  
            table.field_names = ["ID Masyarakat", "Nama Lengkap", "Alamat Rumah", "Nomor HP", "Jumlah Aduan"]  


            for akun in akun_masyarakat:  
                table.add_row(akun)  


            print("\nDaftar Akun Masyarakat:")
            print(table)  

        except mysql.connector.Error as err:  
            print("Gagal mengambil daftar akun masyarakat:", err)
        finally:
            cursor.close()  
            conn.close()  
```


- USER SEBAGAI MASYARAKAT

Kelas Masyarakat merupakan subkelas dari kelas User. Dalam inisialisasinya, kelas ini menerima ID Masyarakat, nama lengkap, nomor HP, dan alamat rumah. Kemudian, atribut-atribut tersebut diassign ke dalam objek. Perlu diperhatikan bahwa atribut no_hp juga disimpan sebagai nomor_hp dan tidak digunakan atribut ID_Masyarakat untuk nomor KTP.
```
class Masyarakat(User):   
    def __init__(self, ID_Masyarakat, nama_lengkap, no_hp, alamat_rumah):  
        super().__init__(nama_lengkap, no_hp)   
        self.ID_Masyarakat = ID_Masyarakat
        self.alamat_rumah = alamat_rumah
        self.nomor_hp = no_hp
        self.no_ktp = ID_Masyarakat
```

Fungsi edit_informasi_akun memungkinkan masyarakat untuk mengedit informasi akun mereka, seperti alamat dan nomor HP. Pengguna diminta untuk memasukkan alamat baru dan nomor HP baru. Jika input nomor HP bukan berupa angka, pengguna akan diminta untuk memasukkan ulang. Setelah itu, informasi akun akan diperbarui dalam database
```
def edit_informasi_akun(self):   

        print("\nEdit Informasi Akun:")
        alamat_baru = input("Masukkan alamat baru: ")  
        

        while True:  
            nomor_hp_baru = input("Masukkan nomor HP baru: ")  
            if nomor_hp_baru.isdigit():  
                break
            else:
                print("Nomor HP hanya boleh berisi angka. Silakan coba lagi.")


        if alamat_baru.strip() == "" or nomor_hp_baru.strip() == "":  
            print("Alamat dan nomor HP tidak boleh kosong. Silakan coba lagi.")
        else:

            conn = buat_koneksi()  
            cursor = conn.cursor()  
            try:
                cursor.execute("UPDATE masyarakat SET alamat_rumah = %s, no_hp = %s WHERE ID_Masyarakat = %s",
                            (alamat_baru, nomor_hp_baru, self.ID_Masyarakat))  
                conn.commit()  
                print("Informasi akun berhasil diperbarui.")
                self.alamat_rumah = alamat_baru   
                self.nomor_hp = nomor_hp_baru  
            except mysql.connector.Error as err:  
                print("Gagal memperbarui informasi akun:", err)
            finally:
                cursor.close()  
                conn.close()  
```

Fungsi lihat_dan_edit_informasi_akun memberikan opsi kepada pengguna untuk melihat dan mengedit informasi akun mereka. Pengguna akan ditampilkan informasi akun mereka terlebih dahulu, kemudian diberikan opsi untuk mengedit informasi akun atau kembali ke menu utama
```
def lihat_dan_edit_informasi_akun(self):   
        while True:  
            self.tampilkan_informasi_akun()   

            print("\n\033[0m\033[91m1.\033[0m Edit Informasi Akun")
            print("\033[0m\033[91m2.\033[0m Kembali ke Menu Utama")

            pilihan = input("Masukkan pilihan Anda: ")

            if pilihan == "1":

                self.edit_informasi_akun() 
            elif pilihan == "2":

                break  
            else:
                print("Pilihan tidak valid. Silakan pilih opsi yang valid.")
```

Fungsi sort_merge_data_kerusakan digunakan untuk mengambil data kerusakan dari database, mengurutkannya berdasarkan kriteria yang diinginkan, dan menampilkannya dalam bentuk tabel
```
 def sort_merge_data_kerusakan(self, conn, key, reverse=False):  
        cursor = conn.cursor()  
        try:  
            cursor.execute(
                "SELECT ID_data, lokasi, DATE_FORMAT(Tanggal, '%d %M %Y'), jenis_kerusakan, deskripsi, jumlah_kerusakan FROM data_kerusakan"  
            )  
            data_kerusakan = cursor.fetchall()  

            sorted_data = self.merge_sort(data_kerusakan, key=key, reverse=reverse)  


            table = PrettyTable()   
            table.field_names = ["ID_data", "Lokasi", "Tanggal", "Jenis Kerusakan", "Deskripsi", "Jumlah Kerusakan"]  


            for data in sorted_data:  
                table.add_row(data)   


            print(f"\nData Kerusakan:")
            table.max_width["Deskripsi"] = 80  
            print(table)  

        except mysql.connector.Error as err:  
            print("Gagal mengambil data kerusakan:", err)
        finally:
            cursor.close()  
```

Fungsi menu_sorting_data_kerusakan meminta pengguna untuk memilih kriteria apa yang ingin digunakan untuk mengurutkan data kerusakan (lokasi atau tanggal). Kemudian, fungsi ini memanggil sort_merge_data_kerusakan dengan kriteria yang dipilih oleh pengguna.
```
def menu_sorting_data_kerusakan(self):  
        choice = input("\nIngin Sorting Berdasarkan Apa? (Lokasi/Tanggal): ").lower()  
        conn = buat_koneksi()  
        if choice == "lokasi":  
            self.sort_merge_data_kerusakan(conn, key=1)   
        elif choice == "tanggal":   
            self.sort_merge_data_kerusakan(conn, key=2)  
        else:
            print("Pilihan Tidak Ada!")  
        conn.close()  
```


Fungsi fibonacci_search melakukan pencarian dalam array menggunakan algoritma pencarian Fibonacci. Ini memerlukan array yang sudah diurutkan (secara default dalam mode naik) dan mengembalikan indeks dari elemen yang cocok dengan nilai x, atau -1 jika tidak ada yang cocok
```
def fibonacci_search(self, arr, x, key):  
            fibMMm2 = 0    
            fibMMm1 = 1    
            fibM = fibMMm2 + fibMMm1   


            while (fibM < len(arr)):  
                fibMMm2 = fibMMm1  
                fibMMm1 = fibM  
                fibM = fibMMm2 + fibMMm1  

            offset = -1  

            while (fibM > 1):  
                i = min(offset + fibMMm2, len(arr) - 1)  


                if (arr[i][key] < x):  
                    fibM = fibMMm1  
                    fibMMm1 = fibMMm2
                    fibMMm2 = fibM - fibMMm1  
                    offset = i  


                elif (arr[i][key] > x):  
                    fibM = fibMMm2  
                    fibMMm1 = fibMMm1 - fibMMm2 
                    fibMMm2 = fibM - fibMMm1


                else:
                    return i  


            if (fibMMm1 and arr[offset + 1][key] == x):  
                return offset + 1


            return -1  
```


Fungsi menu_search_data_kerusakan bertujuan untuk mencari data kerusakan berdasarkan kriteria tertentu, seperti ID, lokasi, atau tanggal. Ini mengambil parameter key untuk menentukan kriteria pencarian dan value untuk nilai yang ingin dicari
```
def menu_search_data_kerusakan(self, key, value):  
        conn = buat_koneksi()  
        cursor = conn.cursor() 
        try:  
            cursor.execute(
                "SELECT ID_data, lokasi, DATE_FORMAT(Tanggal, '%d %M %Y'), jenis_kerusakan, deskripsi, jumlah_kerusakan FROM data_kerusakan"
            ) 
            data_kerusakan = cursor.fetchall() 


            search_table = PrettyTable()  
            search_table.field_names = ["ID Data", "Lokasi", "Tanggal", "Jenis Kerusakan", "Deskripsi", "Jumlah Kerusakan"]  


            if key == "ID_data":  

                found = False  
                for data in data_kerusakan:  
                    if str(data[0]) == value:  
                        search_table.add_row(data)  
                        found = True  
                        break  
                if not found:  
                    print(f"Tidak ada data dengan ID {value}.")  
                    return  
            elif key == "lokasi":  

                found = False   
                for data in data_kerusakan:  
                    if data[1].strip().lower() == value.lower():   
                        search_table.add_row(data)  
                        found = True  
                        break    
                if not found:  
                    print(f"Tidak ada data dengan lokasi {value}.")  
                    return  
            elif key == "tanggal":  

                value = datetime.strptime(value, '%d-%m-%Y').strftime('%d %B %Y')  


                found = False 
                for data in data_kerusakan:  
                    if data[2] == value: 
                        search_table.add_row(data) 
                        found = True  
                if not found:   
                    print(f"Tidak ada data pada tanggal {value}.")  
                    return  
            else:
                print("Key tidak valid. Silakan coba lagi.")  
                return


            search_table.max_width["Deskripsi"] = 80  


            print("Pencarian ditemukan!")
            print(f"\nHasil Pencarian (Berdasarkan {key}):")
            print(search_table)  

        except mysql.connector.Error as err:  
            print("Gagal melakukan pencarian data kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi lihat_data_kerusakan digunakan untuk menampilkan seluruh data kerusakan yang ada dalam database
```
def lihat_data_kerusakan(self):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT ID_data, lokasi, DATE_FORMAT(Tanggal, '%d %M %Y'), jenis_kerusakan, deskripsi, jumlah_kerusakan FROM data_kerusakan")  
            data_kerusakan = cursor.fetchall()  


            table = PrettyTable()  
            table.field_names = ["ID Data", "Lokasi", "Tanggal", "Jenis Kerusakan", "Deskripsi", "Jumlah Kerusakan"]  


            for data in data_kerusakan:  
                table.add_row(data)  


            table.max_width["Deskripsi"] = 80  


            print("\nData Kerusakan:")
            print(table) 

        except mysql.connector.Error as err:  
            print("Gagal mengambil data kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi ini mengambil data kerusakan dari database dan mengelompokkannya berdasarkan jenis kerusakan. Data ini kemudian digunakan untuk membuat diagram batang yang menampilkan jumlah kerusakan untuk setiap jenis kerusakan. Diagram batang ini memberikan visualisasi yang jelas tentang sebaran jumlah kerusakan.
```
def lihat_diagram_kerusakan_bar(self, cursor=None, conn=None):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT jenis_kerusakan, SUM(jumlah_kerusakan) FROM data_kerusakan GROUP BY jenis_kerusakan")  
            jumlah_kerusakan = cursor.fetchall()  


            jenis_kerusakan = [item[0] for item in jumlah_kerusakan]
            jumlah = [item[1] for item in jumlah_kerusakan]


            plt.bar(jenis_kerusakan, jumlah)  
            plt.xlabel('Jenis Kerusakan')  
            plt.ylabel('Jumlah Kerusakan')  
            plt.title('Diagram Batang Jumlah Kerusakan Berdasarkan Jenis Kerusakan')
            plt.xticks(rotation=45)  
            plt.show()  

        except mysql.connector.Error as err:  
            print("Gagal mengambil data jumlah kerusakan:", err)
        finally:
            cursor.close()  
            conn.close() 
```

Fungsi ini  mengambil data kerusakan dari database dan mengelompokkannya berdasarkan jenis kerusakan. Namun, data ini digunakan untuk membuat diagram pie yang menampilkan persentase relatif dari setiap jenis kerusakan terhadap total jumlah kerusakan. Diagram pie sangat baik untuk menunjukkan proporsi atau perbandingan antara kategori.
```
def lihat_diagram_kerusakan_pie(self, cursor=None, conn=None):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT jenis_kerusakan, SUM(jumlah_kerusakan) FROM data_kerusakan GROUP BY jenis_kerusakan")  
            jumlah_kerusakan = cursor.fetchall()  


            jenis_kerusakan = [item[0] for item in jumlah_kerusakan]
            jumlah = [item[1] for item in jumlah_kerusakan]


            plt.pie(jumlah, labels=jenis_kerusakan, autopct='%1.1f%%')  
            plt.title('Diagram Pie Jumlah Kerusakan Berdasarkan Jenis Kerusakan')
            plt.axis('equal')   
            plt.show()  

        except mysql.connector.Error as err:  
            print("Gagal mengambil data jumlah kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi ini mengambil data kerusakan dari database dan mengelompokkannya berdasarkan jenis kerusakan. Data ini digunakan untuk membuat diagram garis yang menampilkan tren atau perubahan jumlah kerusakan dari waktu ke waktu untuk setiap jenis kerusakan. Diagram garis berguna untuk melihat pola atau tren dalam data.
```
def lihat_diagram_kerusakan_garis(self, cursor=None, conn=None):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:  
            cursor.execute("SELECT jenis_kerusakan, SUM(jumlah_kerusakan) FROM data_kerusakan GROUP BY jenis_kerusakan")  
            jumlah_kerusakan = cursor.fetchall()  


            jenis_kerusakan = [item[0] for item in jumlah_kerusakan]
            jumlah = [item[1] for item in jumlah_kerusakan]

            plt.plot(jenis_kerusakan, jumlah, marker='o', linestyle='-')
            plt.xlabel('Jenis Kerusakan')  
            plt.ylabel('Jumlah Kerusakan')  
            plt.title('Diagram Garis Jumlah Kerusakan Berdasarkan Jenis Kerusakan')
            plt.xticks(rotation=45)   
            plt.grid(True)   
            plt.show()  

        except mysql.connector.Error as err:  
            print("Gagal mengambil data jumlah kerusakan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

Fungsi buat_aduan membuat entri baru dalam tabel database aduan, merepresentasikan pengajuan aduan baru oleh seorang masyarakat
```
def buat_aduan(self, lokasi, keterangan_aduan):  
        conn = buat_koneksi()  
        cursor = conn.cursor()  
        try:
            query = "INSERT INTO aduan (ID_Masyarakat, lokasi, Tanggal, keterangan_aduan) VALUES (%s, %s, NOW(), %s)"  
            cursor.execute(query, (self.ID_Masyarakat, lokasi, keterangan_aduan))  
            conn.commit()   
            print("Aduan berhasil dibuat.")
        except mysql.connector.Error as err:  
            print("Gagal membuat aduan:", err)
        finally:
            cursor.close()  
            conn.close()  
```

- MENU

Fungsi menu_masyarakat menyajikan antarmuka menu bagi pengguna yang merupakan bagian dari masyarakan. Fungsi ini memungkinkan untuk melakukan berbagai tindakan terkait dengan melihat data kerusakan, membuat keluhan, melihat informasi akun, dan memilih untuk keluar.
```
def menu_masyarakat(masyarakat):   
    while True:  

        print("\n\033[0m\033[91mMenu Masyarakat:\033[0m")
        print("\033[0m\033[91m1.\033[0m Lihat Data Kerusakan")
        print("\033[0m\033[91m2.\033[0m Lihat Diagram Data")
        print("\033[0m\033[91m3.\033[0m Buat Aduan")
        print("\033[0m\033[91m4.\033[0m Informasi Akun")
        print("\033[0m\033[91m5.\033[0m Keluar")

        pilihan = input("Silahkan Pilih Menu(1/2/3/4/5): ")

        if pilihan == "1":
            masyarakat.lihat_data_kerusakan()  
            choice = input("\nIngin Sorting atau Searching Data Kerusakan? (Sort/Search): ").lower()
            if choice == "sort":
                masyarakat.menu_sorting_data_kerusakan()  
            elif choice == "search":
                sub_choice = input("Ingin mencari berdasarkan ID atau Lokasi? (ID/Lokasi): ").lower()
                if sub_choice == "id":
                    id_data = input("Masukkan ID data yang ingin dicari: ")
                    if id_data.isdigit():  
                        masyarakat.menu_search_data_kerusakan("ID_data", id_data)  
                    else:
                        print("ID harus berupa angka. Silakan coba lagi.")
                elif sub_choice == "lokasi":
                    lokasi = input("Masukkan nama lokasi yang ingin dicari: ")
                    masyarakat.menu_search_data_kerusakan("lokasi", lokasi)  
                else:
                    print("Pilihan tidak valid. Silakan coba lagi.")
            else:
                print("Pilihan tidak ada!")
        elif pilihan == "2":
            print("\nPilih Ingin Disajikan dalam bentuk apa:")
            print("\033[0m\033[91m1.\033[0m Diagram batang")
            print("\033[0m\033[91m2.\033[0m Diagram Pie")
            print("\033[0m\033[91m3.\033[0m Diagram Garis")
            pilihan = input("Silahkan Pilih Diagram (1/2/3): ")
            if pilihan == "1":
                masyarakat.lihat_diagram_kerusakan_bar()  
            elif pilihan == "2":
                masyarakat.lihat_diagram_kerusakan_pie()  
            elif pilihan == "3":
                masyarakat.lihat_diagram_kerusakan_garis()  
            else:
                print("Pilihan tidak ada!")
        elif pilihan == "3":
            lokasi = input("Masukkan lokasi kerusakan: ")
            keterangan_aduan = input("Masukkan deskripsi aduan: ")
            if not lokasi or not keterangan_aduan:   
                print("Lokasi dan deskripsi aduan harus diisi!. Gagal membuat aduan.")
            else:
                masyarakat.buat_aduan(lokasi, keterangan_aduan)  
        elif pilihan == "4":
            masyarakat.lihat_dan_edit_informasi_akun()  
            print("\n")
        elif pilihan == "5":
            print("Keluar dari Menu Masyarakat.")
            main()
            break
        else:
            print("Pilihan Tidak Ada!")
```

Fungsi ambil_info_admin digunakan untuk mengambil informasi admin dari database berdasarkan nama pengguna (username). Fungsi ini terhubung ke database, mengeksekusi query untuk mencari admin dengan nama yang sesuai, dan mengembalikan objek Admin yang berisi informasi admin tersebut jika ditemukan. Jika tidak ditemukan, fungsi akan mengembalikan nilai None
```
def ambil_info_admin(username):  
    conn = buat_koneksi()  
    cursor = conn.cursor()  
    try:  
        cursor.execute("SELECT * FROM admin WHERE nama_lengkap = %s", (username,))  
        admin_data = cursor.fetchone()  

        if admin_data:  
            return Admin(admin_data[0],admin_data[1], admin_data[2])  
        else:
            return None
    except mysql.connector.Error as err:  
        print("Nama admin tidak ada!", err)
    finally:
        cursor.close()  
        conn.close() 
```

Fungsi ambil_info_masyarakat bertujuan untuk mengambil informasi masyarakat dari database berdasarkan nama pengguna (username). Prosesnya mirip dengan fungsi ambil_info_admin, hanya saja query dilakukan dengan mempertimbangkan nama pengguna dalam bentuk lowercase agar lebih fleksibel. Jika informasi masyarakat ditemukan, fungsi akan mengembalikan objek Masyarakat yang berisi detailnya; jika tidak, nilai None akan dikembalikan.
```
def ambil_info_masyarakat(username):  
    conn = buat_koneksi()  
    cursor = conn.cursor()  
    try:  
        cursor.execute("SELECT * FROM masyarakat WHERE LOWER(nama_lengkap) = LOWER(%s)", (username,))  
        masyarakat = cursor.fetchone()   
        if masyarakat:  
            return Masyarakat(masyarakat[0], masyarakat[1], masyarakat[3], masyarakat[2])  
        else:
            return None
    except mysql.connector.Error as err:  
        print("Gagal mengambil info admin:", err)
    finally:
        cursor.close()  
        conn.close()  
```

Fungsi cek_login bertujuan untuk memeriksa login pengguna berdasarkan username dan password yang diberikan. Pertama, fungsi mencoba untuk mencocokkan kredensial dengan data admin. Jika cocok, "admin" akan dikembalikan. Jika tidak, fungsi akan mencoba mencocokkan kredensial dengan data masyarakat. Jika cocok, "masyarakat" akan dikembalikan. Jika tidak ada kecocokan, fungsi akan memanggil fungsi main untuk memulai kembali proses login.
```
def cek_login(username, password):

    conn = buat_koneksi()  
    cursor = conn.cursor()  
    try:  
        cursor.execute("SELECT * FROM admin WHERE nama_lengkap = %s AND ID_Admin = %s", (username, password))  
        admin = cursor.fetchone()   

        if admin:  
            return "admin"  
        else:
            cursor.execute("SELECT * FROM masyarakat WHERE nama_lengkap = %s AND ID_Masyarakat = %s", (username, password))  
            masyarakat = cursor.fetchone()   

            if masyarakat:  
                return "masyarakat"  
            else:
                main()
    except mysql.connector.Error as err:   
        print("Gagal melakukan pengecekan login:", err)
    finally:
        cursor.close()  
        conn.close()  
```

Fungsi main adalah titik masuk utama untuk aplikasi. Setelah menampilkan pesan sambutan, pengguna diminta untuk memilih antara login atau registrasi. Jika login dipilih, pengguna diminta untuk memasukkan username dan password. Fungsi cek_login digunakan untuk memeriksa apakah kredensial yang dimasukkan cocok dengan data admin atau masyarakat. Jika cocok, sesuai dengan tipe pengguna, fungsi menu_admin atau menu_masyarakat dipanggil. Jika registrasi dipilih, fungsi buat_akun_masyarakat dipanggil.
```
def main():
    print("")
    print("================WELOCOME================")
    print("     APLIKASI PENGADUAN MASYARAKAT      ")
    print("        KERUSAKAN EKOSISTEM LAUT        ")
    print("========================================")    
    print("\nSilahkan pilih menu:")
    print("\033[0m\033[91m1.\033[0m Login")
    print("\033[0m\033[91m2.\033[0m Registrasi")

    while True:
        pilihan = input("Masukkan pilihan Anda (1/2): ")
        if pilihan.lower() == "1":
            print("")
            print("==================LOGIN=================")
            username = input("Masukkan Username Anda: ")
            password = input("Masukkan Password/NIK Anda: ")


            user_type = cek_login(username, password)  
            if user_type == "admin":  
                admin = ambil_info_admin(username)  
                if admin:  
                    print("Login berhasil sebagai admin!.")
                    menu_admin(admin)  
                    break
            elif user_type == "masyarakat":  
                masyarakat = ambil_info_masyarakat(username)  
                if masyarakat:  
                    print("Login berhasil sebagai masyarakat!.")
                    menu_masyarakat(masyarakat)   
                    break
            else:
                print("Nama pengguna atau kata sandi salah. Silakan coba lagi!")

        elif pilihan.lower() == "2":
            buat_akun_masyarakat()  

        else:
            print("Pilihan tidak ada!")
```

Baris if __name__ == "__main__": memeriksa apakah skrip dieksekusi langsung sebagai program utama. Jika ya, maka fungsi main() akan dipanggil
```
if __name__ == "__main__":  
    main() 
```
