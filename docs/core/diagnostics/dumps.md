---
title: Dumps - .NET
description: An introduction to dumps in .NET.
ms.date: 10/12/2020
---

# Dumps

A dump is a file that contains a snapshot of the process at the time it was created and can be useful for examining the state of your application. Dumps can be used to debug your .NET application when it is difficult to attach a debugger to it such as production or CI environments. Using dumps allows you to capture the state of the problematic process and examine it without having to stop the application.

## Collect dumps

Dumps can be collected in a variety of ways depending on which platform you are running your app on.

> [!NOTE]
> Collecting a dump inside a container requires PTRACE capability, which can be added via `--cap-add=SYS_PTRACE` or `--privileged`.

> [!NOTE]
> Dumps may contain sensitive information because they can contain the full memory of the running process. Handle them with any security restrictions and guidances in mind.

### Collecting dumps on crash

You can use environment variables to configure your application to collect a dump upon a crash. This is helpful when you want to get an understanding of why a crash happened. For example, capturing a dump when an exception is thrown helps you identify an issue by examining the state of the app when it crashed.

The following table shows the environment variables you can configure for collecting dumps on a crash.

|Environment variable|Description|Default value|
|-------|---------|---|
|`COMPlus_DbgEnableMiniDump`|If set to 1, enable core dump generation.|0|
|`COMPlus_DbgMiniDumpType`|Type of dump to be collected. For more information, see the table below|2 (`MiniDumpWithPrivateReadWriteMemory`)|
|`COMPlus_DbgMiniDumpName`|Path to a file to write the dump to.|`/tmp/coredump.<pid>`|
|`COMPlus_CreateDumpDiagnostics`|If set to 1, enable diagnostic logging of dump process.|0|

The table below shows all the options you may use for `COMPlus_DbgMiniDumpType` which can be specified as a value. For example, setting `COMPlus_DbgMiniDumpType` to 1 means `MiniDumpNormal` type dump will be collected on a crash.

|Value|Name|Description|
|-----|----|-----------|
|1|`MiniDumpNormal`|Include just the information necessary to capture stack traces for all existing threads in a process. Limited GC heap memory and information.|
|2|`MiniDumpWithPrivateReadWriteMemory`|Includes the GC heaps and information necessary to capture stack traces for all existing threads in a process.|
|3|`MiniDumpFilterTriage`|Include just the information necessary to capture stack traces for all existing threads in a process. Limited GC heap memory and information.|
|4|`MiniDumpWithFullMemory`|Include all accessible memory in the process. The raw memory data is included at the end, so that the initial structures can be mapped directly without the raw memory information. This option can result in a very large file.|

### Collecting dumps at specific point in time

You may want to collect a dump when the app hasn't crashed yet. For example, if you want to examine the state of an application that seems to be in a deadlock, configuring the environment variables to collect dumps on crash will not be helpful because the app is still running.

To collect dump at your own request, you can use `dotnet-dump`, which is a CLI tool for collecting and analyzing dumps. For more information on how to use it to collect dumps with `dotnet-dump`, see [Dump collection and analysis utility](dotnet-dump.md).

## Analyze dumps

Dumps can be analyzed using [`dotnet-dump`](dotnet-dump.md).

## See also

Learn more about how you can leverage dumps to help diagnosing problems in your .NET application.

* [Debug Linux dumps](debug-linux-dumps.md) tutorial walks you through how to debug a dump that was collected in Linux.

* [Debug deadlock](debug-deadlock.md) tutorial walks you through how to debug a deadlock in your .NET application using dumps.