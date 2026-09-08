# Chapitre 3 — Le Vat, base de donnees des CDP

`Vat.sol` est volontairement le contrat le plus austere du systeme : pas d'evenements personnalises dans cette version, pas d'appel externe, uniquement de l'arithmetique et des `require`. Deux structures portent tout l'etat economique : `Ilk` (un type de collateral, avec sa dette totale normalisee `Art`, son taux accumule `rate`, son prix de securite `spot`, son plafond de dette `line` et son plancher anti-poussiere `dust`), et `Urn` (une position individuelle, avec son collateral verrouille `ink` et sa dette normalisee `art`).

La distinction entre dette "normalisee" (`art`) et dette reelle est centrale : la dette reelle d'une position est `art * ilk.rate`. `rate` commence a `10**27` (un ray) au moment de l`init` d'un ilk et ne fait que croitre via `fold` (chapitre 6, appele par `Jug`) : multiplier `art` par un `rate` plus grand fait grossir la dette de tous les emprunteurs de cet ilk simultanement, sans avoir a modifier individuellement chaque `Urn` — le meme principe d'index multiplicatif que l'on retrouve chez Aave ou Compound, mais implemente ici en une seule ligne (`ilk.rate = _add(ilk.rate, rate)`).

Le systeme d'unites de dss est strict : `wad` (18 decimales, pour les montants de jetons), `ray` (27 decimales, pour les taux), `rad` (45 decimales, `wad * ray`, pour les montants de Dai en interne). Cette discipline d'unites, associee a l'absence totale de division dans `frob` (les seules divisions du systeme sont dans les contrats peripheriques comme `Jug` ou `Spotter`), est ce qui rend le Vat amenable a la verification formelle mentionnee dans le README.

[Chapitre suivant : frob et la manipulation d'un CDP](04-frob.md)
