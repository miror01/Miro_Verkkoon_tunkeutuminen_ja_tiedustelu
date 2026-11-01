## x) Tiivistelmä

### Karvinen 2025: Wireshark - Getting Started

* Wireshark on pakettianalysaattoriohjelma, jolla voidaan analysoida ja kaapata verkkoliikenteessä kulkevia paketteja.
* Sitä voidaan käyttää muun muassa ongelmien vianmääritykseen, verkkoprotokollien opiskeluun, ja tietoturva-analysointiin.
* Tarkastelu toimii valitsemalla haluttu verkkoliitäntä, jonka jälkeen verkossa kulkevia paketteja voi tarkastella ja lukea.


### Karvinen 2025: Network Interface Names on Linux

* Network interface tarkoittaa verkkoliitäntää, joka on kuin verkkokortti, jonka kautta verkkoliikenne kulkee. Se voi olla fyysinen tai virtuaalinen.
* Verkkoliitäntöjen nimet kertoo usein verkkoliitännän tyypistä. Kaikissa koneissa on esimerkiksi `lo`, eli loopback liitäntä, jolla `localhost`-yhteys muodostetaan.
* Muita etuliitteitä ovat muun muassa `en`, eli Ethernet-yhteys, ja `wl`, joka tarkoittaa WLAN:ia. Esimerkkejä tällaisista verkkoliitäntöjen nimistä ovat `wlp4s0`, ja `enp1s0`.
* Verkkoliitäntöjen, ja niiden IP-osotteiden tarkasteluun, voidaan käyttää Linuxissa vaikkapa komentoa `ip a`.


## a) Linux.
Debian sekä Kali asennettuna jo Virtualboxissa. Käytän tällä hetkellä kurssin tehtäviin Kali Linuxia.

## b) Ei voi kalastaa
Verkkoyhteyden katkaiseminen on tuttua hommaa jo aiemmilta kursseilta, ja hyvin yksinkertaista. Aloitin tehtävän testaamalla verkkoyhteyttä komennolla `ping 1.1.1.1`. Paketit meni kohteeseen, ja packet lossia ei ollut joten verkkoyhteys on päällä. Seuraavaksi katkaisin verkkoyhteyden menemällä VirtualBoxin asetuksiin, ja napsautin Network-kohdasta adapterin tilan `Not attached`.

<img width="430" height="217" alt="image" src="https://github.com/user-attachments/assets/25eb090c-158d-4170-ac03-ef3b45c3a054" />


Testasin nyt aiempaa `ping`-komentoa uudestaan, ja packet loss oli 100%. Tästä näemme, että paketit eivät enää mene läpi, joten verkkoyhteys on katkaistu oikein. Verkkoyhteyden palauttamiseen vaihdoin tilan Network-asetuksista takaisin tilaan `NAT`. Pingaus toimii taas.

<img width="524" height="411" alt="image" src="https://github.com/user-attachments/assets/aa91e23a-8c14-4a64-ae05-2257949c8e79" />

## c) Wireshark

Wiresharkin voi asentaa linuxissa komennolla `sudo apt install wireshark`. Minulla on wireshark kalissa jo asennettuna, ja käyttäjäni on lisättynä wireshark-ryhmään, joten käynnistin wiresharkin kalissa yksinkertaisella komennolla `wireshark`. Valitsin verkkoliitännän `eth0`, ja loin verkkoon liikennettä. Pingasin googlea komennolla `ping 8.8.8.8`, ja pystyin tarkastelemaan Wiresharkista paketteja.

<img width="574" height="392" alt="wireshark" src="https://github.com/user-attachments/assets/90d6779d-fa40-4fe4-91d2-c23d5b5468de" />

## d) Oikeesti TCP/IP

Pingasin uusiksi komennolla `ping google.com`, jotta DNS-palvelinta joudutaan käyttämään selvittääkseen, mikä ip-osoite google.com:lla on. Valitsin DNS-paketin ja tarkastelin sen tietoja.

TCP/IP-pinossa on neljä kerrosta, ja kun tehdään DNS-pyyntö, nämä kaikki on helposti eroteltavissa ja selitettävissä.

<img width="959" height="385" alt="wireshark2" src="https://github.com/user-attachments/assets/6863f9e2-0442-4164-be26-c70b46a4504b" />

Kuvassa laatikoituna näkyy protokollat, joita on käytetty. 
* TCP/IP-pinon ensimmäistä kerrosta, verkkoliitäntäkerrosta, vastaa kuvassa toka rivi, `Ethernet II`. Siinä näkyy MAC-osoitteet, jotka ovat ensimmäisen kerroksen tehtävä.
* Tokaa kerrosta, siis Internet-kerrosta, vastaa kuvassa sitä kolmas rivi. `Internet Protocol Version 4`, eli IPv4, näyttää IP-osoitteet.
* Kolmasta kerrosta eli kuljetuskerrosta, vastaa neljäs rivi. `User Datagram Protocol`, eli UDP, on toinen kuljetuskerroksen protokolla TCP:n lisäksi datan siirtoa varten.
* Viimeinen kerros on sovelluskerros. Sitä vastaa kuvassa alin rivi, eli `Domain Name SYstem (query)`. Käyttää siis DNS-palvelua, jonka tehtävä on muuttaa verkkotunnukset IP-osoitteiksi.
