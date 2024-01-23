<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

# Apache Daffodil SBT Plugin

Plugin to run Daffodil on DFDL schema projects.

## Enable

To enable the plugin, add the following to `project/plugins.sbt`:

```scala
addSbtPlugin("org.apache.daffodil" % "sbt-daffodil" % "<version>")
```

## Features

### Saved Parsers

This plugin adds the ability to create and publish saved parsers of a schema.

For each saved parser to generate, add an entry to the
`daffodilPackageBinInfos` setting. This setting is a Seq of 3-tuples made up of
the resource path to the schema, an optional root element to use in that
schema, and an optional name that is added to the artifact classifier to
differentiate multiple saved parsers. If the optional root element is `None`,
then the first element in the schemas is used. An example of this settings
supporting two roots looks like this:

```scala
daffodilPackageBinInfos := Seq(
  ("/com/example/xsd/mainSchema.dfdl.xsd", Some("record"), None)
  ("/com/example/xsd/mainSchema.dfdl.xsd", Some("fileOrRecords"), Some("file"))
)
```

You must also define which versions of Daffodil to build comptiable saved
parsers using the `daffodilPackageBinVersions` setting. For example, to build
saved parsers for Daffodil 3.6.0 and 3.5.0:

```scala
daffodilPackageBinVersions := Set("3.6.0", "3.5.0")
```

Then run `sbt packageDaffodilBin` to generate saved parsers in the `target/`
directory. For example, assuming a schema project with name of "format",
version set to "1.0", and the above configurations, the task would generate the
following saved parsers:

```
target/format-1.0-daffodil350.bin
target/format-1.0-daffodil360.bin
target/format-1.0-file-daffodil350.bin
target/format-1.0-file-daffodil360.bin
```

Note that the artifact names have the suffix "daffodilXYZ".bin, where XYZ is
the version of Daffodil the saved parser is compatible with.

The `publish`, `publishLocal`, `publishM2` and related publish tasks are
modified to automatically build and publish the saved parsers as a new
artifacts.

# License

Apache Daffodil SBT Plugin is licensed under the [Apache License, v2.0].

[Apache License, v2.0]: https://www.apache.org/licenses/LICENSE-2.0
