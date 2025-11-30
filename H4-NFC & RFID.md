# H4 NFC & RFID | Nathaniel Ssendagire 30.11.2025

## Ympäristö

OS: Windows 11

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: AMD Radeon RX 9060 XT

Processor: AMD Ryzen 7 5700x - 6 cores used

Disk: 1000 GB

Network: NAT

## 1. Tarkastele käytössäsi olevia RFID tuotteita, mieti miten hyvin olet suojautunut RFID urkinnalta?

- Tilanteeni: En ole käytännössä lainkaan suojautunut RFID-urkinnalta.

- Käytän tavallista nahka-/kangaslompakkoa, joka ei sisällä mitään RFID-suojauskerrosta. Tämä tarkoittaa, että kaikki RFID/NFC-korttini ja -passini ovat alttiita skimmaukselle (salakuuntelulle) ja mahdollisesti kloonaukselle.

### Mitä tämä käytännössä tarkoittaa:

- Pankki- ja luottokortit: Joku voisi kävellä minua kohti vilkuittaessa näyttävässä julkisessa paikassa (kauppakeskus, juna, katu) piilossa olevalla NFC-lukijalla ja lukea korttini tiedot ilman, että huomaan mitään. Vaikka he eivät saisi kaikkia tietoja (CVV-koodia yms.), he saavat kortin numeron ja vanhentumispäivän, mikä riittää usein verkkostoihin, joissa 3D-Secure -turvaa (esim. Verified by Visa) ei käytetä.

- Henkilökortit / Pääsykortit: Jos työpaikkani tai kouluni käyttää RFID-pääsykortteja, niitä voitaisiin teoriassa kloonata samalla tavalla, mikä mahdollistaisi luvattoman pääsyn tiloihin.

- Passi: Moderni elektroninen passi sisältää NFC-tagin. Vaikka se vaatii lukijalta salaisen avaimen tiedon lukemiseen, perustiedot (kuten passin numero) eivät ole suojattu ja ne voidaan lukea helposti, mikä on riski identiteettivarkaudelle.

## Miten suojautumistani voisi parantaa?

### Onneksi parannukset ovat yksinkertaisia ja halpoja:

- RFID-suojakotelo / -tasku: Se on tehokkain ja helpoin tapa. Nämä kotelot sisältävät metallikalvon, joka toimii Faradayn häkkinä. Se estää kaikki radiotaajuussignaalit pääsemästä kortteihin tai niistä pois. Kortti on täysin "näkymätön" lukijoille, kunnes se otetaan kotelosta pois. Hinta on useimmiten vain 5-20 euroa.

- RFID-suojalompakko: Sama periaate kuin kotelossa, mutta koko lompakko on suojattu. Tämä on kätevin ratkaisu, koska se korvaa nykyisen lompakkosi suojatulla versiolla.

- Käyttäytymisen muutos: Älä pidä lompakkoa ulkotaskussa tietyissä tilanteissa (esim. tungoksessa). Pidä se sisätaskussa tai taskussa, joka on vaikeammin saavutettavissa. Tämä ei ole yhtä tehokas kuin Faradayn häkki, mutta se vaikeuttaa urkintaa.

Yhteenveto: Nykyinen suojautumiseni on olematonta. Tavallinen lompakko tarjoaa yhtä paljon suojaa kuin paperipussi. RFID-suojalompakon hankinta olisi yksinkertainen ja erittäin tehokas parannus, joka estäisi yleisimmät urkintahyökkäykset kokonaan.

## 2. Tutustu APDU komentojen rakenteeseen (voit käyttää tekoälyä tutustumiseen)

### Deepseekin mukaan: Komento-APDU on kuin kysymys tai käsky. Sen rakenne koostuu pakollisista ja valinnaisista kentistä.

| Kentän Nimi | Pituus (tavua)        | Kuvaus & Esimerkki |
|-------------|------------------------|---------------------|
| CLA         | 1                      | Instruction Class. Määrittelee protokollan tyypin tai kontekstin (esim. ISO 7816, GSM). Esim: 0x00 for ISO 7816. |
| INS         | 1                      | Instruction Code. Varsinainen komento. Esim: 0xA4 (SELECT), 0xB0 (READ BINARY). |
| P1          | 1                      | Parametri 1. Ensimmäinen komentoon liittyvä parametri, esim. osoite tai valintatapa. |
| P2          | 1                      | Parametri 2. Toinen parametrikenttä, joka tarkentaa P1:n merkitystä. |
| Lc          | 0, 1, 2, or 3          | Komentodatan pituus. Ilmoittaa Data-kentän tavumäärän. Puuttuu, jos dataa ei ole. |
| Data        | Muuttuva (Lc:n mukaan) | Vapaaehtoinen datakenttä, esim. PIN-koodi tai tieto jota lähetetään kortille. |
| Le          | 0, 1, 2, or 3          | Odotetun vastausdatan pituus. 0x00 tarkoittaa usein "palauta maksimimäärä". |


## Yksinkertaistettu Esimerkki:
Komentoa "Lue 12 tavun tietoa" voisi esittää hex-muodossa näin:
00 B0 00 00 0C

00 = CLA (ISO-standardikortti)

B0 = INS (READ BINARY -komento)

00 = P1 (Parametri 1)

00 = P2 (Parametri 2)

0C = Le (Odota 12 tavua vastausdataa. Huomaa: 0x0C = 12)

### Response APDU Rakenne (Kortilta Lukijalle)

| Kentän Nimi | Pituus (tavua)           | Kuvaus & Esimerkki |
|-------------|---------------------------|---------------------|
| Data        | Muuttuva (pituus = Le)    | Kortin palauttama pyydetty data, esim. saldo, kortin numero jne. |
| SW1         | 1                         | Status Word 1. Kertoo onnistuiko komento, kuin HTTP-statuskoodi. |
| SW2         | 1                         | Status Word 2. Tarkentaa SW1:n merkitystä ja antaa lisätietoa. |

### Tärkeitä Status Word -pareja (SW1 SW2):

| SW1 SW2 | Merkitys               | Kuvaus |
|---------|-------------------------|--------|
| 90 00   | Success                 | Komento suoritettiin onnistuneesti. |
| 63 C0   | Verification failed     | PIN-tarkistus epäonnistui; Cx kertoo jäljellä olevien yritysten määrän. |
| 6A 82   | File not found          | Pyydettyä tiedostoa ei löytynyt. |
| 69 83   | Authentication blocked  | Kortti lukittu (liian monta väärää PINiä). |

### Yksinkertaistettu Esimerkki:
Vastaus edelliseen READ BINARY -komentoon voisi olla:

48 65 6C 6C 6F 20 57 6F 72 6C 64 21 90 00

48 65 ... 21 = Data (12 tavua, joka vastaa tekstiä "Hello World!" ASCII-muodossa)

90 00 = SW1 SW2 (Success! Kaikki meni hyvin)

## Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit. (Vinkki, useimmat liittyvät henkilökortteihin)

DEF CON -konferenssin "Wall of Sheep" ja Henkilökortin Kloonaus (2022 & recurring)

### Mitä tapahtui? 
DEF CON on maailman suurin hakkerikonferenssi. Ironisesti, juuri tällä tapahtumalla jaettuja RFID-pääsykortteja on hakkeroitu toistuvasti. Vuonna 2022 tutkijat osoittivat, että konferenssin omaa korttia pystyi kloonaamaan halvalla, yleisesti saatavilla olevalla lukijalla/lukijalaitteella.

### Kuinka se tehtiin:

He löysivät kortin salasanan (joka oli sama kaikille korteille) ja käyttivät sita kirjoittaakseen dataa korttiin. Tämä mahdollisti kloonauksen, mutta heidän mielenkiintoisin havaintonsa oli kyky muuttaa kortin näyttämää nimeä. He muuttivat omansa näyttämään "Älä Trusta Minua" ("Don't Trust Me"), korostaen, että vaikka fyysinen pääsy oli mahdollista, korttia ei voinut luottaa digitaalisesti.

### Miksi mielenkiintoinen? 

Tapahtuma korostaa turvallisuuskettejä heikointa lenkkiä: ihmistä ja hänen fyysistä korttiaan. Vaikka järjestelmä itsessään oli turvallinen, kortin kloonaaminen antoi pääsyn fyysiseen tilaan. Se on klassinen esimerkki siitä, miten RFID-turva rikotaan usein sosiaalisen tekniikan ja matalan tason laitetekniikan yhdistelmällä.

Voit lukea koko artikkelin näin: päivitä sivu, paina heti perään CTRL + A ja sitten CTRL + C. Liitä teksti vaikka muistioon (Notepad) ja lue se sieltä. :D https://www.wired.com/story/hid-keycard-authentication-key-vulnerability/

## Lähteet

https://www.wired.com/story/hid-keycard-authentication-key-vulnerability/,

https://chat.deepseek.com/,

https://bishopfox.com/tools/rfid-hacking-2/attack-tools
