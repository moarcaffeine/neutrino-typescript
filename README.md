# Neutrino Typescript

Transpiles TypeScript as part of the build process.

This follows the configuration suggested in
[the official TypeScript / Babel announcement](https://devblogs.microsoft.com/typescript/typescript-and-babel-7/).

## Installation

1. Install dependencies:

   ```bash
   npm install --save-dev git+https://github.com/davidje13/neutrino-typescript#semver:^1.0.8
   npm install --save-dev typescript
   ```

2. Create `tsconfig.json`:

   ```json
   {
     "compilerOptions": {
       "target": "esnext",
       "module": "esnext",
       "moduleResolution": "node",
       "allowJs": true,
       "noEmit": true,
       "strict": true,
       "isolatedModules": true,
       "esModuleInterop": true,
       "resolveJsonModule": true
     },
     "include": [
       "src",
       "test"
     ]
   }
   ```

   _Note: You should list all your source / test folders in `include`.
   You can also set `strict` to false if preferred._

3. Include in `.neutrinorc.js`:

   ```javascript
   const typescript = require('neutrino-typescript');
   // ...

   module.exports = {
     use: [
       typescript(), // must be first in use section
       // ...
       node(), // or whichever target you are using
     ],
   };
   ```

4. Include type checking in `package.json` scripts:

   ```json
   {
     "scripts": {
       "lint": "tsc"
     }
   },
   ```

   To combine this with other linting steps, you can join the commands like so:

   ```json
   {
     "scripts": {
       "lint": "existing lint command && tsc"
     }
   },
   ```

### Generating declaration (`.d.ts`) files

If you are creating a library, you probably want to include a `.d.ts` file.
This can be turned on by specifying `typescript({ declaration: true })` in
your `.neutrinorc.js` file. By default, this will also generate a sourcemap.
You can disable this by setting `declarationMap: false`.

One declaration file will be generated for each entrypoint you have specified
in `mains`. The file will be named to match the input file (for default
neutrino configuration, this means you will get `build/index.d.ts`).

## Linting with ESLint

If you want to use eslint with typescript, you can install the
[neutrino-typescript-eslint](https://github.com/davidje13/neutrino-typescript-eslint#readme) module.

## Testing with Jest

This will work out-of-the-box with Jest, but you will need to install the Jest types:

```bash
npm install --save-dev @types/jest
```
