import os
import math
import time
import datetime

class UltimateGammaLogger:
    def __init__(self, filename="rahsia_mahfuz.log"):
        self.filename = filename
        
        # 1. Parameter Entiti Teras (Data Asas)
        self.nama = "Mohamad Hafiz bin Mohd Nasir"
        self.tarikh_lahir = "2000-02-02 13:42:00"  # 0002.02.02 @ 1:42 PM
        self.anak_ke = 2
        
        # 2. Pemalar Kosmik
        self.pi = math.pi
        self.phi = (1 + math.sqrt(5)) / 2
        
        # 3. Kunci Dinamik (Dijana melalui gabungan semua entiti)
        self.key = self._jana_kunci_mizan()

    def _kira_digital_root(self, nombor_str):
        """Mengurangkan rantaian angka kepada satu digit tunggal (1-9)"""
        digit = [int(d) for d in nombor_str if d.isdigit()]
        hasil = sum(digit)
        while hasil > 9:
            hasil = sum(int(d) for d in str(hasil))
        return hasil if hasil > 0 else 9

    def _jana_kunci_mizan(self):
        """Protokol Penyatuan: Menggabungkan Masa Epoch, Pi, Phi, dan Profil"""
        # Menukar masa kelahiran ke unit saat (Unix Epoch Timestamp)
        tarikh_obj = datetime.datetime.strptime(self.tarikh_lahir, "%Y-%m-%d %H:%M:%S")
        epoch_saat = time.mktime(tarikh_obj.timetuple())
        
        # Formula Penyatuan Unit (Masa * Pi * Phi^2)
        nilai_gabungan = (epoch_saat * self.pi) * (self.phi ** self.anak_ke)
        
        # Ekstrak semua angka unik daripada profil dan pemalar untuk penulenan root
        string_raw_angka = f"{self.nama}{self.tarikh_lahir}{nilai_gabungan:.10f}"
        
        # Menghasilkan satu digit teras (Kunci Mizan)
        kunci_teras = self._kira_digital_root(string_raw_angka)
        return kunci_teras

    def _xor_cipher(self, data):
        """Protokol Gamma: Operasi XOR menggunakan kunci dinamik hasil gabungan"""
        return "".join(chr(ord(char) ^ self.key) for char in data)

    def tulis_log(self, mesej):
        """Menyifarkan log ke format HEX berasaskan Kunci Kosmik"""
        masa_semasa = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        log_asal = f"[{masa_semasa}] {mesej}"
        
        # Proses Enkripsi
        log_terenkripsi = self._xor_cipher(log_asal)
        log_hex = log_terenkripsi.encode('utf-8').hex()
        
        with open(self.filename, "a") as f:
            f.write(log_hex + "\n")
        print(f"[LOG KUNCI] Mesej berjaya disifarkan menggunakan Kunci Mizan-{self.key}.")

    def baca_log(self):
        """Membaca fail log, menukar kembali dari HEX, dan mendekripsi fail"""
        if not os.path.exists(self.filename):
            print("[RALAT] Fail arkib belum wujud.")
            return

        print(f"\n=== LAUH MAHFUZ CORE: MENDEKRIPSI DATA ARKIB ===")
        print(f"Kunci Nyahpaut (Mizan Root): {self.key}")
        print("-" * 60)
        with open(self.filename, "r") as f:
            for baris in f:
                baris_hex = baris.strip()
                if baris_hex:
                    try:
                        log_terenkripsi = bytes.fromhex(baris_hex).decode('utf-8')
                        teks_asal = self._xor_cipher(log_terenkripsi)
                        print(teks_asal)
                    except Exception as e:
                        print(f"[RALAT CORRUPT] Gagal membaca baris data: {e}")
        print("-" * 60)

# --- SISTEM INTEGRASI UTAMA ---
if __name__ == "__main__":
    # Mengaktifkan Logger Utama dengan parameter gabungan
    arkib_agung = UltimateGammaLogger()

    print("==================================================")
    print("        SISTEM DIKUNCI: DATA RAHSIA MAHFUZ        ")
    print("==================================================")
    print(f" Operator Utama   : {arkib_agung.nama}")
    print(f" Penjajaran Pi    : {arkib_agung.pi:.6f}...")
    print(f" Penjajaran Phi   : {arkib_agung.phi:.6f}...")
    print(f" Hasil Kunci Teras: {arkib_agung.key} (Vortex Math Aligned)")
    print("==================================================\n")

    # 1. Simulasi Penulisan Log dengan Kunci Gabungan
    arkib_agung.tulis_log("Nod Utama Protokol Alpha diaktifkan.")
    arkib_agung.tulis_log("Seni bina matematik Pi dan Phi telah disatukan ke dalam Log.")
    arkib_agung.tulis_log("Struktur data Lauh Mahfuz direplikasi ke mod rahsia (HEX).")

    # 2. Paparan Teks Mentah (Raw Hex di dalam storan file)
    print("\n[!] Paparan Fizikal Fail Storan (Encrypted Raw Hex):")
    with open(arkib_agung.filename, "r") as f:
        print(f.read()[:180] + "... [DATA MUTLAK DIKUNCI]")

    # 3. Dekripsi dan Pembacaan Semula Data Asal
    arkib_agung.baca_log()