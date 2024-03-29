# H1 Oma Linux

**HUOMIO:** Koko harjoitustyö, mukaanlukien artikkelien lukeminen, referaatit ja linuxin installointi virtuaalikoneelle on palautuksen kommentin mukaisesti tehty 21.01.2024 klo 23:50 - 22.01.2024 klo 02:45. Tämä huomio on lisätty harjoitustyöhön 22.01.2024 klo 03:45 näkyvyyden parantamiseksi. Tämän tarkempia suoritusaikoja komennoille ei ole tässä raportissa esitelty, koska kaikki toimenpiteet olivat nopeita, virheitä ei kohdattu ja käyttämäni tietokone oli hyvin konfiguroitu uusien virtuaalikoneiden luomista varten.

## Lue ja tiivistä

### Raportin kirjoittaminen.

Opintojaksolla painotetaan harjoitustöiden raportoimista niiden tekemisen lisäksi, jonka avulla voidaan todentaa, että opiskelija on todellisesti tehnyt harjoitustyöt sekä etsinyt omatoimisesti tietoa, jota tarvitsee töiden tekemiseen. Raportoinnin tärkein ominaisuus on se, että harjoitustyö on sen avulla toistettava.

**Hyvän raportin ominaisuudet**

1. **Toistettava**: raportin ohjeita seuraamalla on mahdollista toistaa työ virheilmoituksia myöten.
2. **Täsmällinen**: raportissa on kirjattu tarkasti käytetyt komennot, suoritukseen kulunut aika, onnistumiset ja epäonnistumiset, sekä kohdatut virheilmoitukset.
3. **Helppolukuinen**: raporttia tulee olla helppo seurata.
4. **Viittaa lähteisiin**: raportista ilmenee, miten harjoitustyön tekemistä varten tarvittu tieto on saatu.

Lisäksi hyvässä raportissa on merkittynä, minkä lisenssin alla se on julkaistu. Raportti ei myöskään saa sisältää vilppiä missään sen muodossa.

Lähde:

Karvinen, Tero. Raportin kirjoittaminen. Julkaistu 04.06.2006. Luettu 21.01.2024. https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

### FSF Free Software Definition

Artikkelissa Free Software Foundation määrittelee vapaan ohjelmiston kriteerit. Vapaa ohjelmisto on muutakin kuin vain ohjelmisto, jonka lähdekoodi on avoimesti saatavilla. Ollakseen vapaa, FSF määrittelee ohjelmistoille seuraavat vaativuudet:

* Vapaus 0: Vapaus suorittaa ohjelmistoa kuten haluat, mitä tahansa tarkoitusta varten.
* Vapaus 1: Vapaus tutkia miten ohjelmisto toimii, sekä muokata sitä toimimaan kuten haluat. Ohjelmiston lähdekoodi on oltava saatavissa tämän vapauden toteutumista varten.
* Vapaus 2: Vapaus jakaa ohjelmiston kopioita voidaksesi auttaa muita.
* Vapaus 3: Vapaus jakaa ohjelmistosta muokkaamasi versioita toisille.

Vapaan ohjelmiston ei välttämättä tule olla ilmainen. Usein vapaat ohjelmistot ovat kuitenkin ilmaiseksi saatavilla, sillä niiden vapaat lisenssiehdot takaavat sen, että niitä ja niistä muokattuja versioita tulee voida jakaa toisille käyttäjille.

Artikkeli tarkentaa vapaan ohjelmiston määrittelyä tiettyjen rajaehtojen kohdalla. Ohjelmisto ei esimerkiksi voi olla vapaa, jos se jaetaan laitteiston kanssa, joka ei suostu ajamaan muokattuja versioita ohjelmistosta. Esimerkkejä tämän vapauden rikkomisesta ovat Tivoization, lockdown ja secure boot. Se ei myöskään ole vapaa, jos sen lisenssissä rajoitetaan käyttäjän mahdollisuuksia julkaista omat muokkauksensa ohjelmistosta rajoitetun lisenssin alla. Käyttäjällä itsellään tulee olla vapaus määrittää omien muokkauksiensa avulla tehdyn ohjelmiston julkaisulisenssi.

Ohjelmiston kehitäjä ei myöskään voi lisätä takautuvasti uusia lisenssiehtoja julkaisemalleen ohjelmistolle. Mikäli käyttöoikeus voidaan takautuvasti poistaa, tai käyttöehtoja muokata, ei lisenssi voi olla vapaan ohjelmiston kriteerien määrittelemä. Vapaan ohjelmiston lisenssissä ensisijainen vapaus on käyttäjällä, ei ohjelmiston tuottajalla tai julkaisijalla.

Lähde: 

Free Software Foundation. What is Free Software? Julkaistu 01.01.2024. Luettu 21.01.2024.
https://www.gnu.org/philosophy/free-sw.html


## Linuxin asentaminen virtuaalikoneeseen.

Viikkotehtävien toisessa osiossa tehtävänä on asentaa Linux-käyttöjärjestelmä virtuaalikoneeseen. Harjoitustyötä varten valitsin asentaa Debian -jakelun version 12.4.0 x64 version XFCE työpöydällä opintojakson opettajan ja tehtävän vinkkien mukaisesti.

Virtuaalikoneen isäntänä käytän Lenovon Yoga Slim 7 -kannettavaa tietokonetta, jossa on 16 gigatavua muistia ja AMD Ryzen 4700u prosessori. Isäntäkoneen käyttöjärjestelmä on Arch Linux (Kernel 6.7.0-arch3-1, joka tulee päivittymään kurssin edetessä). Desktop Environment on KDE:n Plasma 5. Oheessa kuvakaappaus Neofetch -ohjelman etsimistä tietokoneen tiedoista. Arch käyttää järjestelmän toteuttamiseen Systemd -daemonia.

![Host OS](/1_Viikko/kuvat/hostos.png)

Kysyin luvan käyttää virtualisoijana (Hypervisor) VirtualBoxin sijaan QEMU -ohjelmaa, joka hyödyntää linux-ytimeen kuuluvaa KVM (Kernel Virtual Machine) ohjelmaa. QEMU käyttää toimiakseen libvirt -palveluja. Virtuaalikonetta ajetaan graafisen virt-manager käyttöliittymän kautta. Näiden ohjelmistojen asennusta ja konfigurointia ei esitetä tässä raportissa, sillä ne ylittävät tehtävän laajuuden. Toistettavuuden nimissä raportin lähteistä löytyy kuitenkin linkki ArchWikiin, jossa on tarkat ohjeet ohjelmistojen asennusta varten. Itse virtuaalikoneen asetukset esitellään virt-managerin kautta, ja niitä muokataan vielä asennuksen jälkeen.

Virt-manager avataan käyttöjärjestelmän valikon kautta, ja ohjelmasta valitaan uuden virtuaalikoneen luonti.

![New VM](/1_Viikko/kuvat/new-vm.png)

Virtuaalikoneen asetukset valitaan luontivaiheessa, ja niiden hyväksymisen jälkeen virtuaalikone käynnistyy. 

![Osa1](/1_Viikko/kuvat/1-create-vm.png)

![Osa2](/1_Viikko/kuvat/2-create-vm.png)

![Osa3](/1_Viikko/kuvat/3-create-vm.png)

![Osa4](/1_Viikko/kuvat/4-create-vm.png)

![Osa5](/1_Viikko/kuvat/5-create-vm.png)

GRUB -menusta valitaan vaihtoehto asentaa käyttöjärjestelmä graafisesti.

![Debian installer](/1_Viikko/kuvat/deb-start-installer.png)

Käyttöjärjestelmän asennuksessa seurataan asennusohjelman ohjeita. Asennusta varten valitaan seuraavat vaihtoehdot:

* Kieli: englanti.
* Sijainti: Eurooppa > Suomi
* Locale: en_GB.UTF-8
* Näppäimistö: Suomalainen
* Hostname: draupnir
* Domain name: jätetään tyhjäksi
* Root salasana: jätetään tyhjäksi. On hyvän tietoturvakäytännön mukaista, että root käyttäjää ei ole. Tällöin mahdollisen hyökkääjän on vaikeampi tunnistaa käyttäjätilit, joilla on oikeudet suorittaa mitä tahansa komentoja tietokoneella.
* Käyttäjän koko nimi: pseudonyymi
* Käyttäjätilin nimi: pseudonyymi, joka jätetään turvallisuussyistä tässä mainitsematta. Salasana hyvän käytännön mukainen.
* Virtuaalisen levyn alustus: ohjattu, koko levy salatulla LVM:llä (logical Volume Manager)
* Kaikki tiedostot samalla osiolla.
* LVM:n salasana: hyvän käytännön mukainen.
* LVM:n koko: koko levy.
* Paketinhallintaohjelman peilipalvelimet: Käytössä. Suomi > ftp.fi.debian.org. Ei HTTP proxyä.

Oheessa kuva levyjen osioinnista

![Partitions](/1_Viikko/kuvat/partitions.png)

Näiden valintojen jälkeen virtuaalikone käynnistettiin uudelleen asennusohjelman ohjeiden mukaisesti.

Virtuaalikone käynnistyy XFCE:n perusnäkymään. Annetaan ohjelmalle käyttäjätilin nimi ja salasana, ja käynnistetään XFCE-työpöytänäkymä. Virtuaalikone on nyt asennettu. Tehdään vielä muutamia huoltotoimenpiteitä.

![xfce](/1_Viikko/kuvat/xfce-desktop.png)

Asennetaan **nala** (front-end apt- pakettihallinnalle, jatkossa voidaan asentaa ohjelmia komennolla sudo nala *paketin nimi*), **neofetch** (järjestelmän asetuksia raportoiva ohjelma), ja **neovim** (vi- ja vim- tekstieditorien pohjalta tehty tekstieditori). Tämä tapahtuu ajamalla seuraava komento:

    sudo apt install nala neofetch neovim

Komennon jälkeen hyväksytään painamalla y

![apt confirm](/1_Viikko/kuvat/apt-confirm.png)

Ajetaan neofetch ja katsotaan mitä se raportoi.

![neofetch initial](/1_Viikko/kuvat/vm-initial-neofetch.png)

Suljetaan nyt virtuaalikone ja muokataan sen asetuksia virt-managerissa.

![vm open infos](/1_Viikko/kuvat/vm-open-infos.png)

![vm hw info](/1_Viikko/kuvat/virt-manager-hw-info.png)

Avataan virtuaalikoneen hardware-näkymä, ja muutetaan grafiikka-asetuksia. Valitaan vaihtoehdoksi "vga". Hyväksytään painamalla apply.

![change gpu](/1_Viikko/kuvat/virt-manager-change-gpu.png)

Käynnistetään nyt virtuaalikone uudelleen ja kirjaudutaan sisään. Suortetaan neofetch ohjelma uudelleen.

![vm restart](/1_Viikko/kuvat/vm-restart.png)

![grub start deb](/1_Viikko/kuvat/grub-debian-start.png)

![neofetch initial](/1_Viikko/kuvat/vm-gpu-changed.png)

Huomataan, että raportoitu GPU on muuttunut. Virtuaalikoneen virtualisoitua hardwarea on siis mahdollista muuttaa. Virtuaalikoneelle voitaisiin myös antaa suoraan käyttöönsä isäntäkoneen grafiikkaohjain, eikä jotakin virtualisoitua grafiikkaohjainta, mutta koska kyseessä on vain kannettava tietokone integroidulla grafiikkapiirillä, en koe tarpeelliseksi konfiguroida GPU passthrough-mallia. Sen sijaan vaihdetaan virtuaalikone takaisin käyttämään Virtio- grafiikkamallinnusta ja hyväksytään 3d-kiihdytys kuten edellä.

Virtuaalikone on nyt käytettävissä siten, kuten käyttäjä sitä haluaa käyttää.

Lähteet:

ArchWiki, luettu syksyllä 2023.

https://wiki.archlinux.org/title/QEMU

https://wiki.archlinux.org/title/Libvirt

https://wiki.archlinux.org/title/Virt-manager

Karvinen, Tero. Linux Palvelimet - opintojakso. Ensimmäinen etäluento. Suullinen tiedonanto ja audiovisuaalinen ohjeistus. Kevät 2024.

## Vapaaehtoinen bonus: Suosikkiohjelmani Linuxilla

HUOM: osiota varten on edellä asennettu nvim-ohjelma virtuaalikoneen asennuksen yhteydessä.

Avataan virtuaalikone ja kirjaudutaan käyttäjällä sisään. Avataan terminaali (pika näppäin-komento ctrl + alt + t). Annetaan seuraavat komennot:

    cd Documents/
    nvim helloworld.txt

Painetaan i -näppäintä päästäksemme insert-moodiin. Kirjoitetaan lyhyt viesti.

![hello world](/1_Viikko/kuvat/nvim-hello.png)

Painetaan esc jotta päästään taas perusmoodiin. Annetaan komento:

    :wq

Jolla tiedosto tallennetaan ja nvim suljetaan. Annetaan nyt komento:

    cat helloworld.txt

Terminaalista voidaan lukea antamamme lyhyt viesti, eli tiedoston sisältö.

Lähteet:

neovim help.txt ohje. Saatavilla käynnistämällä neovim -ohjelma ja antamalla komento :help.

Lisenssi: Harjoitustyö on julkaistu GPL-3 lisenssin alla. Lisenssi on luettavissa palautuksen Github-repositorissa.