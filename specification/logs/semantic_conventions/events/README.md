# Events semantic conventions

Events in OpenTelemetry are a subset of logs with specific set of semantic conventions.

Each event MUST follow the [`events.*`](./events.md) semantic conventions, and it must follow semantic conventions specific for the type of event.

Each event is uniquely identified by the combination of name  (`event.name` attribute) and domain (`event.domain` attribute). Each subfolder in this folder represents a domain. Individual files in the subfolders represent semantic conventions specific to individual events.
