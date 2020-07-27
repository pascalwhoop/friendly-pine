---
title: Writing tests for a NodeJS app with Jasmine in Typescript
date: '2017-06-16T13:54:05.211Z'
excerpt: >-
  Run down: Don’t use this, it’s too old:
  https://github.com/mhevery/jasmine-node
layout: post
---
Run down: Don’t use this, it’s too old: [https://github.com/mhevery/jasmine-node](https://github.com/mhevery/jasmine-node)

Just run

`npm install --save-dev jasmine @types/jasmine nodemon`

add the code below to a file called`jasmine.json`

    {  
      "spec_dir": "dist",  
      "spec_files": [  
        "**/*[sP]pec.js"  
      ],  
      "stopSpecOnExpectationFailure": **true  
    **}

then add this line to your package json

    “test”: “node_modules/nodemon/bin/nodemon.js -w dist ./node_modules/.bin/jasmine — config=jasmine.json”,

and make sure you have your tsc compiler set to output its results in the `dist` folder. Now every time the files compile to javascript, jasmine picks them up and runs your tests. This way you can continue developing normally with your workflow. I for example use this to keep my workflow running quickly (in `package.json`)

    "watch": "node_modules/concurrently/src/main.js -k -p \"[{name}]\" -n \"TypeScript,Node\" -c \"yellow.bold,cyan.bold,green.bold\" \"npm run watch-ts\" \"node_modules/nodemon/bin/nodemon.js --inspect --delay 500ms dist/app/index.js\"",

with the tsconfig.json like so

    {  
      "include": [  
        "src/**/*"  
      ],  
      "compilerOptions": {  
        "target": "ES6",  
        "module": "commonjs",
    "sourceMap": true,                       
        "outDir": "./dist",  
        "rootDir": "./src",  
         "removeComments": false                 
      }  
    }

Hope it helps ☺
