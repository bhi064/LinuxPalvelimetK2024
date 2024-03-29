# Hello Web Server

## Lue ja Tiivistä

### The Apache Software Foundation 2023: Apache HTTP Server Version 2.4 Documentation

* IP-pohjaiset virtuaaliset isäntäkoneet tarvitsevat kukin oman IP-osoitteensa palvellakseen asiakkaita.
* Nimipohjaisilla virtuaalisilla isäntäkoneilla sama IP-osoite voi isännöidä useaa eri verkkosivua, sillä verkkosivujen tunnisteet ovat HTTP-headerissä, joten palvelin osaa palauttaa asiakkaan selaimelle sen sivun, jota se pyytää.
* NImipohjainen verkkosivu on helpompi toteuttaa ja kuluttavat vähemmän julkisen internetin IP-osoitteita.
* Pyydettäessä verkkosivua, Apache verta isännän nimeä sille asetettuihin ServerName ja ServerAlias -arvoihin, joiden perusteella se antaa parhaan arvionsa sivusta, jota haettiin.
* Jos ei löydetä sopivaa arvoa, niin palautetaan ensimmäinen Apachessa konfiguroitu sivu.
* Jokaiselle sivulle konfiguroidaan konfiguraatiotiedostoon oma <VirtualHost> blokki, johon asetetaan ServerName -muuttuja ja joka osoittaa johonkin palvelimen kansioon, jossa palautettava sivu sijaitsee.
* ServerAlias -muuttujalla voidaan osoittaa usea osoite näyttämään samaa sivua.

Lähteet:

The Apache Software Foundation 2023: Apache HTTP Server Version 2.4 Documentation: Name-based Virtual Host Support. Luettavissa https://httpd.apache.org/docs/2.4/vhosts/name-based.html Luettu 05/02/2024.

### Karvinen 2018: Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address

* Apachella voidaan isännöidä useita eri sivuja samalta IP-osoitteelta.
* Ensin installoidaan ja konfiguroidaan Apache Web Server.
* Seuraavaksi editoidaan apachen konfiguraatiotiedostoa siten, että lisätään sinne sivuamme vastaava <VirtualHost> - blokki.
* Luodaan verkkosivu normaalina käyttäjänä polkuun, joka osoitettiin <VirtualHost> -blokissa.
* Testataan sivua selaimella tai curl- komennolla.
* Virtuaaliisäntiä voi olla niin paljon, kuin halutaan, vaikka käytössä olisi vain yksi IP-osoite.

Lähteet:

Karvinen, Tero. Karvinen 2018: Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/ Luettu 05/02/2024.

## Testaa, että Weppipalvelimesi vastaa localhost-osoitteesta.

Ensimmäiseksi päivitetään virtuaalikone ja käynnistetään se varmuuden vuoksi uudelleen.
    
    sudo nala upgrade
    systemctl reboot

Apache-palvelimen asettaminen tehtiin harjoitustyönä kurssin luennolla 30.1.2024. Testataan, että palvelin on edelleen päällä ja localhostiin tarjoillaan sinne osoitettu nettisivu.

    curl localhost

![](/3_Viikko/kuvat/1.png)

Curl palauttaa meille tunnilla tekemäni html-sivun. Katsotaan sitä selaimessa: 

![](/3_Viikko/kuvat/2.png)

Palvelin vastaa localhostista.

Lähteet:

Karvinen, Tero. Linux Palvelimet. Kevät 2024. Kolmas etäluento. 30.01.2024.

## Etsi lokista rivit, jotka syntyvät, kun lataat omalta palvelimeltasi yhden sivun. Analysoi.

Etsitään ensin logit.

    sudo ls /var/log/apache2/

![](/3_Viikko/kuvat/3.png)

Katsotaan logeja tail -ohjelmalla seuraavalla komennolla
    sudo tail -3 /var/log/apache2/other_vhosts_access.log

![](/3_Viikko/kuvat/4.png)

Logeissa näkyy hostname (foo.example.com), portti (80), IP-osoite jossa sivu on (localhostin osoite), päivämäärä ja aika, jolloin pyyntö on tehty, Mikä HTTP-pyyntö on kyseessä (GET), palautettu koodi (Esim 200 - joka vastaa että kaikki OK), vastauksen koko tavuissa (356), sekä tietoja pyynnön tehneestä ohjelmasta, tai selaimesta. Tässä nähdään sekä curl-pyyntö, että Mozilla Firefox-selaimella tehty pyyntö, joka näyttää vielä, että käytössä on X-window system, linux käyttöjärjestelmä, jota suoritetaan x86_64 -konekäskykannallla. Selaimen revisionumero on myös esitetty. Selain käyttää Gecko-selainmoottoria, jonka versionumero ja selaimen versionumero ovat esitettynä viimeisenä.

## Etusivu uusiksi. Tee uusi name based virtual host. hattu.example.com. Pitää pystyä muokkaamaan normaalina käyttäjänä.

Tehdään ensimmäisenä uusi juurikansio sivulle. Läytetään samaa PublicHtml-kansiota, jonka loin kotikansiooni tuntitehtäviä tehdessä. Sille on asetettu oikeudet siten, että apache pystyy näyttämään kansiossa olevia tiedostoja (suoritusoikeus kotikansiossa, vaikka ei luku- tai kirjoitusoikeuksia).

    cd PublicHtml/
	mkdir hattu.example.com

Luodaan sinne index.html -sivu

    touch index.html
	echo "Moro hattuilijat" > index.html
	cat index.html

Viimeisin komento tarkastaa, että tiedostoon kirjoitettin mitä haluttiin.

Seuraavaksi mennään apachen konfiguraatiokansioon.

    cd /etc/apache2/sites-available/

Kopioidaan tuntitehtävän konffitiedosto, koska olemme laiskoja (ja kello on jo paljon -- 02:17, vai onko se sittenkin vähän?)

    sudo cp foo.example.com.conf hattu.example.com.conf

Muokataan tiedostoa sudona

    EDITOR=micro sudoedit hattu.example.com.conf

Kuvakaappaus tiedoston sisällöstä muutosten jälkeen:

![](/3_Viikko/kuvat/5.png)

Enabloidaan sivu apachessa

    sudo a2ensite hattu.example.com

Käynnistetään apache-daemon uudelleen

    sudo systemctl restart apache2
	
Lisätään seuraavaksi hattu.example.com hosts -tiedostoon

    EDITOR=Micro sudoedit /etc/hosts
	
Tässä tiedoston sisältö kuvakaappauksena muutosten jälkeen. hattu.example.com on nyt esitetty.
![](/3_Viikko/kuvat/6.png)

Katsotaan, että selain antaa meille jotakin:

![](/3_Viikko/kuvat/7.png)

Näyttäisi toimivan.


Laitetaan vielä vanha sivu pois päältä.

    sudo a2dissite foo.example.com
    sudo systemctl restart apache2

Kokeillaan hakea tuntitehtävänä ollut sivu

![](/3_Viikko/kuvat/8.png)

Välimuistiin oli jäänyt vanha sivu. Kokeillaan refreshata shift-pohjassa:

![](/3_Viikko/kuvat/9.png)

Hieno homma!

Varmistetaan, että sivua voi muokata ilman sudoa.

    cd ~/PublicHtml/hattu.example.com
    micro index.html
	
![](/3_Viikko/kuvat/10.png)

Virkistetään nyt sivu shift-pohjassa.

![](/3_Viikko/kuvat/11.png)

No sudo necessary!

## Tee validi HTML5 sivu.

    micro index.html

![](/3_Viikko/kuvat/12.png)

Validoidaan sivu osoitteessa https://validator.w3.org/#validate_by_input

![](/3_Viikko/kuvat/13.png)

## Anna esimerkit curl -i ja curl komennoista. Selitä otsakkeet.

curlataan meidän oma localhost-sivu -i flägillä (include), jonka tarkoituksena on palauttaa samat asiat kuin curl, mutta lisätä HTTP-otsakkeet palautukseen.
    curl -i localhost

![](/3_Viikko/kuvat/14.png)

Curl antaa seuraavat otsakkeet:
* HTTP versio ja vastauskoodi (200, ok).
* Päivämäärä, milloin pyyntö tehtiin. (Huomaa, tässä annettiin GMT-aika, eikä paikallinen aika)
* Mikä palvelin on käytössä (Apache, sen versio ja jakeluversio)
* Milloin pyydettyä sivua on viimeksi muokattu.
* Entity tag -tunniste, jonka avulla selvitetään, onko välimuistissa sama sivu, kuin minkä palvelin palauttaisi
* Accept-ranges kertoo, osaako palvelin vastata ei-täydellisiin asiakaspyyntöihin, ja jos osaa, niin miten. Tässä osataan antaa tavuja.
* Content-Length kertoo tiedoston koon tavuissa.
* Content-Type kertoo, minkä muotoista dataa palvelin sivulla palauttaa.

    curl -I

Tekee saman kuin curl -i, mutta jättää palauttamatta tiedoston sisällön, eli palautuksessa on vain HTTP-otsakkeet.

curlataan muodon vuoksi vaikka haaga-helian sivut.

    curl https://www.haaga-helia.fi/fi/

Saadaan vastauksena sivujen html-dokumentti, joka on sama kuin jos sivut avaa selaimella ja katsoo devtools-valikosta sivun html-muotoisen esityksen.
Tiedosto on hyvin suuri, joten sitä ei ole tässä harjoitustyössä kuvakaappauksena esitetty.

Lähteet:

MDM web docs. Accept-Ranges. Luettavissa https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Ranges. Luettu 05.02.2024
curl --help komento


## Tehtävät m), n), ja o) ovat vapaaehtoisia, eikä niitä löydy tästä dokumentista.

Periaatteessa tehtävä o)- oli virtuaalikoneella jo toteutettuna, koska hosts-tiedostossa oli määriteltynä sivut foo.example.com ja hattu.example.com. Molempiin oltaisiin voitu ottaa yhteys.

Yhteiset lähteet:

Karvinen, Tero. Linux Palvelimet 2024 alkukevät. Tehtävänannot. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/. Luettu: 05.02.2024

Tämä harjoitustyö on julkaistu GPLv3 lisenssin alla.