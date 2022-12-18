import os


# Method
# operasi gauss saja
def gauss(matriks, baris, kolom, i=0, z=0):
    if i < baris:
        if z < kolom - 1:
            if matriks[i][z] == 0:
                for j in range(i + 1, baris):
                    if matriks[j][z] != 0:
                        # perubahan posisi pada setiap kolom
                        for k in range(kolom):
                            matriks[i][k], matriks[j][k] = matriks[j][k], matriks[i][k]

                        print(f"\nR{i+1} =><= R{j+1}\n")

                        tampilkanMatriks(matriks, baris, kolom)
                        os.system("pause")

                        break
            if matriks[i][z] != 0:
                if matriks[i][z] != 1:
                    sementara = matriks[i][z]

                    for j in range(z, kolom):
                        matriks[i][j] /= sementara

                    print(f"\nR{i+1} / {sementara}\n")

                    tampilkanMatriks(matriks, baris, kolom)
                    os.system("pause")

                for j in range(i + 1, baris):
                    if matriks[j][z] != 0:
                        sementara = matriks[j][z] / matriks[i][z]

                        for k in range(z, kolom):
                            matriks[j][k] = matriks[j][k] - \
                                (sementara * matriks[i][k])

                        print(f"\nR{j+1}", end="")
                        if sementara > 0:
                            print(" - ", end="")
                        else:
                            print(" + ", end="")
                            sementara *= -1
                        print(f"{sementara}R{i+1}\n")

                        tampilkanMatriks(matriks, baris, kolom)
                        os.system("pause")
                i += 1
                gauss(matriks, baris, kolom, i, z)
            else:
                z += 1
                gauss(matriks, baris, kolom, i, z)

# operasi lanjutan dari gauss


def gaussLanjutan(matriks, baris, kolom):
    solusi = cekSolusi(matriks, baris, kolom)
    if solusi != "Tidak Ada Solusi":
        for i in range(baris - 1, -1, -1):
            for j in range(kolom - 1):
                if matriks[i][j] != 0:
                    for k in range(i - 1, -1, -1):
                        if matriks[k][j] != 0:
                            sementara = matriks[k][j] / matriks[i][j]

                            for l in range(j, kolom):
                                matriks[k][l] -= (sementara * matriks[i][l])
                            if sementara < 0:
                                sementara *= -1
                    break
    # kembali mengecek jenis solusi yang didapatkan dari proses akhir
    solusi = cekSolusi(matriks, baris, kolom)
    print("\n="+solusi.center(48)+"=\n")

    if solusi != "Tidak Ada Solusi":
        tampilkanSPL(matriks, baris, kolom)

# proses jordan setelah proses Gauss


def jordan(matriks, baris, kolom):
    solusi = cekSolusi(matriks, baris, kolom)
    if solusi != "Tidak Ada Solusi":
        for i in range(baris - 1, -1, -1):
            for j in range(kolom - 1):
                if matriks[i][j] != 0:
                    for k in range(i - 1, -1, -1):
                        if matriks[k][j] != 0:
                            sementara = matriks[k][j] / matriks[i][j]
                            for l in range(j, kolom):
                                matriks[k][l] -= (sementara * matriks[i][l])
                            print("="*50)
                            print(f"R{k+1}", end="")
                            if sementara > 0:
                                print(" - ", end="")
                            else:
                                print(" + ", end="")
                                sementara *= -1
                            print(f"{sementara}R{i+1}")
                            tampilkanMatriks(matriks, baris, kolom)
                            os.system("pause")
                    break
    # kembali mengecek jenis solusi setelah melakukan proses eliminasi jordan
    solusi = cekSolusi(matriks, baris, kolom)
    print("\n="+solusi.center(48)+"=\n")

    if solusi != "Tidak Ada Solusi":
        tampilkanSPL(matriks, baris, kolom)

# method untuk mengecek jenis solusi


def cekSolusi(matriks, baris, kolom):
    # proses dibawah ini secara garis besar hanya mengecek baris terbawah yang akan mengecek kolom isi dan juga kolom hasil.
    # jika kolom isi terdapat satu nilai yang bukan nol maka solusi unik
    # jika kolom isi terdapat lebih dari satu nilai yang bukan nol, maka solusi banyak
    # jika kolom isi tidak terdapat nilai yang bukan nol akan tetapi kolom hasil bernilai bukan nol, maka tidak ada solusi atas matriks tersebut
    solusi = "Solusi Unik"
    for i in range(baris - 1, -1, -1):
        cek = 0
        for j in range(kolom - 1):
            if matriks[i][j] != 0:
                cek += 1

        if cek == 1:
            solusi = "Solusi Unik"
        elif cek != 0:
            solusi = "Banyak Solusi"
            break
        elif cek == 0:
            if matriks[i][kolom - 1] != 0:
                solusi = "Tidak Ada Solusi"
                break

    return solusi

# Menampilkan SPL


def tampilkanSPL(matriks, baris, kolom):
    for i in range(baris):
        awalBaris = True
        for j in range(kolom):
            if j < kolom - 1:
                if matriks[i][j] != 0:
                    matriks[i][j] = round(matriks[i][j], 3)
                    if awalBaris == False:
                        if matriks[i][j] > 0:
                            print(" +", end="")
                        else:
                            print(" ", end="")
                    if matriks[i][j] == 1:
                        print(f" x{j}", end="")
                    else:
                        print(f" {matriks[i][j]}x{j}", end="")
                    awalBaris = False
            else:
                if awalBaris == False:
                    print(" =", matriks[i][j])
    print()

# menampilkan matriks


def tampilkanMatriks(matriks, baris, kolom):
    for i in range(baris):
        print(" [", end="")
        for j in range(kolom):
            if matriks[i][j] != 0:
                matriks[i][j] = round(matriks[i][j], 3)
            if j == kolom - 1:
                print(" |", end="")
            print(f" {matriks[i][j]}", end="")
        print(" ]")
    print()


# Main Program
os.system("cls")

while True:
    print("Pemilihan Rumus SPL Yang akan Di eksekusi : ")
    print("1. Gauss")
    print("2. Gauss-Jordan")
    print("3. Keluar")

    pilihan = input("Pilihan (1, 2 atau 3) : ")

    if pilihan == "1" or pilihan == "2":
        baris = int(input("\nBaris : "))
        kolom = int(input("Kolom : "))

        kolom += 1
        # tidak terdapat variabel yang diinisialisasi pada program ini karena telah default untuk x0,x1,x2 ....
        matriks = [[0 for i in range(kolom)] for j in range(baris)]

        for i in range(baris):
            for j in range(kolom):
                if j < kolom-1:
                    print(f"INPUTKAN PERKOLOM x{j} : ", end="")
                else:
                    print(f" Hasil {i+1} : ", end="")
                matriks[i][j] = float(input())
            print()

        os.system("cls")

        print("="+"Sistem Persamaan Linear".center(48)+"=\n")
        tampilkanSPL(matriks, baris, kolom)

        print("\n="+"Matriks Augmented".center(48)+"=\n")
        tampilkanMatriks(matriks, baris, kolom)

        os.system("pause")

        if pilihan == "1":
            gauss(matriks, baris, kolom)
            gaussLanjutan(matriks, baris, kolom)

        elif pilihan == "2":
            gauss(matriks, baris, kolom)
            jordan(matriks, baris, kolom)

        os.system("pause")
        os.system("cls")

    elif pilihan == "3":
        break
