# Steganography Challenge – I AM STEVE [CIT CTF 2025]

**Challenge Type:** Steganography  
**Challenge Name:** ChickenJockey  
**CTF:** CIT CTF 2025  
**Flag:** `CIT{THIS_is_a_crafting_table}`

---

## Challenge Description

A PNG image titled `ChickenJockey.png` was provided. Upon initial inspection, it looked like a normal image, but we suspected it had hidden information embedded using steganographic techniques.

---

##  Quick Tip (CTF Strategy)

> In CTFs, when you see a short base64-looking string in a `zsteg` output — **assume it's base64** and start decoding it.  
> Most stego challenges start with simple LSB tricks before getting fancy.  
> If it looks like base64... it's probably base64.

---

## Step-by-Step Process

###  Step 1: File Validation

```bash
file ChickenJockey.png
```

**Output:**
```
PNG image data, 640 x 363, 8-bit/color RGBA, non-interlaced
```

A standard PNG image — nothing unusual at first glance.

---

###  Step 2: Run `zsteg`

```bash
zsteg ChickenJockey.png
```

**Key Output:**
```
b1,rgb,lsb,xy       .. text: "VEhJU19pc19hX2NyYWZ0aW5nX3RhYmxl"
```

This string looks like base64. So we decode it next.

---

### Step 3: Decode Suspected Base64

```bash
echo "VEhJU19pc19hX2NyYWZ0aW5nX3RhYmxl" | base64 -d
```

**Decoded Output:**
```
THIS_is_a_crafting_table
```

---

##  Final Flag

```text
CIT{THIS_is_a_crafting_table}
```

---

## Other Notes from `zsteg`

You may notice:
- `OpenPGP Public/Secret Key` references — likely noise or a red herring
- Random "DDDD" strings — not meaningful on decode
- LSB in red channel (`b1,r,lsb,xy`) had random string `v6RNA]bgD`

All these are typical **steg distractors**. In most beginner/intermediate CTF challenges, **only one channel matters**.

---

##  What to Remember

* Always check for base64 first — it’s the most common CTF steg trick.
* Only trust readable strings from `zsteg` that decode into meaningful text.
* Extra “OpenPGP” or repeating characters are often meant to throw you off.
* Use your eyes — does something *look* encoded? Try decoding it.
