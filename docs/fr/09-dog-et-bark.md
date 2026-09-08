# Chapitre 9 — Dog et le declenchement des liquidations (bark)

`Dog.sol` est le module de liquidation 2.0 de dss (son commentaire d'en-tete le dit explicitement, en reference a l'ancien module `Cat`/`Flip`, toujours present dans le depot pour compatibilite mais remplace en production par `Dog`/`Clip`). Chaque ilk a un `Clipper` associe, une penalite de liquidation `chop`, et un plafond de dette liquidable en simultane (`hole` par ilk, `Hole` global), avec `dirt`/`Dirt` qui suivent la consommation courante de ce plafond.

`bark(ilk, urn, kpr)` est la fonction que n'importe quel keeper peut appeler sur une position sous-collateralisee. Elle verifie d'abord l'insecurite de la position (`ink * spot < art * rate`), puis calcule la portion de la position a liquider : idealement la totalite, mais bridee par l'espace restant dans les plafonds `Hole`/`hole` de l'ilk. Une logique specifique gere le cas ou une liquidation partielle laisserait un reliquat "poussiereux" (`dust`) : dans ce cas la position entiere est liquidee plutot que de laisser une miette non economiquement liquidable plus tard.

`bark` confisque le collateral et la dette via `vat.grab` (transferant le collateral au `Clipper` et la dette a `Vow`), signale la dette a couvrir a `Vow.fess`, puis declenche l'enchere elle-meme via `ClipperLike(milk.clip).kick`. Un `kpr` (keeper) qui declenche la liquidation peut recevoir une prime, financee par une creation de Dai via `vat.suck` — une incitation economique explicite a surveiller le systeme et liquider rapidement les positions a risque.

[Chapitre suivant : Clip, les encheres hollandaises et Abacus](10-clip-et-abaci.md)
