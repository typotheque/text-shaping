# 1. Unicode Indic text

Before digital typography became available, written shapes in text had been directly reproduced as glyphs in printing, without relying on some underlying abstract units that would be independent from specific fonts and could be exchangeable. Now with digital typography, however, exchanging underlying, digitally encoded text is both a possibility and a need.

## 1.1 Lifecycle of Unicode text

In order to separate the concern of meaningful textual units from the exact font that is used, [_the Unicode Standard_](https://www.unicode.org/standard/standard.html) was designed with an architectural separation between abstract, encoded **characters** and actual glyphs, and has become the predominant text encoding technology today. So now our exchangeable text consists of Unicode-encoded characters, while the exact look of these abstract characters is provided by digital fonts that map characters to glyphs.

> FIGURE: Unicode’s role in the lifecycle of text.\
> User—keyboard—encoding—display;\
> analog text and user, as well as digital editing environment and data.

This abstraction is largely straightforward for simpler scripts like Latin, for example a character U+0041 LATIN CAPITAL LETTER A is just mapped to a glyph “A”, although diacritics can be trickier to handle in digital typography. However for so-called **complex scripts**, such as Arabic and Devanagari, that are supported by the Unicode Standard with their inherent contextual interactions taken into consideration, the abstraction leads to a contextually dynamic and thus complex mapping between characters and glyphs.

> FIGURE: comparison between Latin and Devanagari’s expected shaping behavior.

This intentionally complex mapping is bidirectional. It is meant to be the guidelines for **text representation**, that is to say, how an analog text is **encoded** as an ideally unique sequence of Unicode characters. Then consequently, this mapping is expected to be a responsibility of font technologies for **text rendering** (which is about reproducing the text from its encoded form), where the crucial process of properly mapping a character sequence to a glyph sequence is known as **shaping**.

> FIGURE: highlighted relationship between text representation and text rendering, in the lifecycle of text figure.

[OpenType Layout](https://docs.microsoft.com/en-us/typography/opentype/spec/ttochap1) (OTL) is the predominant text shaping technology today for implementing Unicode-encoded complex scripts. For an introduction, see [section 2, _OTL text shaping_](2-OTL.md). There are also other shaping technologies, such as [AAT](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6AATIntro.html) (Apple Advanced Typography, available in Apple products) and [Graphite](https://graphite.sil.org/) (available in LibreOffice, XeTeX, Firefox, etc.), that require different shaping rules to be supplied by fonts.

## 1.2 Indic encoding principles

বাংলা–অসমীয়া Bangla–Asamiya (or Bengali–Assamese), देवनागरी Devanagari, ગુજરાતી Gujarati, ਗੁਰਮੁਖੀ Gurmukhi, ಕನ್ನಡ Kannada, മലയാളം Malayalam, ଓଡ଼ିଆ Odia (Oriya), தமிழ் Tamil, and తెలుగు Telugu are the nine Indic scripts (also known as Brahmic scripts) that are supported by the Unicode Standard with an encoding model based on the ISCII-88 standard (_Indian Script Code for Information Interchange_, 1988). This **Unicode ISCII** model exhibits strong influences from Sanskrit and Hindi’s Devanagari orthographies.

> FIGURE: Sanskrit–Devanagari alphabet.\
> अ आ इ ई उ ऊ\
> ऋ ॠ ऌ ॡ\
> ए ऐ ओ औ\
> अं अः\
> क ख ग घ ङ\
> च छ ज झ ञ\
> ट ठ ड ढ ण\
> त थ द ध न\
> प फ ब भ म\
> य र ल व\
> श ष स ह

Besides general principles that are shared across all of Unicode’s script-specific encoding models, the Unicode ISCII model has the following Indic-specific principles:

- **akshara-based analysis**
- **phonetic segmentation and serialization**
- **vowel killer doubles as non-directional conjoiner**

Many other Indic scripts are supported by the Unicode Standard with encoding models more or less derived from the Unicode ISCII model, such as ខ្មែរ Khmer (Cambodian), සිංහල Sinhala (Sinhalese), and မြန်မာ Myanmar (Burmese). While there are also Indic scripts such as བོད་ Tibetan and ไทย Thai that are encoded with radically different models.

### 1.2.1 Akshara-based analysis

Many simpler scripts, such as Latin and Arabic, are conventionally considered to have two basic categories of graphemes, letters (basic and spacing) and diacritics (modifying and nonspacing) in the context of digital typography. This simple categorization does not work well for Indic scripts, as Indic scripts tend to have a rich set of letter-like but conceptually composite structures besides basic letters, and the modifying structures in Indic scripts can be either nonspacing or spacing and are often productively derived from basic letters (or letter-like composite structures).

> FIGURE: Latin letters and diacritics vs Indic bases and signs.

Thus for analyzing Indic scripts, graphemes are instead categorized as **bases** (independent structures) and **signs** (dependent and productive structures, known as _mātrā_, _kār_, etc., in various languages).

As an Indic sign can pretty much appear in any side of the depended base, the apparent order of graphic units can contradict the order of phonetic segments represented by bases and signs. In order to systematically reconcile the contradicting graphic order and phonetic order and predict one from the order, when analyzing an Indic text for its Unicode representation, the text is first broken down to a sequence of isolated **akshara** (_akṣara_), the classic unit in which Indic scripts are written and analyzed. Asksharas allow Indic-specific complex encoding–shaping behaviors to be confined within a limited and predictable context.

Indic scripts work in a way that, by conceptually joining a set of basic bases and signs together, variable aksharas are dynamically formed to represent additional syllables. Graphical compositions of aksharas are ultimately obscure, but as an aid to analysis, productive signs can often be identified.
> FIGURE:\
> basic bases क _ka_ and ष _ṣa_ → simple akshara (only क्ष _kṣa_ base) क्ष _kṣa_\
> basic bases क _ka_ and र _ra_ → borderline composite akshara (only क्र _kra_ base or क _ka_ base plus र _ra_ sign?) क्र _kra_\
> basic bases त _ta_ and क _ka_ → composite akshara (त _ta_ sign plus क _ka_ base) त्क _tka_

> FIGURE:\
> base इ → sign {isign}\
> base त → sign {t}\
> base र → sign {reph} or {r} or {rasignbottom}

There can be various ways to analyze a specific akshara. One analysis may suggest an akshara is only a bare base, while some other analysese might suggest it is a composite akshara with a base and signs identified in various ways. In a specific analysis, when all the identified productive signs have been stripped away from an akshara, the leftover is a base. Analytically, an akshara always consists of a single base and optional (zero or more) signs that are applied to the base.

> FIGURE:\
> alternative analyses for प्र and ट्र: base, base + ra sign vertically (works for both), half sign + alternative base horizontally (only works for प्र).\
> alternative analyses for and evolution of ग्न/ग्य/क्य/प्त’s various styles…

Productive signs are key to the extensibility of a script. Shaping and prioritizing them appropriately in digital typography provides reasonable fallback patterns for edge-case spellings that are out of the intended design scope and are thus not optimized specially. It is beneficial to try to find an analytical balance between simpler akshara composition (less contextual variation between base and sign, but more precomposed glyphs) and more productive signs (fewer glyphs, but more contextual variation between base and sign).

There are various ways of categorizing bases:

- _syllabic role in the akshara_: vowel, consonant…
- _encoding and shaping_: atomic, complex…

And signs:

- _syllabic role in the akshara_: virama, vowel, consonant, anusvara, visarga…
- _encoding and shaping_: atomic, complex, reordered…
- _inline spacing_: spacing, nonspacing…
- _visual relation with base_: left-side, bottom-side, top-side, right-side…
- _phonetic relation with base_: initial, leading, trailing…
- _derivation from base_: orphan, conjoining form, dependent form…

### 1.2.2 Phonetic segmentation and serialization

In additional to the fundamental, akshara-bases graphic analysis of bases and signs, in order to segment and serialize an akshara into a one-dimensional character sequence, a phonetic analysis is also employed to aid the analysis. The encoding order of an akshara’s components is thus generally decided regardless of the writing or visual order.

Note the phonetic analysis’ secondary nature, as its role in Unicode’s Indic encoding models is often misinterpreted and overemphasized.

> FIGURE: mapping between graphic and phonetic anatomies of a fictional but valid akshara: র্ক্ষ্রোং.

Each akshara is analyzed according to how Sanskrit systematically identifies the underlying phonetic segments. Phonetically, a typical akshara has a structure of \[C*(V(C)?)?\], that is:

- an onset of zero or more consonant segments (i.e., a consonant cluster)
- an optional nucleus with a single vowel segment
- and when the nucleus is present, an optional coda with conceptually (although atypical) a single consonant segment

Note that the syllable represented by an akshara is rather special. It can have no nucleus, and its coda has highly limited choices, typically only anusvara (_anusvāra_, a nasal consonant or other nasal feature), visarga (_visarga_, a fricative consonant), or anunasika (_anunāsika_, nasalization of the nucleus).

Because of the fundamental graphic analysis, the inherent vowel is not encoded separately from its consonant letter as it has no graphic significance. While the vowel killer sign _is_ encoded as a character of its own, although it does not represent a phonetic segment but only surpresses the inherent vowel.

Graphically atomic written structures (bases or signs) are encoded as character sequences if they represent seuqences of phonetic segments (especially, consonant clusters), while certain graphically composite written structures (aksharas or groups of signs) are encoded atomically because they represent simple phonetic segments, such as U+0906 आ DEVANAGARI LETTER AA for _a_ (which is comparable to Latin precomposed characters such as U+00C1 Á LATIN CAPITAL LETTER A WITH ACUTE) and U+09CB ো BENGALI VOWEL SIGN O for _o_.

> FIGURE: complexly encoded atomic and atomically encoded composite.

Atomically encoded graphically-composite structures are often prone to confusable alternative encodings. Thus when no equivalent sequence is defined to address such issues (e.g., U+09CB ো BENGALI VOWEL SIGN O ≡ <U+09C7 ে BENGALI VOWEL SIGN E, U+09BE া BENGALI VOWEL SIGN AA>), the Unicode Standard tries to specify the preferred encodings and discourage those confusable alternatives.

> FIGURE: equivalent sequence and normalization, as well as “Use” vs. “Do Not Use”.

### 1.2.3 Vowel killer doubles as non-directional conjoiner

As Indic consonant letters are syllabic and include an inherent vowel, pure consonants are marked explicitly with a **vowel killer** sign.

> FIGURE: a typical pure consonant, consonant-cluster akshara, and the interchangeable simplified written form of the consonant cluster.

Traditionally, vowel killers are not used inside a phonetic syllable, but in order to write fewer consonant-cluster bases, the contemporary Hindi orthography allows a vowel killer to be used inside a syllable to mark a pure consonant when writing certain consonant clusters, and such written forms are largely interchangeable with the consonant clusters’ traditional written forms.

This flexibility allows vowel killer characters, such as U+094D DEVANAGARI SIGN VIRAMA, to contextually double as the **conjoiner** in the Unicode ISCII model. The conjoiner is non-directional, as it is not directional in terms of which side of it is conjoined. Such a character with double identity of being both a vowel killer and a conjoiner is conventionally called **virama** (_virāma_).

<!-- Virama is not a good explicit term (see Myanmar virama and Chakma virama). However about “killer–conjoiner”? -->

An akshara that represents a consonant cluster is thus encoded as a sequence of corresponding consonant letters connected with viramas. Contextual shaping is thus required for proper rendering. Certain written forms’ composition might seem obscure in a specific writing system, but the well-understood Sanskrit orthography often reveals their phonetic structures.

Such an encoding model largely leaves the decision of if and how to form a consonant-cluster akshara to a font’s discretion. Such flexible conjoining behavior does not actually work very well for other orthographies and scripts where an explicit vowel killer is largely not interchangeable with conjoined forms.

Also, because consonant-cluster aksharas are encoded as such conjoiner-connected consonant letters without an explicit hierarchy, shaping rules in fonts need to be prioritized appropriately to correctly recognize contextual conditions for certain conjoining forms, and to enable desired fallback behavior when the whole cluster is not captured by a single glyph in a font. This is a major source of complexity in shaping. OTL’s predefined features for Indic scripts have partly addressed this issue.

> FIGURE: characters <क ् ष> → conjoined क्ष

Note when a vowel killer character is used for conjoining consonants, the graphic role of being a conjoiner is the major rationale, while the phonetic role of being a vowel killer do not necessarily make sense.

> FIGURE: atypical aksharas that do not conform to a typical akshara structure, or where a vowel killer does not make sense phonetically: र्ऋ · অ্যা (c.f., ক্যা) · ಉ್ೞ (c.f., ಕ್ೞು).

<!-- https://unicode.org/L2/L2015/15127-kannada-conjunct.pdf -->

## 1.3 Additional conjoining control

A general format control character, U+200D ZERO WIDTH JOINER (ZWJ [zwɪdʒ]), is used in Indic scripts specifically for assisting shaping of consonant clusters, when a desired productive conjoining form is not shaped by the default behavior. <!-- [ZWJ is used on a virama’s other side of the manipulated consonant letter. … between two consonants, immediately next to the virama on either side, so it both prevents the two consonants to form a … requesting the virama to form a productive conjoining form with the consonant not separated from the virama by the ZWJ, preventing an obscure base.] -->

> FIGURE: example of ZWJ and ZWNJ usage.

Another general format control character, U+200D ZERO WIDTH NON-JOINER (ZWNJ [zwɪndʒ]), is used immediately after a virama to ensure the virama acts only as a vowel killer, without being conjoined with any following character, thus effectively terminating an akshara.

<!-- ZWJ and ZWNJ also have some script-specific usages. -->

## 1.4 Character properties

Various character properties are defined in the Unicode Standard for clarifying a character’s identity. The `General_Category` property (`gc`), for example, makes a distinction between letters (`Lo`), nonspacing combining marks (`Mn`), and spacing combining marks (`Mc`). A pair of Indic-specific properties, `Indic_Syllabic_Category`  (`InSC`) and `Indic_Positional_Category` (`InPC`), informatively and experimentally specify a character’s intended role in the Indic syllabic structure (e.g., U+094D DEVANAGARI SIGN VIRAMA has `InSC = Virama` and `InPC = Bottom`).
