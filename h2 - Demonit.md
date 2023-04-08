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

1) Asenna master:
  -> sudo apt-get update  
  -> sudo apt-get -y install salt-master  
  -> hostname -I  
  ![master_hostnameIP](https://user-images.githubusercontent.com/78509164/230739046-49d1bda1-15d4-4a68-a2e4-4f610626f812.png)

2) Asenna slave. Näin siis menettelin molemmilla slave-koneilla:  
  -> sudo apt-get update  
  -> sudo apt-get -y install salt-minion
  -> Tiedostoon /etc/salt/minion kirjoitin master-koneen sijainnin ja kunkin slave-koneen id:n.  
     (sudoedit /etc/salt/minion)
     
![slave1](https://user-images.githubusercontent.com/78509164/230740274-bfa78072-419f-40b3-b898-7b350c8cd0a9.png)


![slave2](https://user-images.githubusercontent.com/78509164/230740261-e18eda62-70cd-45f9-9bec-e6ef581f63d5.png)

     
  -> Lopuksi uudelleenkäynnistin slave-daemonit molemmilla slave-koneilla, jotta uudet muutokset tulisivat voimaan. Käytetty komento: sudo systemctl restart salt-minion.service
  
3) Hyväksy orjien avaimet master-koneella  
  Sain virheilmoituksen "The key glob '*' does not match any unaccepted keys. Ratkaisuksi löysin ohjeen uudelleenkäynnistää salt-master demoni. Löydettyäni sen ihmettelinkin miten en tajunnut sitä heti! :) Uudelleenkäynnistys siis tapahtui komennolla sudo systemctl restart salt-master.service.
  

  
  

xxx
