## Harjoitus 6 ( Wifi Challenge ) | Nathaniel Ssendagire 16.11.2025   

## Ympäristö

OS: Linux Debian

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: VirtualBox: 11.0 GB

Processor: AMD Ryzen 7 5700x - 6 cores used

Disk: 100 GB

Network: NAT

## Harjoitus 1

Tehtävä: What is the channel that the wifi-global Access Point is currently using? 

<img width="1157" height="435" alt="wifi global access point challenge " src="https://github.com/user-attachments/assets/d7b41b2e-2b53-4165-9d7f-46ae20eb45e7" />

Komennolla sudo airodump-ng --band a --essid "wifi-global" wlan0mon rajoitetaan skannaus 5 GHz:n taajuusalueelle (--band a) ja etsitään näkyviin vain wifi‑global‑nimisen verkon tiedot (--essid). Tehtävän tarkoituksena oli selvittää, millä kanavalla kyseinen Access Point toimii. Komennon tuloksesta nähtiin, että wifi‑global operoi kanavalla 44.

## Harjoitus 2

Tehtävä: What is the MAC of the wifi-IT client? 

Tehtävässä piti selvittää wifi-IT ‑verkon asiakkaan (clientin) MAC‑osoite. Ensiksi löydettiin verkon BSSID eli sen Access Pointin MAC‑osoite. Sitten käytettiin tätä samaa BSSID:tä airodump‑ng:n taulukossa ja katsottiin, missä rivissä se esiintyy. Rivin vieressä, jossa lukee STATION, näkyvä MAC‑osoite on kyseisen asiakkaan MAC‑osoite.

<img width="1007" height="797" alt="wifi-it mac address" src="https://github.com/user-attachments/assets/fec4e261-cfa8-4729-af45-2051c5b70960" />


## Harjoitus 3

Tehtävä: What is the probe of 78:C1:A7:BF:72:46? 

Tehtävässä tuli selvittää, mitä verkkoa MAC-osoite 78:C1:A7:BF:72:46 etsii. Tämä tapahtui etsimällä airodump-ng-näkymästä kyseinen MAC-osoite STATION-listasta ja katsomalla sen riviltä Probes-sarakkeen arvoa. Probes-sarake kertoo, mitä verkkoa kyseinen laite yrittää etsiä tai mihin se on aiemmin ollut yhteydessä. Tästä sitten selvisi, että se on "wifi-offices"

<img width="872" height="29" alt="challenge 3 find the probe" src="https://github.com/user-attachments/assets/1286d7b6-f60a-4986-9dfa-f427e2cc2e16" />


## Harjoitus 4

Tehtävä: What is the ESSID of the hidden AP (mac F0:9F:C2:6A:88:26)? 

Kun komennolla iwconfig wlan0 channel 11 lukitaan langaton verkkokortti kanavalle 11, se alkaa kuunnella ja lähettää liikennettä ainoastaan kyseisellä taajuudella. Tämä on tärkeää esimerkiksi MDK4‑työkalua käytettäessä, koska kohdeverkon majakkakehykset (beacon‑kehykset) lähetetään vain yhdellä, verkkokantapisteen käyttämällä kanavalla. Jos verkkokortti kuuntelisi väärää kanavaa tai hyppisi kanavien välillä, kohdeverkon MAC‑osoitetta ei löydettäisi yhtä nopeasti tai tasaisesti. Kanavan lukitseminen oikeaan taajuuteen varmistaa, että työkalu vastaanottaa kaikki tarvittavat kehykset heti ja pystyy suorittamaan hyökkäyksen nopeammin ja tehokkaammin ilman viivettä, mikä näkyy suurempana pakettinopeutena ja parempana tarkkuutena.

<img width="473" height="295" alt="switching the channel" src="https://github.com/user-attachments/assets/a3a2bf76-b7e9-4901-b300-456b39ada268" />

## Bruteforce  

<img width="771" height="357" alt="bruteforcing " src="https://github.com/user-attachments/assets/e25e4974-34e3-4441-bc99-5bb63a453f29" />

En saanut ESSID:tä. Olin ladannut rockyou.txt myös mutta siinä kesti niin kauan, että päätin vain jättää sen tekemättä, mutta tärkein on, että nyt tiedän miten se toimii :D.

## Mitä komento tekee? 

MDK4 on työkalu, jota voidaan käyttää erilaisten Wi‑Fi‑verkkojen testaamiseen ja esimerkiksi SSID‑bruteforce‑hyökkäyksiin. Tässä komennossa kerromme työkalulle, että sen tulee suorittaa bruteforce‑toimintaa MAC‑osoitteeseen, johon liitytään wlan0‑liitännän kautta.

- p tarkoittaa probe‑tilaa, eli MDK4 lähettää probe‑pyyntöjä testatakseen tukiaseman (AP:n) vakautta. Probe‑pyynnöt ovat Wi‑Fi‑viestejä, joilla laite kysyy ympäristöstä: “Onko täällä verkkoja?” Näiden avulla voidaan myös paljastaa piilotettuja SSID‑nimiä.
  
- -t tarkoittaa target, eli kohdetta – tässä tapauksessa MAC‑osoitetta F0:9F:C2:6A:88:26.
- -f tarkoittaa file eli tiedostoa; tässä käytin sanalistaa rockyou-top100000.txt bruteforce‑yrityksiä varten.

Komento 


Seuraavaksi 

## Harjoitus 7

Tehtävä: What is the flag on the wifi-old AP website?

Tässä tehtvässä käytin besside-ng komentoa. Tämä komento hankkii tietyn verkon avaimen.

<img width="1204" height="653" alt="key cracked" src="https://github.com/user-attachments/assets/0fe6be63-bd14-4c0a-9e99-e110a9079b57" />

Tämän jälkeen sammutettiin interface ja laitettiin se monitor moodista managed moodiin jotta voidaan ottaa yhteyttä verkkoon.

<img width="511" height="70" alt="setting the int to managed instead of monitor" src="https://github.com/user-attachments/assets/3135bbc2-d127-49a6-b257-ce520a11a89b" />

Seuraavaksi otettiin yhteys wifi-old verkkoon. Salasana on se avain jonka saimme besside-ng komennolla, mutta vain ilman niitä kaksoispisteitä välissä.

<img width="820" height="96" alt="connecting to wifi-old" src="https://github.com/user-attachments/assets/4f09285a-4026-446d-b050-dceb04db1572" />

Saatiin yhteys verkkoon wlan0 liitäntään. Käytin ip route komentoa nähdäkseni, mikä on se wlan0 liitännän ip osoite.

<img width="746" height="176" alt="image" src="https://github.com/user-attachments/assets/9c18242f-e81c-4cec-8417-e8624385d3e0" />

Kirjoitin wlan0 liitännän ip osoitteen selaimeen ja tadaa saatiin lippu. Testasin ihan perus admin admin salasanaa ja pääsin sillä sisään.

<img width="1077" height="676" alt="we cracked it" src="https://github.com/user-attachments/assets/d5811dbb-36db-4073-a668-37477864302a" />

<img width="1073" height="454" alt="admin admin" src="https://github.com/user-attachments/assets/c7ccfd14-218b-4830-b182-8638bcbe4759" />


## Mitä opin

## Lähteet

