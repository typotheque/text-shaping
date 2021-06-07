# 2.1 Per-script tutorial: देवनागरी Devanagari

> _Work in progress._

This tutorial showcases that, with glyphs already in hand, how one can construct OTL rules to build a basic but functional Devanagari font with desired shaping behavior.

## B.1.1 Scope of the exemplar font project

- Script: देवनागरी Devanagari
    - The script suffix `-deva` is omitted from glyph names in this tutorial.
- Orthography: contemporary हिंदी Hindi and मराठी Marathi

<!-- [Glyph sequences are structured in way that a base is followed by zero or more combining marks. Preceding characters sometimes are shaped into following marks for easier shaping.] -->

<!-- [Workarounds: Eyelash, reordering, ] -->

<!-- [Additional behavior: sign-i matching, vocalic liquid CV syllable…] -->

<!-- [Character mapping; precomposed characters are needed for certain platforms, although they can be broken down in general shaping.] -->

<!-- [Define the intended scope and glyph set according to the information available in the main text, then list all rules (without omission) in the following sections.] -->

<!-- [Glyph set] -->

## B.1.2 Character-to-glyph mapping

> FIGURE: U+0905 → अ (a)

character | | glyph | note
-- | -- | -- | --
0901 | ँ | candrabindu | confusable: sign-candra-e anusvara
0902 | ं | anusvara |
0903 | ः | visarga |
0905 | अ | a |
0906 | आ | aa | a sign-aa
0907 | इ | i |
0908 | ई | ii | confusable: i repha
0909 | उ | u |
090A | ऊ | uu |
090B | ऋ | vocalic-r |
… | … | … |

## B.1.3 Scripts and language systems

```
languagesystem DFLT dflt;
languagesystem dev2 dflt;
```

<!-- Under this feature, we forming `*_nukta` glyphs so they can participate in later shaping rules like normal letters.

GPOS (blwm) can be used to position nukta on a base instead, then later shaping rules will need to both ignore nukta when forming ligatures and position nukta on ligature components properly, but also still always recognize the existence of nukta on every component characters (note the ones not next to a virama thus not blocking a virama’s function automatically) and decide whether these clusters should shape in a way consistent to their nukta-less counterparts. -->

## B.1.4 Reordering

> _Work in progress._

## B.1.5 Lookups

### Complex shaping

> FIGURE:\
> ड (dda) ़ (nukta) → ड़ (ddxa)\
> र (ra) ु (sign-u) → रु (r-u)\
> क (ka) ् (virama) ष (ssa) → क्ष (k-ssa)

<!-- [OTL pre-base forms are left forms for a phonetically (and thus in encoding) trailing position. …] -->

> FIGURE:
> र (ra) ् (virama) → {repha} (repha)
> क (ka) ् (virama) → {k} (k)

```
lookup base.conjoinable {

    # Classic prioritized:
    sub ka virama ssa by k-ssa;
    sub ja virama nya by j-nya;

    # Hindi:
    sub dda nukta by ddxa;
    sub ddha nukta by ddhxa;

    # Perso-Arabic, English, etc., loanwords in Hindi:
    sub ka nukta by kxa;
    sub kha nukta by khxa;
    sub ga nukta by gxa;
    sub ja nukta by jxa;
    sub pha nukta by phxa;

    # For Eyelash encoded with nukta:
    sub ra nukta by rxa;

} base.conjoinable;
```

```
lookup sign.repha {
    sub ra virama by repha;
} sign.repha;
```

```
lookup base.conjoinable.someRa {

    sub ka virama ra by k-ra;
    sub kha virama ra by kh-ra;
    sub ga virama ra by g-ra;
    sub gha virama ra by gh-ra;
    sub nga virama ra by ng-ra;
    sub ca virama ra by c-ra;
    sub cha virama ra by ch-ra;
    sub ja virama ra by j-ra;
    sub jha virama ra by jh-ra;
    sub nya virama ra by ny-ra;
    sub tta virama ra by tt-ra;
    sub ttha virama ra by tth-ra;
    sub dda virama ra by dd-ra;
    sub ddha virama ra by ddh-ra;
    sub nna virama ra by nn-ra;
    sub ta virama ra by t-ra;
    sub tha virama ra by th-ra;
    sub da virama ra by d-ra;
    sub dha virama ra by dh-ra;
    sub na virama ra by n-ra;
    sub pa virama ra by p-ra;
    sub pha virama ra by ph-ra;
    sub ba virama ra by b-ra;
    sub bha virama ra by bh-ra;
    sub ma virama ra by m-ra;
    sub ya virama ra by y-ra;
    sub la virama ra by l-ra;
    sub va virama ra by v-ra;
    sub sha virama ra by sh-ra;
    sub ssa virama ra by ss-ra;
    sub sa virama ra by s-ra;
    sub ha virama ra by h-ra;
    sub lla virama ra by ll-ra;

    # Output of lookup base.conjoinable:
    sub kxa virama ra by kx-ra;
    sub khxa virama ra by khx-ra;
    sub gxa virama ra by gx-ra;
    sub jxa virama ra by jx-ra;
    sub phxa virama ra by phx-ra;

} base.conjoinable.someRa;
```

```
lookup base.conjoinable.lower_priority {
    sub ta virama ta by t-ta;
} base.conjoinable.lower_priority;
```

```
lookup sign.left {

    sub ka virama by k;
    sub kha virama by kh;
    sub ga virama by g;
    sub gha virama by gh;
    sub ca virama by c;
    sub ja virama by j;
    sub jha virama by jh;
    sub nya virama by ny;
    sub nna virama by nn;
    sub ta virama by t;
    sub tha virama by th;
    sub dha virama by dh;
    sub na virama by n;
    sub pa virama by p;
    sub pha virama by ph;
    sub ba virama by b;
    sub bha virama by bh;
    sub ma virama by m;
    sub ya virama by y;
    sub ra virama by r;  # Eyelash encoded with ZWJ (which requests the half form of ra).
    sub la virama by l;
    sub va virama by v;
    sub sha virama by sh;
    sub ssa virama by ss;
    sub sa virama by s;
    sub lla virama by ll;

    # Output of lookup base.conjoinable:
    sub k-ssa virama by k-ss;
    sub j-nya virama by j-ny;
    sub kxa virama by kx;
    sub khxa virama by khx;
    sub gxa virama by gx;
    sub jxa virama by jx;
    sub phxa virama by phx;
    sub rxa virama by r;  # Eyelash encoded with nukta.

    # Output of lookup base.conjoinable.someRa:
    sub k-ra virama by k-r;
    sub kh-ra virama by kh-r;
    sub g-ra virama by g-r;
    sub gh-ra virama by gh-r;
    sub c-ra virama by c-r;
    sub j-ra virama by j-r;
    sub jh-ra virama by jh-r;
    sub ny-ra virama by ny-r;
    sub nn-ra virama by nn-r;
    sub t-ra virama by t-r;
    sub th-ra virama by th-r;
    sub dh-ra virama by dh-r;
    sub n-ra virama by n-r;
    sub p-ra virama by p-r;
    sub ph-ra virama by ph-r;
    sub b-ra virama by b-r;
    sub bh-ra virama by bh-r;
    sub m-ra virama by m-r;
    sub y-ra virama by y-r;
    sub l-ra virama by l-r;
    sub v-ra virama by v-r;
    sub sh-ra virama by sh-r;
    sub ss-ra virama by ss-r;
    sub s-ra virama by s-r;
    sub kx-ra virama by kx-r;
    sub khx-ra virama by khx-r;
    sub gx-ra virama by gx-r;
    sub jx-ra virama by jx-r;
    sub phx-ra virama by phx-r;

    # Output of lookup base.conjoinable.lower_priority:
    sub t-ta virama by t-t;

} sign.left;
```

```
lookup base.post_conjoining {

    # Stemmed, thus conjoinable and should have conjoining form:
    sub cha virama ya by ch-ya;
    sub da virama ma by d-ma;
    sub da virama ya by d-ya;
    sub ha virama ma by h-ma;
    sub ha virama ya by h-ya;

    # With ending component that is conjoinable when inline:
    sub cha virama va by ch-va;
    sub tta virama va by tt-va;
    sub ttha virama va by tth-va;
    sub dda virama va by dd-va;
    sub ddha virama va by ddh-va;
    sub da virama ga by d-ga;
    sub da virama gha by d-gha;
    sub da virama dha by d-dha;
    sub da virama na by d-na;
    sub da virama ba by d-ba;
    sub da virama bha by d-bha;
    sub da virama va by d-va;
    sub ha virama nna by h-nna;
    sub ha virama na by h-na;
    sub ha virama la by h-la;
    sub ha virama va by h-va;

    # Non-conjoinable; less prioritized than C-ra bases:
    sub tta virama tta by tt-tta;
    sub tta virama ttha by tt-ttha;
    sub ttha virama ttha by tth-ttha;
    sub dda virama dda by dd-dda;
    sub dda virama ddha by dd-ddha;
    sub ddha virama ddha by ddh-ddha;
    sub da virama da by d-da;

} base.post_conjoining;
```

```
lookup sign.lower_priority {
    sub virama ya by sign-ya;
} sign.lower_priority;
```

```
lookup base.lower_priority {
    sub ra sign-u by r-u;
    sub ra sign-uu by r-u;
    sub ha sign-vocalic-r by h-vocalic-r;
} base.lower_priority;
```

### Typographical treatments

```
lookup typographical.sign {

    sub repha anusvara by repha_anusvara;

    sub sign-ii anusvara by sign-ii_anusvara;
    sub sign-ii repha by sign-ii_repha;
    sub sign-ii repha anusvara by sign-ii_repha_anusvara;
    sub sign-e anusvara by sign-e_anusvara;
    sub sign-e repha by sign-e_repha;
    sub sign-e repha anusvara by sign-e_repha_anusvara;
    sub sign-ai anusvara by sign-ai_anusvara;
    sub sign-ai repha by sign-ai_repha;
    sub sign-ai repha anusvara by sign-ai_repha_anusvara;
    sub sign-o anusvara by sign-o_anusvara;
    sub sign-o repha by sign-o_repha;
    sub sign-o repha anusvara by sign-o_repha_anusvara;
    sub sign-au anusvara by sign-au_anusvara;
    sub sign-au repha by sign-au_repha;
    sub sign-au repha anusvara by sign-au_repha_anusvara;

    sub sign-candra-e anusvara by sign-candra-e_anusvara;

} typographical.sign;
```

### Mark positioning

<!-- [probably needs a note what this feature requires, showing anchors, and how they work] -->

> _Work in progress._

## B.1.6 Referring to lookups in features

```
feature akhn {
    lookup base.conjoinable;
} akhn;

feature rphf {
    lookup sign.repha;
} rphf;

feature rkrf {
    # Can’t be formed before `lookup sign.repha` because of Adobe shaping engines’ bug.
    lookup base.conjoinable.someRa;
    lookup base.conjoinable.lower_priority;
} rkrf;

feature half {
    lookup sign.left;
} half;

feature cjct {
    lookup base.post_conjoining;
    lookup sign.lower_priority;
    lookup base.lower_priority;
} cjct;

feature pres {
    lookup typographical.sign;
    ...
} pres;

feature abvm {
    lookup positioning.mark.top;
} abvm;

feature blwm {
    lookup positioning.mark.bottom;
} blwm;
```
