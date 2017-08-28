## VII. Port hozzárendelés
### Szolgáltatások kiajánlása port hozzárendeléssel 

Webes alkalmazások gyarkan kerülnek futtatásra egy webszerver konténerben. Például a PHP alkalmazások futhatnak mint [Apache HTTPD](http://httpd.apache.org/) modul, vagy a Java alkalmazások [Tomcat](http://tomcat.apache.org/)-ban.

** A tizenkét-faktoros alkalmazás teljesen önálló** és nem függhet egy webserverbe történő beágyazástól, hogy webes szolgáltatást nyújtson. A webes alkalmazás **kiajánlja a HTTP-t mint szolgáltatást egy porton keresztül** és azon a porton várakozik a beérkező kérésekre.

Lokális fejlesztői környezetben a fejlesztő direktben hívhat az adott szolgáltatás URLjére mint pl: `http://localhost:5000/`, hogy elérje az alkalmazás által nyújtott szolgáltatást. Telepített környezetben egy útvonal választó réteg kezeli a kéréseket a publikus oldalon és továbbítja az alkalmazás által kiszolgált portra.

Tipikus implementáció, hogy a [függőség kezelés](./dependencies) segítségével egy webszerver könyvtárad adunk az alkalmazáshoz mit [Tornado](http://www.tornadoweb.org/) Python-hoz, [Thin](http://code.macournoyer.com/thin/) Ruby-hoz, vagy [Jetty](http://www.eclipse.org/jetty/) Javahoz és más JVM-alapú nyelvekhez. Ez teljesen a *felhasználó oldalán* történik, beágyazva az alkalmazás kódjába. A szerződés a futtató környezettel a porthoz kötődés a kérések kiszolgálásához.

A HTTP nem az egyedüli szolgáltatás ami kiajánlható port hozzáredeléssel. Szinte mindenfajta szerver szoftver futtatása lehetséges a folyamat porthoz kötésével és a beérkező kérések figyelésével. Kiváló példák a [ejabberd](http://www.ejabberd.im/) ([XMPP](http://xmpp.org/)), és [Redis](http://redis.io/) ([Redis protocol](http://redis.io/topics/protocol)).

Megjegyzendő hogy a porthoz rendelés azt is jelenti, hogy az alkalmazás egy másik alkalmazás [támogató szolgáltatásává](./backing-services) válhat, szimplán hozzádva a támogató alkalmazás URL elérését a kliens alkalmazás [konfigjában](./config)  
