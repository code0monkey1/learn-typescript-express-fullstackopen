# Typing an Express App

## Config 

  1. Start a new express app:
       `run npm init -y`
  1. install the typescript package as a dev dependency.
        `npm install typescript --save-dev`
  
  1. TypeScript's Native Compiler `(tsc)` helps us initialize our project by generating our `tsconfig.json` file. `Add` the `tsc` command to the list of executable `scripts` in `package.json`, like so :
   
      ```json
       {
        // ..
        "scripts": {
          "tsc": "tsc"
        },
        // ..
      }
      ```
      > The bare tsc command is often added to scripts so that other scripts can use it.
  1. Now initialize our tsconfig.json settings by running:

       `npm run tsc -- --init`
  
  1. Let the initial `.tsconfig` file have the following values :
           
      ```json
                {
              "compilerOptions": {
                "target": "ES6",
                "outDir": "./build/",
                "module": "commonjs",
                "strict": true,
                "noUnusedLocals": true,
                "noUnusedParameters": true,
                "noImplicitReturns": true,
                "noFallthroughCasesInSwitch": true,
                "esModuleInterop": true
              }
          }
      
     ```
     #### Reason for choosing the above `.tsconfig` presets : 

     > `target` configuration tells the compiler which ECMAScript version to use when generating JavaScript. ES6 is supported by most browsers, so it is a good and safe option.
     
     > `outDir` tells where the compiled code should be placed.

     > `module` tells the compiler that we want to use `CommonJS` modules in the compiled code. This means we can use the old require syntax instead of the import one, which is not supported in older versions of Node, such as version 10.

     > `strict` is a shorthand for multiple separate options: _noImplicitAny, noImplicitThis, alwaysStrict, strictBindCallApply, strictNullChecks, strictFunctionTypes and strictPropertyInitialization_.
     >
     > Using strict is suggested by the official *_[tsconfig documentation](https://www.staging-typescript.org/tsconfig#strict)_*

     > `noUnusedLocals` prevents having unused local variables.
     
     > `noUnusedParameters` throws an error if a function has unused parameters

     > `noImplicitReturns` checks all code paths in a function to ensure they return a value.

     > `noFallthroughCasesInSwitch` ensures that, in a switch case, each case ends either with a return or a break statement.

     > `esModuleInterop` allows interoperability between CommonJS and ES Modules. 