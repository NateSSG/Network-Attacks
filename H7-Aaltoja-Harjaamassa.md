## H7 Aaltoja-Harjaamassa | Nathaniel Ssendagire 7.12.2025

## Ympäristö

OS: Kali Linux

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

Processor: AMD Ryzen 3 7320U - 4 cores used

Disk: 35 GB

Network: NAT

### B) rtl_433

<img width="288" height="103" alt="installing rtl-433" src="https://github.com/user-attachments/assets/a97ee2f2-895b-4c17-b8c1-bacce462e784" />, 


<img width="591" height="115" alt="rtl version" src="https://github.com/user-attachments/assets/61137323-9487-4609-b69b-057a17e735ec" />

### C) Automaattinen analyysi

<img width="1038" height="871" alt="image" src="https://github.com/user-attachments/assets/36af9e7c-6cbe-4498-814d-ad0c4a2fd56f" />


Näyte Converted_433.92M_2000k.cs8 sisältää useita toistuvia 433,92 MHz radiolähetyksiä, jotka rtl_433 tunnisti. Signaalit ovat tyypillisiä kaukosäätimille ja langattomille pistorasioille, jotka käyttävät yksinkertaista OOK-modulaatiota (On-Off Keying).

rtl_433 tunnisti kolme eri protokollaa, jotka kaikki tunnistavat saman lähetyksen hieman eri tavalla:

- KlikAanKlikUit-Switch (KaKu)

- Proove-Security

- Nexa-Security

Nämä kolme ovat keskenään yhteensopivia 433 MHz kauko-ohjattavia pistorasiajärjestelmiä, joten on normaalia, että sama signaali näkyy useana mallina.

### D) Too compex 16? 

<img width="642" height="95" alt="tiedoston muunnos rtl" src="https://github.com/user-attachments/assets/07586278-dfbc-4d8d-88bd-55a3fe2a6ecc" />

Näyte Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s muunnettiin rtl_433 yhteensopivaan muotoon nimeämällä tiedosto muotoon:

- Recorded_433.92M_2000k.cs8

Tämän jälkeen rtl_433 tunnisti siitä seuraavat protokollat:

- KlikAanKlikUit-Switch

- Proove-Security

- Nexa-Security

Kaikki protokollat tunnistivat saman lähettäjän:
ID/House Code: 8785315

Komento oli joka kerralla:
OFF

Näytteessä siis on 433,92 MHz kaukosäätimen OFF-komento, joka toistuu useaan kertaan.

<img width="840" height="787" alt="output of the tiedosto muunnos" src="https://github.com/user-attachments/assets/bbad2f08-6717-40a2-acdc-36cc585afa39" />

### E) Asenna URH

Tässä käytin apuna tunnilla käytyjä komentoja läpi.

<img width="852" height="809" alt="urh installation" src="https://github.com/user-attachments/assets/01ab9d1c-9010-432f-b2f6-774f9b6d7cea" />

<img width="842" height="672" alt="building urh" src="https://github.com/user-attachments/assets/67b9ef38-17f4-494a-a3fd-3cfc828ca90f" />

<img width="287" height="55" alt="urh launch" src="https://github.com/user-attachments/assets/35ed8c53-0833-43d8-87ff-04347eca9ac5" />

### F & G) Tarkastele näytettä

<img width="1696" height="859" alt="radio hacker interface" src="https://github.com/user-attachments/assets/6fef0adc-bf61-4579-96e8-afcf3425618c" />

<img width="913" height="476" alt="urh stuff 2" src="https://github.com/user-attachments/assets/694b0b46-db22-4745-81af-07ef6068d606" />


Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s,
josta näkyy, että näyte on nauhoitettu 11.4.2025 klo 18:33:54, taajuudella 433.92 MHz, näytteenottotaajuudella 2 MSps ja kaistanleveydellä 2 MHz. Näytteen kokonaispituus on noin 0.4 sekuntia.

Silmämääräisesti URH:ssa signaali näyttää tyypilliseltä ASK/OOK-pohjaiselta 433 MHz kaukosäätimen radiokehykseltä, jossa näkyy selkeitä 1‑ ja 0‑pulssijaksoja (On/Off) sekä useita peräkkäisiä samansisältöisiä radiokehyksiä. Kehysten välissä on noin 9.5 ms ja 2.4 ms pituisia taukoja.

<img width="1694" height="870" alt="urh stuff" src="https://github.com/user-attachments/assets/247d4dda-b4d5-4409-94f1-1eeb94f0c849" />

Signaalidata koostuu pitkiä bittijonoja sisältävistä pulssijonoista (“10100000…”), jotka toistuvat useita kertoja.








