# ENT — Fourmilab Random Sequence Tester

The [Fourmilab Random Sequence Tester](https://www.fourmilab.ch/random/),
**ent**, applies various tests to sequences of bytes stored in files
and reports the results of those tests. The program is useful for
evaluating pseudorandom number generators for encryption and
statistical sampling applications, compression algorithms, and other
applications where the information density of a file is of interest.

## Description

**ent** performs a variety of tests on the stream of bytes in its input
file (or standard input if no input file is specified) and produces
output as follows on the standard output stream:

    Entropy = 7.980627 bits per character.

    Optimum compression would reduce the size
    of this 51768 character file by 0 percent.

    Chi square distribution for 51768 samples is 1542.26, and randomly
    would exceed this value less than 0.01 percent of the times.

    Arithmetic mean value of data bytes is 125.93 (127.5 = random).
    Monte Carlo value for Pi is 3.169834647 (error 0.90 percent).
    Serial correlation coefficient is 0.004249 (totally uncorrelated = 0.0).

The values calculated are as follows:

#### Entropy
The information density of the contents of the file, expressed as a
number of bits per character. The results above, which resulted from
processing an image file compressed with JPEG, indicate that the file
is extremely dense in information—essentially random. Hence,
compression of the file is unlikely to reduce its size. By contrast,
the C source code of the program has entropy of about 4.9 bits per
character, indicating that optimal compression of the file would reduce
its size by 38%. \[Hamming, pp. 104–108\]

#### Chi-square Test
The chi-square test is the most commonly used test for the randomness
of data, and is extremely sensitive to errors in pseudorandom sequence
generators. The chi-square distribution is calculated for the stream of
bytes in the file and expressed as an absolute number and a percentage
which indicates how frequently a truly random sequence would exceed the
value calculated. We interpret the percentage as the degree to which
the sequence tested is suspected of being non-random. If the percentage
is greater than 99% or less than 1%, the sequence is almost certainly
not random. If the percentage is between 99% and 95% or between 1% and
5%, the sequence is suspect. Percentages between 90% and 95% and 5% and
10% indicate the sequence is “almost suspect”. Note that our JPEG file,
while very dense in information, is far from random as revealed by the
chi-square test.

Applying this test to the output of various pseudorandom sequence
generators is interesting. The low-order 8 bits returned by the
standard Unix `rand()` function, for example, yields:

    Chi square distribution for 500000 samples is 0.01, and randomly
    would exceed this value more than 99.99 percent of the times.

While an improved generator \[Park & Miller\] reports:

    Chi square distribution for 500000 samples is 212.53, and randomly
    would exceed this value 97.53 percent of the times.

Thus, the standard Unix generator (or at least the low-order bytes it
returns) is unacceptably non-random, while the improved generator is
much better but still sufficiently non-random to cause concern for
demanding applications. Contrast both of these software generators with
the chi-square result of a genuine random sequence created by timing
radioactive decay events.

    Chi square distribution for 500000 samples is 249.51, and randomly
    would exceed this value 40.98 percent of the times.

See \[Knuth, pp. 35–40\] for more information on the chi-square test.

#### Arithmetic Mean
This is simply the result of summing the all the bytes (bits if the 
`-b` option is specified) in the file and dividing by the file length. 
If the data are close to random, this should be about 127.5 (0.5 for 
`-b` option output). If the mean departs from this value, the values 
are consistently high or low.

#### Monte Carlo Value for Pi
Each successive sequence of six bytes is used as 24 bit X and Y 
co-ordinates within a square.  If the distance of the 
randomly-generated point is less than the radius of a circle inscribed 
within the square, the six-byte sequence is considered a “hit”.  The 
percentage of hits can be used to calculate the value of π.  For very 
large streams (this approximation converges very slowly), the value 
will approach the correct value of π if the sequence is close to 
random.  A 500000 byte file created by radioactive decay yielded:

    Monte Carlo value for Pi is 3.143580574 (error 0.06 percent).

#### Serial Correlation Coefficient
This quantity measures the extent to which each byte in the file 
depends upon the previous byte. For random sequences, this value (which 
can be positive or negative) will, of course, be close to zero.  A 
non-random byte stream such as a C program will yield a serial 
correlation coefficient on the order of 0.5.  Wildly predictable data 
such as uncompressed bitmaps will exhibit serial correlation 
coefficients approaching 1.  See [Knuth, pp. 64–65] for more details.

## License

This software is licensed under the Creative Commons
Attribution-ShareAlike license.  Please see [LICENSE.md](LICENSE.md) in
this repository for details.

## References

[Hamming]
Hamming, Richard W.  *Coding and Information Theory*. Englewood Cliffs 
NJ: Prentice-Hall, 1980.  ISBN 978-0-13-139139-0.

[Knuth]
Knuth, Donald E.  *The Art of Computer Programming, Volume 2 / 
Seminumerical Algorithms*.  Reading MA: Addison-Wesley, 1969. ISBN 
978-0-201-89684-8.

[Lempel & Ziv]
Ziv J. and A. Lempel.  “A Universal Algorithm for Sequential Data 
Compression”. IEEE Transactions on Information Theory 23, 3, pp. 
337–343.

[Park & Miller]
Park, Stephen K. and Keith W. Miller.  “Random Number Generators: Good 
Ones Are Hard to Find”. Communications of the ACM, October 1988, p. 
1192.

*[Introduction to Probability and
Statistics](https://www.fourmilab.ch/rpkp/experiments/statistics.html)*
at Fourmilab
