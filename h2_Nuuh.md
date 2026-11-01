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

<img width="524" height="411" alt="image" src="https://github.com/user-attachments/assets/b87ddba9-14ac-4c36-a03c-82b4510abdeb" />

Testasin nyt aiempaa `ping`-komentoa uudestaan, ja packet loss oli 100%. Tästä näemme, että paketit eivät enää mene läpi, joten verkkoyhteys on katkaistu oikein. Verkkoyhteyden palauttamiseen vaihdoin tilan Network-asetuksista takaisin tilaan `NAT`.

<img width="430" height="305" alt="image" src="https://github.com/user-attachments/assets/939e0dd6-09ef-43d2-b9df-6be4cdc7f48f" />

