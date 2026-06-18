# ainconv-tests
A unified test suite for [ainconv-js](https://github.com/mkpoli/ainconv) ([npm](https://www.npmjs.com/package/ainconv)), [ainconv-py](https://github.com/mkpoli/ainconv-py) ([PyPI](https://pypi.org/project/ainconv/)) and [ainconv-rs](https://github.com/mkpoli/ainconv-rs) ([Cargo](https://crates.io/crates/ainconv)).

## Files

- `test_cases.json`: Common test cases for conversion in default settings.
- `robustness.json`: Test special cases for robustness of unambiguous conversions, i.e. it should be less strict for input, so that multiple input that may be well-formed or ill-formed can yield the same output, as long as the input is valid and unambiguous.
- `options.schema.json`: The canonical catalog of conversion options. This is the single source of truth for which options exist, their defaults, and their semantics across all implementations.

## Conversion options

`options.schema.json` is the authoritative list of options. Every implementation
should support **exactly** this set (mapping the canonical camelCase `key` to its
own idiomatic naming) and assert parity against this file in its test suite, so
that an option added in one language fails the others' builds until they catch up.

A case in `test_cases.json` may carry an optional `variants` array describing how
the *same input* converts under non-default conversion options. Each variant has
an `options` object (camelCase keys) and the expected output(s) for whichever
directions it overrides:

```json
{
	"latn": "wenkur",
	"kana": "ウェンクㇽ",
	"variants": [{ "options": { "useWe": true }, "kana": "ヱンクㇽ" }]
}
```

Option keys mirror the per-implementation toggles:

| key | direction | effect |
| --- | --- | --- |
| `ellipsisToAscii` | kana → latn | rewrite `…` (U+2026) as `...` |
| `useWi` / `useWe` / `useWo` | latn → kana | keep `ヰ` / `ヱ` / `ヲ` instead of `ウィ` / `ウェ` / `ウォ` |
| `useSmallI` / `useSmallU` / `useSmallN` | latn → kana | keep small `ィ` / `ゥ` / `ㇴ` codas instead of `イ` / `ウ` / `ン` |

Implementations that do not support a given option (or do not yet read
`variants`) should skip the cases they cannot satisfy; the field is additive and
ignored by older harnesses.
