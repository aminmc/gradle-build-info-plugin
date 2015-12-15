# gradle-build-info-plugin

[![Build Status](https://travis-ci.org/ksoichiro/gradle-build-info-plugin.svg?branch=master)](https://travis-ci.org/ksoichiro/gradle-build-info-plugin)
[![Coverage Status](https://coveralls.io/repos/ksoichiro/gradle-build-info-plugin/badge.svg?branch=master&service=github)](https://coveralls.io/github/ksoichiro/gradle-build-info-plugin?branch=master)

Gradle plugin to include build information such as Git commit ID to your JAR.

## Usage

Apply plugin:

```gradle
plugins {
    id 'com.github.ksoichiro.build.info' version '0.1.1'
}
```

Note that you can use this plugin only when you use java plugin because you build JAR:

```gradle
apply plugin: 'java'
```

When used with Spring Boot Actuator dependency, this plugin will also generate
`git.properties`, which will be recognized by Spring Boot Actuator's
info endpoint.

Then just build your project:

```sh
$ ./gradlew build
```

Now your Manifest in JAR includes special attributes:

```sh
$ unzip -p build/libs/yourJar.jar META-INF/Manifest.MF
Manifest-Version: 1.0
Git-Branch: master
Git-Commit: 0794d59
Git-Committer-Date: 2015-12-15 22:07:12 +0900
Build-Date: 2015-12-15 22:10:55 +0900
```

And when Spring Boot Actuator is used with it, git.properties
will be generated:

```
$ cat build/resources/main/git.properties
#Tue Dec 15 22:10:55 JST 2015
git.commit.id=0794d59
git.commit.time=2015-12-15 22\:07\:12 +0900
git.branch=master
```

## Configuration

You can configure this plugin with `buildInfo` extension in build.gradle:

```gradle
buildInfo {
    // Date format string used to Git committer date.
    committerDateFormat 'yyyy-MM-dd HH:mm:ss Z'

    // Date format string used to build date.
    buildDateFormat 'yyyy-MM-dd HH:mm:ss Z'

    // Set to true if you want to generate/merge Manifest.MF.
    manifestEnabled true

    // Set to true if you want to generate git.properties.
    // Default is false, but when you use Spring Boot Actuator
    // and you don't set this property explicitly to false,
    // git.properties will be generated.
    gitPropertiesEnabled false
}
```

## License

    Copyright 2015 Soichiro Kashima

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
