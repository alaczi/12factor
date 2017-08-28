## III. Konfiguráció
### A konfiguráció tárolása környezeti változókban 

Az alkalmazás *konfigurációja* minden ami változhat a különböző [telepítések](./codebase) között (staging, éles, fejlesztői környezetek). Ez magában foglalja:

* Támogató  [háttérszolgáltatások](./backing-services) konfigurációs paramétereit  mint például az adatbázis, Memchached stb.
* Különböző külső szolgáltatások - mint például Amazon S3, Twitter - azonosításra használt adatait
* Elő-telepítési értékeket mint például a hostnév

Néhány alkalmazás ezeket a konfigurációs adatokat a kódban beágyazva mint konstans tárolja. Ez a tizekét-faktor megsértése, ami megköveteli a **konfiguráció szigorú elválasztását** a forráskódtól. A konfiguráció változik a telepítések között, a forráskód nem.

A legegyszerűbb módja, hogy teszteljük, hogy a konfiguráció megfelelően elválasztott-e a forráskódtól, hogy ha a forráskódot nyiltá tesszük, semmilyen érzékeny adat nem kerül megosztásra.

Megjegyzendő, hogy a *konfiguráció* definiciója **nem tartalmazza** az alkalmazás belső konfigurációját mint például a `config/routes.rb` Rails esetében vagy ahogyan a [kód modulok kapcsolódnak](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html) [Spring](http://spring.io/) esetében. Ez a fajta konfiguráció nem változik a telepítések között, így a forráskódban tárolandó.

Egy másik megközelítése a "konfiguráció" tárolásának a config file-ok melyek nem kerülnek hozzáadásra a verzió kezelőbe, mint például a `config/database.yml` Railsben. Ez nagy előre lépés kódba égetett konstansokkal szemben, de megvan a lehetősége, hogy valaki véletlenül berakja a konfigurációs fileokat a verziókezelőbe. Továbbá lehetséges, hogy a különböző konfigurációs fileok különböző helyeken és formátumban kerülnek tárolásra ami megnehezíti a helyes konfiguráció menedzselését. Továbbá ezek a formátumok általában nyelv és/vagy keretrendszer függőek.

**A tizenkét-faktoros alkalmazást a konfigurációt *környezeti változókban* tárolja** (gyakran rövidítve *env vars* vagy *env*). A környezeti változók egyszerűen változtathatók a különböző telepítéseknél és ellentétben a konfig fileokkal, valószínűtlen, hogy valaki véletlenül hozzáadná a forráskódhoz, és az egyedi kofingurációs file formátumokkal, vagy egyéb konfigurációs mechanizmusokkal mint például a Java System Properties, nyelv és keretrendszer függetlenek.

A konfiguáció management egy másik aspektusa a csoportosítás. Néha az alkalmazás a konfigurációt az telepítések alapján elnevezett csoportokba csomagolja (gyakran hivják ezt "environments"-nek), mint például a `development`, `test`, és `production` Rails esetében. Ez a methódus azonban nem fenntartható, ahogy több és több telepítés jön létre, új elnevezések szükségesek mint `staging` vagy `qa`. Ahogy a project tovább növekszik, a fejlesztők saját speciális környezetüket is hozzáadhatják pl: `joes-staging` ami a lehetséges konfigurációk kombinációinak exponenciális növekedését eredményezi, sérülékennyé téve a konfiguráció managementet. 

A tizenkét-faktoros alkalmazásban a környezeti változok finom szabályzó mechanizmusok és egymástól teljesen függetlenek. Sosem csoportosítottak telepítésenként, helyette függetlenül menedzseltek. Ez a model egyszerűen skálázható ahogy az alkalmazás az életciklusa alatt természetes módon növekszik továbbí telepítésekkel.