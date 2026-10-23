# <h1 align="center">Modul 5 SINGLY LINKED LIST (BAGIAN KEDUA) </h1>
<p align="center">Gilang Haryo Yudianto- 109082500021</p>

## Dasar Teori

### A. Searching pada Singly Linked List

Searching merupakan operasi dasar pada list yang dilakukan dengan mencari node tertentu. Proses ini berjalan dengan mengunjungi setiap node mulai dari elemen pertama, lalu berhenti setelah node yang dicari ditemukan. Karena penelusuran pada singly linked list hanya dapat dilakukan satu arah (maju), pencarian dilakukan secara sekuensial dari first hingga Nil. Jika sampai akhir list node yang dicari tidak ditemukan, maka hasil pencarian dinyatakan tidak ada.

### B. Jenis Pencarian Node

Pencarian node dapat dilakukan berdasarkan beberapa kriteria sesuai kebutuhan program.

#### 1. Pencarian Berdasarkan Data
Menelusuri list untuk mencari node yang isi datanya sesuai dengan data yang dicari, misalnya berdasarkan nama. Pencarian berhenti pada node pertama yang cocok, dan posisi node tersebut di dalam list dapat ditampilkan.

#### 2. Pencarian Berdasarkan Alamat
Menelusuri list untuk mencari node yang alamat pointernya sama dengan alamat yang dicari. Pencarian ini berguna untuk memastikan apakah suatu node benar-benar merupakan anggota dari list tersebut.

#### 3. Pencarian Berdasarkan Rentang Data (Range)
Menelusuri seluruh node dan menampilkan semua node yang nilai datanya berada di antara batas awal dan batas akhir, misalnya rentang harga. Berbeda dengan dua jenis sebelumnya, penelusuran tidak berhenti pada node pertama yang cocok karena bisa terdapat lebih dari satu node yang memenuhi syarat.

### C. Hubungan Searching dengan Operasi Lain

Dengan adanya operasi searching, operasi lain seperti insert after, delete after, dan update menjadi lebih mudah dilakukan. Hal ini karena ketiga operasi tersebut membutuhkan alamat node tertentu sebagai acuan, dan alamat tersebut dapat diperoleh melalui proses pencarian terlebih dahulu.

### D. Operasi Update

Update adalah operasi dasar untuk mengubah data yang ada di dalam node pada list. Proses update biasanya diawali dengan pencarian terhadap node yang akan diubah. Terdapat tiga bentuk update berdasarkan posisi node yang diubah.

#### 1. Update First
Mengubah data pada node pertama list, yaitu node yang ditunjuk oleh first.

#### 2. Update Last
Mengubah data pada node terakhir list. Node terakhir dicari dengan menelusuri list hingga next bernilai Nil, kemudian datanya diperbarui.

#### 3. Update After
Mengubah data pada node yang berada tepat setelah node tertentu yang ditunjuk. Operasi ini hanya dapat dilakukan jika node acuan valid dan memiliki successor.

### E. Primitif dan Modularisasi ADT List

Semua operasi dasar pada list, termasuk searching dan update, disebut operasi primitif, yaitu bagian dari ADT singly linked list yang disimpan dalam dua file. File header (\*.h) menyimpan tipe data serta prototipe primitif atau fungsi, sedangkan file body (\*.cpp) berisi realisasi dari prototipe tersebut. Untuk mengakses seluruh primitif, file \*.h cukup di-include pada program yang menggunakannya.

## Guided 

### 1. File listbuah.h

```C++
#ifndef LISTBUAH_H
#define LISTBUAH_H
#define Nil NULL

#include<iostream>
using namespace std;

struct buah{
    string nama;
    int jumlah; 
    float harga;
};

typedef buah dataBuah; 

typedef struct node *address; 

struct node{ 
    dataBuah isidata;
    address next;
};

struct linkedlist{ 
    address first;
};

bool isEmpty(linkedlist List);
void createList(linkedlist &List);
address alokasi(string nama, int jumlah, float harga);
void dealokasi(address &node);
void printList(linkedlist List);
void insertFirst(linkedlist &List, address nodeBaru);
void insertAfter(linkedlist &List, address nodeBaru, address prev);
void insertLast(linkedlist &List, address nodeBaru);
void delFirst(linkedlist &List);
void delLast(linkedlist &List);
void delAfter(linkedlist &List, address nodeHapus, address nodePrev);
int nbList(linkedlist List);
void deleteList(linkedlist &List);

void updateFirst(linkedlist List);
void updateLast(linkedlist List);
void updateAfter(linkedlist List, address prev);

void FindNodeByData(linkedlist list, string data);
void FindNodeByAddress(linkedlist list, address node);
void FindNodeByRange(linkedlist list, float hargaAwal, float HargaAkhir);

#endif
```
Program ini mendemonstrasikan penerapan file header untuk mendeklarasikan Abstract Data Type (ADT) singly linked list yang mengelola data buah. Struktur datanya dibangun menggunakan struct buah beranggotakan field nama (string), jumlah (int), dan harga (float) dengan alias dataBuah, yang kemudian diintegrasikan ke dalam elemen list berbentuk struct node bersama penunjuk next bertipe address, serta dikendalikan oleh struct linkedlist melalui field utama first. Untuk mendukung manipulasi list, program ini menyediakan prototipe fungsi komprehensif yang dikelompokkan menjadi inisialisasi dan pengecekan (createList, isEmpty), manajemen memori (alokasi, dealokasi), utilitas penampilan dan penghitungan (printList, nbList), penyisipan node (insertFirst, insertAfter, insertLast), penghapusan elemen (delFirst, delLast, delAfter, deleteList), pembaruan informasi (updateFirst, updateLast, updateAfter), hingga fitur pencarian node (FindNodeByData, FindNodeByAddress, FindNodeByRange) berdasarkan kriteria nama, alamat, maupun rentang harga buah.

### 2. File listbuah.cpp

```C++
#include "listBuah.h"
#include <iostream>
using namespace std;

//fungsi untuk cek apakah list kosong atau tidak
bool isEmpty(linkedlist List) {
    if(List.first == Nil){
        return true; 
    } else {
        return false;
    }
}

//pembuatan linked list kosong
void createList(linkedlist &List) {
    List.first = Nil;
}

//pembuatan node baru dengan menerapkan manajemen memori
address alokasi(string nama, int jumlah, float harga) { 
    address nodeBaru = new node; 
    nodeBaru->isidata.nama = nama;
    nodeBaru->isidata.jumlah = jumlah; 
    nodeBaru->isidata.harga = harga;
    nodeBaru->next = Nil;
    return nodeBaru;
}

//penghapusan node dengan menerapkan manajemen memori
void dealokasi(address &node) {
    node->next = Nil;
    delete node;
}

//prosedur-prosedur untuk insert / menambahkan node baru kedalam list
void insertFirst(linkedlist &List, address nodeBaru) {
    nodeBaru->next = List.first; 
    List.first = nodeBaru;
}

void insertAfter(linkedlist &List, address nodeBaru, address Prev) {
    if (Prev != Nil) {
        nodeBaru->next = Prev->next;
        Prev->next = nodeBaru;
    } else {
        cout << "Node sebelumnya tidak valid!" << endl;
    }
}

void insertLast(linkedlist &List, address nodeBaru) {
    if (isEmpty(List)) {
        List.first = nodeBaru;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        nodeBantu->next = nodeBaru;
    }
}

//prosedur-prosedur untuk delete / menghapus node yang ada didalam list
void delFirst(linkedlist &List){
    address nodeHapus;
    if (isEmpty(List) == false) {
        nodeHapus = List.first;
        List.first = List.first->next;
        nodeHapus->next = Nil;
        dealokasi(nodeHapus);
        cout << "Node pertama berhasil terhapus!" << endl;
    } else {
        cout << "List kosong!" << endl;
    }
}

void delLast(linkedlist &List){
    address nodeHapus, nodePrev;
    if(isEmpty(List) == false){
        nodeHapus = List.first;
        if(nodeHapus->next == Nil){
            List.first->next = Nil;
            dealokasi(nodeHapus);
        } else { 
            while(nodeHapus->next != Nil){
                nodePrev = nodeHapus; 
                nodeHapus = nodeHapus->next;
            }
            nodePrev->next = Nil; 
            dealokasi(nodeHapus);
        }
        cout << "Node terakhir berhasil terhapus!" << endl;
    } else {
        cout << "list kosong" << endl;
    }
}

void delAfter(linkedlist &List, address nodeHapus, address nodePrev){
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else { //jika list tidak kosong
        if (nodePrev != Nil && nodePrev->next != Nil) { 
            nodeHapus = nodePrev->next;       
            nodePrev->next = nodeHapus->next;  
            nodeHapus->next = Nil;         
            dealokasi(nodeHapus);
            cout << "Node setelah node " << nodePrev->isidata.nama << " berhasil terhapus!" << endl;
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

//prosedur untuk menampilkan isi list
void printList(linkedlist List) {
    if (isEmpty(List)) {
        cout << "List kosong." << endl;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu != Nil) { 
            cout << "Nama Buah : " << nodeBantu->isidata.nama << ", Jumlah : " << nodeBantu->isidata.jumlah << ", Harga : " << nodeBantu->isidata.harga << endl;
            nodeBantu = nodeBantu->next;
        }
    }
}

//function untuk menampilkan jumlah node didalam list
int nbList(linkedlist List) {
    int count = 0;
    address nodeBantu = List.first;
    while (nodeBantu != Nil) {
        count++;
        nodeBantu = nodeBantu->next; 
    }
    return count;
}

//prosedur untuk menghapus list (menghapus semua node didalam list)
void deleteList(linkedlist &List){
    address nodeBantu, nodeHapus;
    nodeBantu = List.first;
    while(nodeBantu != Nil){
        nodeHapus = nodeBantu;
        nodeBantu = nodeBantu->next;
        dealokasi(nodeHapus); 
    }
    List.first = Nil; 
    cout << "List berhasil terhapus!" << endl;
}

/*----- MATERI PERTEMUAN 5 - SINGLY LINKED LIST (BAGIAN KEDUA) - UPDATE -----*/
//prosedur-prosedur untuk melakukan update data node
void updateFirst(linkedlist List){
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else {
        cout << "Masukkan update data node pertama : " << endl;
        cout << "Nama buah : ";
        cin >> List.first->isidata.nama;
        cout << "Jumlah : ";
        cin >> List.first->isidata.jumlah;
        cout << "Harga : ";
        cin >> List.first->isidata.harga;
        cout << "Data Berhasil Diupdate!" << endl;
        cout << endl;
    }
}

void updateLast(linkedlist List){
    if (isEmpty(List) == true) {
        cout << "List Kosong!" << endl;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        cout << "masukkan update data node terakhir : " << endl;
        cout << "Nama buah : ";
        cin >> nodeBantu->isidata.nama;
        cout << "Jumlah : ";
        cin >> nodeBantu->isidata.jumlah;
        cout << "Harga : ";
        cin >> nodeBantu->isidata.harga;
        cout << "Data Berhasil Diupdate!" << endl;
        cout << endl;
    }
}

void updateAfter(linkedlist List, address nodePrev){
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else {
        if (nodePrev != Nil && nodePrev->next != Nil){
            address nodeBantu = nodePrev->next;
            cout << "masukkan update data node setelah node " << nodePrev->isidata.nama << " : " << endl;
            cout << "Nama buah : ";
            cin >> nodeBantu->isidata.nama;
            cout << "Jumlah : ";
            cin >> nodeBantu->isidata.jumlah;
            cout << "Harga : ";
            cin >> nodeBantu->isidata.harga;
            cout << "Data Berhasil Diupdate!" << endl;
            cout << endl;
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

/*----- MATERI PERTEMUAN 5 - SINGLY LINKED LIST (BAGIAN KEDUA) - SEARCHING -----*/
//prosedur-prosedur untuk searching data
//prosedur untuk mencari node berdasarkan data
void FindNodeByData(linkedlist list, string data){
    if(isEmpty(list) == true){
        cout << "List kosong!" << endl;
    } else {
        address nodeBantu = list.first;
        int posisi = 0;
        bool found = false;
        while(nodeBantu != Nil){
            posisi++;
            if(nodeBantu->isidata.nama == data){
                cout << "Data " << data << " ditemukan pada posisi ke-" << posisi << "!" << endl;
                cout << "Nama Buah : " << nodeBantu->isidata.nama << ", Jumlah : " << nodeBantu->isidata.jumlah << ", Harga : " << nodeBantu->isidata.harga << endl;
                found = true;
                break;
            }
            nodeBantu = nodeBantu->next;
        }
        if(found == false){
            cout << "Node dengan data " << data << " tidak ditemukan!" << endl;
        }
    }
    cout << endl;
}

//prosedur untuk mencari node berdasarkan alamat node
void FindNodeByAddress(linkedlist list, address node) {
    if(isEmpty(list) == true) {
        cout << "List kosong!" << endl;
    } else {
        address nodeBantu = list.first;
        int posisi = 0;
        bool found = false;
        while (nodeBantu != Nil) {
            posisi++;
            if(nodeBantu == node) {
                cout << "Node ditemukan pada posisi ke-" << posisi << "!" << endl;
                cout << "Alamat node : " << nodeBantu << endl;
                cout << "Nama Buah : " << nodeBantu->isidata.nama << ", Jumlah : " << nodeBantu->isidata.jumlah << ", Harga : " << nodeBantu->isidata.harga << endl;
                found = true;
                break;
            }
            nodeBantu = nodeBantu->next;
        }
        if(found == false) {
            cout << "Node dengan alamat " << node << " tidak ditemukan dalam list!" << endl;
        }
    }
    cout << endl;
}

//prosedur untuk mencari node berdasarkan range data (range harga)
void FindNodeByRange(linkedlist list, float hargaAwal, float hargaAkhir) {
    if(isEmpty(list) == true) {
        cout << "List kosong!" << endl;
    } else {
        address nodeBantu = list.first;
        int posisi = 0;
        bool found = false;
        cout << "--- Buah dalam range harga " << hargaAwal << " - " << hargaAkhir << " ---" << endl;
        cout << "-------------------------------------------" << endl;
        while (nodeBantu != Nil) {
            posisi++;
            float harga = nodeBantu->isidata.harga;
            if(harga >= hargaAwal && harga <= hargaAkhir) {
                cout << "Data ditemukan pada posisi ke-" << posisi << " :" << endl;
                cout << "Nama Buah : " << nodeBantu->isidata.nama << ", Jumlah : " << nodeBantu->isidata.jumlah << ", Harga : " << nodeBantu->isidata.harga << endl;
                cout << "-------------------------------------------" << endl;
                found = true;
            }
            nodeBantu = nodeBantu->next;
        }
        if(found == false) {
            cout << "Tidak ada data buah dalam range harga tersebut!" << endl;
            cout << "-------------------------------------------" << endl;
        }
    }
    cout << endl;
}
```
Program C++ ini mengimplementasikan struktur data Abstract Data Type (ADT) Singly Linked List untuk pengelolaan data buah secara dinamis melalui file listBuah.h dan <iostream>, yang mencakup fungsi dasar pembuatan list kosong, alokasi serta dealokasi node, operasi penambahan data (insertFirst, insertAfter, insertLast), penghapusan data (delFirst, delLast, delAfter, deleteList), penelusuran dan perhitungan jumlah node (printList, nbList), pembaruan isi data (updateFirst, updateLast, updateAfter), serta fitur pencarian berdasarkan nama, alamat pointer, maupun rentang harga.

### 3. File main.cpp

```C++
  #include "listBuah.h"

  #include<iostream>
  using namespace std;

  int main(){
      linkedlist List;
      address nodeA, nodeB, nodeC, nodeD, nodeE = Nil;
      createList(List);

      dataBuah dtBuah;

      nodeA = alokasi("Jeruk", 100, 3000);
      nodeB = alokasi("Apel", 75, 4000);
      nodeC = alokasi("Pir", 87, 5000);
      nodeD = alokasi("Semangka", 43, 11500);
      nodeE = alokasi("Durian", 15, 31450);

      insertFirst(List, nodeA);
      insertLast(List, nodeB);
      insertAfter(List, nodeC, nodeA);
      insertAfter(List, nodeD, nodeC);
      insertLast(List, nodeE);

      cout << "--- ISI LIST SETELAH DILAKUKAN INSERT ---" << endl;
      printList(List);
      cout << "jumlah node : " << nbList(List) << endl;
      cout << endl;

      updateFirst(List);
      printList(List);
      updateLast(List);
      printList(List);
      updateAfter(List, nodeD);
      printList(List);

      cout << "--- ISI LIST SETELAH DILAKUKAN UPDATE ---" << endl;
      printList(List);
      cout << "jumlah node : " << nbList(List) << endl;
      cout << endl;

      FindNodeByData(List, "kelapa");
      FindNodeByAddress(List, nodeC);
      FindNodeByRange(List, 5000, 10000);

      delFirst(List);
      printList(List);
      delLast(List);
      printList(List);
      delAfter(List, nodeD, nodeC);
      printList(List);

      cout << "--- ISI LIST SETELAH DILAKUKAN DELETE ---" << endl;
      printList(List);
      cout << "jumlah node : " << nbList(List) << endl;
      cout << endl;

      deleteList(List);
      cout << "--- ISI LIST SETELAH DILAKUKAN HAPUS LIST ---" << endl;
      printList(List);
      cout << "jumlah node : " << nbList(List) << endl;
      cout << endl;

      return 0;
  }
```
Program  ini mengilustrasikan penerapan lengkap ADT singly linked list untuk data buah—yang diimpor dari listBuah.h dengan melakukan inisialisasi list kosong, pengisian dan penyisipan lima node (Jeruk, Pir, Semangka, Apel, Durian) menggunakan berbagai metode insert, pembaruan data lewat update, pengujian fitur searching, serta penghapusan beruntun hingga list kosong total dengan deleteList agar fungsi utama pada main tetap ringkas dan terstruktur

## Unguided 

### 1. Buatlah ADT Singly Linked list 


### Singlylist.h
```C++
#ifndef SINGLYLIST_H_INCLUDED
#define SINGLYLIST_H_INCLUDED

#include <iostream>
#define Nil NULL
using namespace std;

typedef int infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
};

struct List {
    address First;
};

void createList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);

#endif
```
### Singlylist.cpp  
```C++
#include "Singlylist.h"

void createList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.First;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}
```

### main.cpp 
```C++
#include <iostream>
#include "Singlylist.h"
using namespace std;

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    createList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    printInfo(L);
    return 0;
}
```
### Output Unguided 1 :

##### Output 1
![Output 1](https://github.com/Cupa0ng/109082500021_Gilang-Haryo-Yudianto_Pertemuan-6/blob/main/Output_Nomor1_singlylist/Output_Nomor1_singlylist.png)

kode ini mengimplementasikan penerapan ADT singly linked list secara rapi dengan membaginya ke dalam tiga file terpisah. Rancangannya dimulai pada file Singlylist.h (header) yang bertugas mendeklarasikan pondasi data, seperti infotype (bertipe int) sebagai wadah nilai data, pointer address untuk penunjuk elemen, struct ElmList yang menampung variabel info serta pointer next, hingga struct List yang dibekali jangkar utama bernama First. File header yang sudah dipagari include guard (#ifndef, #define, #endif) agar aman dari error saat kompilasi ini juga memuat lima prototipe fungsi dasar: createList, alokasi, dealokasi, printInfo, dan insertFirst. Selanjutnya, seluruh logika di balik fungsi-fungsi tersebut dieksekusi secara nyata di dalam file Singlylist.cpp (body). Di sini, createList akan mengosongkan list dengan menyetel L.First ke Nil, alokasi memesan memori baru lewat perintah new ElmList sekaligus menyetel pointer next-nya ke Nil, dan dealokasi bertugas menghapus elemen menggunakan delete lalu membersihkan sisa pointernya menjadi Nil. Sementara itu, fungsi printInfo bekerja menyisir list hingga ujung untuk mencetak isinya ke layar, dan insertFirst bertugas menyisipkan elemen baru tepat di barisan paling depan. Terakhir, file main.cpp (driver) bertindak sebagai penguji dengan membuat objek list L, menginisialisasinya, lalu mengalokasikan lima angka acak (2, 0, 8, 12, dan 9) untuk dimasukkan berturut-turut memakai insertFirst. Karena karakteristik operasi insert first selalu menumpuk elemen baru di posisi paling depan, urutan angka yang keluar di layar otomatis menjadi terbalik, yakni 9 12 8 0 2 persis seperti visualisasi pada Gambar 4-14 di modul Anda. Melalui pembagian struktur seperti ini, kita bisa melihat dengan jelas bagaimana singly linked list dibangun secara dinamis menggunakan pointer dalam format kode yang modular dan profesional.



### 2. Singlylist Pencarian 8 

### Singlylist.h 
```C++
#ifndef SINGLYLIST_H_INCLUDED
#define SINGLYLIST_H_INCLUDED

#include <iostream>
#define Nil NULL
using namespace std;

typedef int infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
};

struct List {
    address First;
};

void createList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);
address findElm(List L, infotype x);

#endif
```
### Singlylist.cpp 
```C++
#include "Singlylist.h"

void createList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.First;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}


address findElm(List L, infotype x) {
    address P = L.First;
    while (P != Nil) {
        if (P->info == x) {
            return P;
        }
        P = P->next;
    }
    return Nil;
}
```
### main.cpp 
```C++
#include <iostream>
#include "Singlylist.h"
using namespace std;

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    createList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    printInfo(L);
    address hasil = findElm(L, 8);
    if (hasil != Nil) {
        cout << hasil->info << " ditemukan dalam list" << endl;
    } else {
        cout << "8 tidak ditemukan dalam list" << endl;
    }
    return 0;
}
```
### Output Unguided 2 :

##### Output 2
![Output 2](https://github.com/Cupa0ng/109082500021_Gilang-Haryo-Yudianto_Pertemuan-6/blob/main/Output_Nomor2_pencarian8/Output_Nomor2_pencarian8.png)

Program C++ ini mengimplementasikan ADT singly linked list beserta operasi pencarian elemen secara modular melalui pemisahan kode menjadi file header, body, dan driver. Pada file Singlylist.h, didefinisikan struktur data elemen (ElmList) dengan field info bertipe int dan pointer next, struktur List berpenunjuk First, serta prototipe enam fungsi primitif yang dilindungi oleh include guard. Selanjutnya, file Singlylist.cpp merealisasikan seluruh fungsi tersebut, meliputi pembuatan list kosong (createList), alokasi dan dealokasi memori, penelusuran untuk mencetak isi list (printInfo), penyisipan di awal (insertFirst), serta pencarian alamat elemen spesifik (findElm). Terakhir, pada file main.cpp, list diinisialisasi dan diisi dengan lima elemen (2, 0, 8, 12, 9) menggunakan insertFirst sehingga menghasilkan urutan terbalik (9 12 8 0 2), diikuti dengan pengujian fungsi findElm untuk mendeteksi keberadaan angka 8 di dalam list, guna mendemonstrasikan siklus utuh manajemen dan pencarian data berbasis pointer.

### 3. Singlylist Total elemen

### Singlylist.h 
```C++
#ifndef SINGLYLIST_H_INCLUDED
#define SINGLYLIST_H_INCLUDED

#include <iostream>
#define Nil NULL
using namespace std;

typedef int infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
};

struct List {
    address First;
};

void createList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);
int totalInfo(List L);

#endif
```
### Singlylist.cpp 
```C++
#include "Singlylist.h"

void createList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.First;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

int totalInfo(List L) {
    int total = 0;
    address P = L.First;
    while (P != Nil) {
        total = total + P->info;
        P = P->next;
    }
    return total;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}
```
### main.cpp 
```C++
#include <iostream>
#include "Singlylist.h"
using namespace std;

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    createList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    printInfo(L);
    cout << "Total info dari kelima elemen adalah " << totalInfo(L) << endl;
    return 0;
}
```
### Output Unguided 3 :

##### Output 3
![Output 3](https://github.com/Cupa0ng/109082500021_Gilang-Haryo-Yudianto_Pertemuan-6/blob/main/Output_Nomor3_totalelemen/Output_Nomor3_totalelemen.png)

Program ini mendemonstrasikan implementasi ADT singly linked list modular yang terbagi menjadi tiga file untuk melakukan manajemen data dan operasi akumulasi total nilai elemen. Struktur data dideklarasikan pada file Singlylist.h melalui definisi komponen ElmList (berisi data info bertipe int dan pointer next), struktur List berpenunjuk First, serta enam fungsi primitif yang diproteksi oleh include guard. Implementasi fungsi tersebut diwujudkan di dalam Singlylist.cpp, mencakup pembuatan list kosong (createList), alokasi dan dealokasi memori, pencetakan isi list (printInfo), penyisipan di awal (insertFirst), serta fungsi totalInfo yang mengiterasi seluruh elemen untuk mengembalikan akumulasi nilai datanya. Alur program diselesaikan pada file main.cpp yang menguji fungsi-fungsi tersebut dengan menyisipkan lima elemen (2, 0, 8, 12, 9) secara terbalik (9 12 8 0 2), lalu memanggil totalInfo untuk menjumlahkan seluruh elemen tersebut hingga menampilkan hasil akhir berupa nilai 31.

## Kesimpulan
Modul 5 ini mendalami operasi lanjutan singly linked list dalam C++ berbasis pointer, khususnya fungsionalitas pencarian (searching) dan pembaruan (update) data secara sekuensial satu arah. Melalui latihan guided, operasi pencarian diimplementasikan dalam tiga variasi: berdasarkan data nama buah (FindNodeByData), alamat penunjuk (FindNodeByAddress), serta pemindaian menyeluruh untuk rentang harga tertentu (FindNodeByRange). Sementara itu, operasi update dilaksanakan melalui fungsi updateFirst, updateLast, dan updateAfter untuk memodifikasi konten data node tujuan tanpa mengubah posisinya dalam struktur list. Seluruh fungsionalitas tersebut dikemas secara modular ke dalam file header, body, dan driver main.cpp. Beralih ke latihan unguided, fokus bergeser pada manipulasi data integer menggunakan lima primitif dasar, di mana penyisipan menggunakan insertFirst menghasilkan urutan data terbalik (9 12 8 0 2), diikuti dengan pengujian fungsi pencarian elemen (findElm) untuk mendeteksi angka 8 serta fungsi akumulasi (totalInfo) yang berhasil menjumlahkan seluruh nilai elemen hingga menghasilkan total 31.

## Referensi
[1] Deitel, P., & Deitel, H. (2017). C++ How to Program (10th ed.). Boston: Pearson.<br>
[2] Liem, I. (2003). Diktat Pemrograman Prosedural. Bandung: Departemen Teknik Informatika Institut Teknologi Bandung.<br>
[3] Laboratorium Informatika Fakultas Informatika Telkom University. (n.d.). Modul 4: Singly Linked List (Bagian Pertama), Modul Praktikum Struktur Data.<br>
[4] cppreference.com. (n.d.). new expression dan delete expression. https://en.cppreference.com/w/cpp/language/new<br>

