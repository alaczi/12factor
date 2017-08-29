## V. Fordítás, pulbikálás, futtatás
### Szigorúan elkülönített fordítási és futtatási szakasz


A [kódbázis](./codebase) átalakítása - nem fejlesztői - telepítésé a következő három lépéssel történik:

* A *build fázis* tranformáció ami a kódbázis egy futtatható csomaggá úgynevezett *build*-é alakítja. A telepítési folyamat által meghatározott verziót használva a build letölti a [függőségeket](./dependencies) és elkészíti a bináris és asset fileokat. 
* A *release fázis* a build eredményét kombinája a telepítés aktuális [konfigurációjával](./config). Az eredmény *release* tartalmazza mind az adott build verziót és konfigurációt készen az azonnali indításra a cél környezetben.
* A *futtatás fázis* (aka "runtime") futtatja az alkalmazást a cél környezetben, elindítva az alkalmazás [folyamatait](./processes) a kiválasztott release-sel.

![Forráskódból build, ami a konfiggal kombinálva alkotja a release-t.](/images/release.png)

**A tizenkét-faktoros alkalmazás szigorúan elkönítia fordítási, publikálási és futtatási fázisokat**. Például nem lehetséges a forráskód változtatása a futtató környezetben, mert nem lehetséges a változtatások érvényesítése a fordítási fázisba.

A telepítő eszközök rendszerint rendelkeznek különböző release menedzsment eszközökkel, melyek közül a kiemelendő az előző verzióra történő visszagörgetés. Például a [Capistrano](https://github.com/capistrano/capistrano/wiki) telepítő eszköz eltárolja a releaseket egy `releases` alkönytárban ahol az aktuális verzió egy symlink az aktuális release könyvtárra. A `rollback` parancs egyszerűvé teszi a visszaállást az előző verzióra.

Minden release-nek rendelkezni kell egy egyedi release azonosítóval, mint például az időpecsét (`2011-04-06-20:32:17`) vagy egy folyamatosan növekvő verzió szám (`v100`). A releasekhez csak hozzáadni lehet és egy release-t nem lehet módosítani miután elkészült. Minden változás új releaset kell, hogy létrehozzon.

A build folyamat indítása az alkalmazás fejlesztője által történik amikor az új kód telepítésre kerül. Futtatás ezzel szemben automatikusan is történhet, olyan esetekben mint szerver újra indítás vagy mint amikor egy elhalt folyamatot újra indít a folyamat menedzser. Ezért a futtatás fázis a lehető legegyszerűbb kell, hogy legyen, mert egy probléma az alkalmazás futása során előfordulhat az éjszaka közepén, amikor nem áll rendelkezésre fejlesztő. A build folyamat lehet komplex, hiszen a folyamat a fejlesztő előtt folyik, aki indítja a telepítést. 