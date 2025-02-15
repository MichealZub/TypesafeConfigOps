# ${Typesafe} %Config .Ops

[![codecov](https://codecov.io/gh/typesafeconfigops/TypesafeConfigOps/branch/master/graph/badge.svg)](https://codecov.io/gh/typesafeconfigops/TypesafeConfigOps)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/83a505b53f004ad19bcc55fe6d483b56)](https://app.codacy.com/app/lashchenko/TypesafeConfigOps?utm_source=github.com&utm_medium=referral&utm_content=typesafeconfigops/TypesafeConfigOps&utm_campaign=Badge_Grade_Dashboard)
[![Build Status](https://app.travis-ci.com/TypesafeConfigOps/TypesafeConfigOps.svg?branch=master)](https://app.travis-ci.com/TypesafeConfigOps/TypesafeConfigOps)

### Usage
```scala
// build.sbt
libraryDependencies += "io.github.typesafeconfigops" %% "typesafe-config-ops" % "1.4.1.1"
```

### ConfigOptOps
Import `TypesafeConfigOps.ConfigOptOps` allows to extract optional values from configuration.

```scala
import TypesafeConfigOps.ConfigOptOps

private val cfg = ConfigFactory.parseString("""{ "i" = 1 }""")
cfg.optInt("i") // Some(1)
cfg.optInt("ix") // None
```

### ConfigDefaultOps
Import `TypesafeConfigOps.ConfigDefaultOps` allows using default values for non-existing paths in configuration.

```scala
import TypesafeConfigOps.ConfigDefaultOps

private val cfg = ConfigFactory.parseString("""{ "i" = 1 }""")
cfg.getInt("i", 2) // 1
cfg.getInt("ix", 2) // 2
```
### ConfigTemplateOps
Import `TypesafeConfigOps.ConfigTemplateOps` allows to work with templates in configuration.

```scala
import TypesafeConfigOps.ConfigTemplateOps

val cfg = ConfigFactory.parseString(
  """
    |{
    |  code = <img ${imgSrc} ${imgHeight} ${imgWidth} ${imgBorder} />
    |}
  """.stripMargin)

val code = cfg
  .resolveTemplate(
    "imgSrc" -> "src='%s'",
    "imgHeight" -> "height='%dpx'",
    "imgWidth" -> "width='%dpx'",
    "imgBorder" -> "border='#%X%X%X'")
  .formatTemplate("code", "https://google.com/logo.png", 100, 200, 255, 255, 255)

code // "<img src='https://google.com/logo.png' height='100px' width='200px' border='#FFFFFF' />"
```
