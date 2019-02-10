## Modul 1 Sistem Operasi
# Shell Scripting, Cron, dan AWK

## Prasayarat
1. Menginstall Sistem Operasi **Linux** 
2. Paham CLI (Command Line Interface) - [Modul Pengenalan CLI](https://github.com/raldokusuma/modul-pengenalan-CLI)
   
## Daftar Isi

## 1. Shell Scripting
### 1.1 Shell
Sebuah sistem operasi terdiri dari dua komponen utama, yaitu **Kernel** dan **Shell**.

![component](/images/component.png)

* **Kernel** adalah inti dari komputer. Komponen ini memungkinkan terjadinya komunikasi antara software dan hardware. Jika kernel adalah bagian terdalam dari sebuah sistem operasi, maka **shell** adalah bagian terluarnya.
* **Shell** adalah program penerjemah perintah yang menjembatani user dengan kernel. Umumnya, shell menyediakan **prompt** sebagai user interface tempat user menginputkan perintah-perintah yang diinginkan, baik berupa perintah internal maupun eksternal. Setelah menerima input dari user dan menjalankan program/perintah berdasarkan input tersebut, shell akan mengeluarkan output. Shell dapat diakses melalui **Terminal**.

> Catatan: Coba buka terminal di Linux, maka kamu akan menemukan **prompt** shell (biasanya **$**). Disitu, kamu dapat mengetik input berupa perintah, kemudian mengeksekusinya dengan menekan tombol "Enter". Output akan ditampilkan di terminal.

Ada 2 tipe shell utama di Unix/Linux, yaitu:
1. **Bourne Shell** - Prompt untuk shell ini adalah **$**
   * Bourne Shell (sh)
   * POSIX Shell (sh)
   * Korn Shell (ksh)
   * Bourne Again SHell (bash) 
2. **C Shell** - Prompt untuk shell ini adalah **%**
   * C Shell (csh)
   * TENEX/TOPS C Shell (tcsh)

### 1.2 Pemrograman Shell
**Pemrograman shell** adalah menyusun beberapa perintah shell (internal maupun eksternal) menjadi serangkaian perintah untuk melakukan tugas tertentu. <br>
Kelebihan shell di Linux adalah memungkinkan user untuk menyusun serangkaian perintah seperti halnya bahasa pemrograman interpreter, yakni melakukan proses input output, menyeleksi kondisi (decision making), looping, membuat fungsi, dsb. <br>
Pemrograman shell di Unix/Linux juga disebut dengan **shell scripting**.Untuk memudahkan, shell script dapat disimpan ke dalam sebuah **file** yang dapat dieksekusi kapanpun kita inginkan.<br> 
Manfaat belajar shell scripting:
   1. Dapat bekerja secara efektif dan efisien karena tidak perlu mengetik serangkaian perintah secara berulang-ulang, cukup menulis dan mengeksekusi satu file saja
   2. Dapat menjalankan beberapa perintah sebagai satu perintah
   3. Dapat menjalankan perintah secara otomatis

### 1.3 Perintah Dasar Shell

Shell yang digunakan dalam modul ini adalah **Bash** karena paling banyak digunakan pada distro Linux. Untuk memastikan shell apa yang kalian gunakan, coba lakukan:
```bash
$ echo $SHELL
```
Shell memiliki perintah **internal** (built-in shell) dan perintah **eksternal**. Untuk mengecek apakah sebuah perintah termasuk internal atau eksternal, gunakan perintah `type`
```bash
$ type cd
cd is a shell builtin
$ type bash
bash is /bin/bash
$ type read
read is a shell builtin 
$ type chmod
chmod is /bin/chmod
```
* Contoh perintah internal: `cd, pwd, times, alias, umask, exit, logout, fg, bg, ls, mkdir, rmdir, mv, cp, rm, clear, ...`
* Contoh perintah eksternal: `cat, cut, paste, chmod, lpr,...`. Beberapa perintah eksternal dapat dilihat di [Modul Pengenalan CLI](https://github.com/raldokusuma/modul-pengenalan-CLI)

Selain itu, ada beberapa karakter yang cukup penting untuk digunakan dalam shell:
1. **Redirection** (mengirim output ke file atau menerima input dari file) menggunakan operator redirect `>, >>, <, <<`, contoh:
    ```bash
    ls > data
    #hasil output ls dikirim ke file data. jika file belum ada akan dibuat, tetapi jika sudah ada, isinya akan ditimpa

    ls >> data
    #hampir sama, bedanya jika file sudah ada maka isinya akan ditambah di akhir file

    cat < data
    #file data dijadikan input oleh perintah cat
    ```
2. **Pipe** (output suatu perintah menjadi input perintah lain) menggunakan operator `|`, contoh:
    ```bash
    ls -l | sort -s
    #ouput perintah ls -l menjadi input perintah sort -s (urutkan secara descending)

    cat < data | sort > databaru
    ```
3. **Wildcard** menggunakan karakter `*, ?, [ ]`, contoh:
    ```bash
    ls i*
    #tampilkan semua file yang dimulai dengan i

    ls i?i
    #tampilkan file yang dimulai dengan i, kemudian sembarang karakter tunggal, dan diakhiri dengan i

    ls [ab]*
    #tampilkan file yang dimulai dengan salah satu karakter a atau b
    ```
Untuk melihat informasi secara lengkap, silahkan membuka manual bash dengan:
```bash
man bash
```

### 1.4 Simple Shell Script
1. Buatlah sebuah file berekstensi **.sh** menggunakan editor apapun, misalnya `nano`, `vi`, atau `gedit`.
    ```bash 
    nano nama_file.sh
    ```
    Misalnya:
    ```bash 
    nano hello-sisop.sh
    ```
2. Tulis beberapa baris perintah disana, diawali dengan **shebang** `#!/bin/bash`. Shebang berfungsi untuk memberitahu sistem bahwa perintah-perintah yg ada di dalam file tersebut harus dijalankan oleh Bash.
     ```bash 
    #!/bin/bash

    echo "Hello, sis!"
    ```

3. Simpan dan ubah permission file script agar dapat dieksekusi.
    ```bash
    chmod +x hello-sisop.sh
    ```
4. Eksekusi file script dengan cara `./nama_file.sh` atau `bash nama_file.sh`.

    ![ss-1](/images/ss-1.png)

### 1.4 Variabel

Beberapa hal yang perlu diperhatikan dalam mendefinisikan variabel:
   1. Nama variabel hanya boleh terdiri dari:
        * **Huruf** (a-z dan A-Z)
        * **Angka** (0-9)
        * Karakter **underscore** (_)
   2. Nama variabel dimulai dengan huruf atau underscore
   3. Tidak boleh menggunakan karakter spesial seperti `!, *, $, #, -`, dll karena karakter tersebut punya makna khusus untuk shell
   4. Bersifat case sensitive (membedakan huruf besar dan kecil)

**Syntax** 
* Mendefinisikan variabel
  ```bash
  nama_var=nilai
  ```
* Mengakses variabel
  ```bash
  $nama_var
  ```
**Tipe-Tipe Variabel**
* String
  ```bash
  nama_var="string"
  ```
* Integer
  ```bash
  nama_var=nilai
  ```
* Array
  ```bash
  #Jika isi array berupa string
  nama_var=("string0" "string1" "string2" ... "stringN")
  
  #Jika isi array berupa integer
  nama_var=(nilai0 nilai1 nilai2 ... nilaiN)    
  ```

Contoh:

```bash
#!/bin/bash

mata_kuliah="Sistem Operasi A"
semester=4
mahasiswa=("Khawari" "Raldo" "Aguel" "Tamtam")

echo "Variabel string:" $mata_kuliah
echo "Variabel integer:" $semester
echo "Variabel array ke-3:" ${mahasiswa[2]}
```
Output:

![ss-6](/images/ss-6.png)
  
Catatan:
* Syntax array diatas hanya dapat dieksekusi oleh **bash**, sehingga harus dieksekusi dengan cara `bash nama_file.sh` atau `bash ./nama_file.sh`. Jika menggunakan `./nama_file.sh` saja akan muncul error:
  
  ![ss-3](/images/ss-3.png)

#### Special Variable

*Beberapa special variable yang sering dipakai:

| Variabel | Deskripsi |
|---|---|
| $0 | Berisi nama file script yang sedang dijalankan |
| $n | n disini adalah angka desimal positif yang sesuai dengan posisi argumen (argumen pertama adalah `$1`, argumen kedua adalah `$2`, dst) |
| $# | Jumlah argumen yang diinput pada script |
| $* | Semua argumen $n |
| $? | Status exit dari perintah terakhir yang dijalankan |
| $$ | Proses ID (PID) shell saat ini |

Contoh:

```bash
#!/bin/bash

echo "Nama script : $0"
echo "Argumen ke-1 : $1"
echo "Argumen ke-2 : $2"
echo "Hai $1, selamat datang di kelas $2!"
echo "Total argumen : $#"
echo "Semua argumen : $*"
echo "PID : $$" 
```
Output:

![ss-2](/images/ss-2.png)

### 1.5 Input Output

**Read** digunakan untuk mengambil input dari keyboard dengan syntax sebagai berikut:
```bash
read nama_var
```  
**Echo** digunakan untuk menampilkan output dengan syntax sebagai berikut:
```bash
#Menampilkan teks biasa
echo "teks"

#Menampilkan isi dari sebuah variabel
echo $nama_var
```

Contoh:
```bash
#!/bin/bash

echo "Siapa namamu?"
read nama
echo "Hai $nama, selamat datang di praktikum sistem operasi!"
```
Output:

![ss-5](images/ss-5.png)

### 1.6 Quoting 
Shell Unix/Linux memiliki beberapa karakter spesial yang disebut dengan **metakarakter**. Karakter tersebut punya makna khusus jika digunakan di dalam shell script. Beberapa macam metakarakter:
```bash
* ? [ ] ' " \ $ ; & ( ) | ^ < > new-line space tab
```
Ada 4 jenis **quoting**, yaitu:
  
| No | Quoting | Deskripsi|
|---|---|---|
| 1 | Single Quote (') | Semua metakarakter di antara single quote akan kehilangan makna khusus |
| 2 | Double Quote (") | Sebagian besar metakarakter di antara double quote akan kehilangan makna khusus, kecuali `$, backquote, \$, \', \", \\` |
| 3 | Backslash (\\) | Karakter apa pun setelah backslash akan kehilangan makna khusus |
| 4 | Backquote (`) | Apa pun di antara back quote akan diperlakukan sebagai perintah dan akan dieksekusi |

Contoh:
```bash
#!/bin/bash

#Single quote
single=3
echo '$single'

#Double quote
double=3
echo "$single"

#Backslash
echo \<-\$1500.\*\*\>\; \(update\?\) \[y\|n\]

#Backquote
date=`date`
echo "Hari ini:" $date
```

Output:

![ss-4](/images/ss-4.png)


**Backslash Character**

| Karakter | Deskripsi |
|---|---|
| \n | New line (baris baru) |
| \a | Alert bell |
| \b | Backspace |
| \E | Escape character |

### 1.7 Operator Dasar
* Ada beberapa jenis operator yang didukung oleh shell, yaitu:
  1. Operator Aritmatika
  2. Operator Relasional
  3. Operator Boolean
  4. Operator String
  5. Operator File Test

Namun yang akan dibahas lebih jauh hanyalah operator **aritmatika** dan **relasional**.

#### Operator Aritmatika

| No | Operator | Deskripsi | 
|---|---|---|
| 1 | + | Penjumlahan | 
| 2 | - | Pengurangan | 
| 3 | * | Perkalian |
| 4 | / | Pembagian |
| 5 | % | Modulus (sisa pembagian) | 
| 6 | = | Menempatkan nilai di sisi kanan ke variabel di sisi kiri |
| 7 | == | Membandingkan 2 nilai yang sama |
| 8 | != | Membandingkan 2 nilai yang tidak sama |

Contoh:
```bash
#!/bin/bash

A=15
B=7

echo "$A + $B = $((A + B))"
echo "$A - $B = $((A - B))"
echo "$A * $B = $((A * B))"
echo "$A / $B = $((A / B))"
echo "$A % $B = $((A % B))"

B=$A

echo "A = $A"
echo "B = $B"
```
#### Operator Relasional

| No | Operator | Deskripsi | 
|---|---|---|
| 1 | -eq | Memeriksa apakah nilai kedua operan sama (==) |
| 2 | -ne | Memeriksa apakah nilai kedua operan tidak sama (!=) |
| 3 | -gt | Memeriksa apakah nilai operan kiri lebih besar daripada operan kanan (>) |
| 4 | -lt | Memeriksa apakah nilai operan kiri lebih kecil daripada operan kanan (<)  |
| 5 | -ge | Memeriksa apakah nilai operan kiri lebih besar atau sama dengan operan kanan (>=) |
| 6 | -le | Memeriksa apakah nilai operan kiri lebih kecil atau sama dengan operan kanan (<=) |

Operator relasional biasanya digunakan bersama dengan conditional statements, contoh:
```bash
#!/bin/bash

A=15
B=7

if [ $A -eq $B ]
then
   echo "$A -eq $B: A sama dengan B"
else
   echo "$A -eq $B: A tidak sama dengan B"
fi

if [ $A -ne $B ]
then
   echo "$A -ne $B: A tidak sama dengan B"
else
   echo "$A -ne $B: A sama dengan B"
fi

if [ $A -gt $B ]
then
   echo "$A -gt $B: A lebih besar dari B"
else
   echo "$A -gt $B: A lebih kecil dari B"
fi

...

if [ $A -le $B ]
then
   echo "$A -le $B: A lebih kecil atau sama dengan B"
else
   echo "$A -le $B: A lebih besar dari B"
fi
```
### 1.8 Conditional Statements
* **Conditional statements** digunakan untuk memungkinkan program dapat membuat keputusan yang benar dengan memilih tindakan tertentu berdasarkan syarat/kondisi tertentu.
* Ada 2 jenis conditional statements dalam Unix shell, yaitu:
  1. **if...else**
  2. **case...esac**
   
#### If...Else
* Syntax:
  ```bash
  if [ kondisi1 ]
  then 
    perintah1 #dieksekusi jika kondisi1 benar
  elif [ kondisi2 ]
  then
    perintah2 #dieksekusi jika kondisi2 benar
  else
    tindakan3 dieksekusi jika tidak ada kondisi yang benar
  fi
  ```
* Contoh:
  ```bash
  #!/bin/bash

  A=15
  B=7

  if [ $A == $B ]
  then
    echo "A sama dengan B"
  elif [ $A -gt $B ]
  then
    echo "A lebih besar dari B"
  elif [ $A -lt $B ]
  then
    echo "A lebih kecil dari B"
  else
    echo "Tidak ada kondisi yang memenuhi"
  fi
  ```

#### Case...Esac
* Syntax:
  ```bash
  case nama_var in
    pola1)
      perintah1 #dieksekusi jika pola1 cocok
      ;;
    pola2)
      perintah2 #dieksekusi jika pola2 cocok
      ;;
    pola3)
      perintah3 #dieksekusi jika pola3 cocok
      ;;
    *)
      perintah4 #dieksekusi jika tidak ada pola yang cocok
      ;;
  esac
  ```
* Contoh:
  ```bash
  #!/bin/bash

  echo -n "Apa makanan yang kamu suka?"
  read makanan

  case "$makanan" in
    "pentol") 
      echo "Pentol paling enak sedunia adalah pentol raja depan wardug" 
      ;;
    "pisang")
      echo "Hilo pisang enak, lho! (kata Nahda) Tapi itu minuman" 
      ;;
    "indomie")
      echo "Indomie enak, apalagi pake telor" 
      ;;
    *)
      echo "Maap, berarti makanan yang kamu suka gaenak hehe" 
      ;;
  esac
  ```

### 1.9 Loop
* **Loop** digunakan untuk mengeksekusi serangkaian perintah berulang kali. Ada beberapa macam shell loops:
  1. While loop
  2. For loop
  3. Until loop
  4. Select loop

#### While loop
* **While loop** digunakan untuk mengeksekusi serangkaian perintah berulang kali **selama** suatu kondisi terpenuhi.
* While digunakan jika kita ingin memanipulasi suatu variabel secara berulang-ulang.
*  Syntax:
    ```bash
    while kondisi
    do
      perintah #dieksekusi jika kondisi masih terpenuhi
    done
    ```
* Contoh:
  ```bash
  #!/bin/bash
  
  a=0

  while [ $a -lt 10 ]
  do
    echo $a
    a=$((a + 1))
  done
  ```
#### For loop
* **For loop** digunakan untuk mengulang serangkaian perintah untuk setiap item pada daftar.
* Syntax:
    ```bash
    for var in daftar_item
    do
      perintah #dieksekusi untuk setiap item dalam daftar
    done
    ```
* Contoh:
    ```bash
    #!/bin/bash

    for num in 0 1 2 3 4 5 6 7 8 9
    do
      echo $num
    done
    ```

#### Until loop
* Berbeda dengan while loop, **until loop** digunakan untuk mengeksekusi serangkaian perintah berulang kali **sampai** suatu kondisi terpenuhi.
*  Syntax:
    ```bash
    until kondisi
    do
      perintah #dieksekusi jika kondisi belum terpenuhi
    done
    ```
* Contoh:
    ```bash
    #!/bin/bash

    a=0

    until [ ! $a -lt 10 ]
    do
      echo $a
      a=$((a + 1))
    done
    ```

#### Select loop
* **Select loop** digunakan ketika kita ingin membuat sebuah program dengan beberapa daftar pilihan yang bisa dipilih oleh pengguna, misalnya menu.
*  Syntax:
    ```bash
    select var in daftar_item
    do
      perintah #dieksekusi untuk setiap item dalam daftar
    done
    ```
* Contoh:
    ```bash
    #!/bin/bash

    select minuman in teh kopi air jus susu semua gaada
    do
      case $minuman in
        teh|kopi|air|semua) 
          echo "Beli di warung!"
          ;;
        jus|susu)
          echo "Bikin sendiri di rumah!"
        ;;
        gaada) 
          break 
        ;;
        *) echo "ERROR" 
        ;;
      esac
    done
    ```

### 1.10 Function
* **Fungsi** digunakan untuk memecah fungsionalitas keseluruhan script menjadi sub-bagian yang lebih kecil. Sub-bagian itu dapat dipanggil untuk melakukan tugas masing-masing apabila diperlukan.
* Syntax:
  ```bash
  nama_fungsi () { 
    perintah1
    perintah2
    ...
    perintahN
  }
  ```
* Contoh:
  ```bash
  #!/bin/bash

  #define functions
  ask_name() {
    echo "Siapa namamu?"
  }
  reply() {
    read nama
    echo "Hai $nama, selamat datang di praktikum sistem operasi!"  
  }

  #call functions
  ask_name
  reply
  ```
#### Nested Functions
```bash
#!/bin/bash

#define functions
ask_name() {
  echo "Siapa namamu?"
  reply
}
reply() {
  read nama
  echo "Hai $nama, selamat datang di praktikum sistem operasi!"
}

#call functions
ask_name
```

### 1.11 Referensi 
* https://www.tutorialspoint.com/unix/shell_scripting.htm

## 2. Cron
Cron memungkinkan pengguna Linux dan Unix untuk menjalankan perintah atau script pada waktu tertentu secara otomatis. Cron service (daemon) secara konstan memeriksa _/etc/crontab_ file dan _/etc/cron.*/_ direktori juga _/var/spool/cron_ direktori. Setiap user memiliki crontab file masing-masing.

### 2.1 Menambahkan atau mengubah cron job
1. Ketikkan `crontab -e`
2. Ketikkan syntax crontab sesuai command yang diinginkan
3. Untuk melihat entri crontab, jalankan command `crontab -l`
* Syntax crontab<br>
  ![syntax-crontab](images/syntax.png)<br>
* Contoh perintah yang dijalankan dengan cron<br>
  ![contoh-cron](images/contoh-cron.png)<br>
  1. Menjalankan script routing setiap kali PC dinyalakan dan mencatatnya pada _route.log_
  2. Menjalankan script firewall setiap kali PC dinyalakan dan mencatatnya pada _iptables.log_<br>
- Untuk lebih memahami crontab dapat mengakses website [crontab-guru](https://crontab.guru).
    ![crontab-guru](images/crontab-guru.png)<br>

### 2.2 Referensi
* https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-Linux-or-unix-oses/

## 3. AWK
`awk` merupakan sebuah program yang bisa digunakan untuk mengambil catatan/record tertentu dalam sebuah file dan melakukan sebuah/beberapa operasi terhadap catatan/record tersebut.
<br>
Fungsi dasar `awk` adalah memeriksa sebuah file per barisnya (atau satuan teks lain) yang mengandung pola tertentu. Ketika sebuah baris cocok dengan salah satu pola, `awk` akan melakukan action tertentu pada baris tersebut. `awk` melanjutkan proses sampai menemui _end of file_ pada file yang menjadi masukan tadi.
<br>
__FYI__: `awk` versi baru dinamakan `gawk`, tapi biasanya tetap disebut `awk`.

### 3.1 Menjalankan program awk
__Syntax__:
```shell
awk options 'selection _criteria {action }' input-file > output-file
```

Misalnya kita memiliki data mahasiswa sebagai berikut:
```
hafara sidoarjo 1998 2015
satria bali 1996 2015
raldo cepu 1998 2016
awan sidoarjo 1996 2015
khawari semarang 1998 2016
aguel semarang 1998 2016
didin mojokerto 1997 2015
tamtam rembang 1997 2016
```

1. Secara default `awk` akan print semua baris pada file masukan:
    ```shell
    awk '{print}' mahasiswa.txt
    ```

2. Print baris yang mengandung pola yang dimasukkan:
    ```shell
    awk '/sidoarjo/ {print}' mahasiswa.txt
    ```

    Maka hasilnya adalah sebagai berikut:
    ```
    hafara sidoarjo 1998 2015
    awan sidoarjo 1996 2015
    ```

3. Dalam setiap baris, `awk` akan membagi setiap kata yang dipisahkan oleh spasi dan menyimpannya pada variabel `$n`. Jika terdapat 4 kata pada satu baris, maka kata pertama akan disimpan pada variabel `$1`, kata kedua pada variabel `$2`, dan seterusnya. `$0` merepresentasikan semua kata yang ada pada satu baris.
    ```shell
    awk '/semarang/ {print $1,$2}' mahasiswa.txt
    ```

    Maka hasilnya adalah sebagai berikut:
    ```
    khawari semarang
    aguel semarang
    ```

__Catatan__:
Dalam rule program `awk` boleh menghilangkan hanya salah satu di antara action atau pola. Jika pola dihilangkan, maka action akan diberlakukan ke semua baris. Sedangkan jika action dihilangkan, maka setiap baris yang mengandung pola tersebut akan secara default ditampilkan secara penuh.

### 3.2 Special Rules
Program `awk` memiliki rule yang memiliki kelakuan khusus. Di antaranya adalah `BEGIN` dan `END`. Rule `BEGIN` hanya dieksekusi satu kali, yaitu sebelum input dibaca. Rule `END` pun juga dieksekusi satu kali, hanya setelah semua input selesai dibaca. Contoh:
```shell
awk '
BEGIN { print "Jumlah baris yang terdapat \"2015\"" }
/2015/  { ++n }
END   { print "\"2015\" muncul", n, "kali." }' mahasiswa.txt
```

Maka hasilnya adalah sebagai berikut:
```
Jumlah baris yang terdapat "2015"
"2015" muncul 4 kali.
```

Pada contoh di atas, rule kedua hanya memiliki action untuk melakukan perhitungan berapa jumlah baris yang mengandung "2015", namun tidak ada action untuk menampilkan (print).


### 3.3 Referensi
* https://www.gnu.org/software/gawk/manual/gawk.html
* https://www.geeksforgeeks.org/awk-command-unixLinux-examples/

## 4. Latihan
1. Buatlah sebuah program menggunakan bash script untuk menentukan apakah sebuah string yang
Anda diinputkan merupakan palindrom atau bukan.
Contoh: malam = palindrom, makan != palindrom.
2. Buatlah sebuah task scheduling menggunakan crontab dan sebuah bash script untuk memindahkan
semua file mp3 ke /home/\<user>/Music, semua file mp4 ke /home/\<user>/Videos, dan semua file
jpg ke /home/\<user>/Pictures setiap satu menit. Awalnya, semua file mp3, mp4, dan jpg tersebut
terletak di /home/\<user>/Documents.
3. Buatlah sebuah program awk yang bisa menampilkan user yang melakukan proses. Tapi karena
kemungkinan besar jumlah barisnya akan sangat banyak, maka tampilkan secara distinct (tidak ada
user yang sama muncul lebih dari satu kali). Jika sudah bisa, coba masukkan hasilnya ke dalam file
pengguna.log (Hint: menggunakan pipe dan command ps)
