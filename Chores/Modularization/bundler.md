# Module bundlers

A module bundler replaces a module loader.

But, in contrast to a module loader, a module bundler runs at **build time** :star: :

- :one: run the module bundler to generate a bundle file at build time (e.g. bundle.js)
- :two: load the bundle in the browser

If you open the network tab in your browser's developer console, you will see that only **one** file is loaded. No module loader is needed in the browser. **All code is included in the bundle**.

Examples of popular module bundlers are:

- Browserify: bundler for CommonJS modules
- Webpack: bundler for AMD, CommonJS, ES6 modules
