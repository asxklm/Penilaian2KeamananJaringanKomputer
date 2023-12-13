# Penilaian 2 
### Mengerjakan Lab Practice pada link berikut https://cyberdefenders.org/ beserta pengerjaannya pada link berikut https://github.com/asxklm/WriteUP-CyberDefenders.git

## WriteUP-CyberDefenders
### Kelompok 4
| Nama | NRP |
|--------------------------------|------------|
|Sedtia Prakoso Budi Tirto Arto| 5027211014 |
|Arkan Hendri Abdul Ghani Burhan | 5027211026 |
|Bagus Cahyo Arrasyid | 5027211033 |
|Adimas Defatra Bimasena Sultanthik Maryono | 5027211036 |
|Ridho Husni Indrawan | 5027211039 |

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

<br><br>
## PsExec Hunt Blue Team Challenge
#### Category : Network Forensics
`Wireshark` `PCAP` `NetworkMiner` `PSExec` `Windows`

https://cyberdefenders.org/blueteam-ctf-challenges/143#nav-questions


#### 1. In order to effectively trace the attacker's activities within our network, can you determine the IP address of the machine where the attacker initially gained access?
Mencari alamat IP dari mesin penyerang yang mendapatkan akses dilakukan dengan analisis informasi paket yang menunjukkan `Setup Protocol` dan `Protocol Request`

![Screenshot 2023-12-13 093439](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/ba97e1c9-57bf-4608-84a4-741c79dfe936)

Jawabannya `10.0.0.130`

![Screenshot 2023-12-13 092236](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/49001710-0f29-4aa5-a73a-3d9fa2ab9714)

#### 2. To fully comprehend the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?
Menemukan hostname dari mesin yang diserang oleh penyerang dapat dilakukan pencarian `TCP Stream` dari paket diatas (yang mempunyai IP source `10.0.0.130`)

![Screenshot 2023-12-13 085603](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/75e8dd53-e11f-40fd-bb81-ad2126223eaa)

Jawabannya `SALES-PC`

![Screenshot 2023-12-13 092247](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/8deb2e2b-1278-4e97-a7ac-b155123051dd)

#### 3. After identifying the initial entry point, it's crucial to understand how far the attacker has moved laterally within our network. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?
Kredensial autentikasi didapati berada dibawah log aktivitas no.2 sebelumnya, ditemukan kredensial `User: \ssales` pada informasi paket tersebut

![Screenshot 2023-12-13 095031](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/27f1c496-0f6d-481a-baaf-a47e8bc48642)

Jawabannya `ssales`

![Screenshot 2023-12-13 092257](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/23bbf2b4-cb7b-4278-b1d1-bc380031e1b4)

#### 4. After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?
Nama program yang dijalankan oleh penyerang dapat dilihat pada log yang mengandung informasi paket inisiasi `file` didalamnya, didapati aktivitas transfer data `PSEXESCV.exe`

![Screenshot 2023-12-13 085801](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/45a3c71f-5f47-45b6-9e86-348d3c15649f)

Jawabannya `PSEXESVC`

![Screenshot 2023-12-13 092307](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/cc75e9bc-ade8-4426-b997-4ee691fc9dc1)

#### 5. We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?
Jaringan yang digunakan penyerang untuk melakukan instalasi dari PsExec berada pada informasi paket dibawah nomor sebelumnya, didapati tree jaringannya `\\10.0.0.133\ADMIN$`

![Screenshot 2023-12-13 095332](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/29a07f56-e48e-4c7d-adce-54bb0e50916f)

Jawabannya `ADMIN$`

![Screenshot 2023-12-13 093109](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/dbb47019-123a-4978-b73e-46515020ba0f)

#### 6. We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?
Pencarian jaringan komunikasi dai PsExec dapat dilihat tepat diatas paket no.5 dan dibawah paket no.2, karena pusat identifikasi aktivitas berada pada satu bagan.

![Screenshot 2023-12-13 095551](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/2130d6bf-965d-4e4d-a671-3db93ebd3780)

Jawabannya `IPC$`

![Screenshot 2023-12-13 093115](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/a82d8e94-81af-46fc-aeee-4606b6942a17)

#### 7. Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the machine's hostname to which the attacker attempted to pivot within our network?
Hostname mesin yang digunakan penyerang dapat dilihat pada format protocol `BROWSER` dikarenakan bentuk paket yang merujuk pada komunikasi 2 arah menggunakkan internet

![Screenshot 2023-12-13 100038](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/1feb08b5-6e6a-47de-a854-c3986928d10c)

Jawabannya `MARKETING-PC`

![Screenshot 2023-12-13 093121](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/ea45e155-03c0-4f18-9303-374a32d80b80)

<br><br>
## Tomcat Takeover Blue Team Challenge
#### Category : Network Forensics
`Wireshark` `PCAP` `NetworkMiner` `Tomcat` `Network`

https://cyberdefenders.org/blueteam-ctf-challenges/135#nav-questions

#### 1. Given the suspicious activity detected on the web server, the pcap analysis shows a series of requests across various ports, suggesting a potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?
Alamat IP yang menginisiasi penerimaan `Request` dapat dicari manual dengan melihat informasi yang ada pada paket yang telah terekam

![Screenshot 2023-12-13 100906](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/7f622eef-0cda-4416-ad86-084676b9d3ec)

Jawabannya `14.0.0.120`

![Screenshot 2023-12-13 100918](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/f6049baf-5688-4ef0-8859-a7137b17abc7)

#### 2. Based on the identified IP address associated with the attacker, can you ascertain the city from which the attacker's activities originated?
Source IP penyerang yaitu `117.11.88.124` dan kita bisa menggunakkan web IP tracker seperti https://www.iplocation.net/ip-lookup, maka didapati: 

![Screenshot 2023-12-13 101002](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/b09f68cc-cbe4-43b4-bc4d-6918840a13d3)

Jawabannya `Guangzhou`

![Screenshot 2023-12-13 101845](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/b57a3079-f4ba-4048-a2bf-ce772ac71b72)

#### 3. From the pcap analysis, multiple open ports were detected as a result of the attacker's activitie scan. Which of these ports provides access to the web server admin panel?
Ports yang digunakan untuk mengakses portal admin biasanya menggunakkan port 8080 atau publik

![Screenshot 2023-12-13 101216](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/45449b09-c2ae-4b2e-9093-9b574392aa7f)

Jawabannya `8080`

![Screenshot 2023-12-13 101223](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/806c89fb-e75c-431b-81ff-02eda3681a78)

#### 4. Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?
Melakukan kueri wireshark berdasarkan port dan method `(tcp.port == 8080 || udp.port == 8080) && (http.request.method == "GET")`

![Screenshot 2023-12-13 140702](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/66aa86e1-cc0e-47ac-a604-a9365a7a4d4f)

Melakukan cek paket dan melihat bagian `User-Agent`

![Screenshot 2023-12-13 140709](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/929a2254-4a79-4675-a687-3dce06285765)

Jawabannya `gobuster`

![Screenshot 2023-12-13 141003](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/fe4fd0cb-dd3e-4025-b176-20b37db1525e)

#### 5. Subsequent to their efforts to enumerate directories on our web server, the attacker made numerous requests trying to identify administrative interfaces. Which specific directory associated with the admin panel was the attacker able to uncover?
Admin panel yang menjadi sasaran penyerang dapat dicari pada bagian bawah setelah penyerang melakukan testing paket JSP 

![Screenshot 2023-12-13 110341](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/27b49782-6a9f-4767-82c8-9e59b8d2700f)

Jawabannya `/manager`

![Screenshot 2023-12-13 110305](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/65eb59bb-ba24-4091-bfbc-0198ce374cd2)

#### 6. Upon accessing the admin panel, the attacker made attempts to brute-force the login credentials. From the data, can you identify the correct username and password combination that the attacker successfully used for authorization?
Mencari kredensial dapat dengan menganalisis paket dengan method `GET` menuju direktori `/manager/`

![Screenshot 2023-12-13 110032](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/a562ac98-3275-4800-8bf6-8884f9ec95a4)

Jawabannya `admin:tomcat`

![Screenshot 2023-12-13 110043](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/45da5d09-8907-4bcd-8621-aa1d9b6ba1d0)

#### 7. Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the captured data?
Melakukan pencarian `TCP Stream` pada method post pada paket dibawah nomor sebelumnya

![Screenshot 2023-12-13 111647](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/51aeec5e-3bb8-47c5-aa71-52b5e28635cf)

Jawabannya `JXQOZY.war`

![Screenshot 2023-12-13 111742](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/3a20ee02-2ebf-41f8-951b-885af141615a)

#### 8. Upon successfully establishing a reverse shell on our server, the attacker aimed to ensure persistence on the compromised machine. From the analysis, can you determine the specific command they are scheduled to run to maintain their presence?

<br><br>
## GrabThePhisher Blue Team Challenge
#### Category : Threat Intel
`kit` `osint` `phishing` `threat` `intel`

https://cyberdefenders.org/blueteam-ctf-challenges/95#nav-questions

#### 1. Which wallet is used for asking the seed phrase?
Dompet yang ditanyakan pada frasa awal dapat dilihat dalam direktori file yang diberikan

![Screenshot 2023-12-13 115119](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/e826a39b-544c-44b6-929d-94e3ad0e42a7)

Jawabannya `metamask`

![Screenshot 2023-12-13 120706](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/bdd55cd2-a6b9-40f9-886e-29e02767f9fa)

#### 2. What is the file name that has the code for the phishing kit?
Dengan membuka folder `metamask` pada soal sebelumnya kita akan melihat folder `metamask.php` yang digunakan sebagai pishing

![Screenshot 2023-12-13 115141](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/450a13b8-32f6-42d4-a418-5d14da8e91c3)

Jawabannya `metamask.php`

![Screenshot 2023-12-13 120711](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/7f76fea5-1047-40c8-968d-5684e91b1585)

#### 3. In which language was the kit written?
Soal sebelumnya sudah dipaparkan menggunakkan format bahasa `.php`

Jawabannya `php`

![Screenshot 2023-12-13 120717](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/23874650-b71f-4fda-a4be-a600a62f0c6c)

#### 4. What service does the kit use to retrieve the victim's machine information?
Layanan yang digunakan pisher dapat dilihat dengan membuka file `metamask.php` dan melihat format request yang mengarah pada api `sypexgeo`

![Screenshot 2023-12-13 120342](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/36cdcbf6-92d3-40e9-8c88-bd03d7ecce69)

Jawabannya `sypex geo`

![Screenshot 2023-12-13 132037](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/0cbc55f9-d354-4f84-a974-d89c7dd437a6)

#### 5. How many seed phrases were already collected?
Melihat input yang sudah dikoleksi dapat membuka `/log.txt` pada folder `/log`

![Screenshot 2023-12-13 115457](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/ea0797b9-b5f7-4334-af2c-c0d744c2a0fe)

Jawabannya `3`

![Screenshot 2023-12-13 132042](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/ef371a59-95db-4ecb-bb65-11d42a4305d1)

#### 6. Write down the seed phrase of the most recent phishing incident?
Soal sebelumnya kita melihat ada 3 input, maka input terbawah adalah input terbaru yang diterima

Jawabannya `father also recycle embody balance concert mechanic believe owner pair muffin hockey`

![Screenshot 2023-12-13 132048](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/20ed7977-aa69-4fce-ad4a-3bdaa0e3cef5)

#### 7. Which medium had been used for credential dumping?
Merujuk pada soal no.4 dapat dilihat untuk menangkap kredensial korban, pisher menggunakkan telegram sebagai mediumnya

Jawabannya `Telegram`

![Screenshot 2023-12-13 132053](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/c789f6f3-6055-4556-852f-c75a52b61331)

#### 8. What is the token for the channel?
Token dapat dilihat pada bagian `Telegram` bawah pada gambar no.4

Jawabannya `5457463144:AAG8t4k7e2ew3tTi0IBShcWbSia0Irvxm10`

![Screenshot 2023-12-13 132059](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/8a8a35e9-1844-4909-84da-ee58df0d6bd0)

#### 9. What is the chat ID of the phisher's channel?
ID chat dapat dilihat pada bagian `Telegram` bawah pada gambar no.4

Jawabannya `5442785564`

![Screenshot 2023-12-13 132104](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/8862b733-9547-403b-b50c-8011b26739ba)

#### 10. What are the allies of the phish kit developer?
Aliasing dapat dilihat pada gambar no.4 di bagian yang di `comment`

Jawabannya `j1j1b1s@m3r0`

![Screenshot 2023-12-13 132109](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/57dd7dd9-8e5b-4404-ac2e-683096fe96e6)

#### 11. What is the full name of the Phish Actor?
Nama pisher dapat dicari dengan mengkueri api telegram yang diberikan dan mencarinya pada internet, kueri sebagai berikut `https://api.telegram.org/bot5457463144:AAG8t4k7e2ew3tTi0IBShcWbSia0Irvxm10/getChat?chat_id=5442785564`

![Screenshot 2023-12-13 115916](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/1cd63804-30da-4ce2-9d01-8722b2ec6d8d)

Jawabannya `Marcus Aurelius`

![Screenshot 2023-12-13 132114](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/622ba4c1-4198-4a22-a721-671c1ccc3d0c)

#### 12. What is the username of the Phish Actor?
Dilihat dari nomor sebelumnya didapati username dari pishernya

Jawabannya `pumpkinboii`

![Screenshot 2023-12-13 132119](https://github.com/asxklm/Penilaian2KeamananJaringanKomputer/assets/113827418/33789f98-bed6-41db-8ed5-de81e6cc4c3c)

