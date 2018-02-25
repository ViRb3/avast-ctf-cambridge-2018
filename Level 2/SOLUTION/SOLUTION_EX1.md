If we [Google](https://www.google.com/) the image name `262144` we [discover that it factorizes to **2^18**](https://numbermatics.com/n/262144/). Our width **512** factorizes to **2^9**, but **496** doesn't, so it must be the wrong one. To calculate the right height, we have to divide the **___decompressed___ IDAT chunk data size** by the **channel number** and then by the **image width**. A great explanation of the **PNG** file format can be found [here](https://github.com/corkami/pics/blob/master/binary/PNG.png?raw=true).

We extract and decompress the **IDAT chunk data** via `binwalk` with the `-e` switch:

```
→  binwalk -e 262144.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 512 x 496, 8-bit/color RGB, non-interlaced
41            0x29            Zlib compressed data, best compression
2249          0x8C9           Zlib compressed data, default compression
```

Using `pnginfo` we see the channel number:
```
→  pnginfo 262144.png
262144.png...
  Image Width: 512 Image Length: 496
  Bitdepth (Bits/Sample): 8
  Channels (Samples/Pixel): 3
  Pixel depth (Pixel Depth): 24
  Colour Type (Photometric Interpretation): RGB
  Image filter: Single row per byte filter
  Interlacing: No interlacing
  Compression Scheme: Deflate method 8, 32k window
  Resolution: 0, 0 (unit unknown)
  FillOrder: msb-to-lsb
  Byte Order: Network (Big Endian)
  Number of text strings: 0
```

In our case, there are **3** channels.
We then use `pngcheck` with the `-v` switch to see the IDAT chunk offset:
```
root@rootbox 01:08:49 /root/ →  pngcheck -v 262144.png
File: 262144.png (2524 bytes)
  chunk IHDR at offset 0x0000c, length 13
    512 x 496 image, 24-bit RGB, non-interlaced
  chunk IDAT at offset 0x00025, length 2192
    zlib: deflated, 32K window, maximum compression
  chunk zTXt at offset 0x008c1, length 263, keyword: >>
  chunk IEND at offset 0x009d4, length 0
No errors detected in 262144.png (4 chunks, 99.7% compression).
 ```
 
We see that the offset is **0x25**, but since this is the start of the chunk, there is first a **4-byte-long chunk type descriptor**. The beginning of our data section is therefore **0x29**, or the `29` file extracted by `bindiff`. We take its length:
```
→  ls -l 29
-rwxrwxrwx 1 root root 786944 Jan 25 02:16 29
```

To calculate the original height: `786944 / 512 / 3 = 512`. So our original image is **512x512**!
