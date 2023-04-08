Viikko 2 Palvelinten hallinta-kurssia. Aiheena Demonit.

### Lue ja tiivistä

#### Pkg-File-Service - Control Daemons with Salt - Change SSH Server Port

-
-
....

### Asenna OpenSSH-palvelin käsin. Laita se kuuntelemaan oletusportin lisäksi jotain toista porttia. Testaa lopputulos!

Alkuun pystytin salt master-slave arkkitehtuurin hyödyntämällä ohjetta Salt Quickstart - Salt Stack Master and Slave on Ubuntu Linux (https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/):

1) Asenna master:
  -> sudo apt-get update  
  -> sudo apt-get -y install salt-master  
  -> hostname -I  
  ![master_hostnameIP](https://user-images.githubusercontent.com/78509164/230739046-49d1bda1-15d4-4a68-a2e4-4f610626f812.png)

2) Asenna slave:  
  -> sudo apt-get update  
  -> sudo apt-get -y install salt-minion

xxx
