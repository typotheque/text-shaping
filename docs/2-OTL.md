# 2. OTL text shaping

<!-- Introduce text shaping engine first or introduce required shaping rules first? -->

## 2.1 Text shaping engine

A text shaping engine shapes a Unicode character sequence into a final glyph sequence, with aid of the shaping rules coded in a font. There are four groups of OTL shaping engines that matter the most to digital type production. Platform-specific and proprietary shaping engines are available in products of Microsoft (where the shaping engine is now part of DirectWrite, and is often called “Uniscribe” for historical reasons), Apple (part of Core Text), and Adobe. HarfBuzz, on the contrary, is the de facto standard cross-platform and open source shaping engine.

> FIGURE: table of which engines the commonly encountered products use.

For OTL, the exact shaping operations executed by a shaping engine are decided jointly by the engine’s knowledge of the script and the font’s rules. In particular, OTL shaping engines try to match a text against any of the predefined, restrictive character cluster patterns, then insert dotted circles as placeholder base glyphs wherever a failure of the match leads to stray marks.

> FIGURE: example of an OTL Indic cluster pattern and a match failure.

### 2.1.1 Shapers, script tags, and features

How an OTL shaping engine and a specific script’s font should work together is prescribed in Microsoft’s _script development specifications_, and is implemented in shaping engines conceptually as script-specific _shapers_. An OTL font declares one or more _script tags_ to request what shaper should be used.

<!-- Language systems -->

OTL _features_ provide a mechanism for fonts to declare the purpose of a set of rules to shaping engines with predefined semantics, allowing the latter to execute specific sets of rules based on users’ implied or explicit request. For example, rules organized under the feature `liga` are understood as rules for typographically optional ligatures, and can be enabled per users’ discretion; while rules in `init` are understood as required for Arabic letters to correctly connect to each other and are always executed fro Arabic characters.

### 2.1.2 Special treatments for Indic scripts

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

## 2.2 Required shaping operations

Shaping prioritization:

- Normalize Unicode characters to units that matter to shaping
  * Decompose composite characters
  * Create nukta forms
- Orthographical
  - Orthographical operations to be done before reordering
    * Structures that have higher priority than productive signs
    * Productive signs
      - Features that affect reordering
      - Features that do not affect reordering
    * Structures that have lower priority than productive signs
  - Reorder
  - Orthographical operations to be done after reordering
- Typographical

Utilizing the predefined and preordered Indic features:

<!-- Format in a table parallel to the above list? -->

<!-- Can half be used for forming blwf and post forms (<virama, consonant>)? -->

<!-- Figure out a single principle for deciding which feature to use: graphic grouping or prioritization. -->

- _localized forms (GSUB)_
  * feature locl
- _basic shaping forms (GSUB)_
  * feature akhn
  * feature rphf _(reordered)_
  * feature pref _(reordered)_
  <!-- * feature blwf -->
  * feature half
  <!-- * feature post -->
  * feature cjct
- _mandatory presentation forms (GSUB)_
  * feature pres
- _positioning features (GPOS)_
  * feature dist
  * feature abvm
  * feature blwm

<!-- Such interaction is usually dealt with in `pres`, the OTL GSUB feature originally designed for _pre-base substitutions_ but can be used to handle all _presentation forms_.

with OTL glyph positioning rules in `abvm` and `blwm`, features for _above-base mark positioning_ and _below-base mark positioning_, respectively.

Complex bases are formed either in `akhn` or `cjct`, the OTL GSUB features for _akhands_ and _conjunct forms_, respectively. As `akhn` precedes the features for forming complex signs, while `cjct` is after those, where to form a specific complex base depends on its desired shaping priority and whether the complex base is expected to have its own conjoining forms.

 in `nukt`, the OTL GSUB feature for _nukta forms_. -->

### 2.2.1 Typographical

One more signs can be placed on a base, forming a composite akshara. There is usually a preference for where exactly a non-spacing sign should be placed in an akshara. The exact positioning of a non-spacing sign needs to be specified with _glyph positioning_ (GPOS) on an appropriate glyph (not necessary the base).

Multiple non-spacing signs can also be placed at the position. Instead of just stacking, they often interact graphically, and typically require OTL _glyph substitution_ (GSUB) treatments for variation and ligation.

> FIGURE:  
> base त → simple akshara त  
> base and sign <त ं> → composite akshara तं

> FIGURE: composite akshara <त े {reph} {anusvara}> → signs interacting र्तें

Both a base and a sign may have _complex_ (encoded as a character sequence) instead of _atomic_  (encoded as a single character) encoding, and would require complex shaping internally. <!-- They may also be alternative bases and alternative signs, when the boundary is not gone but either or both sides are altered productively (see Kannada consonant-vowel aksharas). -->

> FIGURE:  
> characters <क ् ष> → complex base क्ष  
> characters <र ् त> → complex sign र्त

> FIGURE: a two-by-two chart, ক র্কেং and ক্ষ র্ক্ষেং; how র্ক্ষেং is broken down to a complex base ক্ষ (different from an atomic base ক) and several productive signs, and how the whole structure is a composite akshara (different from a simple akshara ক্ষ).

<!-- [Technically, in a glyph sequence, a composite akshara may consists of multiple GDEF base glyphs and GDEF mark glyphs.] -->

<!-- [Various reordered signs, atomic or complex.] -->

### 2.2.2 Complex bases

In a specific analysis, when all the identified productive signs have been stripped away from an akshara, the leftover is a base, either _atomic_  or _complex_ . <!-- Both consonant clusters and consonant–vowel ones, eg, रु গু (also Javanese vocalicr as consonant-vowel)… --> A complex bases is formed with GSUB rules, either before forming complex signs or after, depending on desired shaping priority and whether the complex base is expected to have its own conjoining forms.

### 2.2.3 Reordered and complex signs

 <!-- (conjoining forms) -->

Complex signs, the productive signs that are encoded as a sequence, are usually conjoining forms of simple or complex consonantal bases, as vowel signs are generally encoded atomically. Conjoining signs are systematically named according to both of their graphic relation and phonetic relation with the base:

- **graphically, conjoined to the base:** _top_ (phonetically initial by default), _left_ (leading by default), _bottom_ (trailing by default), or _right_ (trailing by default)
- **phonetically, in the consonant cluster with the base:** _initial_ (the first leading consonant), _leading_, or _trailing_

Because the Unicode ISCII model does not resolve consonant-cluster shaping priorities on the encoding level, complex signs especially need to be formed in a prioritized way. Typically, conjoining forms are formed in OTL GSUB features in the following order:

- `rphf`: reph form (_top/right initial_)
- `pref`: left-side trailing form (_left trailing_)
- `blwf`: bottom-side forms (_bottom trailing_)
- `half`: half forms (_left leading_)
- `pstf`: right-side forms (_right trailing_)

When the default prioritization does not shape a desired composite akshara, a ZWJ (U+200D <span style="font-variant: all-small-caps;">ZERO WIDTH JOINER</span>) is used to override which side of a virama should absorb the virama and become a conjoining form.

If a virama is neither used for forming a complex base, nor used by either of its flanking consonants for forming a conjoining form, it does not exhibit the conjoiner behavior and is simply left as a vowel killer on the preceding consonant.

<!-- [Dependent forms are conventionally known by various terms: (dependent) vowel/consonant signs (corresponding to independent vowel/consonant letters), superscripts/subscripts, pre/above/below/post-base forms, half forms (either any dependent forms of consonant letters, or specifically the dependent forms of phonetically leading consonants letters that are written as left signs, which are common in scripts such as Devanagari), etc.] -->

A special conjoining form of the initial _ra_ that is graphically different from other (if any) leading conjoining forms, is conventionally referred to as _repha_. Repha is formed in the OTL GSUB feature `rphf` (_reph form_), and is subject to reordering in the Unicode ISCII model because it is encoded initially but its attested graphical forms (top and right) all expect a position after the base in a glyph sequence.

> FIGURE: characters <र ्> त → repha र्त

A left-side trailing form is formed in the OTL GSUB feature `pref` (_pre-base form_), and is subject to reordering.

Bottom-side forms, along with right-side forms, are the predominant consonantal dependent forms in Telugu and Kannada. They are formed in the OTL GSUB features `blwf` (_below-base forms_) and `pstf` (_post-base forms_), respectively, and are subject to vowel sign reordering in Telugu and Kannada.

Left-side leading forms (half form) are formed in the OTL GSUB feature `half` (_half forms_). Half forms are the predominant consonantal dependent forms in Devanagari and Gujarati. Both simple and complex bases can have half forms. Mainly applicable to typical bases that have a vertical stem on the right side, and the vertical stem is lost in their half forms. <!-- [Interchangeable analysis with “a base conjoined by another base below, see: प्त.] -->

<!-- [Pseudo half forms that look like dead consonants but are inside the same akshara, telling from the reordered isign and repha.] -->

> FIGURE: characters <त ्> क → half form त्क

Note the consonant letter ra has a true half form (which is also known as an _eyelash_) besides its more commonly used conjoining form for a phonetically leading/initial position, repha.

> FIGURE:  
> characters <र ्> य → repha र्य  
> characters <र ् ZWJ> य → र्‍य

<!-- [For letters that do not have a half form, virama is shown as a vowel killer. Maintain the consistency between halant forms and half forms for such letters for a smooth experience in typing.] -->

<!-- [Reordering behavior is determined by half forms: …] -->

### 2.2.4 Nukta

Nukta is not an akshara-level sign. Instead, it is placed directly on the grapheme it modifies (typically consonant letters and their dependent forms; certain innovative orthographies apply nukta to vowel letters and vowel signs also). As it is a low-level modifier and a nukta-ed grapheme is generally expected to behave like an atomic grapheme (also sometimes with behavior different from their nukta-less counterparts), nukta is generally ligated early to the modified grapheme with GSUB operations.

<!-- [Possibility for GPOS, mark on ligature, and contextual detection.] -->

> FIGURE: example of creating a nukta form.
