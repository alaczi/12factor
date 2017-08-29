## I. Kódbázis
### Egy forráskód verzió követő rendszerben, tetszőleges számú telepítés

A tizenkét-faktoros alkalmazás forráskódja minding verziókövető rendszerben kerül tárolásra. Ilyen például a [Git](http://git-scm.com/), [Mercurial](https://www.mercurial-scm.org/) vagy [Subversion](http://subversion.apache.org/). A verziókövető adatbázis egy másolatát *code repository*-nak hívják, melynek gyakori rövidítése a *code repo* vagy csak *repo*.

A *kódbázis* bármely repo (centralizált verzió kezelő rendszerben mint a Subverison), vagy bármely repok halmaza amelyek egy azonos gyökér bejegyzésből származnak (decentralizált verzió követő rendszerben mint a Git)

![Egy kódbázis, több telepítés](/images/codebase-deploys.png)

A kódbázis és az alkalmazás között a reláció mindig egy-ez-egyhez:

* ha több kódbázis van az nem egy alkalmazás -- az egy elosztott rendszer. Az elosztott rendszer minden egyes komponense egy alkalmazás és mindegyik önmagában kell, hogy megfeleljen a tizenkét faktornak.
* ha több alkalmazás használja ugyanazt a kódbázist az a tizenkét-faktor szabályainak megszegése. Ebben az esetben a megoldás a megosztott kód könyvtárakba (libs) rendezése amely ezután [függőségkezelőn](./dependencies) keresztül használható.

Alkalmazásonként csak egy kódbázis, de tetszőleges számú telepítés lehetséges. A *telepítés* az alkalmazás egy futó példánya. Ez általában egy éles rendszer és egy vagy több staging rendszer. Továbbá szintén telepítésnek minősülnek a fejlesztők saját gépein futó példányok.

A kódbázis közös minden telepítés esetében, bár telepítésenként különböző verziók lehetnek aktívak. Például a fejlesztő által elvégzett módosításokat melyek még nem kerültek ki a staging rendszerekre. A staging rendszerek már tartalmaznak változásokat amelyek még nem kerültek élesbe. De mind ugyanazon a kódbázison osztozik, így azonsíthatóak mint az alkalmazás különböző telepítései.

