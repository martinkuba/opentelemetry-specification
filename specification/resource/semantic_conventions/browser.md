# Browser

**Status**: [Experimental](../../document-status.md)

**type:** `browser`

**Description**: The web browser in which the application represented by the resource is running. The `browser.*` attributes MUST be used only for resources that represent applications running in a web browser (regardless of whether running on a mobile or desktop device).

<!-- semconv device -->
| Attribute  | Type | Description  | Examples  | Required |
|---|---|---|---|---|
| `browser.brands` | array of strings | Array of brand name and version separated by a space [1] | `[' Not A;Brand 99','Chromium 99',Chrome 99']` | No |
| `browser.platform` | string | The platform on which the browser is running [2] | `Windows`, `macOS`, `Android` | No |
| `browser.user_agent` | string | Full user-agent string provided by the browser [3] | `'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36'` | No |

All of these attributes can be provided by the user agent itself in the form of an HTTP header (e.g. Sec-CH-UA, Sec-CH-Platform, User-Agent). However, the headers could be removed by proxy servers, and are tied to calls from individual clients. In order to support batching through services like the Collector and to prevent loss of data (e.g. due to proxy servers removing headers), these attributes should be used when possible.

**[1]:** This value is intended to be taken from the [UA client hints API](https://wicg.github.io/ua-client-hints/#interface) (navigator.userAgentData.brands).

**[2]:** This value is intended to be taken from the [UA client hints API](https://wicg.github.io/ua-client-hints/#interface) (navigator.userAgentData.platform). The legacy `navigator.platform` API should NOT be used in order for the values to be consistent.

**[3]:** The user-agent value should be provided only from browsers that do not have a mechanism to retrieve brands and platform individually from the User-Agent Client Hints API. To retrieve the value, the legacy `navigator.userAgent` API can be used.
<!-- endsemconv -->