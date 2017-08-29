## XII. Adminisztratív folyamatok
### Az adminisztratív / menedzsment feladatok futtatása mint egyszeri folyamatok 

A [folyamat formáciió](./concurrency) azon folyamatok gyüjteménye amely az alkalmazás üzemszerű működéséhez szükséges. Ezen felül a fejlesztők gyakran szükséges egyszeri adminisztratív vagy karbantartási folyamamatok futtatása mint:

* Adatbázis migráció (pl.: `manage.py migrate` Django-nál, `rake db:migrate` Rails-szel).
* Konzol futtatása (úgy is ismert mint [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop) shell) ami lehetővé teszi tetszőlekes kód futtatását vagy az alkalmazás model felépítésének vizsgálatát az éles adatbázis ellenében. Legtöbb nyelv képes az értelmező futtatására paraméter nélkül (pl.:`python` vagy `perl`) vagy egy külön parancs szolgál erre (pl.: `irb` Ruby, `rails console` Rails esetében).
* Scriptek futtatása ami az alkalmazás kódbázisába berakásra került (pl.: `php scripts/fix_bad_records.php`).

Az egyszeri adminisztratív parancsok futtatása a [hosszan futó folyamatok](./processes) teljesen megegyező környezetben klell hogy történjenek.  Az adott [release](./build-release-run) ellenében futnak, felhasználva ugyanazt a [kódbázist](./codebase) és [konfigurációt](./config) mint a többi folyamat. Az parancsok kódját a az alkalmazás kódjával együtt kell szállítani, elkerülendő a szikronizálási problémákat.

Ugyanazt a [függőség izolációs](./dependencies) technikát kell használni mint a többi folyamat típus esetében. Például ha a Ruby webes folyamat indítása a `bundle exec thin start` paranccsal történik, akkor az adatbázis migrációnak a `bundle exec rake db:migrate` kell használni. Ugyanígy egy Python program esetében ami Virtualenv köryezetet használ, ugyanzokat a vendor függőségeket kell haszni (`bin/python` ) a Tornado webserver és bármely `manage.py` admininsztratív feladat futtatására.
 
A tizenkét-faktor előszeretettel használja azon nyelveket melyek alapból támogatják a REPL paranccssori futtatást, és amelyek egyszerűvé teszik az egyszeri parancsok futtatását. A helyi, feljesztői környezetben a fejlesztők egyszerűen futtathatják az adminisztratív parancsokat direktbe a forráskód könyvtárából. Az éles rendszereken a fejlesztők SSH vagy más, távoli parancsfuttatásra lehetőséget adó eszközök segítségével futtathatják ezen parancsokat.
