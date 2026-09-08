# Chapitre 5 — Les Ilks, le taux accumule et le reglement de la dette

`suck` et `heal` forment une paire symetrique pour la dette non couverte par une position ("sin", la dette du systeme lui-meme plutot que d'un utilisateur). `suck(u, v, rad)` cree simultanement du Dai chez `v` et de la dette chez `u` — c'est la fonction utilisee par `Vow` pour couvrir des frais ou par `Pot` pour payer l'interet d'epargne, une forme de creation monetaire interne au systeme. `heal(rad)` fait l'inverse : elle annule une dette du systeme (`sin`) contre du Dai que l'appelant possede, reduisant `debt` et `vice` (dette totale non couverte) du meme montant.

`fold(i, u, rate)` est la fonction qui fait vivre le `rate` accumule d'un ilk : elle l'incremente de `rate` (le delta calcule ailleurs, typiquement par `Jug.drip`), recalcule la dette totale de l'ilk au nouveau taux, et credite la difference en Dai a l'adresse `u` (le plus souvent `Vow`, le collecteur des frais de stabilite du systeme). C'est la fonction qui transforme un taux d'interet abstrait en Dai reel accumule quelque part.

Le Vat distingue `debt` (le Dai total emis, toujours couvert par du collateral ou par de la dette du systeme comptabilisee) de `vice` (la dette totale non couverte, "sin" agregee). Ces deux compteurs globaux, avec `Line` (le plafond de dette total), donnent une vue d'ensemble de la sante du systeme sans avoir a parcourir chaque position individuellement.

[Chapitre suivant : Jug, les frais de stabilite](06-jug.md)
