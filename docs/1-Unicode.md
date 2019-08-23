# 1. Unicode Indic text

Before digital typography became available, written shapes in text had been directly reproduced as glyphs in printing, without relying on some underlying abstract units that would be independent from specific fonts and could be exchangeable. Now with **digital typography**, however, exchanging underlying, digitally encoded text is both a possibility and a need.

## 1.1 Lifecycle of Unicode text

In order to separate the concern of meaningful textual units from the exact font that is used, [**the Unicode Standard**](https://www.unicode.org/standard/standard.html) was designed with an architectural separation between encoded abstract **characters** and actual glyphs, and has become the predominant text encoding technology today. So now our exchangeable text consists of Unicode-encoded characters, while the exact look of these abstract characters is provided by digital fonts that map characters to glyphs.

> FIGURE: Unicode’s role in the lifecycle of text.\
> User—keyboard—encoding—display;\
> analog text and user, as well as digital editing environment and data.

This abstraction is largely straightforward for simpler scripts like Latin, for example a character U+0041 LATIN CAPITAL LETTER A is just mapped to a glyph “A”, although diacritics can be trickier. However for so-called **complex scripts**, such as Arabic and Devanagari, that are supported by the Unicode Standard with their inherent contextual interactions taken into consideration, the abstraction leads to a contextually dynamic and thus complex mapping between characters and glyphs.

> FIGURE: comparison between Latin and Devanagari’s expected shaping behavior.

This intentionally complex mapping is bidirectional. It is meant to be the guidelines for **text representation**, that is to say, how an analog text is analyzed to have an ideally unique **encoding** of Unicode character sequence. Consequently, this mapping is expected to be a responsibility of font technologies for **text rendering** (which is about reproducing the text’s appearance from its encoded form), where the crucial process of contextually mapping a character sequence to a glyph sequence is known as **shaping**.

> FIGURE: highlighted relationship between text representation and text rendering, in the lifecycle of text figure.

[**OpenType Layout**](https://docs.microsoft.com/en-us/typography/opentype/spec/ttochap1) (OTL) is the predominant text shaping technology today for implementing Unicode-encoded complex scripts. For an introduction, see [section 2, _OTL text shaping_](2-OTL.md). There are also other shaping technologies, such as [AAT](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6AATIntro.html) (Apple Advanced Typography, available in Apple products) and [Graphite](https://graphite.sil.org/) (available in LibreOffice, XeTeX, Firefox, etc.), that require different shaping rules to be supplied by fonts.

## 1.2 Indic encoding principles

বাংলা–অসমীয়া Bangla–Asamiya (or Bengali–Assamese), देवनागरी Devanagari, ગુજરાતી Gujarati, ਗੁਰਮੁਖੀ Gurmukhi, ಕನ್ನಡ Kannada, മലയാളം Malayalam, ଓଡ଼ିଆ Odia (Oriya), தமிழ் Tamil, and తెలుగు Telugu are the nine **Indic scripts** (also known as Brahmic scripts) that are supported by the Unicode Standard with an encoding model based on the ISCII-88 standard (_Indian Script Code for Information Interchange_, 1988). This **Unicode ISCII** model exhibits strong influences from Sanskrit and Hindi’s Devanagari orthographies.

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
- **inherent vowel killer doubles as conjoiner**

Many other Indic scripts are supported by the Unicode Standard with encoding models more or less derived from the Unicode ISCII model, such as ខ្មែរ Khmer (Cambodian), සිංහල Sinhala (Sinhalese), and မြန်မာ Myanmar (Burmese). While there are also Indic scripts such as བོད་ Tibetan and ไทย Thai that are encoded with radically different models.

### 1.2.1 Akshara-based analysis

Many simpler scripts, such as Latin and Arabic, are conventionally considered to have two major categories of graphemes, letters (basic and spacing) and diacritics (modifying and nonspacing) in the context of digital typography. This simple categorization does not work well for Indic scripts, as Indic scripts tend to have a rich set of letter-like but conceptually **composite** structures besides basic letters, and the modifying structures in Indic scripts can be either nonspacing or spacing and are often derived from basic letters (or letter-like composite structures).

> FIGURE: Latin letters and diacritics vs Indic bases and signs.

Thus for analyzing Indic scripts, graphemes are instead categorized as **bases** (independent structures) and **signs** (dependent and **productive** structures, known as _mātrā_, _kār(a)_, etc., in various languages).

As an Indic sign can pretty much appear on any side of the depended base, the apparent order of graphic units can contradict the order of sounds represented by bases and signs. In order to systematically reconcile the contradicting graphic order and phonetic order and predict one from the order, when analyzing an Indic text for its Unicode representation, the text is first broken down to a sequence of isolated **akshara** (_akṣara_), the classic unit in which Indic scripts are written and analyzed. Aksharas allow Indic-specific complex encoding–shaping behaviors to be confined within a limited and predictable context.

Indic scripts work in a way that, by conceptually joining a set of simple bases and signs together, variable aksharas are dynamically formed to represent additional syllables. Graphical compositions of aksharas are ultimately **obscure**, but as an aid to analysis, productive signs can often be identified.

> FIGURE:\
> simple bases क _ka_ and ष _ṣa_ → simple akshara (only क्ष _kṣa_ base) क्ष _kṣa_\
> simple bases क _ka_ and र _ra_ → borderline composite akshara (only क्र _kra_ base or क _ka_ base plus र _ra_ sign?) क्र _kra_\
> simple bases त _ta_ and क _ka_ → composite akshara (त _ta_ sign plus क _ka_ base) त्क _tka_

> FIGURE:\
> base इ → sign {isign}\
> base त → sign {t}\
> base र → sign {reph} or {r} or {rasignbottom}

There can be various ways to analyze a specific akshara. One analysis may suggest an akshara is **atomic**, with only a bare base, while some other analyses might suggest it is **composite**, with a base and signs identified in various ways. In a specific analysis, when all the identified productive signs have been stripped away from an akshara, the leftover is a base. Analytically, an akshara always consists of a single base and optional (zero or more) signs that are applied to the base.

> FIGURE:\
> alternative analyses for प्र and ट्र: base, base + ra sign vertically (works for both), half sign + alternative base horizontally (only works for प्र).\
> alternative analyses for and evolution of ग्न/ग्य/क्य/प्त’s various styles…

Productive signs are key to the extensibility of a script. Shaping and **prioritizing** them appropriately in digital typography provides reasonable **fallback** patterns for edge-case spellings that are out of the intended design scope and are thus not optimized specially. It is beneficial to try to find an analytical balance between simpler akshara composition (less contextual variation between base and sign, but more precomposed glyphs) and more productive signs (fewer glyphs, but more contextual variation between base and sign).

There are various ways of categorizing bases:

- _syllabic role in the akshara_: onset, simple consonant, consonant cluster, nucleus, vowel…
- _encoding and shaping_: atomic, complex…

And signs:

- _syllabic role in the akshara_: onset, simple consonant, consonant cluster, virama, nucleus, vowel, coda, anusvara, visarga, anunasika…
- _encoding and shaping_: atomic, complex, reordered…
- _inline spacing_: spacing, nonspacing…
- _visual relation with base_: left-side, bottom-side, top-side, right-side…
- _phonetic relation with base_: initial, leading, trailing…
- _derivation from base_: orphan, conjoining form, dependent form…

### 1.2.2 Phonetic segmentation and serialization

In additional to the fundamental, akshara-based graphic analysis of bases and signs, in order to **segment** and **serialize** an akshara into a one-dimensional character sequence, a phonetic analysis is also employed to aid the analysis. The encoding order of an akshara’s components thus often contradicts graphic and writing orders. However, note the phonetic analysis’ secondary nature, as its role in Unicode’s Indic encoding models is often misinterpreted and overemphasized.

> FIGURE: mapping between graphic and phonetic anatomies of a fictional but valid akshara: র্ক্ষ্রোং.

Each akshara is analyzed according to how Sanskrit systematically identifies the underlying phonetic segments. Phonetically, a typical akshara has a structure of `C* (V C?)?`, that is:

- an optional **onset**: a single (a **simple consonant**) or more consonant segments (a **consonant cluster**)
- an optional **nucleus**: a single vowel segment
    - if the nucleus is present, an optional **coda**: conceptually a single consonant segment

The syllable represented by an akshara is special. It can be onset-only, and its coda has highly limited choices, typically only **anusvara** (_anusvāra_, a nasal consonant or other nasal feature), **visarga** (_visarga_, a fricative consonant), or **anunasika** (_anunāsika_, nasalization of the nucleus).

Because of the fundamental graphic analysis, the inherent vowel is not encoded separately from its consonant letter as it has no graphic significance. On the other hand, the inherent vowel killer sign is encoded as a character of its own, although it does not represent a phonetic segment but only suppresses the inherent vowel.

Graphically atomic written structures (bases or signs) are **sequentially encoded** (as character sequences) if they represent sequences of phonetic segments (in particular, consonant clusters), while certain graphically composite written structures (aksharas or groups of signs) are **atomically encoded** because they represent simple phonetic segments. Also, atomically encoded graphically composite structures are often prone to confusable alternative encodings. Thus when no equivalent sequence is defined to address such issues, the Unicode Standard tries to specify the preferred encodings and discourage those confusable alternatives.

> FIGURE:\
> Complexly encoded atomic and atomically encoded composite. Equivalent sequence and normalization, as well as “Use” vs. “Do Not Use”.\
> U+0906 आ DEVANAGARI LETTER AA, _a_, (c.f., U+00C1 Á LATIN CAPITAL LETTER A WITH ACUTE), no decomposition.\
> U+09CB ো BENGALI VOWEL SIGN O, _o_, ≡ <U+09C7 ে BENGALI VOWEL SIGN E, U+09BE া BENGALI VOWEL SIGN AA>.

### 1.2.3 Inherent vowel killer doubles as conjoiner

Traditionally, an onset can only be written as either one of the simple consonant bases (if the onset is a simple consonant), or a complex consonant base that is formed by joining simple consonant bases and consonant signs (if is a consonant cluster). As such onset bases imply a nucleus of the **inherent vowel**, onset-only syllables are marked explicitly with a sign that suppresses the inherent vowel, which is conventionally known as an inherent vowel **killer** (known as _virāma_, _halant(a)_, _puḷḷi_, _asat_, etc., in various languages).

In the contemporary Hindi orthography however, in order to use fewer distinct onset bases, certain consonant clusters can have their leading consonants written separately as onset-only aksharas that are marked with a killer. Such simplified written forms are largely interchangeable with those consonant clusters’ traditional written forms.

> FIGURE: a typical pure consonant, consonant cluster akshara, and the interchangeable simplified written form of the consonant cluster.

This flexibility allows characters of killer signs, such as U+094D ् DEVANAGARI SIGN VIRAMA, to contextually double as an artificial **conjoiner** in the Unicode ISCII model, for dynamically conjoining atomically encoded bases into complex bases. The conjoiner is **non-directional**, as it is not predefined which side of it is the base to be conjoined. Because of their Unicode character names, the term **virama** (originated from the killer sign’s Sanskrit name _virāma_) is often used to refer specifically to such characters that have a killer–conjoiner double identity.

A consonant cluster onset is therefore encoded as a sequence of virama-connected simple consonant bases that correspond to the consonant cluster’s component segments. Certain bases might seem phonetically obscure in a specific orthography, but the well-understood Sanskrit orthography often reveals their phonetic components.

> FIGURE: obscure base क्ष → characters <क ् ष> → conjoined base क्ष

Such an encoding model largely leaves the decision of if and how to form a consonant cluster akshara to fonts’ discretion. Note that this flexible conjoining behavior does not actually work very well for other orthographies and scripts, where conjoining is largely not interchangeable with a visible killer.

Because there is no explicit hierarchy in conjoiner-connected base sequences, shaping rules in fonts need to be meticulously prioritized in order to correctly recognize contextual conditions for certain conjoining forms, and to enable appropriate fallback behavior when the whole sequence is not captured by a single precomposed glyph. This is a major source of complexity in shaping. OTL’s predefined features (and their recommended order) for Indic scripts are meant to partly address this issue.

Note that when a killer character is used for conjoining bases, the role of being a conjoiner (per the graphic analysis) is the major rationale, while the role of being a killer (per the phonetic analysis) sometimes just does not make sense in a phonetically atypical akshara.

> FIGURE: atypical aksharas where a killer does not make sense phonetic: र्ऋ (c.f., र्क) · অ্যা (c.f., ক্যা) · ಉ್ೞ (c.f., ಕ್ೞು).

## 1.3 Additional conjoining control

Besides each script’s own conjoiner, a pair of general **format control** characters have special behavior defined for the Unicode ISCII model (except the Malayalam script), primarily used immediately next to a virama character for overriding the default conjoiner behavior.

> FIGURE: example of ZWJ and ZWNJ usage.

U+200D ZERO WIDTH JOINER (**ZWJ**, pronounced as [zwɪdʒ]) is used on either side of a virama to request the base (which is not necessarily atomically encoded) on the virama character’s other side to fully absorb the virama and turn to its productive conjoining form, regardless of what is on the same side.

- The base on the same side, if any, is screened by the ZWJ from the virama’s effect. Therefore, when there are bases on both sides, the ZWJ effectively prevents the two bases from conjoining into a single base and requests a base plus sign composition.

- It is helpful to understand ZWJ as an invisible and unspecified Indic base, that enables virama’s conjoining behavior but does not provide any information about how the other side’s base should be conjoined exactly, thus that base turns to its default, productive conjoining form.

U+200C ZERO WIDTH NON-JOINER (**ZWNJ** [zwɪndʒ]), on the contrary, is only used after a virama to prevent its conjoiner behavior, leaving it to be a killer. The ZWNJ effectively terminates an akshara, causing the preceding akshara to be onset-only.

ZWJ and ZWNJ also have script-specific behaviors defined for Malayalam, Bangla–Asamiya, and Odia.

## 1.4 Character properties

Various character properties are defined in the Unicode Standard for clarifying a character’s identity and behavior. The **`General_Category`** property (commonly referred to with its short name, `gc`), for example, makes a distinction between letters (`Lo`), nonspacing combining marks (`Mn`), and spacing combining marks (`Mc`), among other values. A pair of Indic-specific properties, **`Indic_Syllabic_Category`** (`InSC`) and **`Indic_Positional_Category`** (`InPC`), as quite an ongoing experiment, informatively specify a character’s intended role in an akshara. For example, U+094D DEVANAGARI SIGN VIRAMA has `InSC = Virama` and `InPC = Bottom`.
