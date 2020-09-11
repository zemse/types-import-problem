Discussion: [ethereum-ts/TypeChain#278](https://github.com/ethereum-ts/TypeChain/issues/278)

This is a reproduction of a problem, when `dependencies/index.ts` that exports both a type defination file (`dependencies/type.d.ts`) and a typescript file (`const.ts`), is imported in `main.ts`.

Typescript compiler generates `main.js`, `dependencies/const.js` and `dependencies/index.js` that exports (`./const` and `./type`). But since `dependencies/type` is not resolved by node js, we get following error when doing `node main.js`:

```sh
$ node main.js
internal/modules/cjs/loader.js:969
  throw err;
  ^

Error: Cannot find module './type'
Require stack:
- <path-to-dir>/types-import-problem/dependencies/index.js
- <path-to-dir>/types-import-problem/main.js
```

_Note: Actual path is replaced by `<path-to-dir>`_

### File Structure

```js
├── dependencies
│   ├── type.d.ts // exports an interface
│   ├── const.ts // exports a simple number
│   └── index.ts // exports all from type and const
└── main.ts // imports the interface and the number but from dependencies/index.ts
```
