# H2 Komentaja Pingviini

## Lue ja Tiivistä

### Karvinen 2020: Command line basics revisited

Komentorivi ja sen komentotulkki ovat yksiä varhaisimmista työkaluista Linux ja BSD järjestelmissä jotka ovat edelleen käytössä. Komentorivi tarjoaa tekstimuiotoisen käyttöliittymän tietokoneen käyttämiseen. Linux-jakeluiden graafisissa käyttöliittymissä on terminaali-emulaattori, jonka avulla tietokonetta voi käyttää komentorivin kautta. Tekstimuotoinen syötteiden anto on helposti tietokoneen tulkittavissa, ja sen avulla voidaan tehdä etenkin toistoja nopeasti verrattuna graafisiin käyttöliittymiin.

Linuxissa kaikki elementit ovat tiedostoja, myös kansiot. Komentoriviä käyttäessä ollaan aina jossain kansiossa. Kansion voi tulostaa komennolla

    pwd

Kansioiden välillä voidaan liikkua komennolla

    cd sijainti

Kansion sisällön voi tulostaa komennolla

    ls

Komentoriviltä voidaan myös käynnistää ohjelmia niiden nimen perusteella. Esimerkiksi seuraavat komennot avaavat saman tekstitiedoston eri tekstinkäsittelyohjelmissa:

    nvim Foo.txt
	nano Foo.txt
	pico Foo.txt
	micro Foo.txt

Komentoriviltä voidaan myös luoda, kopioida, siirtää ja poistaa tiedostoja ja kansioita, tiedostoihin voidaan suoraan kirjoittaa esimerkiksi tekstiä, ja niiden sisältöjä voidaan etsiä erilaisten työkalujen avulla.

Terminaaliemulaattori pitää historiaa annetuista komennoista, joka yleensä tulee esille painettaessa nuolinäppäimestä ylös- tai alaspäin. Komentoja voidaan myös etsiä samoin kuin mitä tahansa tiedostosisältöjä esimerkiksi grep-työkalun avulla. Usein terminaaliemulaattoreissa on ns. "autocomplete" -toiminnallisuus, jolla tabulaattori täydentää annetun syötteen ja vaihtaa täydennettyä ehdotusta jos vaihtoehtoja on monia.

Kaikissa linux-jakeluissa on ns. root-kansio, joka sisältää ennalta määritetyt kansiot joihin tallennetaan tietyntyyppisiä tiedostoja. Esimerkiksi ulkoinen media mountataan /media -kansioon, ja konfiguraatiotiedostot tallennetaan /etc -kansioon. Käyttäjän kotihakemisto on yksi tälläinen kansio.

Kaikkia komentoja ei voida suorittaa ilman riittäviä käyttöoikeuksia. Jotkin toiminnot edellyttävät, että ne ajetaan superuser-oikeuksilla, tai että käyttäjä kuuluu johonkin ryhmään jolla on oikeus suorittaa tietty komento. sudo -komento antaa käyttäjän suorittaa jokin annettu komento kohotetuilla oikeuksilla, kunhan käyttäjä antaa salasanan ja hän kuuluu sudo-ryhmään (tai wheel-ryhmään tietyillä jakeluilla), tai käyttäjätili on konfiguroitu sopivaan ns. "sudoers" -tiedostoon.
 
Komentorivillä voidaan suorittaa useita komentoja samalla rivillä, ja komentojen lopputuloksia voidaan putkittaa toisille komennoille.

Lähteet:

Karvinen, Tero. Command Line Basics Revisited. Luettu 29.01.2024. Luettavissa: https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited


## Micro - Asenna micro-editori

Olin asentanut micro-editorin opintojakson etäluennon aikana, joten tehtävän tekemiseksi se on ensin poistettava. Tarkastetaan, että micro todella löytyy virtuaalikoneelta kysymällä sen versiota.

![](/2_Viikko/kuvat/1.png)

Poistetaan micro käyttäen apuna viikolla 1 asentamaamme nala-frontendiä aptille.

![](/2_Viikko/kuvat/2.png)

Tehdään tämän jälkeen laitteistopivitys. Päivityksessä meni noin 2 minuuttia ja siinä päivitettiin 72 pakettia.

![](/2_Viikko/kuvat/3.png)


![](/2_Viikko/kuvat/4.png)

Nyt voimme asentaa micron. Annetaan seuraava komento:

    sudo nala install micro

![](/2_Viikko/kuvat/5.png)

Micro asentui alle 10 sekunnissa.

Kokeillaan nyt käyttää microa. Mennään Documents-kansioon ja tehdään siellä mikrolla muutos edellisviikon helloworld-tiedostoon.

![](/2_Viikko/kuvat/6.png)

Micron käyttöliittymä:

![](/2_Viikko/kuvat/7.png)

Tallennetaan painamalla ctrl+s ja ctrl+q sulkeaksemme micron.

Varmistetaan, että muutos toimi tulostamalla tiedoston sisältö cat-komennolla:

![](/2_Viikko/kuvat/8.png)

Toimii!

Lähde:
Karvinen, Tero. Linux Palvelimet 2024 alkukevät. Etäluento 2.

## Rauta

Listataan tehtävänannon mukaisella komennolla virtuaalikoneen rauta:

![](/2_Viikko/kuvat/9.png)

Virhe: ohjelmaa ei ole asennettu. Asennetaan se.

![](/2_Viikko/kuvat/10.png)

Komento suoritettu 01:47, valmis alle minuutissa.

![](/2_Viikko/kuvat/11.png)

Suoritetaan nyt tehtävänannon komento uudelleen.

![](/2_Viikko/kuvat/12.png)

Listaus on aika pitkä, eikä siihen mahdu kaikki virtuaalikoneen virtualisoima rauta. Tärkeinä huomioina: QEMU virtualisoi mahdollisimman raudanläheisen version HostOS:n prosessorista, ja raportoi sen samalla nimellä kuin HostOS, kuitenkin niin että loogisia prosessoreita on vain 4 normaalin 8 sijaan. Koneen muistiksi raportoidaan 4GiB (Gibibyte) Näytönohjaimena toimii Virtio 1.0 GPU, joka on QEMUn luoma virtualisoitu näytönohjain. HostOS käyttää prosessorin integroitua Vega-pohjaista grafiikkakiihdytintä. Huomataan, että myös moni muu komponentti on virtualisoitu, esimerkiksi verkkokortti, PCIe-väyläohjaimet jne. Komento raportoi myös virtuaalikovalevyn koon.

Tarkemman tehtävänannon puitteissa en osaa analysoida listaa sen paremmin. Virtuaalikoneen raudasta on parempi katsaus viikon 1 tehtävässä.

## Apt
Päätin asentaa ohjelmia, joista olen ennalta tietoinen, mutta joiden käyttöä en osaa muuten kuin korkeintaan aloittelijatasolla. Valitsin ohjelmat tmux (terminal multiplexer), wikit (wikipedia-yhteenvetoja tarjoava ohjelma) ja Chafa (ohjelma konvertoi kuvia ansi/unicode muotoon, jotta ne voidaan näyttää terminaalilla).

![](/2_Viikko/kuvat/13.png)

Yritin asentaa ohjelmat samalla komennolla, mutta tämä ei onnistunut koska chafa oli jo asennettu, ja wikit ei ole aptin repositorioissa käännettynä binäärinä. Chafa on todennäköisesti asennettu viimeviikolla neofetch-ohjelman kanssa. Wikit pitää asentaa node:n avulla sen oman paketinhallintaohjelman kautta.

Yritetään uudelleen. Huomataan, että asennettavia paketteja on nyt todella paljon, koska asennamme koko nodejs-kirjaston kaikkine riippuvuuksineen.

![](/2_Viikko/kuvat/14.png)

![](/2_Viikko/kuvat/15.png)

Ajoin komennon klo 02:14 ja se valmistui 02:15, eli aikaa meni alle 2 min.

Asennetaan nyt wikit.

![](/2_Viikko/kuvat/16.png)

Asennukseen meni 8 sekuntia.


Kokeillaan nyt kaikkia ohjelmia. 

**Wikit**:

![](/2_Viikko/kuvat/17.png)

Ohjelma toimii kuten mainostettu. Se antaa tekstimuotoisen lyhennelmän wikipedian artikkelista.

**Tmux**:

Annetaan komento tmux new. Ohjelma avaa uuden näkymän terminaalista.

![](/2_Viikko/kuvat/18.png)

Annetaan pikanäppäin komento ctrl+b, sen jälkeen dollarimerkki. Nyt voidaan uudelleennimetä sessio. Annetaan sille nimi "LiPa".

![](/2_Viikko/kuvat/19.png)

Ajetaan neofetch (käyttäjänimi sensuroitu).

![](/2_Viikko/kuvat/20.png)

Irtaannutaan sessiosta näppäinkomennolla Ctrl+b, sen jälkeen d.

![](/2_Viikko/kuvat/21.png)

Otetaan taas kiinni sessioon komennolla tmux attach. Huomataan, että taustalle jäi neofetch-ohjelman printtaus.

**Chafa**:

Ladataan netistä jokin vapaasti käytettävissä oleva kuva, jota ei tarvitse lisenssiehdon mukaan attribuoida. Valitsin suomenlipun. Tarkastin chafan syntaksin komennolla
    
	chafa --help

Annetaan chafalle sopiva komento ja katsotaan tuloste:

![](/2_Viikko/kuvat/22.png)

Ohjelma toimii halutulla tavalla.

Lähde:
Tmux cheat sheet. Luettu 29.01.2024. Luettavissa: https://tmuxcheatsheet.com/

Aaron Kili. Tecmint. Wikit - A Command Line Tool to Search Wikipedia on Linux. Luettu 29.01.2024. Luettavissa: https://www.tecmint.com/wikipedia-commandline-tool/

Chafa manual

## FHS 

Tehtävän mainitsemat tärkeät kansiot ovat juurikansio, käyttäjätilien kotikansio, käyttäjän kotikansio, /etc/ kansio, /media/ kansio ja /var/log/ kansio, jossa on järjestelmälogit.

Aloitetaan vaihtamalla juurikansioon ja listaamalla sen sisältö

    cd /
	ls

![](/2_Viikko/kuvat/23.png)

Juurikansio sisältää kaikki tärkeät järjestelmäkansiot. Tummansiniset ovat kansioita, vaaleansiniset tiedostoja. Katsotaan, mitä vmlinuz-tiedostossa on.

![](/2_Viikko/kuvat/24.png)

Tiedoston sisältö on enkoodattu tavalla, jota nano ei osaa lukea. Se sisältää tärkeitä järjestelmän toimimiseen tarkoitettuja tietoja.

Katsotaan nyt kotikansio.

    cd /home/
    ls

Kansiossa tulostetaan vain koneen käyttäjätilien kotihakemistot. Siellä on minun kohdallani vain yksi kansio, joka on nimetty käyttäjätilin mukaan.

Mennään nyt käyttäjän kotikansioon:

    cd
    ls

![](/2_Viikko/kuvat/25.png)

Kansiossa on käyttäjäkohtaisia hakemistoja. Sinne on myös piilotettu ns "dotfile"-tiedostoja, joilla voidaan konfiguroida käyttäjäkohtaisia asetuksia eri ohjelmille. Annetaan komento:

    ls -a

![](/2_Viikko/kuvat/26.png)

Huomataan, että kotikansiossa olikin myös paljon sellaista tietoa, joka on normaalisti piilotettu.

Katsotaan .profile-tiedoston sisältö.

![](/2_Viikko/kuvat/27.png)

Profile-tiedostoon olikin tehnyt muutoksena sen, että terminaali ei näytä sisäänkirjautunutta käyttäjää muodossa käyttäjänimi@domain, vaan pelkän $ merkin.

Katsotaan seuraavaksi /etc/ kansio.

    cd /etc/
	ls
	
![](/2_Viikko/kuvat/28.png)

Kansiossa on suuri määrä eri ohjelmien omia konfigurointikansioita ja tiedostoja. Sitä kautta voidaan antaa globaaleja muutoksia ohjelmien toimintaan kaikille käyttäjille. Yleensä kun asennetaan jokin ohjelma, ensimmäinen toimenpide on tarkastaa /etc/ -kansiosta sen konfiguraatio ja tehdä tarpeen vaatiessa muutoksia.

Katsotaan esimerkiksi sudoers -tiedoston sisältö. Huomaa, että tätä varten tulee käyttää komentoa sudo editorin valinnan yhteydessä, ja muutokset olisi vielä parempi tehdä tiedoston itsensä ohjeiden mukaan.

![](/2_Viikko/kuvat/29.png)

Sudoers antaa siis käyttäjille oikeudet sudo-komennon käyttöön.

Katsotaan seuraavaksi /media/ kansio.

    cd /media/
    ls

![](/2_Viikko/kuvat/30.png)

Kansiossa on kiinnityspisteet virtualisoidulle cd-asemalle. Jos järjestelmään kiinnitetään "ulkoista mediaa" on sen kiinnityspiste tämä kansio.

Katsotaan seuraavaksi logit.

    cd /var/log/
	ls

![](/2_Viikko/kuvat/31.png)

Kansio sisältää eri ohjelmien ja järjestelmän logitiedostoja. Katsotaan vaikka apt-kansiosta history.log tiedosto.

![](/2_Viikko/kuvat/32.png)

Tiedosto kertoo, mitä ohjelmia mikäkin käyttäjä on halunnut asentaa ja milloin.

## The Friendly M. Näytä 2-3 kuvaavaa esimerkkiä grep-komennon käytöstä.

Grep on ohjelma, joka palauttaa jostain lähteestä kaikki rivit, joilta löytyy hakutermiä vastaava lauseke. Esimerkiksi edellisen tehtävän aptin history.log tiedostosta voidaan katsoa, onko jokin ohjelma asennettu aptin avulla, vaikkapa micro.

    grep micro history.log

![](/2_Viikko/kuvat/33.png)

microlle on siis tehty useita toimenpiteitä. Se on asennettu, postettu, asennettu, ja sen jälkeen jokin nodejs:n yhteydessä asennettu tiedosto sisältää tekstin "micro", joten koko rivi on mukana grepin tulosteessa.

Lasketaan, kuinka monta install -komentoa aptillä on suoritettu.

    grep -c Install history.log

Komentorivi palauttaa tulosteen 11. Aptin logeissa on siis 11 riviä, joista löytyy teksti "Install". Huomaa, että teksti on case-sensitive, eli isoilla ja pienillä kirjaimilla on väliä.

![](/2_Viikko/kuvat/34.png)

Lähde:

grep --help -komento.

## Pipe. Näytä esimerkki putkista.

Lasketaan, kuinka monta kertaa sana "Finland" esiintyy wikitin referaatissa Finland-artikkelissa.

    wikit Finland | grep -o 'Finland' | wc -l

Komentorivi raportoi tulokseksi 17 kertaa.

Lähde:

grep --help -komento.
wc --help -komento.

## Tukki.


Aiheutetaan logitapaus epäonnistuneesta toimenpiteestä. Suoritetaan komento:

    su root

Antaen väärä salasana. Katsotaan nyt logit komennolla:

    journalctl --user
	
Kelataan alimmalle riville välilyöntiä näpyttämällä ja huomataan logikirjaus:

![](/2_Viikko/kuvat/35.png)

Käyttäjänimi on sensuroitu. Huomataan, että käytäjä on login perusteella yrittänyt kirjautua root-käyttäjäksi, mutta tämä ei ole onnistunut (root-käyttäjä on disabloitu järjestelmässä asennusvaiheessa.)

Rivillä näkyy aikaleima, domain, komento, virheilmoitus, käyttäjän id, käyttäjätili ja missä tiedostossa virhe tapahtui /dev/pts/0.

Aiheutetaan nyt onnistunut logitapaus. Annetaan komento.

    sudo nala upgrade

Typotin salasanan kerran, mutta kirjoitin sen seuraavaksi oikein. Katsotaan logit samalla komennolla kuin ennen, ja kelataan loppuun välilyönnillä.

![](/2_Viikko/kuvat/36.png)

Huomataan, että komento nala upgrade on nyt suoritettu onnistuneesti ja suljettu. Mitään päivitettävää ei ollut, koska komento sulkeutui välittömästi.

Logiriveiltä löytyy aikaleima, domain, komennon id, mitä komennolla tehtiin. Kuka teki ja millä käyttäjällä.

Tämän tarkempaa analyysiä en tässä vaiheessa login sisällöstä osaa antaa.

Lähde:

journalctl manuaali-sivu.

Lähteet:
Karvinen, Tero. Command Line Basics Revisited. Luettu 29.01.2024. Luettavissa: https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited

## Yhteiset Lähteet

Karvinen, Tero. Linux Palvelimet 2024 alkukevät. Tehtävänannot. Luettu 29.01.2024 Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/

## Lisenssi

Tämä harjoitustyö on julkaistu GPLv3 lisenssin alla. 