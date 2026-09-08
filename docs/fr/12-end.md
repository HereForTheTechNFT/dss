# Chapitre 12 — End, le settlement global (Global Settlement)

`End.sol` implemente le mecanisme d'arret d'urgence du systeme, un filet de securite qui permet a la gouvernance de fermer proprement le protocole (par exemple face a une faille critique ou une defaillance d'oracle prolongee) tout en garantissant que chaque detenteur de Dai puisse recuperer une part equitable du collateral restant.

`cage()`, reserve a la gouvernance, gele instantanement l'ensemble du systeme : elle appelle `cage` en cascade sur le Vat et tous les modules peripheriques (`Cat`, `Dog`, `Vow`, `Spotter`, `Pot`, `Cure`), figeant les prix et empechant toute nouvelle position ou tout nouveau frob. `cage(ilk)`, appelable ensuite par n'importe qui pour chaque ilk, fige le taux de conversion collateral/Dai de reference (`tag`) a partir du dernier prix d'oracle connu.

`skim` liquide chaque position individuellement au `tag` fige (plutot que par enchere), transferant a `End` le collateral necessaire pour couvrir la dette de la position au taux de reference. `snip`/`skip` gerent le cas des encheres de liquidation deja en cours au moment du `cage`, en les annulant proprement. Une fois toutes les positions traitees et un delai `wait` ecoule, `thaw()` fixe la dette totale definitive du systeme ; `flow(ilk)` calcule alors le taux de conversion final par ilk (`fix`), qui peut differer legerement du `tag` initial si le collateral disponible ne suffit pas exactement a couvrir toute la dette. Les detenteurs de Dai utilisent enfin `pack` (mettre leur Dai en file d'attente) puis `cash` (recuperer leur part de collateral au taux `fix`) pour sortir du systeme avec une part proportionnelle et equitable de ce qu'il reste.

[Chapitre suivant : limites et perimetre](13-limites-et-perimetre.md)
