# A. Glyph naming

Systematic and informative glyph naming facilitates design and development. Typotheque’s scheme for glyph names of Indic scripts is introduced in this appendix, and is used across this document. For the following scripts, the Glyph Database ([appendix X](glyph-database.md)) provides recommended names of their core glyphs, including all the atomically encoded glyphs:

- Scripts used by official languages in India:

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

- Other scripts:

    - සිංහල Sinhala

By default the glyph names we talk about are the so called **development glyph names** (d-names), which may be different from **production glyph names** (p-names). The recommendation about p-names will be made available later. Note that for Indic scripts because of their complex shaping, a PDF needs to be tagged (e.g., with `\ActualText`) anyway in order to enable text extraction, which makes it largely unnecessary to supply p-names that are compliant with the [Adobe Glyph List Specification](https://github.com/adobe-type-tools/agl-specification).

## A.1 Structure

A glyph name is up to 63 characters in length, consisting of only characters `A`–`Z`, `a`–`z`, `0`–`9`, period ( `.` ), underscore ( `_` ), and dash ( `-` ). Dash is only valid in d-names. No glyph names can start with a digit or period, except `.notdef`. Although the [OpenType Feature File Specification](https://adobe-type-tools.github.io/afdko/OpenTypeFeatureFileSpecification.html#2.f.i) has cleared seven d-name-only characters, we use only dash due to the [Glyphs app](http://glyphsapp.com)’s restriction.

A **plain name** is followed by a **script suffix** that specifies the script namespace.

Regardless of the script’s OTL script tag(s), the suffix uses the script’s Capitalized four-letter code from [ISO 15924](https://unicode.org/iso15924/iso15924-codes.html), _Codes for the representation of names of scripts_. The suffix is led by a double period (imagine a lied down colon). For how to omit the script suffix by specifying an implied script namespace, see section A.3, _Shorthands and alternatives_.

    a..Latn  # ‹a› glyph “a”, Latin script
    a..Deva  # ‹अ› glyph “a”, Devanagari script

For the sake of readability, plain names are in all-lowercase by default. Bicameral scripts’ capital letters get Capitalized plain names. When a plain name has a composite structure, its constituting parts (e.g., constituting words of a phrase) are joined by dashes.

Atomically encoded glyph typically get a name derived from its Unicode character’s name. The Glyph Database provides recommended forms of these **Unicode-derived names** for all the atomically encoded glyphs, and may incorporate [LettError/glyphNameFormatter](https://github.com/LettError/glyphNameFormatter) in the future. For some behind-the-scenes explanation of the strategies used when deriving those recommended names, see section A.4, _Transforming Unicode character names_.

    vocalic-r..Deva  # ‹ऋ› “vocalic r”, Devanagari; U+09DB DEVANAGARI LETTER VOCALIC R
    candra-a..Deva   # ‹ॲ› “candra a”, Devanagari; U+0972 DEVANAGARI LETTER CANDRA A
    sign-aa..Deva    # ‹…› “sign aa”, Devanagari; U+093E DEVANAGARI VOWEL SIGN AA

For glyphs that are not atomically encoded, **systematic names** are generated with elements taken from Unicode-derived names. Note this secondary derivation is not about treating non-atomically-encoded glyphs as second-class citizens, and does not even have much to do with the encoding model. It is only for the sake of reliable naming, as Unicode characters have standardized and stable names. For the rules, see section A.2, _Systematic names_.

    k-ssa..Deva    # ‹क्ष› “k-ssa” (kṣa), Devanagari
    sign-ra..Deva  # ‹…› conjoining sign of “ra” (graphically bottom, phonetically post-base), Devanagari

One or more **variant suffixes** may follow the plain name and precede the script suffix. Each variant suffix is led by a period. Variant suffixes should be informative, and avoid unnecessary abbreviation.

    five.northern..Deva          # ‹…› “five”, northern variant, Devanagari
    five.northern.tabular..Deva  # ‹…› “five”, tabular variant of northern variant, Devanagari

If the glyph is a **typographic ligature**, join its components’ plain names with dashes.

    repha_anusvara..Deva         # ‹…› ligature of “repha” and “anusvara”, Devanagari
    repha_anusvara.narrow..Deva  # ‹…› ligature of “repha” and “anusvara”, narrow variant, Devanagari

## A.2 Systematic names

According to our [graphic analysis](graphic-analysis.md) of a script’s composition, name a graphically obscure base according to its phonetic segments, and name a conjoining sign according to its base.

With **phonetic segmentation**, split all phonetic segments (except the inherent vowel) of a glyph, transcribe them according to the phonetically corresponding base glyphs, then join them with dashes. Note that how a glyph is encoded is irrelevant to this naming strategy.

    k-ssa..Deva        # ‹क्ष› “k ssa” (kṣa), Devanagari
    r-u..Deva          # ‹रु› “r u” (ru), Devanagari
    h-vocalic-r..Deva  # ‹हृ› “h vocalic-r” (hr̥), Devanagari

For a **conjoining sign**, prefix its corresponding base’s name with `sign` so it is distinguished from the base, then suffix with tags for its graphic (`top`/`left`/`bottom`/`right`) and phonetic relationship (`initial`/`pre`/`post`/…) with its graphic base.

    ra..Deva                   # ‹र› “ra”, Devanagari
    sign-ra-top-initial..Deva  # ‹…› conjoining sign of “ra”, graphically top, phonetically initial, Devanagari
    sign-ra-left-pre..Deva     # ‹र्‍› conjoining sign of “ra”, graphically left, phonetically pre-base, Devanagari
    sign-ra-bottom-post..Deva  # ‹…› conjoining sign of “ra”, graphically bottom, phonetically post-base, Devanagari

In reality though, such cumbersome systematic names are seldom recommended and are only reserved as a fallback mechanism. Instead, most conjoining signs are named in **shorthands**. For an introduction, see section A.3, _Shorthands and alternatives_.

    repha..Deva    # ‹…› “repha”, Devanagari
    r..Deva        # ‹र्‍› “r”, Devanagari
    sign-ra..Deva  # ‹…› conjoining sign of “ra”, Devanagari

## A.3 Shorthands and alternatives

### Shorthands for systematic names

When there is no ambiguity inside a script, the dashes in phonetic segmentation may be systematically omitted.

    ku..Taml    # ‹கு› “ku” (ku) instead of “k u”, Tamil
    kssa..Taml  # ‹க்ஷ› “kssa” (kṣa) instead of ”k ssa”, Tamil
    srii..Taml  # ‹ஸ்ரீ› “srii” (srī) instead of “s r ii”, Tamil

Phonetically pre-base signs, coda signs, and letters with zeroed nucleus can be simply named after the pure consonants, as long as they form a series and there is no ambiguity inside the script.

    k..Deva     # ‹क्‍› implied phonetically pre-base conjoining sign of “ka”, Devanagari
    r..Deva     # ‹र्‍› implied phonetically pre-base conjoining sign of “ra”, Devanagari
    k-ss..Deva  # ‹क्ष्‍› implied phonetically pre-base conjoining sign of “k-ssa”, Devanagari

    # Malayalam
    nn..Mlym  # ‹ൺ› implied chillu of “nna”, Malayalam
    n..Mlym   # ‹ൻ› implied chillu of “na”, Malayalam
    r..Mlym   # ‹ർ› implied chillu of “ra”, Malayalam

    # Kannada
    k..Knda   # ‹ಕ್› implied zero-nucleus “ka”, Kannada
    kh..Knda  # ‹ಖ್› implied zero-nucleus “kha”, Kannada
    g..Knda   # ‹ಗ್› implied zero-nucleus “ga”, Kannada

Shorthands are generally recommended for conjoining signs. They avoid verbose names, and can exhibit some shared behavior across scripts. For example, repha (conjoining sign of ra, phonetically initial):

    repha..Deva  # ‹…› “repha”, (graphically top), Devanagari
    repha..Knda  # ‹…› “repha”, (graphically right), Kannada
    repha..Mlym  # ‹ൎ› “repha”, (graphically top), Malayalam; U+0D4E MALAYALAM LETTER DOT REPH

Phonetically post-base signs are considered the default conjoining signs, and thus do not need to specify the phonetic relationship. The tags for graphic relationship may be omitted when either there is no ambiguity inside a script or a sign’s graphic relationship with the base is consistent with the predominant one of the script.

    sign-ra..Deva  # ‹…› implied phonetically post-base conjoining sign of “ra”, (graphically bottom), Devanagari

    sign-ya..Mlym  # ‹…› implied phonetically post-base conjoining sign of “ya”, (graphically right), Malayalam
    sign-ra..Mlym  # ‹…› implied phonetically post-base conjoining sign of “ra”, (graphically left), Malayalam
    sign-la..Mlym  # ‹…› implied phonetically post-base conjoining sign of “la”, (graphically bottom), Malayalam
    sign-va..Mlym  # ‹…› implied phonetically post-base conjoining sign of “va”, (graphically right), Malayalam

    sign-ya..Guru        # ‹…› implied phonetically post-base conjoining sign of “ya”, (graphically bottom), Gurmukhi
    sign-ya-right..Guru  # ‹…› implied phonetically post-base conjoining sign of “ya”, graphically right, Gurmukhi

### OTL feature tags as variant suffixes

For a variant glyph that is intended specifically for an OTL feature, the feature’s four-letter tag may be used as the suffix, if it is clear enough from the tag alone what the glyph should look like. Tags like `dlig`, `salt`, and `ss01` are examples that do not meet the recommendation.

    one.tnum..Deva     # ‹१› “one”, variant for feature “tnum” (Tabular Figures), Devanagari
    sign-e.init..Beng  # ‹…› conjoining sign of “e”, variant for feature “init” (Initial Forms), Bangla

### Alternative ways of declaring a script namespace

A script namespace may be inherited from a project-wide declaration (e.g., in a .glyphs file’s “Notes” field). For example, a Devanagari-dominated project can have:

    # Script namespace: Devanagari (ISO 15924 code: Deva)

    a          # ‹अ› implied Devanagari “a”
    ka         # ‹क› implied Devanagari “ka”
    one        # ‹१› implied Devanagari “one”
    a..Latn    # ‹a› “a”, Latin
    one..Latn  # ‹1› “a”, Latin

Implied script namespace, however, makes copying glyphs from one project to another a slightly complicated task, as renaming is often necessary.

In a workflow where more characters are supported in d-names, a colon ( `:` ) may be preferred to lead the script namespace suffix (e.g., `a:Deva`). This may aid readability. With a colon, the script code may also be declared as a prefix (`Deva:a`), although in practice a suffixed script code makes it easier to search for glyphs.

## A.4 Transforming Unicode character names

Unicode character names are decent starting points for glyph names, but in order to derive helpful glyph names, they do require some systematic transformation, as well as some overriding. This is an FYI section about the strategies and considerations behind our recommended Unicode-derived glyph names.

Generally, the following unnecessary attributive words are first stripped away from a Unicode character name:

- the script name, for example, `DEVANAGARI`
- general categorization terms, such as `LETTER`, `DIGIT`, `NUMBER`, `FRACTION`, or `PUNCTUATION`
- phonetic categorization terms, `VOWEL` or `CONSONANT`

Also stripped away is `SIGN` or `MARK` when it is not used for disambiguation, especially when the encoded glyph is not considered a base’s conjoining sign (i.e., an orphan sign). For example, simply `virama..Deva` for `U+094D DEVANAGARI SIGN VIRAMA`, but `sign-aa..Deva` for `U+093E DEVANAGARI VOWEL SIGN AA` because there is also `aa..Deva` for `U+0906 DEVANAGARI LETTER AA`.

Then, transform the leftover words to all-lowercase, then replace all word spaces by dashes. Do not reorder words to make related glyphs alphabetically sorted together. Stick to the original word order. Glyph orders should be maintained by sort keys.

Certain **overriding** can be helpful. Scripts like Sinhala and Meetei Mayek use native terms in character names, with the Sanskrit-conforming generic names annotated. If a group of glyphs apparently conform to a generic system, such as the Sanskrit alphabet or the decimal digits, the generic terms should be used instead. Certain Unicode character names can also be inappropriate or even simply misnomers, and thus should be corrected in glyph names.

    ka..Sinh      # ‹ක› “ka”, Sinhala; U+0D9A SINHALA LETTER ALPAPRAANA KAYANNA, annotated with alternative name “sinhala letter ka”
    ka..Mtei      # ‹ꯀ› “ka”, Meetei Mayek; U+ABC0 MEETEI MAYEK LETTER KOK, annotated with alternative name “ka”
    svarita       # ‹…› “svarita“, general script; U+0951 DEVANAGARI STRESS SIGN UDATTA, annotated with alternative name “Vedic tone svarita” and informative note “mostly used for svarita, with rare use for udatta”
    llla..Knda    # ‹ೞ› “llla”, Kannada; U+0CDE KANNADA LETTER FA, annotated with normative alias “KANNADA LETTER LLLA” and informative note “name is a mistake for LLLA”
    aytham..Taml  # ‹ஃ› “aytham”, Tamil; U+0B83 TAMIL SIGN VISARGA, annotated with alternative name “aytham”; “VISARGA” is a misanalysis inherited from ISCII

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
