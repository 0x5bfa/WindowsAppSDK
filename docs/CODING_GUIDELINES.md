# Coding Guidelines

The Windows App SDK prefers using industry-standard coding styles, guidelines, and patterns for any
languages used in implementation or testing.

## Error handling

- **DO** throw exceptions to report exceptional failures.
- **DO** use [WIL error handling helpers](https://github.com/Microsoft/wil/wiki/Error-handling-helpers) when handling error code failures (`THROW_*()`, `RETURN_*()`, `FAIL_FAST_*()`, `LOG_*()`).
- **DO** use existing `HRESULT`s if possible. Be careful to pick an existing HRESULT whose symbolic name and message convey the intent and don't mislead the developer, administrator or user.
- **DO** create new `HRESULT`s with `FACILITY_ITF` **only** when you can't find a reasonable existing one. Any new `FACILITY_ITF` error **must** use code values in the range `0x0200`-`0xFFFF`. See Codes in `FACILITY_ITF` for more details.

## Logging

- **DO** rely on WIL error handling helpers to log failures.
- **DO** use `TraceLoggingWrite()` to log non-failure information.
- **DON'T** use WIL's `LOG_HR()` to report non-failure information. `LOG_HR()` will `FAIL_FAST` if handled a `SUCCEEDED(hr)`.
- **DON'T** define error `HRESULT`s to pass to `LOG_HR()` to report non-failure information. Non-error information should be reported via `TraceLoggingWrite()`.

## C++ coding convention

Please refer to [C++ coding guidelines](guidelines/CppCodingConvention.md).

## Develop an experimental feature

Please refer to [Experimental API guidelines](guidelines/Experimental.md) and [TerminalVelocity guidelines](guidelines/TerminalVelocity.md).

## HybridCRT

Please refer to [HybridCRT guidelines](guidelines/HybridCRT.md).

## Self-contained

Please refer to [Self-contained guidelines](guidelines/SelfContained.md).

## WinRT registration

Please refer to [WinRT registration guidelines](guidelines/WinRT-Registration.md).

## Release policy

### Preview and Stable (non-Experimental)

In order to release an API to Preview or Stable, the `main` branch must meet the following release requirements:

- API proposal was approved
- Tests for the API is added
- Prepared to accept feedback from the public
- Functionality is Feature Complete by a release's fork for Preview1 4a. Approved Design Change Requests (DCR) can be made after Preview1. See the DCR process to approve these exceptions.

### Experimental

The policies for Experimental API differ from non-Experimental API because of the reasons below:

1. **Selfhost**: a change is good but you're interested in community usage and feedback to verify it's ready for Preview/Stable.
2. **Skeleton**: API Review has approved and you've made an initial 'skeletal' implementation to get the interface and infrastructure building but isn't functionally complete e.g. skeletal code compiles but everything returns E_NOTIMPL/false/null/etc). Having the API surface building in main can unblock documentation, test and sample writers.
3. **Feedback**: your API shape isn't locked (or even clear) but you want and/or need feedback from the community, partners and/or others before deciding the API is ready to consider for non-Experimental releases. The API is not yet ready to go through API Review.

Reasons **not** to make Experimental API:

1. **Prototype**: prototypes, ideas and wild experimentation should be done on a 'feature branch' and not checked into main. To make a 'feature branch' checkout a new branch off main (or a child of main) with no intention to checkin to main any time soon (or maybe ever). Make a feature branch and don't check into main

In order to release an API to Experimental, the `main` branch must meet the following release requirements:

- **Consider** Review API with your local-representative
- Tests for the API is added
- Prepared to accept feedback from the public
- Experimental APIs are tagged as `experimental` (The mechanisms to do so may vary in WinRT APIs, Flat-C APIs, texts, etc)
- Experimental APIs don't go through full API Review but should be discussed with your local API Review representative. This ensures your API is aligned with API practices and policies and avoids the risk of significant changes required to pass API Review.
- Has ppropriate test coverage. This is no different than non-Experimental APIs, except meaning of 'appropriate' may vary. For instance, 'skeletal' Experimental APIs don't need the same breadth of test coverage as non-Experimental APIs.
