# date-fp

[![Circle CI](https://circleci.com/gh/cullophid/date-fp.svg?style=svg)](https://circleci.com/gh/cullophid/date-fp)
[![npm version](https://badge.fury.io/js/date-fp.svg)](https://badge.fury.io/js/date-fp)

**date-fp** is a utility library for working with JavaScript dates.

## Motivation
There are several excellent JavaScript libraries for managing dates in JavaScript but they are generally build to be used in the object-oriented programming paradigm. This makes them cumbersome to include in a functional context.

*If you are not familiar with functional programming in javascript read [Professor Frisby's Mostly Adequate Guide to  Functional Programming](https://drboolean.gitbooks.io/mostly-adequate-guide/content/)*

*Also check out [Ramda.js](http://ramdajs.com/0.17/index.html) (A great library for functional programming with javascript)*


## Key concepts
All functions in **date-fp** follows a set of functional programming principles.



### Generic Date objects
**date-fp** operates on normal javascript date objects. There are no wrapper objects, and **date-fp** doest not extend or mutate any native javascript objects. Among other things this means that you can still use normal comparison operators like `<` and `>`.

### Purity
All functions in **date-fp** are pure. For a function to be pure it must follow two basic rules.
1. Pure function always produce the same output given the same input.
2. Pure functions have no side effects. This means that calling the function will not affect the world outside the function.

### Immutability
 Dates in date-fp are newer mutated. All operations that modifies a date returns a copy with the given changes, and leaves the original date object intact and unchanged.

### Currying
All functions in date-fp uses automatic currying. This allows easy partial application of **date-fp** functions. For more information on currying read: [Why Curry Helps](https://web.archive.org/web/20140714014530/http://hughfdjackson.com/javascript/why-curry-helps)

### Composition
functions in **date-fp** take the data(usually a date object) as the last parameter. This, combined with currying, allows for easy function composition.

```js
const tomorrowAsString = compose(D.format('YYY-MM-DD'), D.add('days', 1));

tomorrowAsString(new Date('2015-01-01')); // '2015-01-02';
```


## API

### parse
`String -> Date`

returns a new date from the given datestring
```js
const date = D.parse('2015-01-01') // Thu Jan 01 2015 00:00:00 GMT+0000 (GMT)
```

### clone
`Date -> Date`

returns a copy of the given date
```js
const date = D.clone(new Date('2015-01-01')); // Thu Jan 01 2015 00:00:00 GMT+0000 (GMT)
```

### get
`String -> Date -> Date`

returns the chosen portion of a date

```js
const date = new Date('2015-01-02 11:22:33.444')
D.get('milliseconds', date); // 444
D.get('seconds', date); // 33
D.get('minutes', date); // 22
D.get('hours', date); // 11
D.get('date', date); // 2
D.get('month', date); // 1
D.get('year', date); // 2015

```

### set
`String -> Number -> Date -> Date`

Returns a clone of the supplied date with the specified modification.
Returns an Error if the modification results in an invalid date.

```js
const date = new Date();
D.set('milliseconds', 123, date);
D.set('seconds', 34, date);
D.set('minutes', 22, date);
D.set('hours', 13, date);
D.set('date', 23, date);
D.set('month', 12, date);
D.set('year', 2001, date);
```

### add
`String -> Number -> Date -> Date`

Returns a clone of the supplied date with the specified modification.
Returns an Error if the modification results in an invalid date.

```js
const date = new Date();
D.add('milliseconds', 123, date);
D.add('seconds', 34, date);
D.add('minutes', 22, date);
D.add('hours', 13, date);
D.add('date', 23, date);
D.add('month', 12, date);
D.add('year', 2001, date);

```

### sub
`String -> Number -> Date -> Date`

Returns a clone of the supplied date with the specified modification.
Returns an Error if the modification results in an invalid date.


```js
const date = new Date();
D.sub('milliseconds', 123, date);
D.sub('seconds', 34, date);
D.sub('minutes', 22, date);
D.sub('hours', 13, date);
D.sub('date', 23, date);
D.sub('month', 12, date);
D.sub('year', 2001, date);

```


## diff

`String -> Date -> Date -> Number`

returns the difference between two dates.
Returns an Error if given an invalid date unit.

```js
const date1 = new Date('2014-02-01 11:12:13.123');
D.diff('milliseconds', date1, new Date('2014-02-01 11:12:13.223')); // 100
D.diff('seconds', date1, new Date('2014-02-01 11:12:16.123')); // 3
D.diff('minutes', date1, new Date('2014-02-01 11:8:13.123')); // -4
D.diff('hours', date1, new Date('2014-02-01 22:12:13.123')); // 11
D.diff('days', date1, new Date('2014-02-05 11:12:13.123')); // 4
D.diff('months', date1, new Date('2014-04-01 11:12:13.123')); // 2
D.diff('years', date1, new Date('2015-04-01 11:12:13.123')); // 1
```

## format
`String -> Date -> String`

returns a string representation of a date on the specified format

```js
const date = new Date('2015-01-02 03:04:05.123');
D.format('YYYY-MM-DD HH:mm:ss', date); // '2015-01-02 03:04:05'
D.format('DD/MM/YY', date); // '02/01/15'
D.format('MMMM D YYYY', date); // 'January 2 2015'
```

`format` support the following string tokens:

|part                 | Token |      Output         |
|----------------:|:-----:|---------------------|
|year             | YYYY  | 1971... 2015        |
|                 | YY    | 01... 99            |
|month            | MMMM  | January... December |
|                 | MMM   | Jan... Dec          |
|                 | MM    | 01... 12            |
|                 | M     | 1... 12             |
|date             | DD    | 01... 31            |
|                 | D     | 1... 31             |
|weekday          | dddd  | Sunday... Saturday  |
|                 | ddd   | Sun... Sat          |
|                 | dd    | Su... Sa            |
|                 | dd    | Su... Sa            |
|                 | d     | 0... 6              |
|hours            | HH    | 00... 23            |
|                 | H     | 0... 23             |
|                 | hh    | 01... 12            |
|                 | h     | 1... 12             |
|                 | h     | 1... 12             |
|AM/PM            | A     | AM,PM               |
|                 | a     | am,pm               |
|minutes          | mm    | 01... 59            |
|                 | m     | 1... 59             |
|seconds          | ss    | 01... 59            |
|                 | s     | 1... 59             |
|Fractional Second| SSS   | 001... 999          |
|                 | SS    | 01... 99            |
|                 | S     | 1... 9              |

## Credit
Thanks to John-David Dalton for lodash.curry
Thanks to the team behind Ramda.js and moment.js for inspiration.

## Contributing
yes please!
