# B. About glyphs

## B.1 Glyphs naming convention

Consistently assigned glyph names facilitate font production.

> FIGURE:
> vocalicrsign
> k_ssa.northern-Deva

A simple glyph name is derived from the corresponding Unicode character’s name, using the tool, glyphNameFormatter. <!-- [something about glyphsNameFormatter, where it can be found, how it works, etc] -->

An underscore ( `_` ) joins multiple glyph names to form a ligature name. Note this is syntax is meant to be used only for true ligatures that have multiple components written in a connected way. Glyphs that are only technically ligatures <!-- [what constitutes a technical ligature?]-->, such as glyphs for |ka__nukta|, may use double underscore ( `__` ) instead.

A period ( `.` ) leads a **variation suffix**, which is always after the main body of a glyph name and before the script suffix.

A dash ( `-` ) leads a **script suffix**, which clarifies a glyph’s script namespace, and is always placed at the end, after any variation suffixes. The suffix in principle should be the script’s code in ISO 15924, _Codes for the representation of names of scripts_. If a script namespace is explicitly set at the project level, that script’s suffix can be omitted.

> FIGURE:
> virama
> rasignbottom
> rasignrightinitial

<!-- A dependent sign or conjoining form of a base is systematically named with `sign` suffixed to the base’s name, if it has a corresponding base. It might have a conventional name as well. But how is a base named? -->

## B.2 Reference Glyphs

glyph | name | character | note
-- | -- | -- | --
◌ | dottedcircle | 25CC | placeholder base
अ | a-Deva | 0905 | variant: a.north
… | … | … |
