# Indic text shaping for type designers

_Draft for initial technical review_

梁海 (Liang Hai), independent researcher  
Peter Biľak, Typotheque

Typography occupies space at the intersection of linguistics, text rendering technologies, and design, and requires some knowledge of all of these areas. This document addresses how Indic (or Brahmic) scripts work, how the Unicode Standard encodes those scripts, what shaping behavior is expected for rendering, and how OpenType Layout is utilized to implement the shaping behavior.

To start with, the nine Unicode ISCII scripts, especially the Devanagari script, are selected to drive the initial development of this document.

### Scope and intended audience

Encoded characters for Indic scripts change shape depending on their context. This document is intended for type designers who have designed Devanagari typefaces and wish to turn them into functional fonts. It is also relevant for type users that wish to understand the expected behavior of how text shaping engines process Indic text.

> FIGURE: just an intriguing and welcoming figure.

The intention of this document is to present a tool-independent explanation of the logic and techniques of turning encoded Indic letters and diacritics into rendered text. Hopefully it will allow to understand the techniques and allow type designers to produce fonts in the tool of their choice. The documentation of expected Indic text shaping can be useful for general type users to see if the combination of fonts and text shaping engines they use delivers correct results.

This documentation doesn’t contain instructions how to draw and construct glyphs of Devanagari or other scripts. Linguistic evaluation of Hindi and other languages, such as grammar and orthography, is also outside of the scope of this project, as we look solely at understanding of text representation and rendering of written languages.

### Feedback

The authors will be happy to review feedback and suggestions how to improve this documentation, and we encourage users to [open an issue on GitHub](https://github.com/typotheque/text-shaping/issues). Due to time constrains, however, the authors are unable to help with the production of specific fonts, so if you have such questions, raise them on a public forum such as [TypeDrawers](https://typedrawers.com).

# 1. Unicode text representation and rendering

Before digital typography became available, written shapes in text were directly reproduced in typesetting as glyphs, without relying on underlying abstract units that are independent from specific fonts and can be exchangeable. Now with digital typography, however, exchanging underlying, digitally encoded text is both a possibility and a need.

> FIGURE: Unicode’s role in the lifecycle of text: user—keyboard—encoding—display.

In order to separate the concern of meaningful textual units from what exact font is used, [the Unicode Standard](https://www.unicode.org), as the predominant text encoding technology today, is designed with an architectural separation between abstract _characters_ and actual glyphs. So now our exchangeable text consists only of Unicode-encoded characters, while the exact look of these characters is provided by digital fonts that map characters to glyphs.

> FIGURE: comparison between Latin and Devanagari’s expected shaping behavior.

This abstraction is straightforward for scripts like Latin, for example a character U+0041 <span style="font-variant: all-small-caps;">LATIN CAPITAL LETTER A</span> is largely just mapped to a glyph “A”. But for so-called _complex scripts_ that are supported by Unicode with their inherent contextual interactions taken into consideration, such as Arabic and Devanagari, the abstraction leads to a contextually dynamic and thus complex mapping between characters and glyphs.

> FIGURE: highlighted relationship between text representation and text rendering, in the lifecycle of text figure.

This complex mapping intended by the Unicode Standard is both meant to be the guidelines for _text representation_ (i.e., how text is encoded in Unicode character sequences) and expected to be a responsibility of font technologies for _text rendering_. The process of properly mapping a character sequence to a glyph sequence is known as _text shaping_.

[OpenType Layout](https://docs.microsoft.com/en-us/typography/opentype/spec/ttochap1) (OTL) is the predominant text shaping technology today for implementing Unicode-encoded complex scripts. There are also other shaping technologies, such as [AAT](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6AATIntro.html) (Apple Advanced Typography, available in Apple products) and [Graphite](https://graphite.sil.org/) (available in LibreOffice, XeTeX, Firefox, etc.), that require different shaping rules to be coded in fonts.

## 1.1 Encoding principles and shaping requirements

অসমীয়া–বাংলা Assamese–Bangla (Bengali), देवनागरी Devanagari, ગુજરાતી Gujarati, ਗੁਰਮੁਖੀ Gurmukhi, ಕನ್ನಡ Kannada, മലയാളം Malayalam, ଓଡ଼ିଆ Odia (Oriya), தமிழ் Tamil, and తెలుగు Telugu are the nine Indic (also known as Brahmic) scripts that are supported by Unicode with an encoding model based on ISCII-88 (Indian Script Code for Information Interchange, 1988).

> FIGURE: Sanskrit–Devanagari alphabet.

The _Unicode ISCII_ model’s behavior exhibits strong influence of both Sanskrit and the contemporary Hindi’s Devanagari orthographies. Many other Indic scripts are supported by Unicode with their encoding models more or less derived from the Unicode ISCII model, such as සිංහල Sinhala (Sinhalese), မြန်မာ Myanmar (Burmese), ខ្មែរ Khmer (Cambodian), ᬩᬮᬶ Balinese, and ꦗꦮ Javanese. While scripts such as བོད་ Tibetan, ไทย Thai, and ລາວ Lao are encoded with radically different models.

The key characteristics of the Unicode ISCII encoding model are:

- **phonetic segmentation**, thus certain graphically composite characters
- **phonetic encoding order**, thus shaping-stage reordering of certain glyphs
- **vowel killer doubles as conjoiner**, thus shaping-stage discretion for conjuncts
- **conjoiner joins letters to form conjuncts**, thus shaping-stage prioritization of productive conjoining forms

> FIGURE: how characteristics above play roles in text representation and rendering of an akshar.

<!-- [Unicode Indic properties] -->

### Akshar

When analyzing an Indic text for its Unicode representation, the text is broken down to a sequence of _akshar_ (a classic Indic orthographic syllable), and Indic-specific complex shaping is only expected within each akshar. Each akshar consists of a single independent base and optionally have dependent signs (spacing or not) applied to the base.

In order to segment and order an akshar’s internal components into a one-dimensional character sequence, a limited phonetic analysis is employed as an aid to the fundamental graphic analysis. Note that Unicode’s phonetic analysis for Indic scripts is often misinterpreted and overemphasized.

### Phonetic segmentation, phonetic order, and reordering of akshar components

Each akshar is linearized and segmented into an ordered sequence of characters according to how Sanskrit systematically identifies the underlying phonetic segments. Phonetically, an akshar can be either /C*VC?/, a vowel optionally prefixed with a consonant cluster and optionally suffixed with one of the special consonants, or /C+/, a pure consonant cluster. <!-- [And special cases.] -->

> FIGURE: graphic and phonetic anatomy of an akshar: র্স্ত্বোং.

An inherent vowel is not encoded separately from its base consonant letter, because it does not have graphic significance.

Certain bases are encoded as a complex sequence of basic letters because they represent consonant clusters. While certain graphically composite structures are encoded atomically because they represent simple phonetic segments.

<!-- [The vowel killer sign (virama) suppresses the inherent vowel of a consonant letter, signifying a pure consonant; vowel signs instead alters the inherent vowel to a different one; while vowel modifier signs add additional phonetic value either to or after the nucleus vowel of a living syllable.] -->

<!-- [Decomposition of phonetically encoded atomic split vowel characters.] -->

<!-- [With a generalized analysis, independent vowel letters can be considered as special consonant letters that have a zero consonant and a vowel different from the default inherent one. Dependent vowel signs are these special consonant letters’ productive dependent forms.] -->

Also, the encoding order of an akshar’s component characters is thus decided regardless of the writing or visual order. <!-- [And special cases.] --> Therefore certain intermediate glyphs in shaping process are expected to be _reordered_ by the shaping engine from the originally encoded phonetic position to the graphic position as they appear.

> FIGURE: akshar पि → characters <प ि> → reordered पि

### Vowel killer, conjoiner, and conjuncts

As Indic consonant letters are syllabic, including an inherent vowel, pure consonants are marked explicitly with a _vowel killer_ sign.

A conjunct is encoded as its phonetically equivalent sequence of independent consonant letters that are to be connected with _conjoiners_. Contextual shaping is thus required for proper rendering. Certain written forms’ composition might seem obscure in a specific writing system, but the well-understood Sanskrit orthography often reveals their phonetic structures.

> FIGURE: typical pure consonant, conjunct, and the interchangeable simplified form.

Traditionally vowel killers are not used inside a phonetic syllable, but in order to write fewer conjuncts, the contemporary Hindi orthography allows a vowel killer to be used inside a syllable to mark a pure consonant when writing certain consonant clusters, and such written forms are largely interchangeable with the consonant clusters’ traditional conjunct forms.

This flexibility allows vowel killer characters, such as U+094D <span style="font-variant: all-small-caps;">DEVANAGARI SIGN VIRAMA</span> (_virama_), to contextually double as the conjoiner in the Unicode ISCII model. Such an encoding model largely leaves the decision of how to form a conjunct to a font’s discretion.

> FIGURE: characters <क ् ष> → conjoined क्ष

When used for forming conjuncts, the graphic role of being a conjoiner is the major rationale, and the phonetic role of being a vowel killer don’t necessarily make sense.

> FIGURE: conjuncts that don’t make sense phonetically: र्ऋ · অ্যা · ಉ್ೞ.

Because conjuncts are encoded as such virama-connected consonant letters without an explicit hierarchy, conjunct forming rules in fonts need to be prioritized appropriately to enable desired fallback behavior when the whole cluster is not captured by any single conjunct glyph in a font. This is a major source of complexity in shaping.

> FIGURE: example of ZWJ and ZWNJ usage.

A general format control character, U+200D <span style="font-variant: all-small-caps;">ZERO WIDTH JOINER</span> (ZWJ [zwɪdʒ]), is used in Indic specifically for assisting formation of conjuncts, when a desired productive conjoining form is not shaped by the default behavior. <!-- [ZWJ is used on a virama’s other side of the manipulated consonant letter. … between two consonants, immediately next to the virama on either side, so it both prevents the two consonants to form a … requesting the virama to form a productive conjoining form with the consonant not separated from the virama by the ZWJ, preventing an obscure conjunct.] -->

Another general format control character, U+200D <span style="font-variant: all-small-caps;">ZERO WIDTH NON-JOINER</span> (ZWNJ [zwɪndʒ]), is used immediately after a virama to ensure the virama acts only as a vowel killer, without being conjoined with any following character, thus effectively terminating an akshar.

# 2 Text shaping engines

A text shaping engine shapes a Unicode character sequence into a final glyph sequence, with aid of the shaping rules coded in a font. HarfBuzz (open source), DirectWrite/Uniscribe (Microsoft), Core Text (Apple), and Adobe’s unnamed engine are the four groups of OTL shaping engines that matter the most to digital type production.

> FIGURE: table of which engines the commonly encountered products use.

For OTL, the exact shaping operations executed by a shaping engine are decided jointly by the engine’s knowledge of the script and the font’s rules. In particular, OTL shaping engines try to match a text against any of the predefined, restrictive character cluster patterns, then insert dotted circles as placeholder base glyphs wherever a failure of the match leads to stray marks.

> FIGURE: example of an OTL Indic cluster pattern and a match failure.

## 2.1 Shapers, script tags, and features

How an OTL shaping engine and a specific script’s font should work together is prescribed in Microsoft’s _script development specifications_, and is implemented in shaping engines conceptually as script-specific _shapers_. An OTL font declares one or more _script tags_ to request what shaper should be used.

<!-- Language systems -->

OTL _features_ provide a mechanism for fonts to declare the purpose of a set of rules to shaping engines with predefined semantics, allowing the latter to execute specific sets of rules based on users’ implied or explicit request. For example, rules organized under the feature `liga` are understood as rules for typographically optional ligatures, and can be enabled per users’ discretion; while rules in `init` are understood as required for Arabic letters to correctly connect to each other and are always executed fro Arabic characters. 

## 2.2 Special treatments for Indic scripts

For Indic scripts, in particular, a large set of mandatory features are always applied by shapers, with a largely restricted order that is structured by a couple of _shaping stages_. This is to ensure Indic scripts’ required shaping behavior is always achieved. Certain features are expected to only contain rules that have a particular structure, to allow shapers to extract necessary glyph properties from a font and execute reordering properly.

The OTL shaping behavior for the nine Unicode ISCII scripts have undergone a major change, thus there are the original version (Old Shaping Behavior, sometimes referred to as _Indic 1_) and the version 2 (New Shaping Behavior, commonly referred to as _Indic 2_), requested with different tags.

script | original | version 2
-- | -- | --
Assamese–Bangla | beng | bng2
Devanagari | deva | dev2
Gujarati | gujr | gjr2
Gurmukhi | guru | gur2
Kannada | knda | knd2
Malayalam | mlym | mlm2
Odia | orya | ory2
Tamil | taml | tml2
Telugu | telu | tel2

Unlike their Indic 1 counterparts, Indic 2 shapers no longer statically assume a consonant letter to have a particular type of conjoining form. Instead, such a property is defined dynamically by fonts in certain features (rphf, pref, blwf, half, and pstf) in the _basic shaping forms_ stage and is retrieved by shapers. Consequently, the reordering process in Indic 2 shapers is divided into the _initial reordering_ before any shaping rules kick in, and the _final reordering_ after the basic shaping forms stage.

The nine Unicode ISCII scripts are specified to be subject to the following features and shaping stages:

> FIGURE: table of the nine Unicode ISCII scripts’ specified features and stages.

For some of the nine Unicode ISCII scripts, their Indic 2 shapers are well implemented in major platforms, therefore their Indic 1 tags are mostly obsolete and do not need to be supported by fonts. But some still rely on their Indic 1 shapers, thus their fonts are recommended to support both Indic 1 and Indic 2 shapers.

> FIGURE: table of products that still do not support the Indic 2 shapers.

# 2. Required interactions in shaping

Many scripts, such as Latin and Arabic, are conventionally considered to have two basic categories of graphemes, _letters_ (spacing and basic) and _diacritics_ (non-spacing and modifying) in the context of digital typography. This simple categorization doesn’t work well for Indic scripts, as Indic scripts tend to have a rich set of letter-like but conceptually complex structures besides basic letters, and the modifying structures in Indic scripts can be either non-spacing or spacing and are often closely related to basic letters or letter-like complex structures as their dependent forms.

> FIGURE: Latin letters and diacritics vs Indic bases and signs.

Therefore, for analyzing Indic scripts, their graphemes are instead categorized as _bases_ (independent structures) and _signs_ (dependent and productive structures, known as _mātrā_, _kār_, etc., in various languages).

## 2.1 Indic bases and signs

Fundamentally, Indic scripts work in a way that, through joining (sometimes not apparent graphically, then only conceptually) a set of basic written units (basic bases and basic signs) together to form composite akshars that can represent more complicated sounds.

> FIGURE: a two-by-two chart, ক র্কেং and ক্ষ র্ক্ষেং; how র্ক্ষেং is broken down to a complex base ক্ষ (different from an atomic base ক) and several productive signs, and how the whole structure is a composite akshar (different from a simple akshar ক্ষ).

Composite akshars are ultimately obscure in terms of their graphical structures, but as an aid to analysis, generalized and productive signs can often be identified in composite akshars, thus always an akshar analytically consists of a base and optional productive signs. When a productive sign conceptually has a corresponding independent structure, the sign is considered a _dependent form_ of the latter.

> FIGURE:  
> characters <क ् ष> → complex base क्ष  
> characters <प ् र> → complex base? प्र  
> characters <त ् क> → composite akshar of a base plus a sign त्क

There can be multiple ways to analyze a specific akshar. An analysis may suggest an akshar is either only an obscure base or a composite akshar of a base and signs, and how the signs are identified can differ.

> FIGURE: alternative analyses for प्र and ट्र: base, base + ra sign vertically (works for both), half sign + alternative base horizontally (only works for प्र).

<!-- [The line between a complex base and a composite akshar (in which there are productive signs identified) is not definite. There can be different analyses. In order to build a font with desired shaping behavior, one appropriate analysis needs to be selected and is implemented in the font as shaping rules.] -->

Productive signs provide extensibility to a script, therefore shaping and prioritizing them appropriately provide reasonable fallback patterns for edge case spellings that are out of the intended design scope and are thus not optimized specially. It’s beneficial to try to find an analytical balance between simpler base–sign composition (less contextual variation, but more glyphs) and more productive signs (fewer glyphs, but more contextual variation).

> FIGURE:  
> base इ → dependent {isign}  
> base त → dependent {t}  
> base र → dependent {reph} or {r} or {rasignbottom}

Various ways of sign categorization:

- syllabic: virama, vowel sign, consonant conjoining form, syllable modifier, nukta…
- visual relationship: left-side, bottom-side, top-side, right-side…
- phonetic relationship: pre-base, post-base…
- advance: spacing and non-spacing
- encoding: atomic and complex
- shaping: reordered…
- …

## 2.2 Composite akshars

One more signs can be placed on a base, forming a composite akshar. Due to the encoding principle of phonetic segmentation, some composite akshars (or part of a composite akshar, such as a split vowel sign) are encoded atomically, such as U+0906 आ <span style="font-variant: all-small-caps;">DEVANAGARI LETTER AA</span>, which is comparable to Latin precomposed characters such as U+00C1 Á <span style="font-variant: all-small-caps;">LATIN CAPITAL LETTER A WITH ACUTE</span>. <!-- [Normalized shaping process with a decomposing preprocess.] -->

> FIGURE:  
> base त → simple akshar त  
> base and sign <त ं> → composite akshar तं

Both a base and a sign may then have complex encoding, and would require complex shaping internally.

> FIGURE:  
> characters <क ् ष> → complex base क्ष  
> characters <र ् त> → complex sign र्त

<!-- [Technically, in a glyph sequence, a composite akshar may consists of multiple GDEF base glyphs and GDEF mark glyphs.] -->

### Reordering

In a glyph sequence, conventionally, glyphs on the left side are stored before a base, while glyphs on top, bottom and right sides are all stored after a base. If the encoding order contradicts the expected glyph order, the concerning glyphs generally need to be reordered. Crucial reordering operations are mostly handled by shaping engines.

> FIGURE: Reordering of repha and isign.

<!-- [Various reordered signs, atomic or complex.] -->

### Sign carrier

Akshar-level non-spacing (top or bellow) signs are usually carried by the base, but sometimes another spacing sign or a group of spacing structures jointly are preferred carrier. When the carrier has a significant width, there can often be a preference of where exactly a non-spacing sign should land. The exact positioning on a carrier is specified with OTL glyph positioning rules in `abvm` and `blwm`, features for _above-base mark positioning_ and _below-base mark positioning_, respectively.

### Sign-to-sign interaction

Multiple non-spacing signs can be placed to a carrier at the same side (above or below). Instead of just stacking, they often interact graphically. Such interaction is usually dealt with in `pres`, the OTL GSUB feature originally designed for _pre-base substitutions_ but can be used to handle all _presentation forms_.

> FIGURE: composite akshar <त े {reph} {anusvara}> → signs interacting र्तें

### Nukta

Nukta is not an akshar-level sign. Instead, it is placed directly on the grapheme it modifies (typically consonant letters and their dependent forms; certain innovative orthographies apply nukta to vowel letters and vowel signs also). As it is a low-level modifier and a nukta-ed grapheme is generally expected to behave like an atomic grapheme (also sometimes with behavior different from their nukta-less counterparts), nukta is generally ligated to the modified grapheme in `nukt`, the OTL GSUB feature for _nukta forms_.

<!-- [Possibility for GPOS, mark on ligature, and contextual detection.] -->

> FIGURE: example of creating a nukta form.

## 2.3 Complex bases

In a specific analysis, when all the identified productive signs have been stripped away from an akshar, the leftover is a base, either _atomic_ (encoded atomically as a single character) or _complex_ (encoded complexly as a character sequence). <!-- Both consonant clusters and consonant–vowel ones, eg, रु গু… -->

Complex bases are formed either in `akhn` or `cjct`, the OTL GSUB features for _akhands_ and _conjunct forms_, respectively. As `akhn` precedes the features for forming complex signs, while `cjct` is after those, where to form a specific complex base depends on its desired shaping priority and whether the complex base is expected to have its own conjoining forms.

## 2.4 Complex signs (conjoining forms)

Complex signs, the productive signs that are encoded as a sequence, are usually conjoining forms of simple or complex consonantal bases, as vowel signs are generally encoded atomically. Conjoining signs are systematically named according to both of their graphic relation and phonetic relation with the base:

- **graphically, conjoined to the base:** _top_ (initial by default), _left_ (leading by default), _bottom_ (trailing by default), or _right_ (trailing by default)
- **phonetically, in the consonant cluster with the base:** _initial_ (the first leading consonant), _leading_, or _trailing_

Because the Unicode ISCII model does not resolve conjunct shaping priorities on the encoding level, complex signs especially need to be formed in a prioritized way. Typically, conjoining forms are formed in OTL GSUB features in the following order:

- `rphf`: reph form (_top/right initial_)
- `pref`: left form (_left trailing_)
- `blwf`: bottom forms (_bottom trailing_)
- `half`: half forms (_left leading_)
- `pstf`: right forms (_right trailing_)

When the default prioritization does not shape a desired composite akshar, a ZWJ (U+200D <span style="font-variant: all-small-caps;">ZERO WIDTH JOINER</span>) is used to override which side of a virama should become a conjoining form.

If a virama is neither used for forming a complex base, nor used by either of its flanking consonants for forming a conjoining form, it does not exhibit the conjoiner behavior and is simply left as a vowel killer on the preceding consonant.

<!-- [Dependent forms are conventionally known by various terms: (dependent) vowel/consonant signs (corresponding to independent vowel/consonant letters), superscripts/subscripts, pre/above/below/post-base forms, half forms (either any dependent forms of consonant letters, or specifically the dependent forms of phonetically leading consonants letters that are written as left signs, which are common in scripts such as Devanagari), etc.] -->

### Top/right initial (repha)

A special conjoining form of the initial _ra_ that is graphically different from other (if any) leading conjoining forms, is conventionally referred to as _repha_. Repha is formed in the OTL GSUB feature `rphf` (_reph form_), and is subject to reordering in the Unicode ISCII model because it is encoded initially but its attested graphical forms (top and right) all expect a position after the base in a glyph sequence.

> FIGURE: characters <र ्> त → repha र्त

Script-specific applicability: <!-- [Is this ultimately language-specific?] -->

- **Devanagari and Gujarati:** <ra, virama> BASE → rasignabove (repha) <!-- [Or use the linguistic format “ra, virama → rasignabove / _ BASE” instead for a clearer separation of context?] -->
- **Gurmukhi:** <ra, virama, zerowidthjoiner> BASE → rasignabove (repha), _historical_
- **Assamese–Bangla:** <rabangla/raassamese, virama> BASE → rasignabove (repha)
- **Odia:** <ra, virama> BASE → rasignabove (repha)
- **Telugu:** <ra, virama, zerowidthjoiner> BASE (modern font) or <ra, virama> BASE (historical font) → rasignpostbaseinitial (repha), _historical_
- **Kannada:** <ra, virama> BASE → rasignpostbaseinitial (repha)
- **Malayalam:** <rephdot> BASE → rasignabove (repha), _traditional orthography_
- Tamil: _unattested_

### Left trailing

Formed in the OTL GSUB feature `pref` (_pre-base form_), and is subject to reordering.

Script-specific applicability:

- **Devanagari, Gujarati, Gurmukhi, Assamese–Bangla, and Odia:** _unattested_
- **Telugu and Kannada:** BASE <virama, ra> → rasignprebasetrailing, _stylistic_
- **Malayalam:** BASE <virama, ra> → rasignprebasetrailing, _simplified orthography_
- Tamil: _unattested_

### Bottom trailing

Bottom forms, along with right forms, are the predominant consonantal dependent forms in Telugu and Kannada. Formed in the OTL GSUB feature `blwf` (_below-base forms_), and is subject to reordering in Telugu and Kannada.

Script-specific applicability:

- **Devanagari and Gujarati:** _unattested_ (_rasignbelow, etc., are limited_)
- **Gurmukhi:** BASE <virama, ha/ra/va/…> → ha/ra/va/…signbelow (_additional ones are historical_)
- **Assamese–Bangla and Odia:** BASE <virama, ra/ba> → ra/basignbelow
- **Telugu and Kannada:** BASE <virama, BASE> → khasignbelow/… (khasign)
- **Malayalam:** BASE <virama, BASE> → kasignbelow/…, _traditional orthography_; BASE <virama, la> → lasignbelow, _simplified orthography (limited)_
- **Tamil:** _unattested_

### Left leading (half form)

Formed in the OTL GSUB feature `half` (_half forms_).

<!-- [Pseudo half forms that look like dead consonants but are inside the same akshar, telling from the reordered isign and repha.] -->

Half forms are the predominant consonantal dependent forms in Devanagari and Gujarati. Both simple and complex bases can have half forms. Mainly applicable to typical bases that have a vertical stem on the right side, and the vertical stem is lost in their half forms. <!-- [Interchangeable analysis with “a base conjoined by another base below, see: प्त.] -->

> FIGURE: characters <त ्> क → half form त्क

Note the consonant letter ra has a true half form (which is also known as an _eyelash_) besides its more commonly used conjoining form for a phonetically leading/initial position, _repha_.

> FIGURE:  
> characters <र ्> य → repha र्य  
> characters <र ् ZWJ> य → र्‍य

Script-specific applicability:
- **Devanagari:** <BASE, virama> BASE → kasignpre/… (k/…) and <ra, virama, zerowidthjoiner> BASE → rasignpre (r)
- **Gujarati:** <BASE, virama> BASE → kasignpre/… (k/…)
- **Gurmukhi:** <BASE, virama, zerowidthjoiner> BASE → sasignpre, _historical_
- **Assamese–Bangla, Odia, Telugu, Kannada, Malayalam, and Tamil:** unattested

<!-- [For letters that do not have a half form, virama is shown as a vowel killer. Maintain the consistency between halant forms and half forms for such letters for a smooth experience in typing.] -->

<!-- [Reordering behavior is determined by half forms: …] -->

### Right trailing

Right forms, along with bottom forms, are the predominant consonantal dependent forms in Telugu and Kannada. Formed in the OTL GSUB feature `pstf` (_post-base forms_), and is subject to reordering in Telugu and Kannada.

Script-specific applicability:

- **Devanagari and Gujarati:** _limited_
- **Gurmukhi:** BASE <virama, ya/…> → ya/…signpost _(additional ones are historical)_
- **Assamese–Bangla and Odia:** BASE <virama, ya> → yasignpost (yasign) <!-- [Note Odia’s ambiguous encoding] -->
- **Telugu and Kannada:** BASE <virama, BASE> → kasignpost/… (kasign)
- **Malayalam:** BASE <virama, ya/va> → ya/vasignpost (ya/vasign)
- **Tamil:** _unattested_

Devanagari has a semi-productive right form of Ya, which appears when there isn’t a special complex base form between a stemless consonant and Ya. This form is not shaped as a right form in OTL’s sense (i.e., in the `pstf` feature), because an OTL right form is assumed not to be a vowel sign carrier.

> FIGURE:  
> characters <त ्> य → half form त्य  
> characters ट <् य> → right conjoining form ट्य

# 3. Script- and language-specific requirements

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

# 4 Tutorial on how to build an OTL Devanagari font

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

# 5. Tips and reference for glyphs

# 5.1 Glyphs naming convention

Consistently assigned glyph names facilitate font production.

> FIGURE:  
> vocalicrsign  
> k_ssa.northern-Deva

A simple glyph name is derived from the corresponding Unicode character’s name, using the tool, glyphNameFormatter. <!-- [something about glyphsNameFormatter, where it can be found, how it works, etc] -->

An underscore ( `_` ) joins multiple glyph names to form a ligature name. Note this is syntax is meant to be used only for true ligatures that have multiple components written in a connected way. Glyphs that are only technically ligatures <!-- [what constitutes a technical ligature?]-->, such as glyphs for |ka__nukta|, may use double underscore ( `__` ) instead.

A period ( `.` ) leads a _variation suffix_, which is always after the main body of a glyph name and before the script suffix.

A dash ( `-` ) leads a _script suffix_, which clarifies a glyph’s script namespace, and is always placed at the end, after any variation suffixes. The suffix in principle should be the script’s code in ISO 15924, _Codes for the representation of names of scripts_. If a script namespace is explicitly set at the project level, that script’s suffix can be omitted.

> FIGURE:  
> virama  
> rasignbottom  
> rasignrightinitial
