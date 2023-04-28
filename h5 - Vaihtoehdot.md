## Lue ja tiivistä  

### Milloin ja millä  

Aloitin harjoituksen 27.4.2023 tällä setupilla:  
```
- CPU Intel(R) Core(TM) i5-2500K CPU @ 3.30GHz  3.30 GHz; RAM 8 Gt  
- Windows 10 Pro, 64-bit  
```

## Asenna Salt Windowsille  

Aloitin sillä, että latasin salt-minion ohjelman. Menin osoitteeseen ```https://repo.saltproject.io/windows/``` ja sieltä valitsin tiedoston ```Salt-Minion-Latest-Py3-AMD64.msi```. 

![installingsalt1](https://user-images.githubusercontent.com/78509164/234967251-e1e8739e-8ae9-4cdf-bcdf-2714579a10a4.png)  

Kun sitten avasin ladatun tiedoston, sain tällaisen virheilmoituksen:  

![installingsalt2](https://user-images.githubusercontent.com/78509164/234967572-25915858-3e90-4999-9ec7-d29ed33005b2.png)  

...joten avasin PowerShellin pääkäyttäjänä ja avasin ko. tiedoston siellä:  


![installingsalt3](https://user-images.githubusercontent.com/78509164/234968119-b5c4cebe-1087-4817-ae2c-b0960387588c.png)  

Siitä avautui Windowsin perinteinen setup wizard ikkuna (kuten ylempänäkin näkyy) ja asennus oli vain next, next, yes, finnish-tapainen. Tämän jälkeen kokeilin ajaa komentoa ```salt-call --local test.ping```, mutta sain virheilmoituksen, ettei salt ole ajettava ohjelma/funktio/skripti tai sitä ei löydy.

Väsymys voittaa, joten jatkan huomenna.  

Edit: Jatkan 28.4.2023  
_______________________  

Eilen tosiaan harmillisesti unohdin ottaa kuvakaappauksen virheilmoituksesta, jonka sain kun kokeilin ajaa komentoa ```salt-call --local test.ping``` silloin, kun salt-ohjelma ei ollut kansiossa ```C:\Windows\System32```.  
Eli tänään jatketaan Windows PowerShellissä pääkäyttäjänä.  
Aluksi siis kopioin **salt-ohjelman** edellä mainittuun kansioon (ollessani kansiossa **Downloads**) komennolla ```cp .\Salt-Minion-Latest-Py3-AMD64.msi C:\Windows\System32\```. Sen jälkeen ajoin komentoa ```salt-call --local test.ping```ja nyt se toimi odotetusti:  

![saltcalllocal1](https://user-images.githubusercontent.com/78509164/235097809-4553eeda-a3c6-4b9a-8ff8-b7857a83c821.png)  

## Ei voi kalastaa  

## Hei ikkuna!  

## Installed  

:)

...to be continued
