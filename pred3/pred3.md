# PRED 3
## predznak velicinski zapis
$$ V(b) = (-1)^{b_{n-1}} \sum_{i=0}^{n-2} b_i2^i$$

prvi bit predstavlja predznak, ostali velikost
ima 2 nicli

## zapis z odmikom
$$ V(b) = \sum_{i=0}^{n-2} b_i2^i - 2^{n-1}$$
odmik je obicajno $2^{n-1}$
nekoc priljubljen zapis
pri + je treba odmik odsteti za - pa pristeti

**TLDR: predvoris dec v bin in odstejes odmik**

## eniski komplement

$$
    V(b) = \sum_{i=0}^{n-1}b_i2^i - b_{n-1}(2^n - 1)
$$
$b_i$ je predznak
pozitivna stevila enako kot pri PV
neg st dobimo iz poz z invertiranjem vseh bitov $\equiv 2^n - 1$ (same enice)
predznaka ni treba posebaj obravnavati :)
2 nicli, pri prenosu z MSB je treba na LSB pristeti 1 (EAC)

## zapis predznacenih stevil
- vec nacinov
- v vseh primerih imam n bitno stevilo $b_{n-1}..b_2b_1b_0$



## problemi pri ukljucitvi aritmewtike v rac sys
preliv in prenos
2 resitvi
- poenastavitev posebnega bita
- sporyitev pasti )nek bit lahko doloca ali se sproyi ali se ignorira
- produkt dveh stevil je shranjen v sprenlivki enake vrednosti kot stevil 
mnozenje in div sta zahtevnejsi operaciji
2 resitvi
- ukazi korak mnozenja

## FP zapis
obseg stevil v fiksni vejici za dolocene probleme je premajhen
- potrebovali bi zelo velika ali zelo majhna stevila
- znanstvena notacija omogoca krajsi zapis
- npr. $1*10^{18}$ namesto 1000000000000000000
- st. lahko zapisemo kot $m \times r^e$
- m je mantisa in r je baza 
- ``s sšre,omkamke, exšpmemta vejica plava vzdovzs mantise``
- s spreminjanjem exponenta vejica plava vzdolz mantise levo in desno -> ime fp
- vsako st lahko v fp zapisemo na vec nacinov
- Nekdaj je vsak prozivajalec uporabljal svoj format zapisa v fp
- isti program je lahko na razlicnih racunalnikih dajal razlicne rezulatate

### Enojna natancnost
enojna natancnost (single presision) 32B
<table>
<tr><td>31</td><td>30-23</td><td>22-0</td></tr>
<tr><td>S</td><td>E</tad><td>m</td></tr>
</table>

- predznak S (0 +; 1; -)
- 8b exponent z odmikon 127
- 23b mantisa m (7-mestna dec natancnsot)
- normalizirana vrednost
- obseg $\pm 1,18*10^{-38}, \pm 3,40 *10^{38}$ (v norm obliki)
- aka float


## dvojna natancnost
<table>
<tr><td>63</td><td>62-52</td><td>51-0</td></tr>
<tr><td>S</td><td>E</tad><td>m</td></tr>
</table>


11b exponent e z odmikom 1023
52b mantisa m (16b-mestna dec natancnost)
normaliziran vrednost: $(-1)^5 * 1m, m*2^{E-1023}$
obseg: $\pm 2,22*10^{-308}; \pm 1,80 *10^{308}$ (v norm. obliki)

primer st2:
$2 = +1.0 * 2^1$
$S = 0, m = 0; e = 1$
enojna: $E = 3 + 127 = 128 = 10000000$
dvojna: $E = e + 1023 = 1024 = 1000000010$

### addition v fp
- Sestevanje in odstrevanje v fp
  - prvo st