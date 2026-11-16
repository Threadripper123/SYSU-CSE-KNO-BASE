# ch02 Number Systems, Operations, and Codes-图集

> 主题库：数字电路基础　｜　源文件：ch02 Number Systems, Operations, and Codes-图集.pdf　｜　7,987 字符　｜　43 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTEROUTLINE 2-1 DecimalNumbers  2-2 Binary Numbers 2- 3 Decimal-to-BinaryConversion Binary Arithmetic - ComplementsofBinaryNumbers -6 Signed Numbers 2-7 Arithmetic Operations with Signed Numbers 2-8 HexadecimalNumbers 2-9 Octal Numbers 2-10 BinaryCoded Decimal(BCD) 2-11 DigitalCodes 2-12  ErrorCodes

KEYTERMS Key terms are in order of appearance in the chapter. LSB BCD MSB Alphanumeric Byte ASCll Floating-point number Parity Hexadecimal Cyclicredundancy Octal check(CRC)

Thedigit 2 hasa weight of Thecigit3 has a weieht 10in this position. ofl in his position. 2 × 10 + 3 × 1 20 3

TABLE 2-1 Number Binary Number 0 0 0 0 0 1 0 0 0 1

1 0 1 2 0 4 0 0 0 3 0 0 1 0 0 5 0 0 1 6 0 0

1 0 1 1 1 8 0 0 0 9 0 0 1 10 0  0 11 0 1 12  0 0

13 14 0 1 0 15 1 1

Ball count 1st 2nd 3rd 4th 5th 6th 7th 8th 9th [Ya 2ndball Counter  1 2 9thball Decoder

TABLE 2-2 Binary weights. PositivePowersofTwo NegativePowersofTwo WholeNumbers (FractionalNumber 28 27 26 25 24 23 22 21 20 2-1 2-2 2-3 2-4 2-5 2-6 256 128 64 32 16 8 4 2 1 1/2 1/4 1/8 1/16 1/32 1/64 0.5 0.25 0.125 0.625 0.03125 0.015625

Convertthebinarywholenumber1101101todecimal. Determine the weight of each bit that is a 1, and then find the sum of the weights to get Weight: 26252423222120 Binarynumber: 1101 101 1101101=2+25+23+22+20 =64+32+8+4+1=109

Canvertthefractionalbinarynumber0.1olltodecimal Determine the weight of each bit that is a 1, and then sum the weights to get the decimal fraction. Weight: 2-1 2-2 2-3 2-4 Binary number: 0.1 0 1  0.1011=2-1+2-3+2-4 = 0.5 + 0.125 + 0.0625 = 0.6875

Remainder 3 3 2 Stolo wdhēn the 11 00 wiole-number euotientis0. ISB LSB

Caiy .0101 0.3125 × 2 = 0.625 0 0.625 ×2=1.25 0.25×2 =0.50 0.50×2=1.00 1 Continve to thedeshed-amnbei ofdeoimalpaces aastopwhen the actionalpaitisall zeros

Left cohinan. Middlecclhinnn: whenalisbonosed Boiro lfrom extcolnn aoisleft,soo - 0 = O to theleft,makingaloin this8o1i n: ren-10-2 =1. YO1 Pigh0COl h -011 |-1=0

111 7 ×101 ×5 Partiau 111 35 Praducts 000 +111 100011

1 0 1 0 1 0 1 0 0 1 0 1 0 1 0 1

Negative number 1 0 1 0  0  0 1''s comp lement 0 0 0 0     Carry in (add 1) 2''s compl ement 1 0 O

Determine the decimal values of the signed binary numbers expressed in1''s complement: (a) 00010111 (b) 11101000 (a) The bits and their powers-of-two weights for the positive number are as follows: -27 26 25 24 23 22 21 20  0 0 0 1 0 1 1 1 Summing the weights where there arels 16+4+2+1=+23 (b) The bits and their powers-of-two weights for the negative number are as follows. Noticethatthenegativesignbithasaweightof —2 or —128. -27 26 25 24 23 22 21 20 1 1 1 0 1 0 0 0 Summingthe weightewhere therearels -128+64+32+8=-24 Adding 1 to the result, the final decimal number is -24+1=-23

Determine the decimal values of the signed binary numbers expressed in 2''s complement: (a) 01010110 (b) 10101010 (a) The bits and their powers-of-two weights for the positive number are as follows: -27 26 25 24 23 22 21 20 0 1 0 1 0 1 1 0 cYa 64+16+4+2=+86 (b) The bits and their powers-of-two weights for the negative number are as follows. Notice thatthe negative sign bithas a weightof —2 = —128. -27 2 25 24 23 22 21 20 1 0 1 01 0 1 0 -128+32+8+2=-86

TABLE 2-3 Decimal Binary Hexadecimal 0 0000 0 1 0001 1

2 0010 2 3 0011 3 4 0100 4 5 0101 5 6. 0110. 6

7 0111 7 8 1000 8 9 1001 9 10 1010 A 11 1011 D

11 1011 D 12 1100 C 13 1101 D 14 1110 E 15 1111 F

THexadeermnal  remaindēr 650  40.625 0.625×16=10= A 2.5- →0.5×16=8= 8 =0.125 →0.125 ×16= 2= Stopwhehwhdleminrbef 2 8 A Hexadecmalhmnber quotientis zero. 个 MsD L LsD

Hexadecimal Binary 2''s complement 2''scomplement in in binary hexadecimal Eaaple 2A 00101010 11010110 6

Subtractfrom 2''s complement Hexadecimal in hexadecimal m axim um plus 1 in hexadecimal Eaamle 2A FF-2A D5+1 D6

''scomplement 012 3 45 6 7 8 9 AB CDEF 2''sc omplement F ED C B A 9 8 7 6 5 4 3 2 1 0 plus1 in hexadecimal Example 2A 0 1 2 3 4 5 6 7 8 9 %△4 B CDEF D6 FE  D5+1 CB A 9 8 7 6 4 3 21 0

TABLE 2-4 Octal Digit 0 1 2 3 4 5 6 7 Binary 000 001 010 011 100 101 110 111

TABLE2-5 DecimalbCDconversion. DecimalDigit 0 1 2 3 4 5 6 7 8 9 BCD 0000 0001 0010 0011 0100 0101 0110 0111 1000 1001

0110 0111 67 +0101 0011 ±53 1011 1010 Both groups are invalid (> 9) 120 + 0110 + 0110 Add6 to bothgroups 0001 0010 0000 ValidBCDnumber 1 2 0

TABLE 2-6 Four-bit Gray code. Decimal Binary Gray Code Decimal Binary Gray Code 0 0000 0000 8 1000 1100 1 0001 0001 9 1001 1101 2. 0010. 0011. 10. 1010 .1

2 0010 0011 10 1010 1111 3 0011 0010 11 1011 1110 4 0100 0110 12 1100 1010 5 0101 0111 13 1101 1011 6 0110 0101 14 1110 1001 7 0111 0100 15 1111 1000

IR R emitter/detector emitter/detector  110  101 100 000  000 101 111   001 100 001 110 011 010 010 011 Reflected Nonreflected (a)Binarycode (b)Graycode

TABLE2-7 ControlCharacters GraphicSy bol Name Dec Binary Hex Symbol Dec Binary Hex Symbol Dec Binary Hex Symbol Dec Binary Hex NUL 0 0000000 00 space 32 0100000 20 @ 64 1000000 40  96 1100000 60

SOH 1 0000001 01 ! 33 0100001 21 A 65 1000001 41 a 97 1100001 61 STX 2 0000010 02  34 0100010 22 B 66 1000010 42 b 98 1100010 62 ETX 3 0000011 03 # 35 0100011 23 C 67 1000011 43 c 99 1100011 63 EOT 4 0000100 04 $ 36 0100100 24 D 68 1000100 44 d 100 1100100 64 ENQ 5 0000101 05 % 37 0100101 25 E 69 1000101 45 e 101 1100101 65 ACK 6 0000110 06 & 38 0100110 26 F 70 1000110 46 f 102 1100110 66

BEL 7 0000111 07 , 39 0100111 27 G 71 1000111 47 g 103 1100111 67 BS 8 0001000 08 ( 40 0101000 28 H 72 1001000 48 h 104 1101000 68 HT 9 0001001 09 ) 41 0101001 29 I 73 1001001 49 i 105 1101001 69 LF 10 0001010 0A * 42 0101010 2A J 74 1001010 4A j 106 1101010 6A VT 11 0001011 0B + 43 0101011 2B K 75 1001011 4B k 107 1101011 6B FF 12 0001100 0C 44 0101100 2C L 76 1001100 4C 1 108 1101100 6C , D I2 001 0T 45 0101  77 001 100

CK 15 0OO11O1 OD 43 O1011O1 ZD M1 /7 1OO11O1 4D 1M1 1O9 11O11O1 OD SO 14 0001110 0E 46 0101110 2E N 78 1001110 4E n 110 1101110 6E  SI 15 0001111 0F / 47 01O1111 2F O 79 1O01111 4F 0 111 1101111 6F DLE 16 0010000 10 0 48 0110000 30 P 80 1010000 50 P 112 1110000 70 DC1 17 0010001 11 1 49 0110001 31 Q 81 1010001 51 q 113 1110001 71 DC2 18 0010010 12 2 50 0110010 32 R 82 1010010 52 r 114 1110010 72 DC3 19 0010011 13 3 51 0110011 33 S 83 1010011 53 S 115 1110011 73

DC4 20 0010100 14 4 52 0110100 34 T 84 1010100 54 t 116 1110100 74 NAK 21 0010101 15 5 53 0110101 35 U 85 1010101 55 u 117 1110101 75 SYN 22 0010110 16 6 54 0110110 36 V 86 1010110 56 v 118 1110110 76 ETB 23 0010111 17 7 55 0110111 37 W 87 1010111 57 w 119 1110111 77 CAN 24 0011000 18 8 56 0111000 38 X 88 1011000 58 x 120 1111000 78 EM 25 0011001 19 9 57 0111001 39 Y 89 1011001 59 y 121 1111001 79

SUB 26 0011010 1A 58 0111010 3A Z 90 1011010 5A Z 122 1111010 7A ESC 27 0011011 1B , 59 0111011 3B [ 91 1011011 5B { 123 1111011 7B FS 28 0011100 1C < 60 0111100 3C \ 92 1011100 5C | 124 1111100 7C GS 29 0011101 1D = 61 0111101 3D  93 1011101 5D } 125 1111101 7D RS 30 0011110 1E > 62 0111110 3E  94 1011110 5E ~ 126 1111110 7E US 31 0011111 1F ? 63 0111111 3F 95 1011111 5F Del 127 1111111 7F

The BCD )code with paritybits. Even Parity Odd Parity P BCD P BCD 0 0000 1 0000 1 0001 0 0001 1 0010 0 0010 0 0011 1 0011 1 0100 0 0100 0 0101 1 0101 0 0110 1 0110 1 0111 0 0111 1 1000 0 1000 0 1001 1 1001

Data bitsplus Data bits plusappended bits appondled bis Send. Remainder=0 Append data bits with Divideusing Check ydatarbits1 remainder modulo-2 remainder. (initially subtraction. with x zeros). Remainder ≠0 x-bit generator eode (a)Transmitting endof communication link No errors. Data bits Process the data bits. Remainder=0 Divide using Data bitsplus appended bis Check modulo-2 remainder. subtraction. Remainder≠0 ar-bitrgenenatoiicode error(s). request retransmission. (b)Receivingend ofcommunicationlink

D'' 110100110000 G 1010 110100110000 1010 1110 1010 1000 1C10 1011 1010 1000 1010 100
