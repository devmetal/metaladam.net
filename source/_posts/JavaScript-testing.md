---
title: JavaScript testing
date: 2017-03-10 18:02:35
tags:
  - javascript
  - testing
  - e2e
description: JavaScript unit tests e2e tests in real life
---

This is my very first post about one of my favorite topic. I like to talk or listen about this topic in meetups and in pubs with collegues. In my opinion, the testing in theese days with javascript is like a cocktail. We have to mix up a lots of techniques and technologies for a proper result. And sadly, sometimes when we order a mojito and then we got a sangría.

<!-- more -->

# JavaScript Testing Part 1

Heló, Metál Ádámnak hívnak és ebben a 3 cikksorozatban szeretném végigtárgyalni a javascript tesztelés világát. Az első részben inkább teoretikusan közelíteném meg a dolgot, kevesebb gyakorlati résszel. A gyakorlati részeknek egy egész bejegyzést szánok, mivel igyekszem olyan példákat hozni amelyek nem tanagyag szerűek, hanem olyan problémákat vetnek fel, melyekkel a mindennapi fejlesztés közben találkozhatunk.

## Evrybody know what testing is

Even a person who is not involved in this IT black magic knows what tests are for. We expect some behaviour or some results. In real life we are constantly test everything for example our car needs to be 'tested' every year, we had to check our clothes time to time to be sure we can still put them on etc..
A fejlesztők világában a tesztelés azonban sokkal egyértelműbb. A fejlesztés során keletkező modulokkal, függvényekkel szemben az elvárások általában jól definiáltak és egyértelműen meg lehet határozni hogy mikor hogyan kell viselkedniük. Például ha készítek egy modult, amely megadott feltételek mentén frissít vagy töröl valamit egy másik rendszerből, akkor ennek a modulnak a viselkedése nagyon jól leírható és ezáltal könnyen tesztelhető. Legalábbis elméletben.

## E2E Testing with JavaScript
{% raw %}
<em>Ki rendelt mojitot?</em>
<img src="mojito.png" width="200">
{% endraw %}

I will start writing with the most easiest and hardest topic (in my opinion). The "end to end" testing. Its easy because the e2e test is independent from the application code and used technologies. For example, if the application is written in java or php, the e2e test can be any other language if it supports libs as selenium and it has testing capabilities.

Fontos írnom arról, hogy ez a tesztelési módszer csak a jéghegy csúcsa. Ezzel a módszerrel általában a felhasználói felületet teszteljük. Például ha egy szöveges beviteli mezőbe, beírok egy a mi elvárásaink szerint hibás karakterláncot, akkor ellenőrizhetem hogy a felület a megfelelő módon jelzi e a hibát a felhasználónak.

Ennek a tesztelésnek az előnye, hogy sokmindent lefed. Lefedi egyrészt az alkalmazás felhasználói felületét, és lefedi a backend alkalmazást is. Hátránya hogy általában nem valami gyors, és hogy az alkalmazásnak futnia kell valahol, az összes hozzá kapcsolt rendszerrel együtt. Nem igazán hasznos ha a sokszor kell futtatni a fejlesztés közben.

A JavaScript világában rengeteg eszköz létezik egy e2e tesztcsomag elkészítésére. Egyes keretrendszerek mint az angular, saját az ő elvárásaiknak megfelelő rendszereket alakítanak ki erre a célra. Például a **protractor**.
Ennek az előnye, hogy az angularban használt template nyelvhez készítettek selectorokat.

De mint ahogy azt már mondtam, ebben a nyelvben a tesztelés egy koktél készítéséhez hasonlít. El kell készíteni a saját teszt koktélunkat. Szükségünk van egy teszt futtatóra, például **jest** vagy **mocha**. Azután eldöntjük hogy milyen rendszer fog kapcsolatba lépni az alkalmazásunkal. Ezek tipikusan olyan rendszerek amik képesek általunk megadott lépések sorozatát elvégezni egy böngészőben. Például: tölts be egy oldalt, kattints a kategóriák gombra, majd válaszd ki a "kocsmák a közelben" kategóriát.


A legelterjedtebb ilyen a **Selenium**. Én magam nem nagyon kedvelem, sokat dolgoztam vele a nokiánál és nehézkesnek, lassúnak tartom. Hasonló rendszer a phantomjs, bár az egy fokkal alacsonyabb szintű. Amit én kedvelek az a **nightmare**. Ha két alkotó elem, a teszt futtató és a vezérlő rendszer együtt van, akkor képesek vagyunk megírni a tesztjeinket.

A legtöbb netes cikkel és videóval az a baj, hogy meglátásom szerint, nem mutatnak a valósághoz közeli példákat. A driverek (pl nightmare, selenium) apijára koncentrálnak, hogy az bemutassák iskolapéldákon keresztül. Én úgy gondolom, hogy az api egy olyan dolog amire ott a forráskód vagy a dokumentáció. Én szeretnék bemutatni egy teszt esetet ami közel áll a valóságban használt példákhoz.

**Példa itt**

## Integrációs és unit tesztelés
{% raw %}
<em>Bloody Mary</em>
<img src="bmary.png" width="200">
{% endraw %}

Ezt a két témát én együtt tárgyalnám. Úgy gondolom hogy általában a JavaScript-ben nehezen szétválasztható a két tesztelési módszer. A teszteléshez használt technológiák, azaz a koktélunk ugyanolyan mindkét esetben.

Én személy szerint nem is nagyon szeretem szétválasztani a tesztjeimet két külön módszerrel, mert nagyon kényelmetlen. Nem szeretem hogyha egy vagy kettőnél több teszteléshez használt script van a package-ben. Az újoncoknak a megértés is nehezebb és zavarosabb hogyha sokféle teszt kódot kell, akár különböző módokon karban tartani. Akkor mégnehezebb a helyzet, hogyha a backend és a frontend kódot ugyanabban a package-ban tároljuk. Ekkor azonnal két külön teszt csomagunk van. Azonban hogyha szeretném elválasztani az integrációs és unit teszteket, akkor megalább három, durva esetben négy féle tesztet kell karbantartani.

### Integrációs teszt

Egy picit bajban vagyok ezzel a témával. Véleményem szerint, az IT világában nehéz elválasztani az egyszerű unit teszteket az integrációs tesztektől. De mi is számít integrációs tesztnek? Nemrég olvastam egy cikket, amiben arról írtak, hogy az integrációs tesztek több modult tesztelnek egyszerre. Ebből arra lehet következtetni, hogy a unit teszt, egy-egy modul viselkedését tesztelni, még az integrációs teszt, több modul együttes viselkedését. Azonban ahogy a cikkben is fogalmaztak, a határok nem túl élesek.

Valahogy mégis megpróbálom őket elválasztani. Az integrációs teszt, hasonlóan az e2e teszteléshez, elvárja hogy működjenek bizonyos háttér rendszerek, adatbázisok, szolgáltatások vagy legalábbis azok tesztelői esetleg fejlesztői változatai. 

### Unit teszt

A unit teszt, ezzel szemben a modulokat önmagukban teszteli. Azonban számos olyan alkalom van, hogy egy modul erősen támaszkodik egy másikra. Például egy http kliens modul, ami valamilyen api-tól nyer ki adatokat. Ekkor támaszkodhatunk a mockingra.

A legtöbb helyen ahol eddig dolgoztam, sokszor külön sem választottuk az integrációs és unit teszteket. Szerintem ez a probléma nagyságától és jellegétől függ, hogy szeretnénk e ilyen teszteket.

A helyzet az, hogy a nodejs rendszerei általában olyan gyorsak, hogyha egy unit teszthez felállítok a kód elején egy http szervert, mondjuk egy **expressjs** appot, az is rendkívül gyors lesz bármilyen mock implementáció nélkül.

**példa**

## Tesztelés a gyakorlatban

Odakint rengeteg különböző kész megoldás létezik a tesztelésre. Ezek legtöbbjét a különböző boilerplatek, generátorok és stackek biztosítják számunkra. Ezekről írnék pár szót. 