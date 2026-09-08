# Chapitre 7 — Spotter, prix et ratio de liquidation

`Spotter.sol` est le pont entre les oracles de prix externes (`PipLike`, une interface minimaliste avec une seule fonction `peek` qui renvoie un prix et un booleen de validite) et le `spot` stocke dans chaque `Ilk` du Vat, la valeur reellement utilisee par `frob` pour juger si une position est sure.

`poke(ilk)` lit le prix brut aupres de l'oracle configure pour cet ilk, puis calcule `spot = prix / par / mat`, ou `par` est la valeur de reference du Dai (en principe 1, mais ajustable par gouvernance pour des mecanismes de stabilisation avances) et `mat` le ratio de liquidation de l'ilk (par exemple 1.5 pour un collateral qui exige 150% de collateralisation). Le resultat est donc directement le prix "de securite" : un collateral dont la valeur de marche vaut exactement `mat` fois sa dette a un `spot` tout juste egal a sa dette, au bord de la liquidation.

Cette architecture separe delibarement la lecture du prix (potentiellement couteuse ou sujette a manipulation a court terme) du calcul de securite fait dans `frob` : `Spotter.poke` doit etre appele explicitement (par un bot ou un keeper) pour que le Vat voit un nouveau prix, ce qui laisse a la gouvernance la possibilite de configurer des oracles avec un delai (TWAP, mediane) avant que le prix n'affecte les liquidations.

[Chapitre suivant : Join, les adaptateurs de collateral et le Dai](08-join-et-dai.md)
