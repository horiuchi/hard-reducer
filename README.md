# hard-reducer

Type friendly facade for better reducer.

```sh
npm install hard-reducer -S
# or
yarn add hard-reducer
```

## Concepts

* Type safe interface
* Avoid redundant `type` string definitions
* Keep reducer interface `(State, Action) => State` to use with `redux.combineReducers()`
* Handle Flux Standard Action `<Payload>{ type: string, payload: Payload }`

Check this code to know detail.

[Flow playground](https://flow.org/try/#0PQKgBAAgZgNg9gdzCYAoUYCmBnGBLAOwBcBaAEz2wEMAjGTZNIgTwAcGBBAYyLzgIDCAJ0xUicIQB4B-bESFVCRAFxg5QwgHMANGACSBVgFciugApVm8KmQB8YALxgA3qjBgAFAeNEAlKucwFnZVGQJ1RWJdVktrMlULKzgbMABfVHTUYIYAJUwyIy5MKQBlIjFMeydXdw8yit0qAmZ-MHqiTG03ME1MFU9fR3s8gqLS8o7bLvcuKmxMaVl5SNN9QxNzWOS7D273bl5+YVFxKTCIpV1vDbBEuKm9z3bO262bQYd7Z+7WkcLiyTPWwZVCoKBGAg8PgEMA0Ix4GBkA7Q45iCQeOCsIjYAJgVgiKB4AAeAH5VOotGlBjUwFwlniCcTHGBMdiAHT4zCEolgAA+vLAAHJBaD3ODIYcYVwRBVkfxFuFlkpyfItFd1qs7ttbLt3O5+uclVFHlACKprkQhq8kjYfqo5YIZacFRcomsfJsbXYXI86YqgmwGE5OdywABqIK++lQagAMTNYCazGZHkIPnNGo+9hperAIiIRiEMJzuYD7GmpfcMS9qlNYAAZPWwKbU5nHu50rnO3qY1R42yAPqWpzZR75wsw3vx7qZMUQqH8WlOzBmIRwAC2lEwDpdRv6FIIOndNy1Nh1jwNSwUlxNCYtVtXG63klP3oFr7tYAdqOdhuvbotT17h9XM-TkMsgwZLkmQjIgo39KcEyTFM0xMDMfCzEDS3HIssMrbIK0ras4gSNdN3mNkRGwOAYAAN0wDwW1QvxfHbNJHm7MU4wIQdhwgsc+gnZtuJnUVm3nSUlxOTASjwddWHoHc-xWFUNEPc9c0vRV-zg9xWm-J0JF3HTdFouA8DIUzzO9EswMtbJmRDGDI1A6NuNUZCnA8TCPECBzUlY3NEN45lR1zHDJxEjsxIilwpNlBcCF0aVpNk+TFMS5Ll0fcjt0S9jMnFfKUoqP4xkBCZKlbPBeCoGBnlUZ5fnyf5xgqbNunoS0oBETAAC98mZKA6vmbo7LAdcqFYZkCEwJAAFkpu8sTxpEUZig85oUzkCpNuTJxCBqvA6ueRpEr2zDbPpAALJoyHoIRmUm1g2V6IgPCoRK2WyQK9Vi26CHu4owBJMAAaBoQPB2jozslDk3jIQYVQqUTujW1q2VmeYU0+yVY3nZK6pgGhPoAa0ux5nrZeZ3tx6F8chXjCZgYmyd+9xYvRsZRI5wTcK54oQXQYAwCMahelBcaaRKjoHQrGWZLkhS8sleXsrIrcHQyZk4QRJFEp-dEaScolVEFOkIQ6IRgBFALJfpKi+mZBW0uVh0PEFR2iEFVjxsILhneXd3BX9wVdA8Wi6tUAgjHXGhikwyOYF9+kyEwAOnAV4O064MPPCT6PY-joRE7qlP-X9jhsGYSFA+knLNcSj3-ZIOYa9z8OC7AGO44Tq0aVihuKKomj6IjsuMlYrJAzaSrmUCJOjEwQve8e9I-YII6Trn6owEX5ewAABnY1a69Klqxmq2r6sq37MbmBj-fD6GXmI7YKdc-1WFUylgwRsBgAi0PAJAsuESzuH3sjDobJ97hjxAjDiM475YwYjnZ+lVogIw-n9PmxY2KQLUJVGBdUl5gBIPAr0iCOy-UAXmTAABHeEIg6EFCJCQfET55gkE3GQIGCAqAiG6PfeYrYuBV3bughoFC4jYJmA7PoK9i6OQQZ-cC+YABMP9DzKK9AAoBmgQFCUOtfb41ChEoI8F7SRMNpHv37vBNRCju5F2Bn-XRtDgHhVwWAYxx0b4ozMagVafRj5OEhgvEhB9j6pF0P7DwABmXw5dHFEAAIzMkhvmQ+ugvbeWSZaDRGTLF9FSboHOHh1FJKCbIGimA2TwE0MUoglTUBAA)

## Examples

```js
/* @flow */
import { buildActionCreator, createReducer } from 'hard-reducer'
const { createAction } = buildActionCreator({ prefix: 'counter/' })

const inc = createAction('inc', (val: number) => val)
const dec = createAction('dec', (val: number) => val)

inc(1) //=> { type: 'counter/inc', payload: 1 }

type State = { value: number }

const initialState = { value: 0 }

const reducer = createReducer(initialState)
  // Handle `(State, Payload) => State` in matched context.
  .case(inc, (state, payload) => {
    return {
      value: state.value + payload
    }
  })
  .case(dec, (state, payload) => {
    // $ExpectError
    const p: string = payload
    return {
      value: state.value - payload
    }
  })

// Use it
const ret0 = reducer(initialState, inc(3))
const ret1 = reducer(ret1, dec(1))
```

Use with explicit types.

```js
/* @flow */
import { buildActionCreator } from 'hard-reducer'
import type { ActionCreator, Reducer } from 'hard-reducer/types'

const { createAction } = buildActionCreator({ prefix: 'xxx/' })
const foo: ActionCreator<number, string> = createAction('foo', (val: number) =>
  val.toString()
)

const reducer: Reducer<{
  /* type definition */
}> = createReducer(/* initialState */)
```

See more on `index.js.flow`

## Related projects

* [reduxactions/redux-actions: Flux Standard Action utilities for Redux.](https://github.com/reduxactions/redux-actions)
* [aikoven/typescript-fsa: Type-safe action creator utilities](https://github.com/aikoven/typescript-fsa)
* [acdlite/flux-standard-action: A human-friendly standard for Flux action objects.](https://github.com/acdlite/flux-standard-action)

## TODO

* TypeScript support by index.d.ts
* Error Cases

## LICENSE

MIT
