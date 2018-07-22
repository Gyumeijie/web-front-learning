Module loaders and bundlers both make it more actionable to write modular JavaScript applications.

# Module loaders

A module loader is typically some library that can load, interpret and execute JavaScript modules you defined using a certain module format/syntax, such as ***AMD*** or ***CommonJS***.

When you write modular JavaScript applications, you usually end up having one file per module. So when writing an application that consist of hundreds of modules it could get quite painful to make sure all files are included and in the correct order. So basically a loader will take care of the ***dependency management*** for you, by making sure all modules are loaded when the application is executed. Some popular module loaders are ***RequireJS*** and ***SystemJS*** .

# Module bundlers

Module bundlers are an ***alternative*** to module loaders. Basically they do the same thing (**manage and load interdependent modules**), but do it as part of the application ***build*** rather than at ***runtime***. So instead of loading dependencies as they appear when your code is executed, a bundler stitches together all modules into a single file (a bundle) before the execution. Take a look at ***Webpack*** and ***Browserify*** as two popular options.

> With Webpack, one can leverage ***Code Splitting*** which allows splitting code into chunks loaded on-demand and thus alleviating the issue of a single monolithic file being produced.

# When to use what?

Which one is better simply depends on your application's ***structure*** and ***size***.

The primary advantage of a bundler is that it leaves you with far fewer files that the browser has to download. This can give your application a performance advantage, as it may decrease the amount of time it takes to load.

However, depending on the ***number of modules*** your application has, this doesn't always have to be the case. Especially for big apps a module loader can sometimes provide the better performance, as loading one huge monolithic file can also block starting your app at the beginning. So that is something you have to simply test and find out.


### Resources
- [What is difference between Module Loader and Module Bundler in JavaScript?
](https://stackoverflow.com/questions/38864933/what-is-difference-between-module-loader-and-module-bundler-in-javascript)
- [a-10-minute-primer-to-javascript-modules-module-formats-module-loaders-and-module-bundlers/](https://www.jvandemo.com/a-10-minute-primer-to-javascript-modules-module-formats-module-loaders-and-module-bundlers/)
