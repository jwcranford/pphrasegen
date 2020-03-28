![Java CI](https://github.com/jwcranford/pphrasegen/workflows/Java%20CI/badge.svg?branch=master)

# pphrasegen

pphrasegen is a command-line tool to generate passphrases, based on the 
diceware method described at http://www.diceware.com.  Instead of using 
dice to generate a random passphrase, pphrasegen uses
a cryptographically secure random number generator, as briefly described
at http://world.std.com/%7Ereinhold/dicewarefaq.html#computer.


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


## Why another passphrase generator?

- I like the diceware approach and explanation for generating passphrases.
- I want an off-line solution whose security I can verify.  I can't verify a
web-based solution, even if it's based on open-source software, as I can't 
verify the deployment.


## Usage

pphrasegen requires Java 8 or higher. Download and install the JRE from
https://java.com if you haven't already.

_TODO: replace this section with the actual help message._
```
Usage: pphrasegen [-hV] [-c=<count>] [-w=<wordCount>] <wordFile>
Generates passphrases based on a given word file.
      <wordFile>            The file of words to use when generating a
                              passphrase. The file contains one word per line.
  -c, --count=<count>       Number of passphrases to generate (20 by default)
  -h, --help                Show this help message and exit.
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

The default number of words in a passphrase was designed to get at least
75 bits of entropy in each passphrase, according to the recommendation 
at http://world.std.com/~reinhold/dicewarefaq.html#howlong.


## What is entropy?

From http://world.std.com/~reinhold/dicewarefaq.html#entropy:

> Entropy tells how hard it will be to guess the passphrase itself even 
  if an attacker knows the method you used to select your passphrase. A 
  passphrase is more secure if it is selected using a method that has 
  more entropy.
>
> Entropy is measured in bits. The outcome of a single coin toss -- 
  "heads or tails" -- has one bit of entropy.


## Word list

The diceware8k.txt file from www.diceware.com is distributed with pphrasegen
under the Creative Commons CC-BY 4.0 license 
(https://creativecommons.org/licenses/by/4.0/). See the LICENSE file for details.

<table>
<tr>
    <th>UE: Usable Entries</th>                     <td>8192</td>
</tr><tr>
    <th>E: Entropy bits per word [log2(UE)]</th>    <td>13</td>
</tr><tr>
    <th>A: Average word length</th>                 <td>4.1</td>
</tr><tr>
    <th>W: # words to get at least 75 entropy bits [ceiling(75/E)]</th> <td>6</td>
</tr><tr>
    <th>Average passphrase length for at least 75 entropy bits [A*W]</th>   <td>25</td>
</tr>
</table>

Alternative word lists can be constructed from the standard diceware
files at www.diceware.com and at  
https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases.


## How to verify the security of a word list

For full security, a word list should have no duplicates 
(see http://world.std.com/%7Ereinhold/dicewarefaq.html#tampered).

Verifying this condition is a simple matter at a Unix command-line; 
`sort file |uniq -d` shows the duplicate entries from the file, and there 
shouldn't be any.


## Building from source

Gradle and Java 8+ are required to build pphrasegen.

$ ./gradlew build
