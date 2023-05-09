## H6 - Puolikas (miniprojekti)  

#### Valmiustaso  

Projekti on vielä kesken, joten alpha.  

## Työympäristö  

Toteutin projektin VirtualBoxissa kahdella virtuaalikoneella, joihin asensin Fedora 38:n KDE-ympäristöllä.

Rautana kannettava näillä spekseillä:  
```
- CPU AMD Ryzen 7 4700U, Radeon Graphics, 2,00 GHz; RAM 16 Gt  
- Windows 11 Home, 64-bit  
```  

### Asennettavat ohjelmat ja muut suunnitellut muutokset:  
- Neovim + konfigurointi
- .bashrc konfigurointi / alias 
- konsolin säätöjä / oletusväriteeman muutos  

Päädyin edellä mainittuihin ohjelmiin siksi, että ne on minulle sellaisia, joiden haluan olevan kunnossa kun alan käyttää uutta/uudelleenasennettua konetta. Toteutan projektin siten, että ensin teen muutoksia ja asennan ohjelmia käsin master-koneelle ja automatisoin ne orja-koneelle asennusta varten.  

### Aikaisemmat harjoitukset  

<a href="https://github.com/JanaHalt/InfraAsCode/blob/3a0efe10fece61bdc1350247bb5709c70f993d51/h1-salt.md">H1 - Suolaa</a>
<a href="">H2 - Demonit</a>  
<a href="">H3 - Git</a>  
<a href="">H4 - Komennus</a>  
<a href="">H5 - Vaihtoehdot</a>  

## Valmistelu  

Aloitin sillä, että asensin kahdelle virtuaalikoneelle Fedora 38:n. Seuraavaksi asensin yhdelle salt-masterin ja toiselle salt-minionin. Salt-masterin ja -minionin asennuksessa seurasin tätä ohjetta: <a href="https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/fedora.html">Salt-project - Install Salt on Fedora</a>:   

- Ensin ajoin molemmille koneille päivitykset käyttäen komentoa ```sudo dnf update```  
- Asensin saltin: ```sudo dnf install salt-master -y``` ja ```sudo dnf install salt-minion -y```  
- Enabloin ja käynnistin salt-master & -minion palvelut: ```sudo systemctl enable salt-master && sudo systemctl start salt-master``` ja ```sudo systemctl enable salt-minion && sudo systemctl start salt-minion```  
- Kerroin orja-koneelle, mistä osoitteesta se tavoittaa master-koneen. Orja-koneella, kansiossa **/etc/salt/minion.d/**: ```sudoedit master.conf``` ja luotuun tiedostoon (master.conf) ```master [ip-osoite]```.  
- Käytin komentoa ```sudo salt-key -A``` ja hyväksyin orjan **jhminion** avaimen  
- Testasin komennolla ```sudo salt 'jhminion' test.ping```, että yhteys toimi:  
![yhteysorjaan1](https://user-images.githubusercontent.com/78509164/236862521-92b6d5e0-1553-4d7d-a250-62dea75a1a9a.png)  

## Neovim  

Aloitin asentamalla Neovim käsin master-koneelle komennolla ```sudo dnf install neovim``` ja sen jälkeen ohjelmaa pystyi käynnistämään komennolla ```nvim```. Jatkoin luomalla salt-tilan, jolla asentaisin **neovim**in orja-koneelle.  
Kansioon ```/srv/salt``` loin uuden kansion ```/neovim```, johon lisäsin tiedoston ```init.sls```:  

![neovim3](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/ecc4fce7-7149-453a-8bc7-62c008e26cbd)  

Tämän jälkeen ajoin tilan orja-koneelle komennolla ```sudo salt 'jhminion' state.apply neovim``` ja sain onnistuneen lopputuloksen:  

![neovim1](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/0669d9a9-505e-4894-9082-ba6add34365d)  

![neovim2](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/093a75a1-f390-4249-a77a-3c7da1dca3b6)  

Sitten vielä menin orja-koneelle ja testasin käynnistyykö asennettu ohjelma kirjoittamalla komentoriville ```nvim```:  

![neovim4](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/b461bfa2-c402-4514-9b37-157f79853757)  

#### Neovim konfigurointimuutos  

Seuraavaksi piti luoda konfigurointitiedosto, jolla voisin sitten sitten konfiguroida Neovimin erilaisia ominaisuuksia. Hyödynsin ohjetta <a href="https://builtin.com/software-engineering-perspectives/neovim-configuration">Neovim Configuration for Beginners</a>.  
Loin tiedoston ```init.vim``` polkuun ```~/.config/nvim/```. Halusin, että Neovimin kirjoitustilassa olisi näkyvissä rivinumerot, joten lisäsin edellä mainittuun tiedoston tekstin ```set number```. Ja voilà, nyt rivinumerot näkyy:  

![neovim5](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/c64e11e7-d46b-4715-9ac0-29ee082bee75)  

Jotta saisin saman konfigurointimuutoksen voimaan myös orja-koneelle, aloitin sillä, että ensin kopioin **neovim**in konfigurointitiedoston polkuun ```/srv/salt/neovim``` komennolla ```sudo cp ~/.config/nvim/init.vim```. Tarkistin, että kopiointi onnistui komennolla ```ls /srv/salt/neovim```.  

![neovim6](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/5387deaa-b26f-4127-9a69-979d98a2a148)  

 Tämän jälkeen muokkasin ***neovim***-tilaa, eli lisäsin neovim-tilassa olevaan init.sls tiedostoon seuraavan tekstin:  
 
```
#show row-numbers  
   /usr/share/nvim/sysinit.vim:
     file.managed:
       - source: salt://neovim/init.vim  
       - mode: "644"  
```  

Mode 644 tarkoittaa, että tiedoston omistajalla on luku- ja kirjoitusoikeus ja ryhmällä sekä muilla on vain lukuoikeus. Sitten ajoin tilan orja-koneelle komennolla ```sudo salt 'jhminion' state.apply neovim``` ja sain onnistuneen tuloksen:  

![neovim7kopio](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/a8675134-172d-4c7c-a51f-4cc55585688b)   

Testasin vielä, että **Neovim** orja-koneella käynnistyy siten, että rivinumerot näkyy:  

![neovim8](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/a7fa49a3-0315-4345-908d-0bad6d954302)    


## .bashrc  

Välillä kyllästyttää kirjoittaa pitkää komentoa ```sudo dnf update```. Päätin tehdä sille aliaksen ja siitä sitten tilan, jota voin ajaa sitä orja-koneelle, jotta se toimii siinäkin. Hyödynsin ohjetta <a href="https://linuxize.com/post/how-to-create-bash-aliases/">How to create bash aliases</a>. Aliakset voidaan luoda tiedostoon ***bashrc***, joka on kansiossa ***/etc***.  
Eli menin muokkaamaan tiedostoa ***etc/bashrc*** komennolla ```sudo nvim /etc/bashrc```. Lisäsin ko. tiedoston loppuun tekstin:  
```
# Create alias for dnf update  
alias paivita="sudo dnf update -y"
``` 
Tallennettuani tiedoston kirjauduin ulos käyttäjäprofiilistani ja sitten kirjauduin takaisin (jotta se konfigurointitiedosto otetaan käyttöön) ja kokeilin, toimiiko luomani alias:  

![bashrc1](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/726fbe58-3f2a-46f4-87ee-327723a95e10)  

Loin tällekin muutokselle oman salt-tilan. Eli kansiossa ***/srv/salt*** komennolla ```sudo mkdir alias```. Sitten kopioin siihen **bashrc** tiedoston komennolla ```sudo cp /etc/bashrc .```, jolloin tiedosto kopioitui haluamaani kansioon. Lopuksi loin init.sls tiedoston komennolla ```sudoedit init.sls``` ja siihen:  
```  
#create alias for sudo dnf update  
  /etc/bashrc:
    file.managed:  
      - source: salt://alias/bashrc  
      - mode: "644"  
```  

Sitten ajoin tilan ***jhminion*** orja-koneelle komennolla ```sudo salt 'jhminion' state.apply alias```:  

![bashrc2](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/9129d649-8214-4ca5-944f-93aab11e337e)  

Ja vielä kokeilu orja-koneella, että luotu alias toimii:  

![bashrc3](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/027c6124-f92a-4eb3-a0b6-61d7ef78c134)  

## Konsolin oletusväriteeman muutos  

<a href="https://docs.kde.org/trunk5/en/konsole/konsole/profiles.html">Kde Docs - Profiles</a> sivuilta löysin ohjeen profiilien muokkaamiseen. Ko. sivustolta minulle selvisi, että juuri profiilien avulla on mahdollista muokata mm. konsolin oletusväriteemaa ovat kansiossa ```/home/[kotikansio]/.local/share/konsole```. Tehtävän tekohetkellä oli konsolissani käytössä default-profiili, jossa perusväriteema mustalla taustalla ja valko-/sinisillä kirjaimilla ja tuo edellä mainittu kansio oli tyhjä. Päättelin, että sinne ilmestyy jokin tiedosto silloin, kun luon uuden profiilin ja niin se olikin. Eli menin konsolin asetuksiin ja sieltä *'Configure Konsole...'*:  

![konsole1](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/53f92f61-c024-42cf-a5af-b76bc869c2f8)  

Sitten *'Profiles'* -> *'New'*, jossa sitten pääsin antamaan profiilille haluamaani nimen (tässä "Solarised") ja *'Appearance'* välilehdellä valitsemaan väriteemaa ja/tai muokkaamaan sitä:  

![konsole2](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/aeb3b4ae-aee9-4e11-90b0-f3a88615bf70)  

Tämän jälkeen suljin konsolin ja avasin sen uudestaan ja uusi oletusväriteema oli onnistuneesti käytössä:  

![konsole3](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/770bab54-32bb-416b-8c84-c6cc6ba9ff25)  

Kun uusi oletusväriteema oli onnistuneesti asennettu käsin, seuraavaksi aloin tekemään salt-staten, jolla ajaisin sen myös orja-koneelle. Ensimmäisenä menin tarkistamaan, että ```/home/[kotikansio]/.local/share/konsole``` kansiosta löytyy äsken luomani profiilin konfigurointitiedosto:  

![konsole4](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/62fe4f53-202d-4af6-9d34-f3c3df186929)  

Sitten oli vuorossa konsolin oletusväriteeman muuttaminen orja-koneella saltin avulla. Kuten aiemmin aloitin luomalla uuden kansion ***colortheme*** tiedostopolkuun ```/srv/salt/```. Seuraavaksi kopioin ko. kansioon profiili-konfigurointitiedoston komennolla ```sudo cp /home/[kotikansio]/.local/share/konsole/Solarised.profile .``` (olin tuolloin kansiossa *'/srv/salt/colortheme'*). Tarkistin kopioitumisen ***ls*** komennolla ja sitten siirryin luomaan ***init.sls*** tiedoston, josta tuli tällainen:  

```  
# Set default color scheme for konsole  
  /usr/share/config/konsole/Solarised.profile:  
    file.managed:  
      - source: salt://colortheme/Solarised.profile:  
      - mode: "644"  
 ```  
 
 Ajattuani tilan orja-koneelle komennolla ```sudo salt 'jhminion' state.apply colortheme``` sain virheilmoituksen:  
 
 ![konsole5](https://github.com/JanaHalt/ServerManagement_project/assets/78509164/dca3425b-f5fc-455c-a263-497ee30135b2)  
 
 Epäilen, että virhe voi olla siinä, että kansio, johon yritän kopioida konsolin profiili-konfigurointitiedoston, on väärä. Vinkin, mihin kansioon se kannattaisi kopioida, löysin melko pitkän googlettelun jälkeen täältä <a href="https://forum.kde.org/viewtopic.php?f=22&t=29952">Global config for Konsole</a>. 
 
 Aivan lopuksi tulen mahdollisesti vielä luomaan ***top.sls*** tilan, jolla olisi mahdollista ajaa orja-koneille kaikki aikaisemmin luomani tilat kerralla. 

### Lähteet  

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/fedora.html  

https://linuxize.com/post/how-to-create-bash-aliases/  

https://builtin.com/software-engineering-perspectives/neovim-configuration  

https://docs.kde.org/trunk5/en/konsole/konsole/profiles.html  

https://forum.kde.org/viewtopic.php?f=22&t=29952  

https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#the-top-sls-file  
