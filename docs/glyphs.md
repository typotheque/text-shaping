# A. About glyphs

## A.1 Glyph naming for development

Systematic and informative glyph naming facilitates font development. Across this documentation, Typotheque’s in-house glyph naming scheme for Indic scripts is used, which is introduced here.

For the following scripts, all atomically encoded glyphs and additional glyphs of written units already have a recommended name in the **Glyph Knowledge Base** (section A.2):

- Used by official languages in India:
    - देवनागरी Devanagari
    - ગુજરાતી Gujarati
    - ਗੁਰਮੁਖੀ Gurmukhi
    - বাংলা Bangla
    - ଓଡ଼ିଆ Odia
    - తెలుగు Telugu
    - ಕನ್ನಡ Kannada
    - മലയാളം Malayalam
    - தமிழ் Tamil
    - ꯃꯤꯇꯩ ꯃꯌꯦꯛ Meetei Mayek
    - ᱚᱞ ᱪᱤᱠᱤ Ol Chiki
- Other:
    - සිංහල Sinhala

### A.1.1 General rules

Due to the Glyphs app’s limitation, development glyph names can only use the following characters: `A`–`Z`, `a`–`z`, `0`–`9`, period ( `.` ), underscore ( `_` ), and dash ( `-` ).

For better readability, name written units in all-lowercase by default (e.g., `ka`) . For bicameral scripts, Capitalize the glyph names of capital letters (`Alpha` vs. `alpha`). Abbreviations can retain their casing (`ZWJ`).

### A.1.2 Script namespace

For a glyph of the Latin ‹a›, the glyph name is simply:

    a

The same name is also used by the Devanagari ‹अ›, but with a **script namespace** of “Devanagari” instead of the default “Latin”. The script namespace of a glyph can be explicitly **declared** as a suffix separated from the glyph name’s main body with a dash ( `-` ), in the script’s Capitalized four-letter code from [ISO 15924, _Codes for the representation of names of scripts_](https://unicode.org/iso15924/iso15924-codes.html), regardless of OTL script tags:

    a       # a
    a-Deva  # अ, Devanagari

For the sake of brevity in non-Latin projects, a script namespace can also be **inherited** from a project-wide declaration (e.g., in a .glyphs file’s “Notes” field). Therefore, a Devanagari-dominated project can have:

    # Script namespace: Devanagari (ISO 15924: Deva)

    a-Latn  # a, Latin
    a       # अ
    ka      # क
    one     # १

Elsewhere in this documentation, an inherited script namespace of “Devanagari” is assumed by default.

In a workflow where more characters are supported in development glyph names, a colon ( `:` ) may be preferred as the separator (e.g., `a:Latn`). This may aid readability. The script code may also be declared as a prefix (`Latn:a`).

### A.1.3 Direct and phrasal names

Glyphs of many basic written units can be simply named with the established terms:

    # Devanagari:
    a       # अ
    ka      # क
    one     # १
    virama  # ्

For glyphs that are difficult to name in direct terms, use a phrase (e.g., “vocalic r”) or phonetic composition (“k + ssa”). For phrases derived from Unicode character names, stick to the original word order. For phonetic compositions, in particular ignore how the written units are encoded. Join the parts with underscores ( `_` ):

    # Devanagari:
    vocalic_r  # ऋ; U+090B DEVANAGARI LETTER VOCALIC R
    candra_a   # ॲ; U+0972 DEVANAGARI LETTER CANDRA A
    k_ssa      # क्ष; k + ssa
    r_u        # रु; r + u

Unicode character names are decent sources of direct and phrasal terms for atomically encoded glyphs. But some naming normalization can be helpful.

Use generic terms instead, if a group of glyphs apparently conform to a generic system, such as the Sanskrit alphabet:

    # Sinhala:
    a   # අ; U+0D85 SINHALA LETTER AYANNA
    ka  # ක; U+0D9A SINHALA LETTER ALPAPRAANA KAYANNA

Also, watch out misnomers:

    # Kannada:
    llla  # ೞ; U+0CDE KANNADA LETTER FA, annotated “name is a mistake for LLLA”

For a written unit or phonetic segment, there can be three levels of naming clarity:

1. Names with a modifier
    - For example: “dental/soft ta” vs. “retroflex/hard ta”, “vocalic r”, “heavy ya”.
    - This style is informative, but may look confusing when nested in phrasal names, for example, when naming a phonetic composition (e.g., `h_vocalic_r` for ‹हृ›).
2. Unicode-style names
    - For example: “ta” vs. “tta”, “aa”, “rha”.
    - Although looking unnatural, this style works well in nested phrasal names (e.g., `tt_ttha` for ‹ट्ठ›), but not so well for transcribing words.
3. Language-specific transcriptions
    - For example: “virama” for _virāma_, “prishthamatra” for _pṛṣṭhamātrā_, “tha” and “ta” for _ta_ and _ṭa_ in Tamil.
    - In order to transcribe words in basic Latin letters, certain phonetic segments are folded into one, introducing ambiguity.

### A.1.4 Naming signs

As a major feature of Indic scripts, many signs are conceptually the conjoining forms of certain bases. Such a sign is named after the same term for the base, but is marked with a preceding term “sign”, and is thus disambiguated from the corresponding base. Certain signs do not have corresponding bases. Such an orphan sign is simply named with its distinct term:

    # Devanagari:
    anusvara  # ं; orphan sign anusvara
    i         # उ; i
    sign_i    # ु; conjoining sign of i
    ra        # र; ra
    sign_ra   # ्र; conjoining sign of ra

A base may correspond to multiple conjoining signs. Each conjoining sign has a systematic name, based on its graphic (top, left, right, bottom…) and phonetic (initial or pre/post-base in onset, nucleus, coda…) relationship with an akshara’s base:

    # Devanagari:
    sign_ra_top_initial  # र्◌; conjoining sign of ra, top and initial
    sign_ra_left_pre     # र्‍; conjoining sign of ra, left and pre-base
    sign_ra_bottom_post  # ्र; conjoining sign of ra, bottom and post-base

But shorthands are usually used, which avoid verbose names, and can exhibit some shared behavior across scripts. For example, repha (conjoining sign of ra, phonetically initial in onset):

    # Devanagari:
    repha  # र्◌, (top)

    # Kannada:
    repha  # ರ್◌, (right)

    # Malayalam:
    repha  # ൎ, (top); U+0D4E MALAYALAM LETTER DOT REPH

Phonetically post-base signs are considered the default conjoining signs, and do not need to specify the graphic and phonetic information, unless there is ambiguity:

    # Devanagari:
    sign_ra  # ्र; conjoining sign of ra, (bottom and post-base)

    # Malayalam:
    sign_ya  # ്യ, conjoining sign of ya, (right and post-base)
    sign_ra  # ്ര, conjoining sign of ra, (left and post-base)
    sign_la  # ്ല, conjoining sign of la, (bottom and post-base)
    sign_va  # ്വ, conjoining sign of va, (right and post-base)

    # Gurmukhi:
    sign_ya        # ੍ਯ conjoining sign of ya, (bottom and post-base)
    sign_ya_right  # ੍ਯ, conjoining sign of ya, right (and post-base)

In particular, phonetically pre-base signs have very simple shorthands:

    # Devanagari:
    k     # क्‍, (conjoining sign of ka, left and pre-base)
    r     # र्‍, (conjoining sign of ra, left and pre-base)
    k_ss  # क्ष्‍, (conjoining sign of k_ssa, left and pre-base)
    t_t   # त्त्‍, (conjoining sign of t_ta, left and pre-base)

### A.1.5 Variation

In order to signify a variant glyph, a variation suffix can be appended right after a glyph name’s main body, separated by a period ( `.` ), and before any script namespace suffix. Variation suffixes should be informative, and avoid unnecessary abbreviation:

    # Devanagari:
    a.northern  # अ, northern variant

    # Tamil:
    lai.traditional  # லை, traditional variant

For a variant glyph that is intended to be triggered by a specific OTL feature, the feature tag may be used as the suffix if it is informative enough:

    # Devanagari:
    one.tnum  # १, tabular variant

### A.1.6 Ligation

Typographical ligatures are named by chaining each part’s glyph name main body with double dash ( `__` ):

    # Devanagari:
    t__tha      # त्थ, ligature of t and tha
    ha__sign_u  # हु, ligature of ha and sign_u

Compare with phrasally named non-ligatures:

    # Devanagari:
    t_ta  # त्त
    r_u   # रु

## A.2 The Glyph Knowledge Base

> _Work in progress._

glyph | name | character | note
-- | -- | -- | --
◌ | dottedcircle | 25CC | placeholder base
अ | a-Deva | 0905 | variant: .north
… | … | … |
