# Chapitre 6 — Jug, les frais de stabilite

`Jug.sol` calcule et applique les frais de stabilite (l'interet paye par les emprunteurs de Dai) pour chaque type de collateral. Chaque ilk a un `duty`, un taux d'interet par seconde exprime en ray, auquel s'ajoute un `base` global commun a tous les ilks.

`drip(ilk)` est la fonction centrale : elle calcule le nouveau `rate` accumule via `_rpow(base + duty, temps_ecoule, ONE)`, une exponentiation entiere en virgule fixe implementee en assembly Yul pour composer l'interet sur la duree ecoulee depuis le dernier `drip` (`rho`, le timestamp de la derniere mise a jour). Elle applique ensuite la difference au Vat via `fold`, qui credite cette difference a l'adresse `vow` — c'est ainsi que les interets payes par les emprunteurs financent le contrat `Vow`.

L'appel a `drip` n'est pas automatique : n'importe qui peut l'appeler pour n'importe quel ilk, a n'importe quel moment, et le systeme reste correct meme si personne ne l'appelle pendant longtemps (l'interet s'accumule simplement sur une periode plus longue au prochain appel). C'est un motif recurrent dans dss : plutot que de forcer une mise a jour a chaque interaction (couteux en gaz), le systeme accumule un etat en attente que quiconque peut "declencher" ("drip", "poke", "kick" reviennent partout dans le depot avec ce role).

[Chapitre suivant : Spotter, prix et ratio de liquidation](07-spotter.md)
