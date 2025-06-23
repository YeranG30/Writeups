# Queen's Gambit — Steg Writeup CTF@CIT 2025

**Challenge Name:** Queen's Gambit
**Hash:** SHA1: `819b1c887ba3699ca5cc90b64d8dc8f8712d6bba`

## Summary

We were given an image titled `Queen.png`, with no other direct clues. Given the challenge title references a CHess theme, and the image filename was directly referencing the Queen, we investigated it using a variety of steganographic techniques. Ultimately, the solution involved extracting data using `zsteg` and interpreting it as a chessboard position.

---

## Methodology

### Step 1: File Inspection

We first ran basic metadata tools:

```bash
exiftool Queen.png
```

No meaningful metadata was embedded.

### Step 2: Run Steganography Scanners

We proceeded to enumerate steganographic data using `zsteg`:

```bash
zsteg Queen.png
```

From this output, the following line stood out:

```
b1,rgb,lsb,xy       .. text: "a8 a7 a6 a5 b8 c7 b6 d5 e4 f5 g4 h5 c1 c2 c3 d2 e1 e2 e3 "
```

This immediately looked like chess notation.

### Step 3: Reconstruct the Chessboard

Using an online chess editor, we placed pawns (`P`) on the extracted positions:

```
a8, a7, a6, a5, b8, c7, b6, d5, e4, f5, g4, h5, c1, c2, c3, d2, e1, e2, e3
```

This revealed a nearly full board of white pawns.

### Step 4: Interpreting the Clue

The challenge name **Queen's Gambit** + the Queen image name + a pawn-only board hinted at a message hidden in the position of pawns.

On visual inspection and pattern matching, the pawns subtly formed the characters:
![image](https://github.com/user-attachments/assets/59d8a2f0-53cb-47d0-86ef-c3a97f6a98df)


```
p h w
```

Giving the flag

```
CIT{phw}
```

## Reflections


I believe this is a poorly made challenge. It’s worth noting that visual interpretation was tricky due to how differently board renderers space pieces. This means even if you have the correct FEN or coordinates, slight spacing or font choices can distort the letter recognition, making this frustrating but educational.

## Flag

```
CIT{phw}
```
