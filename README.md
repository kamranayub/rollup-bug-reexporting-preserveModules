## Rollup Bug Reproduction

To build:

    yarn
    yarn build

View build output: [dist/index.js](dist/index.js)

## The Problem

With `preserveModules` set to `true`, Rollup does not seem to de-dupe for nested modules when using `export *`.

### Expected Output

```js
import * as index from './module-a/v1/index.js';
export { index as ModuleA_V1 };
import * as index$1 from './module-b/v1/index.js';
export { index$1 as ModuleB_V1 };
```

### Actual Output

```js
import * as index from './module-a/v1/index.js';
export { index as ModuleA_V1 };
import * as index from './module-b/v1/index.js';
export { index as ModuleB_V1 };
```

Note the invalid duplicate identifier `index` being imported.