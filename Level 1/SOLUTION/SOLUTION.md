## First letter - the array

```
LX CXXVI LX XXXVIII XXXVIII LXII LXXXIX LIX XL CXVI LXXIII LX LIX XCV LXXXV XCI XLIX LIX XLV XXXVI LXX XCIII LX XCVI LXXXVI XCI CXVII LX LXV LV CXIV XCIV LIX LXX LXXIX LXXXVI LXXXIX LX XLIV LXII XLVII CX LIX XCVII CVI XLVII XCVII LIX LXXII LV CIII LV LX LXXIII XLVI XCII LVI LIX XLV XXXVI XLVI C LIX LXXII LXIII LXVII LXXXVIII LIX LXXII LV XCI XXXIX LIX XLIV CXII LXXXVIII CII LX XLIV LIV LXXI LXII LIX XCVII CVII LXXXVIII CX LIX LXVIII LXVII CXII LXI LIX XL CXVI LXXIII LI LIX LXXII XLV LXXIX CXI LX XCVI LXXVII XXXIV LXXVII LVII CVIII XXXIV LVII LI LVIII XLVII CXVI XLVI LXXII LX XXXVII CXIV LVI CX LIX XCIX LXXX LXXX XCII LX XCIV CIII XXXIV LXXV LVIII CII LXXV XLIV LI CXXVI LXII
```
We begin by converting the roman numerals to something more useful.
I created a GoLang script that converts them to bytes and save them into a file.

```golang
package main

import (
	"github.com/StefanSchroeder/Golang-Roman"
	"io/ioutil"
	"strings"
)

func main()  {
	inBytes, _ := ioutil.ReadFile("letter1.txt")
	var inTextNums = strings.Split(string(inBytes), " ")

	var decryptedBytes []byte
	for _, num := range inTextNums {
		decryptedBytes = append(decryptedBytes, byte(roman.Arabic(num)))
	}

	ioutil.WriteFile("letter1_decoded.txt", decryptedBytes, 0644)
}
```

Reading `letter1_decoded.txt`, we get:
```
<~<&&>Y;(tI<;_U[1;-$F]<`V[u<A7r^;FOVY<,>/n;aj/a;H7g7<I.\8;-$.d;H?CX;H7[';,pXf<,6G>;akXn;DCp=;(tI3;H-Oo<`M"M9l"93:/t.H<%r8n;cPP\<^g"K:fK,3~>
```

By the looks of it, this is not random data. It starts and ends in a structured way (`<~` and `~>`), but without further hints, we move on to the second letter.

## Second letter - the instructions
```
I) Command LXXXV archers.
II) Build LXIV tents.
III) Light up XXXII fires.
IV) Send XLVII horses to Caesar.
```

Converting the numerals to integers we get:

```
I) Command 85 archers.
II) Build 64 tents.
III) Light up 32 fires.
IV) Send 47 horses to Caesar.
```

**85, 64, 32** - these numbers hint text encodings, namely: **ASCII85, Base64** and **Base32**. Our data from the first letter contains illegal characters for both Base64 and Base32, so there is only one possible encoding left. Indeed, `<~` and `~>` are also identifiers for the ASCII85 encoding.

#### ASCII85 decoded ([link](https://www.tools4noobs.com/online_tools/ascii85_decode/)):
```
T1pBQ0FSS0FFQVNFVVFaU0dSREVJTlJBSEFaRUtOUzVFQVFURVJDRUpCQUVHTkpBSEpDR1NJQ0FJRTRUVU5KMkdJNUQ2T0pXSU1aVEU9PT0=
```

I used the analysis framework [CyberChef](https://gchq.github.io/CyberChef/) for the rest of the decodings. It allows for multiple actions (recipes) to be easily chained together.

#### Base64 decoded:
```
OZACARKAEASEUQZSGRDEINRAHAZEKNS5EAQTERCEJBAEGNJAHJCGSICAIE4TUNJ2GI5D6OJWIMZTE===
```

#### Base32 decoded:
```
v@ E@ $JC24FD6 82E6] !2DDH@C5 :Di @A9:5:2:?96C32
```

We now have only one hint left. The fourth 'order' says **47**, and the word **Caesar** rings a bell. Surely, it must be a **Caesar (ROT) cipher** with a shift of **47**.

#### ROT 47 decoded (make sure to include symbols/special characters):
```
Go to Syracuse gate. Password is: ophidiainherba
```

Finally, we get the password: `ophidiainherba`
