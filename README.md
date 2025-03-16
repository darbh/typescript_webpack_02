# typescript_webpack_02
# Typescript Empty Project

## Creating Docker container 
> docker run --name node01 -it -v /data:/data ubuntu:22.04 bash

> docker run --name node01 -it -v /data:/data ubuntu_22.04_2025-03-16:22.04:latest bash

> docker run --name node02 -it -v /data:/data ubuntu_22.04_2025-03-16_v01:latest bash

## Bellow all commands are inside container
```bash
apt update
apt install wget
```

## [Installing NVM](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating)
```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash

# Add to vim ~/.bashrc
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

# InstallingLatest Node
nvm install node 
node -v 
```

## Installing global node modules
```bash
npm install -g typescript typescript ts-loader webpack \
webpack-cli \
wrapper-webpack-plugin \
declaration-bundler-webpack-plugin \
copy-webpack-plugin \
clean-webpack-plugin \
@types/node  
```

# Generating config
```bash
cd /data/repo/aushadh.co/aushadh.co_2024-06-02/public_html/price_calculator/
npm init --y # package.json
tsc --init  --outDir ./dist # tsconfig.json 
npm install -D wrapper-webpack-plugin webpack-cli ts-loader 
```
 
## vim webpack.config.js
```js
const path = require('path');
const WrapperPlugin = require('wrapper-webpack-plugin');
module.exports = {
    entry: './src/index.ts',
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                use: 'ts-loader',
                exclude: /node_modules/,
            },
        ],
    },
    resolve: {
        extensions: ['.tsx', '.ts', '.js'],
    },
    plugins: [
        // strict mode for the whole bundle
        new WrapperPlugin({
            test: /\.js$/, // only wrap output of bundle files with '.js' extension
            header: '(function (W,D,N) { "use strict";W[N]=W[N]||{};if(W[N].hb23){return;}W[N].hb23="20221228";\n',
            footer: '\n})(window,document,"__HPLib");'
        })
    ],
    mode: "production",
    output: {
        chunkLoadingGlobal: 'HPChunk',
        chunkLoading: 'jsonp',
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
        // libraryTarget: 'amd',
        // library: "AffHb2023",
    }
};
```

## Sample riles
```bash
mkdir src

echo  "export function add( a:number , b:number ) : number { return a + b ; } " > src/lib.ts
echo -ne "import { add } from './lib'
console.log( add(1,3) )
" > src/index.ts
echo -ne "<html><head><script src='dist/bundle.js'></script></head><body></html>" > index.html

```

## Build 
```bash

npx webpack
cat dist/bundle.js
cat dist/bundle.js.map

npx webpack watch --mode development  
npx webpack watch
```

## Open index.html in browser

## TODO: 
- Add user  
    - useradd --create-home --home-dir /home/hemant --shell /bin/bash hemant
    - su - hemant 
- Do NVM setup for added User 
