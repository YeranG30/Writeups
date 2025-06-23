# Dog Picture Steganography Challenge Writeup CTF\@CIT 2025

**Challenge Name:** Dog Picture

**Category:** Steganography
**Hash:** Verified manually via file analysis

---

## Summary

This challenge provided an innocent-looking PNG image named `dog.png`. A first glance revealed no obvious clues, but through structured steg analysis and bit-plane inspection, we uncovered hidden ASCII content embedded via Least Significant Bit (LSB) steganography. The challenge served as a valuable reminder to systematically test common steg techniques when standard tooling fails.

---

## Methodology

### 1. **Initial File Inspection**

Used `file` and `exiftool` to determine the file type and metadata:

```bash
file dog.png
exiftool dog.png
```

Findings:

* Valid 1200x602 PNG image
* No strange metadata or comments
* ICC Profile present (standard RGB)

### 2. **Steganography Hypothesis**

No anomalies were visible via standard inspection tools. The next logical step was to test LSB techniques:

* PNG format is lossless — commonly used for LSB hiding
* Image dimensions are large enough to contain embedded data

### 3. **Zsteg and Manual Inspection**

Ran `zsteg` to scan for embedded content:

```bash
zsteg dog.png
```

However, `zsteg` encountered a recursion bug:

```
SystemStackError: stack level too deep
```

This crash suggested malformed or complex embedded data. At this point, automated detection was no longer reliable.

### 4. **Fallback to Manual Bit Plane Extraction**

Since `zsteg` is typically used to extract LSB data from various pixel planes and bit orders, we knew its failure meant we’d have to replicate that functionality manually.

We turned to **StegOnline**, which lets us configure key parameters:

* **Pixel Order:** Row
* **Bit Order:** MSB (most significant bit first)
* **Bit Plane:** 0 (least significant bit)
* **Bit Plane Order:** G → B → R (a permutation that cycles bits from green, blue, then red per pixel)

This specific permutation revealed legible ASCII, starting with:

```
...look how cute this dog is :) anyway here's the flag CIT{bL30KMnEbj21}...
```

We learned the encoder likely embedded the message one bit at a time using a custom script in the order: Green → Blue → Red per pixel. Had we picked a different plane order (e.g., R → G → B), the text would’ve appeared as noise.

By rotating these settings methodically, we were able to isolate the valid configuration and extract the flag.

---

## What I Learned

* **When automated tools break, manual inspection becomes critical.** Zsteg’s crash forced a fallback to browser-based tools.
* **Zsteg mimics manual bit-plane reading**, so when it fails, we emulate it: read 1 bit at a time per channel, per pixel, in different orders.
* **Always rotate permutations of bit plane order (e.g., G→B→R, R→G→B)** to find the correct one.
* **Bit plane 0 (LSB) in the green channel is a common spot for hiding data**, due to visual imperceptibility.
* **A structured methodology saves time** — moving from file checks, to zsteg, to fallback tools like StegOnline.

---

## Tools Used

* `file`, `exiftool` – Metadata analysis
* `zsteg` – LSB data extraction (partial failure)
* [StegOnline](https://futureboy.us/stegano/) – Manual LSB visual analysis
* GIMP/Image viewer – Basic image verification

---

## Conclusion

This challenge demonstrated the importance of layering techniques when faced with steganography. Even when zsteg failed, a proper fallback using visual LSB extraction led to a successful flag discovery. This reinforces a key principle: **be flexible with your tools, but rigorous in your process.**

---

**Flag:** CIT{bL30KMnEbj21}
