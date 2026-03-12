## Sestevanje uints
8b
$190 + 20 = 210$

$$
\space \space \space 1011\space 1110 \\
+\space 0001 \space 0100 \\
1101 \space 0010 
$$
carry: 0

$$
\begin{matrix}
    1011 & 1110\\
   + 0100 & 0110 \\
    0000 & 0000
\end{matrix} 
$$
carry: 1;
rezultat ni previlen

## predznacena st v 2k
124
+-123

124 = 0111 1100
-123 = 0111 1011 = 1000 0100 = **1000 0101**

$$
\begin{matrix}
    0111 & 1100\\
    +1000 & 0101\\
    0000 & 0001
\end{matrix}
$$

carry = 1
rez pravilen

124+7 = 131

$$
\begin{matrix}
    0111 & 1100\\
    +0000 & 0111\\
    1000 & 0011
\end{matrix}
$$
c = 0
rezultat je pacaen

ce je rez pravilen ni prislo do overflow-a: v = 0, cene je v = 1

kako izracunati v?
najbol enostavno: v = $c_n \oplus c_{n-1}$

<table>
<tr>
    <td>OP1</td>
    <td>OP2</td>
    <td>Res</td>
    <td>v</td>
</tr>
<tr>
    <td>+</td>
    <td>-</td>
    <td>+/-</td>
    <td>0</td>
</tr>
<tr>
    <td>-</td>
    <td>+</td>
    <td>+/-</td>
    <td>0</td>
</tr>
<tr>
    <td>+</td>
    <td>+</td>
    <td>+</td>
    <td>0</td>
</tr>
<tr>
    <td>+</td>
    <td>+</td>
    <td>-</td>
    <td>1</td>
</tr>
<tr>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>0</td>
</tr>
<tr>
    <td>-</td>
    <td>-</td>
    <td>+</td>
    <td>1</td>
</tr>
</table>

## predstavitev $\R$ v racunalniku
Standard floating point

$57.25_{(10)} \xrightarrow{\text{v dvojisko}} 111001,01_{(2)} \xrightarrow{\text{normaliziramo}}(-1)^0 \cdot 1,1100101 \cdot 2^5$

norm oblika:
$$ 
(-1)^s * 1,m*2^e; m = \text{mantisa}
$$

$(-1)^0$ = poz
$(-1)^1$ = neg

### Enonjna natancnost
Single precision
32b -> float
MSB: s

$E = e+\text{odmik}$
odmik = 127
<table>
<tr>
    <td>1</td> <td>8</td> <td>23</td>
</tr>
<tr>
    <td>S</td> <td>E</td> <td>m</td>
</tr>
</table>


### Dvojna natancnost
double
64b
odmik = 1023
<table>
<tr>
    <td>1</td> <td>11</td> <td>52</td>
</tr>
<tr>
    <td>S</td> <td>E</td> <td>m</td>
</tr>
</table>


### primer
$$
-210.59375_{(10)} = -1101 \space 0010,10011_{(2)} \xrightarrow{\text{normaliziramo}}(-1)^{1} \cdot 1,101 \space 001010011 \cdot 2^7\\
s = 1; e = 7;\\
E=7+127 = 134 = 1000 \space 0110
$$

<table>
<tr><td>S</td><td>E</td><td>m</td></tr>
<tr><td>1</td><td>100 0011 0</td><td>101 0010 1001 1000...0</td></tr>
</table>

v hex: 0xC3529800;

$$
0\text{xBF5}8 \space 0000 \rightarrow 1011 \space 1111 \space 0101 \space 1000...0
$$

locimo na param:
<table>
<tr><td>S</td><td>E</td><td>m</td></tr>
<tr><td>1</td><td>011 1111 0</td><td>101 1000 ...0</td></tr>
</table>

E = -126
e = 126-127 = -1

$-1,1011 \cdot 2^{-1} = -0,11011 = -11011 \cdot 2^{-5} = 27 \cdot 2^{-5} = -0,84375_{(10)}$

### posebnosti
ce fp naleti na tak zapis je to $\pm \infin$:
<table>
<tr><td>s</td><td>1111 1111</td><td>00..0</td></tr>
</table>

rezultat nedefinirane operacije (v mantisi je vsaj 1 enka ostalo je karkoli): NaN:
<table>
<tr><td>1</td><td>1111 1111</td><td>0....1....</td></tr>
</table>

zapis nule:
$(-1)^s \cdot 0, m\cdot 2^{-126}$
<table>
<tr><td>1</td><td>0000 0000</td><td>m</td></tr>
</table>

### operacije v fp
#### sestevanje
A = 0x3F58 0000
B = 0x425C 4000
A+B = ?

damo na skupni factor: izberemo vecji factor

$$
A = 1,1011 \cdot 2^{-1} \rightarrow 0,11011 \rightarrow 0,0000011011 \cdot 2^5\\
B = 1,101110001 \cdot 2^{5}\\
0,0000011011\\
+1,101110001
1,1011111101 \cdot 2^5 = s
$$

s=0x425F A000

#### odstevanje
M = 0xABCD 0000
N = 0x4EB0 0000
P = $\text{M} \cdot \text{N}$

$$
M = - 1,1001101 \cdot 2^{-48} \\
N = + 1,011 \cdot 2^{30}\\
P = -... \cdot 2^{-48 + 30}\\
11001101 \cdot 1011
$$

mnozis isto kot normalna stevila ("tabelca")
ko dobis rezultat rabis nastavit vejico. To naredimo tko da jo vstavimo za (v tem primeru) 10 mest od leve:
$$
P = - 10,0011001111 \cdot 2^{-10}\\
$$
P = 0xBB0C F000