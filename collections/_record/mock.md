---
layout: post
---

疑问1：这样生成的img，每十页都是一样的 
```
var Random = Mock.Random;
Mock.mock(/\/list/, ({ url, body }) => {
  let { currentPage: page } = qs.parse(url.slice(url.indexOf('?')), { ignoreQueryPrefix: true });
  let total = `total|${page * 10}-20`;
  let data = Mock.mock({
    'list|10': [
      {
        img: Random.dataImage('250x250', Random.color(), Random.color(), Random.word()),
        des: '@string',
        title: '@string'
      }
    ],
    'page|+0': page / 1,
    [total]: page * 10
  });
  return data;
});
```