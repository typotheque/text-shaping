# 3. Orthography-specific shaping requirements

Every language has its own extension to the baseline Sanskrit usage, while certain graphemes and interactions of the Sanskrit usage can also be obsolete for the language.

## Devanagari and Gujarati scripts

Signs:

- repha: <ra, virama> BASE → rasignabove (repha) <!-- [Or use the linguistic format “ra, virama → rasignabove / _ BASE” instead for a clearer separation of context?] -->
<!-- - bottom-side trailing: _unattested_ (_rasignbelow, etc., are limited_) -->
- half forms: <BASE, virama> BASE → kasignpre/… (k/…) and <ra, virama, zerowidthjoiner> BASE → rasignpre (r)
- half forms: <BASE, virama> BASE → kasignpre/… (k/…)
<!-- - right-side trailing: _limited_ -->

Devanagari has a semi-productive right form of Ya, which appears when there isn’t a special complex base form between a stemless consonant and Ya. This form is not shaped as a right form in OTL’s sense (i.e., in the `pstf` feature), because an OTL right form is assumed not to be a vowel sign carrier.

> FIGURE:  
> characters <त ्> य → half form त्य  
> characters ट <् य> → right conjoining form ट्य

Bases:

- …


## 3.1 Basic character eligibility

<!-- Baseline: Sanskrit
The classic Sanskrit alphabet:
|A| |Aa| |I| |Ii| |U| |Uu| |Vocalicr| |Vocalicrr| |Vocalicl| |Vocalicll| |E| |Ai| |O| |Au|
|Ka| |Kha| |Ga| |Gha| |Nga| |Ca| |Cha| |Ja| |Jha| |Nya| |Tta| |Ttha| |Dda| |Ddha| |Nna| |Ta| |Tha| |Da| |Dha| |Na| |Pa| |Pha| |Ba| |Bha| |Ma|
|Ya| |Ra| |La| |Va| |Sha| |Ssa| |Sa| |Ha|
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





## Gurmukhi script

Signs:

- repha: <ra, virama, zerowidthjoiner> BASE → rasignabove (repha), _historical_
- bottom-side trailing: BASE <virama, ha/ra/va/…> → ha/ra/va/…signbelow (_additional ones are historical_)
- half forms: <BASE, virama, zerowidthjoiner> BASE → sasignpre, _historical_
- right-side trailing: BASE <virama, ya/…> → ya/…signpost _(additional ones are historical)_

## Assamese–Bangla and Odia scripts

Signs:

- repha: <rabangla/raassamese, virama> BASE → rasignabove (repha)
- repha: <ra, virama> BASE → rasignabove (repha)
- bottom-side trailing: BASE <virama, ra/ba> → ra/basignbelow <!-- [Note Odia’s ambiguous encoding] -->
- right-side trailing: BASE <virama, ya> → yasignpost (yasign) <!-- [Note Odia’s ambiguous encoding] -->

## Telugu and Kannada scripts

Signs:

- repha: <ra, virama, zerowidthjoiner> BASE (modern font) or <ra, virama> BASE (historical font) → rasignpostbaseinitial (repha), _historical_
- repha: <ra, virama> BASE → rasignpostbaseinitial (repha)

- left-side trailing: BASE <virama, ra> → rasignprebasetrailing, _stylistic_

- bottom-side trailing: BASE <virama, BASE> → khasignbelow/… (khasign)
- right-side trailing: BASE <virama, BASE> → kasignpost/… (kasign)

## Malayalam script

Signs:

- Atomic: …, rephdot (reordered).
- repha: <rephdot> BASE → rasignabove (repha), _traditional orthography_
- left-side trailing: BASE <virama, ra> → rasignprebasetrailing, _simplified orthography_
- bottom-side trailing: BASE <virama, BASE> → kasignbelow/…, _traditional orthography_; BASE <virama, la> → lasignbelow, _simplified orthography (limited)_
- right-side trailing: BASE <virama, ya/va> → ya/vasignpost (ya/vasign)

## Tamil script

Signs:

- …

Bases:

- …
