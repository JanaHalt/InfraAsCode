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
Ensiksi loin skriptin **hello.sh**. Kirjoitin sen nanossa, joten aloitin komennolla **nano hello.sh**. Skriptin luomisen jälkeen annoin kaikille oikeudet suorittaa ko. skriptiä - käyttöoikeuksien muokkaamiseen käytin komentoa **chmod ugo+x hello.sh**. Lopuksi vielä kokeilin suorittaa sitä paikellisesti master-koneella komennolla **./hello.sh**. Alla näkyy tulos:  

![HelloSkripti2](https://user-images.githubusercontent.com/78509164/233947746-f9daa704-280c-4e93-b6f8-8bd9caaaf794.png)  

Tämän jälkeen kopioin skriptin **/usr/local/bin** ja **ls** komennolla tarkistin, että se kopioitui sinne ja siirryin takaisin kotihakemistooni (**/home/vagrant**) komennolla **cd**. **/usr/local/bin** kansiossa olevia ohjelmia pystyvät käyttämään myös peruskäyttäjät. Lisäksi nyt skriptiä pystyy ajamaan pelkällä **hello.sh** komennolla, kts alla:  

![HelloSkripti3](https://user-images.githubusercontent.com/78509164/233963255-72a1c91a-bb3e-4d33-b9cd-7a73539f6a17.png)  

### b) Hello.py  

Seuraavana kirjoitin **helloworld.py** skriptin. Tämä skripti eroaa edellisestä hello.sh skriptistä siten, että edellinen suoritetaan bash-kielellä ja tämä pythonilla. Tätäkin skriptiä kirjoitin nanossa, eli aloitin komennolla **nano helloworld.py**. Tämänkin skriptin käyttöoikeuksia muokkasin samalla komennolla kuin edellisen skriptin kohdalla: **chmod ugo+x helloworld.py**. Testasin paikallisesti skriptin toimivuutta master-koneella komennolla **./helloworld.py**:  

![HelloWorld1](https://user-images.githubusercontent.com/78509164/233968599-7a447b81-24aa-43c7-848a-9a7c8e6ca0fe.png)  

Jotta skriptiä voisi ajaa kaikki käyttäjät pelkällä **helloworld.py** komennolla, kopioin sen kansioon **/usr/local/bin**. Jälleen tarkistin onnistuneen kopioitumisen komennolla **ls** ja komennolla **cd** siirryin takaisin kotihakemistooni. Siellä sitten testasin, että skriptiä pystyy ajamaan pelkällä **helloworld.py** komennolla.

![HelloWorld2](https://user-images.githubusercontent.com/78509164/233971418-fa0275a6-d9a5-4755-8312-02eca30625e7.png)  

### c) Automatisointi Saltilla  

Kun varmistuin siitä, että molemmat skriptit toimivat paikallisesti, lähdin toteuttamaan niiden automatisoinnin Saltilla.  

*'Nyt ollaan kotihakemistossa.'* Komennolla **sudo mkdir -p /srv/salt/scripts** loin uuden kansion. Seuraavaksi kopioin molemmat skriptit äsken luotuun kansioon komennoilla **sudo cp hello.sh /srv/salt/scripts** ja **sudo cp helloworld.py /srv/salt/scripts**. Tarkistaakseni, että kopiointi onnistuin siirryin ko. kansioon komennolla **cd /srv/salt/scripts** ja listasin siellä olevat tiedostot komennolla **ls -l**. Käytin parametria *' -l '*, jotta näkisin, että myös käyttöoikeudet pysyivät samoina = eli kaikilla suoritusoikeus.  

![salt1](https://user-images.githubusercontent.com/78509164/233979624-50022db2-5538-462f-80a5-645ee9462232.png)  

Jatkoin luomalla tiedoston **init.sls** kansioon **/srv/salt/scripts**. 
