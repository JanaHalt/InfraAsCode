Viikko 2 Palvelinten hallinta-kurssia. Aiheena Demonit.

## Lue ja tiivistä

### Pkg-File-Service - Control Daemons with Salt - Change SSH Server Port

-
-
....

Toteutin tämän tehtävän VirtualBoxissa asennetuilla virtuaalikoneilla. Koneissa asennettuna Ubuntu 22.04, lisäksi ssh-yhteystesteissä otan tarvittaessa yhteyttä virtuaalikoneelle, jossa on asennettuna Fedora 37 Workstation.  
Toteutin harjoituksen 9.4.2023 alkaen klo 10:45.

## OpenSSH palvelimen asennus käsin

OpenSSH palvelimen asennus on yksinkertaista.  

**sudo apt-get install openssh-server**  Asennetaan OpenSSHn ja otetaan se käyttöön **sudo systemctl enable ssh**.  

**sudo systemctl start sshd** komennolla käynnistetään 

**sudo systemctl status sshd** Tarkistetaan, SSHn statuksen.  

Katsotaan, onko portti 22 auki. Address:Port:n kohdalla näkyy 0.0.0.0:ssh, joten portti 22 on oletettavasti auki. Kokeillaan vielä ssh toimii.
![portitAuki](https://user-images.githubusercontent.com/78509164/230759535-8b1d556c-feb3-4854-820e-f917dfdc44d0.png)  

Sain yhteyden ja sitten poistuin.

![image](https://user-images.githubusercontent.com/78509164/230762717-7ed27d42-ad38-4128-bf97-1e209d4f2a9b.png)



 

  
##### Lähteet

xxx
