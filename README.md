# ainconv-tests
A unified test suite for [ainconv-js](https://github.com/mkpoli/ainconv) ([npm](https://www.npmjs.com/package/ainconv)), [ainconv-py](https://github.com/mkpoli/ainconv-py) ([PyPI](https://pypi.org/project/ainconv/)) and [ainconv-rs](https://github.com/mkpoli/ainconv-rs) ([Cargo](https://crates.io/crates/ainconv)).

## Files

- `test_cases.json`: Common test cases for conversion in default settings.
- `robustness.json`: Test special cases for robustness of unambiguous conversions, i.e. it should be less strict for input, so that multiple input that may be well-formed or ill-formed can yield the same output, as long as the input is valid and unambiguous.
