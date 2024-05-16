# Refleksi Hello Minikube
<details>
  <summary>Log Differences</summary>

  **Sebelum diexpose**
  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/6e84639c-d07e-4c39-8806-2ac2f7cac8e3)

  **Setelah diexpose dan open aplikasi 1 kali**
  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/3810cbdb-0304-4171-ba63-1e428a0c795d)

  **Log setelah 3 kali membuka aplikasi kembali**
  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/078f2a1a-255c-470f-aef6-2ddf4c5f3441)

Dari ketiga gambar di atas, dapat dilihat bahwa sebelum diexpose, service belum memiliki _request_ client, seperti HTTP. Setelah diexpose, service dapat menerima permintaan client. Hal ini terlihat dari adanya peningkatan pesan log. Dalam hal konteks gambar di atas, _request_ yang masuk adalah GET. Hal ini berarti service menerima _request_ tiap kali kita membuka aplikasi
  
</details>

<details>
  <summary>The -n option</summary>

  Pada kubectl, perintah get dengan opsi -n <name_space> artinya perintah get dilakukan pada name space <name_space>. Name space sendiri merupakan pengelompokkan objek kubernetes ke dalam unit yang terisolasi. Hal ini dapat mempermudah kita dalam melakukan manajemen terhadap _resources_ yang kita punya. 

  Perintah `kubectl get pods,services -n kube-system` tidak menampilkan pods yang dibuat secara eksplisit karena pods yang telah dibuat secara eksplisit tidak berada pada name space `kube-system`. Padahal, perintah di atas spesifik melakukan get di name space `kube-system`. Name space `kube-system` digunakan untuk objek yang berkaitan dengan infrastruktur inti dari kluster.
  
  Kita dapat melihat namespace yang kita miliki dengan command `kubectl get ns`. Command tersebut akan memberikan output sebagaimana di bawah ini.

  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/22b10a98-623e-43e9-9615-0b2c2a698601)

  Pod yang telah kita buat sebelumnya berada di _name space_ default. Dengan demikian, kita juga dapat melakukan perintah `kubectl get pods -n default` untuk menampilkan pod tersebut. Atau secara singkat, sama dengan `kubectl get pods`

  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/2bafee45-d59b-4a43-b5d2-f2707650eb9b)

  Hal ini sekaligus mengilustrasikan jawaban dari pertanyaan sebelumnya, kenapa pods yang telah dibuat secara eksplisit tidak muncul pada perintah `kubectl get pods,services -n kube-system`. Yaitu karena pod tersebut tidak berada pada name space kube-system

</details>

# Refleksi Rolling Update Deployment
<details>
  <summary>Perbedaan rolling update dengan recreate deployment strategy</summary>

   Kedua strategi di atas merupakan strategi deployment yang dapat dilakukan untuk mendeploy aplikasi. Perbedaan utama antara keduanya adalah bagaimana update akan dihandle. Pada _**rolling update**_, versi baru dari aplikasi menggantikan versi lama secara bertahap. Hal ini dilakukan dengan memutar pod-pod yang menjalankan aplikasi secara satu persatu. Dengan demikian, tidak ada downtime yang terjadi pada strategi ini saat dilakukan update deployment. Di sisi lain, _**Recreate deployment**_ dilakukan dengan mematikan pod terlebih dahulu, kemudian dijalankan versi yang terbaru. Hal tersebut dapat menyebabkan downtime saat update dilakukan. Meskipun demikian, cara ini relatif lebih mudah dilakukan dari pada strategi _rolling update_.
   
</details>

<details>
  <summary>Contoh implementasi recreate deployment strategy</summary>

  Untuk melakukan implementasi ini, terdapat tiga tahapan yang perlu dilalui. Ketiga tahapan ini adalah 1) scale down deployment saat ini, 2) update deployment, 3) scale up deployment yang telah dilakukan. Hal ini saya lakukan sebagai berikut.

  1. Kondisi mula-mula (terdapat 4 pods dengan versi 3.0.2)
     <img width="472" alt="image" src="https://github.com/bangjai123/tutorial11/assets/120235144/3beb5ccc-3d59-4c72-a96f-6c0f0d186370">

     ![image](https://github.com/bangjai123/tutorial11/assets/120235144/8416fd0b-6467-4d38-bcf3-bbf2f68c5e92)


  2. Scale down ke 0 pods (mematikan pods yang aktif)
     ![image](https://github.com/bangjai123/tutorial11/assets/120235144/c773b8ca-0313-4d43-8f55-43bdff1155a1)

  3. Melakukan update ke versi yang diinginkan (di sini saya memilih 3.2.1)
     ![image](https://github.com/bangjai123/tutorial11/assets/120235144/bdafa5f7-938b-4f3e-a7a5-abbb07cb4062)
     ![image](https://github.com/bangjai123/tutorial11/assets/120235144/05229f7e-509f-4659-bbf2-7f7777e3d0e1)
     ![image](https://github.com/bangjai123/tutorial11/assets/120235144/c029d1a4-4909-4b89-83ad-e593df9d9cca)
     
  4. Scale up ke jumlah pods yang diinginkan (di sini saya memilih 4) (menyalakan kembali layanan)
     ![image](https://github.com/bangjai123/tutorial11/assets/120235144/0c6cb6d7-0fab-4374-845d-15087fe43dc6)

</details>

<details>
  <summary>Membuat manifest file untuk recreate deployment strategy</summary>

  Yaml file untuk metode ini dapat diperoleh dengan pertama-tama mengeksekusi command `kubectl get deployments/spring-petclinic-rest -o yaml > <nama_file>.yaml` dan `kubectl get services/spring-petclinic-rest -o yaml > <nama_file>.yaml`. Selanjutnya, kita perlu mengubah 
  ```
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
```
menjadi 
```
  strategy:
    type: Recreate
```
pada file deployment.yaml. Hasil filenya dapat diliha pada direktori `recreate deployment strategy` pada repository ini.
</details>

<details>
  <summary>Kelebihan penggunaan manifest file</summary>

  Manifest files adalah file yang digunakan untuk mendefinisikan konfigurasi dan deployment aplikasi. Terdapat beberapa kelebihan yang mungkin didapatkan dengan menggunakan manifest file dibanding deployment secara manual. Beberapa kelebihan tersebut di antaranya adalah sebagai berikut.

  1. Automasi
     Kita dapat menggunakan manifest file dalam CI/CD.
       
  2. Konsistensi
     Dengan menggunakan manifest file, konfigurasi yang sama dapat diterapkan berkali-kali sehingga dapat mengurangi faktor kesalahan manusia.
     
  3. Dapat digunakan berkali-kali
     Dengan menyimpan manifest file, kita dapat melakukan deployment yang sama berkali-kali tanpa mengulangi langkah yang sama berulang-ulang.
     
  4. Dokumentasi
     Manifest file dapat berfungsi sebagai dokumentasi tentang konfigurasi dan infrasktruktur aplikasi.

  5. Version control
     Dengan menyimpan manifest file ke sistem _version control_, kita dapat melakukan _tracing_ terhadap perubahan yang terjadi. Dengan demikian, kita dapat mengembalikan ke versi yang lebih baik jika diperlukan.  
     
</details>
