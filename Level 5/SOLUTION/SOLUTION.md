## Analysis

We begin by reading the text word by word, trying to recognize as many hints as possible.

```
XXXVI levers
```
Converted to arabic, this is **36**. Furthermore, levers have only two positions (up and down), so it is safe to assume that the answer will be a combination of **1s** and **0s**.

```
Twelve candles in restless motion
```
The **12** candles are visualized in the image. In the original challenge the candles would change color every time you open the level, thus being in `restless motion`.

```
Blood, grass and the ocean
```
Note how the words are colored in **red**, **green** and **blue** - **RGB**! This hint tells us that we will have to keep in mind the colors of those candles.

```
Dark is zero. Light is one.
```
This line can be interpreted in too many ways without more context. We will get back to it later.

```
The first is up, the second down.
```
The first _what_? Following the context, it means that the first **candle** is up, and the second down. This suggests that we may have to go **column by column** whenever we are trying to build our answer.

```
Start where the sun disappears
```
The sun disappears in the **west**!

## Putting it all together

The most important part of this challenge is connecting the candles with the already known format of the answer. We know that there are **12** candles, but the answer is supposed to be **36** bits long, or **3** times the candles. There are two ways to fill that information - either candles will repeat, or each candle contains **3** bits of information. Indeed, the latter is possible! Each candle is either a **primary** or **secondary** color, which means that we can express each candle color using only the primary colors **red**, **green** and **blue**. That is **3 bits** of information per **12** candles, which makes **36**! If we go back to the 3rd line of the riddle, it now makes sense:
```
Dark is zero. Light is one.
```
If we have black or gray - we put **0**, otherwise - we put **1**.

We create a map of the candle colors:
```
[110]-[100]-[011]-[101]-[110]-[010]
|                                 |
[000]-[000]-[101]-[100]-[000]-[111]
```

Concatenating the colors, column by column, left to right, we get:
```
110000100000011101101100110000010111
```

And this is the answer to this final level of the Avast Challenge.
