# Maalisuora

HostOS: Arch Linux, kernel 6.7.9-arch1-1. Päivitetty 10.03.2024.
Host tietokone: Lenovo Yoga Slim 7, jossa AMD:n R7 4700U -prosessori, 16 Gigatavua muistia ja integroitu Raden RX Vega -näytönohjain. Nvme SSD, jossa vapaata tilaa riittävästi (100+ Gigatavua).

![](/7_Viikko/kuvat/1.png)

GuestOS: Debian Bookworm 12.5 xfce, 4 gigatavua virtuaali-muistia, 4 säiettä, 50 gigatavua tallennustilaa. Virtualisoitu KVM/Qemu/libvirt -pinolla, jonka käyttöliittymänä on virt-manager.

Viikon aiheena on kääntää ohjelmiston lähdekoodi ohjelmaksi ja tehdä siitä suoritettava koko järjestelmänlaajuisesti. Lisäksi viikolla ratkaistaan vanha laboratoriotehtävä, joka käsittää kaikki kurssilla tähän asti opitut asiat soveltavalla tasolla. Viimeiseksi alustetaan seuraavan luentokerran tehtävää varten uusi virtuaalikone, jonka pitää olla niin sanottu puhdas asennus.

Koska HostOS:llä on hyvin tilaa, niin aloitetaan virtuaalikoneen alustamisella ja palataan sen jälkeen yllä mainitun GuestOS:n pariin tekemään tehtäviä. Tämä uusi virtuaalikone alustetaan samoin kuin ylempi GuestOS. Raportti sen alustamiseen löytyy tehtävien [vastausrepositoriosta.](https://github.com/bhi064/LinuxPalvelimetK2024) [Viikolla 5](https://github.com/bhi064/LinuxPalvelimetK2024/blob/main/5_Viikko/viikon-5-palautus.md) tehtävät aloitettiin alustamalla virtuaalikone.
---------------

## Käännä "Hei maailma" haluamallasi kielellä.

Käytetään edellisviikoilta tuttua virtuaalikonettamme nimeltä "florbis". Käytetään Go-ohjelmointikieltä. Sen sijaan, että käyttäisimme paketinhallintatyökaluamme go:n asentamiseen, seurataan virallista [go-dokumentaatiota](https://go.dev/doc/install). Ladataan .tar.gz tiedosto go:n sivuilta, navigoidaan terminaalilla latauskansioon, ja ajetaan ohjeen komento.

	sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.1.linux-amd64.tar.gz

![](/7_Viikko/kuvat/7.png)

Asetetaan nyt path-muuttuja /etc/profile -tiedostoon.

	EDITOR=micro sudoedit /etc/profile

Lisätään tiedoston viimeiselle riville tämä rivi:

    export PATH=$PATH:/usr/local/go/bin

![](/7_Viikko/kuvat/8.png)
	
Kirjaudutaan uudelleen sisään Xfce-työpöydälle.

Varmistetaan, että go-asentui oikein.

![](/7_Viikko/kuvat/9.png)

Listataan go-asennuskansion sisältö.

	ls /usr/local/go
	
![](/7_Viikko/kuvat/10.png)

Ongelma lienee siis PATH muuttujassa. Tarkastetaan vielä profile-tiedosto kuten edellä. Kaikki vaikuttaisi olevan kunnossa, joten en tiedä mistä kiikastaa. Lisätään asennus käyttäjämme .profile -tiedostoon, jolloin asennuksesta pitäisi saada ainakin käyttäjäkohtainen.

    micro ~/.profile

![](/7_Viikko/kuvat/11.png)

Ladataan nyt .profile -tiedosto.

    source ~/.profile

Kysytään nyt go:n versiota.

    go version	

![](/7_Viikko/kuvat/12.png)

Kieli on asennettu. Luodaan käyttäjätilimme kotihakemiston sisältämään Scripts-kansioon oma kansio Go-ohjelmille.

    mkdir -p ~/Scripts/Go

	cd ~/Scripts/Go
	
Luodaan kansioon go:n "Hello World" ohjelma omalla tervehdyksellämme.

    micro Hello

![](/7_Viikko/kuvat/13.png)

Muutetaan vielä tiedostomme nimi .go päätteiseksi.

    mv Hello Hello.go

Rakennetaan nyt go-ohjelmamme.

    go build Hello.go
	
Nyt olemme luoneet "Hello" - tiedoston, joka sisältää ohjelmamme käännettynä suoritettavaan muotoon. On hieman epäselvää, lähteekö Go toimimaan oikein jos luomme virtuaalikoneelle uuden käyttäjän, mutta sen tarkastaminen menee tehtävänannon ulkopuolelle. Voisimme myös installoida go:n käyttäen paketinhallintatyökaluamme, aptiä.

Lähteet: 

Go documentation. Download and Install. Luettavissa: https://go.dev/doc/install. Luettu Maaliskuu 2024.

-----------------------------------------------------------------------------

## Laita Linuxiin uusi komento niin, että kaikki käyttäjät voivat ajaa sitä.

Kopioidaan nyt aikaisemmassa osiossamme kääntämämme Hello-ohjelma /usr/local/bin -kansioon.

    sudo cp Hello /usr/local/bin/Hello

Kokeillaan ajaa ohjelmamme.

    Hello

![](/7_Viikko/kuvat/14.png)

Triviaali Hello World -ohjelmamme toimii kuten haluttu.

Lähde:

Karvinen, T. Linux Palvelimet Kevät 2024. Seitsemäs etäluento. Suullinen tiedonanto. 5.3.2024.

-------------------------------------------------------------------------------

## Ratkaise vanha arvioitava laboratorioharjoitus soveltuvin osin

Tästä raportista ei löydy laboratorioharjoituksen ratkaisua ajanpuutteen ja henkilökohtaisten syiden vuoksi.

Harjoituksissa tehdään kurssilla aiemmin suoritettuja toimenpiteitä soveltaen. Toivottavasti osaamisen esittäminen itse arvioitavassa laboratorioharjoituksessa riittää kurssin suorittamiseen.

---------------------------------------------------------------------------------

## Asenna itsellesi tyhjä virtuaalikone arvioitavaa labraa varten.

Asennus tapahtuu samalla tavalla kuin johdannossa on todettu ja muilla viikoilla referoitu. Ajan säästämisen vuoksi kuvakaappauksia ei ole, mutta tarvittaessa katso viikon 5 ohjeesta.

Listaan valitsemani asetukset:

* GuestOS [debian 12.5.0 bookworm Xfce](https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-12.5.0-amd64-xfce.iso)
* 4096 Mb muistia
* 4 Prosessiointisäiettä
* 50 Gigatavua tallennustilaa
* Virt-managerin sisällä koneen nimi on labra.

![](/7_Viikko/kuvat/2.png)

Aloitetaan asennus suoraan menusta kuten edellisillä viikoilla, käynnistämättä liveCD:tä.

Valitut asetukset:

* Kieli: englanti
* Sijainti: suomi
* Locale: en_US.UTF-8
* Keymap: Finnish
* hostname: deksterinlaboratorio
* Domain name: tyhjä
* root salasana: hyvän salasanakäytännön mukainen
* Käyttäjätili henkilölle anonyymi
* käyttäjänimi bhi064
* Salasana hyvän käytännön mukainen
* Koko levy alustetaan yhdelle osiolle. EXT4 Filesystem.
* Use a network mirror? -> Yes.
* ftp.fi.debian.org
* ei HTTP-proxyä.

Virtuaalikone asentaa debianin, ja se käynnistetään uudelleen.

Kirjaudutaan sisään.

![](/7_Viikko/kuvat/3.png)

Tehdään nyt ne kaksi toimenpidettä jotka meille sallitaan. Päivitetään järjestelmä ja asennetaan tulimuuri ufw.

    sudo apt-get dist-upgrade
	
![](/7_Viikko/kuvat/4.png)

Tehdään sitten tämä root-käyttäjänä.

    su root
	
	sudo apt-get dist-upgrade
	
![](/7_Viikko/kuvat/5.png)

Ja sitten palomuuri:

    sudo apt-get install ufw

![](/7_Viikko/kuvat/6.png)

Jätetään palomuurin konfigurointi tekemättä.

Suljetaan nyt virtuaalikone ja jätetään se odottamaan tiistaita varten.