<!--

@license Apache-2.0

Copyright (c) 2024 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# Bernoulli Random Numbers

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Fill a strided array with pseudorandom numbers drawn from a [Bernoulli][@stdlib/random/base/bernoulli] distribution.



<section class="usage">

## Usage

```javascript
import bernoulli from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-strided-bernoulli@deno/mod.js';
```

You can also import the following named exports from the package:

```javascript
import { factory } from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-strided-bernoulli@deno/mod.js';
```

#### bernoulli( N, p, sp, out, so )

Fills a strided array with pseudorandom numbers drawn from a [Bernoulli][@stdlib/random/base/bernoulli] distribution.

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
bernoulli( out.length, [ 0.5 ], 0, out, 1 );
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **p**: rate parameter.
-   **sp**: index increment for `p`.
-   **out**: output array.
-   **so**: index increment for `out`.

The `N` and stride parameters determine which strided array elements are accessed at runtime. For example, to access every other value in `out`,

```javascript
var out = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

bernoulli( 3, [ 0.5 ], 0, out, 2 );
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

// Initial array:
var p0 = new Float64Array( [ 0.5, 0.5, 0.5, 0.5, 0.5, 0.5 ] );

// Create offset view:
var p1 = new Float64Array( p0.buffer, p0.BYTES_PER_ELEMENT*3 ); // start at 4th element

// Create an output array:
var out = new Float64Array( 3 );

// Fill the output array:
bernoulli( out.length, p1, -1, out, 1 );
```

#### bernoulli.ndarray( N, p, sp, op, out, so, oo )

Fills a strided array with pseudorandom numbers drawn from a [Bernoulli][@stdlib/random/base/bernoulli] distribution using alternative indexing semantics.

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
bernoulli.ndarray( out.length, [ 0.5 ], 0, 0, out, 1, 0 );
```

The function has the following additional parameters:

-   **op**: starting index for `p`.
-   **oo**: starting index for `out`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying `buffer`, the offset parameters support indexing semantics based on starting indices. For example, to access every other value in `out` starting from the second value,

```javascript
var out = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

bernoulli.ndarray( 3, [ 0.5 ], 0, 0, out, 2, 1 );
```

#### bernoulli.factory( \[options] )

Returns a function for filling strided arrays with pseudorandom numbers drawn from a [Bernoulli][@stdlib/random/base/bernoulli] distribution.

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

var random = bernoulli.factory();
// returns <Function>

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
random( out.length, [ 0.5 ], 0, out, 1 );
```

The function accepts the following `options`:

-   **prng**: pseudorandom number generator for generating uniformly distributed pseudorandom numbers on the interval `[0,1)`. If provided, the function **ignores** both the `state` and `seed` options. In order to seed the underlying pseudorandom number generator, one must seed the provided `prng` (assuming the provided `prng` is seedable).
-   **seed**: pseudorandom number generator seed.
-   **state**: a [`Uint32Array`][@stdlib/array/uint32] containing pseudorandom number generator state. If provided, the function ignores the `seed` option.
-   **copy**: `boolean` indicating whether to copy a provided pseudorandom number generator state. Setting this option to `false` allows sharing state between two or more pseudorandom number generators. Setting this option to `true` ensures that an underlying generator has exclusive control over its internal state. Default: `true`.

To use a custom PRNG as the underlying source of uniformly distributed pseudorandom numbers, set the `prng` option.

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';
import minstd from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-minstd@deno/mod.js';

var opts = {
    'prng': minstd.normalized
};
var random = bernoulli.factory( opts );

var out = new Float64Array( 10 );
random( out.length, [ 0.5 ], 0, out, 1 );
```

To seed the underlying pseudorandom number generator, set the `seed` option.

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

var opts = {
    'seed': 12345
};
var random = bernoulli.factory( opts );

var out = new Float64Array( 10 );
random( out.length, [ 0.5 ], 0, out, 1 );
```

* * *

#### random.PRNG

The underlying pseudorandom number generator.

```javascript
var prng = bernoulli.PRNG;
// returns <Function>
```

#### bernoulli.seed

The value used to seed the underlying pseudorandom number generator.

```javascript
var seed = bernoulli.seed;
// returns <Uint32Array>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-minstd-shuffle' ).normalized;

var random = bernoulli.factory({
    'prng': minstd
});
// returns <Function>

var seed = random.seed;
// returns null
```

#### bernoulli.seedLength

Length of underlying pseudorandom number generator seed.

```javascript
var len = bernoulli.seedLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-minstd-shuffle' ).normalized;

var random = bernoulli.factory({
    'prng': minstd
});
// returns <Function>

var len = random.seedLength;
// returns null
```

#### bernoulli.state

Writable property for getting and setting the underlying pseudorandom number generator state.

```javascript
var state = bernoulli.state;
// returns <Uint32Array>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-minstd-shuffle' ).normalized;

var random = bernoulli.factory({
    'prng': minstd
});
// returns <Function>

var state = random.state;
// returns null
```

#### bernoulli.stateLength

Length of underlying pseudorandom number generator state.

```javascript
var len = bernoulli.stateLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-minstd-shuffle' ).normalized;

var random = bernoulli.factory({
    'prng': minstd
});
// returns <Function>

var len = random.stateLength;
// returns null
```

#### bernoulli.byteLength

Size (in bytes) of underlying pseudorandom number generator state.

```javascript
var sz = bernoulli.byteLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-minstd-shuffle' ).normalized;

var random = bernoulli.factory({
    'prng': minstd
});
// returns <Function>

var sz = random.byteLength;
// returns null
```

</section>

<!-- /.usage -->

<section class="notes">

* * *

## Notes

-   If `N <= 0`, both `bernoulli` and `bernoulli.ndarray` leave the output array unchanged.
-   Both `bernoulli` and `bernoulli.ndarray` support array-like objects having getter and setter accessors for array element access.

</section>

<!-- /.notes -->

<section class="examples">

* * *

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import zeros from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-zeros@deno/mod.js';
import zeroTo from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-zero-to@deno/mod.js';
import logEach from 'https://cdn.jsdelivr.net/gh/stdlib-js/console-log-each@deno/mod.js';
import bernoulli from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-strided-bernoulli@deno/mod.js';

// Specify a PRNG seed:
var opts = {
    'seed': 1234
};

// Create a seeded PRNG:
var rand1 = bernoulli.factory( opts );

// Create an array:
var x1 = zeros( 10, 'float64' );

// Fill the array with pseudorandom numbers:
rand1( x1.length, [ 0.5 ], 0, x1, 1 );

// Create another function for filling strided arrays:
var rand2 = bernoulli.factory( opts );
// returns <Function>

// Create a second array:
var x2 = zeros( 10, 'generic' );

// Fill the array with the same pseudorandom numbers:
rand2( x2.length, [ 0.5 ], 0, x2, 1 );

// Create a list of indices:
var idx = zeroTo( x1.length, 'generic' );

// Print the array contents:
logEach( 'x1[%d] = %.2f; x2[%d] = %.2f', idx, x1, idx, x2 );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

* * *

## See Also

-   <span class="package-name">[`@stdlib/random-base/bernoulli`][@stdlib/random/base/bernoulli]</span><span class="delimiter">: </span><span class="description">Bernoulli distributed pseudorandom numbers.</span>
-   <span class="package-name">[`@stdlib/random-array/bernoulli`][@stdlib/random/array/bernoulli]</span><span class="delimiter">: </span><span class="description">create an array containing pseudorandom numbers drawn from a Bernoulli distribution.</span>

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2024. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-strided-bernoulli.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-strided-bernoulli

[test-image]: https://github.com/stdlib-js/random-strided-bernoulli/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/random-strided-bernoulli/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-strided-bernoulli/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-strided-bernoulli?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-strided-bernoulli.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-strided-bernoulli/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/random-strided-bernoulli/tree/deno
[deno-readme]: https://github.com/stdlib-js/random-strided-bernoulli/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/random-strided-bernoulli/tree/umd
[umd-readme]: https://github.com/stdlib-js/random-strided-bernoulli/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/random-strided-bernoulli/tree/esm
[esm-readme]: https://github.com/stdlib-js/random-strided-bernoulli/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/random-strided-bernoulli/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-strided-bernoulli/main/LICENSE

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/random/base/bernoulli]: https://github.com/stdlib-js/random-base-bernoulli/tree/deno

[@stdlib/array/uint32]: https://github.com/stdlib-js/array-uint32/tree/deno

<!-- <related-links> -->

[@stdlib/random/array/bernoulli]: https://github.com/stdlib-js/random-array-bernoulli/tree/deno

<!-- </related-links> -->

</section>

<!-- /.links -->
