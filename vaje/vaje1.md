# Vaje 1
## kolokviji
### 1. kol
- predstavitev informacij in podatkov
- Assembly

### 2. kol
- use iz prvega 
- pouderek na cpu

**pogoj za orpavljanje izpita je vsaj 20% na vsakem kol**

ce sta oba usaj 60 se prizna kot racunski del izpita!

## gradiva
na ucilnici
dodatno gradivo
kvizi

## Stevila kot informacija v racunalniskih systemih
149.28125 DEC -> HEX => 95.48 hex oz. 0x95.48

$$149 / 16 = 9 \text{ ost.} 5$$
$$ 9/16 = 0 \text{ ost. }9$$

$$0.28125 * 16 = 4.5 = 0,5 + 4$$
$$0.5 * 16 = /,0 = 0,0 + 8$$

$$ n * \text{baza}^k = celo stevilo$$
$$ 149,28125 * 16^2 = 38126 = 95,48_{(16)}$$


pretvorba iz dec v bin:
$$ 1001 0101, 01001_{(2)}$$

pretvorba iz hex v dec
$$ 2F,8_{(16)} =2*16^1 + 15*16^0 + 8*16^{-1} = 47,5_{(10)}$$

<table>
    <tr><td>dec</td><td>0-9</td><td>10</td><td>11</td><td>12</td><td>13</td><td>14</td><td>15</td>
    <tr><td>HEX</td><td>0-9</td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td>
</table>

- pretvorba iz bin v dec

    $$ 0b 100010,0110 = 2^5 + 2^1 + 2^{-2} + 2^{-3} = 34 + 02.5 + 0.125 = 34. 375_{(10)}$$

### pretvora iz bin v hex
$$0b 1100 \space 1010 \space 1111, \space 1111 \space 111$$
- grupiramo po 4b in pretvorimo direktno

$$0b 1100 \space 1010 \space 1111, \space 1111 \space 111 = \text{CAF,FE}_{(16)}$$

## predstavitev podatkov v racunalniku
Recimo da imamo v 8b reg zapisano vrednsot 0100 0001. Zanima nas, kaj ta zapis predstavlja.
Lahko bi pomenilo stevilo v bin sepravi 65. Lahko je koda za char 'A'. Lahko je nekaj poljubnega.
Da si to interpretiramo rabimo **kontekst** -> **tip spremenljivke**

### unsigned numbers
- lahko predstavimo vsa ne negativna stevila
- na 8 bitih lahko to v cpp predstavimo kot:
  ```cpp
  uint8_t x = 153;
  ```
$$ 153_{(10)} \rightarrow^{\mathbf{unsigned}} 128 + 16 + 8 + 1  \rightarrow 10011001_{(2)}$$

poramo paziti, da ce imamo npr. 8b register, zapolnimo vsa mesta:
$$33_{(10)} \equiv 32 + 1 \rightarrow 10\space 0001 \rightarrow 0010 \space 0001_{(2)}$$

### signed ints
v cpp:
```cpp
int8_t y = -25;
```
v 8b reg imamo:
$$\underline{1}001 \space 1001 \rightarrow \underline{-}25_{(10)}$$ 
MSB = predznak!
$$ \underline{0}010 \space 0001 \rightarrow \underline{+}33$$

postopek deluje tudi v obratni smeri.
Ta postopek se tipicno ne rabi za zapis predznacenih stevil.

### predstavitev z odmikom:
$$ 0110 \space 0110 \xrightarrow{\text{odmik: }128} 64 +32 +4 +2 -128 = -26_{(10)}$$
$$1010 \space 0000 \xrightarrow{\text{odmik: } 128} 128+32-128 = +32_{(10)} $$

### predstavitev z dvojiskim komplementom
$$ 1110 \space 0111 \xrightarrow{2^\text{'}k} -25_{(10)}$$
ce je levi bit 1 mormo narest negacijo: flippamo bite:
$$0001 \space 1000$$
dobimo **eniski komplement**. Nato pristejemo 1 da dobimo dvojiski komplement:
$$0001 \space 1000 + 1 = 0001 \space 1001$$

$$ 0010 \space 0001 \xrightarrow{2^\text{'k}} +33 $$

#### zapis dec v 2'K
zapis v binarnem

$$-25 = 0001\space 1001 $$

obrnemo bite:
$$ 1110 \space 0110$$
pristejemo 1:
$$ 1110 \space 0110 + 1 = 1110 \space 0111$$

### primerjava vseh
<table>
<tr>
    <td></td>
    <td>min</td>
    <td>max</td>
    <td>nicla</td>
</tr>
<tr>
    <td>uint</td>
    <td>0000 0000 = 0</td>
    <td>1111 1111 = +255</td>
    <td>0000 0000</td>
</tr>
<tr>
    <td>int</td>
    <td>1111 1111 = -127</td>
    <td>0111 1111 = +127</td>
    <td>0000 0000 = +0 <br/>
        1000 0000 = -0
    </td>
</tr>
<tr>
    <td>odmik 128</td>
    <td>0000 0000 = -128</td>
    <td>1111 1111 = 127</td>
    <td>1000 0000</td>
</tr>
<tr>
    <td>2K</td>
    <td>1000 0000 = -128</td>
    <td>0111 1111 = 127</td>
    <td>0000 0000</td>
</tr>
</table>

### primer naloge:
$$ \text{stevilo: }-1080_{(10)} \xrightarrow[\text{16b}]{2^{'}k} 1024 + 32 +16 +8 = 0100 \space 0011 \space 1000 $$
naredimo kompelement

$$ 1011 \space 1100 \space 0111 + 1  =  \space 1011 \space 1100 \space 1000$$
ker je treba stevilo dati v 16 register moramo narediti **sign extention**. To naredimo tako, da prenesemo predznak na MSB in nato popolnimo preostale vrednosti z enicami:

$$\space 1011 \space 1100 \space 1000 = 1111 \space 1011 \space 1100 \space 1000$$

za konec se pretvorimo v hex: 0xFFFF F B C 8