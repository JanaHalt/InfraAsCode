Viikko 2 Palvelinten hallinta-kurssia. Aiheena Demonit.

## Lue ja tiivistä

### Pkg-File-Service - Control Daemons with Salt - Change SSH Server Port (Karvinen 2018)
 
- master-koneella voidaan luoda state, eli tila, jossa koneiden/järjestelmien halutaan olevan  
  - ensin asennetaan haluttu palvelu  
  - sitten muokataan konfigurointitiedosto sellaiseksi kuin halutaan  
  - lopuksi uudelleenkäynnistetään daemon - otetaan uudet konfiguroinnit käyttöön  
- Saltin avulla voidaan käskeä orja-koneita käyttämään master-koneella luotuja tiloja (states)
________________________________________________________

Toteutin tämän tehtävän VirtualBoxissa asennetuilla virtuaalikoneilla. Koneissa asennettuna Ubuntu 22.04 (1 x master + 2 x slave), lisäksi ssh-yhteystesteissä otan tarvittaessa yhteyttä virtuaalikoneelle, jossa on asennettuna Fedora 37 Workstation.  
Toteutin harjoituksen 9.4.2023. // edit: automatisointiosuus 10.4. ->

## OpenSSH palvelimen asennus käsin

OpenSSH palvelimen asennus on yksinkertaista.  

**sudo apt-get install openssh-server** Komennolla asennetaan OpenSSHn ja otetaan se käyttöön -> **sudo systemctl enable ssh**.  

**sudo systemctl start sshd** Komennolla käynnistetään sshd.

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

Alkuun piti luoda hakemisto **/srv/salt/ssh**. Siihen sitten tiedosto **sshd.sls** komennolla **sudoedit sshd.sls**. Kyseiseen tiedostoon sitten kirjoitin seuraavat:  

![sshdsls](https://user-images.githubusercontent.com/78509164/230800497-5cc5cdad-0010-4e5c-bffc-7e38790a5df6.png)  

Tässä kohtaa teki mieli kokeilla, onko master-orja yhteystyö edelleen toimiva, joten käytin komentoa **sudo salt '*' cmd.run 'hostname -I'**. Tämä palautti tällaisen virheen:  

![saltvirhe](https://user-images.githubusercontent.com/78509164/230806696-3fa1d898-5972-4bc1-9f17-2e5a8c489a4e.png)  

Googlettelun jälkeen selvisi, että kannattaa laittaa master-kone unohtamaan orja-koneiden avaimet komennolla **salt-run manage.down removekeys=True** ja sitten uudelleenkäynnistää *salt-minion.service* orja-koneilla, jonka jälkeen voi uudestaan käyttää master-koneella komentoa **sudo salt-key -A** , jolla hyväksytaään orja-koneiden avaimet.  

![yhteysorjakoneilleok](https://user-images.githubusercontent.com/78509164/230807267-e9f731f7-ebf2-458e-b8fb-e9036d89ac78.png)

Jatkuu vielä...  

Jatkan nyt klo 10:15.  
Eilisten haasteiden vuoksi teki mieli aloittaa automatisoinnin alusta asti, joten poistin koko /srv/salt-kansion master-koneelta.  

Aloitin siis **alusta** ja loin tarvittavan hakemiston komennolla **sudo mkdir -p /srv/salt**.  
Ja tähän väliin jälleen testausta, että yhteys orja-koneisiin toimii (kuva alla). Yhteys ok!    

![yhteysorjiin1](https://user-images.githubusercontent.com/78509164/230853369-11d3a466-1640-4d78-853e-5a219650e106.png)  

![image](https://user-images.githubusercontent.com/78509164/230865335-ea4a3322-9b7a-43ba-8b30-d4e287d59703.png)
  

Yllä init.sls tiedoston sisältö. Tiedosto sijaitsee kansiossa **/srv/salt/sshd**. Tiedosto on ns. state-tiedosto -> mitä halutaan, että kone tekee/millainen halutaan, että se on.  

![image](https://user-images.githubusercontent.com/78509164/230867819-18376f6d-6684-47c2-8b18-4aab91007448.png)  

JIPPII!!! Vihdoin ja viimein!! Toisella orja-koneilla siis samanlainen tulos. Ja ihan nolottaa myöntää, että vika taisi olla ainakin osaksi vain siinä, että state-tiedostossa unohdin määrityksissä tehdä kirjoittaa source-tiedoston polkuun alakansion. Elämä on, seuraavan kerran ehkä muistan paremmin! :D  

Sitten ajoin sshd-staten orja-koneille uudestaan määrityksiä muuttamatta. Demoni ei käynnistynyt uudelleen:  

![image](https://user-images.githubusercontent.com/78509164/230869212-24496cb1-8187-4c37-9c54-87f48b5ef1eb.png)  

Seuraavaksi muokkasin sshd_config-tiedostoa, lisäämällä määrityksiin kolmannen portin, 8888. Sitten testasin, käynnistyykö demoni uudelleen kun ko. state ajetaan orja-koneille. Ja demonihan käynnistyikin uudelleen, niin kuin pitikin (sama tulos toisella orja-koneella):  
![image](https://user-images.githubusercontent.com/78509164/230870076-b470fec5-5446-4f34-a764-d7149b6abaff.png)  

![image](https://user-images.githubusercontent.com/78509164/230871430-3ccf839e-0956-434a-a73e-2e5c5450ec1e.png)


##### Lähteet  

https://docs.saltproject.io/en/latest/topics/tutorials/firewall.html#firewall  

https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh  

https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/  

____________________________________
