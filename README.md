# Neodash.nvim

Shamelessly named after the JavaScript package, [Lodash](https://lodash.com).

Also, the initial commit was shamelessly copy-pasted from
[mason.nvim](https://github.com/williamboman/mason.nvim/tree/f7f81ab41b153e2902ebded401a8a0a6abe28607/lua/mason-core/functional)
(sorry, it was just so well done).

## But why?

- I wanted to package this in its own plugin because I found I kept wanting the
  same functionality in several of my own plugins.

- Documentation. Lua packages tend to suck at this.

## Installation

Using [lazy.nvim](https://github.com/folke/lazy.nvim)

```lua
-- init.lua
{
    'darksinge/neodash.nvim',
}
```

## Usage

### Data

```lua
_.table_pack(...)
TODO: document
```

`_.enum`:

`_.set_of`:

### Functions

`_.curryN`:
`_.compose`:
`_.partial`:
`_.identity`:
`_.always`:
`_.T`:
`_.F`:
`_.memoize`:
`_.lazy`:
`_.tap`:
`_.apply_to`:
`_.apply`:
`_.converge`:
`_.apply_spec`:

### Lists

```lua
_.reverse(list: T[]) -> T[]
```

Reverses the given list and returns the result.

---

```lua
_.list_not_nil(...) -> T[]
```

Returns a list of arguments where nil values are omitted.

- `_.find_first(predicate: fun(item: T): boolean, list: T[]) -> T | nil`
  Returns the first item in the list that satisfies the predicate function. If no such item is found, returns nil.

- `_.any(predicate: fun(item: T): boolean, list: T[]) -> boolean`
  Checks if any item in the list satisfies the predicate function. Returns true if at least one item does, else false.

- `_.all(predicate: fun(item: T): boolean, list: T[]) -> boolean`
  Checks if all items in the list satisfy the predicate function. Returns true only if all items satisfy the predicate.

- `_.filter(filter_fn: (fun(item: T): boolean), items: T[]) -> T[]`
  Filters the items in the list based on the filter function and returns the resulting list.

- `_.map(map_fn: (fun(item: T): U), items: T[]) -> U[]`
  Applies the map function to all items in the list and returns the resulting list.

- `_.flatten(value: any[]) -> any[]`
  Flattens a nested list into a single level list.

- `_.filter_map(map_fn: fun(item: T): Optional, list: T[]) -> any[]`
  Applies the map function to all items in the list and filters out items that are not present.

- `_.each(fn: fun(item: T, index: integer), list: T[])`
  Invokes the function on each item in the list. The function is called with two arguments: the item and its index.

- `_.list_copy(list: T[]) -> T[]`
  Returns a copy of the given list.

- `_.concat(a: any, b: any) -> any`
  Concatenates two lists or strings.

- `_.append(value: T, list: T[]) -> T[]`
  Appends a value to the end of a list and returns the new list.

- `_.prepend(value: T, list: T[]) -> T[]`
  Prepends a value to the start of a list and returns the new list.

- `_.zip_table(keys: T[], values: U[]) -> table<T, U>`
  Returns a new table where keys from one list are associated with values from another list.

- `_.nth(offset: number, value: T[]|string) -> T|string|nil`
  Returns the item at the given offset. For a negative offset, items are counted from the end.

- `_.head(value: T[]|string) -> T|string|nil`
  Returns the first item of a list or string.

- `_.last(list: T[]) -> T?`
  Returns the last item from a list.

- `_.length(value: string|any[]) -> integer`
  Returns the length of a list or string.

- `_.sort_by(comp: fun(item: T): any, list: T[]) -> T[]`
  Sorts a list based on the comparison function and returns the sorted list.

- `_.join(sep: string, list: any[]) -> string`
  Joins a list into a string with the given separator.

- `_.uniq_by(id: fun(item: T): any, list: T[]) -> T[]`
  Returns a list with unique items based on the id function.

- `_.partition(predicate: fun(item: T): boolean, list: T[]) -> T[][]`
  Partitions a list into two lists, where the first contains items that satisfy the predicate and the second contains items that don't.

- `_.take(n: integer, list: T[]) -> T[]`
  Returns the first n items from a list.

- `_.drop(n: integer, list: T[]) -> T[]`
  Removes the first n items from a list and returns the remaining items.

- `_.drop_last(n: integer, list: T[]) -> T[]`
  Removes the last n items from a list and returns the remaining items.

- `_.reduce(fn: fun(acc: U, item: T): U, acc: U, list: T[]) -> U`
  Reduces a list to a single value by iteratively applying the function to an accumulator and each item in the list.

- `_.split_every(n: integer, list: T[]) -> T[][]`
  Splits a list into sub-lists each containing n items, except for the last one which may contain less.

- `_.index_by(index: fun(item: T): U, list: T[]) -> table<U, T>`
  Returns a table where each item in the list is indexed by the result of the index function.
