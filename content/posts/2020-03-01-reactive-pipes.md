---
title: "Reactive Pipes"
description: "Mario in RxJava"
date: 2020-03-01
slug: reactive-pipes
---

Unix command line pipes are extremely useful for dealing with multi-stage data processing.
The following chain of commands counts RxJava-related dependencies.

```shell
$ cat build.gradle.kts | grep "rx" | wc -l
```

* `cat build.gradle.kts` — prints the file to the standard output.
* `grep "rx"` — filters out lines without `rx` in them.
* `wc -l` — counts lines.

Notice that each command is independent and doesn’t know anything
about neighbours. Even better —
each command is executed in an isolated process.
Sounds like pure functions, right?

```kotlin
interface FlowTransformer<Upstream, Downstream> {

    fun transform(upstream: Flow<Upstream>): Flow<Downstream>
}

fun <Upstream, Downstream> Flow<Upstream>.compose(
    transformer: FlowTransformer<Upstream, Downstream>
) = transformer.transform(it)
```
