 learn-python
this is my based project for beginner
[Uploaimport os
katalog = [
    {'kode':'MK01','nama':'indomie goreng','harga':3500,'stok':50},
    {'kode':'MK02','nama':'indomie kuah','harga':3500,'stok':45},
    {'kode':'MK03','nama':'mie sedap goreng','harga':3000,'stok':40},
    {'kode':'MN01','nama':'aqua 500ml','harga':4000,'stok':100},
    {'kode':'MN02','nama':'teh botol 350ml','harga':5000,'stok':60},
    {'kode':'MN03','nama':'pocari sweet 500ml','harga':8000,'stok':30},
    {'kode':'CM01','nama':'chitato original','harga':10000,'stok':25},
    {'kode':'CM02','nama':'oreo original','harga':8500,'stok':30},
    {'kode':'CM03','nama':'biskuat','harga':2000,'stok':60},
    {'kode':'KB01','nama':'sabun lifebuoy','harga':5000,'stok':20},
    {'kode':'KB02','nama':'shampoo sunsilk sachet','harga':1500,'stok':80},
    {'kode':'KB03','nama':'pasta gigi pepsodent','harga':12000,'stok':15},
]

#untuk memunculkan produk
def katalog_produk(daftar_katalog):
    for produk in daftar_katalog:
        print(f"kode:{produk['kode']}")
        print(f"nama:{produk['nama']}")
        print(f'harga:{produk["harga"]}')
        print('')

#untuk mencari produk
def cari_produk(kode, daftar_katalog):
    for produk in daftar_katalog:
        if kode == produk['kode']:
            return produk
    return 'produk tidak ada'

#untuk menambahkan ke keranjang
def tambah_keranjang(kode, keranjang, daftar_katalog):
    produk = cari_produk(kode,daftar_katalog)
    if produk == "produk tidak ada":
        return "produk tidak ditemukan"
    elif produk["stok"] > 0:
        keranjang.append(produk)
        return 'pesanan berhasil di konfirmasi'
    else:
        return 'produk habis'

#untuk menghitung total keranjang
def total_keranjang(keranjang):
    total_harga = 0
    for produk in keranjang:
        total_harga= total_harga + produk["harga"]
    return total_harga

#untuk jumlah diskon
def jumlah_diskon(total_harga):
    if total_harga > 50000:
        total = (total_harga/ 100) * 10
        return total
    else:
        return 0
        
#urut pesanan
def urut_pesanan(nomor_pesanan):
    perulangan = nomor_pesanan % 3
    if perulangan == 0:
        return 'andi'
    elif perulangan== 1:
        return 'bima'
    else:
        return 'citra'
        
#kode pesanan
def kode_pesanan(nomor_pesanan,nama_pembeli):
    kode = nama_pembeli[:3].upper() + str(nomor_pesanan).zfill(3)
    return kode

#memasukkan ke txt
def cetak_struk(keranjang,diskon,total_harga,kasir,pembeli,kode):
    bayar = total_harga - diskon
    with open(f'struk_{kode}.txt','w') as file:
        file.write(f'TOKO SEMBAKO\n=============\nkasir : {kasir}\npembeli : {pembeli}\nkode : {kode}\n-------------\n')
        for produk in keranjang:
            file.write(f"{produk['nama']}   {produk['harga']}\n")
        file.write(f'total : {total_harga}\ndiskon : {diskon}\nharga : {bayar}\n')


#memauskkan ke csv
def cetak_csv(kode,pembeli,total,diskon,kasir):
    if not os.path.exists('laporan_harian.csv'):
        with open('laporan_harian.csv', 'a', newline='', encoding='utf-8-sig') as file:
            file.write('kode;pembeli;total;diskon;kasir\n')
    with open('laporan_harian.csv', 'a', newline='', encoding='utf-8-sig') as file:
            file.write(f'{kode};{pembeli};{total};{diskon};{kasir}\n')

keranjang = []
nomor_pesanan = 0

while True:
    print('=== MENU UTAMA ===')
    print('1.lihat katalog')
    print('2.mulai transaksi')
    print('3.keluar')
    pilih = input('pilih nomor: ')
    if pilih == '1':
        katalog_produk(katalog)
    elif pilih == '3':
        break
    elif pilih == '2':
        nama = input('masukkan nama anda: ')
        katalog_produk(katalog)
        while True:
            cari = input('masukkan kode produk (ketik "selesai" jika sudah):')
            if cari == 'selesai':
                break
            print(tambah_keranjang(cari,keranjang,katalog))
        nomor_pesanan = nomor_pesanan + 1
        buat_kode = kode_pesanan(nomor_pesanan, nama)
        kasir_urut = urut_pesanan(nomor_pesanan)
        total = total_keranjang(keranjang)
        diskon = jumlah_diskon(total)
        struk = cetak_struk(keranjang,diskon,total,kasir_urut,nama,buat_kode)
        csv = cetak_csv(buat_kode,nama,total,diskon,kasir_urut)
        print('terima kasih transaksinya!')
        keranjang = []
    else:
        print('nomor menu tidak tersedia! masukkan dengan benar!')
        continue
ding kasir.py…]()
