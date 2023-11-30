# Variables

This concept of variables for the command line and config files was added in
curl 8.3.0.

A user sets a *variable* to a plain string with `--variable name=content` or
from the contents of a file with `--variable name@file` where the file can be
stdin if set to a single dash (`-`).

A variable in this context is given a specific name and it holds contents. Any
number of variables can be set. If you set the same variable name again, it
will be overwritten with new content. Variable names are case sensitive, can
be up to 128 characters long and may consist of the characters a-z, A-Z, 0-9
and underscore.

## Expand

Variables can be expanded in option parameters using `{{name}}` - when the
option name is prefixed with `--expand-`. This makes the content of the
variable `name` get inserted, or a blank if the name does not exist as a
variable. Insert `{{` verbatim in the string by prefixing it with a backslash,
like `\{{`.

For options specified without the `--expand-` prefix, variables will not be
expanded.

Variable content holding null bytes that are not encoded when expanded, will
cause curl to exit with an error.

## Environment variables

Import an environment variable with `--variable %name`. This import makes curl
exit with an error if the given environment variable is not set. A user can
also opt to set a default value if the environment variable does not exist,
using `=content` or `@file` like shown above.

Example: get the `USER` environment variable into the URL, which fails if
there is no such environment variable:

    --variable %USER
    --expand-url "https://example.com/api/{{USER}}/method"

To instead use `dummy` as a default value if the variable does not exist:

    --variable %USER=dummy
    --expand-url "https://example.com/api/{{USER}}/method"

## Expand `--variable`

The `--variable` option itself can also be expanded, which allows variables to
get set using contents from other variables. Examples:

    --expand-variable var1={{var2}}
    --expand-variable fullname=’Mrs {{first}} {{last}}’
    --expand-variable source@{{filename}}

## Functions

When expanding variables, curl offers a set of *functions* to change how they
are expanded. Functions are applied with colon + function name after the
variable, like this: `{{name:function}}`.

Multiple functions can be applied to the variable. They are then applied in a
left-to-right order: `{{name:func1:func2:func3}}`

These functions are available: `trim`, `json`, `url` and `b64`

## Function: `trim`

Expands the variable without leading and trailing whitespace. Whitespace here
means: **horizontal tab, space, new line, vertical tab, form feed and carriage
return**.

Perhaps extra useful when reading data from files.

    --expand-url “https://example.com/{{path:trim}}”

## Function: `json`

Expands the variable as a valid JSON string - without the quotes. This makes
it easier to insert a variable into an argument without risking that weird
content makes it invalid JSON.

    --expand-json “\”full name\”: \”{{first:json}} {{last:json}}\””

To trim the variable first, apply both functions (in the right order):

    --expand-json “\”full name\”: \”{{name:trim:json}}\””

## Function: `url`

Expands the variable URL encoded. Also known as *percent encoded*. It makes
sure all output characters are legal within a URL and the rest are encoded as
`%HH` where `HH` is a two-digit hexadecimal number for the ascii value.

    --expand-data “name={{name:url}}”

To trim the variable first, apply both functions (in the right order):

    --expand-data “name={{name:trim:url}}”

## Function: `b64`

Expands the variable base64 encoded. Base64 is an encoding for binary data
that only uses 64 specific characters.

    --expand-data “content={{value:b64}}”

To trim the variable first, apply both functions (in the right order):

    --expand-data “content={{value:trim:b64}}”

## Examples

Example: get the contents of a file called `$HOME/.secret` into a variable
called `fix`. Make sure that the content is trimmed and percent-encoded sent
as POST data:

    --variable %HOME=/home/default
    --expand-variable fix@{{HOME}}/.secret
    --expand-data "{{fix:trim:url}}"
    https://example.com/
