# 3. Orthography-specific requirements

> _Work in progress._

Because of the multi-dimensional contextual interactions happening in Indic scripts, a script’s all possible shaping requirements tend to show a long tail of edge/corner cases. Fonts therefore need to restrict their scope to a specific set of orthographies to allow a reasonable amount of workload.

The following sections introduce, within each script group, what shaping behavior is required by a specific orthography.

<!-- The distinction between vowel and consonant is an orthographical issue. -->

## 3.1 Devanagari and Gujarati scripts

<!-- Modularize requirements into subsets according to overlapping of orthographies’ requirements, and refer to the subsets in every orthography? -->

<!-- Every base needs to be marked with conjoining-form eligibility (eg, d_ma can have (d_ma)half although not commonly implemented.) -->

Phantom characters that are discouraged: ॓ ॔

### Devanagari, general

Bases:

- atomically encoded (composite ones in parentheses): अ (आ) इ ई उ ऊ ऋ ए (ऐ) (ओ) (औ) क ख ग घ ङ च छ ज झ ञ ट ठ ड ढ ण त थ द ध न प फ ब भ म य र ल व श ष स ह
- various complex bases: <consonant, (virama, consonant)+> (CONSONANT CLUSTER), <CONSONANT CLUSTER, vowel sign>…

Signs:

- atomically encoded: ् ा ि (reordered) ी ु ू ृ े ै ो ौ ं ः
- repha: pre-base initial <ra, virama> → repha (reordered)
- half forms: pre-base <BASE, virama> → half form of BASE (e.g., pre-base <ka, virama> → k)

<!-- [Or use the linguistic format “ra, virama → rasignabove / _ BASE” instead for a clearer separation of context?] -->
<!-- - bottom-side trailing: _unattested_ (_rasignbelow, etc., are limited_) -->
<!-- - right-side trailing: _limited_ -->

Grapheme ya has a semi-productive right-side form, which appears when the preceding base does not have a half form to conjoin to ya.

<!-- This form is not shaped as a right-side form in OTL’s sense (i.e., in the `pstf` feature), because an OTL right form is assumed not to be a vowel sign carrier. -->

> FIGURE:
> characters <त ्> य → half form त्य
> characters ट <् य> → right-side form ट्य

### Sanskrit-Devanagari/Gujarati

Bases:

- atomically encoded, additional: ॠ ऌ ॡ

Signs:

- atomically encoded, additional: ॄ ॢ ॣ
- marginal: ँ ऽ

<!-- Candrabindu on half la; skip combining marks in the half feature. -->

### Hindi-Devanagari

Signs:

- additional: ॉ ँ ़
- marginal: ऽ

### Marathi/Nepali-Devanagari

Signs:

- Pre-base <ra, virama> has a half form in additional to repha, and the unrelated pre-base <ra, nukta, virama> (equivalent: pre-base <rra, virama>) is exploited for this need; the more systematic pre-base <ra, virama, zerowidthjoiner> is later also specified for the same need.

### Marathi-Devanagari

Signs:

- additional: ॅ ॉ
- marginal: ँ ़

<!-- ### Theoretical completion -->

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

## Bangla and Odia scripts

Signs:

- repha: <rabangla/raasamiya, virama> BASE → rasignabove (repha)
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

### General

Note the Unicode Tamil script is a mixture of Tamil and graphemes loaned from the Grantha script.

Subscript and superscript digits 2–4.

Signs:

- Grantha, conditional: ு ூ

Bases:

- Grantha: ஜ ஷ ஹ
- Grantha, marginal: ஶ
- Grantha, complex: <ka, virama, ssa>, <sa/sha, virama, ra, iisign>
