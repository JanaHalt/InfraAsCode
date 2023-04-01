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
Tässä osuudessa seurasin https://terokarvinen.com/2023/salt-vagrant/ esimerkkiä hyvin tarkasti.
- <i>package</i>:
Yritin asentaa orja-koneille Nginx:n komennolla <i>sudo salt '*' state.single pkg.installed nginx</i>. Sain kuitenkin virheilmoituksen:
![nginxnot](https://user-images.githubusercontent.com/78509164/229277976-489d7744-c79c-4082-b059-04aaa20a48e7.png)

Orjalla t001 oli sama lopputulos. Yrittäessäni samaa, mutta apache2:lla sain vastaavan tuloksen. Jatkan vielä selvittelyjä... :) 

- <i>file</i>:
Tiedoston luominen kaikilla orja-koneilla komennolla <i>sudo salt '*' state.single file.managed '/tmp/welcome-to-janahalt.pro'</i>.
![filestate](https://user-images.githubusercontent.com/78509164/229277661-d19730b9-9392-4ade-a9c2-d02cd07edfe4.png)

- service
- <i>user</i>:
Ekana loin käyttäjän <i>tester01</i> komennolla <i>sudo salt '*' state.single user.present tester01</i>, sitten konfiguroin ko. käyttäjätiliä kaikilla orja-koneilla siten, että heidän oletus komentorivitulkki on bash komennolla <i>sudo salt '*' state.single user.present tester01 shell="/bin/bash/"</i>. Lopuksi vielä poistin käyttäjätilin <i>tester01</i> kaikilta orja-koneilta komennolla <i>sudo salt '*' state.signel user.absent tester01</i>. Alla kuvakaappaukset ko. asioista :)
![adduser](https://user-images.githubusercontent.com/78509164/229278535-c45f103a-ddba-4744-aecc-8f38b4b0f71d.png)

![modifyuser](https://user-images.githubusercontent.com/78509164/229278542-02666477-3900-4c4d-92f2-eeae4a6b3987.png)

![removeuser](https://user-images.githubusercontent.com/78509164/229278553-50d6d44d-4b0d-4cf5-8675-8e48fa0e5d27.png)

- cmd.run

![cmdrun](https://user-images.githubusercontent.com/78509164/229240818-542aab2e-a65d-4458-9f2d-e24668bf7bc4.png)




###### TO DO:

e) Tee infraa koodina, esim oma hei maailma.
