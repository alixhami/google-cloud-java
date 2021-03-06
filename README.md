Google Cloud Client Library for Java
==========================

Java idiomatic client for [Google Cloud Platform][cloud-platform] services.

[![CircleCI](https://circleci.com/gh/GoogleCloudPlatform/google-cloud-java/tree/master.svg?style=shield)](https://circleci.com/gh/GoogleCloudPlatform/google-cloud-java/tree/master)
[![Coverage Status](https://coveralls.io/repos/GoogleCloudPlatform/google-cloud-java/badge.svg?branch=master)](https://coveralls.io/r/GoogleCloudPlatform/google-cloud-java?branch=master)
[![Maven](https://img.shields.io/maven-central/v/com.google.cloud/google-cloud.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.google.cloud%22%20a%3A%22google-cloud%22)
[![Codacy Badge](https://api.codacy.com/project/badge/grade/9da006ad7c3a4fe1abd142e77c003917)](https://www.codacy.com/app/mziccard/google-cloud-java)
[![Dependency Status](https://www.versioneye.com/user/projects/58fe4c8d6ac171426c414772/badge.svg?style=flat)](https://www.versioneye.com/user/projects/58fe4c8d6ac171426c414772)

- [Google Cloud Platform Documentation][cloud-platform-docs]
- [Client Library Documentation][client-lib-docs]

This library supports the following Google Cloud Platform services with clients at a [GA](#versioning) quality level:
-  [BigQuery](google-cloud-bigquery) (GA)
-  [Stackdriver Logging](google-cloud-logging) (GA)
-  [Cloud Datastore](google-cloud-datastore) (GA)
-  [Cloud Natural Language](google-cloud-language) (GA)
-  [Cloud Storage](google-cloud-storage) (GA)
-  [Cloud Translation](google-cloud-translate) (GA)
-  [Cloud Vision](google-cloud-vision) (GA)

This library supports the following Google Cloud Platform services with clients at a [Beta](#versioning) quality level:

-  [Cloud Data Loss Prevention](google-cloud-dlp) (Beta)
-  [Stackdriver Error Reporting](google-cloud-errorreporting) (Beta)
-  [Cloud Firestore](google-cloud-firestore) (Beta)
-  [Stackdriver Monitoring](google-cloud-monitoring) (Beta)
-  [Cloud Pub/Sub](google-cloud-pubsub) (Beta)
-  [Cloud Spanner](google-cloud-spanner) (Beta)
-  [Cloud Video Intelligence](google-cloud-video-intelligence) (Beta)
-  [Stackdriver Trace](google-cloud-trace) (Beta)
-  [Text-to-Speech](google-cloud-texttospeech) (Beta)

This library supports the following Google Cloud Platform services with clients at an [Alpha](#versioning) quality level:

-  [Cloud Dataproc](google-cloud-dataproc) (Alpha)
-  [Cloud DNS](google-cloud-dns) (Alpha)
-  [Cloud OS Login](google-cloud-os-login) (Alpha)
-  [Cloud Resource Manager](google-cloud-resourcemanager) (Alpha)
-  [Cloud Speech](google-cloud-speech) (Alpha)
-  [Dialogflow](google-cloud-dialogflow) (Alpha)

These libraries are deprecated and no longer receive updates:

-  [Cloud Compute](google-cloud-compute) (Deprecated)

Quickstart
----------

The easy way to get started is to add the umbrella package which pulls in all of the supported clients as
dependencies. Note that even though the version of the umbrella package is Alpha, the individual clients are
at different support levels (Alpha, Beta, and GA).

[//]: # ({x-version-update-start:google-cloud:released})
If you are using Maven, add this to your pom.xml file
```xml
<dependency>
  <groupId>com.google.cloud</groupId>
  <artifactId>google-cloud</artifactId>
  <version>0.44.0-alpha</version>
</dependency>
```
If you are using Gradle, add this to your dependencies
```Groovy
compile 'com.google.cloud:google-cloud:0.44.0-alpha'
```
If you are using SBT, add this to your dependencies
```Scala
libraryDependencies += "com.google.cloud" % "google-cloud" % "0.44.0-alpha"
```
[//]: # ({x-version-update-end})

It also works just as well to declare a dependency only on the specific clients that you need. See the README of
each client for instructions.

If you're using IntelliJ or Eclipse, you can add client libraries to your project using these IDE plugins: 
* [Cloud Tools for IntelliJ](https://cloud.google.com/tools/intellij/docs/client-libraries)
* [Cloud Tools for Eclipse](https://cloud.google.com/eclipse/docs/libraries)

Besides adding client libraries, the plugins provide additional functionality, such as service account key management. Refer to the documentation for each plugin for more details.

These client libraries can be used on App Engine standard for Java 8 runtime, App Engine flexible (including the Compat runtime).  Most of the libraries do not work on the App Engine standard for Java 7 runtime, however, Datastore, Storage, and Bigquery should work.

If you are running into problems with version conflicts, see [Version Management](#version-management).

Specifying a Project ID
-----------------------

Most `google-cloud` libraries require a project ID.  There are multiple ways to specify this project ID.

1. When using `google-cloud` libraries from within Compute/App Engine, there's no need to specify a project ID.  It is automatically inferred from the production environment.
2. When using `google-cloud` elsewhere, you can do one of the following:
  * Supply the project ID when building the service options.  For example, to use Datastore from a project with ID "PROJECT_ID", you can write:

  ```java
  Datastore datastore = DatastoreOptions.newBuilder().setProjectId("PROJECT_ID").build().getService();
  ```
  * Specify the environment variable `GOOGLE_CLOUD_PROJECT` to be your desired project ID.
  * Set the project ID using the [Google Cloud SDK](https://cloud.google.com/sdk/?hl=en).  To use the SDK, [download the SDK](https://cloud.google.com/sdk/?hl=en) if you haven't already, and set the project ID from the command line.  For example:

  ```
  gcloud config set project PROJECT_ID
  ```

`google-cloud` determines the project ID from the following sources in the listed order, stopping once it finds a value:

1. The project ID supplied when building the service options
2. Project ID specified by the environment variable `GOOGLE_CLOUD_PROJECT`
3. The App Engine project ID
4. The project ID specified in the JSON credentials file pointed by the `GOOGLE_APPLICATION_CREDENTIALS` environment variable
5. The Google Cloud SDK project ID
6. The Compute Engine project ID

In cases where the library may expect a project ID explicitly, we provide a helper that can provide the inferred project ID:
   ```java
     import com.google.cloud.ServiceOptions;
     ...
     String projectId = ServiceOptions.getDefaultProjectId();
   ```

Authentication
--------------

`google-cloud-java` uses
[https://github.com/google/google-auth-library-java](https://github.com/google/google-auth-library-java)
to authenticate requests. `google-auth-library-java` supports a wide range of authentication types;
see the project's [README](https://github.com/google/google-auth-library-java/blob/master/README.md)
and [javadoc](http://google.github.io/google-auth-library-java/releases/0.6.0/apidocs/) for more
details.

To access Google Cloud services, you first need to ensure that the necessary Google Cloud APIs are
enabled for your project. To do this, follow the instructions on the
[authentication document](https://github.com/GoogleCloudPlatform/gcloud-common/blob/master/authentication/readme.md#authentication)
shared by all the Google Cloud language libraries.

Next, choose a method for authenticating API requests from within your project:

1. When using `google-cloud` libraries from within Compute/App Engine, no additional authentication
steps are necessary. For example:
```java
Storage storage = StorageOptions.getDefaultInstance().getService();
```
2. When using `google-cloud` libraries elsewhere, there are several options:
  * [Generate a JSON service account key](https://cloud.google.com/storage/docs/authentication?hl=en#service_accounts).
  After downloading that key, you must do one of the following:
    * Define the environment variable GOOGLE_APPLICATION_CREDENTIALS to be the location of the key.
    For example:
    ```bash
    export GOOGLE_APPLICATION_CREDENTIALS=/path/to/my/key.json
    ```
    * Supply the JSON credentials file when building the service options. For example, this Storage
    object has the necessary permissions to interact with your Google Cloud Storage data:
    ```java
    Storage storage = StorageOptions.newBuilder()
        .setCredentials(ServiceAccountCredentials.fromStream(new FileInputStream("/path/to/my/key.json")))
        .build()
        .getService();
    ```
  * If running locally for development/testing, you can use the
  [Google Cloud SDK](https://cloud.google.com/sdk/). Create Application Default Credentials with
  `gcloud auth application-default login`, and then `google-cloud` will automatically detect such
  credentials.
  * If you already have an OAuth2 access token, you can use it to authenticate (notice that in this
  case, the access token will not be automatically refreshed):
  ```java
  Storage storage = StorageOptions.newBuilder()
      .setCredentials(new GoogleCredentials(new AccessToken(accessToken, expirationTime)))
      .build()
      .getService();
  ```

If no credentials are provided, `google-cloud` will attempt to detect them from the environment
using `GoogleCredentials.getApplicationDefault()` which will search for Application Default
Credentials in the following locations (in order):

1. The credentials file pointed to by the `GOOGLE_APPLICATION_CREDENTIALS` environment variable
2. Credentials provided by the Google Cloud SDK `gcloud auth application-default login` command
3. Google App Engine built-in credentials
4. Google Cloud Shell built-in credentials
5. Google Compute Engine built-in credentials

Troubleshooting
---------------

To get help, follow the instructions in the [Troubleshooting document](https://github.com/GoogleCloudPlatform/google-cloud-java/blob/master/TROUBLESHOOTING.md).

Using a proxy
-------------
Clients in this repository use either HTTP or gRPC for the transport layer.
The README of each client documents the transport layer the client uses.

For HTTP clients, a proxy can be configured by using `http.proxyHost` and
related system properties as documented by
[Java Networking and Proxies](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html).

For gRPC clients, a proxy can be configured by using the
`GRPC_PROXY_EXP` environment variable as documented by
the gRPC [release notes](https://github.com/grpc/grpc-java/releases/tag/v1.0.3).
Please note that gRPC proxy support is currently experimental.

Java Versions
-------------

Java 7 or above is required for using the clients in this repository.

Supported Platforms
-------------------

Clients in this repository use either HTTP or gRPC for the transport layer. All
HTTP-based clients should work in all environments.

For clients that use gRPC, the supported platforms are constrained by the platforms
that [Forked Tomcat Native](http://netty.io/wiki/forked-tomcat-native.html) supports,
which for architectures means only x86_64, and for operating systems means Mac OS X,
Windows, and Linux. Additionally, gRPC constrains the use of platforms with
threading restrictions.

Thus, the following are not supported:

- Android
- Alpine Linux (due to netty-tcnative requiring glibc, which is not present on Alpine)
- Raspberry Pi (since it runs on the ARM architecture)
- Google App Engine Standard Java 7

The following environments should work (among others):

- standalone Windows on x86_64
- standalone Mac OS X on x86_64
- standalone Linux on x86_64
- Google Compute Engine (GCE)
- Google Container Engine (GKE)
- Google App Engine Standard Java 8 (GAE Std J8)
- Google App Engine Flex (GAE Flex)

Testing
-------

This library provides tools to help write tests for code that uses google-cloud services.

See [TESTING] to read more about using our testing helpers.

Versioning
----------

This library follows [Semantic Versioning](http://semver.org/), but with some
additional qualifications:

1. Components marked with `@BetaApi` are considered to be "0.x" features inside
   a "1.x" library. This means they can change between minor and patch releases
   in incompatible ways. These features should not be used by any library "B"
   that itself has consumers, unless the components of library B that use
   `@BetaApi` features are also marked with `@BetaApi`. Features marked as
   `@BetaApi` are on a path to eventually become "1.x" features with the marker
   removed.

   **Special exception for google-cloud-java**: google-cloud-java is
   allowed to depend on `@BetaApi` features in gax-java without declaring the consuming
   code `@BetaApi`, because gax-java and google-cloud-java move in step
   with each other. For this reason, gax-java should not be used
   independently of google-cloud-java.

1. Components marked with `@InternalApi` are technically public, but are only
   public for technical reasons, because of the limitations of Java's access
   modifiers. For the purposes of semver, they should be considered private.

Please note it is currently under active development. Any release versioned 0.x.y is
subject to backwards incompatible changes at any time.

**GA**: Libraries defined at a GA quality level are expected to be stable and all updates in the
libraries are guaranteed to be backwards-compatible. Any backwards-incompatible changes will lead
to the major version increment (1.x.y -> 2.0.0).

**Beta**: Libraries defined at a Beta quality level are expected to be mostly stable and
we're working towards their release candidate. We will address issues and requests with
a higher priority.

**Alpha**: Libraries defined at an Alpha quality level are still a work-in-progress and
are more likely to get backwards-incompatible updates. Additionally, it's possible for Alpha
libraries to get deprecated and deleted before ever being promoted to Beta or GA.

Version Management
------------------

The easiest way to solve version conflicts is to use google-cloud's BOM. In Maven, add the following to your POM:

[//]: # ({x-version-update-start:google-cloud-bom:released})
```xml
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>com.google.cloud</groupId>
        <artifactId>google-cloud-bom</artifactId>
        <version>0.44.0-alpha</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
```
[//]: # ({x-version-update-end})

This BOM is only available starting at version 0.32.0-alpha. If you are having problems with prior versions of
google-cloud, use the following table as a reference to make sure that your versions are compatible. Definitions:

* **alpha**: The version of any alpha package in google-cloud
* **beta**: The version of any beta package in google-cloud
* **GA**: The version of any GA package in google-cloud
* **gax**: The version of com.google.api:gax
* **gax-grpc**: The version of com.google.api:gax-grpc

Something to be aware of is that a package can be promoted from alpha -> beta or beta -> GA between versions, which
means that after a certain point for any given package, the alpha or beta version won't be valid any more.

alpha         | beta         | GA         | gax        | gax-grpc
------------- | ------------ | ---------- | ---------- | --------
0.30.0-alpha  | 0.30.0-beta  | 1.12.0     | 1.15.0     | 1.15.0
0.29.0-alpha  | 0.29.0-beta  | 1.11.0     | 1.15.0     | 1.15.0
0.28.0-alpha  | 0.28.0-beta  | 1.10.0     | 1.14.0     | 1.14.0
0.27.0-alpha  | 0.27.0-beta  | 1.9.0      | 1.13.0     | 0.30.0
0.26.0-alpha  | 0.26.0-beta  | 1.8.0      | 1.9.0      | 0.26.0
0.25.0-alpha  | 0.25.0-beta  | 1.7.0      | 1.8.1      | 0.25.1
0.24.0-alpha  | 0.24.0-beta  | 1.6.0      | 1.8.1      | 0.25.1
0.23.1-alpha  | 0.23.1-beta  | 1.5.1      | 1.8.1      | 0.25.1
0.23.0-alpha  | 0.23.0-beta  | 1.5.0      | 1.5.0      | 0.22.0
0.22.0-alpha  | 0.22.0-beta  | 1.4.0      | 1.5.0      | 0.22.0
0.21.1-alpha  | 0.21.1-beta  | 1.3.1      | 1.5.0      | 0.22.0
0.21.0-alpha  | 0.21.0-beta  | 1.3.0      | 1.5.0      | 0.22.0
0.20.3-alpha  | 0.20.3-beta  | 1.2.3      | 1.4.2      | 0.21.2
0.20.2-alpha  | 0.20.2-beta  | 1.2.2      | 1.4.2      | 0.21.2
0.20.1-alpha  | 0.20.1-beta  | 1.2.1      | 1.4.1      | 0.21.1
0.20.0-alpha  | 0.20.0-beta  | 1.2.0      | 1.3.1      | 0.20.0
0.19.0-alpha  | 0.19.0-beta  | 1.1.0      | 1.3.0      | 0.19.0
0.18.0-alpha  | 0.18.0-beta  | 1.0.2      | 1.1.0      | 0.17.0
0.17.2-alpha  | 0.17.2-beta  | 1.0.1      | 1.0.0      | 0.16.0
0.17.1-alpha  | 0.17.1-beta  | 1.0.0      | 1.0.0      | 0.16.0
0.17.0-alpha  | 0.17.0-beta  | 1.0.0-rc4  | 1.0.0-rc1  | 0.15.0

Contributing
------------

Contributions to this library are always welcome and highly encouraged.

See `google-cloud`'s [CONTRIBUTING] documentation and the [shared documentation](https://github.com/GoogleCloudPlatform/gcloud-common/blob/master/contributing/readme.md#how-to-contribute-to-gcloud) for more information on how to get started.

Please note that this project is released with a Contributor Code of Conduct. By participating in this project you agree to abide by its terms. See [Code of Conduct][code-of-conduct] for more information.

License
-------

Apache 2.0 - See [LICENSE] for more information.


[CONTRIBUTING]:https://github.com/GoogleCloudPlatform/google-cloud-java/blob/master/CONTRIBUTING.md
[code-of-conduct]:https://github.com/GoogleCloudPlatform/google-cloud-java/blob/master/CODE_OF_CONDUCT.md#contributor-code-of-conduct
[LICENSE]: https://github.com/GoogleCloudPlatform/google-cloud-java/blob/master/LICENSE
[TESTING]: https://github.com/GoogleCloudPlatform/google-cloud-java/blob/master/TESTING.md

[cloud-platform]: https://cloud.google.com/
[cloud-platform-docs]: https://cloud.google.com/docs/
[client-lib-docs]: http://googlecloudplatform.github.io/google-cloud-java/latest/apidocs/
