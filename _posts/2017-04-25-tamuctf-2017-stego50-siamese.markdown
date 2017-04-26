<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="chrome=1">
    <title>Midnight by mattgraham</title>
    <link rel="stylesheet" href="stylesheets/styles.css">
    <link rel="stylesheet" href="stylesheets/pygment_trac.css">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
    <script src="javascripts/respond.js"></script>
    <!--[if lt IE 9]>
      <script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
    <!--[if lt IE 8]>
    <link rel="stylesheet" href="stylesheets/ie.css">
    <![endif]-->
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">

</head>

---
title: TAMUctf 2017 - Siamese
date: 2017-04-25 18:29:00 Z
categories:
- write-ups
tags:
- stego
- tamuctf
---

**Category:** Steganography
**Points:** 50
**Solves:** 324
**Description:**

> The internet loves cats. Whats not to love?

# Write-up

let’s see what’s the type of the [file](https://github.com/dbaser/ctfs/blob/master/TAMUctf-2017/stego50-siamese/8ff4da2f7368f800)

```bash
$ file 8ff4da2f7368f800
    
8ff4da2f7368f800: GIF image data, version 89a, 320 x 180
```    

run the `binwalk` to see hidden files...

```bash
$ binwalk 8ff4da2f7368f800  
    
DECIMAL       HEXADECIMAL     DESCRIPTION
---------------------------------------------------------------------------
    0             0x0             GIF image data, version "89a", 320 x 180
    662419        0xA1B93         MySQL MISAM index file Version 10
    3204803       0x30E6C3        Zip archive data, at least v1.0
    3205027       0x30E7A3        End of Zip archive
```

...and extract the zip file with `dcfldd`

```bash
$ dcfldd if=8ff4da2f7368f800 bs=1 skip=3204803 of=file.zip

0 bs=1 skip=3204803 of=file.zip

246+0 records in
246+0 records out
```

`unzip` and `cat` the file!

```bash
$ unzip file.zip 

Archive:  file.zip
 extracting: 8ff4da2f7368f800.txt    

$ cat 8ff4da2f7368f800.txt | base64 -d

gigem{the_cat_goes_meow_3b96b806f6515c23}

```

The flag is: `gigem{the_cat_goes_meow_3b96b806f6515c23}`
