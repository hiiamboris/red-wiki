<table>
  <tr>
    <td>REP:</td>
    <td>XXXX</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>Add mathematical and time functions</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>Donald Tsang</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Draft</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>?????</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td></td>
  </tr>
</table>

# Mathematics (for scientific calculators):
* Trigonometric functions: sin, cos, tan, asin, acos, atan
* Hyperbolic function: sinh, cosh, tanh, asinh, acosh, atanh
* Logarithms: log (for decimal), lg (for binary), lon (for dozenal), ln (for natural)
* Quotient and Modulo in multiple ways: Truncated, Floored, Eucliedian
* Modulo 1: FRACtion (fractional part), SAWtooth (y minus floor of y)
* Rounding: Ceiling, Floor, Truncate, Round Half up, Round Half down, Round half zero
* Factorial or n!=n*(n-1)! Permutation or P(n,r)=n!/(n-r)! Combination or C(n,r)=P(n,r)/r!
* Arbitrary precision arithmetic (in bits, base10, base12, base16 and base60)
     * [Q-format as example](https://en.wikipedia.org/wiki/Q_%28number_format%29)
* Number arithmetic
    * Real numbers (N=a)
        * int! where a is an integer
        * frac! where a is a data set containing either:
            * a sign bit, a numerator part and a denominator part (easier to implement)
            * a sign bit, a size bit, a large part and a small part (better for arithmetic)
            * sign: if the value of a number is smaller than 0, it's "1", or else "0"
            * size: if the absolute of a number is smaller than 1, it's "1", or else "0"
        * float! where a is a data set containing a sign bit, a fraction part and an exponent part
        * [angle!](https://en.wikipedia.org/wiki/Binary_scaling#Binary_angles)
            * full circle's alternate name: turn, cycle, revolution/rev, and rotation/rot, τ
            * 1 full circle = 2π Radian/rad = 360 Degrees/deg = 400 Gradians/grad
            * = 4 Quadrant = 6 Sextant = 24 Hour-angle = 32 Point-wind = 60 Hexacontade = 144/180 Pechus
            * 1 Degree = 60 Minutes, 1 Minute = 60 Seconds, 1 Second = 60 Thirds, 1 Third = 60 Fourths
            * 1 grad = 10 Decigrads,  1 rad = 1000 Milliradians
            * Milliemes: = 6000 USSR-mil = 6283 Trig-mil = 6300 Swed-mil = 6400 NATO-mil
    * Fraction mathematics
        * a/b+c/d=(ad/gcd)/lcm+(bc/gcd)/lcm where gcd(b,d)
        * a/b+c/d=((ad+bc)/gcd)/(bd/gcd) where gcd(ad+bc,bd)
        * Simplification: a/b=(a/gcd)/(b/gcd) where gcd(a,b)
        * (a/b)(c/d)=(ac/gcd)/(bd/gcd) where gcd(ac,bd)
        * (a/b)/(c/d)=(ad/gcd)/(bc/gcd) where gcd(ad,bc)
    * Gaussian numbers (where N=a+bi and i^2+1=0) (i^2=-1)
        * gaussInt! where a and b are both int! datatypes
        * gaussFrac! where a and b are both frac! datatypes
        * gaussFloat! where a and b are both float! datatypes
    * Eulerian numbers (where N=a+bw and w^2+w+1=0) (w^2=-(1+w))
        * eulerInt! where a and b are both int! datatypes
        * eulerFrac! where a and b are both frac! datatypes
        * eulerFloat! where a and b are both float! datatypes
    * Polar Coordinates (where N={r,θ}, θ is angle!)
        * polarInt! where r is int! datatype
        * polarFrac! where r is frac! datatype
        * polarFloat! where r is float! datatype
    * Gaussian/Eulerian integer and fraction arithmetic
        * gaussInt! or gaussFrac! or gaussFloat! ==> eulerFloat! or polarFloat!
        * eulerInt! or eulerFrac! or eulerFloat! ==> gaussFloat! or polarFloat!
        * polarInt! or polarFrac! or polarFloat! ==> gaussFloat! or eulerFloat!
        * Gaussian mathematics
            * (a+bi)+(c+di)=(a+c)+(b+d)i [addition]
            * (a+bi)(c+di)=(ac-bd)+(ad+bc)i [multiplication]
            * (a+bi)/(c+di)=((ac-bd)/(c^2+d^2))+((ad+bc)/(c^2+d^2))i [division]
        * Eulerian mathematics
            * (a+bw)+(c+dw)=(a+c)+(b+d)w [addition]
            * (a+bw)(c+dw)=(ac-bd)+(bc+ad-bd)w [multiplication]
            * (a+bw)/(c+dw)=((ac-bd)/(c^2-cd+d^2))+((bc+ad-bd)/(c^2-cd+d^2))w [division]
        * Gauss-Euler combination mathematics [p=sqrt(3)]
            * a+bi=a+0p+bi+0ip and a+bw=(a-(b/2))+0p+0i+(b/2)ip
            * (a+bp+ci+dip)+(e+fp+gi+hip)=(a+e)+(b+f)p+(c+g)i+(d+h)ip [addition]
            * (a+bp+ci+dip)(e+fp+gi+hip)=(ae+3bf-cg-3dh)+(af+be-ch-dg)p+(ag+bh+ce+df)i+(ah+bg+cf+de)ip [multiplication]
        * Polar Mathematics
            * {r,θ}+{l,φ}={sqrt(r^2+l^2+2rl*cos(abs(θ-φ))),atan2((rsinθ+lsinφ)/(rcosθ+lcosφ))} [addition]
                 * abs is done in angle! in binary angle/radian mods
                 * [atan2](https://en.wikipedia.org/wiki/Atan2) is a function to find θ with x and y
            * {r,θ}{l,φ}={r*l,θ+φ} [multiplication]
            * {r,θ}/{l,φ}={r/l,θ-φ} [division]
    * Normal, Gaussian and Eulerian prime determination
        * Normal Prime determination function prime(p)
            * Agoh-Giuga: Σ i=1 to p-1 (i^(p-1)) = -1 (mod p)
            * Wilson: (p-1)! = -1 (mod p)
            * Agoh-Guiga & Wilson: Π i=1 to p-1 (i^(p-1)) = 1 and Σ i= 1 to p-1 (i^(p-1)) = -1 (mod p)
        * Non-deterministic prime test with primeTest(p,a) a is witness
	    * Fermat test: a^(p-1) = 1 (mod p)
	    * Lucas: fermat test but a^((n-1)/x) != 1 (mod p) for all prime factors of n-1 as x
	    * Solovay–Strassen: a^((n-1)/2) = Jacobi(a|n) (mod n) where:
		* a=b means that Jacobi(a|n) = Jacobi(b|n)
		* Jacobi(ab|n)=Jacobi(a|n)*Jacobi(b|n)
		* Jacobi(a|n) [=0 if a=0 (mod p)][=1 if a=x^2 (mod p) for some x][ =-1 if no x]
        * Other tests: Baillie-PSW, Quadratic Frobenius, Miller-Rabins, Agrawal–Kayal–Saxena, Adleman–Pomerance–Rumely
        * Gaussian Primes
            * base prime is 1+i
            * if a!=0, b!=0, prime(a^2+b^2) and (a^2+b^2)%2=1 then a+bi is prime
            * if b=0, prime(abs(a))=true, abs(a)%4=3 then a is prime
            * if one of [±(x+yi) ±(y+xi) ±(x-yi) ±(y-xi)] is prime, all others are prime (for x>y>0)
        * Eulerian Primes (a+bi)
            * base prime is 2+w
            * if a!=0, b!=0, prime(a^2+ab+b^2) and (a^2+ab+b^2)%3=1 then a+bw is prime
            * if b=0, prime(abs(a))=true, abs(a)%3=2 then a is prime
            * if one of [±(x+yw) ±(x+(x-y)w) ±((x-y)+xw) ±(x-yw) ±(x-(x-y)w) ±((x-y)-xw)] is prime, all others are prime (for 2x>y>0)
    * IEEE754 compatible Floating point arithmetic (for n>1, having one sign bit):
        * 2^(2n) bit float! contains n^2+n-1 exponent bits and 2^(2n)-n^2-n fraction bits
        * 2^(2n+1) bit float! contains n^2+2n exponent bits and 2^(2n+1)-n^2-2n-1 fraction bits
        * 2^(2n) bit decimal! contains round((2^(2n)-n^2-n)*ln2/ln10) fraction bits
        * 2^(2n+1) bit decimal! contains round((2^(2n+1)-n^2-2n-1)*ln2/ln10) fraction bits
        * 2^(2n) bit dozenal! contains round((2^(2n)-n^2-n)*ln2/ln12) fraction bits
        * 2^(2n+1) bit dozenal! contains round((2^(2n+1)-n^2-2n-1)*ln2/ln12) fraction bits
    * Encoding Methods (10^3~10bits) (12^5~18bits)
        * [Binary Integer Decimal](https://en.wikipedia.org/wiki/Binary_Integer_Decimal)
        * [Densely Packed Decimal](https://en.wikipedia.org/wiki/Densely_packed_decimal)
        * Binary Integer Dozenal
        * Densely Packed Dozenal (in development)
    * Extended precision Floating Point arithmetic
        * For 10*(2^(n-1)) bit floating point, there are 2^n exponent bits
        * For 12*(2^(n-1)) bit floating point, there are 2^n exponent bits
* Hyper-operations
    * Hyper(n,a,b) could be represented by "a[n]b" a.k.a Square bracket notation
    * Hyper(n,a,b)=Hyper(n-1,a,Hyper(n,a,b-1))
    * Goldstein function if n=0 then G=b+1, if b=0 (n=1 then G=a) (n=2 then G=0) (n>2 then G=1)
    * Ackermann function if n=0 then G=a+b, if b=0 (n=1 then G=0) (n=2 then G=1) (n>2 then G=a)
    * Clenshaw function if n=0 then G=b+1, if (b=0 & n>0) then G=0
    * Lower function
        * Lower(n,a,b)=Lower(n-1,Lower(n,a,b-1),a)
        * if n=1 then G=a+b, if (n=2 & b=0) then G=0, if (n>2 7 b=1) then G=a
    * Conway chained arrow notation
        * a→b = a^b and X, are sub-chains
        * X→1→Y = X (since 1→Y = 1)
        * X→(p+1)→(q+1) = X→(X→p→(q+1))→q
        * 2→2→Y = 4, X→2→2 = X→(X)

# List of potential Computer Algebra System collabs
* Cadabra CoCoA-5 GAP Macaulay2 MathHandbook Mathics (Mathematic) Maxima OpenAxiom PARI/GP Sage SINGULAR SyMAT SymbolicC++ Symbolism CSymPy/Symengine Xcas/Giac Yacas

# Applied Mathematics (some basics for testing):
* Classical Cryptography functions
    * Affine/Caesar (ax+b mod c translated to character table, autokey and vigenere options)
    * Transposition (square, rectangle, triangle, hexagon, zigzag, irregular, custom)
    * Square-based (Playfair, Double-Playfair/Twosquare, Foursquare mod c^2)
    * Matrix-based ciphers (addition, multiplication and transposition mod c)
    * Table (ADFGVX, Bifid, Trifid, N-dimensional-Polybius-Squares)
    * Straddling-Checkerboard (CT, VIC, SECOM, Snowfall)
        * [Full German collection of checkerboards](http://scz.bplaced.net/m.html)
        * [Scattered checkerboard variations](http://users.telenet.be/d.rijmenants/en/table.htm)
        * How to create a set to conversion tables
    * Historical (BATCO, Slidex, Floradora)
        * [Jefferson disk] (https://en.wikipedia.org/wiki/Jefferson_disk)
        * [Rasterschlüssel-44](http://users.telenet.be/d.rijmenants/en/rasterschlussel44.htm)
        * [FBI Nazi code](http://www.slugsite.com/archives/957)
        * [Schultz code](https://cryptome.org/2015/09/deniability-cipher.pdf)
        * [Reihenschieber](http://jproc.ca/crypto/reihenschieber.html)
        * [Reservehandverfahren](http://www.enigma.hoerenberg.com/index.php?cat=Reservehandverfahren)
    * Huge data (Book/Bible, Dictionary, Telegraph-Codebook, One-Time-Pad) 
        * https://users.telenet.be/d.rijmenants/en/onetimepad.htm
        * https://users.telenet.be/d.rijmenants/en/otp.htm
    * [Playing cards ciphers](https://pthree.org/2014/09/15/playing-card-ciphers/)
    * [GRC-OTG](https://atechblogger.wordpress.com/2011/08/27/a-suggestion-for-enhancing-steve-gibsons-off-the-grid-secure-password-generator/)
    * [Sniper cipher](https://www.sniperflashcards.com/cryptography.php)
    * https://practicalcryptography.com/ciphers/classical-era/
    * https://www.quadibloc.com/crypto/intro.htm
    * [Cipher machines emulation](https://users.telenet.be/d.rijmenants/)
    * [American Cryptogram Association List](https://cryptogram.org/cipher_types.html)
    * http://www.quadibloc.com/crypto/jscrypt.htm ???
    * [Cipher machine database](https://ciphermachines.com/index)
    * [Terrorist Cipher Article](https://www.schneier.com/blog/archives/2014/05/new_al_qaeda_en_1.html)
    * [Terrorist Cipher article](https://www.schneier.com/blog/archives/2014/08/the_security_of_9.html)
    * [Cipher Tool Meta-lists](https://www.winterdragon.org/index.php?page=ciphers)
    * [Cipher list](http://www.groundline.net/ARGarbage/)
* Modern Cryptography and Error Correction
    * Error Correction: Extended Hamming, Extended Binary Golay and Reed–Solomon
    * Shamir's Secret Sharing Scheme or SSSS
    * Bitshift Operations: Arithmetic, Logical, Rotate (without carry AND through carry)
    * Absolute Bitwise Operations: And, Or, Xor, Not-Implies
    * Limited Bitwise Operations: Not, Nand, Nor, Xnor, Implies
    * Flow Diagram
        * addition mod(2^n) is squared plus, subtraction mod(2^n) is squared minus
        * multiplication mod(2^n+1) is circled dot, xor is circled plus
        * Bitshift Operations and Not
    * Cipher parameters
        * Nano-ciphers (key=128 block=64)
            * TEA, XTEA and XXTEA (medium priority)
            * IDEA and IDEA-NXT-128 (medium priority)
            * MISTY1 (medium priority)
            * Hierocrypt-L1 (medium priority)
            * CS-Cipher (low priority)
            * KHAZAD (low priority)
            * Nimbus (low priority)
            * CIPHERUNICORN-E (low priority)
        * Micro-cipher (key=128 block=128)
            * SEED (medium priority)
            * Grand Cru (low priority)
            * Noekeon (low priority)
        * AES-cipher (key=128/192/256 block=128)
            * AES, Camellia, Serpent and Twofish (high priority)
            * ARIA (medium priority)
            * RC6 (medium prioirity)
            * Hierocrypt-3 (medium priority)
            * SC2000 (medium priority)
            * Q (low priority)
            * CIPHERUNICORN-A (low priority)
            * CLEFIA (low priority)
        * variable parameter
            * IDEA-NXT-256 (medium priority)
            * Blowfish (medium priority)
            * CAST-128, CAST-256 (medium priority)
            * RC2, RC5 (medium priority)
            * SAFER+, SAFER++ (medium priority)
            * SHACAL (low priority)
            * NUSH (low priority)
            * Anubis (low priority)
        * static parameter
            * 3DES (key=168 block=64) (medium priority)
            * Skipjack (key=80 block=64) (medium priority)
    * Key derivation: Crypt(C) < PBKDF2 < bcrypt < scrypt (safest)
* Extra Curriculum Mathematics
    * Non-transitive Dice: Miwin's Dice, Efron's Dice, Grime's Dice, 3-Player Dice
    * Shortest Unit Fraction Algorithm (see https://www.ics.uci.edu/~eppstein/numth/egypt/intro.html)
    * Conway's audioactive sequence and elements analysis
    * Dice! Function for https://wiki.roll20.net/Dice_Reference
* Lottery related Algorithms
    * Multicombination or multisubset or M(n,k)=C(n+k-1,k)
    * Bernoulli trial (probability of k successes out on n) = C(n,k)(p^k)((1-p)^(n-k))
    * Lotto(Ball,Pick,Win)=C(P,W)*C(B-P,P-W)/C(B,P)
    * co-lexicographic ordering of the combinations, for lottery tickets (see https://computationalcombinatorics.wordpress.com/2012/09/10/ranking-and-unranking-of-combinations-and-permutations/ and https://www.jjj.de/fxt/fxtbook.pdf)
* Random Number Generators
    * CSPRNG like Fortuna/Yarrow, ISSAC, NIST SP 800-90A, Dual_EC_DRBG, CryptGenRandom
    * Mersenne Twisters (MT)
    * Well Equidistributed Long-period Linear (WELL)
    * Single-instruction-multiple-data-oriented Fast Mersenne Twister (SFMT)
    * [XorShift](https://xorshift.di.unimi.it/) and [XSAdd](https://www.math.sci.hiroshima-u.ac.jp/~m-mat/MT/XSADD/)
    * [Xorgens, Ranut and Rannw](https://maths-people.anu.edu.au/~brent/random.html) and [SplitMix and DotMix](https://dl.acm.org/citation.cfm?doid=2714064.2660195)
    * Blum Blum Shub, , Blum–Micali, Lehmer, Naor-Reingold, Generalized Inversive Congruential
    * Wichmann-Hill like RNGs based on LCG, ICG, MWC, CMWC and LFG
    * Others: Self Shrinking Generator and Shrinking Generator, Feedback with Carry Shift Registers
    * NIST, TestU01 and DieHarder for testing out binary sequences

# Time:
* Traditional: 1 day is 24 hours, 1 hour is 4 quarters, 1 quarter is 15 minutes, 1 minute is 60 seconds
* Metric Time
    * Decimal: 1 day has 10 C-hours, 1 C-hour has 100 C-minutes, 1 C-minute has 100 C-seconds
    * Dozenal: 1 day has 12 Z-hours, 1 Z-hour has 144 Z-minutes, 1 Z-minute has 144 Z-seconds
    * Hexadecimal: 1 day has 16 X-hours, 1 X-hour has 256 X-minutes, 1 X-minute has 256 X-seconds
* Chinese Time:
    * Old Chinese: 1 day is 10 old-shi, 1 old-shi is 10 big-ke, 1 big-ke is 6 small-ke
    * New Chinese: 1 day is 12 new-shi, 1 new-shi is 5 dian, 1 dian is 10 small-ke
    * 1 dian is 2 mid-ke, 1 small-ke is 10 fen, 1 fen is 100 hao
* New Earth: 1 day is 360 degrees, 1 degree is 60 minutes, 1 minute is 60 seconds
* Leap Second mechanism: Default style (direct time change), Google Style (1 second spread out through 100 days)

# Dates ("calendar reformation"):
* 2001 January 1st being first day (and Monday) of the 400 year cycle
* 52 week calendars
    * World Season Calendar: a year is divided into 4 seasons, each containing 13 weeks, with an additional "world day" and the "leap day" added 4-100-400 style
    * Thirteen Month Calendar: a year is divided into 13 months, each containing 4 weeks, with an addition "world day" and the "leap day" added 4-100-400 style
* Leap Week: a year is divided into 52 weeks, and the "leap week" added 5-40-400 style or a much closer style
* Days and months naming system: Positivist, Tranquility, French Republican, NUCal, Common-Civil-Calendar-and-Time
* The Milanković Calendar sync: every 3600 years, there will be no leap day.
    * The Milanković Calendar adds 218 leap days in 900 years (Having one less day then 3600 Gregorian years)
* Special notation for calendar formatting
    * Days: 
        * d means without 0 place holder, dd means in 2 digits
        * ddd means 3 letter abbreviation of weekday/dayname
        * dddd means long form of weekday/dayname, 
        * ddddd means "special abbreviation" format 
    * Weeks: 
        * w means without 0 place holder, ww means in 2 digits
        * www means in 3 letter abbreviation, wwww means in long form
        * wwwww means in "special abbreviation" format
    * Months: 
        * m means without 0 place holder, mm means in 2 digits
        * mmm means in 3 letter abbreviation, mmmm means in long form
        * mmmmm means "special abbreviation" format
    * Years: 
        * y means without 0 place holder, yy means in 2 digits
        * yyy means in 3 digits, yyyy means in 4 digits (full format)
        * yyyyy means in "Cycle-Year" format

# Binary encoding
* Binary to text (for transferring data through printed texts and writings on paper):
    * Small 2^n: binary, quaternary, octal
    * Large 2^n: hexadecimal, base32, base64
    * Non-2^n based encoding: base85, base91
* Binary to mnemonic (for transferring data through phone call and voice recordings):
    * [Bubble Babble](https://github.com/rsaarelm/teratogen/tree/master/src/teratogen/babble)
    * [Vorud](https://github.com/rsaarelm/vorud)
    * [S/KEY (P.18-23)](https://tools.ietf.org/html/rfc2289)
    * [Pretty Good Privacy wordlist (a.k.a biometric word list)](https://en.wikipedia.org/wiki/PGP_word_list)
    * [Bitcoin Improvement Proposal 39 wordlist](https://github.com/bitcoin/bips/tree/master/bip-0039)
    * [Electrum 2 wordlist](https://github.com/spesmilo/electrum/tree/master/lib/wordlist)
    * [Mnemonicode](https://github.com/DonaldTsang/Mnemonicode/)
    * [Wordlist development](http://bitcoin-development.narkive.com/HrfJs1XS/bitcoin-development-bip39-word-list)


# Considering (will be removed if redundant):
* Akan Calendar, Armenian Calendar, Aztec Calendar, Attic Calendar, Babylonian Calendar, Balinese Saka Calendar, Berber Calendar, Borana Calendar, Buddhist calendar, Burmese Calendar, Chinese Calendar, Chula Sakarat, Coligny Calendar, Coptic Calendar, Egyptian Calendar, Enoch calendar, Ethiopian Calendar, Gaelic Calendar, Hellenic Calendar, Hindu Calendar, Igbo Calendar, Iranian Calendar, Japanese Calendar, Javanese Calendar, Korean Calendar, Kurdish Calendar, Mayan Calendar, Macedonian Calendar, Mongolian Calendar, Pawukon Calendar, Rapa Nui Calendar, Roman calendar, Soviet Calendar, Tabular Islamic Calendar, Tamil Calendar, Thai Calendar, Tibetan Calendar, Vikram Samvat, Vira Nirvana Samvat, Yoruba Calendar, Zoroastrian Calendar, (Dreamspell), (Discordian)
* Criteria of calendar inclusion:
* Not a renamed version of Gregorian or Julian Calendar.
* All calendar system with the same properties shall be grouped together (e.g. all calendars with clustered 31 day months will be treated as the same calendar).
* Non-365 day calendars, calendars with non-Gregorian leap days and calendars with months more than 13 will not be excluded.
* Resources
* https://myweb.ecu.edu/mccartyr/calendar-reform.html
* https://individual.utoronto.ca/kalendis/leap/index.htm
* https://individual.utoronto.ca/kalendis/kalendis.htm
* https://www.calendarzone.com/Reform/

# Compression
* https://mattmahoney.net/dc/text.html
* https://mattmahoney.net/dc/mingw.html
* https://mattmahoney.net/dc/silesia.html
* https://mattmahoney.net/dc/zpaq.html
* https://mattmahoney.net/dc/#barf

* https://dlmf.nist.gov/

* Test to check large primes:
    * Lehmer-Power’s Continued Fraction (CFrac)
    * John Pollard’s Monte Carlo Method (RHO)
    * Hendrick Lestra’s Elliptic Curve Method (ECM)
    * Carl Pomermco’s Quadratic Sieve (QS)
    * General Public’s Number Field Sieve (NFS)
* Other methods
    * Dixon’s Factorization Method
    * Euler’s Factorization Method
    * Fermat’s Factorization Method
    * Shank’s Square Forms
    * Trial Division