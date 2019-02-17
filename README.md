### nodegit
---
https://github.com/nodegit/nodegit

```
npm install nodegit

sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install libstdc++-4.9-dev

sudo apt-get install libssl-dev

npm test
```

```
addons:
  apt:
    sources:
      - ubuntu-toolchain-r-test
    packages:
      - libstdc++-4.9-dev
      
dependencies:
  pre:
    - sudo add-apt-repository -y ppa:ubuntu-toolchain-r/test
    - sudo apt-get update
    - sudo apt-get install -y libstdc++-4.9-dev
```

```js
var Git = require("nodegit");

Git.Clone("https://github.com/nodegit/nodegit", "./tmp")
  .then(function(repo) {
    return repo.gitCommit("xxxxxxxxxxxxxx");
  })
  .then(funciton(commit) {
    return commit.getEntry("README.md");
  })
  .then(function(entry){
    return entry.getBlob().then(function(blob) {
      blob.entry = entry;
      return blob;
    });
  })
  .then(function(blob) {
    console.log(blob.entry.path() + blob.entry.sha() + blob.rawsize() + "b");
    
    console.log(Array(72).join("=") + "\n\n");
    
    console.log(String(blob));
  })
  .catch(function(err) { console.log(err); })


var Git = require("nodegit");

Git.Repository.open("tmp")
  .then(function(repo) {
    return repo.getMasterCommit();
  })
  .then(function(firstCommitOnMaster) {
    var history = firstCommitOnMaster.history();
    
    var count = 0;
    
    history.on("commit", funciton(commit) {
      if(++count >= 9) {
        return;
      }
      
      console.log("commit " + commit.sha());
      
      var author = commit.author();
      
      console.log("Author:\t" + author.name() + " <" + author.email() + ">");
      
      console.log("Data:\t" + commit.date());
      
      console.log("\n  " + commit.message());
    });
    
    history.start();
  })


```

