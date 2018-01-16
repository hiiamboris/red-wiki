Moving to Red from LoDash?

# Array

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.chunk               | split (future) | |
| _.compact             | trim | |
| _.concat              | append copy | |
| _.difference          | difference | |
| _.differenceBy        | | |
| _.differenceWith      | | |
| _.drop                | remove | |
| _.dropRight           | | |
| _.dropRightWhile      | | |
| _.dropWhile           | | |
| _.fill                | | |
| _.findIndex           | | |
| _.findLastIndex       | | |
| _.first -> head       | | |
| _.flatten             | | |
| _.flattenDeep         | | |
| _.flattenDepth        | | |
| _.fromPairs           | | |
| _.head                | first | |
| _.indexOf             | index? find | |
| _.initial             | | |
| _.intersection        | intersect | |
| _.intersectionBy      | | |
| _.intersectionWith    | | |
| _.join                | | |
| _.last                | last | |
| _.lastIndexOf         | index? find last | |
| _.nth                 | pick | |
| _.pull                | | |
| _.pullAll             | | |
| _.pullAllBy           | | |
| _.pullAllWith         | | |
| _.pullAt              | | |
| _.remove              | remove-each | |
| _.reverse             | reverse | |
| _.slice               | | |
| _.sortedIndex         | | |
| _.sortedIndexBy       | | |
| _.sortedIndexOf       | | |
| _.sortedLastIndex     | | |
| _.sortedLastIndexBy   | | |
| _.sortedLastIndexOf   | | |
| _.sortedUniq          | | |
| _.sortedUniqBy        | | |
| _.tail                | next *or* copy next | |
| _.take                | take | |
| _.takeRight           | take/last | |
| _.takeRightWhile      | | |
| _.takeWhile           | | |
| _.union               | union | |
| _.unionBy             | | |
| _.unionWith           | | |
| _.uniq                | unique | |
| _.uniqBy              | | |
| _.uniqWith            | | |
| _.unzip               | | |
| _.unzipWith           | | |
| _.without             | trim/with | |
| _.xor                 | difference | |
| _.xorBy               | | |
| _.xorWith             | | |
| _.zip                 | | |
| _.zipObject           | | |
| _.zipObjectDeep       | | |
| _.zipWith             | | |

# Collection

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.countBy             | | |
| _.each                | | |
| _.eachRight           | | |
| _.every               | | |
| _.filter              | | |
| _.find                | find | |
| _.findLast            | find/last | |
| _.flatMap             | | |
| _.flatMapDeep         | | |
| _.flatMapDepth        | | |
| _.forEach             | foreach | |
| _.forEachRight        | | |
| _.groupBy             | | |
| _.includes            | | |
| _.invokeMap           | | |
| _.keyBy               | | |
| _.map                 | | |
| _.orderBy             | | |
| _.partition           | | |
| _.reduce              | | |
| _.reduceRight         | | |
| _.reject              | | |
| _.sample              | | |
| _.sampleSize          | | |
| _.shuffle             | random | |
| _.size                | length? | |
| _.some                | | |
| _.sortBy              | | |

# Date

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.now                 | Now | |

# Function

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.after               | | |
| _.ary                 | | |
| _.before              | | |
| _.bind                | | |
| _.bindKey             | | |
| _.curry               | | |
| _.curryRight          | | |
| _.debounce            | | |
| _.defer               | | |
| _.delay               | | |
| _.flip                | | |
| _.memoize             | | |
| _.negate              | | |
| _.once                | | |
| _.overArgs            | | |
| _.partial             | | |
| _.partialRight        | | |
| _.rearg               | | |
| _.rest                | | |
| _.spread              | | |
| _.throttle            | | |
| _.unary               | | |
| _.wrap                | | |

# Lang

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.castArray           | | |
| _.clone               | copy | |
| _.cloneDeep           | copy/deep | |
| _.cloneDeepWith       | | |
| _.cloneWith           | | |
| _.conformsTo          | | |
| _.eq                  | | |
| _.gt                  | | |
| _.gte                 | | |
| _.isArguments         | | |
| _.isArray             | | |
| _.isArrayBuffer       | | |
| _.isArrayLike         | | |
| _.isArrayLikeObject   | | |
| _.isBoolean           | | |
| _.isBuffer            | | |
| _.isDate              | | |
| _.isElement           | | |
| _.isEmpty             | | |
| _.isEqual             | | |
| _.isEqualWith         | | |
| _.isError             | | |
| _.isFinite            | | |
| _.isFunction          | | |
| _.isInteger           | | |
| _.isLength            | | |
| _.isMap               | | |
| _.isMatch             | | |
| _.isMatchWith         | | |
| _.isNaN               | | |
| _.isNative            | | |
| _.isNil               | | |
| _.isNull              | | |
| _.isNumber            | | |
| _.isObject            | | |
| _.isObjectLike        | | |
| _.isPlainObject       | | |
| _.isRegExp            | | |
| _.isSafeInteger       | | |
| _.isSet               | | |
| _.isString            | | |
| _.isSymbol            | | |
| _.isTypedArray        | | |
| _.isUndefined         | | |
| _.isWeakMap           | | |
| _.isWeakSet           | | |
| _.lt                  | | |
| _.lte                 | | |
| _.toArray             | | |
| _.toFinite            | | |
| _.toInteger           | | |
| _.toLength            | | |
| _.toNumber            | | |
| _.toPlainObject       | | |
| _.toSafeInteger       | | |
| _.toString            | | |

# Math

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.add                 | | |
| _.ceil                | round/ceiling | |
| _.divide              | | |
| _.floor               | round/floor | |
| _.max                 | | |
| _.maxBy               | | |
| _.mean                | | |
| _.meanBy              | | |
| _.min                 | | |
| _.minBy               | | |
| _.multiply            | | |
| _.round               | round/to | |
| _.subtract            | | |
| _.sum                 | | |
| _.sumBy               | | |

# Number

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.clamp               | | |
| _.inRange             | | |
| _.random              | random | |

# Object

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.assign              | | |
| _.assignIn            | | |
| _.assignInWith        | | |
| _.assignWith          | | |
| _.at                  | | |
| _.create              | | |
| _.defaults            | | |
| _.defaultsDeep        | | |
| _.entries             | | |
| _.entriesIn           | | |
| _.extend              | | |
| _.extendWith          | | |
| _.findKey             | | |
| _.findLastKey         | | |
| _.forIn               | | |
| _.forInRight          | | |
| _.forOwn              | | |
| _.forOwnRight         | | |
| _.functions           | | |
| _.functionsIn         | | |
| _.get                 | | |
| _.has                 | | |
| _.hasIn               | | |
| _.invert              | | |
| _.invertBy            | | |
| _.invoke              | | |
| _.keys                | words-of *or* keys-of | |
| _.keysIn              | | |
| _.mapKeys             | | |
| _.mapValues           | | |
| _.merge               | | |
| _.mergeWith           | | |
| _.omit                | | |
| _.omitBy              | | |
| _.pick                | | |
| _.pickBy              | | |
| _.result              | | |
| _.set                 | | |
| _.setWith             | | |
| _.toPairs             | | |
| _.toPairsIn           | | |
| _.transform           | | |
| _.unset               | | |
| _.update              | | |
| _.updateWith          | | |
| _.values              | | |
| _.valuesIn            | | |

# Seq

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _                     | N/A | |
| _.chain               | | |
| _.tap                 | | |
| _.thru                | | |
| _.prototype           | | |
| _.prototype.at        | | |
| _.prototype.chain     | | |
| _.prototype.commit    | | |
| _.prototype.next      | | |
| _.prototype.plant     | | |
| _.prototype.reverse   | | |
| _.prototype.toJSON    | | |
| _.prototype.value     | | |
| _.prototype.valueOf   | | |

# String

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.camelCase           | | |
| _.capitalize          | | |
| _.deburr              | | |
| _.endsWith            | | |
| _.escape              | | |
| _.escapeRegExp        | | |
| _.kebabCase           | | |
| _.lowerCase           | | |
| _.lowerFirst          | lowercase/part <string> 1 | |
| _.pad                 | | |
| _.padEnd              | | |
| _.padStart            | | |
| _.parseInt            | | |
| _.repeat              | | |
| _.replace             | | |
| _.snakeCase           | | |
| _.split               | | |
| _.startCase           | | |
| _.startsWith          | | |
| _.template            | | |
| _.toLower             | lowercase | |
| _.toUpper             | uppercase | |
| _.trim                | trim | |
| _.trimEnd             | trim/tail | |
| _.trimStart           | trim/head | |
| _.truncate            | | |
| _.unescape            | | |
| _.upperCase           | | |
| _.upperFirst          | uppercase/part <string> 1 | |
| _.words               | | |

# Util

| LoDash Name           | Red Name             | Notes |
| --------------------- | -------------------- | ----- |
| _.attempt             | | |
| _.bindAll             | | |
| _.cond                | | |
| _.conforms            | | |
| _.constant            | | |
| _.defaultTo           | | |
| _.flow                | | |
| _.flowRight           | | |
| _.identity            | | |
| _.iteratee            | | |
| _.matches             | | |
| _.matchesProperty     | | |
| _.method              | | |
| _.methodOf            | | |
| _.mixin               | | |
| _.noConflict          | | |
| _.noop                | | |
| _.nthArg              | | |
| _.over                | | |
| _.overEvery           | | |
| _.overSome            | | |
| _.property            | | |
| _.propertyOf          | | |
| _.range               | | |
| _.rangeRight          | | |
| _.runInContext        | | |
| _.stubArray           | | |
| _.stubFalse           | | |
| _.stubObject          | | |
| _.stubString          | | |
| _.stubTrue            | | |
| _.times               | | |
| _.toPath              | | |
| _.uniqueId            | | |
