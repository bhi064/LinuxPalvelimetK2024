# DJ Ango

HostOS: Arch Linux, kernel 6.7.8-arch1-1. Päivitetty 03.03.2024.
Host tietokone: Lenovo Yoga Slim 7, jossa AMD:n R7 4700U -prosessori, 16 Gigatavua muistia ja integroitu Raden RX Vega -näytönohjain. Nvme SSD, jossa vapaata tilaa riittävästi (100+ Gigatavua).

![](/6_Viikko/kuvat/1.png)

GuestOS: Debian Bookworm 12.5 xfce, 4 gigatavua virtuaali-muistia, 4 säiettä, 50 gigatavua tallennustilaa. Virtualisoitu KVM/Qemu/libvirt -pinolla, jonka käyttöliittymänä on virt-manager.

Viikon aiheena on asentaa python-ohjelmointikieltä hyödyntävä Django framework, jonka päällä voi ajaa python ohjelmakoodia hyödyntäviä ohjelmia. Ajatuksena on luoda järjestelmä, jossa Apache-palvelimella pyörii Djangolla toteutettu ohjelma. Osaamista voi soveltaa toteuttamalla jonkin tietokantaa hyödyntävän ohjelman, jonka backend-ohjelmakoodi on toteutettu pythonilla.

-----------

## Lue ja tiivistä Karvinen 2021: [Django 4 Instant Customer Database Tutorial](https://terokarvinen.com/2022/django-instant-crm-tutorial/)

* Asenna virtualenv. Konfiguroi virtualenv-kansio, jolloin pip-paketinhallintaohjelman asentamat paketit asennetaan virtualenv-kansioon, eikä koko järjestelmänlaajuisesti.
* Asenna pip-paketinhallintatyökalu, jonka avulla pythonin pakettien asentaminen on helppoa ja nopeaa (mutta ole varovainen, että asennat oikeat paketit). Paketit kannattaa tallentaa tekstitiedostoon, josta pip lukee ne.
* Djangon asennuksen jälkeen luodaan projektikansio. Django luo sinne oletusprojektin, jossa tulee mukana testisivu ja testipalvelin. Palvelimen saa päälle ajamalla sopivan python-tiedoston.
* Djangossa tulee myös oletuksena hallintatyökalu, johon sisältyy tietokantatuki. Tietokanta tulee päivittää ja migratoida projektille, jonka jälkeen sinne voidaan lisätä pääkäyttäjä.
* Pääkäyttäjän lisäämisen jälkeen voimme lisätä käyttäjiä djangon web-käyttöliittymästä kirjautumalla sisään pääkäyttäjänä.
* Voimme muokata django-projektimme tietokantaa, esimerkiksi luomalla uuden applikaation (esimerkissä crm) ja muokkaamalla projektimme settings.py - tiedostoa siten, että projekti tietää käyttää luotua applikaatiota.
* Djangossa on tuki käyttää tietokantoja oman django.db -mallinsa kautta. Tämä tapahtuu python-kielellä.
* Pythonilla voidaan myös päättää, mitä tietoja luoduista tietokantataouluista haetaan ja näytetään djangon verkkoliittymässä. Esimerkissä näytetään, miten voidaan luoda asiakas-objekteja joilla on nimi, tallentaa ne tietokantatauluun ja näyttää asiakkaiden nimet verkkoportaalin kautta.

Lähde:

Karvinen, T. Django 4 Instant Customer Database Tutorial. Luettavissa: https://terokarvinen.com/2022/django-instant-crm-tutorial/ Luettu: maaliskuu 2024.

-----------

## Lue ja tiivistä Karvinen 2021: [Deploy Django 4 - Production Install](https://terokarvinen.com/2022/deploy-django/)

* Django-projekti saadaan turvallisesti julkaistua verkkoon ajamalla sitä apache-palvelimen päällä. Apachelle on olemassa rajapinta mod_wsgi, jonka avulla tämä yhteensovitus tapahtuu.
* Apachessa luodaan uusi name-based Virtual Host, jonka asetuksissa määritellään esitettävä kansio. (Kotihakemistossa on publicwsgi-kansio, johon projektikansiot tallennetaan).
* Luodaan virtualenv -ohjelmalla uusi virtuaalinen ympäristö, ja asennetaan sinne django pip-paketinhallintatyökalulla. Luodaan uusi django projekti (edellä esitettyyn publicwsgi-kansioon). Projektilla on kolme absoluuttista sijaintia: pääkansio, wsgi.py-tiedoston sijanti ja virtualenv-kansion sijainti.
* Django-projektia tulisi ajaa käyttäjänä, jolla ei ole sudo-oikeuksia.
* Django-projekti saadaan näkymään apache-palvelimella mod_wsgi:n kautta samalla periaatteella, kuin mikä tahansa muukin verkkosivu. Apache-määrityksissä pitää vain esittää, missä kaikki projektin tarvitsemat tiedostot ja kansiot sijaitsevat (mainittu yllä). Tätä varten pitää myös asentaa apachen WSGI-moduuli, libapache2-mod-wsgi-py3.
* Artikkelissa on esitetty esimerkillinen apachen .conf -tiedosto, josta voi olla apua omia projekteja luodessa.
* Kun Django-projekti on saatu pyörimään apachella, tulee siitä disabloida debug-virheilmoitukset, jotta ulkopuolinen hyökkääjä saisi mahdollisimman vähän tietoa käsiinsä mikäli yrittää hyökätä palvelimellemme.
* wsgi.py -tiedostoa tulee "koskettaa" touch-komennolla muutosten jälkeen, jotta sen metadata on kunnossa (milloin viimeksi muokattu).
* Jotta Django projekti näkyisi oikein, tulee apachelle osoittaa, missä staattiset elementit, kuten vaikkapa css-tyylitiedosto sijaitsevat. Tämä tehdään Djangon settings.py -tiedoston kautta siten, että Django kertoo apachelle, missä tyylitiedostot sijaitsevat. Tämä kannattaa tehdä BASE_DIR muuttujan avulla, joka osoittaa Django-projektin juurikansioon sen sijaan, että polku "kovakoodattaisiin" järjestelmän johonkin hakemistopolkuun.
* Vianselvityksessä kannattaa turvautua apachen error.log tiedostoon, tai laittaa djangon debug-virheilmoitukset päälle, jolloin niitä voi katsoa selaimella. Tätä ei saisi tehdä, jos sivu näkyy internettiin, mutta localhostissa näkyville testiprojekteille näin voi huoletta tehdä.
* Asennustapa edellyttää tarkkuutta sen kanssa, että oikeat moduulit on asennettu, muuttujat alustettu ja nimetty oikein (syntaksi täsmää), sallitut hostit määritetty oikein (esim localhost) ja kansioiden oikeudet asetettu siten, että django ja apache voivat suorittaa niiden tiedostoja.



Lähde:

Karvinen, T. Deploy Django 4 - Production Install. Luettavissa: https://terokarvinen.com/2022/deploy-django/ Luettu: maaliskuu 2024.

------------


## a) Tee yksinkertainen esimerkkiohjelma Djangolla. Voit käyttää testipalvelinta, kunhan se ei näy Internetiin. Riittää, kun ohjelmasi näkyy esimerkiksi Django Adminsissa. Voit halutessasi tehdä aivan samanlaisen kuin Teron CRM-esimerkissä.

Tehdään esimerkkiohjelma kurssin viikolla 5 luodulle [virtuaalikoneelle](https://github.com/bhi064/LinuxPalvelimetK2024/blob/main/5_Viikko/viikon-5-palautus.md). Oheessa on linkki raporttiin, jossa esitetään virtuaalikoneen luonti ja apache-palvelimen alustus. Virtuaalikoneella ei ole olemassa olevaa Django-asennusta, jonka vuoksi se on sopiva alusta tehtävälle.

Ensimmäisenä päivitetään järjestelmä.

	sudo apt-get update && sudo apt-get upgrade
	
Järjestelmä oli jo päivitetty.

Luodaan uusi käyttäjä tulevia tehtäviä varten. Tätä käyttäjää ei käytetä tässä tehtävässä, mutta kenties b)-osiossa. Se on kuitenkin luontevaa tehdä nyt, kun muutenkin ylläpidämme järjestelmää.

	sudo adduser kyynkutittaja
	
![](/6_Viikko/kuvat/2.png)

Seuraavaksi asennetaan tarvittavat ohjelmat: virtualenv, pip, python3, micro, ja libapache2-mod-wsgi-py3. Apache on jo asennettu ja konfiguroitu edellisissä tehtävissä.

	sudo nala install virtualenv pip python3 micro libapache2-mod-wsgi-py3
	
![](/6_Viikko/kuvat/3.png)
	
apt (jonka front-end nala on) osasi automaattisesti valita meille python3:n tukeman version pip-ohjelmasta. Nala tekee tämän huomaamisesta helpompaa, kuin aptin käyttö "apt" tai "apt-get" -kutsujen kautta, mutta harjaantunut ylläpitäjä ei toki tarvitse mitään graafisuutta terminaaliinsa.

Tehdään nyt muutama uusi kansio: publicwsgi pääkäyttäjämme kotikansioon, ja sen sisälle virtuaalisen ympäristön kansio env

	mkdir publicwsgi
	cd publicwsgi
	virtualenv --system-site-packages -p python3 env/

![](/6_Viikko/kuvat/4.png)

Käynnistetään virtuaaliympäristö:

	source env/bin/activate

![](/6_Viikko/kuvat/5.png)

Huomataan, että terminaaliimme ilmestyi (env) -merkintä, joka kertoo meille, että virtuaaliympäristö on käytössä.

Tarkastetaan, että ympäristö toimii oikein.

	which pip
	
![](/6_Viikko/kuvat/6.png)
	
Järjestelmä raportoi meille, että pip-paketinhallintaohjelma tulee jatkossa asentamaan paketit ympäristömme määrittämään paikkaan, eikä koko järjestelmään. Nyt meillä voi olla projektissamme eri pip-paketit tai eri versiot niistä, kuin pää järjestelmässämme.

Luodaan nyt publicwsgi-kansioon requirements.txt -tiedosto, johon laitamme kaikki pipin hallitsemat paketit. Näitä on tässä tapauksessa vain yksi: django.
	
	micro requirements.txt
	
kirjoitetaan tiedostoon "django"

![](/6_Viikko/kuvat/7.png)

asennetaan nyt django virtuaaliympäristöömme:

	pip install -r requirements.txt

Tarkastetaan, että django asentui oikein:

	django-admin --version
	
![](/6_Viikko/kuvat/8.png)	
	
Kaikki toimi kuten pitikin. Tehdään nyt esimerkkiohjelma demonstroimaan djangon toimintaa: kuunkuiskaajat.

	django-admin startproject kuunkuiskaajat

Tarkastetaan, että django on luonut projektiimme oman kehyksensä ls-komennoin ja huomaamme, että kaikki vaikuttaisi hyvältä.

![](/6_Viikko/kuvat/9.png)

Kokeillaan nyt devaus-palvelinta:

	cd kuunkuiskaajat/
	./manage.py runserver

![](/6_Viikko/kuvat/10.png)
![](/6_Viikko/kuvat/11.png)

Django näyttäisi olevan toiminnassa. Otetaan palvelin alas näppäinyhdistelmällä ctrl+c. Ajetaan tietokannan migraatiot.

	./manage.py migrate
	
![](/6_Viikko/kuvat/12.png)

Luodaan projektillemme pääkäyttäjä.

	./manage-py createsuperuser
	
![](/6_Viikko/kuvat/13.png)
	
Laitetaan palvelin nyt päälle, ja yritetään kirjautua sisään pääkäyttäjänä.

	./manage.py runserver

Navigoidaan selaimella osoitteeseen: 127.0.0.1:8000/admin ja kirjaudutaan sisään.

![](/6_Viikko/kuvat/14.png)

Pääsimme kirjautumaan sisään. Kirjaudutaan nyt kuitenkin ulos, ja tehdään uusi ohjelma, kuiskaus. Ohjelma kenties tulevaisuudessa tarjoilee meille mietelauseita. Lisätään ohjelma projektimme settings.py -tiedostoon.

	./manage.py startapp kuiskaus
	micro kuunkuiskaajat/settings.py
	
![](/6_Viikko/kuvat/15.png)

Lisätään nyt ohjelmaamme sopivia tietokantatauluja python-koodin kautta (sen sijaan, että tekisimme ne suoraan SQL:lla jollekin tietylle tietokannalle hallintaohjelman kautta), jotta tulevaisuudessa voimme vaikkapa vaihtaa tietokantatoteutusta.

	micro kuiskaus/models.py

![](/6_Viikko/kuvat/16.png)

Luodaan siis uusi taulu, jossa on kaksi saraketta: nimi ja sisältö. Ajetaan migraatiot.

	./manage.py makemigrations
	
![](/6_Viikko/kuvat/17.png)

Huomataan, että teimme kirjoitusvirheen. Korjataan se, ja kokeillaan uudelleen.

![](/6_Viikko/kuvat/18.png)

Nyt makemigrations toimi.


	./manage.py migrate

![](/6_Viikko/kuvat/19.png)
	
Tietokantataulumme on nyt luotu. Rekisteröidään tietokanta.

	micro kuiskaus/admin.py

![](/6_Viikko/kuvat/20.png)

Testataan, että kaikki toimii. Käynnistetään taas testipalvelin ja kirjaudutaan sisään kuten yllä.

![](/6_Viikko/kuvat/21.png)

Luotu ohjelmamme, "kuiskaus" näkyy djangon admin-paneelissa, ja sillä on tietokantataulu, Sayings. Luodaan sinne uusi sanonta:

![](/6_Viikko/kuvat/22.png)

Laitetaan sanonnat vielä "nimen" mukaiseen esitykseen. Muokataan kuiskaus/models.py -tiedostoa.

	micro kuiskaus/models.py

![](/6_Viikko/kuvat/23.png)

Katsotaan, miltä näyttää. Laitetaan palvelin taas pystyyn kuten yllä ja katsotaan selaimella:

![](/6_Viikko/kuvat/24.png)

Meillä on nyt aluillaan tietokantaohjelma, johon voimme tallentaa erilaisia sanontoja. Tulevaisuudessa kenties haluamme palauttaa sieltä satunnaisia sanontoja käyttäjien iloksi, tai luoda generaattorin, joka yhdistelee sanontoja jonkin säännön avulla. Jätetään se kuitenkin tehtävän ulkopuolelle tällä kertaa.


Lähteet:

Karvinen, T. Linux Palvelimet alkukevät 2024. Kuudes etäluento. Suullinen tiedonanto ja visuaaliset esimerkit. 27.2. 2024

Karvinen, T. Django 4 Instant Customer Database Tutorial. Luettavissa: https://terokarvinen.com/2022/django-instant-crm-tutorial/ Luettu: maaliskuu 2024.

-------------

## b) Tee Djangon tuotantotyyppinen asennus

Apache2 palvelin on jo installoituna virtuaalikoneelle, ja localhostissa on esimerkkiverkkosivut.

	curl localhost
	
![](/6_Viikko/kuvat/25.png)
	
Laitetaan a) kohdassa luotu ohjelmamme näkymään tuotantopalvelimella, eli apachen päällä.

	cd publicwsgi/
	cd kuunkuiskaajat/
	mkdir static
	echo "Staattiset terveiset" > static/index.html

Nyt ollaan luotu esimerkkisivu. Laitetaan se apachessa näkyviin.

	sudoedit /etc/apache2/sites-available/kuunkuiskaajat.conf
	
![](/6_Viikko/kuvat/26.png)
	
	sudoedit /etc/hosts

![](/6_Viikko/kuvat/27.png)

Laitetaan nyt sivu päälle

	sudo a2ensite kuunkuiskaajat.conf
	sudo systemctl restart apache2
	
![](/6_Viikko/kuvat/28.png)
	
Laitetaan vanha sivu pois päältä.

	sudo a2dissite omasivu.example.com.conf
	sudo systemctl restart apache2
	
Kun avaamme localhostin selaimella, saamme apachen testisivun. Selvitetään, mistä asia kiikastaa.

Muutetaan hosts-tiedostoa kuten yllä.

![](/6_Viikko/kuvat/30.png)

Nyt staattiset terveiset näkyvät oikein, kun navigoimme oikealle sivulle.

![](/6_Viikko/kuvat/31.png)

Navigoidaan nyt projektikansioon ja käynnistetään a)-kohdassa luotu virtualenv.

	cd /home/ritarirollo/publicwsgi
	source env/bin/activate

django ja mod_wsgi on asennettu jo a)-kohdassa, sekä sopiva projekti luotu ja muokattu. Laitetaan nyt sanonta-ohjelmamme apachen päälle.

	EDITOR=micro sudoedit /etc/apache2/sites-available/kuunkuiskaajat.conf

![](/6_Viikko/kuvat/32.png)

Testataan muutokset

	/sbin/apache2ctl configtest

![](/6_Viikko/kuvat/33.png)	

Huomataan, että olemme tehneet virheen rivillä 13. Välilyönti puuttuu. Korjataan ja kokeillaan uudelleen.

![](/6_Viikko/kuvat/34.png)

Nyt testi toimii.

Potkaistaan vielä apache2 daemonia (eli käynnistetään uudelleen).

![](/6_Viikko/kuvat/35.png)

Palvelin ei vastaa. Tarkastetaan, että kaikki on projektikansiossa vielä ok.

![](/6_Viikko/kuvat/36.png)

Huomataan, että wsgi.py tiedoston sijainti ei ainakaan ole oikein. Muutetaan se.

	EDITOR=micro sudoedit /et/apache2/sites-available/kuunkuiskaajat/kuunkuiskaajat.conf

![](/6_Viikko/kuvat/37.png)

Potkaistaan apache2-daemonia. Django ei vieläkään lähtenyt käyntiin. Tarkastettiin conf-tiedosto, ja huomattiin toinenkin kirjoitusvirhe TWSGI-muuttujan määrittelyssä. Korjataan ja kokeillaan uudelleen.

![](/6_Viikko/kuvat/38.png)

Nyt Django toimii. Muutetaan vielä asetuksia siten, että debug-otetaan pois päältä.

	cd /home/ritarirollo/publicwsgi/kuunkuiskaajat/kuunkuiskaajat/
	micro settings.py
	
![](/6_Viikko/kuvat/39.png)
	
Kosketetaan wsgi.py tiedostoa, jotta sen metadata päivittyy.

	touch wsgi.py

Käynistetään apache uudelleen.

	sudo systemctl restart apache2

![](/6_Viikko/kuvat/40.png)

Nyt sivut eivät näy enää kunnolla, koska sivua ei olla määritelty. Admin-paneeli kyllä toimii:

![](/6_Viikko/kuvat/41.png)

Mutta staattisia tiedostoja ei olla osattu ladata (eli css-tyylitiedosto ei näy). Muutetaan asetuksia:

	micro settings.py

![](/6_Viikko/kuvat/42.png)

Haetaan nyt staattiset tiedostot.

	./manage.py collectstatic
	
![](/6_Viikko/kuvat/43.png)
	
Kokeillaan nyt admin-portaalia.

![](/6_Viikko/kuvat/44.png)

Nyt staattiset elementit näkyvät oikein.


Lähteet:

Karvinen, T. Linux Palvelimet alkukevät 2024. Kuudes etäluento. Suullinen tiedonanto ja visuaaliset esimerkit. 27.2. 2024

Karvinen, T. Deploy Django 4 - Production Install. Luettavissa: https://terokarvinen.com/2022/deploy-django/ Luettu: maaliskuu 2024.

-------------

Tehtävien yhteiset lähteet:

Karvinen, T. Linux Palvelimet alkukevät 2024. Tehtävänannot. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/ luettu: maaliskuu 2024.