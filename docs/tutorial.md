# Tutorial on how to build an OTL Devanagari font

This tutorial showcases that, with glyphs already in hand, how one can construct OTL rules to build a basic but functional Devanagari font with desired shaping behavior.

<!-- [Glyph sequences are structured in way that a base is followed by zero or more combining marks. Preceding characters sometimes are shaped into following marks for easier shaping.] -->

<!-- [Workarounds: Eyelash, reordering, ] -->

<!-- [Additional behavior: isign matching, vocalic liquid CV syllable…] -->

<!-- [Character mapping; precomposed characters are needed for certain platforms, although they can be broken down in general shaping.] -->

<!-- [Define the intended scope and glyph set according to the information available in the main text, then list all rules (without omission) in the following sections.] -->

## 4.1 Scope

<!-- [Devanagari and contemporary Hindi] -->

<!-- [Glyph set] -->

## 4.2 Character to glyph mapping

<!-- […] -->

## 4.3 OpenType Layout

<!-- […] -->

```
languagesystem DFLT dflt;
languagesystem dev2 dflt;

feature nukt { ... } nukt;
feature akhn { ... } akhn;
feature rphf { ... } rphf;
feature half { ... } half;

feature pres { ... } pres;

feature abvm { ... } abvm;
feature blwm { ... } blwm;
```

### Language systems

```
languagesystem DFLT dflt;
languagesystem dev2 dflt;
```

### Nukta forms

Under this feature, we forming `*__nukta` glyphs so they can participate in later shaping rules like normal letters.

```
feature nukt {
    lookup nukt.general {

        # Hindi
        sub dda nukta by dda__nukta;
        sub ddha nukta by ddha__nukta;

        # Perso-Arabic, English, etc, loanwords in Hindi
        sub ka nukta by ka__nukta;
        sub kha nukta by kha__nukta;
        sub ga nukta by ga__nukta;
        sub ja nukta by ja__nukta;
        sub pha nukta by pha__nukta;

    } nukt.general;
} nukt;
```

GPOS (blwm) can be used to position nukta on a base instead, then later shaping rules will need to both ignore Signnukta when forming ligatures and position Signnukta on ligature components properly, but also still always recognize the existence of nukta on every component characters (note the ones not next to a virama thus not blocking a virama’s function automatically) and decide whether these clusters should shape in a way consistent to their nukta-less counterparts.

### Akhand Ligatures

<!-- […] -->

```
feature akhn {

    lookup akhn.classical_prioritized_complex_bases {
        sub ka virama ssa by k_ssa;
        sub ja virama nya by j_nya;
    } akhn.classical_prioritized_complex_bases;

    lookup akhn.rakar_complex_bases {
        sub ka virama ra by k_ra;
        sub kha virama ra by kh_ra;
        sub ga virama ra by g_ra;
        sub gha virama ra by gh_ra;
        sub ca virama ra by c_ra;
        ...
    } akhn.rakar_complex_bases;

    lookup akhn.complex_bases {
        sub cha virama ya by ch_ya;
        sub cha virama va by ch_va;
        sub tta virama tta by tt_tta;
        sub tta virama ttha by tt_ttha;
        ...
    } akhn.complex_bases;

} akhn;
```

### Conjoining forms

<!-- [OTL pre-base forms are left forms for a phonetically (and thus in encoding) trailing position. …] -->

```
feature rphf {
    lookup rphf.general {
        sub ra virama by repha;
    } rphf.general;
} rphf.general;

feature half {
    lookup half.general {
        sub ka virama by k;
        sub kha virama by kh;
        sub ga virama by g;
        ...
    } half.general;
} half;
```

### Presentation forms

<!-- […] -->

```
feature pres {
    lookup pres.general {
        ...
    } pres.general;
} pres;
```

### Above- and below-base mark positioning

<!-- [probably needs a note what this feature requires, showing anchors, and how they work] -->

```
feature abvm {
    lookup abvm.general {
        ...
    } abvm.general;
} abvm;

feature blwm {
    lookup blwm.general {
        ...
    } blwm.general;
} blwm;
```
