---
title: co
date: 2019-12-05 18:24:31
tags: generator
---

# generator function 

read tree files in order:f1.txt, f2.txt, f3.txt 

> fs.readFile: asynchronous return file data

## read.js
```javascript
const fs = require('fs');

function getFileResult(path) {
  return new Promise((resolve, reject) => {
    fs.readFile(path, (err, data) => {
      if(err) {
        reject(err);
      }
      resolve(data);
    })
  })
}

function* read() {
  yield getFileResult('./f1.txt');
  yield getFileResult('./f2.txt');
  yield getFileResult('./f3.txt');
}

// Manual Execution
// const g = read();
// g.next().value.then(res => {
//   console.log('ressss1', res.toString());
// })

// g.next().value.then(res => {
//   console.log('resss2', res.toString());
// })

// Auto Execution
function autoRun(gen) {
  const g = gen();
  let arr = [];
  function run(gObject) {
    const {
      value,
      done
    } = gObject.next();
    console.log('value', value);

    if(done) {
      console.log('arr', arr);
      return arr;
    }
    value.then(res => {
      console.log('res', res);
      arr.push(res.toString());
      run(gObject);
    })
  }
  run(g);
}

autoRun(read);

```

## f1.txt
```
11111111
```

f2.txt and f3.txt as same as f1.txt