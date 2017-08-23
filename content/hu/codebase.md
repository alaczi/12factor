## I. Kódbázis
### Egy forráskód verzió követő rendszerben, tetszőleges számú telepítés

A tizenkét-faktoros alkalmazás forráskódja minding verziókövető rendszerben tárolt. Mint például a [Git](http://git-scm.com/), [Mercurial](https://www.mercurial-scm.org/) vagy [Subversion](http://subversion.apache.org/). A verziókövet adatbázis egy másolatát *code repository*-nak hívják, gyakran rövidítve *code repo* vagy csak *repo*.

A *kódbázis* bármely repo (egy centralizált verzió kezelő rendszerben mint as Subverison), vagy bármely repok halmaza amelyek egy azonos gyökér bejegyzésből származnak (decentralizált verzió követő rendszerben mint a Git)


![One codebase maps to many deploys](/images/codebase-deploys.png)

A kódbázis és az alkalmazás között mindig egy-ez-egyhez a reláció:

* ha több kódbázis van az nem egy alkalmazás -- az egy elosztott rendszer. Minden egyes komponens az elosztott rendszerben egy alkalmazás és mindegyik önmagában kell, hogy megfeleljen a tizenkét faktornak.
* ha több alkalmazás osztja meg ugyanazt a kódbázist az a tizenkét-faktor szabályainak megszegése. A megoldás ebben az esetben a megosztott kód könyvtárakba (libs) rendezése ami ezután használható a [függőségkezelőn](./dependencies) keresztül

Csak egy kódbázis lehetséges alkalmazásonként, de tetszőleges számú telepítés. A *telepítés* az alkalmazás egy futó példánya. Ez általában egy éles rendszer és egy vagy több staging rendszer. Továbbá minden fejlesztőnek lehet egy futó példánya a helyi fejlesztői környezetben, melyek szintén telepítésnek minősülnek.

A kódbázis ugyanaz minden telepítés esetében, bár telepítésenként különböző verziók lehetnek aktívak. Például a fejlesztő már elvégzett módosításokat melyek még nem kerültek ki a staging rendszerekre. A staging rendszerek tartalmaznak változásokat melyek még nem kerültek élesbe. De mind ugyanazon a kódbázison osztozik, így azonsíthatóak mint az alkalmazás különböző telepítései.

