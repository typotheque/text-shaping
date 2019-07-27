# 1. Unicode Indic text

Before digital typography became available, written shapes in text were directly reproduced in typesetting as glyphs, without relying on underlying abstract units that are independent from specific fonts and can be exchangeable. Now with digital typography, however, exchanging underlying, digitally encoded text is both a possibility and a need.

## 1.1 Lifecycle of Unicode text

In order to separate the concern of meaningful textual units from what exact font is used, the predominant text encoding technology today, [the Unicode Standard](https://www.unicode.org), is designed with an architectural separation between abstract _characters_ and actual glyphs. So now our exchangeable text consists only of Unicode-encoded characters, while the exact look of these characters is provided by digital fonts that map characters to glyphs.

> FIGURE: Unicode’s role in the lifecycle of text: user—keyboard—encoding—display.

This abstraction is largely straightforward for scripts like Latin, for example a character U+0041 LATIN CAPITAL LETTER A is just mapped to a glyph “A”. But for so-called _complex scripts_ that are supported by Unicode with their inherent contextual interactions taken into consideration, such as Arabic and Devanagari, the abstraction leads to a contextually dynamic and thus complex mapping between characters and glyphs.

> FIGURE: comparison between Latin and Devanagari’s expected shaping behavior.

This complex mapping intended by the Unicode Standard is both meant to be the guidelines for _text representation_ (i.e., how text is _encoded_ as Unicode character sequences) and expected to be a responsibility of font technologies for _text rendering_. The process of properly mapping a character sequence to a glyph sequence is known as _shaping_.

> FIGURE: highlighted relationship between text representation and text rendering, in the lifecycle of text figure.

[OpenType Layout](https://docs.microsoft.com/en-us/typography/opentype/spec/ttochap1) (OTL) is the predominant text shaping technology today for implementing Unicode-encoded complex scripts. There are also other shaping technologies, such as [AAT](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6AATIntro.html) (Apple Advanced Typography, available in Apple products) and [Graphite](https://graphite.sil.org/) (available in LibreOffice, XeTeX, Firefox, etc.), that require different shaping rules to be coded in fonts.

## 1.2 Indic encoding principles

অসমীয়া–বাংলা Assamese–Bangla (Bengali), देवनागरी Devanagari, ગુજરાતી Gujarati, ਗੁਰਮੁਖੀ Gurmukhi, ಕನ್ನಡ Kannada, മലയാളം Malayalam, ଓଡ଼ିଆ Odia (Oriya), தமிழ் Tamil, and తెలుగు Telugu are the nine Indic (also known as Brahmic) scripts that are supported by Unicode with an encoding model based on the ISCII-88 standard (Indian Script Code for Information Interchange, 1988).

> FIGURE: Sanskrit–Devanagari alphabet.

The _Unicode ISCII_ model’s behavior exhibits strong influence of both Sanskrit and the contemporary Hindi’s Devanagari orthography. Many other Indic scripts are supported by Unicode with their encoding models more or less derived from the Unicode ISCII model, such as ខ្មែរ Khmer (Cambodian), සිංහල Sinhala (Sinhalese), and မြန်မာ Myanmar (Burmese). While scripts such as བོད་ Tibetan, ไทย Thai, and ລາວ Lao are encoded with radically different models.

Indic-specific principles of the Unicode ISCII encoding model are:

- **akshara-based complex shaping**
- **phonetic segmentation and order**
- **vowel killer doubles as non-directional conjoiner**

### 1.2.1 Akshara-based complex shaping

When analyzing an Indic text for its Unicode representation, the text is broken down to a sequence of isolated _akshara_ (a classic Indic orthographic syllable), and Indic-specific complex shaping is only expected within each akshara. Indic scripts have specific restrictions about how graphemes can be assembled together to form an akshara.

Many scripts, such as Latin and Arabic, are conventionally considered to have two basic categories of graphemes, letters (spacing and basic) and diacritics (nonspacing and modifying) in the context of digital typography. This simple categorization doesn’t work well for Indic scripts, as Indic scripts tend to have a rich set of letter-like but conceptually complex structures besides basic letters, and the modifying structures in Indic scripts can be either nonspacing or spacing and are often closely related to basic letters or letter-like complex structures as being their dependent forms.

> FIGURE: Latin letters and diacritics vs Indic bases and signs.

Therefore, for analyzing Indic scripts, their graphemes are instead categorized as _bases_ (independent structures) and _signs_ (dependent and productive structures, known as _mātrā_, _kār_, etc., in various languages). Each akshara consists of a single base and can optionally have signs applied to the base.

Fundamentally, Indic scripts work by joining (sometimes not apparent graphically, then only conceptually) a set of basic written units (basic bases and basic signs) together to form additional aksharas that can represent more complicated sounds. Graphical compositions of aksharas are ultimately obscure, but as an aid to analysis, generalized and productive signs can often be identified, thus always an akshara analytically consists of a base and optional productive signs.

> FIGURE:  
> characters <क ् ष> → simple akshara क्ष  
> characters <प ् र> → simple akshara? प्र  
> characters <त ् क> → composite akshara of a base plus a sign त्क

When a productive sign conceptually has a corresponding independent base, the sign is considered a _dependent form_ of the latter. There are various other ways of categorizing signs:

- syllabic: virama, vowel sign, consonant conjoining form, dependent sign, syllable modifier, nukta…
- visual relationship: left-side, bottom-side, top-side, right-side…
- phonetic relationship: pre-base, post-base…
- advance: spacing and nonspacing
- encoding: atomic and complex
- shaping: reordered…
- …

> FIGURE:  
> base इ → dependent {isign}  
> base त → dependent {t}  
> base र → dependent {reph} or {r} or {rasignbottom}

There can be multiple ways to analyze a specific akshara. An analysis may suggest an akshara is either only a bare base or a composite akshara of a base and signs, and how signs are identified can also differ. In a specific analysis, when all the identified productive signs have been stripped away from an akshara, the leftover is a base.

> FIGURE: alternative analyses for प्र and ट्र: base, base + ra sign vertically (works for both), half sign + alternative base horizontally (only works for प्र).

Productive signs provide extensibility to a script. Shaping and prioritizing them appropriately provide reasonable fallback patterns for edge case spellings that are out of the intended design scope and are thus not optimized specially. It’s beneficial to try to find an analytical balance between simpler base–sign composition (less contextual variation between base and sign, but more precomposed glyphs) and more productive signs (fewer glyphs, but more contextual variation between base and sign).

### 1.2.2 Phonetic segmentation and order

In order to segment and order an akshara’s components into a one-dimensional character sequence, a phonetic analysis is employed. Note that Unicode’s phonetic analysis for Indic scripts is often misinterpreted and overemphasized. The phonetic analysis is only meant to be a limited aid to the fundamental graphic analysis.

Each akshara is linearized and segmented into an ordered sequence of characters according to how Sanskrit systematically identifies the underlying phonetic segments. Phonetically, an akshara can be either /C*VC?/, a vowel optionally prefixed with a consonant cluster and optionally suffixed with one of the special consonants, or /C+/, a pure consonant cluster.

> FIGURE: graphic and phonetic anatomy of an akshara: র্স্ত্বোং.

Certain bases are encoded as a complex sequence of basic letters because they represent consonant clusters. While certain graphically composite aksharas or signs are encoded atomically because they represent simple phonetic segments, such as U+0906 आ DEVANAGARI LETTER AA, which is comparable to Latin precomposed characters such as U+00C1 Á LATIN CAPITAL LETTER A WITH ACUTE.

<!-- [Normalized shaping process with a decomposing preprocess.] -->

Note that, according to the fundamental graphic principle though, an inherent vowel is not encoded separately at all from its base consonant letter because it does not have graphic significance, while the vowel killer sign is encoded as a character despite that it actually removes a phonetic segment of the inherent vowel.

<!-- [The vowel killer sign (virama) suppresses the inherent vowel of a consonant letter, signifying a pure consonant; vowel signs instead alters the inherent vowel to a different one; while vowel modifier signs add additional phonetic value either to or after the nucleus vowel of a living syllable.] -->

<!-- [Decomposition of phonetically encoded atomic split vowel characters.] -->

<!-- [With a generalized analysis, independent vowel letters can be considered as special consonant letters that have a zero consonant and a vowel different from the default inherent one. Dependent vowel signs are these special consonant letters’ productive dependent forms.] -->

Also, the encoding order of an akshara’s components is thus decided regardless of the writing or visual order. <!-- [And special cases.] --> In a glyph sequence, conventionally, glyphs on the left side are stored before a base, while glyphs on top, bottom and right sides are all stored after a base. Certain intermediate glyphs in shaping process are then expected to be _reordered_ by the shaping engine from the originally encoded phonetic position to the expected glyph position, if the encoding order contradicts the expected glyph order.

> FIGURE:  
> akshara र्पि → characters <र ् प ि> → reordered र्पि

### 1.2.3 Vowel killer doubles as conjoiner

As Indic consonant letters are syllabic and include an inherent vowel, pure consonants are marked explicitly with a _vowel killer_ sign.

> FIGURE: a typical pure consonant, consonant-cluster akshara, and the interchangeable simplified written form of the consonant cluster.

Traditionally, vowel killers are not used inside a phonetic syllable, but in order to write fewer consonant-cluster bases, the contemporary Hindi orthography allows a vowel killer to be used inside a syllable to mark a pure consonant when writing certain consonant clusters, and such written forms are largely interchangeable with the consonant clusters’ traditional written forms.

This flexibility allows vowel killer characters, such as U+094D DEVANAGARI SIGN VIRAMA, to contextually double as the _conjoiner_ in the Unicode ISCII model. The conjoiner is non-directional, as it is not directional in terms of which side of it is conjoined. Such a character with double identity of being both a vowel killer and a conjoiner is conventionally called _virama_.

<!-- Virama is not a good explicit term (see Myanmar virama and Chakma virama). However about “killer–conjoiner”? -->

An akshara that represents a consonant cluster is thus encoded as a sequence of corresponding consonant letters connected with viramas. Contextual shaping is thus required for proper rendering. Certain written forms’ composition might seem obscure in a specific writing system, but the well-understood Sanskrit orthography often reveals their phonetic structures.

Such an encoding model largely leaves the decision of if and how to form a consonant-cluster akshara to a font’s discretion. Such flexible conjoining behavior does not actually work very well for other orthographies and scripts where an explicit vowel killer is largely not interchangeable with conjoined forms.

Also, because consonant-cluster aksharas are encoded as such conjoiner-connected consonant letters without an explicit hierarchy, shaping rules in fonts need to be prioritized appropriately to correctly recognize contextual conditions for certain conjoining forms, and to enable desired fallback behavior when the whole cluster is not captured by a single glyph in a font. This is a major source of complexity in shaping. OTL’s predefined features for Indic scripts have partly addressed this issue.

> FIGURE: characters <क ् ष> → conjoined क्ष

Note when a vowel killer character is used for conjoining consonants, the graphic role of being a conjoiner is the major rationale, while the phonetic role of being a vowel killer don’t necessarily make sense.

> FIGURE: atypical aksharas that do not conform to a typical akshara structure, or where a vowel killer does not make sense phonetically: र्ऋ · অ্যা (c.f., ক্যা) · ಉ್ೞ (c.f., ಕ್ೞು).

<!-- https://unicode.org/L2/L2015/15127-kannada-conjunct.pdf -->

## 1.3 Additional conjoining control

A general format control character, U+200D ZERO WIDTH JOINER (ZWJ [zwɪdʒ]), is used in Indic scripts specifically for assisting shaping of consonant clusters, when a desired productive conjoining form is not shaped by the default behavior. <!-- [ZWJ is used on a virama’s other side of the manipulated consonant letter. … between two consonants, immediately next to the virama on either side, so it both prevents the two consonants to form a … requesting the virama to form a productive conjoining form with the consonant not separated from the virama by the ZWJ, preventing an obscure base.] -->

> FIGURE: example of ZWJ and ZWNJ usage.

Another general format control character, U+200D ZERO WIDTH NON-JOINER (ZWNJ [zwɪndʒ]), is used immediately after a virama to ensure the virama acts only as a vowel killer, without being conjoined with any following character, thus effectively terminating an akshara.

<!-- ZWJ and ZWNJ also have some script-specific usages. -->

## 1.4 Character properties

Various character properties are defined in the Unicode Standard for clarifying a character’s identity. The `General_Category` property (`gc`), for example, makes a distinction between letters (`Lo`), nonspacing combining marks (`Mn`), and spacing combining marks (`Mc`). A pair of Indic-specific properties, `Indic_Syllabic_Category`  (`InSC`) and `Indic_Positional_Category` (`InPC`), informatively and experimentally specify a character’s intended role in the Indic syllabic structure (e.g., U+094D DEVANAGARI SIGN VIRAMA has `InSC = Virama` and `InPC = Bottom`).
