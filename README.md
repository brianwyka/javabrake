# Java notifier for Airbrake

[![Build Status](https://travis-ci.org/airbrake/javabrake.svg?branch=master)](https://travis-ci.org/airbrake/javabrake)

<img src="http://f.cl.ly/items/0u0S1z2l3Q05371C1C0I/java%2009.19.32.jpg" width=800px>

## Installation

Gradle:

```gradle
compile 'io.airbrake:javabrake:0.1.4'
```

Maven:

```xml
<dependency>
  <groupId>io.airbrake</groupId>
  <artifactId>javabrake</artifactId>
  <version>0.1.4</version>
  <type>pom</type>
</dependency>
```

Ivy:

```xml
<dependency org='io.airbrake' name='javabrake' rev='0.1.4'>
  <artifact name='javabrake' ext='pom'></artifact>
</dependency>
```

## Quickstart

Configuration:

```java
import io.airbrake.javabrake.Notifier;

int projectId = 12345;
String projectKey = "abcdefg";
Notifier notifier = new Notifier(projectId, projectKey);

notifier.addFilter(
    (Notice notice) -> {
      notice.setContext("environment", "production");
      return notice;
    });
```

Using `notifier` directly:

```java
try {
  do();
} catch (IOException e) {
  notifier.report(e);
}
```

Using `Airbrake` proxy class:

```java
import io.airbrake.javabrake.Airbrake;

try {
  do();
} catch (IOException e) {
  Airbrake.report(e);
}
```

By default `report` sends errors asynchronously returning a `Future`, but synchronous API is also available:

```java
import io.airbrake.javabrake.Notice;

Notice notice = Airbrake.reportSync(e);
if (notice.exception != null) {
    logger.info(notice.exception);
} else {
    logger.info(notice.id);
}
```

You can also set custom params on all reported notices:

```java
notifier.addFilter(
    (Notice notice) -> {
      notice.setParam("myparam", "myvalue");
      return notice;
    });
```

To debug why notices are not sent you can use `onReportedNotice` hook:

```java
notifier.onReportedNotice(
    (notice) -> {
      if (notice.exception != null) {
        logger.info(notice.exception);
      } else {
        logger.info(String.format("notice id=%s url=%s", notice.id, notice.url));
      }
    });
```

## log4j integration

See https://github.com/airbrake/log4javabrake

## log4j2 integration

See https://github.com/airbrake/log4javabrake2

## logback integration

See https://github.com/airbrake/logback

## Build

```shell
./gradlew build
```

Upload to JCentral:

```shell
./gradlew bintrayUpload
```

Upload to Maven Central:

```shell
./gradlew uploadArchives
./gradlew closeAndReleaseRepository
```

Usefull links:
 - http://central.sonatype.org/pages/gradle.html
 - http://central.sonatype.org/pages/releasing-the-deployment.html
