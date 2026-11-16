## a) Käytössäni olevia RFID-tuotteita

Itselläni oikeastaan ainoa yleisesti käytössäni oleva RFID/NFC-tuote on minun puhelimeni. Käytän sillä päivittäin Apple Payn -lähimaksuominaisuutta, jonka takia pankkikortti jää usein kotiin. Olen saanut Apple Payn kautta pankkikorttini puhelimeen. Apple Pay käyttää NFC-tekniikkaa muodostaakseen yhteyden maksupäätteen kanssa. NFC on RFID-tekniikkaa käyttävä hyvin lyhyen kantaman (korkeintaan muutama cm) langaton tiedonsiirtotekniikka.

Suojautuminen tämän kanssa on varsin yksinkertaista. NFC-tekniikka toimii vain todella lyhyellä etäisyydellä, joten sitä on vaikea ulkopuolisen hyväksikäyttää. Apple Payllä on myös muita suojautumistekniikoita, muun muassa se vaatii minulta aina Face ID:n avulla tunnistautumisen ennen maksun suorittamista, ja pankkikortin tiedot eivät välity maksupäätteelle, sillä Apple Pay käyttää ns erillistä "maksutunnistetta" kortin numeron sijaan. 

Aika itsestään selvää mutta en myöskään koskaan jätä puhelintani pois silmistäni julkisilla, se kulkee aina mukanani. Tämä edistää suojausta ja tuo minulle mielenrauhan laitteideni suojaustasosta.


## b) APDU-komentojan rakenne

APDU, eli **Application Protocol Data Unit**, on ymmärtääkseni ns "viestiformaatti", joka tarkoittaa komentoja ja vastauksia, joita älypuhelimet ja kortit käyttävät kommunikoikseen keskenään.

NFC-tekniikan kommunkaatiossa on aina kaksi puolta, aloitteentekijä ja kohde, vaikkapa puhelin ja kortti. Tekniikka perustuu yksinkertaiseen pyyntöön/komentoon ja vastaukseen, jota voi verrata esimerkiksi HTTP-protokollan pyyntö/vastaus-formaattiin. Puhelin lähettää vaikkapa kortille APDU-komennon, jonka kortti käsittelee, ja palauttaa puhelimelle sitten APDU-vastauksen.

Komentojen rakenne perustuu kentistä. Neljä ensimmäistä listasta, eli CLA, INS, P1, ja P2 muodostaa headerin, joka on pakollinen jokaiseen APDU-komentoon, ja loput kolme eli Lc, Data, ja Le taas muodostavat bodyn, ja ovat enemmän tilanneriippuvaisia.

| Kenttä | Käyttötarkoitus |
|---|---|
| CLA | Kertoo komentoluokan |
| INS | Kertoo suoritettavan komennon |
| P1 | Komennon ensimmäinen parametri |
| P2 | Komennon toinen parametri |
| Lc | Ilmoittaa lähetettävän datan pituuden |
| Data | Varsinainen lähetettävä tieto |
| Le | Ilmoittaa, kuinka paljon vastausdataa odotetaan |
