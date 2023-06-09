# Typing an Express App

## Config 

  ---

  > Pro Tip : Take the time needed to create a good setup for yourself and your team, so that everything runs smoothly in the long run

  ---

  1. Start a new express app:
       `npm init -y`
  2. install the typescript package as a dev dependency.
        `npm install typescript --save-dev`
  
  3. TypeScript's Native Compiler `(tsc)` helps us initialize our project by generating our `tsconfig.json` file. `Add` the `tsc` command to the list of executable `scripts` in `package.json`, like so :

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

  4. Now initialize our tsconfig.json settings by running:

       `npm run tsc -- --init`
  
  5. Let the initial `.tsconfig` file have the following values :
           
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

  6. Next, install `express` and the various `types` :

     `npm install express`  

      `npm install --save-dev eslint @types/express @typescript-eslint/eslint-plugin @typescript-eslint/parser` 

      > Confirm that the `package.json` looks like the following :
          
            {
                "name": "whatever",
                "version": "1.0.0",
                "description": "",
                "main": "index.js",
                "scripts": {
                  "tsc": "tsc"
                },
                "author": "",
                "license": "ISC",
                "devDependencies": {
                  "@types/express": "^4.17.13",
                  "@typescript-eslint/eslint-plugin": "^5.12.1",
                  "@typescript-eslint/parser": "^5.12.1",
                  "eslint": "^8.9.0",
                  "typescript": "^4.5.5"
                },
                "dependencies": {
                  "express": "^4.17.3"
                }
          }
  7.  Create an `.eslintrc` file with the following settings : 
      
      ```json 

        {
        "extends": [
          "eslint:recommended",
          "plugin:@typescript-eslint/recommended",
          "plugin:@typescript-eslint/recommended-requiring-type-checking"
        ],
        "plugins": ["@typescript-eslint"],
        "env": {
          "browser": true,
          "es6": true,
          "node": true
        },
        "rules": {
          "@typescript-eslint/semi": ["error"],
          "@typescript-eslint/explicit-function-return-type": "off",
          "@typescript-eslint/explicit-module-boundary-types": "off",
          "@typescript-eslint/restrict-template-expressions": "off",
          "@typescript-eslint/restrict-plus-operands": "off",
          "@typescript-eslint/no-unsafe-member-access": "off",
          "@typescript-eslint/no-unused-vars": [
            "error",
            { "argsIgnorePattern": "^_" }
          ],
          "no-case-declarations": "off"
        },
        "parser": "@typescript-eslint/parser",
          "parserOptions": {
            "project": "./tsconfig.json"
          }
        }
   
      ```
  8. install `ts-node-dev` as a dev dependency , for continuous refresh on save.
   
      `npm install --save-dev ts-node-dev`
  
  9. Now go to `package.json` and write down the scripts to run `development` and `linting`, like so :
     
     ```json
        {
    
          "scripts": {
            "tsc": "tsc",
            "dev": "ts-node-dev index.ts",
            "lint": "eslint --ext .ts ."
          },
         
        }
     ```
   
  10. Create a `Production Build` by running the TypeScript compiler  : 
      >  Imp : Make sure you have _At Least ONE_ `.ts` file before running this command ,else the compiler will complain
      
        Execute :  `npm run tsc`
  
       >  Since we have defined the `outdir` in our `tsconfig.json`, nothing's left but to run the script `npm run tsc`.
      
       > A native runnable JavaScript production build of the Express backend is created in file `index.js` inside the directory `build`. 
     
  11. Currently, if we run ESlint it will also interpret the files in the build  directory .   We don't want that, an it's `.js` code created by the compiler. We can `prevent this` by `creating` a `.eslintignore` file that lists the content we want `ESlint to ignore`.
      
       ```json
         // .eslintignore
          build  //add this to the newly created .eslintignore file
       
       ```
      
  
  1. `Add` an npm `script` in `package.json` for running the application in `Production Mode`
   
      ```json
             {
          // ...
          "scripts": {
            "tsc": "tsc",
            "dev": "ts-node-dev index.ts",
            "lint": "eslint --ext .ts .",
            "start": "node build/index.js"
          },
          // ...
        }
      ```
  1. Finally run the app in production mode by running the script `npm run start` and confirm that the app is running successfully in production, by adding the following starter code to `index.ts` and compiling it.

  > IMP : Before executing  `npm run start` , do run the `npm run tsc` script to compile the `.ts` code to `.js` code first.   
---
  ```javascript
               
        import express from 'express';
        const app = express();
        app.use(express.json());
        
        const PORT = 3000;
        
        app.get('/ping', (_req, res) => {
          console.log('someone pinged here');
          res.send('pong');
        });
        
        app.listen(PORT, () => {
          console.log(`Server running on port ${PORT}`);
        });

  ```
  ---


 > Create a `.gitignore` file and add `node_modules` to it before committing to github

---
