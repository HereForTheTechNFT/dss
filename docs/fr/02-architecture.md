# Chapitre 2 — Architecture du depot : une constellation de petits contrats

Le README du depot resume la philosophie de conception en trois points : le systeme est agnostique au jeton de collateral (il ne connait que des soldes internes, jamais les jetons externes eux-memes hors des adaptateurs), il est concu pour etre verifiable formellement (le coeur, `vat.sol`, ne fait aucun appel externe et ne comporte aucune perte de precision par division), et il est modulaire (chaque piece peut etre remplacee sans toucher au reste).

`vat.sol` est la base de donnees centrale : elle tient les positions de dette (`urns`), les parametres par type de collateral (`ilks`), et les soldes de Dai et de "sin" (dette non couverte). C'est le seul contrat que tous les autres doivent finalement toucher pour faire bouger de la dette ou du collateral.

Autour du Vat gravitent des contrats specialises : `Jug` calcule et applique les frais de stabilite, `Spotter` pousse les prix de collateral dans le Vat, `GemJoin`/`DaiJoin` (dans `join.sol`) sont les adaptateurs qui font entrer et sortir les jetons externes, `Dog` declenche les liquidations et `Clip` les execute par encheres hollandaises (avec `abaci.sol` pour la courbe de prix), `Vow` gere la comptabilite de la dette du systeme et declenche les encheres de surplus (`flap`) et de deficit (`flop`), `Pot` gere le taux d'epargne Dai (DSR), et `End` orchestre l'arret d'urgence global du systeme.

Chaque contrat utilise le meme systeme d'autorisation minimal : un mapping `wards` d'adresses autorisees, modifie par `rely`/`deny`, verifie par le modificateur `auth`. Il n'y a pas de role hierarchise ni de bibliotheque OpenZeppelin ; c'est le pattern d'autorisation le plus depouille possible, pense pour etre entierement gouverne depuis l'exterieur par un contrat de vote.

[Chapitre suivant : le Vat, base de donnees des CDP](03-le-vat.md)
