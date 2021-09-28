# Gradle dependency verification key caching issue repro

Note this set up uses `gradle/verification-metadata.xml` and
`gradle/verification-keyring.keys` to enable fully offline signature
verification.

1. Run `./gradlew jar`, you'll see success
2. `rm gradle/verification-keyring.keys`
3. Run `./gradlew jar`

## Expected

Step 3 fails, because we set `<key-servers enabled="false"/>` but
there is no offline keyring.

## Actual

Step 3 succeeds.

Even running `./gradlew jar --rerun-tasks --refresh-keys --refresh-dependencies --no-build-cache`
passes. `./gradlew --stop` also has no effect.

The only workaround I have found so far is to specify a custom `GRADLE_USER_HOME`,
for example `GRADLE_USER_HOME=/tmp/foo ./gradlew jar` -> this will fail.