![Java CI](https://github.com/jwcranford/pphrasegen/workflows/Java%20CI/badge.svg?branch=master)

# pphrasegen

pphrasegen is a command-line tool to generate passphrases, based
on the diceware method described at http://www.diceware.com.  Instead
of using dice, pphrasegen uses a cryptographically secure random
number generator, as briefly described at
http://world.std.com/%7Ereinhold/dicewarefaq.html#computer, to pick
random words from a given word list to compose a passphrase.


## Why passphrases?

TL;DR - Go read this XKCD comic: https://xkcd.com/936/

From http://www.diceware.com:

> Passphrases differ from passwords only in length. Passwords are
  usually short, six to ten characters.... Passphrases are usually
  much longer -- typically 25 to 64 characters (including
  spaces). Their greater length makes passphrases more secure. Modern
  passphrases were invented by Sigmund N. Porter in 1982.
>
> Picking a good passphrase is one of the most important things you
  can do to preserve the privacy of your computer data and e-mail
  messages. A passphrase should be:
>
> - Known only to you
> - Long enough to be secure
> - Hard to guess -- even by someone who knows you well
> - Easy for you to remember
> - Easy for you to type accurately


Research published in 2012 also found that "a password-composition
policy requiring long passwords with no other restrictions provides
(relative to other tested policies) excellent resistance to guessing"
(Kelley, Patrick Gage, Saranga Komanduri, Michelle L Mazurek, Richard
Shay, Timothy Vidas, Lujo Bauer, Nicolas Christin, Lorrie Faith
Cranor, and Julio Lopez. “Guess Again (and Again and Again): Measuring
Password Strength by Simulating Password-Cracking Algorithms.” In
Security and Privacy (SP), 2012 IEEE Symposium On, 523–537. IEEE,
2012. Available at:
http://ieeexplore.ieee.org/iel5/6233637/6234400/06234434.pdf).

Citing that paper among many others, the U.S. government revised
their password recommendations in 2017 to emphasize password length
and memorability over password complexity. From NIST SP 800-63B
(https://pages.nist.gov/800-63-3/sp800-63b.html):

> Password length has been found to be a primary factor in characterizing 
> password strength.... Users should be encouraged to make their passwords 
> as lengthy as they want, within reason. Since the size of a hashed password 
> is independent of its length, there is no reason not to permit the use of 
> lengthy passwords (or pass phrases) if the user wishes. 


## Why another passphrase generator?

- I like the diceware approach and explanation for generating passphrases.
- I want an off-line solution whose security I can verify.  I can't verify a
web-based solution, even if it's based on open-source software, as I can't 
verify the deployment.


## Usage

pphrasegen requires Java 8 or higher. Download and install the JRE from
https://java.com if you haven't already.

```
Usage: pphrasegen [-hV] [-c=<count>] [-d=<digits>] [-f=<wordFile>]
                  [-s=<specialChars>] [-w=<wordCount>]
Generates passphrases based on a given word file.
  -c, --count=<count>       Number of passphrases to generate (5 by default).
  -d, --digits=<digits>     Number of digits to insert at random locations in
                              the generated passphrase.
  -f, --file=<wordFile>     The word file to use when generating a passphrase.
                              The file should contain one word per line. If not
                              specified, an internal copy of diceware8k.txt is
                              used.
  -h, --help                Show this help message and exit.
  -s, --special=<specialChars>
                            Number of special characters to insert at random
                              locations in the generated passphrase.
  -V, --version             Print version information and exit.
  -w, --words=<wordCount>   Number of words to include in each passphrase.
                            By default, the number of words in each passphrase
                              depends on the size of the input file.
                                  # of words in file   # of words in passphrase
                                  ------------------   ------------------------
                                                1024                          8
                                                2048                          7
                                                4096                          7
                                                8192                          6
```

Note that both a shell script and a batch file is available in the bin
directory.


## On the strength of passphrases generated by pphrasegen

To guarantee a uniform distribution of random numbers, pphrasegen uses the 
largest prefix of the selected word list that is a whole power of two in 
length, following the guidance at 
http://world.std.com/~reinhold/dicewarefaq.html#computer.

Examples:
* The diceware8k.txt file distributed with pphrasegen contains exactly 8192 
  words, so the entire file is used.
* If another file were specified at the command-line with slightly fewer words,
  then only the first 4096 lines of the file would be used. 


## Word list

The diceware8k.txt file from www.diceware.com is distributed with
pphrasegen under the Creative Commons CC-BY 4.0 license
(https://creativecommons.org/licenses/by/4.0/). See the LICENSE
file for details.

<table>
<tr>
    <th>Usable Entries</th>                     <td>8192</td>
</tr><tr>
    <th>Average word length</th>                 <td>4.1</td>
</tr>
</table>

Alternative word lists can be constructed from the standard diceware
files at www.diceware.com, from the EFF diceware lists at 
https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases, 
or just by pulling words from /usr/share/dict/words on a Unix-like computer.
A word list is simply a text file with one word per line, so 
customizing and experimenting with multiple word lists is 
rather straight-forward.


## How to verify the security of a word list

For full security, a word list should have no duplicates 
(see http://world.std.com/%7Ereinhold/dicewarefaq.html#tampered).

Verifying this condition is a simple matter at a Unix command-line; 
`sort file |uniq -d` shows the duplicate entries from the file, and there 
shouldn't be any.


## Additional security tips

* Use a password manager.
* Use two-factor authentication where available.


## Building from source

Java 8+ is required to build pphrasegen from source.

$ ./gradlew build
