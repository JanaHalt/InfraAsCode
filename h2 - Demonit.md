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

Kuten tuossa ekassa kuvakaappauksessa näkyy, niin ei ole määritelty yksittäisiä portteja olemaan auki/kiinni ssh:ta ajatellen. Joten seuraavaksi määritin ufw palomuurin seuraavasti:  

![ufwON](https://user-images.githubusercontent.com/78509164/230764370-22be4ac5-e7da-4a4d-85f3-35ded04eb63c.png)

Sitten vielä muokkasin **sshd_config** tiedostoa, jotta ssh-palvelin kuuntelee oletusportin 22 lisäksi muutakin porttia (2244). Kts alle.  

![sshdkuuntelee](https://user-images.githubusercontent.com/78509164/230765704-ebbad97c-437a-4606-ab3e-c63df8511b1e.png)

...testasin vielä, että ssh yhteys ko. porttien kautta toimii:  

 ![ssh22yes](https://user-images.githubusercontent.com/78509164/230765759-e06273ce-cbc2-4812-8055-7d47d73390f3.png)  
 
 ![ssh2244](https://user-images.githubusercontent.com/78509164/230765761-d8b171e0-0f10-42fd-991c-ee011acc8944.png)  
 
## Nyt sitten automatisointi  

Luodaan hakemisto komennolla **sudo mkdir /srv/salt/ssh** ja siihen tiedosto **sshd.sls** komennolla **sudoedit /srv/salt/ssh/sshd.sls**. sshd.sls tiedostoon seuraavat:  
![sshdsls](https://user-images.githubusercontent.com/78509164/230793468-7f6f81ee-e753-4dd6-aac7-5999d7d6cc89.png)


Testasin toimivuutta komennolla **sudo salt-call --local state.apply ssh**, tuloksena:  
![openssh1](https://user-images.githubusercontent.com/78509164/230781795-48e96421-b9f9-4215-8233-1f117dfdda56.png)  
Kuvassa tosin komento **sudo salt '*' cmd.run 'hostname -I'**, mutta virhe oli samanlainen tuon edellisenkin komennon kanssa.  



Jatkoin state-tiedoston parissa kirjoittamalla sinne seuraavat:  
![sshdstate1](https://user-images.githubusercontent.com/78509164/230793777-9854bfea-689f-4b7e-a2d8-060c3f56e021.png)

State-tiedostossa esiintyvät tiedostopolut viittavat alla näytettyyn tiedostoon **sshd_config**. Siinä tiedostossa minulla oli jo ennestään portit 22 ja 2244 määritetty olemaan auki.  

Nyt testaan staten toimivuutta suorittamalla sitä saltin kautta slave-koneille komennolla **sudo salt '*' state.apply sshd**.  
Tömä ei kuitenkaan toiminut, vaan sain tällaisen virheen  
![saltvirhe](https://user-images.githubusercontent.com/78509164/230794379-a19a6eaf-322d-4aee-82d9-f65605f8077b.png)  
Kuvassa tosin komento **sudo salt '*' cmd.run 'hostname -I'**, mutta virhe oli samanlainen tuon edellisenkin komennon kanssa.  




  
##### Lähteet

xxx
