# Koko Juttu

HostOS: Arch Linux, kernel 6.7.6-arch1-1. Päivitetty 26.02.2024.
Host tietokone: Lenovo Yoga Slim 7, jossa AMD:n R7 4700U -prosessori, 16 Gigatavua muistia ja integroitu Raden RX Vega -näytönohjain. Nvme SSD, jossa vapaata tilaa riittävästi (100+ Gigatavua).

![](/5_Viikko/kuvat/1.png)

GuestOS: asennetaan tehtävän aikana. Virtualisointi tapahtuu KVM/QEMU/Libvirt pinolla, jonka hallintaan käytetään virt-manager graafista käyttöliittymää.

## Koko juttu. Asenna uusi, tyhjä virtuaalikone. Tee koneelle tavalliset alkutoimet. Asenna sille Apache-weppipalvelin ja SSH-etähallintapalvelin. Tee uusi etusivu weppipalvelimelle niin, että sivuja voi muokata normaalikäyttäjän oikeuksin. Käytä tässä name based virtual host -tekniikkaa.

Käytetään apuna aikaisempien viikkojen raportteja. Viikoittain eriteltyinä ja luettavissa: https://github.com/bhi064/LinuxPalvelimetK2024

Aloitetaan luomalla Virt-Managerissa uusi virtuaalikone.

![](/5_Viikko/kuvat/2.png)

GuestOS on Debian 12.4 Bookworm Xfce-työpöydällä.

![](/5_Viikko/kuvat/3.png)

Muistia 4 gigatavua ja 4 virtuaalista prosessointisäiettä.

![](/5_Viikko/kuvat/4.png)

40 Gigatavua tallennustilaa.

![](/5_Viikko/kuvat/5.png)

Annetaan virtuaalikoneelle Virt-managerin sisällä nimi "florbis". Tämä tulee olemaan myös koneen hostname.

![](/5_Viikko/kuvat/6.png)

Hyväksytään asetukset ja käynnistetään virtuaalikone. Käynnistyksen yhteydessä valitaan graafinen asennusohjelma, sen sijaan että käynnistäisimme liveCD:n.

![](/5_Viikko/kuvat/7.png)

Seuraa pitkä virtuaalikoneen asennusprosessi. Tässä on listattuna vain käytetyt asetukset. Tarvittaessa [viikon 1 raportissa](https://github.com/bhi064/LinuxPalvelimetK2024/blob/main/1_Viikko/viikon-1-palautus.md) on tarkempi kuvaus asennuksesta.

Aloitus 01:21

Asetukset:

* English
* Other -> Europe -> Finland
* Locales en_US.UTF8
* Finnish keyboard
* Hostname: Florbis
* Domain name: tyhjä
* Root salasana hyvän käytännön mukainen
* Käyttäjän nimi: anonyymi
* Käyttäjätilin nimi: ritarirollo
* Salasana hyvän käytännön mukainen
* Asennus koko levylle, kaikki samaan osioon.

Järjestelmän asennus alkoi:
Klo 01:24

Järjestelmän asennus päättyi:
Klo 01:28

* Network mirror: Finland. deb.debian.org. Ei HTTP proxyä.

Asennuksen finalisointi päättyi:
Klo 01:31

Käynnistetään virtuaalikone nyt uudelleen.

Ensimmäisenä järjestelmän päivitys.

    sudo apt-get update && sudo apt-get upgrade
	
Ei onnistu, koska käyttäjällämme ei sudo-oikeuksia. Annetaan oikeudet.

    su root
    sudo usermod -aG sudo ritarirollo

![](/5_Viikko/kuvat/8.png)

Kirjaudutaan ulos Xfce:stä, sitten takaisin sisään.

    sudo apt-get update && sudo apt-get upgrade

Ajettu klo 01:38. Valmis klo 01:42.

Asennetaan ja konfiguroidaan apache.

    sudo apt-get install apache2

    cd /etc/apache2/sites-available

    sudoedit omasivu.example.com.conf
	
(LISÄYS: Huomaa ylempänä, että alunperin tässä tapahtui sivua nimetessä kirjoitusvirhe, josta myöhemmät ongelmat tulevat johtumaan. tiedosto on omasivu.example.edit.conf, eikä omasivu.example.com.conf)
	
![](/5_Viikko/kuvat/9.png)

Luodaan nyt apacheen määritetty kansiorakenne ja index.html sivu jota tarjoillaan vierailijoille.

    mkdir -p /home/ritarirollo/omatVerkkosivut/omasivu.example.com
    
	touch /home/ritarirollo/omatVerkkosivut/omasivu/index.html
    
	cat /home/ritarirollo/omatVerkkosivut/omasivu/index.html
    ranskalainen viikinki
    ctrl + C

Simuloidaan nimipalvelua hosts-tiedoston kautta.

    sudoedit /etc/hosts/

![](/5_Viikko/kuvat/10.png)

Laitetaan sivu päälle

    sudo a2ensite omasivu.example.com

    sudo systemctl start apache2

Apache ei lähde käyntiin. Saadaan virhe:

![](/5_Viikko/kuvat/11.png)

Käynnistetään virtuaalikone uudelleen, josko apachessa olisi jokin vika joka estää sitä toimimasta.

Kirjaudutaan sisään.

    sudo systemctl start apache2

Saadaan sama virhe. Kysytään apache2 daemonin tilaa.

    sudo systemctl status apache2

![](/5_Viikko/kuvat/12.png)

Virheilmoitus ohjaa meidät apachen .conf -tiedostoon. Katsotaan sitä, mutta asennetaan ensin micro.

    sudo apt-get install micro

    EDITOR=micro sudoedit /etc/apache2/apache2.conf

![](/5_Viikko/kuvat/13.png)

Syntaksissa ei pitäisi olla mitään vikaa. Tutkin asiaa googlen avulla pitkään, mutta en löytänyt ilmiselvää syytä, mistä virhe johtuisi. Ajan puutteen vuoksi lienee nopeampaa aloittaa alusta, joten toistetaan nyt tehdyt toimenpiteet virtuaalikoneen asennukseksi, mutta seuraavalla muutoksella.

(LISÄYS: Mutta syntaksissa oli kuin olikin virhe. Jossain kohdassa tätä sarjaa olin uudelleennimennyt omasivu.example.edit.conf -> omasivu.example.com.conf, joka oli saanut apachen ilmeisesti juntturaan. Apache luuli, että kaiken pitäisi olla kunnossa, mutta sen etsimää tiedostoa ei virtuaalikoneessa ollutkaan. Henkilökohtaisista syistä aikaa tehtävien tekemiseen ei ole ollut kovin paljon ja tässä kohtaa väsymys painoi niin paljon ettei jo nyt poikkeuksellisen pitkän raportin kirjoittaminen napostellut, kun ilman raporttia tehtävä tähän asti olisi ollut verkkaasti suoritettu.)

Haetaan uusi Debian 12 xfce live ISO osoitteesta: https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-12.5.0-amd64-xfce.iso

Tämä ISO on versio 12.5.0, ja siten uudempi kuin aikaisemmin käyttämämme ISO. Käynnistetään myös HostOS uudelleen.

![](/5_Viikko/kuvat/14.png)

Annetaan varmuuden vuoksi 10 gigaa enemmän tilaa kuin ennen.

![](/5_Viikko/kuvat/15.png)

Uudelleenasennus aloitettiin klo 02:44. Asennus oli valmis uudelleenkäynnistystä varten klo 02:51.

Annetaan taas käyttäjällemme sudo-oikeudet ja kirjaudutaan uudelleen xfce:n työpöydälle.

Asennetaan nala, niin nähdään helpommin, mitä kaikkea updatet päivittävät.

    sudo apt-install nala

    sudo nala update && sudo nala upgrade

![](/5_Viikko/kuvat/16.png)

Ei mitään ilmiselviä muutoksia, mutta meillä on nyt käytössä uudempi versio debianista kuin ennen (12.5, eikä 12.4).

Asennetaan apache ja ufw

    sudo nala install apache2
    sudo nala install ufw

    sudo ufw allow 22/tcp
    sudo ufw allow 80/tcp
    sudo ufw enable
    sudo systemctl enable apache2

Tehdään nyt **ensin** kansiot, joihin omat verkkosivut laitetaan.

    mkdir -p /home/ritarirollo/omatVerkkosivut/omasivu.example.com
    touch /home/ritarirollo/omatVerkkosivut/omasivu.example.com/index.html
    echo "ranskalainen viikinki" > /home/ritarirollo/omatVerkkosivut/omasivu.example.com/index.html

Tarkastetaan kotikansion oikeudet.

    cd
    cd ..
    ls -l

![](/5_Viikko/kuvat/17.png)

Annetaan suoritusoikeudet käyttäjämme kotikansiolle, jotta apache voi näyttää sivuja.

![](/5_Viikko/kuvat/18.png)

konfiguroidaan nyt apache.

    cd /etc/apache2/

    sudo a2dissite 000-default.conf

    sudo systemctl restart apache2

Luodaan sites-available kansioon sopiva configuraatiotiedosto.

    cd sites-available

    sudoedit omasivu.example.com

![](/5_Viikko/kuvat/19.png)

    sudo a2ensite omasivu.example.com

![](/5_Viikko/kuvat/20.png)

saadaan uusi virheilmoitus. Sivua ei ole. tarkastetaan.

    ls /home/ritarirollo/omatVerkkosivut/omasivu.example.com

![](/5_Viikko/kuvat/21.png)

Tiedostopolku on kyllä oikein. Kokeillaan antaa suoritusoikeudet /home/ kansiolle.

    cd /

    sudo chmod a+x /home/

    sudo a2ensite omasivu.example.com

Saadaan sama virheilmoitus. Mennään apachen konfiguraatiokansioon.

    cd /etc/apache2/sites-available/

    ls

![](/5_Viikko/kuvat/22.png)

Huomataan, että .conf - pääte puuttuu.

    sudo cp omasivu.example.com omasivu.example.com.conf

    sudo a2ensite omasivu.example.com

![](/5_Viikko/kuvat/23.png)

Nyt vaikuttaisi toimivan.

    sudo systemctl restart apache2

Ja sitten hosts-tiedoston editointi.

    sudoedit /etc/hosts

![](/5_Viikko/kuvat/24.png)

Curlataan sivut

![](/5_Viikko/kuvat/25.png)

Tarkastetaan selaimella

![](/5_Viikko/kuvat/26.png)

Tarkastetaan, että hosts -tiedosto ohjaa www.google.com osoitteen sivuillemme kuten asetimme yllä (tämä toimii vain omassa virtuaalikoneessa):

![](/5_Viikko/kuvat/27.png)

Hienosti toimii.

Asennetaan ssh-palvelin

    sudo nala install openssh-server

    sudo systemctl start ssh

Testataan nyt ssh:ta. Vaihdetaan käyttäjää.

    su root

    ssh ritarirollo@florbis

![](/5_Viikko/kuvat/28.png)

ssh toimii.

*Virhearvio*:

Luettuani tätä raporttia, huomasin, että ensimmäisellä kerralla localhostissa tarjoiltava verkkosivu oltiin nimetty omasivu.example.edit.conf, eikä omasivu.example.com. Tämä nimi oltiin myöhemmin vaihdettu. Lieneekö siinä ongelmien juuri?

Lähteet:

Karvinen, T. Linux Palvelimet Kevät 2024. Viides luentokerta. Suullinen tiedonanto. 13.03.2024


## Pubkey. Automatisoi kirjautuminen julkisella SSH-avaimella.

Luodaan root-käyttäjälle ssh avain.

    ssh-keygen

Viedään ssh-avain halutulle käyttäjälle halutulle tietokoneelle (eli tässä tapauksessa omalle tietokoneellemme)

    ssh-copy-id ritarirollo@florbis
	
Järjestelmä haluaa, että annamme käyttäjän salasanan.

![](/5_Viikko/kuvat/29.png)

Kokeillaan kirjautua sisään:

    ssh ritarirollo@florbis
	
![](/5_Viikko/kuvat/30.png)

osoite "florbis" on määritetty tietokoneemme hosts-tiedostossa. Se voisi olla myös jokin toinen osoite, kuten etäpalvelintehtävissä tehtiin.

kirjaudutaan nyt ulos ssh:sta.

    exit
	
Olemme nyt root-käyttäjä. Kirjaudutaan ulos root-käyttäjästä.

    exit
	
Olemme takaisin omalla käyttäjällämme terminaalissa.

Lähteet: 

Karvinen, T. LinuxPalvelimet kevät 2024. Suullinen tiedonanto. Viides luento. 13.02.2024.
Karvinen, T. Linux Palvelimet kevät 2024. Tehtävävinkit. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/. Luettu Helmikuu 2024.

## Digging host. Tutki domain-nimesi nimesi tietoja 'host' ja 'dig' -komennoilla. Analysoi tulokset. Vertaa tuloksia nimen vuokraajan (namecheap.com, name.com...) weppiliittymässä näkyviin asetuksiin. (Jos sinulla ei ole omaa nimeä käytössä, voit tutkia jotain muuta nimeä).

Aloitetaan asentamalla bind9-dnsutils ja bind9-host.

Minulla ei ole domain-nimeä, joten tutkitaan nimeä terokarvinen.com

    host terokarvinen.com

![](/5_Viikko/kuvat/31.png)

Host kertoo sivun ip-osoitteen ja mail exchange osoitteen.

    dig terokarvinen.com
	
![](/5_Viikko/kuvat/32.png)

Komento kertoo, mikä tietokone käyttää dig-komentoa mihin osoitteeseen. Komento kertoo, että +cmd optiot ovat oletuksena käytössä. Seuraavaksi kerrotaan vastauksen metatietoja: tehtiin kysely, ei palautettu virheitä, kyselyllä on yksilöity tunniste, ja kerrotaan, mitä flagejä, eli optioita on käytösssä.

OPT PSEUDOSECTION kertoo meille valinnaista metadataa, mutta kyseinen osio ei ole helposti tulkittavissa. Ilmeisesti siinä kerrottaisiin, mitä porttia mahdollinen UDP yhteys käyttäisi.

Seuraavaksi näytetään kysely: etsitään terokarvinen.comia A-recordeissa. Siihen on vastaus, jossa palautetaan A recordin IP-osoite, joka on sama kuin host-komennon palauttama. Se ei kerro, missä mx-postipalvelin sijaitsee.

Lopuksi näytetään, milloin kysely on tehty, sekä vastauksen koko.

Lähteet:

Linux Handbook. Dig command in Linux Explained. Luettavissa: https://linuxhandbook.com/dig-command/ Luettu: Helmikuu 2024.
Dig manpage. Luettu Helmikuu 2024.

Yhteiset Lähteet:

Omat muistiinpanot aikaisempien viikkojen tehtäviin. Luettavissa: https://github.com/bhi064/LinuxPalvelimetK2024

Tehtävänannot:

Karvinen, T. Linux Palvelimet 2024 alkukevät. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/. Luettu: Helmikuu 2024


HUOMIO: Tehtävää on paranneltu 10.3.2024. Lisätty alkuun maininta GuesOS:n toteutuksesta, sekä muutama lause ennen kuvakaappauksia koska saamani palautteen perusteella raporttia oli vaikea seurata.

