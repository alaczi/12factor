## XI. Logok
### Logok kezelése mint esemény folyamatok 

A *logok* biztosítják az alkalmazás viselkedésének átláthatóságát. Egy szerver alapú környezetben általános megoldás egy file-ba írni őket. De ez csak egy lehetséges kimeneti formátum.

*Logs* provide visibility into the behavior of a running app.  In server-based environments they are commonly written to a file on disk (a "logfile"); but this is only an output format.

A logok az összes futó folyamat és támogató szolgáltatások kimenetéről összegyüjtött, összefésült, időben rendezett eseméyek [adatfolyama](https://adam.herokuapp.com/past/2011/4/1/logs_are_streams_not_files/). A logok nyers formájukban tipikusan text formátumúak, egy sor egy eseményt tartalmaz (bár hiba esetén a backtrace több soros is lehet). A logoknak nincs fix kezdete és vége, folyamatosan generálódik az alkalmazás működése alatt.
  
**A tizenkét-faktoros alkalmazás sosem kezeli a kiemeneti folyam átirányitását vagy tárolását.** Sosem próbál log file(okat) írni vagy azokat menedzselni. Ehelyett minden futó folyamat az esemény folyamát a standard outputra (`stdout`) írja, bufferelés nélkül. A fejlesztés folyamán a fejlesztők egyszerűen láthatják és monitorozhaták a kimenetet ahogy az alkalmazásást a termináljukban futtatják.   

Staging és éles telepítések esetén minden folyamat kimenetét a futtató környezet összegyűjti és a megfelelő módon egy vagy több helyre átirányitja megtekintésre vagy hosszú távú tárolásra. Ezek a tárolási helyek nem láthatóak vagy konfigurálhatóak az alkalmazás által, menedzselésük teljes egészében a futtató környezet felelősége. Ezen célra open-source megoldások állnak rendelkezésre mint a [Logplex](https://github.com/heroku/logplex) vagy [Fluent](https://github.com/fluent/fluentd))

Az alkalmazás kimenetét átirányíthatjuk fileba vagy megfigyeljetjük egy terminálon keresztül a `tail` segítségével. Legjobb megoldás azonban továbbítani egy log indexelő és analizáló rendszerbe mint [Splunk](http://www.splunk.com/) vagy általános célú adattárház rendszerbe mint a [Hadoop/Hive](http://hive.apache.org/). Ezen rendszerek kiváló és rugalmas szolgáltatásokat biztosítanak az alkalmazás időbeni viselkedésének vizsgálatára, ideértve:

* Egy múltbeli esemény megtalálása.
* Trendek széles körű grafikonos analizíálása (mint például a lekérések száma percenként).
* Aktív monitorozás és figyelmeztető rendszer felhasználó által definiált mértékeken (például amikor a percenkénti hibák száma meghalad egy határértéket).
