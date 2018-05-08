# Bash Functions

## Make and Goto (stolen directly from Steve)

Makes a new directory and moves into it from the terminal

* Add to aliases.sh:

```mg() {[ -n "$1" ] && mkdir -p "$@" && cd $_;}```

* To call from bash (once you're in the desired parent directory):

```mg [directory name]```

## Simple HTML (also stolen but tweaked)

Quick-create and links the following files for an HTML project:

* index.html
* styles.css
* main.js

Add to aliases.sh:

```bash
simplehtml () {
echo '<!doctype html> 
<html lang="en">
<head>
<meta charset="utf-8">
<title>My Awesome Page</title>
<link rel="stylesheet" type="text/css" href="./styles.css">

</head>

<body>
<section id="contentHook">
</section>

<script src="main.js"></script>
</body>
</html>' >> index.html

touch styles.css
touch main.js
code .
}
```

* To call from bash (once you're in the desired parent directory):

```simplehtml```

## Create New GitHub Repository

This function will create a new repository with a prepopulated .gitignore file and an empty readme. It will also link the repo to GitHub and make the first commit.

* $1 is link to GitHub repo copied from website

Add to aliases.sh:

```bash

gnr () {
echo '# Windows thumbnail cache files
Thumbs.db
ehthumbs.db
ehthumbs_vista.db

# Dump file
*.stackdump

# Folder config file
[Dd]esktop.ini

# Recycle Bin used on file shares
$RECYCLE.BIN/

# Windows Installer files
*.cab
*.msi
*.msm
*.msp

# Windows shortcuts
*.lnk

#other
node_modules/
dist/
.DS_Store
.sass-cache
.stylelintrc
bower_components
__pycache__
*.pyc
bin
obj
*.db
*.sqlite3
.vscode' >> .gitignore

echo '# Project:



## Requirements:



## Currently Working On:



## Remaining Features:



## Stretch Goals:



## Completed Features:



## Data Structure:



' >> README.md
git init
git add .
git commit -m "first commit"
git remote add origin $1
git push -u origin master

}

```

To call from bash (once you're in the desired parent directory):

```gnr [repo link copied from GitHub]```

## Initialize A New HTML Project

This function creates the directory structure and the following files per class requirements:

* dist
* src
  * (dir) scripts
    * main.js
  * (dir) styles
    * styles.css
  * .eslintrc
  * Gruntfile.js
  * index.html
  * package.json
* .gitignore

It then initializes node and installs the packages listed in package.json.

Add to aliases.sh

```bash
htmlinit () {

mkdir dist
mkdir src
# Add anything that belongs in the root directory above this line

mkdir ./src/styles
mkdir ./src/scripts
touch ./src/scripts/main.js
touch ./src/styles/styles.css

echo '<!doctype html> 
<html lang="en">
<head>
<meta charset="utf-8">
<title>'My Awesome Page'</title>
<link rel="stylesheet" type="text/css" href="./styles/styles.css">

</head>

<body>
<nav></nav>

<header></header>

<section id="contentHook">
</section>

<footer></footer>

<script src="./scripts/main.js"></script>
</body>
</html>' >> ./src/index.html

echo '{
"parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module",
    "ecmaFeatures": {
        "jsx": true
    }
},
"rules": {
    "semi": 0,
    "quotes": [
        "error",
        "double"
    ],
    "eqeqeq": 2,
    "no-trailing-spaces": 2,
    "no-unused-vars": "off"
}
}' >> ./src/.eslintrc

echo '{
"name": "src",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1"
},
"author": "",
"license": "ISC",
"devDependencies": {
"grunt": "^1.0.2",
"grunt-browserify": "^5.3.0",
"grunt-contrib-copy": "^1.0.0",
"grunt-contrib-uglify-es": "git://github.com/gruntjs/grunt-contrib-uglify.git#harmony",
"grunt-contrib-watch": "^1.0.0",
"grunt-eslint": "^20.1.0",
"grunt-sass": "^2.1.0"
},
"dependencies": {}
}' >> ./src/package.json

echo '
module.exports = function (grunt) {
// Project configuration.
grunt.initConfig({
    pkg: grunt.file.readJSON("package.json"),
    watch: {
        scripts: {
            files: [
                "./scripts/**/*.js",
                "./index.html",
                "./styles/**/*.scss",
                "!node_modules/**/*.js"
            ],
            tasks: ["eslint", "sass", "browserify", "copy"],
            options: {
                spawn: false,
            },
        }
    },
    eslint: {
        src: [
            "./scripts/**/*.js",
            "!node_modules/**/*.js"
        ]
    },
        sass: {
        options: {
            sourceMap: true
        },
        dist: {
            files: {
                "./styles/styles.css": "./styles/styles.scss"
            }
        }
        },
    browserify: {
        options: {
            browserifyOptions: {
                debug: true,
                paths: ["./scripts"],
            }
        },
        dist: {
            files: {
                "../dist/bundle.js": ["scripts/**/*.js"]
            }
        }
    },
    copy: {
        main: {
            files: [
                //includes files within path
                {expand: true, src: ["index.html"], dest: "../dist/", filter: "isFile"},
                {expand: true, src: ["styles/*.css"], dest: "../dist/", filter: "isFile"}]
        }
    },
    uglify: {
        options: {
            banner: "/*! <%= pkg.name %> <%= grunt.template.today('"'"'yyyy-mm-dd'"'"') %> */"
        },
        build: {
            files: [{
                expand: true,
                cwd: "../dist",
                src: "bundle.js",
                dest: "../dist",
                ext: ".min.js"
            }]
        }
    }
})
// Load the plugin that provides the "uglify" task.
grunt.loadNpmTasks("grunt-contrib-uglify-es")
grunt.loadNpmTasks("grunt-sass")
grunt.loadNpmTasks("grunt-contrib-watch")
grunt.loadNpmTasks("grunt-eslint")
grunt.loadNpmTasks("grunt-browserify");
grunt.loadNpmTasks("grunt-contrib-copy");
// Default task(s).
grunt.registerTask("default", ["eslint", "sass", "browserify", "uglify", "copy", "watch"])
}' >> ./src/Gruntfile.js

cd ./src/

npm init
npm install

cd ..

code .
}

```

To call from bash (once you're in the desired parent directory):

```htmlinit```
