# raven-lua

A small Lua interface to [Sentry](https://sentry.readthedocs.org/) that also
has a helpful wrapper function `call()` that takes any arbitrary Lua function
(with arguments) and executes it, traps any errors and reports it automatically
to Sentry.

# Updates from Original

The original library from Cloudflare has not been updated since January 2015.  There were some bugs that were caught by users, and pull requests made to fix those bugs.  Those updates were merged into this forked copy of the original repo.

1. Fixes https connections: [PR #4](https://github.com/cloudflare/raven-lua/pull/4) by [mattrobenolt](https://github.com/mattrobenolt)
2. Fixes invalid value `tags` for parameters: [PR #13](https://github.com/cloudflare/raven-lua/pull/13) by [ye](https://github.com/cloudflare/raven-lua/pull/13)

# Synopsis

```lua

local raven = require 'raven'

-- http://pub:secret@127.0.0.1:8080/sentry/proj-id
local rvn = raven:new("http://pub:secret@127.0.0.1:8080/sentry/proj-id", {
   tags = { foo = "bar" },
})

-- Send a message to sentry
local id, err = rvn:captureMessage(
  "Sentry is a realtime event logging and aggregation platform.",
  { tags = { abc = "def" } } -- optional
)
if not id then
   print(err)
end

-- Send an exception to sentry
local exception = {{
   ["type"]= "SyntaxError",
   ["value"]= "Wattttt!",
   ["module"]= "__builtins__"
}}
local id, err = rvn:captureException(
   exception,
   { tags = { abc = "def" } } -- optional
)
if not id then
   print(err)
end

-- Catch an exception and send it to sentry
function bad_func(n)
   return not_defined_func(n)
end

-- variable 'ok' should be false, and an exception will be sent to sentry
local ok = rvn:call(bad_func, 1)

```
# Documents

See docs/index.html for more details.

# Prerequisites

```
# for unit tests
$luarocks install lunit
$luarocks install luaposix

# for generating docs
$luarocks install ldoc
```
