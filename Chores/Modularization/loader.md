# Module loaders

A module loader interprets and loads a module written in a certain [module format](./format.md). 

A module loader runs at **runtime** :star: :

- :one:	load the module loader in the browser
- :two: tell the module loader which main app or entry file to load
- :three: the module loader downloads and interprets the main app file
- :four: the module loader downloads files as needed

If you open the network tab in your browser's developer console, you will see that many files are loaded on demand by the **module loader**.

A few examples of popular module loaders are:

- RequireJS: loader for modules in AMD format
- SystemJS: loader for modules in AMD, CommonJS, UMD or System.register format

