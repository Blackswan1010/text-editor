# JATE
![License: MIT](https://img.shields.io/badge/MIT-blue.svg) 

## Description 

Making a text editor Progressive Web App(PWA).

Deployed Link: []()

## Installation

To start this application, clone the project's repo to your machine and open in your visual code editor. Within the terminal enter 'npm run start' to run the server. Now in your default browser's url bar, enter 'localhost:3000'. To examine the service worker, IndexedDB, and cachedStorage right click the page to Inpsect element, then Application. At the left of the Application's tab you'll see a navigator for Application and Storage that has the service worker, IndexedDB, and cacheStorage. 
 

## Sample PWA
```js
plugins: [
      new HtmlWebpackPlugin({
        title: 'Client Server',
        template: './index.html',
        favicon: './favicon.ico',
      }),

      new InjectManifest({
        swSrc: './src-sw.js',
        swDest: 'src-sw.js',
      }),

      new WebpackPwaManifest({
        fingerprints: false,
        inject: true,
        name: 'Just Another Text Editor',
        short_name: 'JATE',
        description: 'PWA Text Editor!',
        background_color: '#225ca3',
        theme_color: '#225ca3',
        start_url: '/',
        publicPath: '/',
        icons: [
          {
            src: path.resolve('src/images/logo.png'),
            sizes: [96, 128, 192, 256, 384, 512],
            destination: path.join('assets', 'icons'),
          },
        ],
      }),
    ],
```
The configured plugins used to serve the bundles, inject a service worker into the build, and generate a web app manifest file for PWA functionality.


```js
 rules: [
        {
          test: /\.css$/,
          use: ['style-loader', 'css-loader'],
        },
        {
          test: /\.?js$/,
          exclude: /node_modules/,
         
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env'],
              plugins: ['@babel/plugin-proposal-object-rest-spread', '@babel/transform-runtime'],
            },
          },
        },
      ],
```
The rules set here targets css to handle styles and target js files to transpile Javscript code using Babel.


```js
export const putDb = async (content) => {
  console.log('PUT ONE TO DB');
  const jateDB = await openDB('jate', 1);
  const tx = jateDB.transaction('jate', 'readwrite');
  const store = tx.objectStore('jate');
  const request = store.put({id: 1, value: content});

  const result = await request;
  console.log(result);
}

export const getDb = async () => {
  console.log('GET ONE FROM DB')
  const jateDB = await openDB('jate', 1);
  const tx = jateDB.transaction('jate', 'readonly');
  const store = tx.objectStore('jate');
  const request = store.get(1);

  const result = await request;
  console.log(result);
  return result?.value;
}
```
These functions are for updating and retrieving content from the web browser's database.


```js
const butInstall = document.getElementById('buttonInstall');

window.addEventListener('beforeinstallprompt', (event) => {
  
    event.preventDefault();
    
    window.deferredPrompt = event;
    
    butInstall.removeAttribute('hidden');
});

butInstall.addEventListener('click', async () => {
    const promptEvent = window.deferredPrompt;
    if (!promptEvent) {
        
        return;
    }
   
    promptEvent.prompt();

    window.deferredPrompt = null;

    butInstall.classList.toggle('hidden', true);
});

window.addEventListener('appinstalled', (event) => {
    window.deferredPrompt = null;
});
```
The event listeners added for the installation process for the text editor PWA, that shows an installation button and triggering the installation prompt when it is clicked.


## Author Info 

#### Anthony Nguyen

* [https://github.com/Blackswan1010](https://github.com/Blackswan1010) 

