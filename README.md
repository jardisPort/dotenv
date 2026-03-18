# Jardis DotEnv Port

![Build Status](https://github.com/jardisPort/dotenv/actions/workflows/ci.yml/badge.svg)
[![License: PolyForm Shield](https://img.shields.io/badge/License-PolyForm%20Shield-blue.svg)](LICENSE.md)
[![PHP Version](https://img.shields.io/badge/PHP-%3E%3D8.2-777BB4.svg)](https://www.php.net/)
[![PHPStan Level](https://img.shields.io/badge/PHPStan-Level%208-brightgreen.svg)](phpstan.neon)
[![PSR-12](https://img.shields.io/badge/Code%20Style-PSR--12-blue.svg)](phpcs.xml)

> Part of the **[Jardis Business Platform](https://jardis.io)** — Enterprise-grade PHP components for Domain-Driven Design

Environment loader contract with two loading modes — public (writes to `$_ENV`) and private (returns an isolated array). Type-hint against this interface to swap environment loading strategies without affecting your application code. The distinction between public and private loading gives you precise control over which values enter the global environment.

---

## Interfaces

### **DotEnvInterface**

`JardisPort\DotEnv\DotEnvInterface`

A two-mode contract for loading `.env` files. Public loading populates the system environment; private loading returns an isolated array that never touches global state.

| Method | Signature | Description |
|--------|-----------|-------------|
| `loadPublic` | `loadPublic(string $pathToEnvFiles): void` | Loads environment variables into `$_ENV` and the system environment. Throws `Exception` on failure. |
| `loadPrivate` | `loadPrivate(string $pathToEnvFiles): mixed` | Loads and returns environment variables as an array without writing to global state. Returns `null` if no variables are found. Throws `Exception` on failure. |

---

## Installation

```bash
composer require jardisport/dotenv
```

## Usage

```php
use JardisPort\DotEnv\DotEnvInterface;

// Type-hint against the interface — not against any specific implementation
class AppBootstrap
{
    public function __construct(
        private readonly DotEnvInterface $dotenv,
    ) {}

    public function boot(string $configPath): void
    {
        // Publicly loaded values become available via getenv() and $_ENV
        $this->dotenv->loadPublic($configPath);
    }
}

// Private loading — values stay isolated, never touch global state
class ServiceContainer
{
    public function __construct(
        private readonly DotEnvInterface $dotenv,
    ) {}

    public function loadSecrets(string $secretsPath): array
    {
        return $this->dotenv->loadPrivate($secretsPath) ?? [];
    }
}
```

## Implemented by

- **[jardissupport/dotenv](https://github.com/jardisSupport/dotenv)** — `.env` file parser with support for variable interpolation, type casting, and secret resolver integration

## Documentation

Full documentation, guides, and API reference:

**[jardis.io/docs/port/dotenv](https://jardis.io/docs/port/dotenv)**

## License

This package is licensed under the [PolyForm Shield License 1.0.0](LICENSE.md). Free for all use except building competing frameworks or developer tooling.

---

**[Jardis](https://jardis.io)** · [Documentation](https://jardis.io/docs) · [Headgent](https://headgent.com)
