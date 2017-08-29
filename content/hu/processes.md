## VI. Folyamatok
### Alkalmazások futtatása mint egy vagy több állapot mentes folyamat 

Az alkalmazás futtatása az adott környezetben egy vagy több folyamatként (process) történik.

Legegyszerűbb esetben a kód egy önálló script, a futtató környezet pedig a fejlesztő saját laptopja az adott nyelv futtató környezetével. A folyamat indítása parancssorból történik (pl: `python my_script.py`). A spektrum másik olalán egy kifinomult alkalmazás éles telepítése [több folyamat típust is használ, egy vagy több futó processként példányosítva](./concurrency).

**A tizenkét-faktoros alkalmazás állapot mentes és [folyamatai nem osztanak meg semmit](http://en.wikipedia.org/wiki/Shared_nothing_architecture).** Bármilyen adat, melyet szükséges megőrizni perzisztens [támogató szolgáltatásban](./backing-services) tárolandó, tipikusan adatbázisban.

A folyamat állapot tere vagy a fájlrendszer csak időszakos, egyszer használatos cache szerepet tölthet be. Például letölthető egy nagy file a helyi filerendszerre, feldolgozható és a művelet eredménye eltárolható az adatbázisban. A tizenkét-faktoros alkalmazás sosem feltételezi, hogy bármi ami a memóriában vagy a diszk-en cachelésre került rendelkezésre fog állni egy jövőbeli kérésre vagy művelehez - mivel a többféle folyamatból sok fut egyszerre, valószínű , hogy egy jövőbeni kérést egy másik folyamat fog kiszolgálni. Mégha csak egy futó folyamat is van, egy újraindítás (amit egy telepítés, konfiguráció változás vagy a futtató környezet költözése vált ki) rendszerint kitöröl minden lokális (memória, filerendszer) állapotot.

Asset csomagoló eszközök (mint a [Jammit](http://documentcloud.github.com/jammit/) vagy [django-compressor](http://django-compressor.readthedocs.org/)) a filerendszert használják a lefordított asset-ek cachelésére. A tizenkét faktoros alkalmazás inkább a [build fázisban](./build-release-run) teszi ezt, mint a [Rails asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html).

Néhány webes rendszer a ["ragadós sessiön-ökre"](http://en.wikipedia.org/wiki/Load_balancing_%28computing%29#Persistence) épül - és a felhasználó munkamenet (session) adatait az alkalmazás folyamat memória terében tárolja, elvárva, hogy a jövőbeni kérések ugyanattól a felhasználótól ugyanarra a folyamatra érkezzenek. A ragadós munkamenet (sticky session) a tizenkét-faktor megszegése és az alkalmazás sohasem építhet rá. A munkamenet adat remek jelölt olyan adattárhoz, mely támogatja az adat időbeni lejáratát. Ilyen például a [Memcached](http://memcached.org/) vagy [Redis](http://redis.io/).
