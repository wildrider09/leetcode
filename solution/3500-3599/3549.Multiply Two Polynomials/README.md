---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3500-3599/3549.Multiply%20Two%20Polynomials/README_EN.md
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3549. Multiply Two Polynomials 🔒](https://leetcode.com/problems/multiply-two-polynomials)

[Chinese Version](/solution/3500-3599/3549.Multiply%20Two%20Polynomials/README.md)

## Description

<!-- description:start -->

<p data-end="315" data-start="119">You are given two integer arrays <code>poly1</code> and <code>poly2</code>, where the element at index <code>i</code> in each array represents the coefficient of <code>x<sup>i</sup></code> in a polynomial.</p>

<p>Let <code>A(x)</code> and <code>B(x)</code> be the polynomials represented by <code>poly1</code> and <code>poly2</code>, respectively.</p>

<p>Return an integer array <code>result</code> of length <code>(poly1.length + poly2.length - 1)</code> representing the coefficients of the product polynomial <code>R(x) = A(x) * B(x)</code>, where <code>result[i]</code> denotes the coefficient of <code>x<sup>i</sup></code> in <code>R(x)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">poly1 = [3,2,5], poly2 = [1,4]</span></p>

<p><strong>Output:</strong> <span class="example-io">[3,14,13,20]</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li><code>A(x) = 3 + 2x + 5x<sup>2</sup></code> and <code>B(x) = 1 + 4x</code></li>
	<li><code>R(x) = (3 + 2x + 5x<sup>2</sup>) * (1 + 4x)</code></li>
	<li><code>R(x) = 3 * 1 + (3 * 4 + 2 * 1)x + (2 * 4 + 5 * 1)x<sup>2</sup> + (5 * 4)x<sup>3</sup></code></li>
	<li><code>R(x) = 3 + 14x + 13x<sup>2</sup> + 20x<sup>3</sup></code></li>
	<li>Thus, result = <code>[3, 14, 13, 20]</code>.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">poly1 = [1,0,-2], poly2 = [-1]</span></p>

<p><strong>Output:</strong> <span class="example-io">[-1,0,2]</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li><code>A(x) = 1 + 0x - 2x<sup>2</sup></code> and <code>B(x) = -1</code></li>
	<li><code>R(x) = (1 + 0x - 2x<sup>2</sup>) * (-1)</code></li>
	<li><code>R(x) = -1 + 0x + 2x<sup>2</sup></code></li>
	<li>Thus, result = <code>[-1, 0, 2]</code>.</li>
</ul>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">poly1 = [1,5,-3], poly2 = [-4,2,0]</span></p>

<p><strong>Output:</strong> <span class="example-io">[-4,-18,22,-6,0]</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li><code>A(x) = 1 + 5x - 3x<sup>2</sup></code> and <code>B(x) = -4 + 2x + 0x<sup>2</sup></code></li>
	<li><code>R(x) = (1 + 5x - 3x<sup>2</sup>) * (-4 + 2x + 0x<sup>2</sup>)</code></li>
	<li><code>R(x) = 1 * -4 + (1 * 2 + 5 * -4)x + (5 * 2 + -3 * -4)x<sup>2</sup> + (-3 * 2)x<sup>3</sup> + 0x<sup>4</sup></code></li>
	<li><code>R(x) = -4 -18x + 22x<sup>2</sup> -6x<sup>3</sup> + 0x<sup>4</sup></code></li>
	<li>Thus, result = <code>[-4, -18, 22, -6, 0]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= poly1.length, poly2.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>3</sup> &lt;= poly1[i], poly2[i] &lt;= 10<sup>3</sup></code></li>
	<li><code>poly1</code> and <code>poly2</code> contain at least one non-zero coefficient.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: FFT

We can use the Fast Fourier Transform (FFT) to efficiently compute the product of two polynomials. FFT is an efficient algorithm that can compute the product of polynomials in $O(n \log n)$ time complexity.

The specific steps are as follows:

1. **Padding the length** Let the result length be $m = |A|+|B|-1$, and round it up to the nearest power of 2, $n$, to facilitate divide-and-conquer FFT.
2. **FFT transformation** Perform the forward FFT (with `invert=False`) on both coefficient sequences.
3. **Pointwise multiplication** Multiply the corresponding elements in the frequency domain.
4. **Inverse FFT** Perform the inverse FFT (with `invert=True`) on the product sequence, and round the real parts to the nearest integer to obtain the final coefficients.

The time complexity is $O(n \log n)$, and the space complexity is $O(n)$, where $n$ is the length of the polynomials.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public long[] multiply(int[] poly1, int[] poly2) {
        if (poly1 == null || poly2 == null || poly1.length == 0 || poly2.length == 0) {
            return new long[0];
        }

        int m = poly1.length + poly2.length - 1;
        int n = 1;
        while (n < m) n <<= 1;

        Complex[] fa = new Complex[n];
        Complex[] fb = new Complex[n];
        for (int i = 0; i < n; i++) {
            fa[i] = new Complex(i < poly1.length ? poly1[i] : 0, 0);
            fb[i] = new Complex(i < poly2.length ? poly2[i] : 0, 0);
        }

        fft(fa, false);
        fft(fb, false);

        for (int i = 0; i < n; i++) {
            fa[i] = fa[i].mul(fb[i]);
        }

        fft(fa, true);

        long[] res = new long[m];
        for (int i = 0; i < m; i++) {
            res[i] = Math.round(fa[i].re);
        }
        return res;
    }

    private static void fft(Complex[] a, boolean invert) {
        int n = a.length;

        for (int i = 1, j = 0; i < n; i++) {
            int bit = n >>> 1;
            while ((j & bit) != 0) {
                j ^= bit;
                bit >>>= 1;
            }
            j ^= bit;
            if (i < j) {
                Complex tmp = a[i];
                a[i] = a[j];
                a[j] = tmp;
            }
        }

        for (int len = 2; len <= n; len <<= 1) {
            double ang = 2 * Math.PI / len * (invert ? -1 : 1);
            Complex wlen = new Complex(Math.cos(ang), Math.sin(ang));

            for (int i = 0; i < n; i += len) {
                Complex w = new Complex(1, 0);
                int half = len >>> 1;
                for (int j = 0; j < half; j++) {
                    Complex u = a[i + j];
                    Complex v = a[i + j + half].mul(w);
                    a[i + j] = u.add(v);
                    a[i + j + half] = u.sub(v);
                    w = w.mul(wlen);
                }
            }
        }

        if (invert) {
            for (int i = 0; i < n; i++) {
                a[i].re /= n;
                a[i].im /= n;
            }
        }
    }

    private static final class Complex {
        double re, im;
        Complex(double re, double im) {
            this.re = re;
            this.im = im;
        }
        Complex add(Complex o) {
            return new Complex(re + o.re, im + o.im);
        }
        Complex sub(Complex o) {
            return new Complex(re - o.re, im - o.im);
        }
        Complex mul(Complex o) {
            return new Complex(re * o.re - im * o.im, re * o.im + im * o.re);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
