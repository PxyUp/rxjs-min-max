# rxjs-min-max

RxJs Operators for emits minimum/maximum value on each iteration.

[![NPM](https://nodei.co/npm/rxjs-min-max.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/rxjs-min-max/)

# Example

  ```javascript
    import { maxStream, minStream } from 'rxjs-min-max'

    of(1, 1, 2, 2, 2, 0, 1, 2, 3, -1, 4).pipe(
      maxStream(),
    )
    .subscribe(x => console.log(x)); // 1, 1, 2, 2, 2, 2, 2, 2, 3, 3, 4

    of<Person>(
      { age: 4, name: 'Foo'},
      { age: 7, name: 'Bar'},
      { age: 3, name: 'Foo'},
      { age: 6, name: 'Foo'},
    ).pipe(
      minStream((p: Person, q: Person) => p.age > q.age),
    )
    .subscribe(x => console.log(x));
 
    // displays:
    // { age: 4, name: 'Foo' }
    // { age: 4, name: 'Foo' }
    // { age: 3, name: 'Foo'},
    // { age: 3, name: 'Foo'},
 
  ```

  # Performance

  You can used this operators like min/max:
  
```javascript 
  .pipe(
    minStream(), // maxStream
    last()
) => min() / max()
```

If size of stream big(n > 10000), minStream(maxStream) work faster than min(max):

```javascript
var rxjsMinMax = require('./dist/index.js');
var rxjs = require('rxjs');
var last = require('rxjs/operators').last;
var min = require('rxjs/operators').min;

const n = 100000;
const array = Array.from({ length: n }, () => Math.floor(Math.random() * n));

console.time('minStream');
rxjs.from(array).pipe(rxjsMinMax.minStream(), last()).subscribe((x) => console.log(x));
console.timeEnd('minStream');

console.time('min');
rxjs.from(array).pipe(min()).subscribe((x) => console.log(x));
console.timeEnd('min');

// 0
// minStream: 11.276ms
// 0
// min: 40.306ms
```