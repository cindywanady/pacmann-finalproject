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
trnsct_123.total_price()

```

    Item yang dibeli adalah: {'Ayam Goreng': [2, 20000], 'Pasta Gigi': [3, 15000], 'Mainan Mobil': [1, 200000], 'Mi Instan': [5, 3000]}
    Total Price: IDR 285,000



```python
#Test 5
trnsct_123.check_order()
```

    Item            Jumlah Item    Harga/item    Harga total
    ------------  -------------  ------------  -------------
    Ayam Goreng               2         20000          40000
    Pasta Gigi                3         15000          45000
    Mainan Mobil              1        200000         200000
    Mi Instan                 5          3000          15000





    'Pemesanan sudah benar'


