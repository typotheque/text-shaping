# B.1 Tutorial on how to build an OTL Devanagari font

> _Work in progress._

This tutorial showcases that, with glyphs already in hand, how one can construct OTL rules to build a basic but functional Devanagari font with desired shaping behavior.

## B.1.1 Scope of the exemplar font project

- Script: देवनागरी Devanagari
    - The script suffix `-deva` is omitted from glyph namaes in this tutorial.
- Orthography: contemporary हिंदी Hindi and मराठी Marathi

<!-- [Glyph sequences are structured in way that a base is followed by zero or more combining marks. Preceding characters sometimes are shaped into following marks for easier shaping.] -->

<!-- [Workarounds: Eyelash, reordering, ] -->

<!-- [Additional behavior: isign matching, vocalic liquid CV syllable…] -->

<!-- [Character mapping; precomposed characters are needed for certain platforms, although they can be broken down in general shaping.] -->

<!-- [Define the intended scope and glyph set according to the information available in the main text, then list all rules (without omission) in the following sections.] -->

<!-- [Glyph set] -->

## B.1.2 Character-to-glyph mapping

> FIGURE: U+0905 → अ (a)

character | | glyph | note
-- | -- | -- | --
0901 | ँ | candrabindu | ≈ esigncandra anusvara
0902 | ं | anusvara |
0903 | ः | visarga |
0905 | अ | a |
0906 | आ | aa | a aasign
0907 | इ | i |
0908 | ई | ii | ≈ i repha
0909 | उ | u |
090A | ऊ | uu |
090B | ऋ | vocalicr |
… | … | … |

## B.1.3 Scripts and language systems

```
languagesystem DFLT dflt;
languagesystem dev2 dflt;
```

<!-- Under this feature, we forming `*__nukta` glyphs so they can participate in later shaping rules like normal letters.

GPOS (blwm) can be used to position nukta on a base instead, then later shaping rules will need to both ignore Signnukta when forming ligatures and position Signnukta on ligature components properly, but also still always recognize the existence of nukta on every component characters (note the ones not next to a virama thus not blocking a virama’s function automatically) and decide whether these clusters should shape in a way consistent to their nukta-less counterparts. -->

## B.1.4 Reordering

> _Work in progress._

## B.1.5 Lookups

### Complex bases

> FIGURE:\
> ड (dda) ़ (nukta) → ड़ (dddha)\
> र (ra) ु (usign) → रु (r_u)\
> क (ka) ् (virama) ष (ssa) → क्ष (k_ssa)

```
lookup base.conjoinable.with_nukta {

    # Hindi:
    sub dda nukta by dddha;
    sub ddha nukta by rha;

    # Marathi Eyelash encoded with nukta:
    sub ra nukta by rra;

    # Perso-Arabic, English, etc., loanwords in Hindi:
    sub ka nukta by qa;
    sub kha nukta by khha;
    sub ga nukta by ghha;
    sub ja nukta by za;
    sub pha nukta by fa;

} base.conjoinable.with_nukta;
```

```
lookup base.conjoinable {

    # Classic prioritized:
    sub ka virama ssa by k_ssa;
    sub ja virama nya by j_nya;

} base.conjoinable;
```

```
lookup base.conjoinable.C_ra {

    sub ka virama ra by k_ra;
    sub kha virama ra by kh_ra;
    sub ga virama ra by g_ra;
    sub gha virama ra by gh_ra;
    sub nga virama ra by ng_ra;
    sub ca virama ra by c_ra;
    sub cha virama ra by ch_ra;
    sub ja virama ra by j_ra;
    sub jha virama ra by jh_ra;
    sub nya virama ra by ny_ra;
    sub tta virama ra by tt_ra;
    sub ttha virama ra by tth_ra;
    sub dda virama ra by dd_ra;
    sub ddha virama ra by ddh_ra;
    sub nna virama ra by nn_ra;
    sub ta virama ra by t_ra;
    sub tha virama ra by th_ra;
    sub da virama ra by d_ra;
    sub dha virama ra by dh_ra;
    sub na virama ra by n_ra;
    sub pa virama ra by p_ra;
    sub pha virama ra by ph_ra;
    sub ba virama ra by b_ra;
    sub bha virama ra by bh_ra;
    sub ma virama ra by m_ra;
    sub ya virama ra by y_ra;
    sub la virama ra by l_ra;
    sub va virama ra by v_ra;
    sub sha virama ra by sh_ra;
    sub ssa virama ra by ss_ra;
    sub sa virama ra by s_ra;
    sub ha virama ra by h_ra;
    sub lla virama ra by ll_ra;

    # Output of lookup base.conjoinable.with_nukta:
    sub qa virama ra by q_ra;
    sub khha virama ra by khh_ra;
    sub ghha virama ra by ghh_ra;
    sub za virama ra by z_ra;
    sub fa virama ra by f_ra;

} base.conjoinable.C_ra;
```

```
lookup base.post_conjoining {

    # Stemmed, thus conjoinable and should have conjoining form:
    sub cha-deva virama-deva ya-deva by ch_ya-deva;
    sub tta-deva virama-deva ya-deva by tt_ya-deva;
    sub ttha-deva virama-deva ya-deva by tth_ya-deva;
    sub dda-deva virama-deva ya-deva by dd_ya-deva;
    sub ddha-deva virama-deva ya-deva by ddh_ya-deva;
    sub da-deva virama-deva ma-deva by d_ma-deva;
    sub da-deva virama-deva ya-deva by d_ya-deva;
    sub ha-deva virama-deva ma-deva by h_ma-deva;
    sub ha-deva virama-deva ya-deva by h_ya-deva;

    # With ending component that is conjoinable when inline:
    sub cha-deva virama-deva va-deva by ch_va-deva;
    sub tta-deva virama-deva va-deva by tt_va-deva;
    sub ttha-deva virama-deva va-deva by tth_va-deva;
    sub dda-deva virama-deva va-deva by dd_va-deva;
    sub ddha-deva virama-deva va-deva by ddh_va-deva;
    sub da-deva virama-deva ga-deva by d_ga-deva;
    sub da-deva virama-deva gha-deva by d_gha-deva;
    sub da-deva virama-deva dha-deva by d_dha-deva;
    sub da-deva virama-deva na-deva by d_na-deva;
    sub da-deva virama-deva ba-deva by d_ba-deva;
    sub da-deva virama-deva bha-deva by d_bha-deva;
    sub da-deva virama-deva va-deva by d_va-deva;
    sub ha-deva virama-deva nna-deva by h_nna-deva;
    sub ha-deva virama-deva na-deva by h_na-deva;
    sub ha-deva virama-deva la-deva by h_la-deva;
    sub ha-deva virama-deva va-deva by h_va-deva;

    # Non-conjoinable; less prioritized than C_ra bases:
    sub tta-deva virama-deva tta-deva by tt_tta-deva;
    sub tta-deva virama-deva ttha-deva by tt_ttha-deva;
    sub ttha-deva virama-deva ttha-deva by tth_ttha-deva;
    sub dda-deva virama-deva dda-deva by dd_dda-deva;
    sub dda-deva virama-deva ddha-deva by dd_ddha-deva;
    sub ddha-deva virama-deva ddha-deva by ddh_ddha-deva;
    sub da-deva virama-deva da-deva by d_da-deva;

} base.post_conjoining;
```

```
lookup base.post_conjoining.with_sign {

    sub ra usign by r_u;
    sub ra uusign by r_u;
    sub ha vocalicrsign by h_vocalicr;

    # Having recognizable components that have conjoining forms:
    sub t ta by t_ta;

} base.post_conjoining.with_sign;
```

### Complex signs

<!-- [OTL pre-base forms are left forms for a phonetically (and thus in encoding) trailing position. …] -->

> FIGURE:
> र (ra) ् (virama) → {repha} (repha)
> क (ka) ् (virama) → {k} (k)

```
lookup sign.repha {
    sub ra virama by repha;
} sign.repha;
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

    # Output of lookup base.conjoinable.with_nukta:
    sub qa virama by q;
    sub khha virama by khh;
    sub ghha virama by ghh;
    sub za virama by z;
    sub fa virama by f;
    sub rra virama by r;  # Eyelash encoded with nukta.

    # Output of lookup base.conjoinable:
    sub k_ssa virama by k_ss;
    sub j_nya virama by j_ny;

    # Output of lookup base.conjoinable.C_ra:
    sub k_ra virama by k_r;
    sub kh_ra virama by kh_r;
    sub g_ra virama by g_r;
    sub gh_ra virama by gh_r;
    sub c_ra virama by c_r;
    sub j_ra virama by j_r;
    sub jh_ra virama by jh_r;
    sub ny_ra virama by ny_r;
    sub nn_ra virama by nn_r;
    sub t_ra virama by t_r;
    sub th_ra virama by th_r;
    sub dh_ra virama by dh_r;
    sub n_ra virama by n_r;
    sub p_ra virama by p_r;
    sub ph_ra virama by ph_r;
    sub b_ra virama by b_r;
    sub bh_ra virama by bh_r;
    sub m_ra virama by m_r;
    sub y_ra virama by y_r;
    sub l_ra virama by l_r;
    sub v_ra virama by v_r;
    sub sh_ra virama by sh_r;
    sub ss_ra virama by ss_r;
    sub s_ra virama by s_r;
    sub q_ra virama by q_r;
    sub khh_ra virama by khh_r;
    sub ghh_ra virama by ghh_r;
    sub z_ra virama by z_r;
    sub f_ra virama by f_r;

} sign.left;
```

```
lookup sign.post_conjoining {
    sub t t by t_t;
} sign.post_conjoining;
```

### Typographical treatments

```
lookup typographical.sign {

    sub repha anusvara by repha_anusvara;

    sub iisign anusvara by iisign_anusvara;
    sub iisign repha by iisign_repha;
    sub iisign repha anusvara by iisign_repha_anusvara;
    sub esign anusvara by esign_anusvara;
    sub esign repha by esign_repha;
    sub esign repha anusvara by esign_repha_anusvara;
    sub aisign anusvara by aisign_anusvara;
    sub aisign repha by aisign_repha;
    sub aisign repha anusvara by aisign_repha_anusvara;
    sub osign repha by osign_repha;
    sub osign repha anusvara by osign_repha_anusvara;
    sub ausign anusvara by ausign_anusvara;
    sub ausign repha by ausign_repha;
    sub ausign repha anusvara by ausign_repha_anusvara;

    sub esigncandra anusvara by esigncandra_anusvara;

} typographical.sign;
```

### Mark positioning

<!-- [probably needs a note what this feature requires, showing anchors, and how they work] -->

> _Work in progress._

## B.1.6 Referring to lookups in features

```
feature akhn {
    lookup base.conjoinable.with_nukta;
    lookup base.conjoinable;
} akhn;

feature rphf {
    lookup sign.repha;
} rphf;

feature rkrf {
    lookup base.conjoinable.C_ra;
} rkrf;

feature half {
    lookup sign.left;
} half;

feature cjct {
    lookup base.post_conjoining;
    lookup sign.post_conjoining;
    lookup base.post_conjoining.with_sign;
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
