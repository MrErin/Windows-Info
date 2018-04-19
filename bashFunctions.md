# Make and Goto (stolen directly from Steve)
Makes a new directory and moves into it from the terminal

* Add to aliases.sh:

```mg() {[ -n "$1" ] && mkdir -p "$@" && cd $_;}```

* To call from bash (once you're in the desired parent directory):

```mg [directory name]```


# Simple HTML (also stolen but tweaked)
Quick-create files for an HTML project. 
* $1 is the name for the css file
* $2 is the name for the js file
* You'll get a non-fatal error if you don't provide names for both files. 

Add to aliases.sh:

```
simplehtml () {
echo '<!doctype html> 
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>My Awesome Page</title>
  <link rel="stylesheet" type="text/css" href="./'$1'">

</head>

<body>
  <nav></nav>

  <header></header>

  <article id="contentHook">
    <header></header>
    <section></section>
    <footer></footer>
  </article>

  <footer></footer>

  <script src="'$2'"></script>
</body>
</html>' >> index.html

  touch $1
  touch $2
}
```

* To call from bash (once you're in the desired parent directory):

```simplehtml [stylesheet name].css [javascript file name].js```


# Create New GitHub Repository
* $1 is link to GitHub repo copied from website

Add to aliases.sh:

```
gnr () {
  echo "# Project:
  
  # Requirements:
  
  # Currently working on:
  
  # Completed features:
  
  # Stretch Goals:" >> README.md
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

#ESLint
node_modules/
package.json
package-lock.json
.eslintrc.js

#other
bower_components
__pycache__
*.pyc
bin
obj
*.db
*.sqlite3
.vscode' >> .gitignore
  git init
  git add .
  git commit -m "setup/first commit"
  git remote add origin $1
  git push -u origin master
}
```

To call from bash (once you're in the desired parent directory):

```gnr [repo link copied from GitHub]```