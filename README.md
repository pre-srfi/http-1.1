# SRFI nnn: Title

by Amirouche Boubekki, Another Person, Third Person

# Status

Early Draft

# Abstract

HTTP/1.1 is a widespread protocol to communicate on the Internet, and
the basis of of the web.  This library offers the routine necessary,
to read and write HTTP/1.1 and HTTP/1.0 without assuming a particular
implementation of network sockets.

# Issues

- The generators / accumulators might raise connection close or in the
  case of the generator return eof. Is that really a problem?

# Rationale

??? detailed rationale. This should be 200-500 words long. Please
explain why the proposal should be incorporated as a standard feature
in Scheme implementations. List related standards and SRFIs, including
dependencies, conflicts, and replacements. If there are other
standards which this proposal will replace or with which it will
compete, please explain why the present proposal is a substantial
improvement.

## Survey of prior art

GitHub's version of Markdown can make tables. For example:

| System        | Procedure | Signature                 |
| ------------- |:---------:| ------------------------- |
| System A      | `jumble`  | _list_ _elem_             |
| System B      | `bungle`  | _elem_ _list_             |
| System C      | `frob`    | _list_ _elem_ _predicate_ |

# Specification

## `(http-error? obj)`

Returns `#t` if `OBJ` is an HTTP error. Otherwise, it returns `#f`.

## `(http-error-message OBJ)`

Return a human readable string describing the error.

## `(http-error-payload OBJ)`

Return an object that can help understand the error or possibly
workaround it.

## `(http-request-line-read generator)`

`GENERATOR` must generate one byte at a time.  `http-request-line-read`
returns three values:

1. A string representing the HTTP method
2. A string representing the URI
3. A pair of numbers representing the HTTP version

When this procedure returns, `GENERATOR` is positioned after the
request line, at the first byte of headers or in the case when there
is no headers, upon a return byte.

## `(http-request-line-write accumulator method uri version)`

TODO

## `(http-response-line-read generator)`

`GENERATOR` must generate one byte at a time.  `http-response-line-read`
returns two, possibly three values:

1. A pair of numbers representing the HTTP version,
2. A number representing the status code,
3. Possibly a third and last string object representing the reason.

When this procedure returns, `GENERATOR` is positioned after the
response line, at the first byte of headers or in the case where there
is no headers, upon a return byte.

**Note:** This procedure will read what is called "status line" in
RFCXYZ.

## `(http-response-line-write accumulator version code reason)`

TODO

## `(http-headers-read generator)`

`GENERATOR` must generate one byte at a time.  `http-headers-read`
returns an association list of string key-value pairs, where string
values have space trimmed both on the left and on the right.

## `(http-headers-write accumulator headers)`

## `(http-chunks-generator generator)`

`GENERATOR` must return one byte at a time. This procedure returns a
generator that follows the protocol:

Each chunk is generated as two values. The first value is the chunk
extensions as a string that is possibly the empty string and the
second value is a generator of the bytes the current chunk.

After consuming all generators generated by the generator returned by
`http-chunks-generator` it is necessary to call `http-headers-read` to
consume the end of `GENERATOR` that will possibly yield trailing
headers and will position `GENERATOR` at the end of the request or
response.

## `(http-chunked-body-generator generator)`

`GENERATOR` must return one byte at a time. This procedure returns a
generator that generates one byte at a time of a chunked body. It will
ignore chunk extensions and trailing headers.

## `(http-chunk accumulator bytevector)`

TODO

## `(http-request-read generator)`

## `(http-request-write accumulator method uri version headers body)`

## `(http-response-read generator)`

## `(http-response-write accumulator version status-code reason headers body)`

# Implementation

??? explanation of how it meets the sample implementation requirement
(see process), and the code, if possible, or a link to it Source for
the sample implementation.

# Acknowledgements

??? Give credits where credits is due.

# References

??? Optional section with links to web pages, books and papers that
helped design the SRFI.

# Copyright

Copyright (C) Firstname Lastname (20XY).

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
