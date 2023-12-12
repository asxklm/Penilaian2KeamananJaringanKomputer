# Penilaian 2 
### Mengerjakan Lab Practice pada link berikut https://cyberdefenders.org/ beserta pengerjaannya pada link berikut https://github.com/asxklm/WriteUP-CyberDefenders.git

<br><br>
## WriteUP-CyberDefenders
### Kelompok 4
| Nama | NRP |
|--------------------------------|------------|
|Sedtia Prakoso Budi Tirto Arto| 5027211014 |
|Arkan Hendri Abdul Ghani Burhan | 5027211026 |
|Bagus Cahyo Arrasyid | 5027211033 |
|Adimas Defatra Bimasena Sultanthik Maryono | 5027211036 |
|Ridho Husni Indrawan | 5027211039 |

<br><br>
## WebStrike Blue Team Challenge
#### Category : Network Forensics
`Wireshark` `PCAP` `Exfiltration`

https://cyberdefenders.org/blueteam-ctf-challenges/149#nav-questions

#### 1. Understanding the geographical origin of the attack aids in geo-blocking measures and threat intelligence analysis. What city did the attack originate from?
Jika dilihat dari file packet capture wireshark, guna mendapatkan kota dari organisasi penyerang dapat menggunakkan IP tracker online pada google.

![Screenshot 2023-12-12 002109](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/4c118ef9-506c-4d55-91c4-b82b7a11c4c1)

Source IP penyerang yaitu `117.11.88.124` dan kita bisa menggunakkan web IP tracker seperti https://www.iplocation.net/ip-lookup, maka didapati: 

![Screenshot 2023-12-12 002520](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/5ad187f6-252e-42f0-9d7d-53f734b02495)

Jawabannya `Tianjin`

![Screenshot 2023-12-12 002620](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/9725fa9f-20bc-48d7-906e-64890d4ef6d1)

#### 2. Knowing the attacker's user-agent assists in creating robust filtering rules. What's the attacker's user agent?
Untuk mendapatkan UA atau User Agent atau aplikasi yang digunakan penyerang, dapat dilihat menggunakkan `TCP Follow` pada salah satu paketnya.

![Screenshot 2023-12-12 002923](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/7533623b-495c-4fe0-9c53-b650c2590e2d)

Jawabannya `Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0`

![Screenshot 2023-12-12 002819](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/77d0bb37-920d-43cd-a357-448a2787ef5e)

#### 3. We need to identify if there were potential vulnerabilities exploited. What's the name of the malicious web shell uploaded?
Mencari web shell yang mencurigakan dari list dapat menggunakkan kueri `http` protocol karena bentuk web, dan melakukan analisis pada path yang mengandung `/uploads/` dan terlihat abnormal atau mencurigakan.

![Screenshot 2023-12-12 004135](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/6807eede-8865-4a29-9fd7-ecc604550326)

Jawabannya `image.jpg.php`

![Screenshot 2023-12-12 003111](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/5576b3eb-7080-43a3-9994-cacb2ab82f88)

#### 4. Knowing the directory where files uploaded are stored is important for reinforcing defenses against unauthorized access. Which directory is used by the website to store the uploaded files?
Menemukan direktori yang menyimpan file upload sangat mudah, masih sama dengan sebelumnya menggunakkan kueri `http` dan melihat path yang mengandung `/upload/`

![Screenshot 2023-12-12 004528](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/2e831256-6935-4c05-b754-e51a176d2319)

Jawabannya `/reviews/uploads/`

![Screenshot 2023-12-12 004712](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/683f5d4c-83ca-4e55-bb2f-1248db51643f)

#### 5. Identifying the port utilized by the web shell helps improve firewall configurations for blocking unauthorized outbound traffic. What port was used by the malicious web shell?
Mencari `port` yang digunakan oleh web shell yang mencurigakan dapat dilihat langsung dari return packet sesudah melakukan request web shell yang mencurigakan tersebut.

![Screenshot 2023-12-12 004936](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/f7baa48f-eaab-4f60-8997-f9c359e064f4)

Jawabannya `8080`

![Screenshot 2023-12-12 005159](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/e44901cc-f17a-4bad-93ea-1174d2d334fa)

#### 6. Understanding the value of compromised data assists in prioritizing incident response actions. What file was the attacker trying to exfiltrate?
Menemukan file yang ingin di exfiltrate oleh penyerang dapat dilihat dari warna line capture yang berbeda dari lainnya atau dapat menggunakkan kueri `_ws.expert.severity == "Warning"` guna mencari aktivitas berbahaya yang dilakukan

![Screenshot 2023-12-12 005936](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/b5482e0a-87ea-4fab-b571-4408066bfeb8)

Kemudian melakukan analisis dengan `TCP Stream` untuk mendapatkan detail aktivitas yang dilakukan, dan ditemukan aktivitas menyusupi file `passwd`

![Screenshot 2023-12-12 010054](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/3a5fffb0-ff5a-48a9-96c8-53aa3bd3f0c9)

Jawabannya `passwd`

![Screenshot 2023-12-12 005345](https://github.com/asxklm/WriteUP-CyberDefenders/assets/113827418/a047d0c1-0dba-46bb-9a0d-00a47472bedf)

<br><br>
## PoisonedCredentials Blue Team Challenge
#### Category : Network Forensics
`Wireshark` `PCAP` `Credentials`

https://cyberdefenders.org/blueteam-ctf-challenges/146#nav-questions

#### 1. In the context of the incident described in the scenario, the attacker initiated their actions by taking advantage of benign network traffic from legitimate machines. Can you identify the specific mistyped query made by the machine with the IP address 192.168.232.162?
Melakukan queri wireshark `ip.src == 192.168.232.162` untuk mencari queri yang dilakukan alamat IP tersebut, kemudian analisis info paket dari masing-masing yang dilakukan IP tersebut

![Screenshot 2023-12-12 233442](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/bf526f18-b48b-44ed-9ae4-cb35ebde4adb)

Jawabannya `FILESHAARE`

![Screenshot 2023-12-12 231043](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/261f720e-c076-475b-bf53-6018dbd9c729)

#### 2. We are investigating a network security incident. For a thorough investigation, we need to determine the IP address of the rogue machine. What is the IP address of the machine acting as the rogue entity?
Alamat IP didapatkan setelah melakukan analisis paket yang telah mengkueri `FILESHAARE` dalam paket capture tersebut

![Screenshot 2023-12-12 234304](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/227e6ad5-e4f2-4454-8a6e-533bfea53e7e)

Jawabannya `192.168.232.215`

![Screenshot 2023-12-12 231119](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/1e337090-9c2f-443c-9ead-d5b743f396d3)

#### 3. During our investigation, it's crucial to identify all affected machines. What is the IP address of the second machine that received poisoned responses from the rogue machine?
Mencari alamat IP penerima respon beracun dapat dengan kueri wireshark `ip.src == 192.168.232.215` yang merujuk pada IP penyerang dan melihat IP destinasi. Didapati terdapat 2 alamat IP yang menjadi destinasi seranngan, maka diambil alamat IP yang kedua

![Screenshot 2023-12-12 234405](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/026d234a-9d11-452e-9d69-eba6a169e0d8)

Jawabannya `192.168.232.176`

![Screenshot 2023-12-12 231127](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/2c099590-ded4-4e90-be07-ac5bf5dd2d66)

#### 4. We suspect that user accounts may have been compromised. To assess this, we must determine the username associated with the compromised account. What is the username of the account that the attacker compromised?
Mencari kredensial yang dikompromi oleh penyerang dapat dengan analisis info paket secara manual atau dapat menggunakkan queri `ntlmssp.auth.username` dan akan muncul username dari paket tersebut

![Screenshot 2023-12-12 233103](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/aa47d1fd-fe19-4f13-bdfc-535701dab582)

Jawabannya `janesmith`

![Screenshot 2023-12-12 231135](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/70d4784d-35d8-4d77-9271-897e9e6b4d74)

#### 5. As part of our investigation, we aim to understand the extent of the attacker's activities. What is the hostname of the machine that the attacker accessed via SMB?
Menemukan hostname mesin yang diserang via SMB dapa ditemukan dengan mengkueri `SMB` / `SMB2` pada capture dan mengikuti `TCP Stream` dari paket tersebut.

![Screenshot 2023-12-12 231628](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/82ef65e5-fc86-4925-a222-36b5ec38a6d9)

Jawabannya `ACCOUNTINGPC`

![Screenshot 2023-12-12 231143](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/e2c0af2e-95ad-453b-917a-fef1f0fa341a)
