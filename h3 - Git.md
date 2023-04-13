Toteutan tehtävän 13.4.2023. Käytän pöytäkonetta, jossa on Windows 10 viimeisimpine päivityksineen sekä VirtualBoxissa 
toimivaa virtuaalikonetta, jossa on asennettu Debian 11.

## A) Online - New repository  

Loin uuden repositoryn menemällä omassa profiilissa välilehdelle **"Repositories"** ja sieltä klikkaamalla **"New"**. 
Näkyviin tulee sivu, jossa sitten täytetään uuden repositoryn tiedot. Kuten läksyn ohjeessa sanottiin, niin repositoryn 
nimessä ja lyhyessä kuvauksessa on sana "summer".
![uusirepo1](https://user-images.githubusercontent.com/78509164/231682123-159f3fb8-4b2d-4f2a-a3ab-2835e21cc2fd.png)  
______________________________________
![uusirepo2](https://user-images.githubusercontent.com/78509164/231683211-5f54723f-766e-42c2-91ac-c7940e2f8d4c.png)  

## B) Dolly - Clone, Change, Push  

Seuraavassa vaiheessa kloonasin äsken luodun repositoryn itselleni virtuaalikoneen komentoriviltä käsin. Sitä varten tarvitsen 
linkin, jonka avulla kloonaus tapahtuu. Sen saan näin:  
- mene repositoryn sivuille  
- klikkaa **Code**  
- klikkaa **SSH**  
- kopioi linkki  

![sshlinkki](https://user-images.githubusercontent.com/78509164/231691352-3a8c8905-f36b-4480-bedd-efcfef001010.png)  

Sitten kloonaamaan. Eli komentorivillä **git clone [ssh-kloonauslinkki]** ja enter. Tästä sitten toki tuli virheviesti, 
ettei minulla ole pääsyä, koska publickey puuttuu. Joten mennäänpä hoitamaan asia kuntoon.  

Komentorivillä:  
- **ssh-keygen**  
- **cat /home/janahalt/.ssh/id_rsa.pub**  
- kopioin publickey:n 

Githubissa omaan profiiliin, sieltä asetuksiin ja **SSH and GPG keys** ja **New SSH key**.  

![sshavainlisätty](https://user-images.githubusercontent.com/78509164/231695309-f4c1cd40-f83f-4be6-814a-42a5ab0fc8d2.png)  

Nyt sitten päästään jatkamaan repositoryn kloonausta. Eli komentorivillä **git clone [ssh-kloonauslinkki]**

![repokloonattu](https://user-images.githubusercontent.com/78509164/231696611-a104d869-eebc-42c0-9aed-bb9ee23cbce1.png)  

Komennolla **git pull** tarkistin, että ollaan up to date, vaikka toki tässä tapauksessa se oli turhaa, mutta kuitenkin. 
Harjoituksen vuoksi :)  
Tein muutoksia **README.md** tiedostoon:  

![nanoreadme1](https://user-images.githubusercontent.com/78509164/231699752-87639305-b564-44b1-86d0-a4c52a45c663.png)  

![nanoreadme2](https://user-images.githubusercontent.com/78509164/231700180-879a5858-be2c-4498-94f7-d74e753ebe3b.png)  

Seuraavaksi sitten laitoin muutokset palvelimelle. Käytin "yhdistelmäkomentoa" **git add . && git commit; git pull && git push**. Ja siinähän ne muutokset näkyvät!  

![muutoksetwebissä](https://user-images.githubusercontent.com/78509164/231722849-a99e0d68-178b-40fa-9bf5-be733b606c1b.png)  

## C) Doh! Silly change, no commit, git reset --hard  

Seuraavaksi hölmö muutos ja sen poistaminen **git reset --hard** komennolla. Eli menin taas muokkaamaan **README.md** tiedostoa. Sen jälkeen käytin komentoa **git add .**  Tarkistin komennolla **cat README.md**, että muutos meni tiedostoon. En kuitenkaan käyttänyt **git commit** komentoa, vaan poistin tuon muutoksen, *oho, mitä nyt!"* komennolla **git reset --hard**.  Ja komentohan toimi kuten pitikin.

![ohomitänyt3](https://user-images.githubusercontent.com/78509164/231735317-616ab8c3-a384-4bf4-9fcf-9d62b620f108.png)  

## D) Tukki - check out and explain git log  

Tässä kohtaa käytin komentoa **git log --patch**  

![gitlog1](https://user-images.githubusercontent.com/78509164/231740136-08d4df75-12f8-4088-aa75-cb1dad0eed03.png)  

Analyysini sisältää osittain oletuksia, kaikki ei välttämättä ole aitoa faktaa. Kirjoitan ranskalaisin viivoin järjestyksessä sen mukaan, miten asiat näkyvat yllä olevassa kuvassa.

- commit (vaaleanruskealla) ja sen perässä numero/kirjainyhdistelmä kertoo kyseisen commitin tunnisteen  
- author: kuka on tehnyt muutoksen  
- date: milloin on muutos tehty, päivämäärä ja kellonaika  
- commitin "nimi", tässä tapauksessa "ADD UPDATES"  
- diff .... : kertoo, mihin tiedostoon on tehty muutoksia, tässä tapauksessa README.md tiedostoon  
- index ....: En ole varma mistä tämä kertoo. Olisiko jokin tunniste, vaikka sen tarkan kohdan tunniste, johon on muutos tehty?  
- --- a/ ja +++ b/: kertovat taas mihin tiedostoon on tehty muutoksia?  
- se vaaleansininen "@@ ... @@"": kertonee riveistä, joilla on tehty muutoksia  
- SummerFun: repositoryn nimi
- Super Funny Summer Project: repositoryn lyhyt kuvaus  
- sitten vihreällä: yksi +, jonka vieressä on  tyhjä tila kertoo, että lisäsin tyhjän rivin. + ja tekst sen vieressä kertovat, mitä tekstiä lisäsin  
- sitten taas seuraava commit ja sen tunnistenumero sekä author, päivämäärä + aika, jne 






