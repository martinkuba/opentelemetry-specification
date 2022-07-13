# Event

**Status**: [Experimental](../../../document-status.md)

**type:** `event`

**Description**: All events are described by these attributes.

<!-- semconv browser -->
| Attribute  | Type | Description  | Examples  | Requirement Level |
|---|---|---|---|---|
| `event.name` | string | The event name that uniquely identifies the type of the event within the domain. | `exception`; `interaction` | Required |
| `event.domain` | string | Identifies the group of related events. Each event within a domain MUST have a unique value for the event.name attribute. | `browser`; `mobile` | Recommended |
| `event.data` | any | It is recommended that event attributes are sent as a nested object value in this attribute. [1] | Recommended |
<!-- endsemconv -->



