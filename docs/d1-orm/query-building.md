---
title: Using the Query Builder
---

This guide assumes that you have basic SQL knowledge to understand the types of queries that are made. Some examples will be used to demonstrate usage - in this case, we'll use a "Users" table.

## Getting started

We need to import the GenerateQuery method from the library, which will be used to create the SQL for you.

```ts
import { GenerateQuery, QueryType } from 'd1-orm';
```

??? tip "When Using TypeScript"
	You should create a type for each table, which will be used to restrict the types of your queries for you. When using [Models](./models.md), this can be automatically done for you.

	An example of this would be

	```ts
	type User = {
		id: number;
		name: string;
		is_admin: boolean;
		email: string | null; // in this case, the other columns are NOT NULLable, however this one is
	};
	```

## Creating Queries

The query builder is not specific to D1, so it will not actually run the queries for you, but instead return an object containing `#!ts query: string` and `#!ts bindings:unknown[]`.

### Selects

The following options are available:

- where
- orderBy
- limit
- offset (*requires limit)

For example:

=== "TypeScript"
	```ts linenums="1"
	const result = GenerateQuery<User>(QueryType.SELECT, 'users', // The name of the table
		{
			where: {
				name: "John Doe"
			}, //this uses the type from above to enforce it to properties which exist on the table
			limit: 1 //we only want the first user
			offset: 3 // skip the first three users named 'John Doe' when performing this query

			// Using orderBy is a special case, so there's a few possible syntaxes for it
			orderBy: 'id', // ORDER BY id 
			orderBy: ['id', 'name'], // ORDER BY (id, name)
			orderBy: { column: 'id', descending: true, nullLast: true }, // ORDER BY id DESC NULLS LAST
			orderBy: [{ column: 'id', descending: true, nullLast: true }, { column: 'name', descending: false, nullLast: false }], // ORDER BY (id DESC NULLS LAST, name)
		}
	)
	// This will return something like the following
	{
		query: 'SELECT * FROM users WHERE name = ? LIMIT 1 OFFSET 3 ORDER BY ...',
		bindings: ["John Doe"]
	}
	```
=== "JavaScript"
	```js linenums="1"
	const result = GenerateQuery(QueryType.SELECT, 'users', // The name of the table
		{
			where: {
				name: "John Doe"
			},
			limit: 1 //we only want the first user
			offset: 3 // skip the first three users named 'John Doe' when performing this query

			// Using orderBy is a special case, so there's a few possible syntaxes for it
			orderBy: 'id', // ORDER BY id 
			orderBy: ['id', 'name'], // ORDER BY (id, name)
			orderBy: { column: 'id', descending: true, nullLast: true }, // ORDER BY id DESC NULLS LAST
			orderBy: [{ column: 'id', descending: true, nullLast: true }, { column: 'name', descending: false, nullLast: false }], // ORDER BY (id DESC NULLS LAST, name)
		}
	)
	// This will return something like the following
	{
		query: 'SELECT * FROM users WHERE name = ? LIMIT 1 OFFSET 3 ORDER BY ...',
		bindings: ["John Doe"]
	}
	```

### Deleting
The `where` option is the only one available here.

For example:

=== "TypeScript"
	```ts linenums="1"
	const result = GenerateQuery<User>(QueryType.DELETE, 'users',
		{
			where: {
				name: "John Doe"
			},
		}
	)
	// This will return something like the following
	{
		query: 'DELETE FROM users WHERE name = ?',
		bindings: ["John Doe"]
	}
	```
=== "JavaScript"
	```js linenums="1"
	const result = GenerateQuery(QueryType.DELETE, 'users',
		{
			where: {
				name: "John Doe"
			},
		}
	)
	// This will return something like the following
	{
		query: 'DELETE FROM users WHERE name = ?',
		bindings: ["John Doe"]
	}
	```

### Insert/Insert Or Replace
The `data` option is *required* here.

For example:

=== "TypeScript"
	=== "Insert"
		```ts linenums="1"
		const result = GenerateQuery<User>(QueryType.INSERT, 'users',
			{
				data: {
					name: "John Doe",
					is_admin: false,
				},
			}
		)
		// This will return something like the following
		{
			query: 'INSERT INTO users (name, is_admin) VALUES (?, ?)',
			bindings: ["John Doe", false]
		}
		```
	=== "Insert Or Replace"
		```ts linenums="1"
		const result = GenerateQuery<User>(QueryType.INSERT_OR_REPLACE, 'users',
			{
				data: {
					name: "John Doe",
					is_admin: false,
				},
			}
		)
		// This will return something like the following
		{
			query: 'INSERT or REPLACE INTO users (name, is_admin) VALUES (?, ?)',
			bindings: ["John Doe", false]
		}
		```


=== "JavaScript"
	=== "Insert"
		```js linenums="1"
		const result = GenerateQuery(QueryType.INSERT, 'users',
			{
				data: {
					name: "John Doe",
					is_admin: false,
				},
			}
		)
		// This will return something like the following
		{
			query: 'INSERT INTO users (name, is_admin) VALUES (?, ?)',
			bindings: ["John Doe", false]
		}
		```
	=== "Insert Or Replace"
		```js linenums="1"
		const result = GenerateQuery(QueryType.INSERT_OR_REPLACE, 'users',
			{
				data: {
					name: "John Doe",
					is_admin: false,
				},
			}
		)
		// This will return something like the following
		{
			query: 'INSERT or REPLACE INTO users (name, is_admin) VALUES (?, ?)',
			bindings: ["John Doe", false]
		}
		```
	

### Updating
The `data` option is *required* here, the `where` option is optional.

For example:

=== "TypeScript"
	```ts linenums="1"
	const result = GenerateQuery<User>(QueryType.UPDATE, 'users',
		{
			data: {
				is_admin: false,
			},
			where: {
				name: "John Doe"
			}
		}
	)
	// This will return something like the following
	{
		query: 'UPDATE users SET is_admin = ? WHERE name = ?',
		bindings: [false, "John Doe"]
	}
	```

=== "JavaScript"
	```js linenums="1"
	const result = GenerateQuery(QueryType.UPDATE, 'users',
		{
			data: {
				is_admin: false,
			},
			where: {
				name: "John Doe"
			}
		}
	)
	// This will return something like the following
	{
		query: 'UPDATE users SET is_admin = ? WHERE name = ?',
		bindings: [false, "John Doe"]
	}
	```

### Upserting

Refer to [the Upserting Guide](./upserting.md) to understand the options for this.
