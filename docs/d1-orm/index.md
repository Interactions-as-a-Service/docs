---
title: D1 ORM
---

D1-ORM is a library for Cloudflare's D1 Database product.

Advantages:

- Dependency-free
- Strictly typed, see TypeScript hints for your tables in your editor of choice
- Object oriented - use classes to interact with your items
- No SQL knowledge required

## Installation
Use your preferred package manager to install the library, for example NPM.

=== "NPM"
	```
	npm install d1-orm
	```

=== "Yarn"
	```
	yarn add d1-orm
	```

=== "PNPM"
	```
	pnpm add d1-orm
	```

## Usage
Refer to the following guides for using the library.

- [`Models`](./models.md) - The simpler, recommended interface to querying with advantages such as automatically-generated types based on your table structure
- [`Query Building`](./query-building.md) - Advanced usage, not specific to D1
- [`Upserting`](./upserting.md) - What is it? How does it work?

## API Reference
Refer to [the Typedoc site](https://orm.interactions.rest/modules)
