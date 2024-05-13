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

  Pada kubctl, perintah get dengan opsi -n <name_space> artinya perintah get dilakukan pada name space <name_space>. Name space sendiri merupakan pengelompokkan objek kubernetes ke dalam unit yang terisolasi. Hal ini dapat mempermudah kita dalam melakukan manajemen terhadap _resources_ yang kita punya. 

  Perintah `kubectl get pods,services -n kube-system` tidak menampilkan pods yang dibuat secara eksplisit karena pods yang telah dibuat secara eksplisit tidak berada pada name space `kube-system`. Padahal, perintah di atas spesifik melakukan get di name space `kube-system`. Name space `kube-system` digunakan untuk objek yang berkaitan dengan infrastruktur inti dari kluster.
  
  Kita dapat melihat namespace yang kita miliki dengan command `kubectl get ns`. Command tersebut akan memberikan output sebagaimana di bawah ini.

  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/22b10a98-623e-43e9-9615-0b2c2a698601)

  Pod yang telah kita buat sebelumnya berada di _name space_ default. Dengan demikian, kita juga dapat melakukan perintah `kubectl get pods -n default` untuk menampilkan pod tersebut. Atau secara singkat, sama dengan `kubectl get pods`

  ![image](https://github.com/bangjai123/tutorial11/assets/120235144/2bafee45-d59b-4a43-b5d2-f2707650eb9b)

  Hal ini sekaligus mengilustrasikan jawaban dari pertanyaan sebelumnya, kenapa pods yang telah dibuat secara eksplisit tidak muncul pada perintah `kubectl get pods,services -n kube-system`. Yaitu karena pod tersebut tidak berada pada name space kube-system

</details>

# Refleksi Rolling Update Deployment
<details>
  <summary></summary>
</details>
