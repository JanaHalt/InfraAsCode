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

Nyt salttia pystyy käyttämään windowsilla paikallisesti. Eli PowerShellissä pääkäyttäjänä esimerkiksi komento ```salt-call --local cmd.run whoami```:  

![localrun1](https://user-images.githubusercontent.com/78509164/235114661-9893a15a-ebcf-407c-825e-91ca2e9770cb.png)  

Toisena esimerkkinä vaikka tiedoston luominen. Olin C:\ hakemistossa ja käytin komentoa ```salt-call --local state.single file.managed C:/Users/janka/kokeilu.txt``` ja tässä tulos:   

![statesingle1](https://user-images.githubusercontent.com/78509164/235117201-a579b0a9-92c5-443e-8e43-8eb52d2b8223.png)  

![statesingle2](https://user-images.githubusercontent.com/78509164/235117627-848e0879-0ffd-4086-9629-7eb5e224e486.png)  

Kokeilin vielä, saanko tiedostoa auki vaikka notepadilla:  

![statesingle3](https://user-images.githubusercontent.com/78509164/235118062-5f322231-7616-4cc0-b999-7a9b7f8f317a.png)  

## Hei ikkuna!  

Nyt tiedetään, että salttia voi käyttää windowsilla paikallisesti vaikka tiedostojen luomiseen (edellinen esimerkki). Minulla ei ennestään oikein ollut kokemusta powershell skriptien kanssa, joten sain vähän pähkäiltävää.  

![skripti1](https://user-images.githubusercontent.com/78509164/235121602-14c7e2c9-954f-41ae-8e78-a77e66732040.png)  

![skripti2](https://user-images.githubusercontent.com/78509164/235121945-f21a9ecb-cba2-4bdb-aefd-456503ed2cbb.png)  

![skripti3](https://user-images.githubusercontent.com/78509164/235122226-e817036f-0dfb-4458-bb87-47e5ccfa179d.png)  

Skripti toimii, eli tulostaa komentoriville halutun tekstin. Kuitenkin, skriptin sisältö tuollaisena aiheuttaisi sen, että jos se avataan windowsin file-explorerista tuplaklikkaamalla, niin ikkuna vain vilahtaa auki, ilman että edes voisi ehtiä lukea sitä haluttua tekstiä.  
Tästä syystä muokkkasin skriptin sisältöä - lisäsin sanan ```pause``` tiedoston loppuun:  

![skripti4](https://user-images.githubusercontent.com/78509164/235124336-f8a79396-60c1-4fa3-afd0-4c3e1e0360f6.png)  

Edellä mainittu muutos aiheuttaa sen, että jos skripti ajetaan komentoriviltä/powershellissä, niin haluttu teksti tulostuu ruudulle ja skripti jää odottamaan, että käyttäjä painaa jotain näppäintä, jolloin skripti loppuu.  
File-explorerissa tuplaklikkaamalla vastaavasti skripti avaa komentokehotteen ja siihen ikkunaan tulostuu haluttu teksti. Sitten painamalla mitä vaan näppäintä skripti lopettaa ja komentokehotteen ikkuna sulkeutuu.  
Vaihtoehtoisesti voidaan avata uusi powershell tai komentokehote ikkuna ja kirjoittaa pelkkä ```hellowindows``` ja saadaan haluttu teksti näkyviin :)  

![skripti6](https://user-images.githubusercontent.com/78509164/235126839-d0b5dce9-480e-41c0-bbbe-6fe403c11c8c.png)  

![skripti7](https://user-images.githubusercontent.com/78509164/235127285-05d1c46b-cfc4-4977-9985-0b8f4488cfb3.png)  

![skripti8](https://user-images.githubusercontent.com/78509164/235127818-0cf817a2-7322-4ea9-9a19-a1752de7cb7e.png)  

## Installed  


### Lähteet  

https://www.windowscentral.com/how-create-and-run-batch-file-windows-10  


:)

...to be continued
