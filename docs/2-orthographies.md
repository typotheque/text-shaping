# 2. Script- and orthography-specific shaping requirements

Every language has its own extension to the baseline Sanskrit usage, while certain graphemes and interactions of the Sanskrit usage can also be obsolete for the language.

## 2.5 Devanagari-specific knowledge

## 3.1 Basic character eligibility

<!-- Baseline: Sanskrit
The classic Sanskrit alphabet:
|A| |Aa| |I| |Ii| |U| |Uu| |Vocalicr| |Vocalicrr| |Vocalicl| |Vocalicll| |E| |Ai| |O| |Au|
|Ka| |Kha| |Ga| |Gha| |Nga| |Ca| |Cha| |Ja| |Jha| |Nya| |Tta| |Ttha| |Dda| |Ddha| |Nna| |Ta| |Tha| |Da| |Dha| |Na| |Pa| |Pha| |Ba| |Bha| |Ma|
|Ya| |Ra| |La| |Va| |Sha| |Ssa| |Sa| |Ha|
with signs ◌|Signvirama| ◌|Signaa| |Signi|◌ ◌|Signii| ◌|Signu| ◌|Signuu| ◌|Signvocalicr| ◌|Signvocalicrr| ◌|Signvocalicl| ◌|Signvocalicll| ◌|Signe| ◌|Signai| ◌|Signo| ◌|Signau| ◌|Signanusvara| ◌|Signvisarga|, a modifier letter |Avagraha|, and a marginal sign ◌|Signcandrabindu|.
Various dependent forms and complex bases.
Hindi
Marathi
Nepali
Theoretical completion -->

## 3.2 Interaction eligibility

> FIGURE: two-dimensional table between languages and interactions.

<!-- - Nukta: Generally used on consonant letters, the sign nukta is a secondary modifier that theoretically can actually be applied to any letter and signs. The interaction is often restricted with actual language usage: Hindi: Dda, Ddha; Perso-Arabic: Ka, Kha, Ga, Ja, Pha; English: Ja, Pha; Marathi, Nepali: Ra (Technical consideration: <Ra_Signnukta, Signvirama, <L>> as one of the two ways of encoding R-Deva, if later operations do not consider the <Ra, Signnukta> sequence anymore.); Kashmiri: Ca, Cha, Ja

- Complex bases: CC conjuncts, especially `*_ra` for stemmed letters; Cv conjuncts such as ra_usign, ra_uusign, ha_rsignvocalic. `*Ra`: K|Kh|G|Gh|C|J|Jh|Ny|Nn|T|Th|D|Dh|N|P|Ph|B|Bh|M|Y|L|V|Sh|Ss|S|H

- Dependent form: vowel letter -> sign (only trailing; both sides often encoded atomically), consonant letter -> sign (leading, trailing, and coda; ) `*`: K|Kh|G|Gh|C|J|Jh|Ny|Nn|T|Th|Dh|N|P|Ph|B|Bh|M|Y|R|L|V|Sh|Ss|S

- [Sanskrit has a repha form on a vowel base. Not well supported by shaping engines.]

Combined interactions:

- Dependent forms (i.e., half forms) of obscure conjuncts (i.e., CC conjuncts). `*R`: K|Kh|G|Gh|C|J|Jh|Ny|Nn|T|Th|Dh|N|P|Ph|B|Bh|M|Y|L|V|Sh|Ss|S -->
