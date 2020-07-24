# Install Wordpress to XAMPP folder
[full YouTube guide](https://www.youtube.com/watch?time_continue=604&v=CTNFZRdDotM&feature=emb_title)
- start Apache and MySql in XAMPP Control Panel
- dowload wordpress zip from [wordpress.org](https://wordpress.org/download/)
- extract in xampp/htdocs folder and rename to your website name
- go to ```localhost/``` and create a database for the new website
- go to ```localhost/website-name``` and follow the installation process, database user is root with blank password
- go to ```localhost/website-name/wp-admin``` to login

# Install Underscore
- download theme zip from [underscores.me](https://underscores.me/)
- extract folder to ```xampp/htdocs/website-name/wp-content/themes```
- login to ```localhost/website-name/wp-admin```
- activate theme in Appearance/Themes menu

# Install Tailwind CSS within Underscore
- open ```xampp/htdocs/website-name/wp-content/themes/theme-name``` in code editor
- run in terminal ```npm i --save-dev tailwindcss postcss-cli autoprefixer laravel-mix cross-env  ```
- run ```npx tailwind init```
- in the root of the theme folder create postcss.config.js file
with code inside:
``` js
module.exports = {
	plugins:[
        require('tailwindcss'),
        require('autoprefixer'),
    ]
}
```
- in the root of the theme folder create webpack.config.js file with a code in it
``` js
const mix = require('laravel-mix');
const tailwindcss = require('tailwindcss');

mix.postCss('./resources/css/app.css','./css/app.css',[
    tailwindcss('./tailwind.config.js')
]);
mix.js('./resources/js/app.js', './js/app.js');

```
- create directory and file ```resources/css/app.css``` with the code in it
``` css
@tailwind base;

@tailwind components;

@tailwind utilities;
```
- create folder and file ```resources/js/app.js```
- add this code to package.json/ scripts section from [laravel-mix website](https://laravel-mix.com/docs/5.0/installation) 
```js
"scripts": {
    "dev": "npm run development",
    "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
    "watch": "npm run development -- --watch",
    "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
    "prod": "npm run production",
    "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
}
```
- run ``` npm run dev ``` 
- change within ```functions.php```
``` php
function ottawa_train_yards_v_1_scripts() {
	wp_enqueue_style( 'ottawa-train-yards-v-1-style', get_template_directory_uri() . 'css/app.css' );
	wp_style_add_data( 'ottawa-train-yards-v-1-style', 'rtl', 'replace' );

	wp_enqueue_script( 'ottawa-train-yards-v-1-navigation', get_template_directory_uri() . '/js/navigation.js', array(), _S_VERSION, true );

	if ( is_singular() && comments_open() && get_option( 'thread_comments' ) ) {
		wp_enqueue_script( 'comment-reply' );
	}
}
```
- save old styles from ```style.css``` to ```old-styles.css``` to refer to after if needed
- run ```npm run dev``` to rebuild
- create ```.gitignore``` file
