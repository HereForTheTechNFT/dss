# Chapitre 13 — Limites connues et perimetre de ce parcours

Ce depot, historiquement `makerdao/dss`, redirige desormais vers `sky-ecosystem/dss` suite au rebranding de MakerDAO en Sky Protocol : le code et l'architecture documentes ici restent ceux du systeme MCD original, la migration de marque n'a pas change la logique des contrats presentee dans ce parcours.

Ce parcours couvre le "coeur" du systeme (`vat`, `jug`, `spot`, `join`, `dog`, `clip`, `abaci`, `vow`, `pot`, `end`) mais laisse de cote plusieurs pieces presentes dans le depot ou son ecosysteme : l'ancien module de liquidation par encheres anglaises (`cat.sol`/`flip.sol`, conserve pour compatibilite historique mais remplace en production par `dog.sol`/`clip.sol`), `cure.sol` (l'agregateur de creances utilise par `End` pour les cas ou une partie du collateral a ete recuperee hors-chaine), ainsi que toute la couche de gouvernance (le module de vote et le jeton MKR/SKY eux-memes, qui vivent dans des depots separes).

La licence du depot est AGPL-3.0-or-later, une licence copyleft plus stricte que celles vues dans les parcours precedents (MIT pour Curve et Lido, BUSL/GPL pour Aave et Uniswap) : toute reutilisation ou modification distribuee du code doit rester open source sous les memes termes.

Rien n'a ete installe, compile, deploye ni execute pour ecrire ces chapitres. Aucun test n'a ete lance ; ces chapitres decrivent ce que le code Solidity dit faire, en renvoyant aux fichiers cites. Le depot fournit sa propre suite de tests (dossier `src/test/`) pour verification independante.
