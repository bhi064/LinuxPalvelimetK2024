# Maailma kuulee

*Huomio*: tämän viikon tehtävien aiheena on asentaa Apache-websivupalvelin pilvessä olevalle tietokoneelle ja julkaista sen avulla verkkosivut.
Tätä varten tarvitaan selain. Pilvipalvelinkone toteutetaan virtualisoituna ja sen ominaisuudet esitellään sopivassa osiossa.

HostOS:llä ei siis ole tehtävän kannalta väliä. Tässä tapauksessa tehtävä on tehty windows 10 - käyttöjärjestelmää käyttävällä kannettavalla tietokoneella, jossa on intelin i7 6600u prosessori ja 16 gigatavua muistia. Windowsin versio on 22H2 ja OsBuild: 19045.3930

## Lue ja Tiivistä

### Karvinen 2012: First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS

* DigitalOcean on vain yksi palveluntarjoaja monien pilvipalveluntarjoajien joukossa, ja on valikoitunut koska tarjoaa ilmaisia krediittejä GitHub education -paketin kautta. Internetissä näkyvien palvelimien käyttäjien salasanojen tulee aina olla poikkeuksellisen vahvoja.
* Ensin luodaan käyttäjätunnus ja luodaan selaimen kautta virtuaalikone.
* Seuraavaksi kirjaudutaan virtuaalikoneelle ssh:n kautta joko salasanan avulla tai ssh public avaimen kautta.
* Virtuaalikoneelle asennetaan palomuuri, mikäli se ei tule palveluntarjoajan imagen kautta ja tehdään siihen sopiva reikä ssh-liikenteelle. Sen jälkeen palomuuri aktivoidaan.
* Luodaan uusi käyttäjä (ei root), jolle ainnetaan oikeudet suorittaa koodia root-käyttäjänä lisäämällä se sudo-, admin- tai vastaaviin ryhmiin.
* Root-tili otetaan tietoturvasyistä pois käytöstä.
* Suoritetaan ohjelmistopäivitykset sopivalla komennolla.
* Voidaan aloittaa virtuaalikoneen käyttö esimerkiksi apache-palvelimena. Tehdään palomuuriin reiät sopivalle liikenteelle.
* Domain-palvelun kautta voidaan ostaa jokin domain nimi, ja asettaa nimi osoittamaan palvelimemme ip-osoitteeseen domain-palveluntarjoajan selainkäyttöliittymän kautta.

Lähde:

Karvinen, Tero. 2012. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. Luettavissa: https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu Helmikuussa 2024.

### Susanna Lehto 2022: Teoriasta käytäntöön pilvipalvelimen avulla (h4) (opiskelijan esimerkkiraportti) Kohdat a), d), e) ja f)


a)

* Palveluntarjoaja kannattaa valita tilanteen mukaan. Kurssin puitteissa sopii ilmaisia krediittejä tarjoava yritys, mutta omaa palvelua pystyttäessä kannattaa kiinnittää huomiota, missä maassa palvelin fyysisesti sijaitsee.
* DigitalOceanin alennuskoodi ei välttämättä toimi. Usein kannattaa varautua maksamaan jotain, vähintään $1 katetarkastusmaksu.
* DigitalOceanilla on selainkäyttöliittymä, josta voi säätää käyttäjätilin asetuksia, luoda virtuaalikoneita ja vaihtaa niiden hardware -konfiguraatiota. Täältä voidaan myös valita palvelimen fyysinen sijainti.
* Autentikaatio voidaan hoitaa SSH avainten välityksellä (private & public key pair) tai salasanan kautta.
* Kannattaa harkita, mitä palveluita palvelimelleen oikeasti tarvitsee ja mistä haluaa maksaa.
* Virtuaalipalvelimen luonnin jälkeen sille osoitetaan staattinen ip-osoite, josta palvelimeen voidaan ottaa yhteys.
* NameCheap on erillinen domain-nimipalvelu, jolla on myös selain-pohjainen käyttöliittymä domain nimien ostamista ja hallitsemista varten.
* Ilmaisia tarjouksia lunastaessa käy usein vastoinkäymisiä, joten pitää olla tarkkana miten asiakastiedot täyttää.

d)

* Virtuaalikoneeseen voidaan ottaa yhteys ssh:n kautta millä tahansa verkkoon liitetyllä laitteella joka tukee protokollaa.
* Virtuaalikone kannattaa päivittää ja palomuuri asentaa ja konfiguroida heti ensitöiksi.

e)

* Virtuaalikoneella ei kannata toimia root-käyttäjänä (ja root-käyttäjä kannattaa disabloida) vaan kannattaa tehdä uusi käyttäjä ja antaa sille sudo-oikeudet.
* Uudella käyttäjällä apache palvelin asennetaan ja konfiguroidaan kuten viikon 3 tehtävissä.
* Huomataan, että palomuuriin on tehtävä sopivat portit SSH-yhteydelle ennen palomuurin laittamista päälle, ja apache tarvitsee sopivat portit auki jotta se voi tarjota verkkosivuja http tai https protokollien avulla.

f)

* Palvelin päivitetään apt-paketinhallintaohjelman kautta niin kuin paikallinenkin virtuaalikone.

Lähde:

Lehto, S. 2022: Teoriasta käytäntöön pilvipalvelimen avulla. Luettavissa: https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu Helmikuu 2024.


## Vuokraa oma virtuaalipalvelin haluamaltasi palveluntarjoajalta

Haaga-Helia tarjoaa opiskelijoilleen mahdollisuuden kokeilla Microsoftin Azure-palveluita, ja olin tehnyt käyttäjätilin toisen kurssin ohjeiden mukaisesti palveluun.

Mielestäni tehtävänanto ei edellytä käyttäjätilin luomista johonkin palveluun, vaan pelkästään pilvipalvelimen vuokrausta sen kautta, jonka vuoksi Azure:n käyttäjätilin luontia ei tässä raportissa esitetä.

Kirjauduin palveluun verkkoosoitteessa https://portal.azure.com ja aloitin luomaan virtuaalikonetta.

Virtuaalikoneen asetukset esitetään kuvakaappauksissa, jotta niitä olisi vaikeampi jäsentää automatisoidulla botilla, koska raportti on julkisessa internetissä näkyvillä. (Tämänhän ei pitäisi olla julkista tietoa.)

![](/4_Viikko/kuvat/1.png)

![](/4_Viikko/kuvat/2.png)

![](/4_Viikko/kuvat/3.png)

![](/4_Viikko/kuvat/4.png)

![](/4_Viikko/kuvat/5.png)

![](/4_Viikko/kuvat/6.png)

![](/4_Viikko/kuvat/7.png)

![](/4_Viikko/kuvat/8.png)

![](/4_Viikko/kuvat/9.png)

Virtuaalipalvelin luotiin, ja palvelu kertoi minulle sen olevan valmis.

![](/4_Viikko/kuvat/10.png)

Otetaan yhteys virtuaalikoneeseen käyttäen Azuren selain CLI-työkalua.

![](/4_Viikko/kuvat/11.png)

![](/4_Viikko/kuvat/12.png)

Hyväksytään.

![](/4_Viikko/kuvat/13.png)

Azure valmisteli asetuksia. Aloitin klo 00:33. Noin klo 00:35 sain ilmoituksen virheestä.

![](/4_Viikko/kuvat/14.png)

Suurensin selainta, ja kokeilin uudestaan ottaa Azure CLI-yhteyden. Nyt yhteys toimi. En ole varma miksi se aluksi hylättiin, mutta toimi nyt uudelleen kokeilun kanssa.

![](/4_Viikko/kuvat/15.png)

Selaimessa ajatteva terminaali siis toimii ainakin teoriassa.

Azure halusi ottaa yhteyden MS-käyttäjätunnukseni avulla. Tämä ei toimisi, koska virtuaalikoneella ei ole sopivaa käyttäjää. Tietoturvasyistä tästä ei ole kuvakaappausta. Kokeillaan "Native SSH" vaihtoehdon avulla, eli käyttäen windowsin omaa terminaalia. (Voisimme myös antaa selain-bashillä suoraan komennon ssh käyttäjänimi@ip-osoite, mutta tehdään nyt "oikeiden työkalujen" avulla.)

![](/4_Viikko/kuvat/16.png)

Olemme sisällä.

Lähde:

Hakulinen, T. 2023. Suullinen tiedonanto Haaga-Helian kurssilla Johdanto ICT-Infrastruktuuriin ja pilvipalveluihin. Loppusyksy 2023.
Omat raportit ja muistiinpanot viikolta 3. https://github.com/bhi064/LinuxPalvelimetK2024/blob/main/3_Viikko/viikon-3-palautus.md

## Tee alkutoimet omalla virtuaalipalvelimellasi.

Ajetaan nyt seuraavat toimenpiteet:

* Installoidaan ufw
* Konfiguroidaan ufw (portit 22 ja 80 auki)
* Päivitetään virtuaalikone


    sudo apt install ufw
	
	sudo ufw allow 22/tcp
	
	sudo ufw allow 80/tcp
	
	sudo ufw enable
	
Viimeisin komento hyväksytään antamalla varmiste "y".

    sudo apt upgrade
	
	sudo apt update
	
Viimeisin komento varmistaa, että meillä on viimeisimmät versiot olemassa paketeistamme. Alkutoimet ovat valmiit.

Lähde:

Karvinen, T. Linux Palvelimet 2024 alkukevät. Etäluento. Suullinen tiedonanto.
Karvinen, Tero. 2012. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. Luettavissa: https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu Helmikuussa 2024.


## Asenna weppipalvelin omalle virtuaalipalvelimellesi. Kokeile, että se näkyy julkisesti.

* Installoidaan Apache2

    sudo apt install apache2
	
Luodaan nyt jokin verkkosivu. 

* Luodaan jokin (verkko)sivu, joka näytetään apachen avulla.

Suoritetaan seuraavat komennot:

![](/4_Viikko/kuvat/17.png)

Muistetaan, että tarvittiin vielä väliin kansio, jotta sivut saadaan organisoitua hyvin.

![](/4_Viikko/kuvat/18.png)

Korjattu.

* Konfiguroidaan apache.

![](/4_Viikko/kuvat/19.png)

Luodaan uusi conf-tiedosto sivullemme.

    sudoedit naurattaja.example.com.conf

Muutetaan sen sisältö osoittamaan tehtyyn sivuun.

![](/4_Viikko/kuvat/20.png)

Enabloidaan tehty sivu.

![](/4_Viikko/kuvat/21.png)

Muokataan vielä hosts-tiedostoa:

![](/4_Viikko/kuvat/22.png)

Testataan, että sivu näkyy HostOS:n selaimesta:

![](/4_Viikko/kuvat/23.png)

Testasin verkkosivua myös puhelimen selaimesta. Sivut näkyvät samalla tavalla, mutta kuvaa en ottanut todisteeksi, että tämä toimenpide on suoritettu. Raportin lukija voi kurssin ajan varmentaa, että sivut toimivat ottamalla itse yhteyden näytettyyn ip-osoiteeseen.

Toimenpiteiden jälkeen annoin terminaalissa komennon:

    exit
	
Joka sulki ssh-yhteyden.

Lähde:

Omat muistiinpanot ja raportti viikolta kolme: https://github.com/bhi064/LinuxPalvelimetK2024/blob/main/3_Viikko/viikon-3-palautus.md

## Vuokraa domain-nimi ja aseta se osoittamaan virtuaalipalvelimeesi.

Päätin tehdä vaihtoehtoisen tehtävän, enkä osta domain-nimipalvelua verkkosivuilleni. Hosts-tiedoston asetukset on esitetty ylemmässä tehtävässä.

Tehdään siis analyysi vaikkapa terokarvinen.com - sivujen nimestä whois - palvelun kautta. https://who.is

![](/4_Viikko/kuvat/24.png)

Saamme seuraavat tulokset

![](/4_Viikko/kuvat/25.png)

![](/4_Viikko/kuvat/26.png)

![](/4_Viikko/kuvat/27.png)

![](/4_Viikko/kuvat/28.png)

Tietojen perusteella tiedämme, että domainin registrar on ranskalainen Gandi.net palvelu. Sivut on rekisteröity 06/11/2010, ja varaus on päivitetty viimeksi 12/10/2023. Gandi:n toimiston osoite on esitetty Registrarin tiedoissa.

DNS-tiedoista näemme, että sivujen julkinen ip-osoite on 139.162.131.217. TTL on 4094 sekuntia, eli noin tunti. Muutokset siis tulevat voimaan noin tunnissa, jos IP-osoitteelle asetettaisiinkin jokin toinen nimi ja terokarvinen.com otettaisiin pois käytöstä.

Diagnostics -välilehdessä ajetaan ping ja traceroute komennot. Niiden perusteella verkkosivujen palvelin toimii linode -palvelussa.

Tarkempi analyysi jätetään tehtävän ulkopuolelle.

Huomio: domain nimipalvelun asettaminen omille verkkosivuille onnistuu palveluntarjoajan selainpohjaisen käyttöliittymän kautta. Se on yleensä tehty intuitiiviseksi ja helpoksi, ja palveluntarjoajilla on ohjeita, miten heidän palveluaan käytetään. En koe tässä tärkeäksi nimen vuokraamista, mutta olen varma että osaisin tehtävässä tiivistetyn artikkelin perusteella kyllä tehdä tarvittavat toimenpiteet. Eri palveluntarjoajilla on hieman erilaiset käyttöliittymät, jotka saattavat ajan saatossa muuttua, joten tarkoitus ei ole opiskella ulkoa miten tämä tapahtuu, vaan mitä tietoja ja tietueita tulee palvelussa kytkeä toisiinsa.

Yhteiset lähteet:

Karvinen, T. 2024. Linux Palvelimet 2024 alkukevät. Tehtävänannot. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/. Luettu Helmikuu 2024.
