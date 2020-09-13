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
  - [`csvz-meta-relations` A csvz file can contain a file called `relations.csv`](#csvz-meta-relations-a-csvz-file-can-contain-a-file-called-relationscsv)
  - [`csvz-meta-csv` A csvz file can contain a file called `csv.csv`](#csvz-meta-csv-A-csvz-file-can-contain-a-file-called-csvcsv)
  - [`csvz-meta-per-file` The ability to include individual meta-files per csv file](#csvz-meta-per-file-the-ability-to-include-individual-meta-files-per-csv-file)
  - [Suggested specification fragments](#suggested-specification-fragments)
- [A list of `csvz-compliant` Tools and Libraries](#a-list-of-csvz-compliant-tools-and-libraries)
- [Contribute](#contribute)
- [License](#license)

-----

## The csvz specification

The `csvz` specification is broken into meaningful fragments.

Files can call themselves `csvz-compliant` if they only comply with the first fragment of the specification, [`csvz-0`](#csvz-0-a-csvz-file-is-literally-just-a-bunch-of-csv-files-in-a-zip-file-with-a-file-name-that-ends-with-csvz).

They can also indicate other fragments of the specification that they have implemented, such as [`csvz-meta-tables`](#csvz-meta-tables-a-csvz-file-can-contain-a-file-called-tablescsv-describing-the-contents-of-the-file), [`csv-meta-relations`](#csvz-meta-relations-a-csvz-file-can-contain-a-file-called-relations.csv) etc.

-----

### `csvz-0` A csvz file is literally just a bunch of `csv` files, in a zip file with a file name that ends with ".csvz"

A csvz file is compliant with `csvz-0` if it is literally just a bunch of `csv` files, in a zip file, that has been renamed to have a ".csvz" file extension.

(Note that each fragment has a fragment identifier written at the beginning of the fragment. For example this is `csvz-0` and the next fragment is `csvz-meta-tables`. Fragments are optional, but it is good to know which fragments you do or do not comply with.)

The `csv` files themselves should be parseable with most csv reading software.

(Anywhere that this spec refers to "a csv file" it means a file that complies with [`RFC 4180`](http://www.faqs.org/rfcs/rfc4180.html) or a compatible [dialect](https://www.w3.org/TR/2015/REC-tabular-metadata-20151217/#dialect-descriptions) as described by the [CSV on the Web Working Group](http://www.w3.org/2013/csvw/), unless a stricter definition is explicitly given.)

(Anywhere that `the csvz specification` refers to "this spec" it means `the csvz specification`.)

-----

### **`csvz-meta-tables`** A `csvz` file can contain a file called `tables.csv` describing the contents of the file

Metadata about the contents of the csvz file is contained in a directory called "_meta". The file `tables.csv`, if present, is inside this directory.

(Assume that the csvz reserves the right to create other .csv files under the _meta folder, and to create more folders under it. Details appear in subsequent spec fragments.)

The file `tables.csv` contains metadata about all of the csv files included in the `csvz` file.

(The file `tables.csv` is a csv file.)

(Anywhere that this spec refers to a file with a name that ends with ".csv" it means the file is a "csv file", as described in [`csvz-0`](#csvz-0-a-csvz-file-is-literally-just-a-bunch-of-csv-files-in-a-zip-file-with-a-file-name-that-ends-with-csvz).)

The file `tables.csv` meets the following description:

- There is a header row naming the columns in this file
- Each data row describes a different csv file within this `csvz` file
- The columns must include a column called "filename"
- There may be more columns.
- Here are some suggestions:
  - `bytes` - the size of the file in bytes
  - `rows` - the number of rows in the file
  - `columns` - the number of columns in the file
  - `description` - a description of the file
  - `published` - the date the data in the file was first published
  - `source` - information about the source of the data in the file
  - `has-column-names` - a `true/false` value indicating if the file has a header row containing column names
  - `skip-rows` - How many rows need to be skipped, before the data begins? (Rarely need to specify this, but when you need it, you need it!)
  - (todo: where information in table.csv conflicts with information in `csv.csv`, then `tables.csv` has precedence over `csv.csv`, for the file it describes. For example `csv.csv` may indicate that all files have header rows, but a specific file may not, and this would be indicated in `tables.csv`)
- The file `tables.csv` may also describe itself. See [Russell](http://wiki.secretgeek.net/paradox#bertrand-russell-making-life-impossible-for-frege-since-1902). Note that `bytes` (for example) might cause a paradox.

(The word "must" is used for parts of the specification that are required for a file or tool to claim compliance with the standards described in this spec. The word "may" is used for parts which are not required; Optional sections may be covered in more detail, as required elements in a subsequent fragment of this spec.)

(Whenever suggestions are provided, they are not required for conformance with the current spec fragment. These suggestion may be described more fully in later spec fragments, in which they may be required.)

(Expectations around the encoding of `true/false` values, and other fundamental `data-types`, are not currently defined.)

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
  - `nullable` - a `true/false` value indicating if the column can be null
  - `max-length` - a nullable column, that describes the maximum length of the column, in cases where the data-type supports a maximum length
  - `unique` - a `true/false` value indicating if the values in the column should be unique
  - `primary-key` - a `true/false` value indicating if the column can serve as (part or whole of) the primary key of the table.
  - `description` - a description of the column
  - `units` - a nullable name description of the unit of measure
  - `ordinal` - the order in which the columns have been written to the file. In cases where there is no header row, or where columns are re-ordered, this can be helpful.
  - `published` - the date the data in the file was first published
  - `source` - information about the source of the data in the file

(The word "should" is used for parts of the specification that are not required, but which will lead to difficulty for users of the data or the tools if they are not complied with.)

-----

### `csvz-meta-relations` A csvz file can contain a file called `relations.csv`

Metadata about the contents of the csvz file is contained in a directory called "_meta". The file `relations.csv`, if present, is inside this directory.

The file `relations.csv` contains metadata about all of the relationships between any of the columns in any of the files in the `csvz` file.

The file `relations.csv` meets the following description:

- There is a header row naming the columns in this file
- Each data row describes a different foreign key relationship within the `csvz` file.
- The columns must include these columns:
  - called "table", "key-column", "foreign-table","foreign-key-column"
- There may also be a column called "key-name". In the case of a composite keys, there would be multiple rows with the same "key-name".
- todo: There may be more columns needed to describe the relationships.

-----

### `csvz-meta-csv` A csvz file can contain a file called `csv.csv`

(todo: this section is still very much a draft)

Metadata about the rules of the csvz file are contained in a directory called "_meta". The file `csv.csv`, if present, is inside this directory.

The file `csv.csv` contains metadata about how the csv files in this `csvz` file are formatted, from a general csv standards point of view.

(Later spec fragments will give exact definitions for the expected columns and supported columns, their possible values and the meanings of those values.)

But to comply with `csvz-meta-csv` the file `csv.csv` must:

- Be formatted as [`strict-4180`](https://tools.ietf.org/html/rfc4180)
- Have a header row naming the columns in this file
- Data rows, each of which describes a different and very specific but fundamental aspect of the csv format used by all other csv files in this csvz file.
- Suggested aspects that can be described (and which should be described in subsequent fragments)
  - `encoding - what file encoding is used for the csv files (utf-x, BOM, etc.)
  - `field separator` - examples comma, tab, semicolon, space, various emoji
  - `row separator` - examples CRLF, LF, CR, semicolon, exclamation point, backtick
  - `qualifier` - what qualifiers (if any) are used for embedding delimiters. perhaps qualifiers are not used. Can single/double/mixed/other be used?
  - `escaping` - Are qualifiers doubled or escaped? (If escaped, escaped with what?)
  - `null-values` - how are `null-values` represented? e.g. the literal string `null` with no quotes? or `NULL`, or `nil`? Or are empty strings, unquoted, to be treated as NULLs?
  - `has-column-names` - a `true/false` value indicating if every csv file (other than this one) file has a header row containing column names. (Can be over-ridden by a `has-column-names` value in the `tables.csv` file, if present.)
  - user defined `data-types` can be handled elsewhere, but a limited number of common fundamental `data-types` could be most expediently described in `csv.csv`, such as:
    - date formats - e.g. ISO 8601
    - boolean formats
    - binary data (hint: base 64 coded)
    - integer ranges.
    - floats
- Later spec fragments may further describe "sensible defaults" for these things
- Later spec fragments may describe "sensible heuristics" for detecting delimiters/qualifiers/quoting and escaping rules, etc.


(todo: See also [csvw dialect descriptions](https://www.w3.org/TR/2015/REC-tabular-metadata-20151217/#dialect-descriptions))

-----

### `csvz-meta-per-file` The ability to include individual meta-files per csv file

This fragment extends all other `csvz-meta-*` fragments.

Consider an example where a single csv file, `people.csv` inside the csvz follows different standards to the other files.

It's csv conventions could be described in a file: `_meta/csv/people.csv` and those would be taken to override the conventions in `_meta/csv.csv`

Similarly, a file can have its own `_meta/tables/{filename}.csv` file, `_meta/columns/{filename}.csv` and `_meta/relations/{filename}.csv`.

This methods can be assumed to extend for all other `_meta/*.csv` files.

A `per-file` meta file is assumed to have higher **precedence** than the files directly contained in `_meta/*.csv`.

For example: if `_meta/columns.csv` decribed the columns of `states.csv` in one way, but `_meta/columns/states.csv` described those columns in another way, all details for `states.csv` in `_meta/columns.csv` should be ignored, and those in `_meta/columns/states.csv` used instead. (i.e. they are not *combined*).

(Note - combining might be more interesting, useful. Would let you build up/inherit attributes. But would also need a way to "erase" a rule, and I can't think of a way to do that so let's stick with "no combining")

(Suggestion for authors of Tooling that reads these files: they may want to provide optional debug information that describes where meta data was sourced from, highlighting situations where precedence rules needed to be applied.)

You can also mix and match `_meta/*.csv` with `per-file` meta information, without loss of meaning.

For example the table `states.csv` may be described in `_meta/tables.csv` while it's columns may be described in `_meta/columns/states.csv`

-----

### Unwritten meta fragments

More `meta-*` spec fragments may be needed to describe other meta files.

For example:

- `indexes` - what indexes can/should be built on the tables (if the data)
- `data-types` - what types are used, how are they encoded (e.g. dates: how? binary data base-64 encoded? etc), what ranges exist for numbers etc.
- `user-defined-types` - how can types be extended?
- `schemas` - consider situations where directories are used to describe separate schemas\databases (or other namespacing concepts)
- `directories` - instead of defining schemas (or other namespaces) perhaps the concept of directories could be directly described, a kind of set of routing rules/conventions. in the directories.csv you might in effect say, the directories in the root directory (other than _meta) are to be treated as "server" names. the next level are to be treated as "share" names... or perhaps you will say, "under "/databases" the directory names in there are treated as "database" names, and the names under that are "schema" names.
- `naming` - perhaps you will define naming conventions, e.g. ways to pull data from names, or use names to know which files can be combined into one logical unit later.
- `deltas` - csv files may hold operations on data, instead of data itself, i.e. details of insert,update,delete,(upsert) operations
- `constraints` - what other constraints exist on the data
- `partitions` - consider situations where a single table is split across multiple files, and or a `csvz` itself is split amongst multiple files, including
- `formulas` - are there calculated columns? what form do the calculations take?

## A list of `csvz-compliant` Tools and Libraries

The following tools and libraries are able to read, write or process `.csvz` files.

| Tool      | Actions | Compliance |Description                                                                                          |
|-----------|---------|------------|-----------------------------------------------------------------------------------------------------|
|[Sylvan.Data.CsvZip](https://github.com/MarkPflug/Sylvan.Data.CsvZip) | Create / Read | `csvz-0` `csvz-meta-tables` `csvz-meta-columns` | Library for programatically creating and reading .csvz files |
|[Sylvan.Tools.CsvZip](https://github.com/MarkPflug/Sylvan.Data.CsvZip#sylvantoolscsvzip-tool) | Create | `csvz-0` `csvz-meta-tables` `csvz-meta-columns` | .NET global tool for creating .csvz files from the commandline |
|           |         |            | Packs a set of csv files into a new csvz file, and generates a `tables.csv` and `columns.csv`   |
|           |         |            | Converts a `.csvz` file into a `.xlsx` file, that can be opened by Excel.                        |
|           |         |            | Converts a `.csvz` file into a `.xlsx` file, that can be opened by Excel.                          |
|           |         |            | Converts a `.xlsx` file into a `.csvz` file (note that not all of Excel's features are respected.) |
|           |         |            | Exports a `sqlite` database into a new `.csvz` file                                                |
|           |         |            | Creates a new `sqlite` database from a `.csvz` file                                                |
|           |         |            | Exports a `mysql` database into a new `.csvz` file                                                 |
|           |         |            | Creates a new `PostgreSQL` database from a `.csvz` file                                            |
|           |         |            | Exports a `PostgreSQL` database into a new `.csvz` file                                            |
|           |         |            | Save a JSON file as a series of csv files and _meta files (ready for zipping)                      |
|           |         |            | Load some or all of an unzipped csvz as a single json object (limited filtering ability)           |
|           |         |            | Validates which spec fragments a `csvz` file complies with                                         |
|           |         |            | (More tools...)                                                                                    |

If you know of a `csvz` compliant tool, or you have created one (hint hint), a pull request is welcome.

Suggestion: You can **use existing `csvz` or `csv` libraries** to build a new type of connection (e.g. A tool to create/read csvz files from an Oracle database, using existing libraries, would take some Oracle knowledge, and not much else.)

-----

## Contribute

To experience the fun of contributing, see [Contributing](contributing.md)

Contributors definitely includes people who raise issues. **Raising issues is the quickest way to contribute.** Also look for issues marked `good first issue` or `help wanted`

A community forum for discussion/ideas for implementors and tool builders is much needed, following [issue #14](https://github.com/secretGeek/csvz/issues/14) to find where the community will be built.

## License

[![CC0](cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [Leon Bambrick](http://secretgeek.net) has waived all copyright and related or neighboring rights to this work.

-----

> Some ideas are too smart to live; other ideas are too dumb to die.
