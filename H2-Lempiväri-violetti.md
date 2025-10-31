# H2 Lempiväri-Violetti | Nathaniel Ssendagire 30.10.2025

## Ympäristö

OS: Kali Linux

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

Processor: AMD Ryzen 3 7320U - 4 cores used

Disk: 35 GB

Network: NAT

## Tiivistys

## Pyramid Of Pain
Tuskan pyramidi kuvaa eri tasoisia hyökkäyksen tunnisteita (esim. hashit, IP-osoitteet, domainit, työkalut, TTP:t) ja sitä, kuinka paljon “kipua” niiden estäminen aiheuttaa hyökkääjälle. Mitä korkeammalle pyramidissa mennään (kohti TTP:itä), sitä vaikeampaa ja kalliimpaa hyökkääjän on mukautua.

## Diamond Model
Timanttimalli esittää kyberhyökkäyksen neljän keskeisen elementin. Hyökkääjän (adversary), kyvykkyyden (capability), infrastruktuurin (infrastructure) ja uhrin (victim), väliset suhteet, joita analysoimalla voidaan ymmärtää ja jäljittää hyökkäyksen kokonaiskuva.


## A) Apache log

<img width="583" height="85" alt="apache2 installation" src="https://github.com/user-attachments/assets/992e0384-427b-431d-bd8d-7e205d16b924" />

apache2 lataus

<img width="645" height="173" alt="enabling apache2" src="https://github.com/user-attachments/assets/5aaf1aa4-b07f-4aa1-b1de-216a46d8af9a" />

apache2 käynnistys

<img width="397" height="272" alt="curling apache2 from terminal 200ok" src="https://github.com/user-attachments/assets/52b36f44-be7b-492c-8b6d-2eca660caa1e" />

Tämä tuloste kertoo, että Apache-palvelin toimii oikein ja vastaa HTTP-pyyntöihin. Se antaa perustiedot sivusta, kuten sen koon, muokkausajan ja tyypin. Komento on hyödyllinen palvelimen tilan tarkistamiseen ilman että koko sivua tarvitsee ladata.

<img width="660" height="76" alt="log from apache2" src="https://github.com/user-attachments/assets/e2afe468-a773-415e-976f-9fb4ca93c87a" />

Apache-lokirivi kertoo, että koneeni teki HTTP HEAD -pyynnön juurisivulle  klo 16:56:10, ja palvelin vastasi onnistuneesti. Käytin curl -komentoa, joten käyttäjäagenttina näkyy curl/8.13.0. Tämä on hyvä esimerkki siitä, miten Apache tallentaa jokaisen HTTP-pyynnön lokiin.

## B) Nmapped

<img width="650" height="471" alt="portscan using nmap" src="https://github.com/user-attachments/assets/5644af27-589f-435d-8024-6576084366c9" />

## Mitä komento tekee
- nmap -A: Ajaa laajan skannauksen, joka sisältää:
- Palvelun tunnistuksen
- Käyttöjärjestelmän tunnistuksen
- Skriptien ajon (NSE)
- Tracerouten (reitityksen)
- -p 80: Skannaa vain portin 80, joka on HTTP-palveluiden oletusportti.
- localhost: Kohteena on oma kone.
- -oN nmap_A_localhost.txt: Tallentaa tulokset tiedostoon.

## Skannauksen tulokset
- Host is up: Palvelin vastaa, eli se on käynnissä.
- Portti 80/tcp on auki: HTTP-palvelu toimii.
- Palvelu: Apache HTTP Server, versio 2.4.65 (Debian).
- Sivun otsikko: "Apache2 Debian Default Page: It works" → Tämä on Apache-palvelimen oletussivu.
- HTTP-header: Paljastaa palvelimen version ja käyttöjärjestelmän.
- OS-tunnistus: Nmap arvioi, että kone käyttää Linuxin versiota 2.6.x tai 5.x.
- Etäisyys: 0 hops → Skannaus tehtiin paikallisesti (ei verkon yli).
- Varoitus: OSScan ei ole täysin luotettava, koska vain yksi portti oli auki eikä yhtään suljettua → OS-tunnistus toimii paremmin, kun on useita portteja.

Skannaus paljasti, että oma koneeni pyörittää Apache-verkkopalvelinta portissa 80, ja palvelin vastaa HTTP-pyyntöihin normaalisti. Nmapin laaja skannaus tunnisti palvelun version, sivun otsikon ja arvioi käyttöjärjestelmän. Koska skannaus tehtiin vain yhdelle portille, OS-tunnistus ei ole täysin tarkka.

## C) Skriptit
<img width="355" height="244" alt="eeeeee" src="https://github.com/user-attachments/assets/6784a253-5231-44c6-84fc-3a3828f8c402" />

Automaattiset skriptit, jotka olivat päällä ovat: http-title, http-server-header ja http-headers.

## D) Jäljet lokissa

<img width="1657" height="461" alt="command to check all access logs" src="https://github.com/user-attachments/assets/8cffbf03-15ff-4760-98a6-e50d32403ace" />

Ensiksi etsin apache2 access logista lokeja jotka sisältää sanan nmap, jos ei ole se tulostaa "No direct nmap character"

<img width="727" height="91" alt="ip based scan" src="https://github.com/user-attachments/assets/652a054a-75df-4698-8eca-db449bf16a5d" />

## Mitä kukin osa tekee:

- sudo — aja komento järjestelmänvalvojan oikeuksin (tarvitaan, koska lokit ovat root-oikeuksin).

- awk '{print $1}' /var/log/apache2/access.log* — tulostaa jokaisesta lokirivistä ensimmäisen kentän (Apache-access.log-muodossa ensimmäinen kenttä on yleensä asiakkaan IP).

- sort — lajittelee kaikki IP-osoitteet aakkosjärjestykseen (tarvitaan uniq-komennon käyttöä varten).

- uniq -c — laskee peräkkäisten, samanlaisten rivien esiintymät (eli laskee kunkin IP:n pyyntöjen määrän).

- sort -nr — lajittelee tuloksen numeerisesti laskevaan järjestykseen (suuremmasta pienempään).

- head — näyttää ensimmäiset rivit (yleensä top-10, tässä oletus 10).

## Selitys:

- 127.0.0.1 on tehnyt 28 pyyntöä (yhteensä tutkituissa access.log* -tiedostoissa).

- ::1 (IPv6 loopback) on tehnyt 1 pyynnön.

- Tämä vahvistaa, että eniten liikennettä on tullut localhostista (odotettavaa, kun skannasit paikallista palvelinta).

## Mitä tästä tulisi ymmärtää:

- Porttiskannaukset ja NSE-skriptit usein aiheuttavat suuria määriä pyyntöjä lyhyessä ajassa samalta IP:ltä (tässä localhost). Tämä käsky on hyvä nopea tapa löytää mahdollinen skannaaja /      melun lähde lokeista.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<img width="822" height="207" alt="user-agent found in the command" src="https://github.com/user-attachments/assets/e488b9d1-8174-4749-ac9f-ca3b86da4a75" />

Kyseessä on paketin dekoodattu HTTP-request. Siitä näkyy metodi, URI ja otsakkeet kuten Host, Depth (PROPFIND), User-Agent.

Tämä antaa varman todisteen siitä, että skanneri lähetti kyseisen User-Agent-kentän — toisin sanoen nmap esiintyi oikeasti HTTP-pyynnössä.

## E) Wire sharking

Tässä tehtävässä siepattiin verkkoliikennettä tcpdump-komennolla samalla kun ajettiin Nmap-skannaus paikalliseen palvelimeen porttiin 80. Tavoitteena oli tallentaa skannauksen aikana syntyvä liikenne ja nähdä, mitä Nmap lähettää HTTP-palvelimelle.

## Komentojen selitys
– sudo tcpdump -i lo -w nmap_scan_lo.pcap port 80 &
Tämä käynnistää tcpdumpin taustalle ja tallentaa kaiken portin 80 liikenteen loopback-liitännästä (lo) tiedostoon nmap_scan_lo.pcap.
– sudo nmap -A -p 80 localhost
Tämä ajaa Nmapin täydellisen skannauksen (-A) porttiin 80 paikallisessa koneessa. Se tunnistaa palvelun version, mahdolliset skriptit ja HTTP-otsikot.
Skannauksen tuloksena saadaan:
- Palvelu: Apache httpd 2.4.65 (Debian)
- HTTP-otsikko: Apache/2.4.65 (Debian)
- Sivun otsikko: Apache2 Debian Default Page: It works
– sudo pkill -f "tcpdump -i lo"
Tämä lopettaa tcpdumpin, kun skannaus on valmis.
– 335 packets captured
Tcpdump sai talteen 335 pakettia.

Yksi HTTP-pyyntö, jonka Nmap lähetti:
```
GET /nmaplowercheck1761824260 HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)

```
Tämä kertoo, että Nmap lähettää HTTP-pyynnön, jossa on:
- Erikoisosoite /nmaplowercheck... — Nmap käyttää tätä tunnistaakseen palvelimen vasteen.
- User-Agent: Nmapin oma tunniste, joka paljastaa että kyseessä on Nmapin skriptimoottori.







<img width="581" height="368" alt="pcap file creation and listening and recording packets" src="https://github.com/user-attachments/assets/6b384bc9-3538-4d35-a55c-a9f87e9b0cec" />

<img width="552" height="212" alt="killed the process" src="https://github.com/user-attachments/assets/7326d368-2514-4ee2-9ed4-3f038cedba88" />

<img width="817" height="186" alt="useragent v2" src="https://github.com/user-attachments/assets/cb34813b-aac3-48e7-9c0f-b1fec9a5bf43" />

## F) Net grep

<img width="597" height="103" alt="nmap grep" src="https://github.com/user-attachments/assets/711d9274-2bbf-42cb-8ff6-5c19ccf92b77" />

Koska unohdin aluksi ajaa komennon ennen kuin tein Nmap-skannauksen, en ehtinyt siepata sitä liikennettä, jossa sana "nmap" olisi näkynyt. Myöhemmin, kun tein skannauksen "stealth"-asetuksilla (eli piilotin käyttäjäagentin), Nmap ei enää lähettänyt liikenteessä sanaa "nmap". Siksi ngrep ei löytänyt mitään osumaa, ja näytti vain risuaitoja, joka tarkoittaa että se kyllä näkee liikennettä, mutta ei mitään mikä vastaisi hakusanaa.

## G) Agentti
Käytin tätä kirjaa apuna komennon kirjoittamiseen: https://learning.oreilly.com/library/view/nmap-network-exploration/9781786467454/62ae3cc1-af7b-4046-89c1-a6eaa6c0b759.xhtml

<img width="1207" height="101" alt="command to make the nmap look normal" src="https://github.com/user-attachments/assets/11868b60-8d53-47c8-97ae-7b6f93809735" />


## Mitä komento tekee?
Komento käyttää Nmap-työkalua skannaamaan paikallisen koneen porttia 80, joka on HTTP-verkkopalveluiden oletusportti. Se käyttää kolmea skriptiä: http-title, joka hakee verkkosivun otsikon, http-headers, joka näyttää HTTP-vastauksen otsakkeet, ja http-server-header, joka paljastaa palvelimen ohjelmiston tiedot.

Skriptien mukana annetaan lisäparametri http.useragent , joka määrittää käyttäjäagentin eli sen, miltä selain näyttää palvelimelle. Tässä käytetään Chrome-selaimen käyttäjäagenttia Windows 10:llä, jotta palvelin vastaisi kuin oikealle selaimelle.

Lopuksi -oN nmap_custom_ua.txt tallentaa skannauksen tulokset tiedostoon nimeltä nmap_custom_ua.txt normaalissa tekstimuodossa.


<img width="1207" height="101" alt="command to make the nmap look normal" src="https://github.com/user-attachments/assets/fd94dee6-e816-421e-bbf7-afb16cfef01a" />

## H) Pienemmät jäljet

Komennon suorituksen jälkeen, analysoitiin 20 riviä apache2:n access.log tiedostoa, jossa sitten näkyi selkeästi, ettei nmappia enään näkynyt.

<img width="1197" height="437" alt="output of command that make nmap look normal" src="https://github.com/user-attachments/assets/caa64eed-0cfe-4315-8eb3-5232e3b48dbe" />


<img width="1198" height="775" alt="command to listen and scan and kill the wireshark processs thing" src="https://github.com/user-attachments/assets/ffe0df7b-34f9-4603-909e-d0d80c96f9d2" />

<img width="1181" height="527" alt="output of the ninja command" src="https://github.com/user-attachments/assets/36ff7af2-af14-46ee-b7a2-a05265477966" />

<img width="760" height="143" alt="analyzing it with tshark" src="https://github.com/user-attachments/assets/39e6fcd9-1151-4dcb-82e9-ffabeeaf743f" />

Tämä komento analysoi kaapatun verkkoliikenteen pcap-tiedostoa ja etsii HTTP-pyynnöissä käytettyjä käyttäjäagentteja (eli selaimen tunnistetietoja).

– tshark -r /tmp/nmap_custom_ua.pcap: Avaa pcap-tiedoston, joka sisältää verkkoliikenteen tallenteen.

– -T fields -e http.user_agent: Tulostaa vain käyttäjäagentti-kentän jokaisesta HTTP-pyynnöstä.

– | sort | uniq -c: Järjestää käyttäjäagentit aakkosjärjestykseen ja laskee, kuinka monta kertaa kukin esiintyy.

## I) Hieman vaikeampi

## Skriptin valinta ja muokkaus

<img width="473" height="68" alt="the script we modified" src="https://github.com/user-attachments/assets/560863c1-3e35-46d3-9800-1ec1a8750c75" />

Valitsin muokattavaksi skriptin http-devframework.nse. Löysin tietoa skriptistä näiltä sivuilta: https://www.infosecmatter.com/nmap-nse-library/?nse=http-devframework, https://seclists.org/nmap-dev/2013/q3/448





Avasin Nmapin kirjastoista tiedoston nselib/http.lua ja etsin siellä määrittelyn, joka asettaa oletus‑User‑Agent‑otsakkeen (USER_AGENT). Korvasin alkuperäisen merkkijonon (jossa esiintyi “Nmap Scripting Engine”) tavallisella selaimen User‑Agentilla (esim. Mozilla/Chromium‑muotoinen merkkijono). Tallensin muutokset ja loin varmuuskopion alkuperäisestä tiedostosta ennen muokkausta.

<img width="1375" height="55" alt="user-agent text change script" src="https://github.com/user-attachments/assets/86724e45-b6d7-4384-ad7d-b602ad5b5d48" />

## Testiskannaus ja lokitarkastus
Suoritin porttiskannauksen kohteena localhost (127.0.0.1) käyttäen HTTP‑skriptejä, kuten http-title. Tarkastin Apache‑palvelimen access.log‑tiedoston. Aiemmin skannauksissa esiintynyt User‑Agent‑kentän teksti “Nmap Scripting Engine” ei enää näkynyt; lokiriveissä näkyi ainoastaan selain‑tyylinen User‑Agent tai "-".

<img width="1537" height="553" alt="portscan output working no nmap is being displayed" src="https://github.com/user-attachments/assets/3b7f2f57-a43a-421c-8898-f767b80c8443" />

## Sieppaus ja protokollan analyysi (Wireshark / pcap)
Sieppasin loopback‑liikenteen tcpdumpilla ja avasin tallenteen Wiresharkissa. HTTP‑pyyntöjen Hypertext Transfer Protocol ‑osiosta tarkastin User-Agent‑otsakkeet. Kaikissa paketeissa User‑Agent oli selain‑tyylinen eikä sisältänyt merkkijonoa “nmap”.

<img width="827" height="235" alt="didnt find nmap" src="https://github.com/user-attachments/assets/215ee0bb-0895-4bd2-bd5f-b25d7536b0b4" />
<img width="1278" height="763" alt="wireshark capture" src="https://github.com/user-attachments/assets/25df6d50-3f94-42bb-b53c-b14e08c84240" />

## Vahvistus lisäskannauksilla
Suoritin toistuvia porttiskannauksia ja toistin loki‑ sekä pcap‑tarkastukset. Tulokset osoittivat johdonmukaisesti, että sana „nmap” ei enää näy missään Apache‑lokeissa eikä siepatussa verkkoliikenteessä.




<img width="1186" height="372" alt="doing port scan with uptime " src="https://github.com/user-attachments/assets/c1f80b37-cf5e-4dc8-bf4b-996aa94393df" />

<img width="1586" height="830" alt="evidence of no nmap showing" src="https://github.com/user-attachments/assets/778f48db-d42e-4d6e-96d0-6ab2dcae4f45" />

## Lähteet


- <a href="https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#h2-lempivari-violetti"> lempivari-violetti.</a>,
- <a href="https://learning.oreilly.com/library/view/nmap-network-exploration/9781786467454/8bd39499-1b35-4df4-befb-1c9a6772db6e.xhtml">Nmap: Network Exploration and Security Auditing Cookbook </a>,
- <a href="http://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html">Pyramid of Pain</a>,
- <a href="https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf">Cyber Kill Chain</a>,
- <a href="https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf">Diamond Model</a>,
- <a href="https://attack.mitre.org/">ATT&CK</a>,
