# H5 Mininet | Nathaniel Ssendagire 23.11.2025

## Ympäristö

OS: Linux Ubuntu

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: mininet SSH with Windows Subsystem for Linux

Processor: AMD Ryzen 7 5700x - 6 cores used

Disk: 100 GB

Network: NAT

## EvilGinx2

## Sovelluksen asennus

Aloitin lataamalla Evilginx2:n GitHubista git clone https://github.com/kgretzky/evilginx2.git


<img width="970" height="78" alt="purged the old go" src="https://github.com/user-attachments/assets/0a40f9c0-6c25-4ed4-bb96-c584556379a2" />

Ensimmäinen ongelma: Go-kielen versio oli liian vanha. Jouduin poistamaan vanhan version ja asentamaan uuden:

    # Vanhan version poisto
    sudo apt purge golang-go

    # Uuden Go-version asennus
    - wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz
    - sudo tar -C /usr/local -xzf go1.21.6.linux-amd64.tar.gz
    - export PATH=/usr/local/go/bin:$PATH

<img width="1083" height="441" alt="downloaded up to date go" src="https://github.com/user-attachments/assets/6dc9ed0b-520e-4609-8ad5-48cc110e30f6" />

<img width="823" height="457" alt="updated the vendor and built the evilginx" src="https://github.com/user-attachments/assets/ed635480-9159-4e59-9e6f-25fca475f2c1" />

Seuraava ongelma: Vendor-kirjastot eivät olleet ajan tasalla. Jouduin päivittämään ne:

    go mod vendor
    go mod tidy

Sitten käänsin Evilginx2:n :

    go build -o evilginx2

## Työkalun Testaus

Käynnistin Evilginx2:n developer-moodissa:

    sudo ./evilginx2 -developer

<img width="1106" height="539" alt="starting evilginx" src="https://github.com/user-attachments/assets/00245e55-497f-4105-a428-06871a4da9bb" />


## Tutkittiin mitä komentoja sieltä löytyy

Käytin help-komentoa tutkiakseni saatavilla olevia komentoja ja niiden toimintoja:

    help
    help config
    help phishlets

<img width="962" height="552" alt="ran the help command to see what i can do" src="https://github.com/user-attachments/assets/edce173f-4c44-428c-861d-9479853db068" />

## Testi

<img width="918" height="553" alt="image" src="https://github.com/user-attachments/assets/fe30bfb3-9c74-48bb-bc89-40896bbe1288" />

Testasin komentoja turvallisesti käyttämällä vain localhost-IP-osoitetta, koska oikeiden verkkotunnusten käyttö olisi eettisesti väärin ja laiton:
    
    config domain localhost
    config ipv4 external 127.0.0.1

Kun testasin generoitua linkkiä, se ei toiminut - mikä oli odotettua, koska työkalu on suunniteltu oikeille verkkotunnuksille.

## Phishletien Tutkiminen

Kiinnostuin phishleteistä, jotka näyttivät olevan sivustokonfiguraatioita. Tarkastelin esimerkkiphishletiä:

<img width="912" height="423" alt="image" src="https://github.com/user-attachments/assets/6e74d53e-f507-4b9c-bd14-eec708cd0bfb" />

Kopioin osia esimerkistä ja loin oman mukautetun phishletin, joka ilmestyi phishlet-listaan:

<img width="1116" height="548" alt="updated localtest" src="https://github.com/user-attachments/assets/abd33ac1-d9a7-47c7-b4c2-24c10857d100" />

<img width="618" height="178" alt="image" src="https://github.com/user-attachments/assets/cf9ef791-af63-40e6-8882-efa1f3eeb1e4" />

<img width="622" height="116" alt="image" src="https://github.com/user-attachments/assets/0ad076b2-6fbd-4188-bb24-4a4a0a89c2b3" />

## Yhteenveto

Oma phishletini näkyi onnistuneesti phishlet-listauksessa, mikä osoitti että ymmärsin phishletien perusrakenteen ja pidin konfiguraatiot turvallisina localhost-ympäristössä.

Tärkeää: Kaikki testaus suoritettiing ainoastaan omaan localhost-ympäristöön eettisistä syistä - en käyttänyt oikeita verkkotunnuksia enkä yrittänyt huijata ketään.

## TCP SYN-Flood hyökkäys

### 1. Mininet-ympäristön Alustus

Ryu Controllerin Käynnistys: 

    ryu-manager ryu.app.simple_switch_13



Mitä tehtiin: Käynnistettiin OpenFlow-säätöohjelma, joka hallinnoi verkon kytkentäelementtejä ja reititystä.

Mininet-verkoston Luonti:

    sudo mn --topo single,3 --mac --switch ovsk --controller remote

Mitä tehtiin: Luotiin virtuaaliverkko, jossa on:

    3 hostia (h1, h2, h3)

    1 kytkin (s1)

    Etäsäätö RemoteControllerilla

    Staattiset MAC-osoitteet konfiguraation helpottamiseksi


<img width="592" height="524" alt="remote pc terminal" src="https://github.com/user-attachments/assets/2ec6c03b-a4b8-4952-9c8c-91654fbcec45" />

### 2. Verkon Toiminnan Varmistus

Konnektiivisuuden Testaus
    
    mininet> pingall
    *** Results: 0% dropped (6/6 received)

Mitä tehtiin: Testattiin kaikkien hostien välistä yhteyttä. 0% häviötä vahvisti, että verkko oli täysin toiminnassa ja kaikki hostit pystyvät kommunikoimaan keskenään.

<img width="416" height="116" alt="pingall command " src="https://github.com/user-attachments/assets/978c2915-56ed-4502-a7ef-e3fc5d3e15a1" />


### 3. Kohdepalvelimen Käyttöönotto

HTTP-palvelimen Käynnistys

    h1 python3 -m http.server 80 &

Mitä tehtiin: Hostissa h1 käynnistettiin Pythonin sisäänrakennettu HTTP-palvelin porttiin 80. '&'-merkki ajoi palvelimen taustalle, jotta Mininet-konsoli pysyi käytettävissä.

Palvelimen Toiminnan Varmistus

    h2 curl http://10.0.0.1

Mitä tehtiin: Hostista h2 testattiin HTTP-yhteyttä kohteeseen 10.0.0.1 (h1). Onnistunut HTML-dokumentin palautus varmisti, että palvelin vastasi normaalisti ennen hyökkäystä.

<img width="867" height="538" alt="image" src="https://github.com/user-attachments/assets/bf5ee9df-2aac-4e39-96a4-f951fc9f38f1" />

### 4. SYN-Flood Hyökkäyksen Toteutus

Hyökkäyksen Käynnistys

    h2 hping3 -S --flood -p 80 10.0.0.1 &

Mitä tehtiin: Hostissa h2 käynnistettiin SYN-Flood hyökkäys:

- -S: Lähetettiin ainoastaan SYN-paketteja (TCP-yhteydenavauksia)

- --flood: Lähetettiin paketteja mahdollisimman nopeasti ilman odotusaikoja

- -p 80: Kohdistettiin porttiin 80 (HTTP-palvelin)

- 10.0.0.1: Kohdehosti h1

- &: Suoritettiin taustalla

Hyökkäyksen Monitorointi

    h2 ps aux | grep hping3

Tulokset:

    root        6649 98.4% CPU - hping3 -S --flood -p 80 10.0.0.1

Mikä tarkastettiin: Varmistettiin, että hyökkäysprosessi oli aktiivisessa tilassa ja käytti 99.0% CPU:sta, mikä osoitti hyökkäyksen olevan täydessä käynnissä.

<img width="913" height="139" alt="image" src="https://github.com/user-attachments/assets/0db74d67-6d66-4a1b-a6c7-5de2b704c720" />


## Xterm ongelma

Larin laatimassa pdf tiedostossa liittyen tähän tehtävään oli selkeet ohjeet miten saada xterm toimimaan:

        Jos Xterm auheuttaa ongelmia niin
        2. Aja scripti ./get_xauth.sh
        Se palauttaa jotain tämän näköistä
        mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 22ce67f9c6514c99d2903e2b9d97e496
        3. Kopioi tämä ja aja seuraava komento
        sudo -s xauth add mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 22ce67f9c6514c99d2903e2b9d97e496
        4. Tämän jälkeen xtermin pitäisi toimia mininetissä.

### 5. Reaaliaikainen Pakettien Visualisointi

    xterm h1
    # Ja sitten h1-terminaalissa:
    tcpdump -i h2-eth0 -n "tcp[tcpflags] == tcp-syn and dst host 10.0.0.1"

<img width="523" height="407" alt="image" src="https://github.com/user-attachments/assets/345fa17c-4774-485b-b613-9cc44575c3d0" />


Mitä havaittiin: Nähtiin reaaliaikaisesti kuinka tuhansia SYN-paketteja saapui sekunnissa kohdehostiin h1 porttiin 80. Tämä visuaalinen havainnollistus vahvisti hyökkäyksen massiivisen luonteen.



<img width="225" height="47" alt="packets captured" src="https://github.com/user-attachments/assets/1b5ec4e9-d8cf-4c12-b38c-5c8f303375e3" />


<img width="609" height="730" alt="the packet flood in action" src="https://github.com/user-attachments/assets/28eec388-35a6-4535-a197-88c3af8e7416" />

SYN-Flood hyökkäyksiä ehkäistään parhaiten monipuolisella lähestymistavalla: palvelimilla käytetään SYN cookies -tekniikkaa, joka estää yhteyksien muistin täyttymisen; verkkolaitteissa konfiguroidaan rate limiting rajoittamaan SYN-pakettien määrää; palomuurit suodattavat epäilyttävän liikenteen; ja ISP-tasolla käytetään blackholingia haitallisen liikenteen kohdentamiseksi. Yhdistelmä näistä menetelmistä luo robustin puolustuksen, joka kykenee torjumaan jopa laajamittaisia DDoS-hyökkäyksiä.


## Lähteet

https://www.sciencedirect.com/science/article/pii/S2352340925000460, 

https://github.com/kgretzky/evilginx2, 

https://hhmoodle.haaga-helia.fi/pluginfile.php/4347169/mod_resource/content/1/03-mininet.pdf, 






