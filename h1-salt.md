# h1 - Hei maailma!
******************************

Tätä harjoitusta tehdessä toimin omalla pöytäkoneellani, jolla on asennettu Windows 10, tietääkseni kaikkine viimeisimpine päivityksineen. Toteutin harjoituksen 31.3.-1.4. aikana.

### Lue ja tiivistä:
Karvinen 2023: Create a Web Page Using Github <i>https://terokarvinen.com/2023/create-a-web-page-using-github/</i>

Nettisivujen luominen Githubilla on todella helppoa ja nopeaa.
  - Rekisteröidy ja luo uusi <i>repository</i>
  - Lisää .md tiedosto -  tästä tulee uusi nettisivu.
  - Lisää tekstiä äsken luotuun tiedostoon ja paina <i>commit</i>.
  Valmista!
  
Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves <i>https://terokarvinen.com/2023/salt-vagrant/</i>

Tiivis tietopaketti siitä, miten Vagrant ohjelmistolla ja Saltilla (configuration management system) luodaan ja hallitaan tietokoneita. Tässä kyseisessä harjoituksessa yksi master-tietokone ja kaksi orja-tietokonetta. 
  - Konfigurointitiedot tietokoneista, jotka halutaan luoda ovat Vagrantfile:ssa.
  - Orjien avaimet hyväksytään komennolla <i>sudo salt-key -A</i>, jossa -A tarkoittaa kaikki.
  - Voidaan käyttää tavallisia komentorivin komentoja.
  - Orjilta voidaan pyytää tietoja, esimerkiksi mikä on niiden IP-osoite tai käyttöjärjestelmä.
  - state.single komennoilla kuvataan haluttu lopputila, muutoksia tehdään vain jos tarvitaan.
  - Voidaan luoda erilaisia tiloja, kuten ko. artikkelissa "hello" ja niitä voidaan ajaa tietyille tai kaikille orjille.
  - Top.sls tiedosto määrää, mitkä tilat ajetaan mille orjille.

### Asenna Debian 11 Vagrantilla
Alkuun piti asentaa Vagrant koneelle. Käytän kotipöytäkoneellani Windows 10. Joten ensin latasin Vagrant Windows AMD64 binary täältä: https://developer.hashicorp.com/vagrant/downloads .

![image](https://user-images.githubusercontent.com/78509164/229228996-33fff4ef-3a2b-4847-8665-30fcd71c8d37.png)

Asennuksen jälkeen piti vielä uudelleenkäynnistää kone, mutta sitten pääsin jatkamaan.

### Asenna Vagrant filessa konfiguroitu kolmen koneen verkko
Avasin Windows Powershellin järjestelmävalvojana ja loin kansion <i>saltdemo</i> komennolla <i>mkdir</i> ja siihen tiedoston <i>Vagrantfile</i>. 

Jatkoin komennolla <i>vagrant up</i> - kolme Vagrantfile:ssä konfiguroitua konetta käynnistyivät parissa minuutissa.

### Hyväksy orjat
Kirjauduin master-koneelle ssh:lla käyttäen komentoa <i>vagrant ssh tmaster</i> ja hyväksyin orja-koneiden avaimet komennolla <i>sudo salt-key -A</i>.

![acceptingkeys](https://user-images.githubusercontent.com/78509164/229236213-746a1a49-50c1-4575-bb44-f96950cc8da0.png)

...ja testasin yhteyden <i>sudo salt '*' test.ping</i>:

![testingconnection](https://user-images.githubusercontent.com/78509164/229236893-805994df-e3ed-4e29-9895-e263f50724aa.png)

### Näytä esimerkit tiloista...:
Tässä osuudessa seurasin https://terokarvinen.com/2023/salt-vagrant/ esimerkkiä hyvin tarkasti.
- <i>package</i>:
Yritin asentaa orja-koneille Nginx:n komennolla <i>sudo salt '*' state.single pkg.installed nginx</i>. Sain kuitenkin virheilmoituksen:
![nginxnot](https://user-images.githubusercontent.com/78509164/229277976-489d7744-c79c-4082-b059-04aaa20a48e7.png)

Orjalla t001 oli sama lopputulos. Yrittäessäni samaa, mutta apache2:lla sain vastaavan tuloksen. Googlettelun jälkeen selvisi, että Vagrantin sisällä toimivilla virtuaalikoneilla oli väärä aika/päivämäärä. Ratkaisu löytyi täältä https://ahelpme.com/linux/ubuntu/ubuntu-apt-inrelease-is-not-valid-yet-invalid-for-another-151d-18h-5min-59s/ ja täältä https://github.com/MichaIng/DietPi/issues/5478#issuecomment-1119741371 .

![timesync1](https://user-images.githubusercontent.com/78509164/229310863-22a8c697-621c-4e6b-a38f-1e59a9598878.png)

![timesync2](https://user-images.githubusercontent.com/78509164/229310888-16e4c17c-2518-428e-98d7-d9d4632f0996.png)

![syst_timesync](https://user-images.githubusercontent.com/78509164/229310899-b21f2a59-8a5e-4367-8861-4bd2e8c58e2b.png)

![timesync3](https://user-images.githubusercontent.com/78509164/229310905-d141ffb0-6a19-478f-89ad-7345a4300f96.png)

Aikani ihmettelin, miksi en edelleenkään pystynyt asentamaan orja-koneille esimerkiksi apache2:ta. Sitten tajusin, etten ajanut yllä (kuvissa) mainittu mainittuja komentoja niille. Eli jatkoin harjoitusta sillä, että ajoin nuo samat komennot, paitsi kaavalla <i>sudo salt '*'....</i>. Tämän kaiken jälkeen Apache2:n asennus orja-koneille onnistui!

![apacheinstalled3](https://user-images.githubusercontent.com/78509164/229312106-f7bad087-2f79-4ea4-845c-68fc2487711a.png)


- <i>file</i>:
Tiedoston luominen kaikilla orja-koneilla komennolla <i>sudo salt '*' state.single file.managed '/tmp/welcome-to-janahalt.pro'</i>.
![filestate](https://user-images.githubusercontent.com/78509164/229277661-d19730b9-9392-4ade-a9c2-d02cd07edfe4.png)

- <i>service</i>:
Tässä sitten varmistin, että daemon pyörii molemmilla orja-koneilla käyttäen komentoa <i>sudo salt '*' state.single service.running apache2</i> ja lisäksi <i>curl -s 192.168.12.102|grep title</i>, jotta varmistuin, että kaikki oli kunnossa.
  
![servicerunning1](https://user-images.githubusercontent.com/78509164/229312487-cf8f24c1-1ebd-41f7-be61-1a401ba82b8a.png)

![image](https://user-images.githubusercontent.com/78509164/229313120-02abcfc5-863e-4384-87ca-9461b59d39ff.png)


- <i>user</i>:
Ekana loin käyttäjän <i>tester01</i> komennolla <i>sudo salt '*' state.single user.present tester01</i>, sitten konfiguroin ko. käyttäjätiliä kaikilla orja-koneilla siten, että heidän oletus komentorivitulkki on bash komennolla <i>sudo salt '*' state.single user.present tester01 shell="/bin/bash/"</i>. Lopuksi vielä poistin käyttäjätilin <i>tester01</i> kaikilta orja-koneilta komennolla <i>sudo salt '*' state.signel user.absent tester01</i>. Alla kuvakaappaukset ko. asioista :)

![adduser](https://user-images.githubusercontent.com/78509164/229278535-c45f103a-ddba-4744-aecc-8f38b4b0f71d.png)

![modifyuser](https://user-images.githubusercontent.com/78509164/229278542-02666477-3900-4c4d-92f2-eeae4a6b3987.png)

![removeuser](https://user-images.githubusercontent.com/78509164/229278553-50d6d44d-4b0d-4cf5-8675-8e48fa0e5d27.png)

- cmd.run

![cmdrun](https://user-images.githubusercontent.com/78509164/229240818-542aab2e-a65d-4458-9f2d-e24668bf7bc4.png)

### Infraa koodina
Tässä kohdassa loin hakemiston, johon sitten voin kirjoittaa "state-tiedostoja". Käytin komentoa <i>sudo mkdir -p /srv/salt/greeting</i> ja menin muokkaamaan (samala loin sen) tiedostoa <i>init.sls</i> komennolla <i>sudoedit /srv/salt/greeting/init.sls</i>.
Ajoin tilan "greeting" komennolla <i>sudo salt '*' state.apply greeting</i>, jolloin tulos oli seuraavanlainen (sama orja-koneella t002):

![stateapplied](https://user-images.githubusercontent.com/78509164/229314645-93dc4c05-28b9-45a3-9e04-d6a2fbb23dd2.png)

Tässä vielä <i>/srv/salt/greeting/init.sls</i> ja <i>greetingall.txt</i> tiedostojen sisällöt:

![greetingsisältö](https://user-images.githubusercontent.com/78509164/229315234-5056dd81-07ec-4bfd-8e53-da13ed42877d.png)

![greetingallsisältö](https://user-images.githubusercontent.com/78509164/229315252-f3617080-7047-497a-9234-cd593b5dc48f.png)

Lopuksi vielä loin tiedoston, johon oli tarkoitus kirjoittaa konfiguraatiot mitkä tilat ajelaan mille orja-koneelle. Tähti '*' tarkoittaa, että kaikille orja-koneille.

![topsls](https://user-images.githubusercontent.com/78509164/229315366-05f10271-b2de-4e6e-ace7-de38437f587c.png)

-----------------------------
#### Lähteet

https://terokarvinen.com/2023/salt-vagrant/

https://tuomasvalkamo.com/CMS-course/week-1/

https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/

https://terokarvinen.com/2023/create-a-web-page-using-github/

https://developer.hashicorp.com/vagrant/downloads

https://ahelpme.com/linux/ubuntu/ubuntu-apt-inrelease-is-not-valid-yet-invalid-for-another-151d-18h-5min-59s/

https://github.com/MichaIng/DietPi/issues/5478#issuecomment-1119741371

