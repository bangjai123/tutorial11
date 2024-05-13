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
</details>
