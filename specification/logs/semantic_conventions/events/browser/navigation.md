# Navigation

**Status**: [Experimental](../../../../document-status.md)

**type:** `navigation`

**Description**: Represents a browser navigation event.

The following attributes MUST be set with these predefined values:

<!-- semconv browser -->
| Attribute  | Type | Value  | Requirement Level |
|---|---|---|---|
| `event.name` | string | `navigation` | Required |
| `event.domain` | string | `browser` | Required |
<!-- endsemconv -->

Additional attributes are mapped from the [Navigation Timing API](https://www.w3.org/TR/navigation-timing-2/#sec-PerformanceNavigationTiming).

This list is not exhaustive and is listed as an example. All numeric and string values thar are provided by the Navigation Timing API SHOULD be included.


| Attribute  | Type | Example  | Requirement Level |
|---|---|---|---|
| `name` | string | `https://example.com/a/b` | Recommended |
| `unloadEventStart` | double | 0 | Recommended |
| `unloadEventEnd` | double | 0 | Recommended |
| `domInteractive` | double | 1002.34 | Recommended |
| `domContentLoadedEventStart` | double | 1423.54 | Recommended |
| `domContentLoadedEventEnd` | double | 1428.51 | Recommended |
| `domComplete` | double | 1512.43 | Recommended |
| `loadEventStart` | double | 1589.23 | Recommended |
| `loadEventEnd` | double | 1599.21 | Recommended |
| `type` | string | `navigate`; `reload`; `back_forward` | Recommended |
| `redirectCount` | int | 0 | Recommended |
