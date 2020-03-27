# Sega Saturn disc matrix font (SSDM)
if you ever have owned a japanese Sega Saturn game, some of the discs come with a fancy code label on the inner ring

### why?
due the coronavirus confinement, i had plenty of time to spare and so, tried to reverse-engineering the code, successfully must add

### how?
the code is just a simple array of bits, 64 in size, the bottom line of every grid just marks the bottom position for correct orientation and reading, the grid is numbered from 0 to 5 as follows, the code is used to simply encode each game id number, which are all unique

MATRIX GRID
`5 3 1
4 2 0
- - -`

luckyly, I had enough games (or samples) to have a clear representation of each number (minus number 8), a symbol (-), and 4 letters (P, K, G, S). It was easy to decode the format following a binary pattern and placing the known numbers on it

```543210
110000 0
110001 1
110010 2
110011 3
110100 4
110101 5
110110 6
110111 7
111000 8 (unknown but easy to figure)
111001 9```

A similar pattern was followed for the alphabet, and filling the missing glyphs was easy

```543210
000001 - A
000010 - B
000011 - C
000100 - D
000101 - E
000110 - F
000111 - G (known)
001000 - H
001001 - I
001010 - J
001011 - K (known)
001100 - L
001101 - M
001110 - N
001111 - O
010000 - P (known)
010001 - Q
010010 - R
010011 - S (known)
010100 - T
010101 - U
010110 - V
010111 - W
011000 - X
011001 - Y
011010 - Z```

The last part was a little bit tricker, the two symbols left that appears on the discs, correlates with a (-) mark, and a filled grid with all the bits 1

so if the 64 character list is true...

00 000000
01 000001 A
[...]
45 101101 - character
[...]
48 110000 0 number
[...]
63 111111 used as block separator

Then, since the code is following binary, it's obvious to turn to ASCII character set for inspiration, and sure enough:

45 101101 - char
46 101110 . char
47 101111 / char
48 110000 0 number

Which perfectly fill the gaps, while preserves the known value of the - char, so the resulting table looks like this:

00 - 000000 - @
01 - 000001 - A
02 - 000010 - B
03 - 000011 - C
04 - 000100 - D
05 - 000101 - E
06 - 000110 - F
07 - 000111 - G
08 - 001000 - H
09 - 001001 - I
10 - 001010 - J
11 - 001011 - K
12 - 001100 - L
13 - 001101 - M
14 - 001110 - N
15 - 001111 - O
16 - 010000 - P
17 - 010001 - Q
18 - 010010 - R
19 - 010011 - S
20 - 010100 - T
21 - 010101 - U
22 - 010110 - V
23 - 010111 - W
24 - 011000 - X
25 - 011001 - Y
26 - 011010 - Z
27 - 011011 - [
28 - 011100 - \
29 - 011101 - ]
30 - 011110 - ^
31 - 011111 - _
32 - 100000 - `
33 - 100001 - !
34 - 100010 - “
35 - 100011 - #
36 - 100100 - $
37 - 100101 - %
38 - 100110 - &
39 - 100111 - ‘
40 - 101000 - (
41 - 101001 - )
42 - 101010 - *
43 - 101011 - +
44 - 101100 - ,
45 - 101101 - -
46 - 101110 - .
47 - 101111 - /
48 - 110000 - 0
49 - 110001 - 1
50 - 110010 - 2
51 - 110011 - 3
52 - 110100 - 4
53 - 110101 - 5
54 - 110110 - 6
55 - 110111 - 7
56 - 111000 - 8
57 - 111001 - 9
58 - 111010 - :
59 - 111011 - ;
60 - 111100 - <
61 - 111101 - =
62 - 111110 - >
63 - 111111 - ? --> used as string delimiter

The only quirkness is that the placement of the symbols is somehow arbitrary (it doesn't match ASCII placement, but they do respect the ASCII arrangement), and that the format uses the "full" symbol 63 to mark the beginning and the end of the encoding (surely for optical data reading).

So, the final format for any give Sega Saturn CD is:

Fighters Megamix 1996
Plain Text on the CD: GS-9126P-01302-P2K
Encoded Text ??GS-9126P-01302-P2KFL?

If you have reached this point, you'll probably realize by now, as I did, that the code is not a SEGA idea, but a CD manufacturer idea.

First, because not all Sega Saturn CDs have the encoding, which is odd since SEGA hold the platform.
Second, because the format adds extra characters at the end of the string (probably manufacturing details like lot job, date, factory number, and more, which is usual for serial numbers), and that the format is totally a optical recognition code, but from 1990's where QR was not even developed.
And third, because some discs have very diferent marks and number fonts used for engraving the inner cd id number, that reveals multiple CD creation machines or plants or companies.

Probably, since this looks very high tech, even compared with cds of the same era, country of origin and destination system, the culprit of this format was the [Taiyo Yuden](https://en.wikipedia.org/wiki/Taiyo_Yuden) company, since it was known for the absolute best CD in the entire industry, but this claim is impossible to prove, and this encoding was not found on any other disc whatsover to this day.

you'll also find the [bitfontmaker2 sourceCode](https://github.com/OOQQ/Sega-Saturn-disc-matrix-font/blob/master/bitFontMaker2Source.txt) used to [generate the font](https://www.pentacom.jp/entacom/bitfontmaker2/) 

###### made by [OOQQ](https://github.com/OOQQ/)
