## X. Fejlesztői és éles környezet hasonlósága
### Fejlesztői, staging és éles környezetek legyenek a lehető leghasonlóbbak 

Tradícionálisan lényeges eltérés volt a fejlesztői (a fejlesztő saját gépén változtatásokat végez az alkalmazás [kódján](./codebase)) és éles (az alkalmazás futó telepítése ami publikusan elérhető a felhasználók számára) környezetek között. Ez eltérések a következő területeket érinthetik:

* **Időbeni eltérés**: A fejlesztő napokat, heteket akár hónapokat is dolgozik a kódon mielőtt élesbe kerül
* **Személyi eltérés**: A feljesztő írja a kódot, az üzemeltetés telepíti
* **Eszközbeni eltérés**: A fejlesztő más eszközöket (nginx, SQLite, osx) használ mint az éles telepítés (Apache, MySQL, Linux) 

**A tizenkét-faktoros alkalmazás tervezésekor a [folyamtos telepítés](http://avc.com/2011/02/continuous-deployment/) követelményeit tartjuk szem előtt minimalizálva a különbséget a fejlesztői és éles verziók között**. A fentieket figyelembe véve:

* Legyen az időbeni eltérés kisebb: a fejlesztő által eszközölt változtatás órákkal de akár percekkel később is telepítésre kerülhet.
* Legyen a személyi eltérés minimális: a kódot író fejlesztő részt vesz a telepítésben és képes az éles működést figyelemmel kísérni.
* Legyen az eszközökbeni eltérés minimális: a fejlesztői és éles rendszerek legyenek a lehető leghasonlóbbak.

Összegezve a fentieket egy táblázatban:

<table>
  <tr>
    <th></th>
    <th>Tradícionális alkalmazás</th>
    <th>Tizenkét-faktoros alkalmazás</th>
  </tr>
  <tr>
    <th>Telepítések közötti idő</th>
    <td>Hetek</td>
    <td>Órák</td>
  </tr>
  <tr>
    <th>A kódot írók és akik a telepítést végzik</th>
    <td>Különböző személyek</td>
    <td>Azonos személyek</td>
  </tr>
  <tr>
    <th>Fejlsztői és éles rendszerek</th>
    <td>Különbözőek</td>
    <td>A lehető leghasonlóbbak</td>
  </tr>
</table>


A [támogató szolgáltatások](./backing-services), mint az alkalmazás adatbázisa, városor vagy cache, az egyik legfontosabb terület ahol a fejlesztői - éles rendszer hasonlósága igen fontos. legtöbb nyelv rendelkezik eszközkönyvtárakkal ami megkönnyiti a háttér szolgáltatások elérését, beleértve az *adaptereket* is az adott szogáltatásokhoz. Néhány példa a következő táblázatban.
 
<table>
  <tr>
    <th>Típus</th>
    <th>Nyelv</th>
    <th>Könyvtár</th>
    <th>Adapter</th>
  </tr>
  <tr>
    <td>Adatbázis</td>
    <td>Ruby/Rails</td>
    <td>ActiveRecord</td>
    <td>MySQL, PostgreSQL, SQLite</td>
  </tr>
  <tr>
    <td>Queue</td>
    <td>Python/Django</td>
    <td>Celery</td>
    <td>RabbitMQ, Beanstalkd, Redis</td>
  </tr>
  <tr>
    <td>Cache</td>
    <td>Ruby/Rails</td>
    <td>ActiveSupport::Cache</td>
    <td>Memória, filerendszer, Memcached</td>
  </tr>
</table>

A fejlesztők gyakran helyettesítik a támogató szolgáltatásokat valami könnyedebb verzióval, amíg az éles rendszeren komolyabb, robosztusabb megoldás kerül használatra. Például SQLite lokálisan és PostgreSQL éles rendszerben, vagy memoria cache fejlesztéskor és memcached az éles környezetben. 

**A tizenkét-faktoros alkalmazás nem támogatja a különböző szolgáltatások használatát a fejlesztői és éles rendszerekben**, még akkor sem, ha elméletben az adapterek absztrakciói képesek elrejteni a szogáltatások különbözőségeit. A kis kompatibilitás beli különbségek bármikor felmerülhetnek és ennek eredményekép ami működött a fejlesztői környezetben, hibát jelezhet az éles környezetben. Ezek a hibák súrlódáshoz vezetnek és ellenérzést szülhetnek a folyamatos telepítéssel szemben. A surlódások és ennek következtében a folyamatos telepítés háttérbe szorításának költsége az alkalmazás életcikusa alatt szélsőségesen nagyra nőhet.  

Mindzonáltal a könnyebb verziók használata a háttérszolgáltatásoknak egyre kevésbé vonzó. A modern szolgáltatások mint Memcached, Postgres, RabbitMQ telepítése már nem bonyolult, hála a modern csomag menedzser eszközöknek mint a [Homebrew](http://mxcl.github.com/homebrew/) vagy az [apt-get](https://help.ubuntu.com/community/AptGet/Howto). Alternatív megoldásként szóba jöhet deklaratív telepítő eszközök mint például a [Chef](http://www.opscode.com/chef/) vagy [Puppet](http://docs.puppetlabs.com/) kombinálása virtuális környezetekkel mint a [Vagrant](http://vagrantup.com/) melyek lehetővé teszik a fejlesztők számára, hogy saját gépükön az éles rendszerekhez teljesen hasonló környezetben dolgozzanak. Ezen eszközök installálási és használati ráfordítása pedig alacsony ha figyelembe veszük a az előnyeit a hasonló fejlesztői és éles rendszerek használatának.

A különböző háttérszolgáltatások adapterei továbbra is hasznosak, mert egy esetleges váltás esetén megkönnyítik az átállást. De minden alkalmazás telepítés (fejlesztői, staging, éles) ugyanazt a típusú és verziójú támogató szolgáltatást kell, hogy használja.