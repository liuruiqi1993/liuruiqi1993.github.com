---
layout: post
title: 笔试
date: 2020-04-16
---
```
var readline = require('readline');
const rl = readline.createInterface({
        input: process.stdin,7
        output: process.stdout
});
rl.on('line', function(line){
});

for await (const line of rl) {
    // input.txt 中的每一行在这里将会被连续地用作 `line`。
    console.log(`Line from file: ${line}`);
}
```




















