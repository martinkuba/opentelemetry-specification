# Events SDK

**Status**: [Experimental](../document-status.md)

<details>
<summary>Table of Contents</summary>

<!-- Re-generate TOC with `markdown-toc --no-first-h1 -i` -->

<!-- toc -->

- [Overview](#overview)
- [EventLoggerProvider](#eventloggerprovider)
- [EventLogger](#eventlogger)
  * [Emit Event](#emit-event)
- [Additional Interfaces](#additional-interfaces)

<!-- tocstop -->

</details>

Users of OpenTelemetry need a way for instrumentation interactions with the
OpenTelemetry API to actually produce telemetry. The OpenTelemetry SDK
(henceforth referred to as the SDK) is an implementation of the OpenTelemetry
API that provides users with this functionally.

All implementations of the OpenTelemetry API MUST provide an SDK.

## Overview

From OpenTelemetry's perspective LogRecords and Events are both represented
using the same [data model](./event-api.md#data-model). Therefore, the default
implementation of an Event SDK MUST generate events using the [Logs Data Model](./data-model.md).

The SDK MUST use the default [Logs SDK](./sdk.md) to generate, process and export `LogRecord`s.

## EventLoggerProvider

The `EventLoggerProvider` MUST be implemented as a proxy to an instance of `LoggerProvider`.

All `LogRecord`s produced by any `EventLogger` from the `EventLoggerProvider` SHOULD be associated with the `Resource` from the provided `LoggerProvider`.

### EventLoggerProvider Creation

The SDK SHOULD allow the creation of multiple independent `EventLoggerProviders`s.

### EventLogger Creation

It SHOULD only be possible to create `EventLogger` instances through a `EventLoggerProvider`
(see [Events API](event-api.md)).

The `EventLoggerProvider` MUST implement the [Get an EventLogger API](event-api.md#get-an-eventlogger).

The input provided by the user MUST be used to create
an [`InstrumentationScope`](../glossary.md#instrumentation-scope) instance which
is associated with `Event`s emitted by the created `EventLogger`.

In the case where an invalid `name` (null or empty string) is specified, a
working `EventLogger` MUST be returned as a fallback rather than returning null or
throwing an exception. Its `name` SHOULD keep the original invalid value, and a
message reporting that the specified value is invalid SHOULD be logged.

### Configuration

The `EventLoggerProvider` MUST accept an instance of `LoggerProvider`. Any configuration
related to processing MUST be done by configuring the `LoggerProvider` directly.

### Shutdown

This method provides a way for provider to do any cleanup required.

`Shutdown` MUST be called only once for each `EventLoggerProvider` instance. After
the call to `Shutdown`, subsequent attempts to get an `EventLogger` are not allowed.
SDKs SHOULD return a valid no-op `EventLogger` for these calls, if possible.

`Shutdown` SHOULD provide a way to let the caller know whether it succeeded,
failed or timed out.

`Shutdown` SHOULD complete or abort within some timeout. `Shutdown` MAY be
implemented as a blocking API or an asynchronous API which notifies the caller
via a callback or an event. [OpenTelemetry SDK](../overview.md#sdk) authors MAY
decide if they want to make the shutdown timeout configurable.

`Shutdown` MUST be implemented at least by invoking `Shutdown` on the delegate
`LoggerProvider`, which in effect invokes `Shutdown` on all registered 
registered [LogRecordProcessors](#logrecordprocessor).

### ForceFlush

This method provides a way for the provider to notify the delegate `LoggerProvider`
to force all registered [LogRecordProcessors](#logrecordprocessor) to immediately export all
`LogRecords` that have not yet been exported.

`ForceFlush` SHOULD provide a way to let the caller know whether it succeeded,
failed or timed out. `ForceFlush` SHOULD return some **ERROR** status if there
is an error condition; and if there is no error condition, it SHOULD return
some **NO ERROR** status, language implementations MAY decide how to model
**ERROR** and **NO ERROR**.

`ForceFlush` SHOULD complete or abort within some timeout. `ForceFlush` MAY be
implemented as a blocking API or an asynchronous API which notifies the caller
via a callback or an event. [OpenTelemetry SDK](../overview.md#sdk) authors MAY
decide if they want to make the flush timeout configurable.

`ForceFlush` MUST invoke `ForceFlush` on all
[LogRecordProcessors](#logrecordprocessor) that are registered with the delegate
`LoggerProvider`.

## EventLogger

### Emit Event

Emit a `LogRecord` representing an `Event`.

**Implementation Requirements:**

The implementation MUST use the parameters
to [emit a logRecord](./bridge-api.md#emit-a-logrecord) as follows:

* The `Name` MUST be used to set
  the `event.name` [Attribute](./data-model.md#field-attributes). If
  the `Attributes` provided by the user contain an `event.name` attribute the
  value provided in the `Name` takes precedence.
* If provided by the user, the `Payload` MUST be used to set
  the [Body](./data-model.md#field-body). If not provided, `Body` MUST not be
  set.
* If provided by the user, the `Timestamp` MUST be used to set
  the [Timestamp](./data-model.md#field-timestamp). If not provided, `Timestamp`
  MUST be set to the current time when [emit](#emit-event) was called.
* The [Observed Timestamp](./data-model.md#field-observedtimestamp) MUST not be
  set. (NOTE: [emit a logRecord](./bridge-api.md#emit-a-logrecord) will
  set `ObservedTimestamp` to the current time when unset.)
* If provided by the user, the `Context` MUST be used to set
  the [Context](./bridge-api.md#emit-a-logrecord). If not provided, `Context`
  MUST be set to the current Context.
* If provided by the user, the `SeverityNumber` MUST be used to set
  the [Severity Number](./data-model.md#field-severitynumber) when emitting the
  logRecord. If not provided, `SeverityNumber` MUST be set
  to `SEVERITY_NUMBER_INFO=9`.
* The [Severity Text](./data-model.md#field-severitytext) MUST not be set.
* If provided by the user, the `Attributes` MUST be used to set
  the [Attributes](./data-model.md#field-attributes). The user
  provided `Attributes` MUST not take over the `event.name`
  attribute previously discussed.
