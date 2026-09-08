# Chapitre 1 — Presentation de MakerDAO / Sky (dss)

`dss` (Dai Stablecoin System) est le coeur du protocole qui emet le Dai, un stablecoin sur-collateralise et decentralise. A la difference d'un stablecoin adosse a des reserves bancaires, chaque Dai en circulation est garanti par une position de dette collateralisee (CDP, ici appelee "vault" ou "urn") : un utilisateur depose un actif en garantie et emprunte du Dai contre cette garantie, avec obligation de rester sur-collateralise sous peine de liquidation.

MakerDAO s'est rebaptise Sky Protocol : le depot historique `makerdao/dss` redirige aujourd'hui vers `sky-ecosystem/dss`, mais le code et l'architecture decrits ici restent ceux du systeme MCD (Multi Collateral Dai) tel qu'il a ete developpe et audite sous le nom Maker.

Le systeme est deliberativement mine ("modular"), a l'oppose d'Aave ou Curve qui centralisent l'essentiel de la logique dans un ou deux contrats : chaque responsabilite (comptabilite des dettes, frais, prix, liquidation, encheres, arret d'urgence) vit dans son propre petit contrat, relies entre eux par une autorisation explicite (`rely`/`auth`).

Ce parcours s'appuie sur les fichiers de `src/` : `vat.sol`, `jug.sol`, `spot.sol`, `join.sol`, `dog.sol`, `clip.sol`, `abaci.sol`, `vow.sol`, `pot.sol`, `end.sol`. Rien n'a ete installe, compile ni execute pour ecrire ces chapitres.

[Chapitre suivant : architecture du depot](02-architecture.md)
