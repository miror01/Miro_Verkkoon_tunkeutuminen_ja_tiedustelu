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

## e) Mitäs tuli surffattua?
Latasin tehtävään tarkoitetun tiedoston Kalin kautta verkkosivuilta. Avasin sen Wiresharkissa painamalla `File`, ja `Open`. Tutkin kaappausta, ja avasin kätevän tulkinta-ikkunan Wiresharkissa `Statistics`-kohdasta, josta valitsin `Capture File Properties`. Tästä näin kätevästi tietoja, kuten että kaapattiin 283 pakettia, ja kaappaus kesti 7 sekuntia.

<img width="643" height="426" alt="image" src="https://github.com/user-attachments/assets/b3590465-f876-484a-8b10-5ef257b8448a" />

`Statistics`-valikossa on useita käteviä työkaluja. `Endpoints`-kohdasta ainakin näin, että eri IPv4-osoitteita oli 7.

`Protocol Hierarchy`-välilehdeltä nähdään käytössä olevat protokollat.

<img width="761" height="344" alt="protocolhierarchy" src="https://github.com/user-attachments/assets/30c8317b-37dd-4961-b52a-9c4258ffb8f1" />

## g) Verkkokortin merkki

Tehtävää varten, tarkistin ekana MAC-osoitteen taas `Endpoints`-välilehdeltä Wiresharkissa. Käytin w3schools.com:in MAC-address lookup- palvelua johon kirjoitin löytyneen MAC-osoitteen, ja löysin valmistajan QUEM/KVM.

<img width="540" height="324" alt="endpoints" src="https://github.com/user-attachments/assets/486c6477-1eaa-4f26-8c64-1599038c79b1" />


<img width="1140" height="732" alt="image" src="https://github.com/user-attachments/assets/f7dee556-8171-4125-873b-84fa2ee078e7" />

## h) Weppipalvelin

Kirjoitin suodattimeksi `tls.handshake.extensions_server_name`, ja tuloksena näkyi mahdollisia webbiosotteita, joilla on surffattu.

<img width="793" height="369" alt="tls" src="https://github.com/user-attachments/assets/04d1f6b2-427a-44da-8310-56f270bea06c" />

## i) Analyysi.

Aloitin taas oman liikenteeni sieppaamisen samalla tavalla kuten aiemmin, ja avasin testauksen vuoksi menemällä wikipedian verkkosivuille. Wiresharkista näkyy selkeästi verkkoliikenteen eri vaiheet, kuten ensimmäinen DNS-kysely wikipedian osoitteesta. Seuraavaksi on ensimmäinen kättely TCP-yhteyden muodostamista varten. TCP-protokolla käyttää kolmivaiheista kättelyä TCP-yhteyden muodostamista varten. Seuraavaksi yhteys salataan TLS-protokollalla.

Näiden avulla on helppoa havainnollistaa, miten verkkosivun avaaminen koostuu eri vaiheista, ja miten ne toteutuu. 

<img width="959" height="425" alt="analyysi" src="https://github.com/user-attachments/assets/a85b92ef-019b-442b-8e35-e8dddcaa68e3" />

## Lähteet)

Tero Karvinen: Verkkoon tunkeutuminen ja tiedustelu: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Karvinen 2025: Wireshark - Getting Started: https://terokarvinen.com/wireshark-getting-started/ Luettu 1.11.2026

Karvinen 2025: Network Interface Names on Linux: https://terokarvinen.com/network-interface-linux/ Luettu 1.11.2026

W3Schools Mac Address Lookup: https://www.w3schools.com/tools/tool_mac_lookup.php

