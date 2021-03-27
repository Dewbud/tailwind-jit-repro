using any of the following
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
either by importing a .css file
or including in the style tags of a Vue SFC

results in
```
[vite] Internal server error: Maximum call stack size exceeded
  Plugin: vite:css
  File: /home/anonymous/tailwind-jit-repro/src/styles.css
      at candidatePermutations (/home/anonymous/tailwind-jit-repro/node_modules/@tailwindcss/jit/src/lib/generateRules.js:19:32)
      at candidatePermutations.next (<anonymous>)
      at candidatePermutations (/home/anonymous/tailwind-jit-repro/node_modules/@tailwindcss/jit/src/lib/generateRules.js:41:10)
      at candidatePermutations.next (<anonymous>)
      at candidatePermutations (/home/anonymous/tailwind-jit-repro/node_modules/@tailwindcss/jit/src/lib/generateRules.js:41:10)
      at candidatePermutations.next (<anonymous>)
      at candidatePermutations (/home/anonymous/tailwind-jit-repro/node_modules/@tailwindcss/jit/src/lib/generateRules.js:41:10)
      at candidatePermutations.next (<anonymous>)
      at candidatePermutations (/home/anonymous/tailwind-jit-repro/node_modules/@tailwindcss/jit/src/lib/generateRules.js:41:10)
      at candidatePermutations.next (<anonymous>) (x2)
```

Example:
```ts
defineComponent({
  name: 'HelloWorld',
  props: {
    msg: {
      type: String,
      required: true
    },
    things: Array as PropType<string[]>,
  },
  setup: () => {
    const count = ref(0)
    return {
      count,
      stuff: [] as string[] | undefined,
    }
  }
})
```

`candidatePermutations` just yields forever when it hits the line containing:

`things: Array as PropType<string[]>`
```
// console.log(candidate, lastIndex)
defineComponent, Infinity
PropType Infinity
Component Infinity
props Infinity
things Infinity
Array Infinity
as Infinity
string[] Infinity
string[] 4
string[] 4
string[] 4
```

Some other Typescript syntaxes that have caused this

```ts
const cats: Cat[] = [
    {}
];

let items = [] as string[] | undefined;

type ExampleContext = {
  testMethod<K extends keyof Example>(
    key: K,
    payload?: Parameters<Example[K]>[1]
  ): ReturnType<Example[K]>;
};
```

Not sure why this only happen when using `@tailwind` rules but I'm sure there's regex wizardry involved.
