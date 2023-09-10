### Latar Belakang

Andi adalah seorang pemilik supermarket besar di salah satu kota di Indonesia memiliki rencana untuk melakukan perbaikan proses bisnis, yaitu membuat self-service di supermarket sehingga customer bisa langsung memasukkan item, jumlah item, dan harga item yang dibeli.

### Requirements:
1. Customer membuat ID transaksi customer 
trnsct_123=Transaction()

2. Customer memasukkan nama item, jumlah item, dan harga barang
add_item([<nama_item>,jumlah_item>,<harga_per_item>])

3. Jika ada kesalahan dalam memasukkan nama item/jumlah item atau harga item tetapi tidak ingin menghapus , customer bisa melakukan update
a. Update Nama Item
update_item_name(<nama_item>,<update_nama_item>)
b. Update Jumlah Item
update_item_qty(<nama_item>,<update_jumlah_item>)
c. Update Harga Item
update_item_price(<nama_item>,<update_harga_item>)

4. Jika batal membeli item belanjaan, Customer bisa melakukan
a. Menghapus salah satu item dari nama item dengan method delete_item(<nama_item>). Ketika menghapus salah satu item, maka jumlah item dan harga per item pada baris/list tersebut akan ikut terhapus
b. Langsung menghapus semua transaksi atau reset transaksi dengan mehtod reset_transaction()

5.Customer sudah selesai berbelanja online, tetapi masih ragu apakah harga barang dan nama barang yang diinpu sudah benar. Bisa saja customer melakukan kesalahan input harga barang tetapi lupa untuk input nama barangnya. Bisa menggunkan input check_order()
a. Mengeluarkan pesan "Pemesanan sudah benar" jika tidak ada kesalahan input
b. Mengeluarkan pesan "Terdapat kesalahan input data" jika terjadi kesalahan input
c. Mengeluarkan output transaksi atau pemesanan apa saja yang sudah dibeli

6.Setelah pengecekan, customer bisa menghitung total belanja yang sudah dibeli. Andi dapat menggunakan method total_price(). Pada supermarket terdapat ketentuan
a. Jika total belanja Andi di atas Rp 200,000 akan mendapatkan diskon 5%
b. Jika total belanja Andi di atas Rp 300,000 akan mendapatkan diskon 8%
c. Jika total belanja Andi di atas Rp 500,000 akan mendapatkan diskon 10%



![flowchartcashier.jpg](attachment:flowchartcashier.jpg)

## Function Explanation

1. Kelas Transaction:
    - Ini adalah kelas utama yang mewakili transaksi pelanggan di supermarket.
2. __init__(self):
    - Konstruktor kelas ini menginisialisasi sebuah daftar kosong bernama items untuk menyimpan barang belanjaan pelanggan.
3. add_item(self, nama_item, jumlah_item, harga_per_item):
    - Menambahkan barang baru ke dalam transaksi.
    - nama_item: Nama barang.
    - jumlah_item: Jumlah barang.
    - harga_per_item: Harga per barang.
    - Memvalidasi data masukan, memastikan nilainya berupa angka dan positif.
    - Menghitung total harga untuk barang tersebut dan menyimpannya dalam transaksi.
4. update_item_name(self, old_name, name_update):
    - Memperbarui nama barang yang sudah ada dalam transaksi.
5. update_item_qty(self, nama_item, qty_update):
    - Memperbarui jumlah barang yang sudah ada dalam transaksi.
6. update_item_price(self, nama_item, price_update):
    - Memperbarui harga per barang yang sudah ada dalam transaksi.
7. reset_transaction(self):
    - Menghapus seluruh transaksi, efektif memulai transaksi baru.
8. delete_item(self, nama_item):
    - Menghapus barang tertentu dari transaksi.
9. check_order(self):
    - Memeriksa seluruh transaksi untuk kesalahan masukan, memastikan bahwa semua data barang lengkap.
    - Menampilkan tabel barang dalam format yang mudah dibaca dengan menggunakan library 'tabulate'.
    - Mengembalikan pesan yang mengindikasikan apakah pesanan sudah benar atau terdapat kesalahan masukan.
10. total_price(self):
    - Menghitung total harga dari seluruh transaksi.
    - Mengaplikasikan diskon berdasarkan jumlah pembelian total.
    - Menampilkan ringkasan barang yang dibeli dan total harga, termasuk diskon.


```python
from tabulate import tabulate

class Transaction:
    def __init__(self):
        self.items = []

    def add_item(self, nama_item, jumlah_item, harga_per_item):
        if not isinstance(jumlah_item, (int, float)) or not isinstance(harga_per_item, (int, float)):
            print('Item quantity and price must be numeric values.')
            return
        if jumlah_item <= 0 or harga_per_item <= 0 :
            print('Item quantity and price must be positive numbers.')
            return
        item = {
            'nama_item': nama_item,
            'jumlah_item': jumlah_item,
            'harga_per_item': harga_per_item,
            'total_harga': jumlah_item*harga_per_item,
        }
        self.items.append(item)


    def update_item_name(self, old_name, name_update):
        for item in self.items:
            if item['nama_item'] == old_name:
                item['nama_item'] = name_update
    def update_item_qty(self, nama_item, qty_update):
        for item in self.items:
            if item['nama_item'] == nama_item:
                item['jumlah_item'] = qty_update

    def update_item_price(self, nama_item,price_update):
        for item in self.items:
            if item['nama_item'] == nama_item:
                item['harga_per_item'] = price_update
    def reset_transaction(self):
        self.items =[]
        print('Semua Item Berhasil di Delete')
    def delete_item(self, nama_item):
        self.items = [item for item in self.items if item['nama_item'] != nama_item]
        print (self.items)
    def check_order(self):  
        headers = ['Item', 'Jumlah Item', 'Harga/item', 'Harga total']
        item_data = ([item['nama_item'], item['jumlah_item'],item['harga_per_item'],item['total_harga']] for item in self.items)
        print(tabulate(item_data, headers=headers, tablefmt=''))
        for item in self.items:
            if not all (item.values()):
                return 'Terdapat kesalahan input data'  
        return 'Pemesanan sudah benar'

    def total_price(self):      
        for item in self.items:
            total_all_item=sum(item['total_harga']for item in self.items)
        disc=0
        if total_all_item>500000:
            disc=0.1
        elif total_all_item>300000:
            disc=0.08
        elif total_all_item>200000:
            disc=0.05
        total_paid=(1-disc)*total_all_item
        item_summary = {}
        for item in self.items:
            item_summary [item['nama_item']] = [item['jumlah_item'],item['harga_per_item']]
        print(f"Item yang dibeli adalah: {item_summary}")
        print(f"Total Price: IDR {'{:,.0f}'.format(total_paid)}")
            
 
    

    
```


```python
trnsct_123 = Transaction()
```


```python
#Test 1
trnsct_123.add_item('Ayam Goreng',2,20000)
trnsct_123.add_item('Pasta Gigi',3,15000)
```


```python
#Test 2
trnsct_123.delete_item('Pasta Gigi')
```

    [{'nama_item': 'Ayam Goreng', 'jumlah_item': 2, 'harga_per_item': 20000, 'total_harga': 40000}]



```python
#Test 3
trnsct_123.reset_transaction()
```

    Semua Item Berhasil di Delete



```python
#Test 4
trnsct_123.add_item('Ayam Goreng',2,20000)
trnsct_123.add_item('Pasta Gigi',3,15000)
trnsct_123.add_item('Mainan Mobil',1,200000)
trnsct_123.add_item('Mi Instan',5,3000)
trnsct_123.check_order()


```

    Item            Jumlah Item    Harga/item    Harga total
    ------------  -------------  ------------  -------------
    Ayam Goreng               2         20000          40000
    Pasta Gigi                3         15000          45000
    Mainan Mobil              1        200000         200000
    Mi Instan                 5          3000          15000





    'Pemesanan sudah benar'




```python
#Test 5
trnsct_123.total_price()
```

    Item yang dibeli adalah: {'Ayam Goreng': [2, 20000], 'Pasta Gigi': [3, 15000], 'Mainan Mobil': [1, 200000], 'Mi Instan': [5, 3000]}
    Total Price: IDR 285,000

# pacmann-finalproject
# pacmann
# pacmann
