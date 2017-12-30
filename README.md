# pphrasegen

A Java command-line tool to generate passphrases, based on the diceware
method described at http://www.diceware.com.  While the diceware method 
describes how to generate random passphrases with dice, pphrasegen uses
a cryptographically secure random number generator instead, as briefly described
at http://world.std.com/%7Ereinhold/dicewarefaq.html#computer.


## Why passphrases?

The case for using a passphrase instead of a password is most succintly
made in an XKCD comic at https://xkcd.com/936/.

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
web-based solution, even if it's based on open-source software, as I can't verify
the deployment.
- I want to experiment with different word lists.


## Usage

```
pphrasegen -h | --help
   Prints this summary.
pphrasegen file [w [p]]
   Generates p passphrases of w words each from the given file.
   The default behavior is to generate 20 passphrases with enough words
   each to get at least 75 bits of entropy in each passphrase, according
   to the size of the word list.

      # of words in list   # of words in passphrase
      ------------------   ------------------------
                    1024                          8
                    2048                          7
                    4096                          7
                    8192                          6
```

Note that both a shell script and a batch file is available in the bin
directory.


## How to verify the security of a word list

For full security, a word list should have no duplicates 
(see http://world.std.com/%7Ereinhold/dicewarefaq.html#tampered).

Verifying this condition is a simple matter at a Unix command-line; 
`sort file |uniq -d` shows the duplicate entries from the file, and there 
shouldn't be any.


## On the strength of passphrases generated by pphrasegen

To guarantee a uniform distribution of random numbers, pphrasegen uses the 
largest prefix of the selected word list that is a whole power of two in 
length. 

For the standard diceware-size word lists of 7776 words, pphrasegen
uses only the first 4096 words. This 
approach provides log2(4096) = 12 bits of entropy per word in the passphrase.
A six-word passphrase thus provides 72 bits of entropy, while a seven-word
passphrase provides 84 bits of entropy.

Similary, for the EFF short list of 1296, only the first 1024 words are used. 
This approach yields log2(1024) = 10 bits of entropy
per word, with an 8-word passphrase providing 80 bits of entropy.


## What is entropy?

From http://world.std.com/~reinhold/dicewarefaq.html#entropy:

> Entropy tells how hard it will be to guess the passphrase itself even if an attacker knows the method you used to select your passphrase. A passphrase is more secure if it is selected using a method that has more entropy.
>
> Entropy is measured in bits. The outcome of a single coin toss -- "heads or tails" -- has one bit of entropy.


## Word lists distributed with pphrasegen

<table>
<tr>
    <th>File</th>
    <th>Description</th>
    <th># Entries</th>
    <th>UE: Usable Entries</th>
    <th>E: Entropy bits per word [log2(UE)]</th>
    <th>A: Average word length</th>
    <th>W: # words to get at least 75 entropy bits [ceiling(75/E)]</th>
    <th>Average passphrase length for at least 75 entropy bits [A*W]</th>
</tr>
<tr>
    <td>diceware.txt and beale.wordlist.txt</td>
    <td>Standard Diceware word list and alternative word list edited by Alan Beale
    (adapted from http://www.diceware.com).
    The original lists were modified to remove any PGP signatures and the mapping to actual dice to obtain
    simply the words from the list.</td>
    <td>7776</td>
    <td>4096</td>
    <td>12</td>
    <td>4.35</td> 
    <td>7</td>
    <td>30.5</td>
</tr>
<tr>
    <td>diceware8k.txt and beale.wordlist.8k.txt</td>
    <td>Diceware 8k word list (from http://world.std.com/%7Ereinhold/dicewarefaq.html#computer).
    beale.wordlist.8k.txt was created by extending beale.wordlist.txt with the same bigrams as were added to diceware8k.txt; see
    http://world.std.com/%7Ereinhold/dicewarefaq.html#diceware8k for details.</td>
    <td>8192</td>
    <td>8192</td>
    <td>13</td>
    <td>4.1</td> 
    <td>6</td>
    <td>25</td>
</tr>
<tr>
    <td>eff_large_wordlist.txt</td>
    <td>EFF Large word list (from https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases),
    modified to remove the mapping to actual dice to obtain simply the words from the list.</td>
    <td>7776</td>
    <td>4096</td>
    <td>12</td>
    <td>7.0</td> 
    <td>7</td>
    <td>49</td>
</tr>
<tr>
    <td>eff_short_wordlist_1.txt</td>
    <td>First EFF Short word list (from https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases),
    modified to
        remove the mapping to actual dice to obtain
        simply the words from the list.</td>
    <td>1296</td>
    <td>1024</td>
    <td>10</td>
    <td>4.5</td> 
    <td>8</td>
    <td>36</td>
</tr>
<tr>
    <td>eff_large_wordlist.8k.txt</td>
    <td>EFF Large word list extended with same bigrams as diceware8k.txt</td>
    <td>8192</td>
    <td>8192</td>
    <td>13</td>
    <td>6.74</td> 
    <td>6</td>
    <td>40.4</td>
</tr>
<tr>
    <td>eff_short_wordlist_1.2k.txt</td>
    <td>First EFF Short word list extended with shortest non-duplicate words from EFF Large word list</td>
    <td>2048</td>
    <td>2048</td>
    <td>11</td>
    <td>4.83</td> 
    <td>7</td>
    <td>33.8</td>
</tr>
<tr>
    <td>eff_short_wordlist_1.bigrams.2k.txt</td>
    <td>First EFF Short word list extended with selected bigrams from diceware8k.txt</td>
    <td>2048</td>
    <td>2048</td>
    <td>11</td>
    <td>3.61</td> 
    <td>7</td>
    <td>25.3</td>
</tr>
<tr>
    <td>eff_short_wordlist_1.pairs.2k.txt</td>
    <td>First EFF Short word list extended with letter and number pairs from diceware.txt</td>
    <td>2048</td>
    <td>2048</td>
    <td>11</td>
    <td>3.61</td>
    <td>7</td>
    <td>25.3</td>
</tr>
</table>

According to https://www.eff.org/deeplinks/2016/07/new-wordlists-random-passphrases,
the EFF lists claims to fix a number of problems with the original Diceware lists:

> - It contains many rare words such as _buret, novo, vacuo_
> - It contains unusual proper names such as _della, ervin, eaton, moran_
> - It contains a few strange letter sequences such as _aaaa, ll, nbis_
> - It contains some words with punctuation such as _ain't, don't, he'll_
> - It contains individual letters and non-word bigrams like _tl, wq, zf_
> - It contains numbers and variants such as _46, 99_ and _99th_
> - It contains many vulgar words
> - Diceware passwords need spaces to be correctly decoded, e.g. _in_ and
_put_ are in the list as well as _input_.

The final two entries in the table above are my attempt to balance average
word length with ease of memorization. eff_short_wordlist_1.bigrams.2k.txt
favors bigrams with one letter and one number, while
eff_short_wordlist_1.pairs.2k.txt favors pairs of two numbers and two
letters.

In the interests of transparency, the following commands were used to add words
to eff_short_wordlist_1.txt:

```
$ cp eff_short_wordlist_1.txt eff_short_wordlist_1.bigrams.2k.txt
$ perl -ne 'print if (/^\d[a-z]$/ || /^[a-z]\d$/ || /^[aeiouyn]\w$/ || /^\w[aeiouyst]$/)' diceware8k.txt  >> eff_short_wordlist_1.bigrams.2k.txt
$ echo pq >> eff_short_wordlist_1.bigrams.2k.txt
$ echo qp >> eff_short_wordlist_1.bigrams.2k.txt

$ cp eff_short_wordlist_1.txt eff_short_wordlist_1.pairs.2k.txt
$ perl -ne 'print if /^\w\w$/' diceware.txt >> eff_short_wordlist_1.pairs.2k.txt
```  

Then 14 letter pairs with q, x, and y were manually removed from eff_short_wordlist_1.pairs.2k.txt
to get precisely 2048 words.

See the LICENSE file for full copyright details on the distributed
word lists.


## To build

Gradle and Java 8 are required to build pphrasegen.

$ gradle build
