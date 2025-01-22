# project-UAS
# NAMA : FAHRI MUHAMAD FATIH SANJAYA
# NIM : 352310955
# KELAS : IE.23.C.13
# Untuk link youtube penjelasan 
# https://youtu.be/86wxT-lrdj0

import os
from prettytable import PrettyTable

# Class untuk menyimpan data mahasiswa
class Mahasiswa:
    def __init__(self, nim, nama, nilai):
        self.nim = nim
        self.nama = nama
        self.nilai = nilai

# Class untuk menangani tampilan
class View:
    @staticmethod
    def tampilkan_data(mahasiswa_list):
        if not mahasiswa_list:
            print("Tidak ada data mahasiswa.")
            return

        tabel = PrettyTable()
        tabel.field_names = ["No", "NIM", "Nama", "Nilai"]
        for i, mahasiswa in enumerate(mahasiswa_list, start=1):
            tabel.add_row([i, mahasiswa.nim, mahasiswa.nama, mahasiswa.nilai])
        print(tabel)

    @staticmethod
    def tampilkan_pesan(pesan):
        print(pesan)

# Class untuk memproses data
class Process:
    def __init__(self):
        self.mahasiswa_list = []

    def tambah_data(self, nim, nama, nilai):
        try:
            if not nim.isdigit():
                raise ValueError("NIM harus berupa angka.")
            if not nama.isalpha():
                raise ValueError("Nama hanya boleh berisi huruf.")
            if not (0 <= float(nilai) <= 100):
                raise ValueError("Nilai harus antara 0 - 100.")

            mahasiswa = Mahasiswa(nim, nama, float(nilai))
            self.mahasiswa_list.append(mahasiswa)
            return True, "Data berhasil ditambahkan."
        except ValueError as e:
            return False, str(e)

    def hapus_data(self, index):
        try:
            index = int(index) - 1
            if index < 0 or index >= len(self.mahasiswa_list):
                raise IndexError("Index tidak valid.")

            self.mahasiswa_list.pop(index)
            return True, "Data berhasil dihapus."
        except (ValueError, IndexError) as e:
            return False, str(e)

# Main program
def main():
    process = Process()
    view = View()

    while True:
        os.system('cls' if os.name == 'nt' else 'clear')
        print("=== Pengelolaan Data Mahasiswa ===")
        print("1. Tambah Data Mahasiswa")
        print("2. Hapus Data Mahasiswa")
        print("3. Tampilkan Data Mahasiswa")
        print("4. Keluar")

        pilihan = input("Pilih menu: ")

        if pilihan == "1":
            nim = input("Masukkan NIM: ")
            nama = input("Masukkan Nama: ")
            nilai = input("Masukkan Nilai: ")

            sukses, pesan = process.tambah_data(nim, nama, nilai)
            view.tampilkan_pesan(pesan)

        elif pilihan == "2":
            view.tampilkan_data(process.mahasiswa_list)
            index = input("Masukkan nomor data yang ingin dihapus: ")

            sukses, pesan = process.hapus_data(index)
            view.tampilkan_pesan(pesan)

        elif pilihan == "3":
            view.tampilkan_data(process.mahasiswa_list)
            input("Tekan Enter untuk kembali ke menu.")

        elif pilihan == "4":
            print("Program selesai.")
            break

        else:
            view.tampilkan_pesan("Pilihan tidak valid.")

if __name__ == "__main__":
    main()
