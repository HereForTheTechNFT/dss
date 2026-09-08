# Chapitre 4 — frob et la manipulation d'un CDP

`frob(i, u, v, w, dink, dart)` est la fonction la plus dense du Vat : c'est elle qui deplace du collateral et de la dette pour un `Urn`. Son nom (argot pour "fiddle with", bidouiller) reflete son role generique : un seul appel sert aussi bien a deposer du collateral, en retirer, emprunter du Dai, ou en rembourser, selon le signe de `dink` (variation de collateral) et `dart` (variation de dette normalisee).

Les trois adresses `u`, `v`, `w` peuvent differer : `u` est le proprietaire de la position modifiee, `v` la source ou destination du collateral, `w` la destination du Dai emprunte. Cette separation permet des flux sophistiques (par exemple un contrat tiers qui emprunte pour le compte d'un utilisateur) tout en verifiant le consentement de chaque partie separement via `wish` (basee sur `hope`/`nope`, l'equivalent d'une approbation ERC-20 mais pour l'autorisation d'agir sur une position).

Apres avoir applique les deltas, `frob` verifie une serie d'invariants dans un ordre precis : soit la dette diminue, soit les plafonds de dette (`ilk.line`, `Line` global) ne sont pas depasses ; soit la position devient plus sure, soit elle reste dans les clous du ratio de collateralisation (`tab <= ink * spot`, ou `tab` est la dette en valeur, `spot` le prix de securite deja ajuste de la marge de securite par `Spotter`) ; et enfin soit la dette de la position est nulle, soit elle depasse le plancher anti-poussiere `dust` — une position avec trop peu de dette pour justifier le cout de sa liquidation eventuelle est simplement interdite.

`fork` permet de scinder une position en deux en respectant les memes contraintes de securite des deux cotes ; `grab`, reserve aux contrats autorises (typiquement `Dog`), est la version qui ignore ces contraintes de consentement et de securite : c'est la fonction utilisee pour confisquer le collateral d'une position en cours de liquidation.

[Chapitre suivant : les Ilks et l'accumulation du taux](05-ilks-et-rate.md)
