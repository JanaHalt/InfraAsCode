## H6 - Puolikas (miniprojekti)  

#### Valmiustaso  

Projekti on vielä kesken, joten alpha.  

## Työympäristö  

Toteutin projektin VirtualBoxissa kahdella virtuaalikoneella, joihin asensin Fedora 38:n KDE-ympäristöllä.

Rautana kannettava näillä spekseillä:  
```
- CPU AMD Ryzen 7 4700U, Radeon Graphics, 2,00 GHz  
- RAM 16 Gt  
- Windows 11 Home, 64-bit  
```  

### Asennettavat ohjelmat ja muut suunnitellut muutokset:  
- Neovim + konfigurointi
- .bashrc konfigurointi / alias 
- konsolin säätöjä / oletusväriteeman muutos  
- GO tools. Eli GO:n tarvitsemien työkalujen asennus 

Päädyin edellä mainittuihin ohjelmiin siksi, että ne on minulle sellaisia, joiden haluan olevan kunnossa kun alan käyttää uutta/uudelleenasennettua konetta. Toteutan projektin siten, että ensin teen muutoksia ja asennan ohjelmia käsin master-koneelle ja automatisoin ne orja-koneelle asennusta varten.  

### Aikaisemmat harjoitukset ja linkki Palvelinhallinta-kurssiin 

<a href="https://github.com/JanaHalt/InfraAsCode/blob/3a0efe10fece61bdc1350247bb5709c70f993d51/h1-salt.md">H1 - Suolaa</a>  
<a href="https://github.com/JanaHalt/InfraAsCode/blob/546a5fbaee6336279941376184d8f9d9f4536db7/h2%20-%20Demonit.md">H2 - Demonit</a>  
<a href="https://github.com/JanaHalt/InfraAsCode/blob/546a5fbaee6336279941376184d8f9d9f4536db7/h3%20-%20Git.md">H3 - Git</a>  
<a href="https://github.com/JanaHalt/InfraAsCode/blob/546a5fbaee6336279941376184d8f9d9f4536db7/h4%20-%20Komennus.md">H4 - Komennus</a>  
<a href="https://github.com/JanaHalt/InfraAsCode/blob/546a5fbaee6336279941376184d8f9d9f4536db7/h5%20-%20Vaihtoehdot.md">H5 - Vaihtoehdot</a>  
<a href="https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/">Palvelinhallinta - kevät 2023</a>

## Valmistelu  

Aloitin sillä, että asensin kahdelle virtuaalikoneelle Fedora 38:n. Seuraavaksi asensin yhdelle salt-masterin ja toiselle salt-minionin. Salt-masterin ja -minionin asennuksessa seurasin tätä ohjetta: <a href="https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/fedora.html">Salt-project - Install Salt on Fedora</a>:   

- Ensin ajoin molemmille koneille päivitykset käyttäen komentoa ```sudo dnf update```  
- Asensin saltin: ```sudo dnf install salt-master -y``` ja ```sudo dnf install salt-minion -y```  
- Enabloin ja käynnistin salt-master & -minion palvelut: ```sudo systemctl enable salt-master && sudo systemctl start salt-master``` ja ```sudo systemctl enable salt-minion && sudo systemctl start salt-minion```  
- Kerroin orja-koneelle, mistä osoitteesta se tavoittaa master-koneen. Orja-koneella, kansiossa **/etc/salt/minion.d/**: ```sudoedit master.conf``` ja luotuun tiedostoon (master.conf) ```master [ip-osoite]```.  
- Käytin komentoa ```sudo salt-key -A``` ja hyväksyin orjan **jhminion** avaimen  
- Testasin komennolla ```sudo salt 'jhminion' test.ping```, että yhteys toimi:  

  ![yhteysorjaan1](https://github.com/JanaHalt/InfraAsCode/assets/78509164/15b84506-e20f-4cb2-b655-e9ba2e806737)  

## Neovim  

Aloitin asentamalla Neovim käsin master-koneelle komennolla ```sudo dnf install neovim``` ja sen jälkeen ohjelmaa pystyi käynnistämään komennolla ```nvim```. Jatkoin luomalla salt-tilan, jolla asentaisin **neovim**in orja-koneelle.  
Kansioon ```/srv/salt``` loin uuden kansion ```/neovim```, johon lisäsin tiedoston ```init.sls```:  

  ![neovim3](https://github.com/JanaHalt/InfraAsCode/assets/78509164/076b4540-887c-40dd-ad30-d8d5640549f0)  

Tämän jälkeen ajoin tilan orja-koneelle komennolla ```sudo salt 'jhminion' state.apply neovim``` ja sain onnistuneen lopputuloksen:  

![neovim1](https://github.com/JanaHalt/InfraAsCode/assets/78509164/c7888421-0157-4f91-8c67-9d3deea1fd7a)  

![neovim2](https://github.com/JanaHalt/InfraAsCode/assets/78509164/3c33c86a-5191-4a57-84cc-29e24b977c36)  


Sitten vielä menin orja-koneelle ja testasin käynnistyykö asennettu ohjelma kirjoittamalla komentoriville ```nvim```:  

![neovim4](https://github.com/JanaHalt/InfraAsCode/assets/78509164/7b122a94-56e5-4889-821f-da1336c3d2f9)  

#### Neovim konfigurointimuutos  

Seuraavaksi piti luoda konfigurointitiedosto, jolla voisin sitten sitten konfiguroida Neovimin erilaisia ominaisuuksia. Hyödynsin ohjetta <a href="https://builtin.com/software-engineering-perspectives/neovim-configuration">Neovim Configuration for Beginners</a>.  
Loin tiedoston ```init.vim``` polkuun ```~/.config/nvim/```. Halusin, että Neovimin kirjoitustilassa olisi näkyvissä rivinumerot, joten lisäsin edellä mainittuun tiedoston tekstin ```set number```. Ja voilà, nyt rivinumerot näkyy:  

![neovim5](https://github.com/JanaHalt/InfraAsCode/assets/78509164/91fd1598-8463-4516-b980-ad3d0fffca01)    

Jotta saisin saman konfigurointimuutoksen voimaan myös orja-koneelle, aloitin sillä, että ensin kopioin **neovim**in konfigurointitiedoston polkuun ```/srv/salt/neovim``` komennolla ```sudo cp ~/.config/nvim/init.vim```. Tarkistin, että kopiointi onnistui komennolla ```ls /srv/salt/neovim```.  

![neovim6](https://github.com/JanaHalt/InfraAsCode/assets/78509164/36241f34-5a46-4d06-836d-acfe6a579b5e)   

 Tämän jälkeen muokkasin ***neovim***-tilaa, eli lisäsin neovim-tilassa olevaan init.sls tiedostoon seuraavan tekstin:  
 
```
# Show row-numbers  
  /usr/share/nvim/sysinit.vim:
    file.managed:
      - source: salt://neovim/init.vim  
      - mode: "0644"  
```  

Mode 644 tarkoittaa, että tiedoston omistajalla on luku- ja kirjoitusoikeus ja ryhmällä sekä muilla on vain lukuoikeus. Sitten ajoin tilan orja-koneelle komennolla ```sudo salt 'jhminion' state.apply neovim``` ja sain onnistuneen tuloksen:  

![neovim7kopio](https://github.com/JanaHalt/InfraAsCode/assets/78509164/47f7f9c4-1993-4f86-ad71-3b19c7c36d8c)    

Testasin vielä, että **Neovim** orja-koneella käynnistyy siten, että rivinumerot näkyy:  

![neovim8](https://github.com/JanaHalt/InfraAsCode/assets/78509164/9fffcfd0-effc-4dd0-8551-3d474f0f6146)  

## .bashrc  

Välillä kyllästyttää kirjoittaa pitkää komentoa ```sudo dnf update```. Päätin tehdä sille aliaksen ja siitä sitten tilan, jota voin ajaa sitä orja-koneelle, jotta se toimii siinäkin. Hyödynsin ohjetta <a href="https://linuxize.com/post/how-to-create-bash-aliases/">How to create bash aliases</a>. Aliakset voidaan luoda tiedostoon ***bashrc***, joka on kansiossa ***/etc***.  
Eli menin muokkaamaan tiedostoa ***etc/bashrc*** komennolla ```sudo nvim /etc/bashrc```. Lisäsin ko. tiedoston loppuun tekstin:  
```
# Create alias  
alias paivita="sudo dnf update -y"
``` 
Tallennettuani tiedoston kirjauduin ulos käyttäjäprofiilistani ja sitten kirjauduin takaisin (jotta se konfigurointitiedosto otetaan käyttöön) ja kokeilin, toimiiko luomani alias:  

![bashrc1](https://github.com/JanaHalt/InfraAsCode/assets/78509164/509d91f5-6255-48a9-8bfa-36deb2d0ca2b)   

Loin tällekin muutokselle oman salt-tilan. Eli kansiossa ***/srv/salt*** komennolla ```sudo mkdir alias```. Sitten kopioin siihen **bashrc** tiedoston komennolla ```sudo cp /etc/bashrc .```, jolloin tiedosto kopioitui haluamaani kansioon. Lopuksi loin init.sls tiedoston komennolla ```sudoedit init.sls``` ja siihen:  
```  
# Create alias for sudo dnf update  
/etc/bashrc:
  file.managed:  
    - source: salt://alias/bashrc  
    - mode: "0644"  
```  

Sitten ajoin tilan ***jhminion*** orja-koneelle komennolla ```sudo salt 'jhminion' state.apply alias```:  

![bashrc2](https://github.com/JanaHalt/InfraAsCode/assets/78509164/8c0cd1ae-f051-45a3-870d-fd65698eb34a)  

Ja vielä kokeilu orja-koneella, että luotu alias toimii:  

![bashrc3](https://github.com/JanaHalt/InfraAsCode/assets/78509164/cb8995dc-a142-4453-acaf-6949ad0a4580)   

## Konsolin oletusväriteeman muutos  

<a href="https://docs.kde.org/trunk5/en/konsole/konsole/profiles.html">Kde Docs - Profiles</a> sivuilta löysin ohjeen profiilien muokkaamiseen. Ko. sivustolta minulle selvisi, että juuri profiilien avulla on mahdollista muokata mm. konsolin oletusväriteemaa ovat kansiossa ```/home/[kotikansio]/.local/share/konsole```. Tehtävän tekohetkellä oli konsolissani käytössä default-profiili, jossa perusväriteema mustalla taustalla ja valko-/sinisillä kirjaimilla ja tuo edellä mainittu kansio oli tyhjä. Päättelin, että sinne ilmestyy jokin tiedosto silloin, kun luon uuden profiilin ja niin se olikin. Eli menin konsolin asetuksiin ja sieltä *'Configure Konsole...'*:  
  
![konsole1](https://github.com/JanaHalt/InfraAsCode/assets/78509164/4d771045-6609-4844-b257-98c3ba988af2)  

Sitten *'Profiles'* -> *'New'*, jossa sitten pääsin antamaan profiilille haluamaani nimen (tässä "Solarised") ja *'Appearance'* välilehdellä valitsemaan väriteemaa ja/tai muokkaamaan sitä:  

![konsole2](https://github.com/JanaHalt/InfraAsCode/assets/78509164/707b030a-c037-4ab5-a2fc-b238382f6209)   

Tämän jälkeen suljin konsolin ja avasin sen uudestaan ja uusi oletusväriteema oli onnistuneesti käytössä:  

![konsole3](https://github.com/JanaHalt/InfraAsCode/assets/78509164/f92b71a5-1e41-4e8a-ad56-c4249dfcfdd2)   

Kun uusi oletusväriteema oli onnistuneesti asennettu käsin, seuraavaksi aloin tekemään salt-staten, jolla ajaisin sen myös orja-koneelle. Ensimmäisenä menin tarkistamaan, että ```/home/[kotikansio]/.local/share/konsole``` kansiosta löytyy äsken luomani profiilin konfigurointitiedosto:  

![konsole4](https://github.com/JanaHalt/InfraAsCode/assets/78509164/7f8dbe48-8d48-4440-aef3-dc6aed4babce)    

Sitten oli vuorossa konsolin oletusväriteeman muuttaminen orja-koneella saltin avulla. Kuten aiemmin aloitin luomalla uuden kansion ***colortheme*** tiedostopolkuun ```/srv/salt/```. Seuraavaksi kopioin ko. kansioon profiili-konfigurointitiedoston komennolla ```sudo cp /home/[kotikansio]/.local/share/konsole/Solarised.profile .``` (olin tuolloin kansiossa *'/srv/salt/colortheme'*). Tarkistin kopioitumisen ***ls*** komennolla ja sitten siirryin luomaan ***init.sls*** tiedoston, josta tuli tällainen:  

```  
# Set default color scheme for konsole  
/usr/share/config/konsole/Solarised.profile:  
  file.managed:  
    - source: salt://colortheme/Solarised.profile:  
    - mode: "644"  
 ```  
 
 Ajattuani tilan orja-koneelle komennolla ```sudo salt 'jhminion' state.apply colortheme``` sain virheilmoituksen:  
 
![konsole5](https://github.com/JanaHalt/InfraAsCode/assets/78509164/0ed7f190-f43a-4878-bfbc-3ca09cd9879a)  
 
 Epäilen, että virhe voi olla siinä, että kansio, johon yritän kopioida konsolin profiili-konfigurointitiedoston, on väärä. Vinkin, mihin kansioon se kannattaisi kopioida, löysin melko pitkän googlettelun jälkeen täältä <a href="https://forum.kde.org/viewtopic.php?f=22&t=29952">Global config for Konsole</a>. 
 
 Aivan lopuksi tulen mahdollisesti vielä luomaan ***top.sls*** tilan, jolla olisi mahdollista ajaa orja-koneille kaikki aikaisemmin luomani tilat kerralla.  
 
_____________________
 
Jatkan perjantaina 12.5.23.  

Eilen sain luennolla hyvän vinkin <a href="www.terokarvinen.com">Tero Karviselta</a>, että ratkaisuna ongelmaan voisi olla saltin ```file.directory```. Menin tutkimaan asiaa vielä <a href="https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html">Saltin dokumentaatiosta</a> ja päädyin lisäämään ```colortheme``` tilan ***init.sls*** tiedostoon seuraavan pätkän:  

```  
# Check parent directory  
/usr/share/konsole:  
  file.directory:  
    - user: root  
    - group: root  
    - dir_mode: "0755"  
    - file_mode: "0644"  
    - makedirs: True  
 ```  
 
Tilanne näytti korjaantuvan, sillä:  

![colortheme1](https://github.com/JanaHalt/InfraAsCode/assets/78509164/5fdbf55e-7928-4cb8-8c50-486018b10e4d)  

Mutta orja-koneella konsoli oli edelleen musta-valkoinen.  

_______________________________________  

Jatkoin 14.5.2023  

Konsolin oletusväriteeman muutos osoittautui varsin hankalaksi. En aikaa vievästä asian tutkimisesta huolimatta löytänyt paikkaa mihin pitäisi laittaa konfigurointitiedostoa, joka tekisi tekemästäni väriteema-profiilista oletuksen.  

Lopulta päädyin vaihtamaan tavoitetta ***oletusväriteeman muutos*** toiseksi, ***GO kielen työkalujen asennus***.  

##### EDIT 15.5.2023  

En saanut rauhaa asian suhteen, joten oli pakko yrittää vielä kerran ratkaista tämä. Ja vihdoin ja viimein onnistuin! Alla init.sls tiedon sisältö, jolla sitten sain tämän toimimaan:  

![konsole8](https://github.com/JanaHalt/InfraAsCode/assets/78509164/1ab3d382-ff1a-4201-b341-234870537b33)   

Ja tässä näkyy, että myös orja-koneella on vaihtunut oletusväriteema:  

![konsole9](https://github.com/JanaHalt/InfraAsCode/assets/78509164/6ce77deb-4447-4dff-9378-643d28107594)  

![konsole10](https://github.com/JanaHalt/InfraAsCode/assets/78509164/8d561be5-f178-4de2-8de9-19d7b7ee9857)  

## GO tools  

Eli ensin käsin. Seurasin ohjetta <a href="https://developer.fedoraproject.org/tech/languages/go/go-installation.html">GO on Fedora</a>. Ihan ensimmäisena ```sudo dnf install golang```. GO:n koodia kirjoitetaan **työtilassa**, joka määritetään ***GOPATH*** muuttujan avulla. Yleensä tähän käytetään arvoa ***$HOME/go***. Joten tässä jatko:  

```
mkdir -p $HOME/go  
echo 'export GOPATH=$HOME/go' >> $HOME/.bashrc  
source $HOME/.bashrc  
```  

Sen jälkeen ohje neuvoi tarkistamaan, että muuttuja ***GOPATH*** on määritelty oikein ```go env GOPATH```. Muuttuja olikin määritelty oikein ja edellä mainitun komennon tuloksena saatiin:  
```go/janahalt/go```.  

Lopuksi piti tottakai kokeilla, toimiiko edellä asennettu oikeasti. Koska en osaa koodata GO:ssa, niin kirjoitin täysin esimerkkiohjelman kuin ohjeessa <a href="https://developer.fedoraproject.org/tech/languages/go/go-programs.html">Writing Go programs</a>. Eli ensin loin kansion, johon ohjelmani tulisi, sitten siirryin siihen kansioon ja loin tyhjän tiedoston **hello.go**. Alla näkyy myös ***hello.go*** tiedoston sisältö. Samassa kansiossa ollessa voidaan luotua ohjelmaa ajaa komennolla ```go run [ohjelman nimi]```. Tämä tapa soveltuu pienille ja nopeille kokeiluile. <a href="https://developer.fedoraproject.org/tech/languages/go/go-programs.html">Writing Go programs</a> sivulla löytyy tarkemmat ohjeet, mikäli haluaa kirjoittaa ohjeilmia, joilla on riippuvuuksia yms.  

![GO2](https://github.com/JanaHalt/InfraAsCode/assets/78509164/495ae6d9-936f-413a-a079-135e36bd4abd)  

![GO3](https://github.com/JanaHalt/InfraAsCode/assets/78509164/d790f3a5-de10-44d3-b2f2-ed93071d21f1)  

Nyt sitten automatisointi:  

Loin uuden kansion ***golang*** kansioon ```/srv/salt/```. Tähän uuteen kansioon loin sitten uuden tila-tiedoston ***init.sls***, tällä sisällöllä:  

![GOin1](https://github.com/JanaHalt/InfraAsCode/assets/78509164/286dfa2f-f92d-47f5-a8e3-5332fd260304)  

***pkg.installed*** osuus asentui onnistuneesti, mutta **init.sls**:n toisesta osuudesta tuli virheilmoitus:  

![GOin2](https://github.com/JanaHalt/InfraAsCode/assets/78509164/f5c87739-3206-4acb-bf56-b2e311a53a09)  

Kokeilin muuttaa init.sls tiedostossa kohdan ```$HOME/go``` -> ```~/$HOME/go``` ja nyt tilan ajaminen onnistui ongelmitta.  


![GOin3](https://github.com/JanaHalt/InfraAsCode/assets/78509164/45e94d7b-128a-4682-9f56-5d454751df9e)  

Seuraavaksi menin testaamaan suoraan orja-koneelle, että kaikki on kuten pitää. Alla kuvakaappaukset testauksesta:  

![GOin5](https://github.com/JanaHalt/InfraAsCode/assets/78509164/b222f13f-a63f-4075-9085-acd0f84ee7eb)  

![GOin4](https://github.com/JanaHalt/InfraAsCode/assets/78509164/831a2e61-8cd4-4e29-86de-d159ba2e7cc6)  

## TOP.SLS  

Tällä tila-tiedostolla voidaan kaikki aiemmin luodut tilat ajaa orja-koneelle kerralla komennolla ```sudo salt '*' state.highstate```. Alla ko. tiedoston sisältö:  

```  
base:  
  '*':
    - alias  
    - colortheme  
    - golang  
    - neovim  
```  

![TOPSLS](https://github.com/JanaHalt/InfraAsCode/assets/78509164/906b159d-876e-494a-a06a-6f94e6f5e4dd)  



### Lähteet  

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/fedora.html  

https://linuxize.com/post/how-to-create-bash-aliases/  

https://builtin.com/software-engineering-perspectives/neovim-configuration  

https://docs.kde.org/trunk5/en/konsole/konsole/profiles.html  

https://forum.kde.org/viewtopic.php?f=22&t=29952  

https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#the-top-sls-file  

https://developer.fedoraproject.org/tech/languages/go/go-installation.html  

https://developer.fedoraproject.org/tech/languages/go/go-programs.html  
