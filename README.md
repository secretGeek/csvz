# csvz

`csvz` is the hot new open database standard that is taking the entire technological world by storm.

A `csvz` file is literally just a bunch of `csv` files, in a zip file, that has been renamed to have a ".csvz" file extension.

-----

> Are you using `csvz` ? Why not? `csvz` is the brave technology that unites the worlds of data science, sql and no-sql. Is it no-sql's answer to the rdbms? Or is it the rdbms answer to no-sql? You decide.

-----

## Contents

- [The csvz specification](#the-csvz-specification)
  - [`csvz-0` A csvz file is literally just a bunch of `csv` files, in a zip file with a file name that ends with ".csvz"](#csvz-0-a-csvz-file-is-literally-just-a-bunch-of-csv-files-in-a-zip-file-with-a-file-name-that-ends-with-csvz)
  - [`csvz-meta-tables` A csvz file can contain a file called `tables.csv` describing the contents of the file](#csvz-meta-tables-a-csvz-file-can-contain-a-file-called-tablescsv-describing-the-contents-of-the-file)
  - [`csvz-meta-columns` A csvz file can contain a file called `columns.csv`](#csvz-meta-columns-a-csvz-file-can-contain-a-file-called-columnscsv)
  - [`csvz-meta-relations` A csvz file can contain a file called `relationships.csv`](#csvz-meta-relations-a-csvz-file-can-contain-a-file-called-relationshipscsv)
  - [Unwritten meta fragments](#unwritten-meta-fragments)
- [A list of `csvz-compliant` Tools and Libraries](#a-list-of-csvz-compliant-tools-and-libraries)
- [Contribute](#contribute)
- [License](#license)

## The csvz specification

The `csvz` specification is broken into meaningful fragments.

Files can call themselves `csvz-compliant` if they only comply with the first fragment of the specification, [`csvz-0`](#csvz-0-a-csvz-file-is-literally-just-a-bunch-of-csv-files-in-a-zip-file-with-a-file-name-that-ends-with-csvz).

They can also indicate other fragments of the specification that they have implemented, such as [`csvz-meta-tables`](#csvz-meta-tables-a-csvz-file-can-contain-a-file-called-tablescsv-describing-the-contents-of-the-file), [`csv-meta-relations`](#csvz-meta-relations-a-csvz-file-can-contain-a-file-called-relationships.csv) etc.

-----

### `csvz-0` A csvz file is literally just a bunch of `csv` files, in a zip file with a file name that ends with ".csvz"

A csvz file is compliant with `csvz-0` if it is literally just a bunch of `csv` files, in a zip file, that has been renamed to have a ".csvz" file extension.

The `csv` files themselves should comply with [`RFC 4180`](https://github.com/secretGeek/AwesomeCSV#standards).

(Anywhere that this spec refers to "a csv file" it means a file that complies with `RFC 4180`.) (at the very least... other more specific csv format details will be covered in a later spec fragment).

(Anywhere that `the csvz specification` refers to "this spec" it means `the csvz specification`.)

-----

### **`csvz-meta-tables`** A `csvz` file can contain a file called `tables.csv` describing the contents of the file

Metadata about the contents of the csvz file is contained in a directory called "_meta". The file `tables.csv`, if present, is inside this directory.

The file `tables.csv` contains metadata about all of the csv files included in the `csvz` file.

(The file `tables.csv` is a csv file.)

(Anywhere that this spec refers to a file with a name that ends with ".csv" it means the file is a "csv file", as described in [`csvz-0`](#csvz-0-a-csvz-file-is-literally-just-a-bunch-of-csv-files-in-a-zip-file-with-a-file-name-that-ends-with-csvz).)

The file `tables.csv` meets the following description:

- There is a header row naming the columns in this file
- Each data row describes a different csv file within this `csvz` file
- The columns must include a column called "filename"
- There may be more columns. Some suggestions (not required for this specification)
  - `bytes` - the size of the file in bytes
  - `rows` - the number of rows in the file
  - `columns` - the number of columns in the file
  - `description` - a description of the file
  - `published` - the date the data in the file was first published
  - `source` - information about the source of the data in the file
  - `has-column-names` - a `true/false` value indicating if the file has a header row containing column names.
  - `skip-rows` - How many rows need to be skipped, before the data begins? (Rarely need to specify this, but when you need it, you need it!)
- The file `tables.csv` may also describe itself. See [Russell](http://wiki.secretgeek.net/paradox#bertrand-russell-making-life-impossible-for-frege-since-1902).

(The word "must" is used for parts of the specification that are required for a file or tool to claim compliance with the standards described in this spec. The word "may" is used for parts which are not required; Optional sections may be covered in more detail, as required elements in a subsequent fragment of this spec.)

(`help-wanted` the method of encoding `true/false` values is not currently defined.)

-----

### `csvz-meta-columns` A csvz file can contain a file called `columns.csv`

Metadata about the contents of the csvz file is contained in a directory called "_meta". The file `columns.csv`, if present, is inside this directory.

The file `columns.csv` contains metadata about all of the columns in all of the csv files included in the `csvz` file.

The file `columns.csv` meets the following description:

- There is a header row naming the columns in this file
- Each data row describes a different column in a different file
- The columns must include a column called "filename" and a column called "column".
- It is expected that the columns "filename" and "column" are unique.
  - If the columns "filename" and "column" are not unique, then any meta data about that file may not be correctly interpreted. This may cause difficulties
- There should be more columns than just the "filename" and "column" column. Some suggestions:
  - `data-type` - the type of the column. (Data-types are not described in this spec fragment, and will be covered in later spec fragments.)
  - `nullable` - a value indicating if the column can be null
  - `max-length` - a nullable column, that describes the maximum length of the column, in cases where the data-type supports a maximum length
  - `unique` - a true/false value indicating if the values in the column should be unique
  - `primary-key` - a true/false value indicating if the column can serve as (part or whole of) the primary key of the table.
  - `description` - a description of the column
  - `units` - a nullable name description of the unit of measure
  - `ordinal` - the order in which the columns have been written to the file. In cases where there is no header row, or where columns are re-ordered, this can be helpful.
  - `published` - the date the data in the file was first published
  - `source` - information about the source of the data in the file

(The word "should" is used for parts of the specification that are not required, but which will lead to difficulty for users of the data or the tools if they are not complied with.)

-----

### `csvz-meta-relations` A csvz file can contain a file called `relationships.csv`

Metadata about the contents of the csvz file is contained in a directory called "_meta". The file `relationships.csv`, if present, is inside this directory.

The file `relationships.csv` contains metadata about all of the relationships between any of the columns in any of the files in the `csvz` file.

The file `relationships.csv` meets the following description:

- There is a header row naming the columns in this file
- Each data row describes a different foreign key relationship within the `csvz` file.
- The columns must include these columns:
  - called "table", "key-column", "foreign-table","foreign-key-column"
- There may also be a column called "key-name". In the case of a composite keys, there would be multiple rows with the same "key-name".
- There should be more columns to describe the relationships. It's late at night here. I have chores to do. The woods are lovely dark and deep.

### Unwritten meta fragments

More `meta-*` spec fragments may be needed to describe other meta files.

For example:

- `csv`:
  - what file encoding is used for the csv files (utf-x, BOM, etc.)
  - what separators/qualifiers are used? e.g. what field/row separators?
  - What quoting is used, e..g single/double/mixed/other?
  - Are delimiters doubled or escaped? (if escaped, escaped with what?)
  - The spec can also cover what defaults are sensible for each of these things
  - how are `nulls` represented? e.g. the literal string `null` with no quotes? or `NULL`, or `nil`? Or are empty strings, unquoted, to be treated as NULLs?
- `indexes` - what indexes can/should be built on the tables (if the data)
- `data-types` - what types are used, how are they encoded (e.g. dates: how? binary data base-64 encoded? etc), what ranges exist for numbers etc.
- `user-defined-types` - how can types be extended?
- `schemas` - consider situations where directories are used to describe separate schemas (or other namespacing concepts)
- `deltas` - csv files may hold operations on data, instead of data itself, i.e. details of insert,update,delete,(upsert) operations
- `constraints` - what other constraints exist on the data
- `partitions` - consider situations where a single table is split across multiple files, and or a `csvz` itself is split amongst multiple files, including
- `formulas` - are there calculated columns? what form do the calculations take?

## A list of `csvz-compliant` Tools and Libraries

The following tools and libraries are able to read, write or process `.csvz` files.

| Tool      | Written in | Description                                                                                          |
|-----------|------------|------------------------------------------------------------------------------------------------------|
|           |            | - Packs a set of csv files into a new csvz file, and generates a `tables.csv` and `columns.csv`   |
|           |            | - Converts a `.csvz` file into a `.xlsx` file, that can be opened by Excel.                          |
|           |            | - Converts a `.xlsx` file into a `.csvx` file (note that not all of Excel's features are respected.) |
|           |            | - Exports a `sqlite` database into a new `.csvx` file                                                |
|           |            | - Creates a new `sqlite` database from a `.csvx` file                                                |
|           |            | - Exports a `mysql` database into a new `.csvx` file                                                 |
|           |            | - Creates a new `PostgreSQL` database from a `.csvx` file                                            |
|           |            | - Exports a `PostgreSQL` database into a new `.csvx` file                                            |
|           |            | - Save a JSON file as a series of csv files and _meta files (ready for zipping)                      |
|           |            | - Load some or all of an unzipped csvz as a single json object (limited filtering ability)           |
|           |            | - (More tools...)                                                                                    |

Note that there are currently no `csvz` compliant tools or libraries listed in this table.

If you are aware of one, or you have created one (hint hint), a pull request is welcome.

-----

## Contribute

To experience the fun of contributing, see [Contributing](contributing.md)

Contributors definitely includes people who raise issues. **Raising issues is the quickest way to contribute.** Also look for issues marked `good first issue` or `help wanted`


## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [Leon Bambrick](http://secretgeek.net) has waived all copyright and related or neighboring rights to this work.

-----

> Some ideas are too smart to live; other ideas are too dumb to die.
