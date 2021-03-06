# B. Recommended glyph naming

Systematic and informative glyph naming facilitates design and development. Typotheque’s glyph naming scheme for Indic scripts is introduced in this appendix, and is used across the documentation project.

For the following scripts, the **Database of Registered Glyphs** (to be made availabale later) provides recommended names for all of their common glyphs, including all the atomically encoded glyphs: देवनागरी Devanagari, ગુજરાતી Gujarati, ਗੁਰਮੁਖੀ Gurmukhi, বাংলা Bangla, ଓଡ଼ିଆ Odia, తెలుగు Telugu, ಕನ್ನಡ Kannada, മലയാളം Malayalam, தமிழ் Tamil, ꯃꯤꯇꯩ ꯃꯌꯦꯛ Meetei Mayek, ᱚᱞ ᱪᱤᱠᱤ Ol Chiki, and සිංහල Sinhala.

## Quick reference

There are four general syntactic characters and some alternative formats:

- **Word separator:** dash (-), or **camelCasing**

    * Each typographically non-decomposable (i.e., orthographically unique) component is named with either a single all-lowercase word, or a phrase joined by dashes: इ **i**, ि **sign<mark>-</mark>i** (dependent sign _i_), ह **ha**, ह्र **h<mark>-</mark>ra** (_hra_), हृ **h<mark>-</mark>vocalic<mark>-</mark>r** (_hṛ_)
    * When dashes are undesirable or not allowed (such as in production names), use the equivalent camelCase format: **i**, **signI**, **ha**, **hRa**, **hVocalicR**

- **Component separator:** underscore (_)

    * Inside a typographical ligature, components are joined by underscores: म्ह **m<mark>\_</mark>ha** (ligation between म्‍ m and ह ha), हु **ha<mark>\_</mark>sign-u** (ligation between ह ha and ु sign-u)

- **Variation suffix separator:** period (.xxxx)

    * A typographical variant is denoted with a period-seprated suffix: रि **sign-i<mark>.short</mark>** (followed by base ra), की **sign-ii<mark>.long</mark>** (preceded by base ka)

- **Script prefix separator:** colon (xxxx:), or dash for **suffixing** (-xxxx)

    * Formally, the Unicode script a glyph belongs to is denoted by an unambiguous, colon-seprated prefix of the lowercased Unicode script code: क **<mark>deva:</mark>ka**, க **<mark>taml:</mark>ka**, র্চি **<mark>beng:</mark>sign-i_repha.short** (followed by base beng:ca)
    * When a colon-separated prefix is undesirable or not allowed (such as in the Glyphs app), attach a dash-separated suffix to the last underscore-separated component instead: **ka<mark>-deva</mark>**, **ka<mark>-taml</mark>**, **sign-i_repha<mark>-beng</mark>.short**
    * Script prefixes/suffixes can often be ommitted in casual discussions when the script is clear.

Each script has its own shorthands, and thus one needs to refer to the database of registered glyphs for recommended names. Some commonly used shorthand solutions are outlined below:

- Conjoining signs: ਕ੍ਰ guru:sign-ra (preceded by base ka), క్ర telu:sign-ra, ക്ര mlym:sign-ra
- Pure consonant bases and signs whose phonetic names are unambiguous within the script: क्‍ deva:k, క్ telu:k, ൿ mlym:k
- Nukta-ed components: फ़ phxa, फ़्र phx-ra, फ़्‍ phx, फ़्र्‍ phx-r

## Principles

- Typographical decomposition (which is the basis of glyph naming) is separate from shaping productivity. `ya-post` may be considered productive or not in shaping, but `tt-ya` is apparently typographically decomposable into `<tta, ya-post>`.

- Anything that makes an orthographical difference should be named as a unique entity that is free of typographical composing (dot-suffixing and underscore-ligation). Then the existence of dot-suffixing and/or underscore-ligation can mark the interface between orthography and typography.

- …

By default the glyph names we talk about are the so called **development glyph names**, which may be different from **production glyph names**. The recommendation about production names will be made available later. For Indic scripts, because of their complex shaping, a PDF needs to be tagged (e.g., with `\ActualText`) anyway in order to enable text extraction, which makes it largely unnecessary to supply production names that are compliant with the [Adobe Glyph List Specification](https://github.com/adobe-type-tools/agl-specification).

## A.1 Structure

A glyph name is up to 63 characters in length, consisting of only characters `A`–`Z`, `a`–`z`, `0`–`9`, period ( `.` ), underscore ( `_` ), and dash ( `-` ). Dash is only valid in development names. No glyph names can start with a digit or period, except `.notdef`. Although the [OpenType Feature File Specification](https://adobe-type-tools.github.io/afdko/OpenTypeFeatureFileSpecification.html#2.f.i) has cleared seven development-name–only characters, we use only dash due to the [Glyphs app](http://glyphsapp.com)’s restriction.

A **plain name** is followed by a **script tag** that specifies the script namespace.

Regardless of the script’s OTL script tag(s), the suffix uses the script’s Capitalized four-letter code from [ISO 15924](https://unicode.org/iso15924/iso15924-codes.html), _Codes for the representation of names of scripts_. The tag is separated from the plain name by a colon. For how to omit the script tag by specifying an implied script namespace, see section A.3, _Shorthands and alternatives_.

    Latn:a  # ‹a› glyph “a”, Latin script
    Deva:a  # ‹अ› glyph “a”, Devanagari script

For the sake of readability, plain names are in all-lowercase by default. Bicameral scripts’ capital letters get Capitalized plain names. When a plain name has a composite structure, its constituting parts (e.g., constituting words of a phrase) are joined by dashes.

Atomically encoded glyph typically get a name derived from its Unicode character’s name. The Glyph Database provides recommended forms of these **Unicode-derived names** for all the atomically encoded glyphs, and may incorporate [LettError/glyphNameFormatter](https://github.com/LettError/glyphNameFormatter) in the future. For some behind-the-scenes explanation of the strategies used when deriving those recommended names, see section A.4, _Transforming Unicode character names_.

    Deva:vocalic-r  # ‹ऋ› “vocalic r”, Devanagari; U+09DB DEVANAGARI LETTER VOCALIC R
    Deva:candra-a   # ‹ॲ› “candra a”, Devanagari; U+0972 DEVANAGARI LETTER CANDRA A
    Deva:sign-aa    # ‹…› “sign aa”, Devanagari; U+093E DEVANAGARI VOWEL SIGN AA

For glyphs that are not atomically encoded, **systematic names** are generated with elements taken from Unicode-derived names. Note this secondary derivation is not about treating non-atomically-encoded glyphs as second-class citizens, and does not even have much to do with the encoding model. It is only for the sake of reliable naming, as Unicode characters have standardized and stable names. For the rules, see section A.2, _Systematic names_.

    Deva:k-ssa    # ‹क्ष› “k-ssa” (kṣa), Devanagari
    Deva:sign-ra  # ‹…› conjoining sign of “ra” (graphically bottom, phonetically post-base), Devanagari

One or more **variant suffixes** may follow the plain name. Each variant suffix is led by a period. Variant suffixes should be informative, and avoid unnecessary abbreviation.

    Deva:five.northern          # ‹…› “five”, northern variant, Devanagari
    Deva:five.northern.tabular  # ‹…› “five”, tabular variant of northern variant, Devanagari

If the glyph is a **typographic ligature**, join its components’ plain names with dashes.

    Deva:repha_anusvara         # ‹…› ligature of “repha” and “anusvara”, Devanagari
    Deva:repha_anusvara.narrow  # ‹…› ligature of “repha” and “anusvara”, narrow variant, Devanagari

## A.2 Systematic names

According to our [graphic analysis](graphic-analysis.md) of a script’s composition, name a graphically obscure base according to its phonetic segments, and name a conjoining sign according to its base.

With **phonetic segmentation**, split all phonetic segments (except the inherent vowel) of a glyph, transcribe them according to the phonetically corresponding base glyphs, then join them with dashes. Note that how a glyph is encoded is irrelevant to this naming strategy.

    Deva:k-ssa        # ‹क्ष› “k ssa” (kṣa), Devanagari
    Deva:r-u          # ‹रु› “r u” (ru), Devanagari
    Deva:h-vocalic-r  # ‹हृ› “h vocalic-r” (hr̥), Devanagari

For a **conjoining sign**, prefix its corresponding base’s name with `sign` so it is distinguished from the base, then suffix with tags for its graphic (`top`/`left`/`bottom`/`right`) and phonetic relationship (`initial`/`pre`/`post`/…) with its graphic base.

    Deva:ra                   # ‹र› “ra”, Devanagari
    Deva:sign-ra-top-initial  # ‹…› conjoining sign of “ra”, graphically top, phonetically initial, Devanagari
    Deva:sign-ra-left-pre     # ‹र्‍› conjoining sign of “ra”, graphically left, phonetically pre-base, Devanagari
    Deva:sign-ra-bottom-post  # ‹…› conjoining sign of “ra”, graphically bottom, phonetically post-base, Devanagari

In reality though, such cumbersome systematic names are seldom recommended and are only reserved as a fallback mechanism. Instead, most conjoining signs are named in **shorthands**. For an introduction, see section A.3, _Shorthands and alternatives_.

    Deva:repha    # ‹…› “repha”, Devanagari
    Deva:r        # ‹र्‍› “r”, Devanagari
    Deva:sign-ra  # ‹…› conjoining sign of “ra”, Devanagari

## A.3 Shorthands and alternatives

### Shorthands for systematic names

When there is no ambiguity inside a script, the dashes in phonetic segmentation (only in phonetic segmentation, not elsewhere) may be systematically omitted.

    Taml:ku    # ‹கு› “ku” (ku) instead of “k u”, Tamil
    Taml:kssa  # ‹க்ஷ› “kssa” (kṣa) instead of ”k ssa”, Tamil
    Taml:srii  # ‹ஸ்ரீ› “srii” (srī) instead of “s r ii”, Tamil

Phonetically pre-base signs, coda signs, and letters with zeroed nucleus can be simply named after the pure consonants, as long as they form a series and there is no ambiguity inside the script.

    Deva:k     # ‹क्‍› implied phonetically pre-base conjoining sign of “ka”, Devanagari
    Deva:r     # ‹र्‍› implied phonetically pre-base conjoining sign of “ra”, Devanagari
    Deva:k-ss  # ‹क्ष्‍› implied phonetically pre-base conjoining sign of “k-ssa”, Devanagari

    # Malayalam
    Mlym:nn  # ‹ൺ› implied chillu of “nna”, Malayalam
    Mlym:n   # ‹ൻ› implied chillu of “na”, Malayalam
    Mlym:r   # ‹ർ› implied chillu of “ra”, Malayalam

    # Kannada
    Knda:k   # ‹ಕ್› implied zero-nucleus “ka”, Kannada
    Knda:kh  # ‹ಖ್› implied zero-nucleus “kha”, Kannada
    Knda:g   # ‹ಗ್› implied zero-nucleus “ga”, Kannada

Shorthands are generally recommended for conjoining signs. They avoid verbose names, and can exhibit some shared behavior across scripts. For example, repha (conjoining sign of ra, phonetically initial):

    Deva:repha  # ‹…› “repha”, (graphically top), Devanagari
    Knda:repha  # ‹…› “repha”, (graphically right), Kannada
    Mlym:repha  # ‹ൎ› “repha”, (graphically top), Malayalam; U+0D4E MALAYALAM LETTER DOT REPH

Phonetically post-base signs are considered the default conjoining signs, and thus do not need to specify the phonetic relationship. The tags for graphic relationship may be omitted when either there is no ambiguity inside a script or a sign’s graphic relationship with the base is consistent with the predominant one of the script.

    Deva:sign-ra  # ‹…› implied phonetically post-base conjoining sign of “ra”, (graphically bottom), Devanagari

    Mlym:sign-ya  # ‹…› implied phonetically post-base conjoining sign of “ya”, (graphically right), Malayalam
    Mlym:sign-ra  # ‹…› implied phonetically post-base conjoining sign of “ra”, (graphically left), Malayalam
    Mlym:sign-la  # ‹…› implied phonetically post-base conjoining sign of “la”, (graphically bottom), Malayalam
    Mlym:sign-va  # ‹…› implied phonetically post-base conjoining sign of “va”, (graphically right), Malayalam

    Guru:sign-ya        # ‹…› implied phonetically post-base conjoining sign of “ya”, (graphically bottom), Gurmukhi
    Guru:sign-ya-right  # ‹…› implied phonetically post-base conjoining sign of “ya”, graphically right, Gurmukhi

### OTL feature tags as variant suffixes

For a variant glyph that is intended specifically for an OTL feature, the feature’s four-letter tag may be used as the suffix, if it is clear enough from the tag alone what the glyph should look like. Tags like `dlig`, `salt`, and `ss01` are examples that do not meet the recommendation.

    Deva:one.tnum     # ‹१› “one”, variant for feature “tnum” (Tabular Figures), Devanagari
    Beng:sign-e.init  # ‹…› conjoining sign of “e”, variant for feature “init” (Initial Forms), Bangla

### Alternative ways of declaring a script namespace

A script namespace may be inherited from a project-wide declaration (e.g., in a .glyphs file’s “Notes” field). For example, a Devanagari-dominated project can have:

    # Script namespace: Devanagari (ISO 15924 code: Deva)

    a          # ‹अ› implied Devanagari “a”
    ka         # ‹क› implied Devanagari “ka”
    one        # ‹१› implied Devanagari “one”
    Latn:a    # ‹a› “a”, Latin
    Latn:one  # ‹1› “a”, Latin

Implied script namespace, however, makes copying glyphs from one project to another a slightly complicated task, as renaming is often necessary.

In a workflow where more characters are supported in development names, a colon ( `:` ) may be preferred to lead the script namespace suffix (e.g., `a:Deva`). This may aid readability. With a colon, the script code may also be declared as a prefix (`Deva:a`), although in practice a suffixed script code makes it easier to search for glyphs.

## A.4 Transforming Unicode character names

Unicode character names are decent starting points for glyph names, but in order to derive helpful glyph names, they do require some systematic transformation, as well as some overriding. This is an FYI section about the strategies and considerations behind our recommended Unicode-derived glyph names.

Generally, the following unnecessary attributive words are first stripped away from a Unicode character name:

- the script name, for example, `DEVANAGARI`
- general categorization terms, such as `LETTER`, `DIGIT`, `NUMBER`, `FRACTION`, or `PUNCTUATION`
- phonetic categorization terms, `VOWEL` or `CONSONANT`

Also stripped away is `SIGN` or `MARK` when it is not used for disambiguation, especially when the encoded glyph is not considered a base’s conjoining sign (i.e., an orphan sign). For example, simply `Deva:virama` for `U+094D DEVANAGARI SIGN VIRAMA`, but `Deva:sign-aa` for `U+093E DEVANAGARI VOWEL SIGN AA` because there is also `Deva:aa` for `U+0906 DEVANAGARI LETTER AA`.

Then, transform the leftover words to all-lowercase, then replace all word spaces by dashes. Do not reorder words to make related glyphs alphabetically sorted together. Stick to the original word order. Glyph orders should be maintained by sort keys.

Certain **overriding** can be helpful. Scripts like Sinhala and Meetei Mayek use native terms in character names, with the Sanskrit-conforming generic names annotated. If a group of glyphs apparently conform to a generic system, such as the Sanskrit alphabet or the decimal digits, the generic terms should be used instead. Certain Unicode character names can also be inappropriate or even simply misnomers, and thus should be corrected in glyph names.

    Sinh:ka      # ‹ක› “ka”, Sinhala; U+0D9A SINHALA LETTER ALPAPRAANA KAYANNA, annotated with alternative name “sinhala letter ka”
    Mtei:ka      # ‹ꯀ› “ka”, Meetei Mayek; U+ABC0 MEETEI MAYEK LETTER KOK, annotated with alternative name “ka”
    svarita       # ‹…› “svarita“, general script; U+0951 DEVANAGARI STRESS SIGN UDATTA, annotated with alternative name “Vedic tone svarita” and informative note “mostly used for svarita, with rare use for udatta”
    Knda:llla    # ‹ೞ› “llla”, Kannada; U+0CDE KANNADA LETTER FA, annotated with normative alias “KANNADA LETTER LLLA” and informative note “name is a mistake for LLLA”
    Taml:aytham  # ‹ஃ› “aytham”, Tamil; U+0B83 TAMIL SIGN VISARGA, annotated with alternative name “aytham”; “VISARGA” is a misanalysis inherited from ISCII

Sometimes the Unicode character names are very awkward, and glyph names may benefit from an **alternative set of terms**. As a rule of thumb, for a given script, if generally its letters are already alphasyllabic (syllabary or abugida), transcribe the letters’ pronunciations (see Meetei Mayek); otherwise (abjad or alphabet), transcribe native letter names (see Ol Chiki). Note that Latin is a special case (e.g., ‹B› is not named “Be”) only because there is no transcription involved.

Also, note at least three **levels of clarity** exhibited in Unicode character names and commonly used terms:

1. letter-level terms with an attribute
    - For example: “dental/soft ta” vs. “retroflex/hard ta”, “vocalic r” vs. “ra”, “heavy ya” vs. “ya”.
    - This style is informative, but is often cumbersome, and may look confusing when nested in composite names (e.g., `h-vocalic-r` for ‹हृ›).
2. letter-level multigraph terms
    - For example: “ta” vs. “tta”, “a” vs. “aa”, “ra” vs. “rha”.
    - This style strikes a decent balance between conciseness and unambiguousness, and it works well in nested composite names (e.g., `tt-ttha` for ‹ट्ठ›).
    - But it looks unnatural and does not work well for transcribing words, where letter boundaries are unclear. May work as an unusual scheme for word-level transcription when boundaries are marked.
3. word-level lossy transcriptions
    - For example: “virama” for _virāma_, “prishthamatra” for _pr̥ṣṭhamātrā_, “tha” and “ta” for _ta/tha_ and _ṭa/ṭha_ (Malayalam and Tamil).
    - This is the everyday style. It relies on folding rules to simplify transcriptions of certain phonetic segments, in order to deliver natural- and simple-looking transcriptions of words, but also introduces ambiguity.
    - Many folding rules are language-specific, such as the last example.
