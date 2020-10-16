# 2.2 Per-script tutorial: தமிழ் Tamil

> _Work in progress._

## B.2.1 Scope of the exemplar font project

- Script: தமிழ் Tamil
    - The script suffix `..Taml` is omitted from glyph names in this tutorial.
- Orthography: contemporary தமிழ் Tamil

## B.2.2 Character-to-glyph mapping

> FIGURE: U+0B85 → அ (a)

character | | glyph | note
-- | -- | -- | --
0B83 | ஃ | aytham | SIGN VISARGA is misnomer.
0B85 | அ | a |
0B86 | ஆ | aa | confusable: a sign-uu (see sign-uu)
0B87 | இ | i |
0B88 | ஈ | ii |
0B89 | உ | u |
0B8A | ஊ | uu | borderline confusable: u lla / au-length
0B8E | எ | e |
0B8F | ஏ | ee |
0B90 | ஐ | ai |
0B92 | ஒ | o |
0B93 | ஓ | oo |
0B94 | ஔ | au | ≡ 0B92 0BD7 (o sign-au)
0B95 | க | ka |
0B99 | ங | nga |
0B9A | ச | ca |
0B9C | ஜ | ja | Grantha
0B9E | ஞ | nya |
0B9F | ட | tta |
0BA3 | ண | nna |
0BA4 | த | ta |
0BA8 | ந | na |
0BA9 | ன | nnna |
0BAA | ப | pa |
0BAE | ம | ma |
0BAF | ய | ya |
0BB0 | ர | ra |
0BB1 | ற | rra |
0BB2 | ல | la |
0BB3 | ள | lla | confusable: au-length
0BB4 | ழ | llla |
0BB5 | வ | va |
0BB6 | ஶ | sha | Grantha; marginal usage
0BB7 | ஷ | ssa | Grantha
0BB8 | ஸ | sa | Grantha
0BB9 | ஹ | ha | Grantha
0BBE | ா | sign-aa |
0BBF | ி | sign-i |
0BC0 | ீ | sign-ii |
0BC1 | ு | sign-u | phonetic; only graphically definite on Grantha bases
0BC2 | ூ | sign-uu | phonetic; only graphically definite on Grantha bases
0BC6 | ெ | sign-e |
0BC7 | ே | sign-ee |
0BC8 | ை | sign-ai |
0BCA | ொ | sign-o | ≡ 0BC6 0BBE (sign-e sign-aa)
0BCB | ோ | sign-oo | ≡ 0BC7 0BBE (sign-ee sign-aa)
0BCC | ௌ | sign-au | ≡ 0BC6 0BD7 (sign-e au-length)
0BCD | ் | virama |
0BD7 | ௗ | au-length | confusable: lla

<!-- Need to confirm usage of U+0BD0 om and U+0BF9 rupee. -->

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
lookup base.grantha_conjunct {

    sub ka virama ssa by kssa;

    sub sa virama ra sign-ii by srii;
    sub sha virama ra sign-ii by srii;

} base.grantha_conjunct;

lookup base.consonant_vowel {
    sub ka sign-i by ki;
    sub ka sign-ii by kii;
    sub ka sign-u by ku;
    sub ka sign-uu by kuu;
    sub nga sign-i by ngi;
    sub nga sign-ii by ngii;
    sub nga sign-u by ngu;
    sub nga sign-uu by nguu;
    sub ca sign-i by ci;
    sub ca sign-ii by cii;
    sub ca sign-u by cu;
    sub ca sign-uu by cuu;
    sub nya sign-i by nyi;
    sub nya sign-ii by nyii;
    sub nya sign-u by nyu;
    sub nya sign-uu by nyuu;
    sub tta sign-i by tti;
    sub tta sign-ii by ttii;
    sub tta sign-u by ttu;
    sub tta sign-uu by ttuu;
    sub nna sign-i by nni;
    sub nna sign-ii by nnii;
    sub nna sign-u by nnu;
    sub nna sign-uu by nnuu;
    sub ta sign-i by ti;
    sub ta sign-ii by tii;
    sub ta sign-u by tu;
    sub ta sign-uu by tuu;
    sub na sign-i by ni;
    sub na sign-ii by nii;
    sub na sign-u by nu;
    sub na sign-uu by nuu;
    sub pa sign-i by pi;
    sub pa sign-ii by pii;
    sub pa sign-u by pu;
    sub pa sign-uu by puu;
    sub ma sign-i by mi;
    sub ma sign-ii by mii;
    sub ma sign-u by mu;
    sub ma sign-uu by muu;
    sub ya sign-i by yi;
    sub ya sign-ii by yii;
    sub ya sign-u by yu;
    sub ya sign-uu by yuu;
    sub ra sign-i by ri;
    sub ra sign-ii by rii;
    sub ra sign-u by ru;
    sub ra sign-uu by ruu;
    sub la sign-i by li;
    sub la sign-ii by lii;
    sub la sign-u by lu;
    sub la sign-uu by luu;
    sub va sign-i by vi;
    sub va sign-ii by vii;
    sub va sign-u by vu;
    sub va sign-uu by vuu;
    sub llla sign-i by llli;
    sub llla sign-ii by lllii;
    sub llla sign-u by lllu;
    sub llla sign-uu by llluu;
    sub lla sign-i by lli;
    sub lla sign-ii by llii;
    sub lla sign-u by llu;
    sub lla sign-uu by lluu;
    sub rra sign-i by rri;
    sub rra sign-ii by rrii;
    sub rra sign-u by rru;
    sub rra sign-uu by rruu;
    sub nnna sign-i by nnni;
    sub nnna sign-ii by nnnii;
    sub nnna sign-u by nnnu;
    sub nnna sign-uu by nnnuu;
    sub ja sign-i by ji;
    sub ja sign-ii by jii;
    sub ja sign-u by ju;
    sub ja sign-uu by juu;
    sub ssa sign-i by ssi;
    sub ssa sign-ii by ssii;
    sub ssa sign-u by ssu;
    sub ssa sign-uu by ssuu;
    sub sa sign-i by si;
    sub sa sign-ii by sii;
    sub sa sign-u by su;
    sub sa sign-uu by suu;
    sub ha sign-i by hi;
    sub ha sign-ii by hii;
    sub ha sign-u by hu;
    sub ha sign-uu by huu;
    sub kssa sign-i by kssi;
    sub kssa sign-ii by kssii;
    sub kssa sign-u by kssu;
    sub kssa sign-uu by kssuu;
    sub sha sign-i by shi;
    sub sha sign-ii by shii;
    sub sha sign-u by shu;
    sub sha sign-uu by shuu;
} base.consonant_vowel;
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

    sub nna sign-aa by nnaa.traditional;
    sub rra sign-aa by rraa.traditional;
    sub nnna sign-aa by nnnaa.traditional;

    sub sign-ai nna by nnai.traditional;
    sub sign-ai la by lai.traditional;
    sub sign-ai lla by llai.traditional;
    sub sign-ai nnna by nnnai.traditional;

} stylistic.traditional;
```

### Mark positioning

> _Work in progress._

## B.2.6 Referring to lookups in features

```
feature akhn {
    lookup base.grantha_conjunct;
    lookup base.consonant_vowel;
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

feature abvm {
    lookup positioning.mark.top;
} abvm;
```
