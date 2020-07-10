# Sega Saturn Disc Matrix font (SSDM)
<img src="https://github.com/OOQQ/ooqq.me/blob/master/blob/ssdm/ssdm.png" align="center" alt="SSDM">

-----

If you ever have owned a japanese Sega Saturn game, some of the CDs came with a fancy code on the inner ring.

### Findings
* Code is an array of 6 bits, it can create up to 64 different arrangements (characters).
* Array is numbered from 0 to 5 to adress each bit, for easier deciphering.
* Matrix seems to correlate with each game ID number, which are all unique.

THE MATRIX GRID
```
5 3 1
4 2 0
* * * optical guideline orientation 
```

Fortunately there was enough games (or samples) to have a clear representation of each number (minus number 8), a character (-), and 4 letters (P, K, G, S).
It was easy to decode the format following a binary pattern and placing the known numbers on it:
```
543210
110000 0
110001 1
110010 2
110011 3
110100 4
110101 5
110110 6
110111 7
111000 8 (unknown but easy to figure)
111001 9
```

A similar pattern was followed for the alphabet:
```
543210
000001 A
000010 B
000011 C
000100 D
000101 E
000110 F
000111 G (known)
001000 H
001001 I
001010 J
001011 K (known)
001100 L
001101 M
001110 N
001111 O
010000 P (known)
010001 Q
010010 R
010011 S (known)
010100 T
010101 U
010110 V
010111 W
011000 X
011001 Y
011010 Z
```

The last part was a little bit trickier. The two symbols left that appears on the discs matches the (-) character, and a filled grid with all the bits set to 1

```
101101 (-) char
101110 unknown
101111 unknown
110000 (0) number
```
And since the code is pretty much binary, it's obvious to turn to the [ASCII character set](https://ooqq.me/blob/charTables/) for inspiration, and sure enough:
```
101101 (-) char
101110 (.) char
101111 (/) char
110000 (0) number
```
Which perfectly fills the gaps, while preserving the known value of the (-) character, so the resulting table (following ASCII arrangement) looks like this:
```
00 000000 @ --> probably never to be used
01 000001 A
02 000010 B
03 000011 C
04 000100 D
05 000101 E
06 000110 F
07 000111 G
08 001000 H
09 001001 I
10 001010 J
11 001011 K
12 001100 L
13 001101 M
14 001110 N
15 001111 O
16 010000 P
17 010001 Q
18 010010 R
19 010011 S
20 010100 T
21 010101 U
22 010110 V
23 010111 W
24 011000 X
25 011001 Y
26 011010 Z
27 011011 [
28 011100 \
29 011101 ]
30 011110 ^
31 011111 _
32 100000 `
33 100001 !
34 100010 “
35 100011 #
36 100100 $
37 100101 %
38 100110 &
39 100111 ‘
40 101000 (
41 101001 )
42 101010 *
43 101011 +
44 101100 ,
45 101101 -
46 101110 .
47 101111 /
48 110000 0
49 110001 1
50 110010 2
51 110011 3
52 110100 4
53 110101 5
54 110110 6
55 110111 7
56 111000 8
57 111001 9
58 111010 :
59 111011 ;
60 111100 <
61 111101 =
62 111110 >
63 111111 ? --> used as string delimiter
```
The only quirk is that the placement of symbols is somehow arbitrary (it doesn't match ASCII placement, but they do respect the ASCII arrangement), and that the format uses the "filled" symbol 63 to mark the beginning and the end of the encoding (for easy optical data reading).

So for any given Sega Saturn CD which contains the encoding, now we can read everything encoded on it:
```
Fighters Megamix 1996
CD Text:        GS-9126P-01302-P2K
CD Encoded:   ??GS-9126P-01302-P2KFL?
```
If you have reached this point, you'll probably realize now that the code is not a SEGA idea, but a CD manufacturer idea because:

* Not all Sega Saturn CDs have the encoding, which is impossible since SEGA holds the platform and you needed a license from SEGA to publish a game.
* The format adds extra characters at the end of the string (probably manufacturing details like lot job, date, factory number, and more, which is usual for serial numbers), and that the format is a optical recognition code, completely unnecesary for commercialization.
* Some discs have very diferent marks and number fonts used for CD manufacturing engraving, so it reveals multiple CD creation machines, plants, or companies involved.

Upon further delving into CD making process, marks on the CD are part of the [SID](https://support.discogs.com/hc/en-us/articles/360005006654-Database-Guidelines-6-Format#CD_Matrix) from the [IFPI commitee](https://www.ifpi.org/content/library/sid-code-implementation-guide.pdf), so the mastering IFPI L237 that's tied with the press code of IFPI40XX on the Sega Saturn CDs, correlates to a Japanese [JVC](http://wiki.musik-sammler.de/index.php?title=Diskussion:Herstellungsland_(CDs_/_DVDs)) manufacturing plant. So it's pretty safe to also assume that any game published by Victor on the Sega Saturn does contain the encoding, as well as any game manufactured on that CD factory.

The only question that remains is in which machine did they created these masters, since this format never appeared again on any other CD, not even from the same manufacturer, not even on the same game machine, country of origin, or at any other date. So it appears it was an exclusive method for the Sega Saturn software discs, a format that died as soon as the machine that used those discs, died.

You'll also find the [bitfontmaker2 sourceCode](https://github.com/voodoocoltd/Sega-Saturn-Disc-Matrix-font/blob/master/bitFontMaker2Source.txt) used to [generate the font](https://www.pentacom.jp/entacom/bitfontmaker2/) 

###### made by [OOQQ](https://github.com/OOQQ/)
