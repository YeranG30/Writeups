# Blank Image (steg) – CIT CTF 2025

**Challenge Type:** Steganography  
**Challenge Name:** Unknown (8x17 image)  
**CTF:** CIT CTF 2025  
**Flag:** `CIT{n1F0Rsm0Er40}`

---

## Challenge Description

An image was provided that appeared to be blank (8x17 pixels, RGBA). Our goal was to extract hidden information possibly embedded using LSB techniques or appended data.

---

## Methodology

1. **Downloaded the image using `wget`**
2. **Analyzed the file metadata and structure using standard tools**
3. **Used `binwalk` and `strings` to check for embedded data or plaintext**
4. **Used `zsteg` to analyze LSB-encoded text**
5. **Flag found in alpha channel (LSB) using `zsteg`**

---

## Commands Used

```bash
# Step 1: Rename file if needed (remove URL tokens)
mv 'image.png?token=...' image.png

# Step 2: Basic checks
file image.png
exiftool image.png
strings image.png | less

# Step 3: Check for embedded files
binwalk -e image.png

# Step 4: Use zsteg to analyze LSB channels
zsteg image.png
```

**Output:**
```
b1,a,lsb,xy         .. text: "CIT{n1F0Rsm0Er40}"
```

---

## Conclusion

The flag was hidden using least significant bit encoding in the **alpha channel** of the PNG image. `zsteg` identified and extracted the flag efficiently without requiring any brute-force or decoding tricks.
