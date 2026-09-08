# Chapitre 8 — Join, les adaptateurs de collateral et le jeton Dai

Le Vat ne connait que des soldes internes abstraits (`gem[ilk][usr]`, `dai[usr]`) ; il ne detient jamais directement de jetons ERC-20 externes. Le pont entre les deux mondes est fait par les adaptateurs de `join.sol`, un choix de conception qui isole le coeur verifiable formellement de la diversite et des bizarreries des standards de jetons.

`GemJoin` est l'adaptateur generique pour un collateral ERC-20 bien comporte : `join` transfere les jetons de l'appelant vers l'adaptateur puis credite `gem[ilk][usr]` dans le Vat via `slip` ; `exit` fait l'inverse. Le README precise que des adaptateurs specialises existent pour des besoins particuliers (ether natif, jetons a decimales non standard) : `GemJoin` n'est qu'un exemple de reference, chaque nouveau type de collateral peut necessiter son propre adaptateur audite separement.

`DaiJoin` fait le pont symetrique pour le Dai lui-meme : le Dai "interne" du Vat (une simple entree dans le mapping `dai`) n'est pas directement transferable hors du systeme. `DaiJoin.exit` deplace du Dai interne vers l'adaptateur puis frappe (`mint`) l'equivalent en jeton ERC-20 `DSToken` externe pour l'utilisateur ; `join` fait l'inverse en brulant le jeton externe et creditant le Dai interne. C'est cette etape qui fait qu'un solde de Dai "en circulation" (le jeton ERC-20 que l'on voit dans un portefeuille) est toujours le miroir exact d'un solde de Dai retire du Vat.

[Chapitre suivant : Dog et le declenchement des liquidations](09-dog-et-bark.md)
