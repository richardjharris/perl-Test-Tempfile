## Name

```perl
Test::TempFile - compact way to use tempfiles in unit tests
```

## Synopsis

```perl
# Under Test::More

my $t = Test::TempFile->new([content]);

download_file($url, $t->path);
$t->exists_ok
   ->content_is("Expected content", 'test message')
   ->json_is({ foo => 'bar' }, 'expect file to be JSON' );

my $t = Test::TempFile->to_json({ a => 1 });
run_some_script( config_file => $t->path );
```

## Description

This is a simple module for creating temporarily files with optional initial
content, then running various tests on the state of the tempfile.

It is intended for testing code that uses files as input or output.

**UTF-8 is assumed everywhere**. In the future a binary/raw subclass of this
module may be released.

## Constructors

- new ( \[content\] )

    Create a new tempfile. Upon creation the file will exist but be empty. When
    this object is destroyed, the file will be deleted if it still exists.

    `content` may be a string or an arrayref of strings (which will be joined
    using the empty string). The tempfile will be populated with this content.

- to\_json ( data )

    Create a new tempfile, with content set to the JSON representation of `data`.

- to\_yaml ( data )

    Create a new tempfile, with content set to the YAML representation of `data`.

## Instance Methods

- path

    Returns the string path to this tempfile.

- absolute

    As `path` but guaranteed to be absolute.

- content

    Returns the content of this tempfile as a string.

- set\_content ( content )

    Sets the content of this tempfile. `content` may be a string, or an
    arrayref of strings (which are passed to `join('', ...)`.

    Returns the [Test::TempFile](https://metacpan.org/pod/Test::TempFile) object to allow chaining.

- append\_content ( content )

    Appends `content` to the end of the tempfile. `content` must be a
    string.

    Returns the [Test::TempFile](https://metacpan.org/pod/Test::TempFile) object to allow chaining.

- exists

    Returns whether or not the tempfile currently exists.

- empty

    Returns true if the file is non-existent or existent but empty.

- filehandle ( \[mode\] )

    Returns a filehandle to the tempfile. The default `mode` is '>'
    (read-only) but others may be used ('<', '>>' etc.)

- unlink

    Unlinks (deletes) the tempfile if it exists.

- from\_json

    Interprets the tempfile contents as JSON and returned the decoded
    Perl data.

- from\_yaml

    Interprets the tempfile contents as JSON and returned the decoded
    Perl data.

## Test Methods

These methods can be used inside unit tests. They always return the
[Test::TempFile](https://metacpan.org/pod/Test::TempFile) object itself, so multiple tests can be chained.

- exists\_ok ( \[message\] )

    Asserts that the tempfile exists.

- not\_exists\_ok ( \[message\] )

    Asserts that the tempfile does not exist.

- empty\_ok ( \[message\] )

    Asserts that the tempfile is empty.

- not\_empty\_ok ( \[message\] )

    Asserts that the tempfile is not empty.

- content\_is ( expected \[, message\] )

    Asserts that the tempfile contents are equal to `expected`.

- content\_like ( expected \[, message\] )

    Asserts that the tempfile contents match the regex `expected`.

- json\_is ( expected \[, message\] )

    Asserts that the tempfile contains JSON content equivalent to the Perl data in
    `expected`.

- yaml\_is ( expected \[, message\] )

    Asserts that the tempfile contains YAML content equivalent to the Perl data in
    `expected`.

- assert ( coderef, message )

    Calls `coderef` with `$_` set to this object. Asserts that the `coderef`
    returns true. Returns the original object.

    Is useful when chaining multiple tests together:

    ```perl
    $t->not_empty_ok
      ->assert(sub { $_->content =~ /foo/ })
      ->assert(sub { $_->content !~ /bar/ });
    ```
