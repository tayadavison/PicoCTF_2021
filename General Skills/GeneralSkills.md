# General Skills

*Solved: 7/7, Points: 110*
| Challenges |
| ---- |
| [Obedient Cat](#obedient-cat-5-pts) |
| [Python Wrangling](#Python-Wrangling-10-pts) |
| [Wave a flag](#Wave-a-flag-10-pts) |
| [Nice netcat…](#Nice-netcat-15-pts) |
| [Static ain’t always noise](#Static-aint-always-noise-20-pts) |
| [Tab, Tab, Attack](#Tab-Tab-Attack-20-pts) |
| [Magikarp Ground Mission](#Magikarp-Ground-Mission-30-pts) |

## Obedient Cat (5 pts) 
(*Taya*)

    This file has a flag in plain sight (aka "in-the-clear"). Download flag
Problem gives “flag” file. Download it and it has the flag:
`picoCTF{s4n1ty_v3r1f13d_f28ac910}` 


## Python Wrangling (10 pts) 
(*Tiare*)

    Python scripts are invoked kind of like programs in the Terminal... Can you run this Python script using this password to get the flag? Hint: Get the Python script accessible in your shell by entering the following command in the Terminal prompt: $ wget https://mercury.picoctf.net/static/0bf545252b5120845e3b568b9ad0277e/ende.py
Problem gives: encrypted flag txt file, “password” txt file and python script

Running the command prompt that is given in the hint uploads python script to directory saved as `ende.py` file

Running `$ python ende.py -h` results in this output:

    Usage: ende.py (-e/-d) [file]
    Examples:
        To decrypt a file named 'pole.txt', do: '$ python ende.py -d pole.txt'
To get flag, save the encrypted flag file from problem as “pole.txt” in directory, then run `$ python ende.py -d pole.txt` 
The program then asks for the password; once the text from the “password.txt” file is imputed, the flag is outputted: `picoCTF{4p0110_1n_7h3_h0us3_6008014f} `
_Tiare

## Wave a flag (10 pts)
(*Tiare*)

    Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information…


    Hint: To get the file accessible in your shell, enter the following in the Terminal prompt: $ wget https://mercury.picoctf.net/static/b28b6021d6040b086c2226ebeb913bc2/warm
    Hint: Run this program by entering the following in the Terminal prompt: $ ./warm, but you'll first have to make it executable with $ chmod +x warm
    Hint: -h and --help are the most common arguments to give to programs to get more information from them!
The hints basically tell you what to do.
In the webshell, run: `$ wget https://mercury.picoctf.net/static/b28b6021d6040b086c2226ebeb913bc2/warm` (the link can also be copied from “This program” link from the problem description)
Then to make file executable: `$ chmod +x warm`
`$ ./warm` outputs the following text: `Hello user! Pass me a -h to learn what I can do!`
Then, as expected `$ ./warm -h` gives the flag:
`Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_d6969390}`

## Nice netcat… (15 pts)
(*Tiare*)

    There is a nice program that you can talk to by using this command in a shell: $ nc mercury.picoctf.net 22342, but it doesn't speak English…

In the webshell, run `$ nc mercury.picoctf.net 22342` which outputs numbers separated by newlines `(112 105 99 111 67 84 70 123 103 48 48 100 95 107 49 116 116 121 33 95 110 49 99 51 95 107 49 116 116 121 33 95 53 102 98 53 101 53 49 100 125 10)`
Using a [ASCII converter](https://www.browserling.com/tools/ascii-to-text), or coding one yourself like a boss 😎, the numbers turn into characters of the flag: `picoCTF{g00d_k1tty!_n1c3_k1tty!_5fb5e51d}`

## Static ain’t always noise (20 pts)
(*Taya*)
    
    Can you look at the data in this binary: static? This BASH script might help!

Probably a better way to do this but I just downloaded the “static” file opened it in notepad and the flag was there: 
`picoCTF{d15a5m_t34s3r_98d35619}`


## Tab, Tab, Attack (20 pts)
(*Taya*)

    Using tabcomplete in the Terminal will add years to your life, esp. when dealing with long rambling directory structures and filenames: Addadshashanammu.zip

Probably a better way to do this using the terminal but I just downloaded the `Addadshashanammu.zip` folder and then clicked through each folder until I got to the file `fang-of-haynekhtnamet` at the end. Opened in notepad and searched for picoCTF to find the flag: 
`picoCTF{l3v3l_up!_t4k3_4_r35t!_f3553887}`


## Magikarp Ground Mission (30 pts)
(*Taya*)

    Do you know how to move between directories and read files in the shell? Start the container, `ssh` to it, and then `ls` once connected to begin. Login via `ssh` as `ctf-player` with the password, `481e7b14`
Launch challenge instance and follow the instructions `ssh ctf-player@venus.picoctf.net -p 49615` and password: `481e7b14` then run `$ ls` and see 2 files: `1of3.flag.txt`  `instructions-to-2of3.txt`.

1. Run `$ cat 1of3.flag.txt` and the output is: `picoCTF{xxsh_` Part 1 of the flag!
1. Run `$ cat instructions-to-2of3.txt` and the output is: `Next, go to the root of all things, more succinctly '/'`
1. Run `$ cd /`
1. Run `$ ls` and get a lot of files in the output including `2of3.flag.txt` and  `instructions-to-3of3.txt`.
1. Run `$ cat 2of3.flag.txt` and the output is: `0ut_0f_\/\/4t3r_` Part 2 of the flag!
1. Run `$ cat instructions-to-3of3.txt` and the output is: `Lastly, ctf-player, go home... more succinctly '~'`
1. Run `$ cd ~`
1. Run `$ ls` and see file: `3of3.flag.txt`.
1. Run `$ cat 3of3.flag.txt`  and the output is: `1118a9a4}` Part 3 of the flag!

Put it together to get the flag: `picoCTF{xxsh_0ut_0f_\/\/4t3r_1118a9a4}`
