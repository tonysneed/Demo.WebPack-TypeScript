# Demo: WebPack with TypeScript

*Demo of using [WebPack](https://webpack.js.org) with [TypeScript](https://www.typescriptlang.org).*

See <https://www.typescriptlang.org/docs/handbook/react-&-webpack.html>.

1. Install webpack globally

    ```
    npm install -g webpack
    ```

2. Add development-time dependencies on 
   [awesome-typescript-loader](https://www.npmjs.com/package/awesome-typescript-loader), 
   [source-map-loader](https://www.npmjs.com/package/source-map-loader), 
   [concurrently](https://github.com/kimmobrunfeldt/concurrently), and 
   [lite-server](https://github.com/johnpapa/lite-server).

    ```
    npm install --save-dev typescript awesome-typescript-loader source-map-loader concurrently lite-server
    ```

3. Add a TypeScript configuration file.

    ```json
    {
        "compilerOptions": {
            "outDir": "./dist/",
            "sourceMap": true,
            "noImplicitAny": true,
            "module": "commonjs",
            "target": "es5"
        },
        "include": [
            "./src/**/*"
        ]
    }
    ```

4. Add **src** and **dist** folders to the project root.

5. Add a TypeScript file with an export to the src folder.

    ```ts
    // greeter.ts
    export class Greeter {
        greet(): string {
            let message = "Super World";
            return `Hello ${message}!`;
        }
    }
    ```

6. Add a TypeScript file with an import to the src folder.

    ```ts
    // index.ts
    import { Greeter } from "./greeter";

    let greeter = new Greeter();
    let greeting = greeter.greet();

    document.write(greeting);
    ```

7. Add an index.html file to the project root.
    - Include a script tag referencing bundled output

    ```html
    <!doctype html>
    <html>

    <head>
        <meta charset="utf-8" />
        <title>Web Pack Demo</title>
        <script src="./dist/bundle.js"></script>
    </head>

    <body>
    </body>

    </html>
    ```

8. Create a webpack configuration file.
    - Create a webpack.config.js file at the root of the project directory.

    ```js
    module.exports = {
        entry: "./src/index.ts",
        output: {
            filename: "bundle.js",
            path: __dirname + "/dist"
        },

        // Enable sourcemaps for debugging webpack's output.
        devtool: "source-map",

        resolve: {
            // Add '.ts' and '.tsx' as resolvable extensions.
            extensions: ["", ".webpack.js", ".web.js", ".ts", ".tsx", ".js"]
        },

        module: {
            loaders: [
                // All files with a '.ts' or '.tsx' extension will be handled by 'awesome-typescript-loader'.
                { test: /\.tsx?$/, loader: "awesome-typescript-loader" }
            ],

            preLoaders: [
                // All output '.js' files will have any sourcemaps re-processed by 'source-map-loader'.
                { test: /\.js$/, loader: "source-map-loader" }
            ]
        },
    };
    ```

9. Run webpack from the command-line.

    ```
    webpack
    ```

    - Open index.html from the Finder or File Explorer.
        + You should see the greeting displayed: **Hello WebPack!**

10. Add support for running webpack and lite-server concurrently.
    - Add a start script to package.json.

    ```json
    "scripts": {
    "start": "webpack && concurrently \"webpack -w\" \"lite-server\" ",
    "test": "echo \"Error: no test specified\" && exit 1"
    },
    ```

    - Run the app using npm.
        + A browser should open using http://localhost:3000/.
        + You should see the greeting displayed.

    ```
    npm start
    ```

    - Update greeter.ts to change the greeting.
        + Without refreshing the browser, you should see the new greeting displayed.

    