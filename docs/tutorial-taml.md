# B.2 Tutorial on how to build an OTL Tamil font

> _Work in progress._

## B.2.1 Scope of the exemplar font project

- Script: தமிழ் Tamil
    - The script suffix `-taml` is omitted from glyph namaes in this tutorial.
- Orthography: contemporary தமிழ் Tamil

## B.2.2 Character-to-glyph mapping

> FIGURE: U+0B85 → அ (a)

character | | glyph | note
-- | -- | -- | --
0B83 | ஃ | aytham | SIGN VISARGA is misnomer.
0B85 | அ | a |
0B86 | ஆ | aa | ≈ a uusign
0B87 | இ | i |
0B88 | ஈ | ii |
0B89 | உ | u |
0B8A | ஊ | uu |
0B8E | எ | e |
0B8F | ஏ | ee |
0B90 | ஐ | ai |
… | … | … |

## B.2.3 Scripts and language systems

```
languagesystem DFLT dflt;
languagesystem taml dflt;
languagesystem tml2 dflt;
```

## B.2.4 Reordering

> _Work in progress._

## B.2.5 Lookups

### Complex bases

```
lookup base {

    sub ka virama ssa by kssa;

    sub ka isign by ki;
    sub ka iisign by kii;
    sub ka usign by ku;
    sub ka uusign by kuu;
    sub nga isign by ngi;
    sub nga iisign by ngii;
    sub nga usign by ngu;
    sub nga uusign by nguu;
    sub ca isign by ci;
    sub ca iisign by cii;
    sub ca usign by cu;
    sub ca uusign by cuu;
    sub nya isign by nyi;
    sub nya iisign by nyii;
    sub nya usign by nyu;
    sub nya uusign by nyuu;
    sub tta isign by tti;
    sub tta iisign by ttii;
    sub tta usign by ttu;
    sub tta uusign by ttuu;
    sub nna isign by nni;
    sub nna iisign by nnii;
    sub nna usign by nnu;
    sub nna uusign by nnuu;
    sub ta isign by ti;
    sub ta iisign by tii;
    sub ta usign by tu;
    sub ta uusign by tuu;
    sub na isign by ni;
    sub na iisign by nii;
    sub na usign by nu;
    sub na uusign by nuu;
    sub pa isign by pi;
    sub pa iisign by pii;
    sub pa usign by pu;
    sub pa uusign by puu;
    sub ma isign by mi;
    sub ma iisign by mii;
    sub ma usign by mu;
    sub ma uusign by muu;
    sub ya isign by yi;
    sub ya iisign by yii;
    sub ya usign by yu;
    sub ya uusign by yuu;
    sub ra isign by ri;
    sub ra iisign by rii;
    sub ra usign by ru;
    sub ra uusign by ruu;
    sub la isign by li;
    sub la iisign by lii;
    sub la usign by lu;
    sub la uusign by luu;
    sub va isign by vi;
    sub va iisign by vii;
    sub va usign by vu;
    sub va uusign by vuu;
    sub llla isign by llli;
    sub llla iisign by lllii;
    sub llla usign by lllu;
    sub llla uusign by llluu;
    sub lla isign by lli;
    sub lla iisign by llii;
    sub lla usign by llu;
    sub lla uusign by lluu;
    sub rra isign by rri;
    sub rra iisign by rrii;
    sub rra usign by rru;
    sub rra uusign by rruu;
    sub nnna isign by nnni;
    sub nnna iisign by nnnii;
    sub nnna usign by nnnu;
    sub nnna uusign by nnnuu;
    sub ja isign by ji;
    sub ja iisign by jii;
    sub ja usign by ju;
    sub ja uusign by juu;
    sub ssa isign by ssi;
    sub ssa iisign by ssii;
    sub ssa usign by ssu;
    sub ssa uusign by ssuu;
    sub sa isign by si;
    sub sa iisign by sii;
    sub sa usign by su;
    sub sa uusign by suu;
    sub ha isign by hi;
    sub ha iisign by hii;
    sub ha usign by hu;
    sub ha uusign by huu;
    sub kssa isign by kssi;
    sub kssa iisign by kssii;
    sub kssa usign by kssu;
    sub kssa uusign by kssuu;
    sub sha isign by shi;
    sub sha iisign by shii;
    sub sha usign by shu;
    sub sha uusign by shuu;

    sub sa virama ra iisign by srii;
    sub sha virama ra iisign by srii;

} base;
```

### Stylistic alternatives

```
lookup stylistic.alt_ra.contextual {
    sub ra' virama by ra.alt;
} stylistic.alt_ra.contextual;

lookup stylistic.alt_ra.single {
    sub ri by ri.alt;
    sub rii by rii.alt;
} stylistic.alt_ra.single;
```

```
lookup stylistic.traditional {

    sub nna aasign by nnaa.traditional;
    sub rra aasign by rraa.traditional;
    sub nnna aasign by nnnaa.traditional;

    sub aisign nna by nnai.traditional;
    sub aisign la by lai.traditional;
    sub aisign lla by llai.traditional;
    sub aisign nnna by nnnai.traditional;

} stylistic.traditional;
```

### Mark positioning

> _Work in progress._

## B.2.6 Referring to lookups in features

```
feature akhn {
    lookup base;
} akhn;

feature ss01 {
    featureNames { name "Stylistic Alternates: Alternative Ra"; };
    lookup stylistic.alt_ra.contextual;
    lookup stylistic.alt_ra.single;
} ss01;

feature salt {
    lookup stylistic.alt_ra.contextual;
    lookup stylistic.alt_ra.single;
} salt;

feature ss02 {
    featureNames { name "Discretionary Ligatures: Traditional Aa/Ai Sign"; };
    lookup stylistic.traditional;
} ss02;

feature dlig {
    lookup stylistic.traditional;
} dlig;
```
