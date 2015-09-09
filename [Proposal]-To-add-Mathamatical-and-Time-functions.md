# Mathematics (for scientific calculators):
* Trigonometric functions: sin, cos, tan, asin, acos, atan
* Hyperbolic function: sinh, cosh, tanh, asinh, acosh, atanh
* Logarithms: log (for decimal), lg (for binary), lon (for duodecimal/dozenal), ln (for natural)
* Quotient and Modulo in multiple ways: Truncated, Floored, Eucliedian
* Modulo 1: FRACtion (fractional part), SAWtooth (y minus floor of y)
* Rounding: Ceiling, Floor, Truncate, Round Half up, Round Half down, Round half zero
* Factorial or n!=n*(n-1)! Permutation or P(n,r)=n!/(n-r)! Combination or C(n,r)=P(n,r)/r!
* Arbitrary precision arithmatic (in bits, base10, base12, base16 and base60)
* Complex number arithmatic (i^2=-1 "Gaussian integers" and w^3=1 "Eulerian integers")
* Number data types: integers(int_A), fractions (int_A / int_B), floating-point (frac_A * exp_A)

# Applied Mathematics (some basics for testing):
* Classical Cryptography functions
    * Affine/Caesar (ax+b mod c translated to character table, autokey and vigenere options)
    * Transposition (square, rectangle, triangle, hexagon, zigzag, irregular, custom)
    * Square-based (Off-The-Grid, Playfair, Double-Playfair/Twosquare, Foursquare mod c^2)
    * Matrix-based ciphers (addition, multiplication and transposition mod c)
    * Table (ADFGVX, Bifid, Trifid, N-dimensional-Polybius-Squares, Straddling-Checkerboard)
    * Historical (BATCO, Slidex, Rasterschlüssel-44, Jefferson, Reihenschieber, Reservehandverfahren)
    * Huge data (Book/Bible, Dictionary, Telegraph-Codebook, One-Time-Pad) 
    * Playing cards (Solitare, https://pthree.org/2014/09/15/playing-card-ciphers/)
* Modern Cryptography and Error Correction
    * Error Correction: Extended Hamming, Extended Binary Golay and Reed–Solomon
    * Shamir's Secret Sharing Scheme
    * 3DES, AES, serpent, blowfish and twofish in multiple modes of operation
* Extra Curriculum Mathematics
    * Non-transitive Dice: Miwin's Dice, Efron's Dice, Grime's Dice, 3-Player Dice
    * Shortest Unit Fraction Algorithm (see https://www.ics.uci.edu/~eppstein/numth/egypt/intro.html)
    * Conway's audioactive sequence and elements analysis
* Lottery related Algorithms
    * Multicombination or multisubset or M(n,k)=C(n+k-1,k)
    * Bernoulli trial (probability of k successes out on n) = C(n,k)*p^k*(1-p)^(n-k)
    * Lotto(Ball,Pick,Win)=C(P,W)*C(B-P,P-W)/C(B,P)=(P!(B-P)!(P-W)!)^2/(W!B!(B-2P+W)!)
    * co-lexicographic ordering of the combinations, for lottery tickets (see https://computationalcombinatorics.wordpress.com/2012/09/10/ranking-and-unranking-of-combinations-and-permutations/ and http://www.jjj.de/fxt/fxtbook.pdf)

# Time ("time unit reformation"):
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

# Dates ("calendar reformation") with 2001 Janurary 1st being first day of the 400 year cycle:
* 52 week calendars
    * World Season Calendar: a year is divided into 4 seasons, each containing 13 weeks, with an additional "world day" and the "leap day" added 4-100-400 style
    * Thirteen Month Calendar: a year is divided into 13 months, each containing 4 weeks, with an addition "world day" and the "leap day" added 4-100-400 style
* Leap Week: a year is divided into 52 weeks, and the "leap week" added 5-40-400 style or a much closer style
* Days and months naming system: Positivist, Tranquility, French Republican, NUCal, Common-Civil-Calendar-and-Time
* The Milanković Calendar sync: every 3600 years, there will be no leap day.
    * The Milanković Calendar adds 218 leap days in 900 years (Having one less day then 3600 Gregorian years)

# Binary encoding (for transferring data through texts or phone calls):
* Binary to text (paper): binary, quaternary, octal, hexadecimal, base32, base64, base85, base91
* Binary to mnemonic (phone): S/KEY, bubble babble, Pretty Good Privacy wordlist (a.k.a biometric word list) and Bitcoin Improvement Proposal 39 wordlist (a.k.a. BIP39 wordlist, Electrum 2 wordlist)

# Considering (will be removed if redundant):
* Akan Calendar, Armenian Calendar, Aztec Calendar, Attic Calendar, Babylonian Calendar, Balinese Saka Calendar, Berber Calendar, Borana Calendar, Buddhist calendar, Burmese Calendar, Chinese Calendar, Chula Sakarat, Coligny Calendar, Coptic Calendar, Egyptian Calendar, Enoch calendar, Ethiopian Calendar, Gaelic Calendar, Hellenic Calendar, Hindu Calendar, Igbo Calendar, Iranian Calendar, Japanese Calendar, Javanese Calendar, Korean Calendar, Kurdish Calendar, Mayan Calendar, Macedonian Calendar, Mongolian Calendar, Pawukon Calendar, Rapa Nui Calendar, Roman calendar, Soviet Calendar, Tabular Islamic Calendar, Tamil Calendar, Thai Calendar, Tibetan Calendar, Vikram Samvat, Vira Nirvana Samvat, Yoruba Calendar, Zoroastrian Calendar, (Dreamspell), (Discordian)
* Criteria of calendar inclusion:
* Not a renamed version of Gregorian or Julian Calendar.
* All calendar system with the same properties shall be grouped together (e.g. all calendars with clustered 31 day months will be treated as the same calendar).
* Non-365 day calendars, calendars with non-Gregorian leap days and calendars with months more than 13 will not be excluded.
* Resources
* http://myweb.ecu.edu/mccartyr/calendar-reform.html
* http://individual.utoronto.ca/kalendis/leap/index.htm
* http://individual.utoronto.ca/kalendis/kalendis.htm
* http://www.calendarzone.com/Reform/