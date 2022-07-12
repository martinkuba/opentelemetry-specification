# Navigation

**Status**: [Experimental](../../../../document-status.md)

**type:** `resource`

**Description**: Represents a resource observed by the browser.

<!-- semconv browser -->
| Attribute  | Type | Value  | Requirement Level |
|---|---|---|---|
| `event.name` | string | `resource` | Required |
| `event.domain` | string | `browser` | Required |
<!-- endsemconv -->

Additional attributes are mapped from the [Resource Timing API](https://www.w3.org/TR/resource-timing-2/#sec-performanceresourcetiming).

This list is not exhaustive and is listed as an example. All numeric and string values thar are provided by the Resource Timing API SHOULD be included.

| Attribute  | Type | Example  | Requirement Level |
|---|---|---|---|
| `name` | string | `https://example.com/a/b/` | Recommended |
| `redirectStart` | double | 0 | Recommended |
| `redirectEnd` | double | 0 | Recommended |
| `fetchStart` | double | 27.80000112 | Recommended |
| `domainLookupStart` | double | 33.5 | Recommended |
| `domainLookupEnd` | double | 60.5999 | Recommended |
| `connectStart` | double | 60.5999 | Recommended |
| `connectEnd` | double | 113.7 | Recommended |
| `secureConnectionStart` | double | 72.8 | Recommended |
| `requestStart` | double | 114.3 | Recommended |
| `responseStart` | double | 738.8 | Recommended |
| `responseEnd` | double | 790.599 | Recommended |
| `transferSize` | double | 71967 | Recommended |
| `encodedBodySize` | double | 71667 | Recommended |
| `decodedBodySize` | double | 364537 | Recommended |
