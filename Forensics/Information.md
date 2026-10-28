> Files can always be changed in a secret way. Can you find the flag? [cat.jpg](https://challenge-files.cylabacademy.net/library/c585b4adbdaa06f409db7bf5012c2e58e9aab24669764a5f1cf0f9ce0d60dcfd/cat.jpg)

---

## Challenge Access

| Field    | Value                                                                                                                                  |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Category | Forensics                                                                                                                              |
| Points   | Easy                                                                                                                                   |
| Author   | susie                                                                                                                                  |
| Files    | [`cat.jpg`](https://challenge-files.cylabacademy.net/library/c585b4adbdaa06f409db7bf5012c2e58e9aab24669764a5f1cf0f9ce0d60dcfd/cat.jpg) |

---

## Commands Used

- `exiftool` → Inspect image metadata
- `base64` → Decode the suspicious field

---

## Solution

### Step 1 -- Understand the Problem

The hint says files "can always be changed in a secret way." That's a nod to metadata: image files carry a bundle of EXIF/XMP/IPTC fields (camera info, copyright, timestamps) that have nothing to do with the pixels and can be edited freely without touching how the image looks. If a flag is hiding anywhere in a plain .jpg with no other files given, metadata is the first thing to check.

---

### Step 2 -- Inspect the Image Metadata

```bash
$ exiftool cat.jpg
ExifTool Version Number         : 12.76
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2026:10:27 12:58:19+01:00
File Access Date/Time           : 2026:10:27 13:01:40+01:00
File Inode Change Date/Time     : 2026:10:27 12:58:45+01:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 13.25
License                         : YWNhZGVteXt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

Most of this is boilerplate JPEG/XMP metadata, but the `License` field stands out: a normal image license is a short human-readable string ("CC BY-NC 4.0", a URL, etc.), not a 44-character block of mixed-case alphanumerics. That shape is the signature of base64.

---

### Step 3 -- Decode the License Field

```bash
$ echo "YWNhZGVteXt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 --decode
academy{...}
```

The decoded string is the flag itself, already in `academy{...}` format, so no further processing is needed.

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Metadata is a first-class hiding spot.** `exiftool` should be one of the first things run against any image/doc/media file in a forensics challenge, before reaching for binwalk, strings, or a hex editor.
- **Spot the field that doesn't belong.** The giveaway wasn't that metadata existed, it was that one ordinary-looking field (`License`) held data with the wrong _shape_ for what it claims to be.
- **Recognize base64 by eye.** A block of `A-Za-z0-9+/` characters, often padded with `=`, with no spaces and no obvious structure, is almost always base64 and worth a decode attempt before anything fancier.