# 3. Text shaping engines

A text shaping engine shapes a Unicode character sequence into a final glyph sequence, with aid of the shaping rules coded in a font. HarfBuzz (open source), DirectWrite/Uniscribe (Microsoft), Core Text (Apple), and Adobe’s unnamed engine are the four groups of OTL shaping engines that matter the most to digital type production.

> FIGURE: table of which engines the commonly encountered products use.

For OTL, the exact shaping operations executed by a shaping engine are decided jointly by the engine’s knowledge of the script and the font’s rules. In particular, OTL shaping engines try to match a text against any of the predefined, restrictive character cluster patterns, then insert dotted circles as placeholder base glyphs wherever a failure of the match leads to stray marks.

> FIGURE: example of an OTL Indic cluster pattern and a match failure.

## 3.1 Shapers, script tags, and features

How an OTL shaping engine and a specific script’s font should work together is prescribed in Microsoft’s _script development specifications_, and is implemented in shaping engines conceptually as script-specific _shapers_. An OTL font declares one or more _script tags_ to request what shaper should be used.

<!-- Language systems -->

OTL _features_ provide a mechanism for fonts to declare the purpose of a set of rules to shaping engines with predefined semantics, allowing the latter to execute specific sets of rules based on users’ implied or explicit request. For example, rules organized under the feature `liga` are understood as rules for typographically optional ligatures, and can be enabled per users’ discretion; while rules in `init` are understood as required for Arabic letters to correctly connect to each other and are always executed fro Arabic characters.

## 3.2 Special treatments for Indic scripts

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
