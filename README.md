# pec [![build status](https://secure.travis-ci.org/thlorenz/pec.svg)](http://travis-ci.org/thlorenz/pec)

Poker equity calculator. Compares two combos or one combo against a range to compute winning equity.

```js
const { raceRange, rates } = require('pec')

const combo = [ 'Jh', 'Js' ]
const range = [
  [ 'Kh', 'Ks' ], [ 'Kh', 'Kd' ], [ 'Kh', 'Kc' ],
  [ 'Ks', 'Kd' ], [ 'Ks', 'Kc' ], [ 'Kd', 'Kc' ],
  [ 'Qh', 'Qs' ], [ 'Qh', 'Qd' ], [ 'Qh', 'Qc' ],
  [ 'Qs', 'Qd' ], [ 'Qs', 'Qc' ], [ 'Qd', 'Qc' ]
]

const { win, loose, tie } = raceRange(combo, range, 1E4)
const { winRate, looseRate, tieRate } = rates({ win, loose, tie })

console.log('JJ performs as follows vs. [ KK, QQ ]')
console.log('win: %d%% (%d times)', winRate, win)
console.log('loose: %d%% (%d times)', looseRate, loose)
console.log('tie: %d%% (%d times)', tieRate, tie)
```

    JJ performs as follows vs. [ KK, QQ ]
    win: 18.13% (21750 times)
    loose: 81.43% (97718 times)
    tie: 0.44% (532 times)

For more examples see the [tests](test/pec.single.js) and the [webworker example](examples/webworker.js).

You can launch the web worker via `npm install && npm run demo`.

## Installation

    npm install pec

## [API](https://thlorenz.github.io/pec)

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### BackgroundWorker.raceRange

**Parameters**

-   `combo` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** to race i.e. `[ 'As', 'Ad' ]`
-   `total` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** the total number of times to race, `100` are processed
    each time and `update` invoked until the `total` is reached
-   `trackCombos` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the counts for each combos are tracked (optional, default `false`)
-   `board` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>?** if supplied the range will be raced
    against subsets boards that include all cards of the given board (optional, default `null`)

Returns **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** the uid generated to identify this background job,
the same uid will be included in the message the result to identify it with the job

### BackgroundWorker.stop

Stops any races in progress.

### createBackgroundWorker

Creates a background worker which uses a web worker
under the hood to process _race_ requests.

**Parameters**

-   `update` **funcion** will be called with updates: `{ win, loose, tie, iterations, uid }`

Returns **BackgroundWorker** backgroundWorker

### raceCodesForBoard

Same as @see raceCombosForBoard, except that the combo cards are given
as their codes obtained via [phe](https://github.com/thlorenz/phe) `cardCodes`.

**Parameters**

-   `combo1`  
-   `combo2`  
-   `times`  
-   `board`  

### raceCodes

Same as @see raceCombos, except that the combo cards are given
as their codes obtained via [phe](https://github.com/thlorenz/phe) `cardCodes`.

**Parameters**

-   `combo1`  
-   `combo2`  
-   `times`  

### raceRangeCodesForBoard

Same as @see raceRangeForBoard, except that the combo, range cards and board are given
as their codes obtained via [phe](https://github.com/thlorenz/phe) `cardCodes`.

**Parameters**

-   `comboCodes`  
-   `rangeCodes`  
-   `times`  
-   `trackCombos`  
-   `boardCodes`  

### raceRangeCodes

Same as @see raceRange, except that the combo and range cards are given
as their codes obtained via [phe](https://github.com/thlorenz/phe) `cardCodes`.

**Parameters**

-   `combo1`  
-   `range`  
-   `times`  
-   `trackCombos`  

### raceCombosForBoard

Races two combos against each other.

**Parameters**

-   `combo1` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** first combo to race i.e. `[ 'As', 'Ad' ]`
-   `combo2` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** second combo to race i.e. `[ 'As', 'Ad' ]`
-   `times` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** the number of times to race, if not supplied combos are races against all possible boards (optional, default `null`)
-   `board` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>?** omit for preflop, but provide for
    postflop to race against boards that just add a turn or river card to the given one (optional, default `null`)

Returns **any** count of how many times combo1 wins, looses or ties, i.e. `{ win, loose, tie }`

### raceCombos

Races two combos against each other.

**Parameters**

-   `combo1` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** first combo to race i.e. `[ 'As', 'Ad' ]`
-   `combo2` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** second combo to race i.e. `[ 'As', 'Ad' ]`
-   `times` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** the number of times to race, if not supplied combos are races against all possible boards (optional, default `null`)

Returns **any** count of how many times combo1 wins, looses or ties, i.e. `{ win, loose, tie }`

### raceRangeForBoard

Race the given combo vs. the given combo to count number of wins, losses and ties.
The boards created for the race will include all cards of the given board.

**Parameters**

-   `combo` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** to race i.e. `[ 'As', 'Ad' ]`
-   `range` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>>** multiple combos to raise against it, i.e. `[ [ 'Ks', 'Kd' ], [ 'Qs', 'Qd' ] ]`
-   `times` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** the number of times to race, if not supplied combos are races against all possible boards (optional, default `null`)
-   `trackCombos` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the counts for each combos are tracked (optional, default `false`)
-   `board` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>?** omit for preflop, but provide for
    postflop to race against boards that just add a turn or river card to the given one (optional, default `null`)

Returns **any** count of how many times the combo wins, looses or ties, i.e. `{ win, loose, tie }`

### raceRange

Race the given combo vs. the given combo to count number of wins, losses and ties.

**Parameters**

-   `combo` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** to race i.e. `[ 'As', 'Ad' ]`
-   `range` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>>** multiple combos to raise against it, i.e. `[ [ 'Ks', 'Kd' ], [ 'Qs', 'Qd' ] ]`
-   `times` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** the number of times to race, if not supplied combos are races against all possible boards (optional, default `null`)
-   `trackCombos` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the counts for each combos are tracked (optional, default `false`)

Returns **any** count of how many times the combo wins, looses or ties, i.e. `{ win, loose, tie }`

### rates

Given win, loose and tie count it converts those to winning rates
in percent.

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `$0.win` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** number of wins
    -   `$0.loose` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** number of losses
    -   `$0.tie` **[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** number of ties
    -   `$0.combos` **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)&lt;[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String), [Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)>?** map of counts per combo,
        if given their rates are calculated as well (optional, default `null`)

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** win rates \`{ winRate, looseRate, tieRate, combos? }

## License

MIT
