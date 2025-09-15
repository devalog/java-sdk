# Devintest Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Fdevalog%2Fjava-sdk)
[![Maven Central](https://img.shields.io/maven-central/v/io.github.devalog/sdk-name-test)](https://central.sonatype.com/artifact/io.github.devalog/sdk-name-test)

Welcome to my API

## Installation

### Gradle

Add the dependency in your `build.gradle` file:

```groovy
dependencies {
  implementation 'io.github.devalog:sdk-name-test'
}
```

### Maven

Add the dependency in your `pom.xml` file:

```xml
<dependency>
  <groupId>io.github.devalog</groupId>
  <artifactId>sdk-name-test</artifactId>
  <version>0.0.2</version>
</dependency>
```

## Reference

A full reference for this library is available [here](https://github.com/devalog/java-sdk/blob/HEAD/./reference.md).

## Usage

Instantiate and use the client with the following:

```java
package com.example.usage;

import com.devintest.api.DevintestApiClient;
import com.devintest.api.resources.imdb.types.CreateMovieRequest;

public class Example {
    public static void main(String[] args) {
        DevintestApiClient client = DevintestApiClient
            .builder()
            .build();

        client.imdb().createMovie(
            CreateMovieRequest
                .builder()
                .title("title")
                .rating(1.1)
                .build()
        );
    }
}
```

## Base Url

You can set a custom base URL when constructing the client.

```java
import com.devintest.api.DevintestApiClient;

DevintestApiClient client = DevintestApiClient
    .builder()
    .url("https://example.com")
    .build();
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), an API exception will be thrown.

```java
import com.devintest.api.core.DevintestApiApiException;

try {
    client.imdb().createMovie(...);
} catch (DevintestApiApiException e) {
    // Do something with the API exception...
}
```

## Advanced

### Custom Client

This SDK is built to work with any instance of `OkHttpClient`. By default, if no client is provided, the SDK will construct one. 
However, you can pass your own client like so:

```java
import com.devintest.api.DevintestApiClient;
import okhttp3.OkHttpClient;

OkHttpClient customClient = ...;

DevintestApiClient client = DevintestApiClient
    .builder()
    .httpClient(customClient)
    .build();
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retryable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2).

A request is deemed retryable when any of the following HTTP status codes is returned:

- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) (Internal Server Errors)

Use the `maxRetries` client option to configure this behavior.

```java
import com.devintest.api.DevintestApiClient;

DevintestApiClient client = DevintestApiClient
    .builder()
    .maxRetries(1)
    .build();
```

### Timeouts

The SDK defaults to a 60 second timeout. You can configure this with a timeout option at the client or request level.

```java
import com.devintest.api.DevintestApiClient;
import com.devintest.api.core.RequestOptions;

// Client level
DevintestApiClient client = DevintestApiClient
    .builder()
    .timeout(10)
    .build();

// Request level
client.imdb().createMovie(
    ...,
    RequestOptions
        .builder()
        .timeout(10)
        .build()
);
```

### Custom Headers

The SDK allows you to add custom headers to requests. You can configure headers at the client level or at the request level.

```java
import com.devintest.api.DevintestApiClient;
import com.devintest.api.core.RequestOptions;

// Client level
DevintestApiClient client = DevintestApiClient
    .builder()
    .addHeader("X-Custom-Header", "custom-value")
    .addHeader("X-Request-Id", "abc-123")
    .build();
;

// Request level
client.imdb().createMovie(
    ...,
    RequestOptions
        .builder()
        .addHeader("X-Request-Header", "request-value")
        .build()
);
```
