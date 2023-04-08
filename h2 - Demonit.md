Viikko 2 Palvelinten hallinta-kurssia. Aiheena Demonit.

### Lue ja tiivistä

#### Pkg-File-Service - Control Daemons with Salt - Change SSH Server Port

-
-
....

### Asenna OpenSSH-palvelin käsin. Laita se kuuntelemaan oletusportin lisäksi jotain toista porttia. Testaa lopputulos!

Toteutin tämän tehtävän VirtualBoxissa asennetuilla virtuaalikoneilla. Koneissa asennettuna Ubuntu 22.04. Toteutusajankohta 8.4.2023 klo 22:10 alkaen.

![alkusetti](https://user-images.githubusercontent.com/78509164/230739335-8d4dd76a-bdce-41f2-a676-5076c604c9c1.png)

Alussa minulla oli vain tuo eka Ubuntu Desktop, mutta kloonasin siitä kaksi konetta lisää ja muokkasin niiden nimet. Lisäksi koneelle Slave 1 lisäsin harjoituksen vuoksi käyttäjän "eka" ja koneelle Slave 2 käyttäjän "toka".

Alkuun pystytin salt master-slave arkkitehtuurin hyödyntämällä ohjetta Salt Quickstart - Salt Stack Master and Slave on Ubuntu Linux (https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/):

#### Asenna master:
  -> sudo apt-get update  
  -> sudo apt-get -y install salt-master  
  -> hostname -I  
  ![master_hostnameIP](https://user-images.githubusercontent.com/78509164/230739046-49d1bda1-15d4-4a68-a2e4-4f610626f812.png)

#### Asenna slave. Näin siis menettelin molemmilla slave-koneilla:  
  -> sudo apt-get update  
  -> sudo apt-get -y install salt-minion
  -> Tiedostoon /etc/salt/minion kirjoitin master-koneen sijainnin ja kunkin slave-koneen id:n.  
     (sudoedit /etc/salt/minion)
     
![slave1](https://user-images.githubusercontent.com/78509164/230740274-bfa78072-419f-40b3-b898-7b350c8cd0a9.png)


![slave2](https://user-images.githubusercontent.com/78509164/230740261-e18eda62-70cd-45f9-9bec-e6ef581f63d5.png)

     
  -> Lopuksi uudelleenkäynnistin salt-demonit molemmilla slave-koneilla, jotta uudet muutokset tulisivat voimaan. Käytetty komento: sudo systemctl restart salt-minion.service
  
#### Hyväksy orjien avaimet master-koneella  
Sain virheilmoituksen "The key glob '*' does not match any unaccepted keys. Löysin ohjeen uudelleenkäynnistää salt-master demoni komennolla sudo systemctl restart salt-master.service.  

Edellisestä huolimatta avaimia ei edelleenkään kuulunut. Tarkistin kaikkien koneiden IP-osoitteet ja kaikilla oli sama 10.0.2.15. Tässä kohtaa tajusin mennä tarkistamaan virtuaalikoneiden verkkoasetuksia virtualboxin puolella. Sekä master- että orja-koneilla oli verkkoasetuksissa kohdassa "attached to" NAT. Muutin sen "Bridged Adapter":ksi. Sitten toki piti käydä laittamassa masterin uusi IP osoite kansioon /etc/salt/minion molemmilla slave-koneilla. Tämän jälkeen toki piti jälleen uudelleenkäynnistää salt-demonit slave-koneilla ja sitten asiat alkoi rullaa!

![networksettings](https://user-images.githubusercontent.com/78509164/230741398-36e8637c-ee3a-4908-91f0-9fa448d54153.png)

![uusi_masterIP](https://user-images.githubusercontent.com/78509164/230741746-9dc58da4-45c8-4317-bd5e-2a1a49f0fef1.png)

![image](https://user-images.githubusercontent.com/78509164/230741891-430d042b-33a9-4e34-9f70-2a2c8bfc1b01.png)


Tähän kaikkeen edellämainittuun meni reilu tunti. Nyt väsymys alkaa voittaa, joten jatkan huomenna :)


 

  
  

xxx
