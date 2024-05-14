# Javet Sanitizer

# This project is archived. All features are moved to [swc4j](https://github.com/caoccao/swc4j). Please review [Hello Swc4j, Goodbye Antlr](https://blog.caoccao.com/hello-swc4j-goodbye-antlr-f9a63e45a3d4) for more details.

[![Maven Central](https://img.shields.io/maven-central/v/com.caoccao.javet.sanitizer/javet-sanitizer?style=for-the-badge)](https://central.sonatype.com/search?q=g:com.caoccao.javet.sanitizer) [![Discord](https://img.shields.io/discord/870518906115211305?label=join%20our%20Discord&style=for-the-badge)](https://discord.gg/R4vvKU96gw) [![Donate](https://img.shields.io/badge/Donate-green?style=for-the-badge)](https://opencollective.com/javet)

[![Build](https://github.com/caoccao/JavetSanitizer/actions/workflows/multiple_build.yml/badge.svg)](https://github.com/caoccao/JavetSanitizer/actions/workflows/multiple_build.yml)

Javet Sanitizer is a sanitizer framework for parsing and validating JavaScript code on JVM. It is built on top of [antlr4](https://github.com/antlr/antlr4) and [grammars-v4](https://github.com/antlr/grammars-v4).

Javet Sanitizer provides a set of rich checkers at AST level for [Javet](https://github.com/caoccao/Javet) so that applications can address and eliminate the potential threats before the JavaScript code is executed.

## Why do I need to sanitize the JavaScript code?

A script engine like Javet can be shared by multiple scripts, however one script may tamper the script engine to hack the next script to be executed. For instance, the built-in `JSON` can be hijacked so that `stringify`, `parse` may work improperly during the JSON serialization or deserialization.

Javet Sanitizer is designed to protect the script engine from that kind of attacks.

## Why not use Babel?

- Babel is too slow.
- Babel AST cannot be easily imported to JVM.

## Features

- AST Pattern Matching
- [Built-in Object Protection](docs/features/built_in_object_protection.md)
- [Keyword Restriction](docs/features/keyword_restriction.md)
- [Function Restriction](docs/features/function_restriction.md)
- [Identifier Restriction](docs/features/identifier_restriction.md)
- [Identifier Deletion](docs/features/identifier_deletion.md)
- [Identifier Freeze](docs/features/identifier_freeze.md)
- [Identifier Naming Convention](docs/features/identifier_naming_convention.md)
- Complete Customization

## Quick Start

- Follow the [installation](docs/installation.md) to set up the project dependency.
- Create a Java [file](src/test/java/com/caoccao/javet/sanitizer/tutorials/TutorialQuickStart.java) as follows.

```java
public static void main(String[] args) {
    JavetSanitizerStatementListChecker checker = new JavetSanitizerStatementListChecker();

    // 1. Check if keyword const can be used.
    String codeString = "const a = 1;";
    try {
        checker.check(codeString);
        System.out.println("1. " + codeString + " // Valid.");
    } catch (JavetSanitizerException ignored) {
    }

    // 2. Check if keyword var can be used.
    codeString = "var a = 1;";
    try {
        checker.check(codeString);
    } catch (JavetSanitizerException e) {
        System.out.println("2. " + codeString + " // Invalid: " + e.getMessage());
    }

    // 3. Check if Object is mutable.
    codeString = "Object = {};";
    try {
        checker.check(codeString);
    } catch (JavetSanitizerException e) {
        System.out.println("3. " + codeString + " // Invalid: " + e.getMessage());
    }
}
```

- The output is as follows.

```java
1. const a = 1; // Valid.
2. var a = 1; // Invalid: Keyword var is not allowed.
3. Object = {}; // Invalid: Identifier Object is not allowed.
```

## Blog

- [Hello Swc4j, Goodbye Antlr](https://blog.caoccao.com/hello-swc4j-goodbye-antlr-f9a63e45a3d4)
- [How to Compromise V8 on JVM](https://sjtucaocao.medium.com/how-to-compromise-v8-on-jvm-ceb385572461)

## Document

- [Installation](docs/installation.md)
- [Tutorials](docs/tutorials/)
- [Development](docs/development.md)
- [Errors](docs/errors.md)
- [Release Notes](docs/release-notes.md)

## License

[APACHE LICENSE, VERSION 2.0](LICENSE)
