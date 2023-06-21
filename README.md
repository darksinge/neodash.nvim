# Neodash.nvim

Shamelessly named after the JavaScript package, [Lodash](https://lodash.com).

Also, the initial commit was shamelessly copy-pasted from
[mason.nvim](https://github.com/williamboman/mason.nvim/tree/f7f81ab41b153e2902ebded401a8a0a6abe28607/lua/mason-core/functional)
(sorry, it was just so well done).

## But why?

- I wanted to package these functions into a plugin because I kept needing the
  same functionality in several of my projects for NeoVim.

- Documentation. Lua packages tend to suck at this. I wanted to make it better.

## Installation

Using [lazy.nvim](https://github.com/folke/lazy.nvim)

```lua
-- init.lua
{
    'darksinge/neodash.nvim',
}
```

## Contributing

If you'd like to contribute, please open a new issue with your idea and let's
chat!

Things that would be extremely welcome:

- Example snippets!
- ChatGPT generated the majority of the documentation under [Usage](#usage), so
  it's possible some of the documentation is incorrect.
- Also, ChatGPT generated the majority of the documentation, so some of the
  method descriptions read like garbage.

If you find issues in the documentation or are willing to add examples, your PR would be welcome!

## Usage

### Data

- ```lua
  _.table_pack(...) -> table
  ```

  Packs the given arguments into a table.

  Example:

  ```lua
  local packed = _.table_pack('a', 'b', 'c')
  print(packed[1]) -- a
  print(packed[2]) -- b
  print(packed[3]) -- c
  print(packed.n)  -- 3
  ```

  <hr>

- ```lua
  ---@generic T : string
  ---@param values T[]
  ---@return table<T, T>
  _.enum(values: string[]) -> table<string, string>
  ```

  Creates an enum from the given list of string values. The returned table uses
  each string as both a key and a value.

  Example:

  ```lua
  _.enum({ 'foo', 'bar', 'baz' })
  -- => { foo = 'foo', bar = 'bar', baz = 'baz' }
  ```

  <hr>

- ```lua
  ---@generic T
  ---@param list T[]
  ---@return table<T, boolean>
  _.set_of(list: any[]) -> table<any, boolean>
  ```

  Creates a set from the given list. The returned table uses each value from
  the list as a key, and the value for each key is `true`.

  Example:

  ```lua
  _.enum({ 'foo', 'bar', 'baz' })
  -- => { foo = true, bar = true, baz = true}
  ```

### Functions

- ```lua
    ---@generic T : fun(...)
    ---@param fn T
    ---@param arity integer
    ---@return T
    _.curryN(fn: T, arity: integer) -> T
  ```

  This function `curryN` takes a function `fn` and an integer `arity` as
  arguments. It returns a curried version of the input function `fn`, which
  waits until it has received `arity` number of arguments before being invoked.

  Example:

  ```lua
  local function sumThreeNumbers (a, b, c)
      return a + b + c
  end

  local curriedSum = \_.curryN(sumThreeNumbers, 3)
  print(curriedSum(1)(2)(3)) -- Output: 6
  ```

  <hr>

- ```lua
  _.compose(...) -> T
  ```

````

This function `compose` takes one or more functions as arguments and returns
a function. When the returned function is called, it runs the original input
functions from right to left, passing the result of each function into the
next.

- ```lua
  ---@generic T
  ---@param fn fun(...): T
  ---@return fun(...): T
  _.partial(fn: fun(...): T, ...) -> fun(...): T
  ```

  This function `partial` takes a function `fn` and any number of arguments. It
  returns a new function that, when called, will invoke `fn` with the initial
  arguments provided to `partial`, followed by any additional arguments
  provided when the new function is invoked.

- ```lua
  ---@generic T
  ---@param value T
  ---@return T
  _.identity(value: T) -> T
  ```

  This function `identity` takes an argument `value` and returns the same
  `value` as is.

- ```lua
  ---@generic T
  ---@return fun(): T
  _.always(a: T) -> fun(): T
  ```

  This function `always` takes an argument `a` and returns a new function. When
  the new function is invoked, it will always return `a`.

- ```lua
  ---@return true
  _.T = _.always(true)
  ```

  A function that always returns `true`.

- ```lua
  ---@return false
  _.F = _.always(false)
  ```

  A function that always returns `false`.

- ```lua
  ---@generic T : fun(...)
  ---@param fn T
  ---@param cache_key_generator (fun(...): any)?
  ---@return T
  _.memoize(fn: T, cache_key_generator: fun(...): any) -> T
  ```

  This function `memoize` takes a function `fn` and an optional function
  `cache_key_generator` as arguments. It returns a new function that caches the
  result of `fn` for each unique set of arguments. The `cache_key_generator` is
  used to generate keys for the cache, defaulting to `_.identity` if no
  generator is provided.

- ```lua
  ---@generic T
  ---@param fn fun(): T
  ---@return fun(): T
  _.lazy(fn: fun(): T) -> fun(): T
  ```

  This function `lazy` takes a function `fn` as argument. It returns a new
  function that, when called, will call `fn` and cache its result. Subsequent
  calls to the new function will return the cached result.

- ```lua
  ---@generic U
  ---@param fn fun(value: U): unknown
  ---@param value U
  ---@return U
  _.tap(fn: fun(value: U): T, value: U) -> U
  ```

  The function `_.tap()` takes two arguments: a function `fn` and a value. It
  applies `fn` to the value and then returns the value regardless of the result
  of the function application.

- ```lua
  ---@generic T, U
  ---@param value T
  ---@param fn fun(value: T): U
  ---@return U
  _.apply_to(value: T, fn: fun(value: T): U) -> U
  ```

  This function `apply_to` takes a `value` and a function `fn` as arguments. It
  returns the result of applying `fn` to `value`.

- ```lua
  ---@generic T, R, V
  ---@param fn fun (args...: V[]): R
  ---@param args V[]
  ---@return R
  _.apply(fn: fun (args...: V[]): R, args: V[]) -> R
  ```

  This function `apply` takes a function `fn` and an array `args` as arguments.
  It returns the result of applying `fn` to the elements of `args`.

- ```lua
  ---@generic T, V
  ---@param fn fun(...): T
  ---@param fns (fun(value: V))[]
  ---@param val V
  ---@return T
  _.converge(fn: fun(...): T, fns: (fun(value: V))[], val: V) -> T
  ```

  This function `converge` takes a function `fn`, a list of functions `fns`,
  and a value `val`. It applies each function in `fns` to `val`, collects the
  results into an array, and applies `fn` to that array.

### Lists

- ```lua
  _.reverse(list: T[]) -> T[]
  ```

  Reverses the given list and returns the result.

- ```lua
  _.list_not_nil(...) -> T[]
  ```

  Returns a list of arguments where nil values are omitted.

- ```lua
  _.find_first(predicate: fun(item: T): boolean, list: T[]) -> T | nil
  ```

  Returns the first item in the list that satisfies the predicate function. If
  no such item is found, returns nil.

- ```lua
  _.any(predicate: fun(item: T): boolean, list: T[]) -> boolean
  ```

  Checks if any item in the list satisfies the predicate function. Returns true
  if at least one item does, else false.

- ```lua
  _.all(predicate: fun(item: T): boolean, list: T[]) -> boolean
  ```

  Checks if all items in the list satisfy the predicate function. Returns true
  only if all items satisfy the predicate.

- ```lua
  _.filter(filter_fn: (fun(item: T): boolean), items: T[]) -> T[]
  ```

  Filters the items in the list based on the filter function and returns the
  resulting list.

- ```lua
  _.map(map_fn: (fun(item: T): U), items: T[]) -> U[]
  ```

  Applies the map function to all items in the list and returns the resulting
  list.

- ```lua
  _.flatten(value: any[]) -> any[]
  ```

  Flattens a nested list into a single level list.

- ```lua
  _.filter_map(map_fn: fun(item: T): Optional, list: T[]) -> any[]
  ```

  Applies the map function to all items in the list and filters out items that
  are not present.

- ```lua
  _.each(fn: fun(item: T, index: integer), list: T[])
  ```

  Invokes the function on each item in the list. The function is called with
  two arguments: the item and its index.

- ```lua
  _.list_copy(list: T[]) -> T[]
  ```

  Returns a copy of the given list.

- ```lua
  _.concat(a: any, b: any) -> any
  ```

  Concatenates two lists or strings.

- ```lua
  _.append(value: T, list: T[]) -> T[]
  ```

  Appends a value to the end of a list and returns the new list.

- ```lua
  _.prepend(value: T, list: T[]) -> T[]
  ```

  Prepends a value to the start of a list and returns the new list.

- ```lua
  _.zip_table(keys: T[], values: U[]) -> table<T, U>
  ```

  Returns a new table where keys from one list are associated with values from
  another list.

- ```lua
  _.nth(offset: number, value: T[]|string) -> T|string|nil
  ```

  Returns the item at the given offset. For a negative offset, items are
  counted from the end.

- ```lua
  _.head(value: T[]|string) -> T|string|nil
  ```

  Returns the first item of a list or string.

- ```lua
  _.last(list: T[]) -> T?
  ```

  Returns the last item from a list.

- ```lua
  _.length(value: string|any[]) -> integer
  ```

  Returns the length of a list or string.

- ```lua
  _.sort_by(comp: fun(item: T): any, list: T[]) -> T[]
  ```

  Sorts a list based on the comparison function and returns the sorted list.

- ```lua
  _.join(sep: string, list: any[]) -> string
  ```

  Joins a list into a string with the given separator.

- ```lua
  _.uniq_by(id: fun(item: T): any, list: T[]) -> T[]
  ```

  Returns a list with unique items based on the id function.

- ```lua
  _.partition(predicate: fun(item: T): boolean, list: T[]) -> T[][]
  ```

  Partitions a list into two lists, where the first contains items that satisfy
  the predicate and the second contains items that don't.

- ```lua
  _.take(n: integer, list: T[]) -> T[]
  ```

  Returns the first n items from a list.

- ```lua
  _.drop(n: integer, list: T[]) -> T[]
  ```

  Removes the first n items from a list and returns the remaining items.

- ```lua
  _.drop_last(n: integer, list: T[]) -> T[]
  ```

  Removes the last n items from a list and returns the remaining items.

- ```lua
  _.reduce(fn: fun(acc: U, item: T): U, acc: U, list: T[]) -> U
  ```

  Reduces a list to a single value by iteratively applying the function to an
  accumulator and each item in the list.

- ```lua
  _.split_every(n: integer, list: T[]) -> T[][]
  ```

  Splits a list into sub-lists each containing n items, except for the last one
  which may contain less.

- ```lua
  _.index_by(index: fun(item: T): U, list: T[]) -> table<U, T>
  ```

  Returns a table where each item in the list is indexed by the result of the
  index function.

### Logic

- ```lua
  ---@generic T
  ---@param predicates (fun(item: T): boolean)[]
  ---@param item T
  ---@return boolean
  _.all_pass(predicates: (fun(item: T): boolean)[], item: T) -> boolean
  ```

  Takes an array of predicate functions and an item. It returns true if all
  predicate functions pass for the item (i.e., return true), otherwise returns
  false.

- ```lua
  ---@generic T
  ---@param predicates (fun(item: T): boolean)[]
  ---@param item T
  ---@return boolean
  _.any_pass(predicates: (fun(item: T): boolean)[], item: T) -> boolean
  ```

  Takes an array of predicate functions and an item. Returns true if any of the
  predicate functions pass for the item (i.e., return true), otherwise returns
  false.

- ```lua
  ---@generic T, U
  ---@param predicate fun(item: T): boolean
  ---@param on_true fun(item: T): U
  ---@param on_false fun(item: T): U
  ---@param value T
  ---@return U
  _.if_else(predicate: fun(item: T): boolean, on_true: fun(item: T): U, on_false: fun(item: T): U, value: T) -> U
  ```

  Takes a predicate function, two transformation functions, and a value. If the
  predicate returns true for the value, it applies the first transformation
  function (on_true) on the value, otherwise applies the second transformation
  function (on_false).

- ```lua
  ---@param value boolean
  ---@return boolean
  _.is_not(value: boolean) -> boolean
  ```

  Takes a boolean value and returns its negation.

- ```lua
  ---@generic T
  ---@param predicate fun(value: T): boolean
  ---@param value T
  ---@return boolean
  _.complement(predicate: fun(value: T): boolean, value: T) -> boolean
  ```

  Takes a predicate function and a value, returns the negation of the predicate
  result for that value.

- ```lua
  ---@generic T, U
  ---@param predicate_transformer_pairs {[1]: (fun(value: T): boolean), [2]: (fun(value: T): U)}[]
  ---@param value T
  ---@return U?
  _.cond(predicate_transformer_pairs: {[1]: (fun(value: T): boolean), [2]: (fun(value: T): U)}[], value: T) -> U?
  ```

  Takes an array of predicate-transformer pairs and a value. For each pair, it
  checks if the value passes the predicate (the first element of the pair), if
  so, it returns the result of applying the transformer function (the second
  element of the pair) on the value. If none of the predicates pass, it returns
  nil.

- ```lua
  ---@generic T
  ---@param default_val T
  ---@param val T?
  ---@return T
  _.default_to(default_val: T, val: T?) -> T
  ```

  Takes a default value and an optional value. If the optional value is not
  nil, it returns the optional value, otherwise it returns the default value.

### Relation

- ```lua
  _.equals(expected: any, value: any) -> boolean
  ```

  Checks if the provided value is equal to the expected value. Returns true if
  they are equal, false otherwise.

- ```lua
  _.not_equals(expected: any, value: any) -> boolean
  ```

  Checks if the provided value is not equal to the expected value. Returns true
  if they are not equal, false otherwise.

- ```lua
  _.prop_eq(property: any, value: any, tbl: table) -> boolean
  ```

  Checks if the value of a given property in the provided table is equal to the
  provided value. Returns true if they are equal, false otherwise.

- ```lua
  _.prop_satisfies(predicate: fun(value: any): boolean, property: any, tbl: table) -> boolean
  ```

  Applies a predicate function to the value of a given property in the provided
  table. Returns the result of the predicate.

- ```lua
  _.path_satisfies(predicate: fun(value: any): boolean, path: any[], tbl: table) -> boolean
  ```

  Applies a predicate function to the value at a given path in the provided
  table. Returns the result of the predicate.

- ```lua
  _.min(a: number, b: number) -> number
  ```

  Subtracts the first number from the second one. Returns the result.

- ```lua
  _.add(a: number, b: number) -> number
  ```

  Adds two numbers together. Returns the result.

### Strings

- ```lua
  ---@param pattern string
  ---@param str string
  _.matches(pattern: string, str: string) -> boolean
  ```

  Returns true if the string matches the provided pattern.

- ```lua
  _.match(pattern: string, str: string) -> table
  ```

  Returns a table with the matches of the provided pattern in the string.

- ```lua
  ---@param template string
  ---@param str string
  _.format(template: string, str: string) -> string
  ```

  Formats the string according to the provided template.

- ```lua
  ---@param sep string
  ---@param str string
  _.split(sep: string, str: string) -> table
  ```

  Splits the string into a table by the provided separator.

- ```lua
  ---@param pattern string
  ---@param repl string|function|table
  ---@param str string
  _.gsub(pattern: string, repl: string|function|table, str: string) -> string
  ```

  Returns a string where the matches for the pattern have been replaced by the
  repl.

- ```lua
  _.trim(str: string) -> string
  ```

  Returns the string without leading/trailing whitespace.

- ```lua
  ---https://github.com/nvim-lua/nvim-package-specification/blob/93475e47545b579fd20b6c5ce13c4163e7956046/lua/packspec/schema.lua#L8-L37
  ---@param str string
  ---@return string
  _.dedent(str: string) -> string
  ```

  Removes common leading indentation from a multiline string.

- ```lua
  ---@param prefix string
  ---@str string
  _.starts_with(prefix: string, str: string) -> boolean
  ```

  Returns true if the string starts with the provided prefix.

- ```lua
  ---@param str string
  _.to_upper(str: string) -> string
  ```

  Returns the string converted to uppercase.

- ```lua
  ---@param str string
  _.to_lower(str: string) -> string
  ```

  Returns the string converted to lowercase.

- ```lua
  ---@param pattern string
  ---@param str string
  _.trim_start_matches(pattern: string, str: string) -> string
  ```

  Returns the string with leading characters that match the pattern removed.

- ```lua
  ---@param pattern string
  ---@param str string
  _.trim_end_matches(pattern: string, str: string) -> string
  ```

  Returns the string with trailing characters that match the pattern removed.

- ```lua
  _.strip_prefix(prefix_pattern: string, str: string) -> string
  ```

  Returns the string with the prefix that matches the prefix_pattern removed.

- ```lua
  _.strip_suffix(suffix_pattern: string, str: string) -> string
  ```

  Returns the string with the suffix that matches the suffix_pattern removed.

### Numbers

- ```lua
  ---@param number number
  _.negate(number: number) -> number
  ```

  Negates the given number.

- ```lua
  _.gt(number: number, value: number) -> boolean
  ```

  Returns true if the value is greater than the number.

- ```lua
  _.gte(number: number, value: number) -> boolean
  ```

  Returns true if the value is greater than or equal to the number.

- ```lua
  _.lt(number: number, value: number) -> boolean
  ```

  Returns true if the value is less than the number.

- ```lua
  _.lte(number: number, value: number) -> boolean
  ```

  Returns true if the value is less than or equal to the number.

- ```lua
  _.inc(increment: number, value: number) -> number
  ```

  Increases the value by the increment.

- ```lua
  _.dec(decrement: number, value: number) -> number
  ```

  Decreases the value by the decrement.

### Tables

- ```lua
  ---@generic T, U
  ---@param index T
  ---@param tbl table<T, U>
  ---@return U?
  _.prop(index: T, tbl: table<T, U>) -> U?
  ```

  Returns the value at the specified index in the table.

- ```lua
  ---@param path any[]
  ---@param tbl table
  _.path(path: any[], tbl: table) -> any
  ```

  Returns the value at the specified path in the table.

- ```lua
  ---@generic T, U
  ---@param keys T[]
  ---@param tbl table<T, U>
  ---@return table<T, U>
  _.pick(keys: T[], tbl: table<T, U>) -> table<T, U>
  ```

  Returns a new table with only the keys specified in the keys array.

- ```lua
  _.keys(tbl: table) -> table
  ```

  Returns an array of the table's keys.

- ```lua
  _.size(tbl: table) -> number
  ```

  Returns the number of keys in the table.

- ```lua
  ---@generic K, V
  ---@param tbl table<K, V>
  ---@return { [1]: K, [2]: V }[]
  _.to_pairs(tbl: table<K, V>) -> { [1]: K, [2]: V }[]
  ```

  Returns an array of key-value pairs from the table.

- ```lua
  ---@generic K, V
  ---@param pairs { [1]: K, [2]: V }[]
  ---@return table<K, V>
  _.from_pairs(pairs: { [1]: K, [2]: V }[]) -> table<K, V>
  ```

  Returns a new table created from the array of key-value pairs.

- ```lua
  ---@generic K, V
  ---@param tbl table<K, V>
  ---@return table<V, K>
  _.invert(tbl: table<K, V>) -> table<V, K>
  ```

  Returns a new table where keys become values and values become keys.

- ```lua
  ---@generic K, V
  ---@param transforms table<K, fun (value: V): V>
  ---@param tbl table<K, V>
  ---@return table<K, V>
  _.evolve(transforms: table<K, fun (value: V): V>, tbl: table<K, V>) -> table<K, V>
  ```

  Returns a new table where each value is transformed by its corresponding
  function in the transforms table.

- ```lua
  ---@generic T : table
  ---@param left T
  ---@param right T
  ---@return T
  _.merge_left(left: T, right: T) -> T
  ```

  Returns a new table that is the result of merging the two input tables, with
  values from the left table overriding those from the right.

- ```lua
  ---@generic K, V
  ---@param key K
  ---@param value V
  ---@param tbl table<K, V>
  ---@return table<K, V>
  _.assoc(key: K, value: V, tbl: table<K, V>) -> table<K, V>
  ```

  Returns a new table with the key set to the specified value.

- ```lua
  ---@generic K, V
  ---@param key K
  ---@param tbl table<K, V>
  ---@return table<K, V>
  _.dissoc(key: K, tbl: table<K, V>) -> table<K, V>
  ```

  Returns a new table with the specified key removed.

### Type

- ```lua
  _.is_nil(value: any) -> boolean
  ```

  Checks if the value is nil.

- ```lua
  ---@param typ type
  ---@param value any
  _.is(typ: type, value: any) -> boolean
  ```

  Checks if the type of the value matches the provided type.
````
