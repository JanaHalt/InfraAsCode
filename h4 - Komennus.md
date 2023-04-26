## Komennus  

### Milloin ja millä
Teen harjoituksen 24.4.2023 seuraavalla kokoonpanolla:  
```
- CPU Intel(R) Core(TM) i5-2500K CPU @ 3.30GHz  3.30 GHz; RAM 8 Gt  
- Windows 10 Pro, 64-bit  
- Vagrant 2.3.4, jossa 3 virtuaalikonetta: 1 x master ja 2 x slave  
- Virtuaalikoneilla Debian 11  
```  

### a) Hello.sh  
Ensiksi loin skriptin ```hello.sh```. Kirjoitin sen nanossa, joten aloitin komennolla ```nano hello.sh```. Skriptin luomisen jälkeen annoin kaikille oikeudet suorittaa ko. skriptiä - käyttöoikeuksien muokkaamiseen käytin komentoa ```chmod ugo+x hello.sh```. Lopuksi vielä kokeilin suorittaa sitä paikellisesti master-koneella komennolla ```./hello.sh```. Alla näkyy tulos:  

![HelloSkripti2](https://user-images.githubusercontent.com/78509164/233947746-f9daa704-280c-4e93-b6f8-8bd9caaaf794.png)  

Tämän jälkeen kopioin skriptin ```/usr/local/bin``` kansioon ja ```ls``` komennolla tarkistin, että se kopioitui sinne ja siirryin takaisin kotihakemistooni (**/home/vagrant**) komennolla ```cd```. ```/usr/local/bin``` kansiossa olevia ohjelmia pystyvät käyttämään myös peruskäyttäjät. Lisäksi nyt skriptiä pystyy ajamaan pelkällä ```hello.sh``` komennolla, kts alla:  

![HelloSkripti3](https://user-images.githubusercontent.com/78509164/233963255-72a1c91a-bb3e-4d33-b9cd-7a73539f6a17.png)  

### b) Hello.py  

Seuraavana kirjoitin ```helloworld.py``` skriptin. Tämä skripti eroaa edellisestä hello.sh skriptistä siten, että edellinen suoritetaan bash-kielellä ja tämä pythonilla. Tätäkin skriptiä kirjoitin nanossa, eli aloitin komennolla ```nano helloworld.py```. Tämänkin skriptin käyttöoikeuksia muokkasin samalla komennolla kuin edellisen skriptin kohdalla: ```chmod ugo+x helloworld.py```. Testasin paikallisesti skriptin toimivuutta master-koneella komennolla ```./helloworld.py```:  

![HelloWorld1](https://user-images.githubusercontent.com/78509164/233968599-7a447b81-24aa-43c7-848a-9a7c8e6ca0fe.png)  

Jotta skriptiä voisi ajaa kaikki käyttäjät pelkällä ```helloworld.py``` komennolla, kopioin sen kansioon ```/usr/local/bin```. Jälleen tarkistin onnistuneen kopioitumisen komennolla ```ls``` ja komennolla ```cd``` siirryin takaisin kotihakemistooni. Siellä sitten testasin, että skriptiä pystyy ajamaan pelkällä ```helloworld.py``` komennolla.

![HelloWorld2](https://user-images.githubusercontent.com/78509164/233971418-fa0275a6-d9a5-4755-8312-02eca30625e7.png)  

### c) Automatisointi Saltilla  

Kun varmistuin siitä, että molemmat skriptit toimivat paikallisesti, lähdin toteuttamaan niiden automatisoinnin Saltilla.  

*Nyt ollaan kotihakemistossa.*  
Komennolla ```sudo mkdir -p /srv/salt/scripts``` loin uuden kansion. Seuraavaksi kopioin molemmat skriptit äsken luotuun kansioon komennoilla ```sudo cp hello.sh /srv/salt/scripts``` ja ```sudo cp helloworld.py /srv/salt/scripts```. Tarkistaakseni, että kopiointi onnistuin siirryin ko. kansioon komennolla ```cd /srv/salt/scripts``` ja listasin siellä olevat tiedostot komennolla ```ls -l```. Käytin parametria ``` -l ```, jotta näkisin, että myös käyttöoikeudet pysyivät samoina = eli kaikilla suoritusoikeus.  

![salt1](https://user-images.githubusercontent.com/78509164/233979624-50022db2-5538-462f-80a5-645ee9462232.png)  

Jatkoin luomalla tiedoston ```init.sls``` kansioon ```/srv/salt/scripts```. Tässä tuon tiedoston sisältö:  

![init](https://user-images.githubusercontent.com/78509164/233987653-090ea11f-ef48-43ce-90a9-f6b79345b79d.png)  

Eli kummallekin skriptille loin oman tilan.  
```file.managed``` luo uuden tiedoston orja-koneelle.  
```name``` kertoo polun, mihin tiedosto orja-koneella luodaan.  
```source``` kertoo polun, josta tiedosto kopioidaan.  
```mode 0755``` kertoo käyttöoikeuksista ja tarkoittaa, että käyttäjä/omistaja voi lukea, kirjoittaa ja suorittaa, ryhmä voi lukea ja suorittaa ja muut voivat lukea ja suorittaa tiedostoa.  

Ennen kuin testasin, toimiiko tila koneille, halusin varmistaa, että yhteys orja-koneisiin on toimiva. Joten käytin komentoa ```sudo salt '*' test.ping```. Ja yhteyshän toimi:  

![yhteysorjiin](https://user-images.githubusercontent.com/78509164/233974301-6dbdc2fc-060f-47f2-b372-f43ada59a0cb.png)  

Jatkoin komennolla ```sudo salt '*' state.apply scripts```, ja tämä toimi kuten pitikin. Kuvakaappauksessa alla näkyy lopputulos orja-koneelle t002. Orja-koneella t001 tulos oli täysin sama.  

![skriptittoimii1](https://user-images.githubusercontent.com/78509164/233991692-98406521-b480-45f5-acf6-990c2f3415e2.png)  

### Asenna  

Tässä kohtaa oli tarkoitus asentaa jokin yhden binäärin ohjelma Saltilla orjille. Päädyin microon. Ensin asensin sen master-koneelle. Asensin micron githubista ```wget https://github.com/zyedidia/micro/releases/download/v2.0.11/micro-2.0.11-linux64-static.tar.gz```. Sitten purin ladatun tiedoston komennolla ```tar -xf micro-2.0.11-linux64-static.tar.gz```. Tarkistuksena vielä kurkkasin micro-2.0.11 kansion sisältöön:  

![microasennus2](https://user-images.githubusercontent.com/78509164/233997774-94a3bd33-5088-4767-8380-402da7ca401c.png)  

Jotta pystyisin asentamaan **micron** myös orja-koneille Saltin avulla, se piti kopioida ```/srv/salt/scripts``` kansioon. Olin kansiossa ```/home/vagrant/micro-2.0.11``` ja käytin komentoa ```sudo cp micro /srv/salt/scripts```. Sitten vielä tarkistus ```cd /srv/salt/scripts``` ja ```ls```:  

![microasennus3](https://user-images.githubusercontent.com/78509164/234000528-e6dd35b2-6785-4109-a8ca-5f7186af6857.png)  

Seuraavaksi muokkasin ```init.sls``` tiedostoa. Lisäsin sinne tämän:  

![microtila](https://user-images.githubusercontent.com/78509164/234004878-84b26888-0eb3-409f-9a58-39caa75b6dc2.png)  

Jatkoin testaamalla, asentaako komento ```sudo salt '*' state.apply scripts``` micron orja-koneille:  

![microasennus4](https://user-images.githubusercontent.com/78509164/234005388-8bc2eded-43ac-404e-94cf-2f194a16da0a.png)  

Kuten yllä olevassa kuvassa näkyy, micron asennus orja-koneille onnistui. Kuvassa orja-koneen t002 lopputulos. Orja-koneen t001 lopputulos oli sama. Huomaa, että kohdassa **succeeded** lukee **3 (changed=1)**, eli tapahtui vain yksi muutos = micron asennus. 

Aivan lopuksi vielä testasin, toimiiko äsken orja-koneille asennettu micro. Olin kirjautuneena master-koneelle, joten kirjauduin ulos komennolla ```exit``` ja sitten kirjauduin orja-koneelle ```t001``` komennolla ```vagrant ssh t001```. Sitten pääsin kokeilemaan microa:  

![microkokeilut001](https://user-images.githubusercontent.com/78509164/234039756-190039d6-1c50-4121-a195-b721077d46b8.png)  

![microkokeilut001toimii](https://user-images.githubusercontent.com/78509164/234039833-01a0f092-3407-4c14-967f-d700ad8b403b.png)  

Kaikki kunnossa :)  
