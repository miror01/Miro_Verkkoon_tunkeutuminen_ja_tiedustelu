## a) Käytössäni olevia RFID-tuotteita

Itselläni oikeastaan ainoa yleisesti käytössäni oleva RFID/NFC-tuote on minun puhelimeni. Käytän sillä päivittäin Apple Payn lähimaksuominaisuutta, jonka takia pankkikortti jää usein kotiin, sillä se on digitaalisena puhelimessani Apple Walletissa. Apple Pay käyttää NFC-tekniikkaa muodostaakseen yhteyden maksupäätteen kanssa. NFC on RFID-tekniikkaa käyttävä hyvin lyhyen kantaman (korkeintaan muutama cm) langaton tiedonsiirtotekniikka joka mahdollistaa puhelimen ja maksupäätteen kommunikoinnin lyhyellä etäisyydellä.

Suojautuminen tämän kanssa on varsin yksinkertaista. NFC-tekniikka toimii vain todella lyhyellä etäisyydellä, joten sitä on vaikea ulkopuolisen hyväksikäyttää. Apple Paylla on myös muita suojautumistekniikoita, muun muassa se vaatii minulta aina Face ID:n avulla tunnistautumisen ennen maksun suorittamista, ja pankkikortin tiedot eivät välity maksupäätteelle, sillä Apple Pay käyttää ns erillistä "maksutunnistetta" kortin numeron sijaan. 

Aika itsestään selvää mutta en myöskään koskaan jätä puhelintani pois silmistäni julkisilla, se kulkee aina mukanani. Tämä edistää suojausta ja tuo minulle mielenrauhan laitteideni suojaustasosta.


## b) APDU-komentojen rakenne

APDU, eli **Application Protocol Data Unit**, on viestiformaatti, joka tarkoittaa komentoja ja vastauksia, joita muun muassa älypuhelimet ja kortit käyttävät kommunikoikseen keskenään.

NFC-tekniikan kommunikaatiossa on aina kaksi puolta, aloitteentekijä ja kohde, vaikkapa puhelin ja kortti. Tekniikka perustuu yksinkertaiseen pyyntöön/komentoon ja vastaukseen, jota voi verrata esimerkiksi HTTP-protokollan pyyntö/vastaus-formaattiin. Puhelin lähettää vaikkapa kortille APDU-komennon, jonka kortti käsittelee, ja palauttaa puhelimelle sitten APDU-vastauksen.

Komentojen rakenne perustuu kentistä. Neljä ensimmäistä kenttää listasta, eli CLA, INS, P1, ja P2 muodostaa headerin, joka on pakollinen jokaiseen APDU-komentoon. Loput kolme eli Lc, Data, ja Le taas muodostavat bodyn, ja ovat valinnaisia, riippuen komennosta.

| Kenttä | Käyttötarkoitus |
|---|---|
| CLA | Kertoo komentoluokan |
| INS | Kertoo suoritettavan komennon |
| P1 | Komennon ensimmäinen parametri |
| P2 | Komennon toinen parametri |
| Lc | Kertoo lähetettävän datan pituuden |
| Data | Lähetettävä tieto |
| Le | Kertoo kuinka paljon vastausdataa odotetaan |


## c) RFID-uutinen

Löysin uutisen "SecurityWeek"- nimiseltä sivustolta vuodelta 2024. Se käsitteli sitä kuinka miljoonista RFID-korteista on löydetty vakava tietoturva-aukko, jota voitiin hyödyntää esimerkiksi kortin kloonaamiseen. Kloonattu kortti toimii kuten alkuperäinen, eli sillä voi avata muun muassa muuten lukittuja ovia, ja mahdollistaa luvattoman pääsyn muun muassa toimistotiloihin.

Ranskalainen tietoturvayritys Quarkslab löysi kiinalaisen Shanghai Fudan Microelectronicsin MIFARE Classic RFID-korteista laitteistotason takaoven. Sen avulla pystyttiin ohittamaan tunnistautuminen.

Tutkija löysi salaisen avaimen joka liittyi takaoveen, se oli sama jokaisessa kyseisen korttimallin kortissa. Sen avulla pystyttiin murtamaan korttien salausavaimet, ja kloonaus mahdollistui, vaatien yleensä vain muutaman minuutin fyysisen pääsyn korttiin.

Tämä on mielestäni hyvä uutinen muistuttamaan RFID-tekniikan tietoturvariskeistä. Ongelma ei ole aina käyttäjässä, vaan se voi sijaita myös laitteistossa.

## Lähteet

* Application Protocol Data Unit (APDU). Luettavissa: https://www.cardlogix.com/glossary/apdu-application-protocol-data-unit-smart-card/ Luettu 16.11.2026.
* Major Backdoor in Millions of RFID Cards Allows Instant Cloning. Luettavissa: https://www.securityweek.com/major-backdoor-in-millions-of-rfid-cards-allows-instant-cloning/ Luettu 16.11.2026
