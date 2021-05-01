# Cryptography

*Solved: 6, Points: 580*
| Challenges | Points |
| ---- | ---- |
| [Mod 26](#mod-26-10-pts) | 10 pts |
| [New Caesar](#new-caesar-60-pts) | 60 pts |
| [Pixelated](#pixelated-100-pts) | 100 pts |
| [Play Nice](#play-nice-110-pts) | 110 pts |
| [New Vignere](#new-vignere-300-pts) | 300 pts |


## Mod 26 (10 pts)

>Cryptography can be easy, do you know what ROT13 is? `cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_nSkgmDJE}`  

Pretty simple question just use a [ROT13 decoder](https://rot13.com/) and get the flag: `picoCTF{next_time_I'll_try_2_rounds_of_rot13_aFxtzQWR}`

*_Taya*

## New Caesar (60 pts)

>We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{})  
`lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil` [new_caesar.py](https://mercury.picoctf.net/static/c9043977604318594ab73d126a01d0b1/new_caesar.py)  
Hint 1: How does the cipher work if the alphabet isn't 26 letters?  
Hint 2: Even though the letters are split up, the same paradigms still apply 

In the `new_caesar.py` file there is the encryption algorithm that was used. There are a few important things to note. 
- On line 4 it gives the alphabet: `ALPHABET = string.ascii_lowercase[:16]` 
    - This means the alphabet used is the first 16 lowercase letters of the alphabet: 'a-p'.
- On line 21 it checks the letters of the key are all in ALPHABET (a-p): `assert all([k in ALPHABET for k in key])`
- On line 22 it checks the length of the key is 1: `assert len(key) == 1`

Using this information and the code used to encrypt it, we can write a python script to decrypt the information and we only need to try 16 options for the key.

The encryption method first passes the string to b16_encode which doubles the length of the string by converting each letter to the binary value and splitting it in half to produce 2 letters.
    
    def b16_encode(plain):
	enc = ""
	for c in plain:
		binary = "{0:08b}".format(ord(c))
		enc += ALPHABET[int(binary[:4], 2)]
		enc += ALPHABET[int(binary[4:], 2)]
	return enc

For example if the flag was just a the following line: `binary = "{0:08b}".format(ord('a'))` would give `01100001` and then `binary[:4] = 0110` or 6 and `binary[4:] = 0001` or 1. These are the index of the characters in ALPHABET. so enc="gb" for plain = "a" This is the code to reverse that:

    def b16_decode(enc):
        plain = ""
        for c in range(0,len(enc), 2):
            upperhalf = (ord(enc[c])-LOWERCASE_OFFSET)<<4
            lowerhalf = ord(enc[c+1])-LOWERCASE_OFFSET
            plain += chr(lowerhalf+upperhalf)
        return plain

Next, the encoded string is passed to the shift method, character by character.
    
    def shift(c, k):
        t1 = ord(c) - LOWERCASE_OFFSET
        t2 = ord(k) - LOWERCASE_OFFSET
        return ALPHABET[(t1 + t2) % len(ALPHABET)]

    enc = ""
    for i, c in enumerate(b16):
        enc += shift(c, key[i % len(key)])

This just shifts the letters by the letter in key. To reverse:

    def unshift(c, k):
        t1 = ord(c) - LOWERCASE_OFFSET
        t2 = ord(k) - LOWERCASE_OFFSET
        t3 = t1-t2
        if t3 < 0:
            t3 += 16
        return ALPHABET[(t3) % len(ALPHABET)]

Then we can just run through every letter in ALPHABET and see which ones return valid characters. The whole script is:

    import string

    LOWERCASE_OFFSET = ord("a")
    ALPHABET = string.ascii_lowercase[:16]
        
    def b16_decode(enc):
        plain = ""
        for c in range(0,len(enc), 2):
            upperhalf = (ord(enc[c])-LOWERCASE_OFFSET)<<4
            lowerhalf = ord(enc[c+1])-LOWERCASE_OFFSET
            plain += chr(lowerhalf+upperhalf)
        return plain

    def unshift(c, k):
        t1 = ord(c) - LOWERCASE_OFFSET
        t2 = ord(k) - LOWERCASE_OFFSET
        t3 = t1-t2
        if t3 < 0:
            t3 += 16
        return ALPHABET[(t3) % len(ALPHABET)]

    enc = 'lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil'
    for letter in ALPHABET:
        print(letter)
        key = letter
        dec = ""
        for i, c in enumerate(enc):
            dec += unshift(c, key[i % len(key)])
        myFlag = b16_decode(dec)
        print(myFlag)

The only valid output is when key = f. The decoded string is: et_tu?_431db62c5618cd75f1d0b83832b67b46

So the flag is: `picoCTF{et_tu?_431db62c5618cd75f1d0b83832b67b46}`

*_Taya*

## Pixelated (100 pts)

>I have these 2 images, can you make a flag out of them? [scrambled1.png](https://mercury.picoctf.net/static/9f2d081f12c05202359632c1989e7927/scrambled1.png) [scrambled2.png](https://mercury.picoctf.net/static/9f2d081f12c05202359632c1989e7927/scrambled2.png)  
Hint 1: [https://en.wikipedia.org/wiki/Visual_cryptography](https://en.wikipedia.org/wiki/Visual_cryptography)  
Hint 2: Think of different ways you can "stack" images  

The Wikipedia page from Hint 1 talks about visual cryptography in which an image can be broken into multiple other images which when printed on transparencies and stacked, reveal the original image.
I used Photoshop to stack the two pngs given in the challenge statement.
It took a lot of playing around and trying random stuff out, changing the “blending mode” of one of the layers to “Hard Mix” did the trick:

![pixelated 1](./pictures/pixelated-1.png "stacking the two images") 

After using a white filter and desaturating the image to grayscale (to make the picture less seizure-inducing), the flag was clearer but figuring out the exact characters still took a pit of guesswork (Thank you Taya!)

![pixelated 2](./pictures/pixelated-2.png "the flag") 

The flag: `picoCTF{7188864c}`  

*_Tiare & Taya*

## Play Nice (110 pts)

>Not all ancient ciphers were so bad... The flag is not in standard format. `nc mercury.picoctf.net 19354` [playfair.py](https://mercury.picoctf.net/static/9ea1604c8767cd6545948ad54670c2bf/playfair.py)  

So when I solved this in the competition I looked at the playfair.py file and actually wrote a python script to decrypt the plaintext they give when connecting to `nc mercury.picoctf.net 19354`. 

Afterwards I realized at the bottom of playfair.py there is a link to a [wikepedia page](https://en.wikipedia.org/wiki/Playfair_cipher). Apparently it is a well known cipher. You can use this [playfair cipher decrypter](https://www.dcode.fr/playfair-cipher) or any other one you find online. Input that alphabet they give you in a 6x6 grid. Then just decrypt the message they give you and submit it to get the flag: `dbc8bf9bae7152d35d3c200c46a0fa30`

*_Taya*

## New Vignere (300 pts)

>Another slight twist on a classic, see if you can recover the flag. (Wrap with picoCTF{})  
`ilnipdjheipnenhhedionepegiejmleoehejfcnimdgehimnepedhhfbafmcgdek` [new_vignere.py](https://mercury.picoctf.net/static/d86ead586609c44b84b04e08966a4d35/new_vignere.py)  
Hint: [https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher#Cryptanalysis](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher#Cryptanalysis)  

This problem is very similar to [New Caesar](#new-caesar-60-pts) with a bit of added complexity. The given file `new_vignere.py` has the encryption method used and it has the same b16_encode and shift method. THe difference this time is the information we have about the flag and the key.

    flag = "redacted"
    assert all([c in "abcdef0123456789" for c in flag])

    key = "redacted"
    assert all([k in ALPHABET for k in key]) and len(key) < 15

We have the following information:
- ALPHABET is still all lowercase letters a-p
- The flag is some combination of the following letters and numbers "abcdef0123456789"
- The key is some combination of letters in ALPHABET
- the length of the key is anywhere from 1-14

The first thing I did was pass "abcdef0123456789" to b16_encode to seee the possible values from the shift. Also, since b16_encode returns 2 characters for every input character, I spaced it out accordingly:

    b16FlagAlphabet = b16_encode("abcdef0123456789")
    print("encoded:", end=' ')
    for i in range(0, len(b16FlagAlphabet),2):
        print(b16FlagAlphabet[i]+b16FlagAlphabet[i+1], end=' ')
    
    output:
    encoded: gb gc gd ge gf gg da db dc dd de df dg dh di dj

This gives us very valuable information for when we unshift the encrypted message, every other letter must be 'g' or 'd'. 

Use the same unshift method from [New Caesar](#new-caesar-60-pts).  I wrote a script to unshift every other letter in the encoded message with the possible letter options for the key. If the unshifted letter is a g or a d then I print the possible letter for key in that position.

    encoded = "ilnipdjheipnenhhedionepegiejmleoehejfcnimdgehimnepedhhfbafmcgdek"

    for i in range (0, len(encoded), 2):
        for letter in ALPHABET:
            dec = ""
            dec = unshift(encoded[i], letter)
            if dec == "g":
                print("letter: "+letter, end=' ')
            if dec == "d":
                print("letter: "+letter, end=' ')
        print("    i: "+ str(i) + " encoded letter: "+ encoded[i])

The output:

    letter: c letter: f     i: 0 encoded letter: i
    letter: h letter: k     i: 2 encoded letter: n
    letter: j letter: m     i: 4 encoded letter: p
    letter: d letter: g     i: 6 encoded letter: j
    letter: b letter: o     i: 8 encoded letter: e
    letter: j letter: m     i: 10 encoded letter: p
    letter: b letter: o     i: 12 encoded letter: e
    letter: b letter: e     i: 14 encoded letter: h
    letter: b letter: o     i: 16 encoded letter: e
    letter: c letter: f     i: 18 encoded letter: i
    letter: h letter: k     i: 20 encoded letter: n
    letter: j letter: m     i: 22 encoded letter: p
    letter: a letter: d     i: 24 encoded letter: g
    letter: b letter: o     i: 26 encoded letter: e
    letter: g letter: j     i: 28 encoded letter: m
    letter: b letter: o     i: 30 encoded letter: e
    letter: b letter: o     i: 32 encoded letter: e
    letter: b letter: o     i: 34 encoded letter: e
    letter: c letter: p     i: 36 encoded letter: f
    letter: h letter: k     i: 38 encoded letter: n
    letter: g letter: j     i: 40 encoded letter: m
    letter: a letter: d     i: 42 encoded letter: g
    letter: b letter: e     i: 44 encoded letter: h
    letter: g letter: j     i: 46 encoded letter: m
    letter: b letter: o     i: 48 encoded letter: e
    letter: b letter: o     i: 50 encoded letter: e
    letter: b letter: e     i: 52 encoded letter: h
    letter: c letter: p     i: 54 encoded letter: f
    letter: k letter: n     i: 56 encoded letter: a
    letter: g letter: j     i: 58 encoded letter: m
    letter: a letter: d     i: 60 encoded letter: g
    letter: b letter: o     i: 62 encoded letter: e

Analysing the letters, we see that the letters seem to repeat every 18 letters. Since the longest value for key is 14 and we are currently only checking every other letter I assumed the length of the key is 9. We can find the letter for each index of the key by checking every 9 letters. However since we only have every other letter we need to check every 18 letters (only the even ones). For key[0] check index 0, 18, 36, and 54 and see the common letter is 'c'. For key[1] check index 10, 28, 46 and see the common letter is 'j'. For key[2] check index 2, 20, 38, 56 and see the common letter is 'k'. 

Continue for the rest of the key value sand we get the key is: 'cjkojbdbb' or cjkbjbdbb. key[3] could be 'o' or 'b' since index 12, 30 and 48 all have both letters as options. We just need to try both of those options and see which one returns all valid characters. THe final script:

    import string

    LOWERCASE_OFFSET = ord("a")
    ALPHABET = string.ascii_lowercase[:16]
        
    def b16_decode(enc):
        plain = ""
        for c in range(0,len(enc), 2):
            upperhalf = (ord(enc[c])-LOWERCASE_OFFSET)<<4
            lowerhalf = ord(enc[c+1])-LOWERCASE_OFFSET
            plain += chr(lowerhalf+upperhalf)
        return plain

    def unshift(c, k):
        t1 = ord(c) - LOWERCASE_OFFSET
        t2 = ord(k) - LOWERCASE_OFFSET
        t3 = t1-t2
        if t3 < 0:
            t3 += 16
        return ALPHABET[(t3) % len(ALPHABET)]

    encoded = "ilnipdjheipnenhhedionepegiejmleoehejfcnimdgehimnepedhhfbafmcgdek"
    key1 = "cjkojbdbb"
    key2 = "cjkbjbdbb"
    dec1 = ""
    dec2 = ""
    for i, c in enumerate(encoded):
        dec1 += unshift(c, key1[i % len(key1)]) 
        dec2 += unshift(c, key2[i % len(key2)]) 
    flag1 = b16_decode(dec1)
    flag2 = b16_decode(dec2)
    print(flag1)
    print(flag2)

The output with no invalid characters is: `b7bf6c4d2e3c7715489723f360f8d128`

The flag: `picoCTF{b7bf6c4d2e3c7715489723f360f8d128}`

*_Taya*