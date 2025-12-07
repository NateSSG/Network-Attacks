## H7 Aaltoja-Harjaamassa | Nathaniel Ssendagire 7.12.2025

## Ympäristö

OS: Kali Linux

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

Processor: AMD Ryzen 3 7320U - 4 cores used

Disk: 35 GB

Network: NAT

### Lue ja Tiivistä

- 433 MHz taajuus on yleinen matalatehoisille kaukosäätimille ja sääasemille.

- Signaali on ASK/OOK‑moduloitu (On-Off Keying), eli kanttaajuus on päällä tai pois bittien esittämiseksi.

- Universal Radio Hacker (URH) -ohjelmalla voi tallentaa, analysoida ja jopa lähettää signaaleja uudelleen.

- SDR‑laite (esim. RTL-SDR) vastaanottaa signaalin, URH:n spektrianalysaattori näyttää tarkan taajuuden ja signaalin voimakkuuden.

- Signaalin demodulaatio ja bittien pituuden määrittäminen (esim. ~700 µs per bitti) mahdollistaa sen tallentamisen ja uudelleenlähetyksen.

- Käytännön sovelluksena voidaan ohjata esimerkiksi 433 MHz kauko-ohjattuja pistorasioita tai laitteita, mikä havainnollistaa signaalien tallentamisen ja uudelleenlähetyksen mahdollisuuksia.

### A) WebSDR

<img width="736" height="358" alt="image" src="https://github.com/user-attachments/assets/fe84839d-d753-446c-b576-806f85c95805" />


<img width="1365" height="835" alt="image" src="https://github.com/user-attachments/assets/1a7d2de5-26a1-492c-9027-7899e0cee05d" />

Käytin WebSDR-etävastaanotinta ja viritin vastaanottimen taajuudelle 6070 kHz, joka on julkinen lyhytaaltoradiolähetys. WebSDR ilmoitti modulaatioksi AM (Amplitude Modulation), mikä on tyypillinen lyhytaaltolähetysten modulaatiomuoto. Aallonpituus taajuudelle 6070 kHz on noin 49,4 metriä, laskettuna kaavalla λ = c / f. Löysin lähetyksen säätämällä vastaanottimen HF-alueelle (3–30 MHz), skannaamalla vesiputousnäkymää ja valitsemalla voimakkaan signaalin kohdalta 6070 kHz. Kun modulaatioksi asetettiin AM, radion julkinen ohjelmasisältö alkoi kuulua kaiuttimista. Otin ruutukaappauksen asetuksista ja signaalin näkyvyydestä Waterfall-näkymässä.

<img width="1203" height="201" alt="image" src="https://github.com/user-attachments/assets/da943b7b-ab50-4699-9904-6ece157c49bd" />

Jos zoomaa vesiputoukseen, siinä näkyy tarkemmin että mitä ohjelmia pyörii tietyillä kanavilla.


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

### F) Tarkastele näytettä

<img width="1696" height="859" alt="radio hacker interface" src="https://github.com/user-attachments/assets/6fef0adc-bf61-4579-96e8-afcf3425618c" />

<img width="913" height="476" alt="urh stuff 2" src="https://github.com/user-attachments/assets/694b0b46-db22-4745-81af-07ef6068d606" />


Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s,
josta näkyy, että näyte on nauhoitettu 11.4.2025 klo 18:33:54, taajuudella 433.92 MHz, näytteenottotaajuudella 2 MSps ja kaistanleveydellä 2 MHz. Näytteen kokonaispituus on noin 0.4 sekuntia.

Silmämääräisesti URH:ssa signaali näyttää tyypilliseltä ASK/OOK-pohjaiselta 433 MHz kaukosäätimen radiokehykseltä, jossa näkyy selkeitä 1‑ ja 0‑pulssijaksoja (On/Off) sekä useita peräkkäisiä samansisältöisiä radiokehyksiä. Kehysten välissä on noin 9.5 ms ja 2.4 ms pituisia taukoja.

<img width="1694" height="870" alt="urh stuff" src="https://github.com/user-attachments/assets/247d4dda-b4d5-4409-94f1-1eeb94f0c849" />

Signaalidata koostuu pitkiä bittijonoja sisältävistä pulssijonoista (“10100000…”), jotka toistuvat useita kertoja.

### G) Bittistä

<img width="1683" height="642" alt="sample thingy" src="https://github.com/user-attachments/assets/67faa248-2427-4ee6-b4cb-da72dede0337" />

<img width="783" height="143" alt="ASCII data" src="https://github.com/user-attachments/assets/3f171ecd-4dfe-409b-b644-a53985096855" />

<img width="1407" height="58" alt="pause samples" src="https://github.com/user-attachments/assets/8681fca1-1b2d-4475-b4ed-f5b1cd4bd22f" />

URH näyttää demoduloidun bittijonon raakabitteinä (1 ja 0), jotka on johdettu ASK/OOK-modulaatiosta.

Tämän jälkeen URH näyttää:

[Pause: 19965 samples]


Tämä “Pause” tarkoittaa, että signaalissa oli hiljaisuutta = lähetys ei ollut päällä.

### 2. Miksi “Pause” on tärkeä?

Koska tauon pituus kertoo:

- missä paketti (frame) loppuu

- milloin uusi paketti alkaa

- mikä on lähetyksen toistotahti (samalla logiikalla kuin kaukosäätimissä)

Esimerkiksi:

- Pause: 19965 samples

- Sample rate: 2 MSps

- Aika = 19965 / 2 000 000 = 0,00998 s = noin 10 ms

Eli tässä signaali on hiljaa noin 10 millisekuntia bittijonojen välissä.

### 3. Mitä tämä kertoo bittitasosta?

Se kertoo, että signaali koostuu toistuvista paketeista, joissa:

- ensin tulee pitkä bittijono (raakabitit)

- sitten on 10 ms tauko

- sitten sama tai hieman muokattu bittijono lähetetään uudestaan

Tämä on täysin normaalia esim.
433 MHz kaukosäätimille
Ovikelloille
Plug-on/off-saksalaisille (KlikAanKlikUit jne.)

Lähetin toistaa paketin 3–10 kertaa, jotta vastaanotin varmasti kuulee sen.

Tiivistelmä: URH:n bittinäkymä tuottaa ASK/OOK-modulaatiosta saadut raakabitit pitkinä 1- ja 0-sekvensseinä. Näiden sekvenssien välissä näkyvät “Pause”-kohdat, joiden pituus on noin 10 ms. Nämä tauot eivät ole bittien osa, vaan ne kertovat pakettien (framejen) välisestä hiljaisuudesta. Lähetin toistaa saman bittikehyksen useita kertoja, ja 10 ms tauko erottaa nämä toistot toisistaan.

## Lähteet

https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html,

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#h7-aaltoja-harjaamassa,

https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s, 

http://websdr.ewi.utwente.nl:8901/








