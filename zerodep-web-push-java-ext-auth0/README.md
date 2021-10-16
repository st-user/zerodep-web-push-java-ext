# zerodep-web-push-java-ext-auth0

Provides an implementation for `com.zerodeplibs.webpush.jwt.VAPIDJWTGenerator`
utilizing <a href="https://github.com/auth0/java-jwt">Java JWT - auth0</a>.

## Requirements

The version of [com.auth0:java-jwt](https://mvnrepository.com/artifact/com.auth0/java-jwt) must be
3.14.0 or higher(the latest version is recommended).

## Usage

### pom.xml

You can use this sub-module by adding the dependency to your pom.xml.

```

TBD

```

### java

By calling `com.zerodeplibs.webpush.VAPIDKeyPairs#of(PrivateKeySource, PublicKeySource)`, the
implementation class provided by this sub-module is loaded automatically.

``` java

VAPIDKeyPairs.of(
    PrivateKeySources.of..... ,
    PublicKeySources.of.......
);

```

## Limitations

When creating a JWT by calling `VAPIDJWTGenerator#generate`, the claims in the token's payload are
specified by the given `com.zerodeplibs.webpush.jwt.VAPIDJWTParam` object.

Also, by using `VAPIDJWTParam.getBuilder#additionalClaim`, you can specify arbitrary claims like the
following.

``` java

VAPIDJWTParam param = VAPIDJWTParam.getBuilder()
    .resourceURLString("https://example.com")
    .expiresAfterSeconds(60)
    .subject("mailto:test@example.com")
    .additionalClaim("myArbitraryClaim", "valueOfTheClaim") // Specifys an arbitrary claim.
    .build();

String jwt = generator.generate(param);
.....
```

However, the underlying Auth0's library can support only `Map`, `List`, `Boolean`,
`Integer`, `Long`, `Double`, `String` and `Date` as a type of claim.

So the following example doesn't work.

``` java

VAPIDJWTParam param = VAPIDJWTParam.getBuilder()
    .resourceURLString("https://example.com")
    .expiresAfterSeconds(60)
    .subject("mailto:test@example.com")
    .additionalClaim("myArbitraryClaim", new MyClaim("...."))
    .build();

String jwt = generator.generate(param);// An exception will be thrown.

```

For more information, please consult the javadoc
of `com.auth0.jwt.JWTCreator.Builder#withPayload(java.util.Map<String, ?>)`.