We begin by checking the file type:
```
→  file wooden_crate
wooden_crate: gzip compressed data, was "smaller_box", last modified: Mon Jan 15 23:20:09 2018, max compression, from FAT filesystem (MS-DOS, OS/2, NT)
```

Next, we extract the archive. Note that `gunzip` cares about the extension, so we append one:
```
→  mv wooden_crate wooden_crate.gz
→  gunzip wooden_crate.gz
```

We check the new file:
```
→  file wooden_crate
wooden_crate: 7-zip archive data, version 0.4
```

A password-protected **7-ZIP** archive.
There are three possibilities: the password is supposed to be bruteforced, it's hidden in one of the two archives, or it's given as a hint.
We try the easiest one first - the old man advised us to use a `sword`, and surely, the password is `sword`.

```
→  7z x wooden_crate

7-Zip [64] 9.20  Copyright (c) 1999-2010 Igor Pavlov  2010-11-18
p7zip Version 9.20 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,8 CPUs)

Processing archive: wooden_crate

Enter password (will not be echoed) :

Extracting  3331746f725f6e6168745f65726f6d

Everything is Ok

Size:       2976
Compressed: 1065
```

Great, another file. We check its type:
```
→  file 3331746f725f6e6168745f65726f6d
3331746f725f6e6168745f65726f6d: ASCII text, with very long lines, with no line terminators
```

The first thing that strikes us is the file name. That is hex! Let's try to decode it as ASCII:
```
→  echo 3331746f725f6e6168745f65726f6d | xxd -r -p
31tor_naht_erom
```

We reverse `31tor_naht_erom` and get: `more_than_rot13`. A handy piece of advice :)

Next, we check the file's content:
```
→  cat 3331746f725f6e6168745f65726f6d
bGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbApNICwgcHNzI2IkZCAgLiBcICwgd2IpIHMmfCEgICAgLiBcXFxcXFwgLCByX3x8Y31zICAgICAgICAgICAgICAgICAgICAgICAgLiBNCmxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGwKTSAsIF9fY19gX19fIC4gbSAsIGJgIHJfICAgICAgIC4gXFxcXFxcICwgKX4jIHRwKVt0cCkgICAgICAgICAgICAgICAgICAgIC4gTQpNICwgX19jX2BfX2EgLiBdICwgcV8gY18gICAgICAgLiBcXFxcXFwgLCB8ficgcHtbY18gICAgICAgICAgICAgICAgICAgICAgLiBNCk0gLCBfX2NfYF9fYyAuIF0gLCByYCB0XyBgXyAgICAuIFxcXFxcXCAsICR3eyB0cClbYF8gICAgICAgICAgICAgICAgICAgICAuIE0KTSAsIF9fY19gX19mIC4gXSAsIGdfIHVjIGFfICAgIC4gXFxcXFxcICwgKX4jIHB3W2FfICAgICAgICAgICAgICAgICAgICAgIC4gTQpNICwgX19jX2BfX3AgLiBdICwgdXUgc18gICAgICAgLiBcXFxcXFwgLCBycHt7IHRwKSAgICAgICAgICAgICAgICAgICAgICAgLiBNCk0gLCBfX2NfYF9fciAuIF0gLCBkXyAgICAgICAgICAuIFxcXFxcXCAsICEmJHcgdHApICAgICAgICAgICAgICAgICAgICAgICAuIE0KTSAsIF9fY19gX19zIC4gXSAsIGdwIGRfIF9iICAgIC4gXFxcXFxcICwgfH4nIHN7W3EqJXQgISUjIHMkaSx0cClaYi4gICAgIC4gTQpNICwgX19jX2BfYF8gLiBdICwgYl8gZGMgX2AgX2MgLiBcXFxcXFwgLCApfiMgcSoldCAhJSMgcyRpLHRwKVp0cilaYy5bc3sgLiBNCk0gLCBfX2NfYF9gYyAuIF0gLCB1dCByYCAgICAgICAuIFxcXFxcXCAsIHh9ciByeyAgICAgICAgICAgICAgICAgICAgICAgICAuIE0KTSAsIF9fY19gX2BlIC4gXSAsIGdfIHVoIGVjICAgIC4gXFxcXFxcICwgcnwhIHJ7W2VjICAgICAgICAgICAgICAgICAgICAgIC4gTQpNICwgX19jX2BfYGggLiBdICwgZmQgdWQgICAgICAgLiBcXFxcXFwgLCB5fXQgJHd+IyUgX19jX2BfYF8gICAgICAgICAgICAgLiBNCk0gLCBfX2NfYF9gcSAuIF0gLCBnXyB0aCBkdSAgICAuIFxcXFxcXCAsICQmcSBye1tkdSAgICAgICAgICAgICAgICAgICAgICAuIE0KTSAsIF9fY19gX2B0IC4gXSAsIF9fIHJnICAgICAgIC4gXFxcXFxcICwgcHNzIHB7W3J7ICAgICAgICAgICAgICAgICAgICAgIC4gTQpNICwgX19jX2BfYV8gLiBdICwgdXUgc18gICAgICAgLiBcXFxcXFwgLCBycHt7IHRwKSAgICAgICAgICAgICAgICAgICAgICAgLiBNCk0gLCBfX2NfYF9hYSAuIF0gLCB1YyAgICAgICAgICAuIFxcXFxcXCAsIHd7JSAgICAgICAgICAgICAgICAgICAgICAgICAgICAuIE0KbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbApNICwgcHNzI2IkZCAgLiBcICwgfGJ8XyMqIHdiKSBzJnwhICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgLiBNCmxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGwKTSAsIF9fY19hX19fIC4gaSAsIGJgIHJoIHJiIHJfIE0gYHQgY18gYXIgc18gTSBidCBnXyBycCBjYCBNIHFfIHJxIHJfIHJfIC4gTQpNICwgX19jX2FfYF8gLiBpICwgcmggc3AgY18gcGcgTSBzYiBzcCBjXyBwZyBNIHNnIHViIHVfIF9fIE0gaF8gX2IgYXUgc2YgLiBNCk0gLCBfX2NfYV9hXyAuIGkgLCBoYyBjcCBgdSBfXyBNIGh0IHRyIGJwIGhmIE0gc3MgaHQgc2ggdHQgTSBwcyBiciBoZSBfcyAuIE0KTSAsIF9fY19hX2JfIC4gaSAsIHRfIHN1IGh0IHRmIE0gYmEgZXEgdWYgc2cgTSBgcyB0cCB0YCBycyBNIGVwIGZ0IGV1IF9jIC4gTQpNICwgX19jX2FfY18gLiBpICwgX3AgX3AgX3AgX3EgTSBfcCBfcCBfcCBfcSBNIF9hIF9xIGZyIF9wIE0gX3AgX3AgX3EgX3AgLiBNCk0gLCBfX2NfYV9kXyAuIGkgLCBfcCBmaCBfciBfcSBNIF9zIF9yIF9oIF9iIE0gX2IgZmggX3AgZnUgTSBfYSBmciBmdSBfYiAuIE0KTSAsIF9fY19hX2VfIC4gaSAsIGZ1IF9xIGZyIF9nIE0gX2IgZmggX3AgZnUgTSBfXyBfXyBfXyBfXyBNIF9fIF9fIF9fIF9fIC4gTQpsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxs
```

We immediately notice that there are a lot of repetitive characters, and also no special symbols.
We try to decode it, starting with the famous **Base64**.
Surely, we get it right from the first time:

```
→  cat 3331746f725f6e6168745f65726f6d  | base64 -d
llllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllll
M , pss#b$d  . \ , wb) s&|!    . \\\\\\ , r_||c}s                        . M
llllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllll
M , __c_`___ . m , b` r_       . \\\\\\ , )~# tp)[tp)                    . M
M , __c_`__a . ] , q_ c_       . \\\\\\ , |~' p{[c_                      . M
M , __c_`__c . ] , r` t_ `_    . \\\\\\ , $w{ tp)[`_                     . M
M , __c_`__f . ] , g_ uc a_    . \\\\\\ , )~# pw[a_                      . M
M , __c_`__p . ] , uu s_       . \\\\\\ , rp{{ tp)                       . M
M , __c_`__r . ] , d_          . \\\\\\ , !&$w tp)                       . M
M , __c_`__s . ] , gp d_ _b    . \\\\\\ , |~' s{[q*%t !%# s$i,tp)Zb.     . M
M , __c_`_`_ . ] , b_ dc _` _c . \\\\\\ , )~# q*%t !%# s$i,tp)Ztr)Zc.[s{ . M
M , __c_`_`c . ] , ut r`       . \\\\\\ , x}r r{                         . M
M , __c_`_`e . ] , g_ uh ec    . \\\\\\ , r|! r{[ec                      . M
M , __c_`_`h . ] , fd ud       . \\\\\\ , y}t $w~#% __c_`_`_             . M
M , __c_`_`q . ] , g_ th du    . \\\\\\ , $&q r{[du                      . M
M , __c_`_`t . ] , __ rg       . \\\\\\ , pss p{[r{                      . M
M , __c_`_a_ . ] , uu s_       . \\\\\\ , rp{{ tp)                       . M
M , __c_`_aa . ] , uc          . \\\\\\ , w{%                            . M
llllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllll
M , pss#b$d  . \ , |b|_#* wb) s&|!                                       . M
llllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllll
M , __c_a___ . i , b` rh rb r_ M `t c_ ar s_ M bt g_ rp c` M q_ rq r_ r_ . M
M , __c_a_`_ . i , rh sp c_ pg M sb sp c_ pg M sg ub u_ __ M h_ _b au sf . M
M , __c_a_a_ . i , hc cp `u __ M ht tr bp hf M ss ht sh tt M ps br he _s . M
M , __c_a_b_ . i , t_ su ht tf M ba eq uf sg M `s tp t` rs M ep ft eu _c . M
M , __c_a_c_ . i , _p _p _p _q M _p _p _p _q M _a _q fr _p M _p _p _q _p . M
M , __c_a_d_ . i , _p fh _r _q M _s _r _h _b M _b fh _p fu M _a fr fu _b . M
M , __c_a_e_ . i , fu _q fr _g M _b fh _p fu M __ __ __ __ M __ __ __ __ . M
llllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllllll
```

That is a wonderfully structured text, but sadly we can't read it. 
Fortunately for us, we know this is **ROT encrypted** with a shift greater than **13**.
We could go ahead and use a [Caesar Cipher bruteforce tool](https://www.dcode.fr/caesar-cipher),
or use the handy [CyberChef framework](https://gchq.github.io/CyberChef/) I suggested in the first level.

Either case, we figure out that a shift of **47** gives us a readable output:
```
============================================================================
| [ ADDR3S5  ] - [ H3X DUMP    ] ------ [ C0MM4ND                        ] |
============================================================================
| [ 00401000 ] > [ 31 C0       ] ------ [ XOR EAX,EAX                    ] |
| [ 00401002 ] . [ B0 40       ] ------ [ MOV AL,40                      ] |
| [ 00401004 ] . [ C1 E0 10    ] ------ [ SHL EAX,10                     ] |
| [ 00401007 ] . [ 80 F4 20    ] ------ [ XOR AH,20                      ] |
| [ 0040100A ] . [ FF D0       ] ------ [ CALL EAX                       ] |
| [ 0040100C ] . [ 50          ] ------ [ PUSH EAX                       ] |
| [ 0040100D ] . [ 8A 50 03    ] ------ [ MOV DL,BYTE PTR DS:[EAX+3]     ] |
| [ 00401010 ] . [ 30 54 01 04 ] ------ [ XOR BYTE PTR DS:[EAX+ECX+4],DL ] |
| [ 00401014 ] . [ FE C1       ] ------ [ INC CL                         ] |
| [ 00401016 ] . [ 80 F9 64    ] ------ [ CMP CL,64                      ] |
| [ 00401019 ] . [ 75 F5       ] ------ [ JNE SHORT 00401010             ] |
| [ 0040101B ] . [ 80 E9 5F    ] ------ [ SUB CL,5F                      ] |
| [ 0040101E ] . [ 00 C8       ] ------ [ ADD AL,CL                      ] |
| [ 00401020 ] . [ FF D0       ] ------ [ CALL EAX                       ] |
| [ 00401022 ] . [ F4          ] ------ [ HLT                            ] |
============================================================================
| [ ADDR3S5  ] - [ M3M0RY H3X DUMP                                       ] |
============================================================================
| [ 00402000 ] : [ 31 C9 C3 C0 | 1E 40 2C D0 | 3E 80 CA 41 | B0 CB C0 C0 ] |
| [ 00402010 ] : [ C9 DA 40 A8 | D3 DA 40 A8 | D8 F3 F0 00 | 90 03 2F D7 ] |
| [ 00402020 ] : [ 94 4A 1F 00 | 9E EC 3A 97 | DD 9E D9 EE | AD 3C 96 0D ] |
| [ 00402030 ] : [ E0 DF 9E E7 | 32 6B F7 D8 | 1D EA E1 CD | 6A 7E 6F 04 ] |
| [ 00402040 ] : [ 0A 0A 0A 0B | 0A 0A 0A 0B | 02 0B 7C 0A | 0A 0A 0B 0A ] |
| [ 00402050 ] : [ 0A 79 0C 0B | 0D 0C 09 03 | 03 79 0A 7F | 02 7C 7F 03 ] |
| [ 00402060 ] : [ 7F 0B 7C 08 | 03 79 0A 7F | 00 00 00 00 | 00 00 00 00 ] |
============================================================================
```

Fantastic, a hex and memory dump! Hop over to the [second part](SOLUTION_CONT.md) of the solution, where we will get to work.
