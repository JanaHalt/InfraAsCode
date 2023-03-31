# h1 - Hei maailma!
============================

Tätä harjoitusta tehdessä toimin omalla pöytäkoneellani, jolla on asennettu Windows 10, tietääkseni kaikkine viimeisimpine päivityksineen. 

### Lue ja tiivistä:
- .....
- ....
- ....

#### Asenna Debian 11 Vagrantilla
Alkuun piti asentaa Vagrant koneelle. Käytän kotipöytäkoneellani Windows 10. Joten ensin latasin Vagrant Windows AMD64 binary täältä: https://developer.hashicorp.com/vagrant/downloads .

![image](https://user-images.githubusercontent.com/78509164/229228996-33fff4ef-3a2b-4847-8665-30fcd71c8d37.png)

Asennuksen jälkeen piti vielä uudelleenkäynnistää kone, mutta sitten pääsin jatkamaan.

#### Asenna Vagrant filessa konfiguroitu kolmen koneen verkko
Avasin Windows Powershellin järjestelmävalvojana ja loin kansion <i>saltdemo</i> komennolla <i>mkdir</i> ja siihen tiedoston <i>Vagrantfile</i>. 

Jatkoin komennolla <i>vagrant up</i> - kolme Vagrantfile:ssä konfiguroitua konetta käynnistyivät parissa minuutissa.

#### Hyväksy orjat
Kirjauduin master-koneelle ssh:lla käyttäen komentoa <i>vagrant ssh tmaster</i> ja hyväksyin orja-koneiden avaimet komennolla <i>sudo salt-key -A</i>.

![acceptingkeys](https://user-images.githubusercontent.com/78509164/229236213-746a1a49-50c1-4575-bb44-f96950cc8da0.png)

...ja testasin yhteyden <i>sudo salt '*' test.ping</i>:

![testingconnection](https://user-images.githubusercontent.com/78509164/229236893-805994df-e3ed-4e29-9895-e263f50724aa.png)

#### Näytä esimerkit tiloista...:
- <i>package</i>:
Asensin <i>tree</i>n orja-koneille komennolla <i>sudo salt '*' state.single pkg.installed tree</i>
![treeinstalled](https://user-images.githubusercontent.com/78509164/229240063-b0b975aa-9c66-4b50-8f4b-c3f8763ee935.png)

Orjalla t002 oli sama lopputulos.

- file
- service
- user
- cmd.run

![cmdrun](https://user-images.githubusercontent.com/78509164/229240818-542aab2e-a65d-4458-9f2d-e24668bf7bc4.png)




## Lähteet

e) Tee infraa koodina, esim oma hei maailma.
