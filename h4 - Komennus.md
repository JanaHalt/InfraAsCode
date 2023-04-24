## Komennus  

### Milloin ja millä
Teen harjoituksen 24.4.2023 seuraavalla kokoonpanolla:  
```
- CPU Intel(R) Core(TM) i5-2500K CPU @ 3.30GHz  3.30 GHz; RAM 8 Gt  
- Windows 10 Pro, 64-bit  
- Vagrant 2.3.4, jossa 3 virtuaalikonetta: 1 x master ja 2 x slave  
- Virtuaalikoneilla Debian 11  
```  

### Hello.sh  
Ensiksi loin skriptin **hello.sh**. Kirjoitin sen nanossa, joten aloitin komennolla **nano hello.sh**. Skriptin luomisen jälkeen annoin kaikille oikeudet suorittaa ko. skriptiä - käyttöoikeuksien muokkaamiseen käytin komentoa **chmod ugo+x hello.sh**. Lopuksi vielä kokeilin suorittaa sitä paikellisesti master-koneella komennolla **./hello.sh**. Alla näkyy tulos:  

![HelloSkripti2](https://user-images.githubusercontent.com/78509164/233947746-f9daa704-280c-4e93-b6f8-8bd9caaaf794.png)  

Tämän jälkeen kopioin skriptin **/usr/local/bin** ja **ls** komennolla tarkistin, että se kopioitui sinne. Tuossa kansiossa olevia ohjelmia pystyvät käyttämään myös peruskäyttäjät. Lisäksi nyt skriptiä pystyy ajamaan pelkällä **hellos.sh** komennolla, kts alla:  

![HelloSkripti3](https://user-images.githubusercontent.com/78509164/233963255-72a1c91a-bb3e-4d33-b9cd-7a73539f6a17.png)  
