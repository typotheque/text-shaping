# A. Tutorial on how to build an OTL Devanagari font

This tutorial showcases that, with glyphs already in hand, how one can construct OTL rules to build a basic but functional Devanagari font with desired shaping behavior.

## A.1 Scope of the exemplar font

- Script: Devanagari
- Orthography: contemporary Hindi

<!-- [Glyph sequences are structured in way that a base is followed by zero or more combining marks. Preceding characters sometimes are shaped into following marks for easier shaping.] -->

<!-- [Workarounds: Eyelash, reordering, ] -->

<!-- [Additional behavior: isign matching, vocalic liquid CV syllable…] -->

<!-- [Character mapping; precomposed characters are needed for certain platforms, although they can be broken down in general shaping.] -->

<!-- [Define the intended scope and glyph set according to the information available in the main text, then list all rules (without omission) in the following sections.] -->

<!-- [Glyph set] -->

## A.2 Character to glyph mapping

> FIGURE: U+0905 → अ (a)

character | glyph | note
-- | -- | --
0901 | candrabindu |
0902 | anusvara |
0903 | visarga |
0905 | a |
0906 | aa | composite
0907 | i |
0908 | ii |
0909 | u |
090A | uu |
… | … |

## A.3 Scripts and language systems

```
languagesystem DFLT dflt;
languagesystem dev2 dflt;
```

<!-- Under this feature, we forming `*__nukta` glyphs so they can participate in later shaping rules like normal letters.

GPOS (blwm) can be used to position nukta on a base instead, then later shaping rules will need to both ignore Signnukta when forming ligatures and position Signnukta on ligature components properly, but also still always recognize the existence of nukta on every component characters (note the ones not next to a virama thus not blocking a virama’s function automatically) and decide whether these clusters should shape in a way consistent to their nukta-less counterparts. -->

## A.4 Lookups

### Complex bases

> FIGURE: ड (dda) ़ (nukta) → ड़ (dddha)

```
lookup nukta_forms {

    # Hindi
    sub dda nukta by dddha;
    sub ddha nukta by rha;

    # Perso-Arabic, English, etc, loanwords in Hindi
    sub ka nukta by qa;
    sub kha nukta by khha;
    sub ga nukta by ghha;
    sub ja nukta by za;
    sub pha nukta by fa;

} nukta_forms;
```

> FIGURE:  
> क (ka) ् (virama) ष (ssa) → क्ष (k_ssa)  
> र (ra) ु (usign) → रु (ra_usign)

```
lookup complex_bases.classical {
    sub ka virama ssa by k_ssa;
    sub ja virama nya by j_nya;
} complex_bases.classical;
```

```
lookup complex_bases.rakar_forms {

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

    sub qa virama ra by q_ra;
    sub khha virama ra by khh_ra;
    sub ghha virama ra by ghh_ra;
    sub za virama ra by z_ra;
    sub fa virama ra by f_ra;

} complex_bases.rakar_forms;
```

```
lookup complex_bases.vowel_sign_forms {
    sub ra usign by ra_usign;
    sub ra uusign by ra_uusign;
    sub ha vocalicrsign by ha_vocalicrsign;
} complex_bases.vowel_sign_forms;
```

```
lookup complex_bases.general {
    sub cha virama ya by ch_ya;
    sub cha virama va by ch_va;
    sub tta virama tta by tt_tta;
    sub tta virama ttha by tt_ttha;
    sub tta virama ya by tt_ya;
    sub tta virama va by tt_va;
    sub ttha virama ttha by tth_ttha;
    sub ttha virama ya by tth_ya;
    sub ttha virama va by tth_va;
    sub dda virama dda by dd_dda;
    sub dda virama ddha by dd_ddha;
    sub dda virama ya by dd_ya;
    sub dda virama va by dd_va;
    sub ddha virama ddha by ddh_ddha;
    sub ddha virama ya by ddh_ya;
    sub ddha virama va by ddh_va;
    sub ta virama ta by t_ta;
    sub da virama ga by d_ga;
    sub da virama gha by d_gha;
    sub da virama da by d_da;
    sub da virama dha by d_dha;
    sub da virama na by d_na;
    sub da virama ba by d_ba;
    sub da virama bha by d_bha;
    sub da virama ma by d_ma;
    sub da virama ya by d_ya;
    sub da virama va by d_va;
    sub ha virama nna by h_nna;
    sub ha virama na by h_na;
    sub ha virama ma by h_ma;
    sub ha virama ya by h_ya;
    sub ha virama la by h_la;
    sub ha virama va by h_va;
} complex_bases.general;
```

### Complex signs

<!-- [OTL pre-base forms are left forms for a phonetically (and thus in encoding) trailing position. …] -->

> FIGURE:  
> र (ra) ् (virama) → {repha} (repha)  
> क (ka) ् (virama) → {k} (k)

```
lookup repha {
    sub ra virama by repha;
} repha;
```

```
lookup half_forms {

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
    sub ra virama by r;
    sub la virama by l;
    sub va virama by v;
    sub sha virama by sh;
    sub ssa virama by ss;
    sub sa virama by s;

    sub k_ssa virama by k_ss;
    sub j_nya virama by j_ny;

    sub qa virama by q;
    sub khha virama by khh;
    sub ghha virama by ghh;
    sub za virama by z;
    sub fa virama by f;

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

    sub t_ta virama by t_t;

} half_forms;
```

### Reordering

…

### Typographical treatments

<!-- […] -->

```
lookup sign_interaction {
    sub esign repha by esign_repha;
    ...
} sign_interaction;
```

### Mark positioning

<!-- [probably needs a note what this feature requires, showing anchors, and how they work] -->

…

## A.5 Registering lookups under features

```
languagesystem DFLT dflt;
languagesystem dev2 dflt;

feature akhn {
  lookup nukta_forms;
  lookup complex_bases.classical;
  lookup complex_bases.rakar_forms;
  lookup complex_bases.vowel_sign_forms;
  lookup complex_bases.general;
} akhn;

feature rphf {
  lookup repha;
} rphf;

feature half {
  lookup half_forms;
} half;

feature pres {
  lookup sign_interaction;
} pres;

feature abvm {
  lookup above_base_mark_positioning;
} abvm;

feature blwm {
  lookup below_base_mark_positioning;
} blwm;
```
