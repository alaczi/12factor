## VIII. Párhuzamosság
### Skálázás a folyamat model segítségével

Bármely számítógépes program egy vagy több folyamatot idíthat. Webes alkalmazások különböző megközelítést alkalmazhatnak a kiszolgáló folyamatok indításhoz. A PHP folyamatok az Apache gyermek folyamataiként léteznek, szükség esetén kerülnek indításra. A Java folyamatok ezzel ellentétes megközelítést használnak: a JVM egy nagy ős-folyamatként indul, lefoglalva egy nagyobb szeletet a rendszer erőforrásaiból (CPU, memória) és a konkurenciát saját magán belül szálak segítségével menedzseli. Mindkét esetben a futó folyamatok minimálisan láthatóak az alkalmazás fejlesztője számára.

![A futó folyamatok száma arányos a terheléssel, a terhelés változatossága a folyamatok típusával.](/images/process-types.png)

**A tizenkét-faktoros alkalmazásban a folyamatok elsődleges prioritást élveznek**. A tizenkét-faktoros alkalmazás folyamat kezelése hasonló [szolgálatás démonok futtatása a unix folyamat model segítségével](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/). Felhasználva ezt a modelt a tervezési fázis során a különböző feladatok hozzárendelhetőel a különböző *folyamat típusokhoz*. Például a HTTP kérések `web process`-ként, míg egy hosszan futó háttérfeladat `worker process`-ként kezelhető a leghatékonyabban.
 
A fentiek nem zárják ki a folyamatok saját belső multiplex működését, mint például a szálak egy VM-en belül vagy az aszinkron / esemény alapú model az [EventMachine](http://rubyeventmachine.com/), [Twisted](http://twistedmatrix.com/trac/), vagy [Node.js](http://nodejs.org/) esetében. Azonban egy virtuális gép nem nőhet a végtelenségik, így az alkalmazásnak képesnek kell lennie több folyamat párhuzamos indítására, akár különböző fizikai gépeken is.

A folyamat model előnyei akkor látszanak igazán amikor eljön az ideje az alkalmazás skálázásának. Az [elszigetelt, horizontálisan particionált folyamatok](./processes) eredményeként a konkurencia növelése egyszerű és megbízható művelet. 

A tizenkét-faktoros alkalmazás folyamatainak [tilos démonizálódni](http://dustin.github.com/2010/02/28/running-processes.html) vagy PID fileokat írni. Ehelyett az operációs rendszer folyamatkezelőjét kell használni (mint az [Upstart](http://upstart.ubuntu.com/), elosztott folyamatkezelő egy felhő platformon, vagy egyedi eszközök mint a [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) a fejlesztői környezetben) a [kimenetek](./logs) menedzselésére, az elhalt folyamatok észlelésére és a felhasználói újraindítások és leállítások kezelésére.