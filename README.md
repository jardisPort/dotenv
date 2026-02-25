# JardisPort DotEnv Interface

![Build Status](https://github.com/jardisCore/dotenv/actions/workflows/ci.yml/badge.svg)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D8.2-blue.svg)](https://www.php.net/)
[![PHPStan Level](https://img.shields.io/badge/PHPStan-Level%208-success.svg)](phpstan.neon)
[![PSR-4](https://img.shields.io/badge/autoload-PSR--4-blue.svg)](https://www.php-fig.org/psr/psr-4/)
[![PSR-12](https://img.shields.io/badge/code%20style-PSR--12-orange.svg)](phpcs.xml)

A **contract-only PHP library** that defines the `DotEnvInterface` for Domain-Driven Design applications.

## Overview

This package provides exclusively an interface – **no implementation**. It serves as an abstraction layer for DotEnv functionality and distinguishes between:

- **Public environment variables** (system-wide, loaded into `$_ENV`)
- **Private environment variables** (application-specific, returned as an array)

## Installation

```bash
composer require jardisport/dotenv
```

## Interface

```php
namespace JardisPort\DotEnv;

interface DotEnvInterface
{
    /**
     * Loads public environment variables into the system environment ($_ENV, getenv)
     *
     * @param string $pathToEnvFiles Path to directory containing .env files
     * @throws Exception
     */
    public function loadPublic(string $pathToEnvFiles): void;

    /**
     * Loads and returns private environment variables as an array
     *
     * @param string $pathToEnvFiles Path to directory containing .env files
     * @return array<string, mixed>|null
     * @throws Exception
     */
    public function loadPrivate(string $pathToEnvFiles): mixed;
}
```

## Usage

Implement the interface in your own class:

```php
use JardisPort\DotEnv\DotEnvInterface;

class MyDotEnv implements DotEnvInterface
{
    public function loadPublic(string $pathToEnvFiles): void
    {
        // Implementation for loading public variables
    }

    public function loadPrivate(string $pathToEnvFiles): mixed
    {
        // Implementation for loading private variables
        return $privateVars;
    }
}
```

## Development

This project uses Docker for all development tasks. **Never run Composer, PHPStan, or PHPCS commands directly on the host** – always use the Makefile targets.

### Prerequisites

- Docker
- Docker Compose
- Make

### Available Commands

```bash
# Install dependencies
make install

# Update dependencies
make update

# Regenerate autoloader
make autoload

# Check code style (PSR-12)
make phpcs

# Run static analysis (PHPStan Level 8)
make phpstan

# Open container shell
make shell

# Clean up Docker resources
make remove
```

## Quality Standards

### PHP_CodeSniffer
- **Standard**: PSR-12
- **Line length**: 120 (limit), 150 (absolute limit)
- Strict types required: `declare(strict_types=1);`

### PHPStan
- **Level**: 8 (maximum strictness)
- **Analyzed paths**: `src/`

### Pre-Commit Hook

After installation, a Git pre-commit hook is automatically set up that:

1. Validates branch names: `(feature|fix|hotfix)/{1-7 digits}_{description}` or `:{7-40 hex chars}`
2. Validates Git username
3. Runs PHPCS on staged PHP files
4. Blocks commits on errors

## PHP Requirements

- **Minimum**: PHP 8.2
- **Recommended**: PHP 8.3

## Versioning

This project follows [Semantic Versioning](https://semver.org/). Since this is a contract, breaking changes are rare and only appear in major versions.

## License

MIT License - see [LICENSE](LICENSE) for details.

## Support

- **Issues**: [GitHub Issues](https://github.com/JardisPort/detenv/issues)
- **Email**: jardisCore@headgent.dev

## Authors

Jardis Core Development Team
