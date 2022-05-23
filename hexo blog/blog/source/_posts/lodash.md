---
title: lodash
date: 2020-2-25 18:43:29
tags: [Front end article]
index_img: /img/post/18.jpeg
---

[lodash.js](https://www.yuque.com/attachments/yuque/0/2021/js/758572/1615534059042-a028f54b-7519-479e-9105-24a6a16aa014.js?_lake_card=%7B%22uid%22%3A%221615534027081-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fjs%2F758572%2F1615534059042-a028f54b-7519-479e-9105-24a6a16aa014.js%22%2C%22name%22%3A%22lodash.js%22%2C%22size%22%3A540512%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22UwTOe%22%2C%22card%22%3A%22file%22%7D)

[https://www.lodashjs.com/](https://www.lodashjs.com/)

# Array 方法 数组

## chunk()

把数组拆分成一个二维数组，拆分后的第 1 个数组的长度为第二个参数的值

```js
console.log(_.chunk(["a", "b", "c", "d"], 2)); //[["a", "b"],["c", "d"]]
```

## compact()

过滤掉原数组里的非真（转布尔值后为 false）数据

```js
console.log(_.compact([0, 1, false, 2, "", 3, null, NaN, undefined])); //[1, 2, 3]
```

## concat()

合并数组，与 Array 对象的方法一样

```js
var array = [1];
var other = _.concat(array, 2, [3], [[4]]);

console.log(other);
// => [1, 2, 3, [4]]

console.log(array);
// => [1]
```

## difference()

在第一个数组中把第二个数组里的数据都排除掉

```js
console.log(_.difference([1, 3, 5, 7, 9], [3, 7])); // [1, 5, 9]
```

## differenceBy

与上面的方法一样，只不过它可以再接收一个迭代器的函数做为参数

```js
console.log(_.differenceBy([3.1, 2.2, 1.3], [4.4, 2.5], Math.floor)); //[3.1, 1.3]
```

## differenceWith()

与上面的方法一样，只不过它可以接收一个比较器的函数做为参数，对每个数据都要比较一下

```javascript
var objects = [
  { x: 1, y: 2 },
  { x: 2, y: 1 },
];
console.log(_.differenceWith(objects, [{ x: 1, y: 2 }], _.isEqual)); //[{ 'x': 2, 'y': 1 }]
```

## drop()

切掉数组的前 n（第二个参数，默认为 1）位

```js
console.log(_.drop(["a", "b", "c", "d", "e"], 2)); //['c', 'd', 'e']
```

## dropRight()

切割数组，切掉数组的后 n 位

```js
_.dropRight([1, 2, 3]);
// => [1, 2]

_.dropRight([1, 2, 3], 2);
// => [1]

_.dropRight([1, 2, 3], 5);
// => []

_.dropRight([1, 2, 3], 0);
// => [1, 2, 3]
```

## dropWhile()

去掉数组中，从起点到第二个方法返回假的数据。与 Array 对象身上的 filter()方法一样

```js
var users = [
  { user: "barney", active: false },
  { user: "fred", active: false },
  { user: "pebbles", active: true },
];

_.dropWhile(users, function (o) {
  return !o.active;
});
// => objects for ['pebbles']

// The `_.matches` iteratee shorthand.
_.dropWhile(users, { user: "barney", active: false });
// => objects for ['fred', 'pebbles']

// The `_.matchesProperty` iteratee shorthand.
_.dropWhile(users, ["active", false]);
// => objects for ['pebbles']

// The `_.property` iteratee shorthand.
_.dropWhile(users, "active");
// => objects for ['barney', 'fred', 'pebbles']
```

## dropRightWhile()

与上面一样，不过它是从右边开始查，查到返回假的那个数据都去除

```js
var users = [
  { user: "barney", active: true },
  { user: "fred", active: false },
  { user: "pebbles", active: false },
];

_.dropRightWhile(users, function (o) {
  return !o.active;
});
// => objects for ['barney']

// The `_.matches` iteratee shorthand.
_.dropRightWhile(users, { user: "pebbles", active: false });
// => objects for ['barney', 'fred']

// The `_.matchesProperty` iteratee shorthand.
_.dropRightWhile(users, ["active", false]);
// => objects for ['barney']

// The `_.property` iteratee shorthand.
_.dropRightWhile(users, "active");
// => objects for ['barney', 'fred', 'pebbles']
```

## take()

提取数组的前 n（第二个参数，默认为 1）位。与 drop 方法相反

```js
_.take([1, 2, 3]);
// => [1]

_.take([1, 2, 3], 2);
// => [1, 2]

_.take([1, 2, 3], 5);
// => [1, 2, 3]

_.take([1, 2, 3], 0);
// => []
```

## takeRight()/takeWhile()/takeRightWhile()

与上面的一样

```js
_.takeRight([1, 2, 3]);
// => [3]

_.takeRight([1, 2, 3], 2);
// => [2, 3]

_.takeRight([1, 2, 3], 5);
// => [1, 2, 3]

_.takeRight([1, 2, 3], 0);
// => []
```

```js
var users = [
  { user: "barney", active: false },
  { user: "fred", active: false },
  { user: "pebbles", active: true },
];

_.takeWhile(users, function (o) {
  return !o.active;
});
// => objects for ['barney', 'fred']

// The `_.matches` iteratee shorthand.
_.takeWhile(users, { user: "barney", active: false });
// => objects for ['barney']

// The `_.matchesProperty` iteratee shorthand.
_.takeWhile(users, ["active", false]);
// => objects for ['barney', 'fred']

// The `_.property` iteratee shorthand.
_.takeWhile(users, "active");
// => []
```

```js
var users = [
  { user: "barney", active: true },
  { user: "fred", active: false },
  { user: "pebbles", active: false },
];

_.takeRightWhile(users, function (o) {
  return !o.active;
});
// => objects for ['fred', 'pebbles']

// The `_.matches` iteratee shorthand.
_.takeRightWhile(users, { user: "pebbles", active: false });
// => objects for ['pebbles']

// The `_.matchesProperty` iteratee shorthand.
_.takeRightWhile(users, ["active", false]);
// => objects for ['fred', 'pebbles']

// The `_.property` iteratee shorthand.
_.takeRightWhile(users, "active");
// => []
```

## fill()

填充数组，与 Array 对象身上的 fill()方法一样

```js
var array = [1, 2, 3];

_.fill(array, "a");
console.log(array);
// => ['a', 'a', 'a']

_.fill(Array(3), 2);
// => [2, 2, 2]

_.fill([4, 6, 8, 10], "*", 1, 3);
// => [4, '*', '*', 10]
```

## findIndex()

查找到第一个满足条件的数据的索引值（从左往右查），没找到返回-1。与 Array 对象身上的 findIndex()方法一样

```js
var users = [
  { user: "barney", active: false },
  { user: "fred", active: false },
  { user: "pebbles", active: true },
];

_.findIndex(users, function (o) {
  return o.user == "barney";
});
// => 0

// The `_.matches` iteratee shorthand.
_.findIndex(users, { user: "fred", active: false });
// => 1

// The `_.matchesProperty` iteratee shorthand.
_.findIndex(users, ["active", false]);
// => 0

// The `_.property` iteratee shorthand.
_.findIndex(users, "active");
// => 2
```

## findLastIndex()

这与上面的 findIndex 是一样的，区别是它是从右往左的查

```js
var users = [
  { user: "barney", active: true },
  { user: "fred", active: false },
  { user: "pebbles", active: false },
];

_.findLastIndex(users, function (o) {
  return o.user == "pebbles";
});
// => 2

// The `_.matches` iteratee shorthand.
_.findLastIndex(users, { user: "barney", active: true });
// => 0

// The `_.matchesProperty` iteratee shorthand.
_.findLastIndex(users, ["active", false]);
// => 2

// The `_.property` iteratee shorthand.
_.findLastIndex(users, "active");
// => 0
```

## flatten()

减少一级数组嵌套深度，与 Array 的 flat()这个方法相似

```js
_.flatten([1, [2, [3, [4]], 5]]);
// => [1, 2, [3, [4]], 5]
```

## flattenDeep()

把数组递归为一维数组。相当于[].flat(Infinity)

```js
console.log(_.flattenDeep(["a", ["b", ["c", ["d"]]]])); //["a", "b", "c", "d"]
```

## flattenDepth()

减少 n（第二个参数）层数组的嵌套。相当于[].flat(2)

```js
var array = [1, [2, [3, [4]], 5]];

_.flattenDepth(array, 1);
// => [1, 2, [3, [4]], 5]

_.flattenDepth(array, 2);
// => [1, 2, 3, [4], 5]
```

## fromPairs()

把数组转换为一个对象，与 Object.fromEntries()方法一样

## head()/first()

获取数组里第一个元素，就是取下标为 0 的那个数据

```js
_.head([1, 2, 3]);
// => 1

_.head([]);
// => undefined
```

## last()

取数组里的最后一位数据，取下标为 length-1 的那个数据

```js
_.last([1, 2, 3]);
// => 3
```

## indexOf()

查找数据，并返回数据对应的索引值，与 Array 对象身上的 indexOf()方法一样

```js
_.indexOf([1, 2, 1, 2], 2);
// => 1

// Search from the `fromIndex`.
_.indexOf([1, 2, 1, 2], 2, 2);
// => 3
```

## lastIndexOf()

查找数据，并返回数据对应的索引值，与 Array 对象身上的 lastIndexOf()方法一样

```js
_.lastIndexOf([1, 2, 1, 2], 2);
// => 3

// Search from the `fromIndex`.
_.lastIndexOf([1, 2, 1, 2], 2, 2);
// => 1
```

## initial()

获取数组里除了最后一位的所有数据。相当于删除数组里的最后一个数据，与 Array 对象身上的 pop()方法一样。区别在于 pop 方法会改变原数组，而这个方法不会改变原数组

```js
_.initial([1, 2, 3]);
// => [1, 2]
```

## tail()

获取除了 array 数组第一个元素以外的全部元素，想当于 Array 对象身上的 shift()，与 initial()相反

```js
_.tail([1, 2, 3]);
// => [2, 3]
```

## intersection()

取数组的交集

```js
console.log(_.intersection(["a", "b"], ["b", "c"], ["e", "b"])); //['b']
```

```js
_.intersection([2, 1], [4, 2], [1, 2]);
// => [2]
```

## union()

取数组的并集（合并起来，去掉重复的）

```js
console.log(_.union([2], [1, 2])); //[2, 1]
```

```js
_.union([2], [1, 2]);
// => [2, 1]
```

## xor()

删除数组的交集，留下非交集的部分

```js
console.log(_.xor(["a", "b"], ["b", "c"], ["e", "b"])); //["a", "c", "e"]
```

```js
_.xor([2, 1], [2, 3]);
// => [1, 3]
```

## join()

把数组转成字符串，这个方法原生的 Array 对象也有

```js
_.join(["a", "b", "c"], "~");
// => 'a~b~c'
```

## nth()

取数组里的某个数据，就是通过下标取到某个数据。只不过它的数字可以为负。表示倒着找

```js
var array = ["a", "b", "c", "d"];
console.log(
  _.nth(array, 1), //b
  _.nth(array, -3) //c
);
```

```js
var array = ["a", "b", "c", "d"];

_.nth(array, 1);
// => 'b'

_.nth(array, -2);
// => 'c';
```

# 以下这 4 个方法

## pull()

根据给的参数（参数为数据）删除原数组里的对应数据

```js
var array = [1, 2, 3, 1, 2, 3];

_.pull(array, 2, 3);
console.log(array);
// => [1, 1]
```

## pullAll()

与上面的方法一样，就是参数为数组（好比 call,apply 这两个方法

```js
var array = [1, 2, 3, 1, 2, 3];

_.pullAll(array, [2, 3]);
console.log(array);
// => [1, 1]
```

## pullAllBy()\pullAllWith()

与前面方面的语法一样

```js
var array = [{ x: 1 }, { x: 2 }, { x: 3 }, { x: 1 }];

_.pullAllBy(array, [{ x: 1 }, { x: 3 }], "x");
console.log(array);
// => [{ 'x': 2 }]
```

```js
var array = [
  { x: 1, y: 2 },
  { x: 3, y: 4 },
  { x: 5, y: 6 },
];

_.pullAllWith(array, [{ x: 3, y: 4 }], _.isEqual);
console.log(array);
// => [{ 'x': 1, 'y': 2 }, { 'x': 5, 'y': 6 }]
```

## pullAt()

根据给的参数（参数为索引）删除原数组里的对应数据

```js
var array = [5, 10, 15, 20];
var evens = _.pullAt(array, 1, 3);

console.log(array);
// => [5, 15]

console.log(evens);
// => [10, 20]
```

# 用后面 remove 的方法代替

## remove()

根据函数删除原数组里的数据

```js
var arr = ["a", "b", "c", "d", "e"];
_.remove(arr, function (value, index, array) {
  return index > 2;
});
console.log(arr); //["a", "b", "c"]
```

```js
var array = [1, 2, 3, 4];
var evens = _.remove(array, function (n) {
  return n % 2 == 0;
});

console.log(array);
// => [1, 3]

console.log(evens);
// => [2, 4]
```

## without()

根据给的参数（参数为数据）删除原数组里的对应数据

```js
_.without([2, 1, 2, 3], 1, 2);
// => [3]
```

## reverse()

颠倒数组，这个方法原生的 Array 对象也有

```js
var array = [1, 2, 3];

_.reverse(array);
// => [3, 2, 1]

console.log(array);
// => [3, 2, 1]
```

## slice()

截取数组，这个方法原生的 Array 对象也有

## uniq()数组去重

```js
console.log(_.uniq([1, 2, 2, 1])); //[1, 2]
//uniqBy()/uniqWith() 与前面的一样
```

```js
_.uniq([2, 1, 2]);
// => [2, 1]
```

## zip()

把各数组中索引值相同的数据放到一起，组成新数组

```javascript
console.log(_.zip(["小明", "小红", "小刚"], ["男", "女", "男"], [12, 13, 14])); //[["小明", "男", 12],["小红", "女", 13],["小刚", "男", 14]]
```

```javascript
_.zip(["fred", "barney"], [30, 40], [true, false]);
// => [['fred', 30, true], ['barney', 40, false]]
```

## zipObject()

与上面方法一样，区别是它输出的是对象

```js
_.zipObject(["a", "b"], [1, 2]);
// => { 'a': 1, 'b': 2 }
```

```javascript
console.log(
  _.zipObject(["小明", "小红", "小刚"], ["男", "女", "男"], [12, 13, 14])
); //{小明: "男", 小红: "女", 小刚: "男"}
```

## zipWith()

```js
_.zipWith([1, 2], [10, 20], [100, 200], function (a, b, c) {
  return a + b + c;
});
// => [111, 222]
```

## unzip()

这个方法与 zip 相反，把每个数组里索引值一样的数据放在一起

```js
var zipped = _.zip(["fred", "barney"], [30, 40], [true, false]);
// => [['fred', 30, true], ['barney', 40, false]]

_.unzip(zipped);
// => [['fred', 'barney'], [30, 40], [true, false]]
```

```javascript
console.log(
  _.unzip([
    ["小明", "男", 12],
    ["小红", "女", 13],
    ["小刚", "男", 14],
  ])
); //[['小明', '小红', '小刚'], ['男', '女', '男'], [12, 13, 14]]
```

## unzipWith()

与 zipWidth()一样，接收了一个迭代器的函数

```js
var zipped = _.zip([1, 2], [10, 20], [100, 200]);
// => [[1, 10, 100], [2, 20, 200]]

_.unzipWith(zipped, _.add);
// => [3, 30, 300]
```

# Collection   集合

## countBy()

按照一定规则统计数量，key 循环次数，value 为匹配到的数量

```javascript
console.log(_.countBy(["one", "two", "three"], "length")); //{3: 2, 5: 1}	按每个字符串的length进行统计，length为3的有两个数据。length为5的有1个数据
```

## groupBy()

按照一定规则进行分组，key 为循环次数，value 为匹配到的数组

```javascript
console.log(_.groupBy(["one", "two", "three"], "length")); //{3: ["one", "two"], 5: ["three"]}
```

```js
_.groupBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': [4.2], '6': [6.1, 6.3] }

// The `_.property` iteratee shorthand.
_.groupBy(["one", "two", "three"], "length");
// => { '3': ['one', 'two'], '5': ['three'] }
```

## each()/forEach()

循环，与原生 Array.forEach 一样

```js
_([1, 2]).forEach(function (value) {
  console.log(value);
});
// => Logs `1` then `2`.

_.forEach({ a: 1, b: 2 }, function (value, key) {
  console.log(key);
});
// => Logs 'a' then 'b' (iteration order is not guaranteed).
```

## eachRight()/forEachRight()

倒着循环

```js
_.forEachRight([1, 2], function (value) {
  console.log(value);
});
// => Logs `2` then `1`.
```

## every()

与原生 Array.every 方法一样

```js
_.every([true, 1, null, "yes"], Boolean);
// => false

var users = [
  { user: "barney", age: 36, active: false },
  { user: "fred", age: 40, active: false },
];

// The `_.matches` iteratee shorthand.
_.every(users, { user: "barney", active: false });
// => false

// The `_.matchesProperty` iteratee shorthand.
_.every(users, ["active", false]);
// => true

// The `_.property` iteratee shorthand.
_.every(users, "active");
// => false
```

## filter()

过滤数组，与 Array 对象上的 filter()方法一样

```js
var users = [
  { user: "barney", age: 36, active: true },
  { user: "fred", age: 40, active: false },
];

_.filter(users, function (o) {
  return !o.active;
});
// => objects for ['fred']

// The `_.matches` iteratee shorthand.
_.filter(users, { age: 36, active: true });
// => objects for ['barney']

// The `_.matchesProperty` iteratee shorthand.
_.filter(users, ["active", false]);
// => objects for ['fred']

// The `_.property` iteratee shorthand.
_.filter(users, "active");
// => objects for ['barney']
```

## find()

查找据，与 Array 对象上的 find()方法一样

```js
var users = [
  { user: "barney", age: 36, active: true },
  { user: "fred", age: 40, active: false },
  { user: "pebbles", age: 1, active: true },
];

_.find(users, function (o) {
  return o.age < 40;
});
// => object for 'barney'

// The `_.matches` iteratee shorthand.
_.find(users, { age: 1, active: true });
// => object for 'pebbles'

// The `_.matchesProperty` iteratee shorthand.
_.find(users, ["active", false]);
// => object for 'fred'

// The `_.property` iteratee shorthand.
_.find(users, "active");
// => object for 'barney'
```

## findLast()

与上面一样，区别在于它是从右往左查

```js
_.findLast([1, 2, 3, 4], function (n) {
  return n % 2 == 1;
});
// => 3
```

## flatMap()

生成一个扁平化的数组，与原生的 flatMap()方法一样

```js
function duplicate(n) {
  return [n, n];
}

_.flatMap([1, 2], duplicate);
// => [1, 1, 2, 2]
```

## flatMapDeep()

与上面一样，不过它可以递归

```js
function duplicate(n) {
  return [[[n, n]]];
}

_.flatMapDeep([1, 2], duplicate);
// => [1, 1, 2, 2]
```

## flatMapDepth()

与上面一样，它可以递归，并且可以指定递归的深度

```js
function duplicate(n) {
  return [[[n, n]]];
}

_.flatMapDepth([1, 2], duplicate, 2);
// => [[1, 1], [2, 2]]
```

## includes()

与 Array 对象上的 includes()方法一样

```js
_.includes([1, 2, 3], 1);
// => true

_.includes([1, 2, 3], 1, 2);
// => false

_.includes({ user: "fred", age: 40 }, "fred");
// => true

_.includes("pebbles", "eb");
// => true
```

## invokeMap()

使用第二个参数（方法）去处理数组，返回处理后的结果（数组）

```javascript
console.log(
  _.invokeMap(
    [
      [5, 1, 7],
      [3, 2, 1],
    ],
    "sort"
  ), //[ [1, 5, 7],[1, 2, 3]]
  _.invokeMap([123, 456], String.prototype.split, "") //[["1", "2", "3"],["4", "5", "6"]]
);
```

## keyBy()

创建一个对象，里面的 key 由第二个参数决定。value 为原数组里对应的那条数据

```javascript
var array = [
  { dir: "left", code: 97 },
  { dir: "right", code: 100 },
];
var result = _.keyBy(array, function (o) {
  return String.fromCharCode(o.code); //key为使用fromCharCode解析后的字符。value为它所在数组里的那条数据
});
console.log(result);

//key为dir，value为key所在原数组里的那条数据
console.log(_.keyBy(array, "dir"));
```

## orderBy()

排序，既能升序又能降序

```js
var users = [
  { user: "fred", age: 48 },
  { user: "barney", age: 34 },
  { user: "fred", age: 40 },
  { user: "barney", age: 36 },
];
console.log(
  _.orderBy(users, "age", "asc"), //以age属性的值进行升序排序
  _.orderBy(users, "user", "desc") //以user属性的值进行降序排序
);
```

## sortBy()

排序，只能升序

```js
console.log(
  _.sortBy(users, function (o) {
    return o.user;
  })
);
```

## partition()

根据第 2 个参数把一个数组分拆成一个二维数组

```javascript
var users = [
  { user: "barney", age: 36, active: false },
  { user: "fred", age: 40, active: true },
  { user: "pebbles", age: 1, active: false },
];
console.log(
  _.partition(users, function (o) {
    //active为true的放在一起，active为false的放在一起
    return o.active;
  }),
  _.partition(users, { age: 1, active: false }) //把第二个参数对应的数据放一起，其余的放一起
);
```

## reduce()

与 Array 对象上的 reduce()方法一样

## reduceRight()

与 Array 对象上的 reduceRight()方法一样

## reject()

```js
var users = [
  { user: "barney", age: 36, active: false },
  { user: "fred", age: 40, active: true },
];
console.log(
  _.reject(users, function (o) {
    return o.active; //barney
  }),
  _.reject(users, { age: 36, active: false }), //fred
  _.reject(users, ["user", "fred"]), //barney
  _.reject(users, "age") //[]
);
```

## sample()

从数组中随机取一个数据

```js
console.log(_.sample(["a", "b", "c", "d", "e"]));
```

## sampleSize()

获得 n 个随机数据

```js
console.log(_.sampleSize(["a", "b", "c", "d", "e"], 3));
```

## shuffle()

随机排序

```js
console.log(_.shuffle(["a", "b", "c", "d", "e"]));
```

## size()

返回集合长度

```js
console.log(
  _.size(["a", "b", "c", "d", "e"]), //5
  _.size({ a: 1, b: 2 }), //2
  _.size("kaivon") //6
);
```

## some()

与 Array 对象上的 some()方法一样

# Function 方法

## defer()

推迟调用函数，在第二次事件循环的时候调用

```js
_.defer(function (text) {
  console.log(text);
}, "第二次事件循环");
console.log("第一次事件循环");
```

## delay()

```js
_.delay(
  function (text) {
    console.log(text);
  },
  1000,
  "延迟一秒执行"
);
```

## flip()

调用函数时翻转参数

```js
function fn1() {
  console.log(arguments);
}
fn1 = _.flip(fn1);
fn1(1, 2, 3);
```

## negate()

结果取反函数

```js
function fn2(n) {
  return n % 2 == 0;
}
console.log(_.filter([1, 2, 3, 4, 5, 6], _.negate(fn2))); //[1, 3, 5]
```

## once()

函数只能调用一次

```js
function fn3() {
  console.log("fn3");
}
var newFn3 = _.once(fn3);
newFn3();
newFn3(); //不起作用
```

# Lang 方法

## castArray()

强制转为数组，其实就是在外面加一层方括号

```js
console.log(
  _.castArray("a"), //["a"]
  _.castArray({ a: 1, b: 2 }) //[{a: 1, b: 2}]
);
```

## clone()

浅拷贝

```js
var obj1 = {
  a: 1,
  b: {
    c: 2,
  },
};
var obj2 = _.clone(obj1);
console.log(obj1, obj2);
改一个数据;
(obj2.b.c = 3), console.log(obj1, obj2);
一改都变;
```

## cloneDeep()

深拷贝

```js
var obj3 = _.cloneDeep(obj1);
(obj3.b.c = 4), console.log(obj1, obj3);
```

## conformsTo()

通过第二个参数来检测对象的属性值是否满足条件

```js
var object = { a: 1, b: 2 };
console.log(
  _.conformsTo(object, {
    b: function (n) {
      return n > 1;
    },
  }), //true
  _.conformsTo(object, {
    b: function (n) {
      return n > 2;
    },
  }) //false
);
```

## ea()

比较两个值是否相等。与 Object.is()这个方法一样

```js
console.log(
  _.eq(12, 12), //true
  _.eq({ a: 1 }, { a: 1 }), //false
  _.eq(NaN, NaN) //true
);
```

## gt()

第一个值是否大于第二个值

```js
console.log(
  _.gt(3, 1), //true
  _.gt(3, 3) //false
);
```

## gte()

第一个值是否大于等于第二个值

## lt()

小于

## lte()

小于等于

## isArray()

```js
console.log(
  _.isArray([1, 2, 3]), //true
  _.isArray(document.body.children), //false
  _.isObject({}), //true
  _.isObject(null) //false
);
```

## toArray()

```js
console.log(
  _.toArray({ a: 1, b: 2 }), //[1, 2]
  _.toArray("abc"), //["a", "b", "c"]
  _.toArray(null) //[]
);
```

# Object 方法

## assign()

合并对象，与 Object.assign()方法一样

## assignIn()/extend()

与上面一样，不过它能继承原型身上的属性

## assignInWith()/extendWith()

与上面一样，接收一个比较器的函数做为参数

## assignWith()

也是接收一个比较器的函数做为参数

## at()

根据传入的属性创建一个数组

```javascript
var object = { a: [{ b: { c: 3 } }, 4] };
console.log(_.at(object, ["a[0].b.c", "a[1]"])); //[3, 4]
```

## create()

与 Object.create()一样

## defaults()

合并对象，与 assign()一样，不过 assign 方法合并时遇到相同的属性，后面的会覆盖前面的。defaults 刚好相反，前面的覆盖后面的

```js
console.log(
  _.defaults({ a: 1 }, { b: 2 }, { a: 3 }), //{a: 1, b: 2}
  _.assign({ a: 1 }, { b: 2 }, { a: 3 }) //{a: 3, b: 2}
);
```

## defaultsDeep()

与 defaults 一致，不过它会深递归

## toPairs()/entries()

把对象里可枚举的属性(不包括继承的)创建成一个数组，与 Object.entities()的方法一样

## toPairsIn()/entriesIn()

与上面的一样，但它包括继承的属性

## findKey()

与前面讲的 find 方法一样，只不过它返回的是 key

```js
var users = {
  barney: { age: 36, active: true },
  fred: { age: 40, active: false },
  pebbles: { age: 1, active: true },
};
console.log(_.findKey(users, { age: 1, active: true })); //pebbles
```

## findLastKey()

与上面一样，只不过它从反方向开始遍历

## forIn()

与原生 的 for...in 循环一样，只不过它是一个函数，语法与**forEach**一样。它遍历的是自己的属性与继承的属性

## forInRight()

与上面一样，只不过是反方向遍历

## forOwn()

与 forIn()一样，只不过 forOwn 只能遍历到自己的属性

## forOwnRight()

与上面一样，只不过是反方向遍历

## functions()/functionsIn()

## get()

获取属性的值，与 Object.defineProperty()   属性描述对象上的 get 方法一致

## set()

设置属性的值，与 Object.defineProperty()   属性描述对象上的 set 方法一致

## setWith()

与上面的一样，只不过可以给一个参数决定返回的是对象还是数组

```js
console.log(_.setWith({}, "[0][1]", "a", Array));
```

## has()

检查属性是否为对象的直接属性，与 Object.hasOwnProperty()方法返回 true 一样

## hasIn()

检查属性是对象的直接属性还是继承属性，也与 Object.hasOwnProperty()一样，true 表示直接属性，false 表示继承属性

## invert()

把对象的 key 与 value 颠倒，后面的属性会覆盖前面的属性

```js
var object = { a: 1, b: 2, c: 1 };
console.log(_.invert(object)); //{1: "c", 2: "b"}
```

## invertBy()

与上面一样，它遇到相同的值后不会覆盖，而是会把所有放在一个数组里。另外它多了一个遍历器方法

## invoke()

调用方法去处理取到的属性值

```js
var object = { a: [{ b: { c: [1, 2, 3, 4] } }] };
console.log(_.invoke(object, "a[0].b.c.slice", 1, 3)); //[2, 3]	用slice方法去截取a[0].b.c的1-3位
```

## keys()

把对象的 key 放到一个数组里，与 Object.keys()的方法一样

## keysIn()

与上面一样，只不过它包含继承到的属性

## values()

把对象的 value 放到一个数组里，与 Object.value()的方法一样

## valuesIn()

与上面一样，只不过它包含继承到的属性

## mapKeys()

修改对象的 key，value 不会变

```js
var result = _.mapKeys({ a: 1, b: 2 }, function (value, key) {
  return key + value;
});
console.log(result); //{a1: 1, b2: 2}
```

## mapValues()

与上个方法一样，只不过它修改的是 value，key 不会变

## merge()

它与 assign 一样，不过它遇到相同的属性名后并不会覆盖，它会合并

```js
var object = {
  a: [{ b: 2 }, { d: 4 }],
};
var other = {
  a: [{ c: 3 }, { e: 5 }],
};
console.log(_.merge(object, other));
```

## mergeWith()

与上面的方法一致，不过多了接收一个比较器的函数做为参数

## omit()

删除对象里的属性

```js
console.log(_.omit({ a: 1, b: "2", c: 3 }, ["a", "c"])); //{b: "2"}
```

## \_.omitBy

与上面一样，不过是接收一个迭代器的函数做为参数

## pick()

筛选对象里的属性

```js
console.log(_.pick({ a: 1, b: "2", c: 3 }, ["a", "c"])); //{a: 1, c: 3}
```

## pickBy()

与上面一样，不过是可接收一个迭代器的函数做为参数

## result()

获取对象属性，它与 get 一样。只不过它遇到函数的属性时会调用函数，并且把 this 指向对象本身

```js
var obj = {
  a: 12,
  b: function () {
    console.log(this.a);
  },
};
console.log(_.result(obj, "a")); //12
_.result(obj, "b"); //12
console.log(_.get(obj, "b")); //它只能取到这个函数，并不能执行
```

## toPairs()、toPairsIn()

把对象的 key 与 value 一起放到数组里

```js
function Foo() {
  this.a = 1;
  this.b = 2;
}
Foo.prototype.c = 3;
console.log(_.toPairs(new Foo()));
console.log(_.toPairsIn(new Foo()));
```

## unset()

删除属性

```js
var object = { a: [{ b: { c: 7 } }] };
_.unset(object, "a[0].b.c"), console.log(object);
```

## update()

这个与 set 一样，不过它可以接收一个函数的参数

```js
var object = { a: 10 };
_.update(object, "a", function (n) {
  return n * n;
});
console.log(object); ///{a: 100}
```

## updateWith()

与上面的一样，不过可以接收一个路径的参数，决定生成的属性放在哪里

```js
var object = {};
_.updateWith(
  object,
  "[a][b]",
  function () {
    return 12;
  },
  Object
);
console.log(object);
```

# String 方法

## camelCase()

转换字符串为驼峰格式

```js
console.log(_.camelCase("kaivon_chen"), _.camelCase("kaivon chen"));
```

## capitalize()

首字母为大写

```js
console.log(_.capitalize("kaivon")); //Kaivon
```

## endsWith()

查检结尾的字符

```js
console.log(_.endsWith("abc", "c")); //true
```

## escape()

把特殊字符转义成真正的 HTML 实体字符

```js
console.log(_.escape("ka<iv>on")); //ka&lt;iv&gt;on
```

## unescape()

与上面相反

```js
console.log(_.unescape("ka&lt;iv&gt;on")); //ka<iv>on
```

## kebabCase()

转换字符为加-的形式

```js
console.log(_.kebabCase("k a i")); //k-a-i
```

## lowerCase()/toLower()

转小写

## upperCase()/toUpper()

转大写

## lowerFirst()

首字符转小写

## upperFirst()

首字符转大写

## pad()

填充字符串到指定的长度(左右填充)

```js
console.log(_.pad("abc", 8, "-")); //--abc---
```

## padEnd()

```js
console.log(_.padEnd("abc", 8, "-"));
```

## padStart()

```js
console.log(_.padStart("abc", 8, "-"));
```

## parseInt()

把字符串类型的数字转成数字，

## repeat()

重复字符串

```js
console.log(_.repeat("kaivon", 2)); //kaivonkaivon
```

## replace()

替换字符串

```js
console.log(_.replace("kaivon", "von", "***")); //kai***
```

## snakeCase()

转换字符串为\_的形式

```js
console.log(_.snakeCase("k a i")); //k_a_i
```

## split()

分隔字符串为数组，与原生 String.split()一样

## startCase()

转换字符串为+空格的形式，并且首字符大写

```js
console.log(
  _.startCase("kaivon-chen"), //Kaivon Chen
  _.startCase("kaivonChen"), //Kaivon Chen
  _.startCase("kaivon_chen") //Kaivon Chen
);
```

## startsWith()

检查字符串的开始字符

```js
console.log(_.startsWith("kaivon", "k")); //true
```

## template()

编译模板

```js
var compiled = _.template("hello <%= user %>!"); //user为一个占位符
console.log(compiled({ user: "kaivon" })); //拿到数据后，给user赋值，它就能正确解析出内容了
```

## trim()

去除首尾空格，或者指定字符

```js
console.log(_.trim("kaivon-", "-")); //kaivon
```

## trimEnd()

去除后面的空格，或者指定字符

## trimStart()

与上面的一样，只不过去除的是左边的

## truncate()

```js
console.log(
  _.truncate("Hi kaivon! How are you feeling today? I am felling great!")
); //Hi kaivon! How are you feel...
console.log(
  _.truncate("Hi kaivon! How are you feeling today? I am felling great!", {
    //'length': 10,	//限制固定的字符个数
    separator: /!/, //加个正则，遇到第一个空格后就加三个点
  })
);
```

## words()

把字符串的单词拆分成数组

```js
console.log(_.words("kaivon chen")); //["kaivon", "chen"]
```
