# WriteUP-CyberDefenders
## Kelompok 4
- Bagus Cahyo Arrasyid (5027211033)

## WebStrike Blue Team Challenge
### Category : Network Forensics
`Wireshark` `PCAP` `Exfiltration`

https://cyberdefenders.org/blueteam-ctf-challenges/149#nav-questions

### 1. Understanding the geographical origin of the attack aids in geo-blocking measures and threat intelligence analysis. What city did the attack originate from?
Jika dilihat dari file packet capture wireshark, guna mendapatkan kota dari organisasi penyerang dapat menggunakkan IP tracker online pada google.

![Screenshot 2023-12-12 002109](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/4c118ef9-506c-4d55-91c4-b82b7a11c4c1)

Source IP penyerang yaitu `117.11.88.124` dan kita bisa menggunakkan web IP tracker seperti https://www.iplocation.net/ip-lookup, maka didapati: 

![Screenshot 2023-12-12 002520](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/5ad187f6-252e-42f0-9d7d-53f734b02495)

Jawabannya `Tianjin`

![Screenshot 2023-12-12 002620](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/9725fa9f-20bc-48d7-906e-64890d4ef6d1)

### 2. Knowing the attacker's user-agent assists in creating robust filtering rules. What's the attacker's user agent?
Untuk mendapatkan UA atau User Agent atau aplikasi yang digunakan penyerang, dapat dilihat menggunakkan `TCP Follow` pada salah satu paketnya.

![Screenshot 2023-12-12 002923](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/7533623b-495c-4fe0-9c53-b650c2590e2d)

Jawabannya `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`

![Screenshot 2023-12-12 002819](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/77d0bb37-920d-43cd-a357-448a2787ef5e)

### 3. We need to identify if there were potential vulnerabilities exploited. What's the name of the malicious web shell uploaded?
Mencari web shell yang mencurigakan dari list dapat menggunakkan kueri `http` protocol karena bentuk web, dan melakukan analisis pada path yang mengandung `/upload/` dan terlihat abnormal atau mencurigakan.

![Screenshot 2023-12-12 004135](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/6807eede-8865-4a29-9fd7-ecc604550326)

Jawabannya `image.jpg.php`

![Screenshot 2023-12-12 003111](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/5576b3eb-7080-43a3-9994-cacb2ab82f88)

### 4. Knowing the directory where files uploaded are stored is important for reinforcing defenses against unauthorized access. Which directory is used by the website to store the uploaded files?
Menemukan direktori yang menyimpan file upload sangat mudah, masih sama dengan sebelumnya menggunakkan kueri `http` dan melihat path yang mengandung `/upload/`

![Screenshot 2023-12-12 004528](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/2e831256-6935-4c05-b754-e51a176d2319)

Jawabannya `/reviews/uploads/`

![Screenshot 2023-12-12 004712](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/683f5d4c-83ca-4e55-bb2f-1248db51643f)

### 5. Identifying the port utilized by the web shell helps improve firewall configurations for blocking unauthorized outbound traffic. What port was used by the malicious web shell?
Mencari `port` yang digunakan oleh web shell yang mencurigakan dapat dilihat langsung dari return packet sesudah melakukan request web shell yang mencurigakan tersebut.

![Screenshot 2023-12-12 004936](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/f7baa48f-eaab-4f60-8997-f9c359e064f4)

Jawabannya `8080`

![Screenshot 2023-12-12 005159](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/e44901cc-f17a-4bad-93ea-1174d2d334fa)

### 6. Understanding the value of compromised data assists in prioritizing incident response actions. What file was the attacker trying to exfiltrate?
Menemukan file yang ingin di exfiltrate oleh penyerang dapat dilihat dari warna line capture yang berbeda dari lainnya atau dapat menggunakkan kueri `_ws.expert.severity == "Warning"` guna mencari aktivitas berbahaya yang dilakukan

![Screenshot 2023-12-12 005936](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/b5482e0a-87ea-4fab-b571-4408066bfeb8)

Kemudian melakukan analisis dengan `TCP Stream` untuk mendapatkan detail aktivitas yang dilakukan, dan ditemukan aktivitas menyusupi file `passwd`

![Screenshot 2023-12-12 010054](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/3a5fffb0-ff5a-48a9-96c8-53aa3bd3f0c9)

Jawabannya `passwd`

![Screenshot 2023-12-12 005345](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/a047d0c1-0dba-46bb-9a0d-00a47472bedf)
