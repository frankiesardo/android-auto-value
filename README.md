Android AutoValue
============

Port of Google AutoValue for the Android platform.

Why AutoValue?
--------

Because it's awesome.
If you read the [rationale](http://goo.gl/Ter394) you'll be convinced as well.

Ok then why an Android port?
--------

Two main reasons:

- Google Auto is a monolithic dependency that comes with a lot of libraries, some of them quite big (I'm looking at you Guava) potentially polluting your namespace and increasing apk size.
Android AutoValue splits the project in two libraries, one to be included in your apk (which just contains the interface) and one only used during compilation.

- Android AutoValue supports Parcelable generation.
That's right. One of the most verbose implementation in Android is now made as quick implementing `Serializable`.
Even quicker because you don't have to generate a `serialVersionUID`.
Just add `implements Parcelable` to your value objects and you're done.
This is by far the simplest and fastest way to generate `Parcelable`s on Android with zero reflection and completely transparent to the rest of your application.

Fine, how do I use it?
--------

```java
@AutoValue
abstract class SomeModel implements Parcelable {
  abstract String name();
  abstract List<SomeSubModel> subModels();
  abstract Map<String, OtherSubModel> modelsMap();

  static SomeModel create(String name, List<SomeSubModel> subModels, Map<String, OtherSubModel> modelsMap) {
    return new AutoValue_SomeModel(name, subModels, modelsMap);
  }
}
```

That's that simple. And you get `Parcelable`, `hashCode`, `equals` and `toString` implementations for free.
As your models evolve you don't need to worry about keeping all the boilerplate in sync with the new implementation, it's already taken care of.

Sounds great, where can I download it?
--------

The easy way is to use Gradle.

```groovy
buildscript {
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath 'com.android.tools.build:gradle:+'
  }
}

apply plugin: 'android'

dependencies {
  compile 'com.github.frankiesardo:android-auto-value:+'
  provided 'com.github.frankiesardo:android-auto-value-processor:+'
}
```

Check out the sample project for a working example.

License
-------

    Copyright 2014 Frankie Sardo
    Copyright 2013 Google, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
