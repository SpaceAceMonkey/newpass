# newpass

Newpass is a bash script for generating passphrases. I wrote this with Okta in mind, but its wide array of options makes it appropriate for generating passphrases for use anywhere.

#### Included in this repository
- enable_with_additions.txt
-- This list is made up of the public domain ENABLE word list, along with some additions used by popular word games.
- newpass
-- This is a bash script that will generate passphrases using a wide variety of options, and an optional word list.

#### Installation
Clone this repository, and make newpass executable.

```git clone git@github.com:SpaceAceMonkey/newpass.git```

```cd newpass```

```chmod +x newpass```

Optionally, move newpass into a directory within your PATH, so you don't have to specify its location each time you use it.

### Usage
Omit ./ from the following commands if you have moved newpass into your PATH. Otherwise, this guide assumes you are working from within the same directory as newpass.

**Display help**

```./newpass -h```

```
Usage: newpass [options ...]
Available switches:
-a|--allspace: insert DELIMITER between all elements of the passphrase.
-b|--bashrandom: Use Bash's \$RANDOM to generate random numbers, rather than using od to format output from /dev/urandom. This is intended as a backup to od and /dev/urandom, and is subject to Bash's \$RANDOM limitations.
-c|--wordcount "n" [4]: Use n words from WORDLIST when generating passphrase.
-d|--delimiter "DELIMITER" [space]: Set DELIMITER to the string specified in "DELIMITER."
-f|--capfirst: Capitalize the first letter of each word in the passphrase.
-F|--randomcapfirst: Randomly capitalize the first letter of some words in passphrase. See also -r|--randomcappercentage.
-k|--symbolcount "n" [1]: Use n symbols from SYMBOL_STRING when generating passphrase.
-K|--symbolstring "SYMBOL_STRING" [!@#$%^&*()]: Set SYMBOL_STRING to the value in "SYMBOL_STRING." The characters in this string will be used when inserting symbols into the passphrase.
-l|--minwordlength "n" [2]: Filter WORDLIST to use no words shorter than n letters.
-L|--maxwordlength "n" [6]: Filter WORDLIST to use no words longer than n letters.
-m|--numbermin "n" [10]: Use numbers no smaller than n when adding numbers to the passphrase.
-M|--numbermax "n" [98]: Use numbers no larger than n when adding numbers to the passphrase. This number cannot exceed 32767.
-n|--numbercount "x" [1]: Add x numbers to the passphrase.
-q|--quiet: Suppress all output except for the generated passphrase.
-r|--randomcappercentage "n" (0-100) [50]: When applying random capitalization transforms, capitalize a letter n percent of the time.
-R|--randomcap: Apply random capitalization to all letters in each word, using --randomcappercentage.
-s|--wordspace: Insert DELIMITER between words in the passphrase.
-S|--numberspace: Insert DELIMITER between numbers in the passphrase.
-w|--wordlist "WORDLIST" [enable_with_additions.txt]: Use the file specified in WORDLIST to select words for the passphrase.
-z|--symbolspace: Insert DELIMITER between symbols in the passphrase.
```

**Generate a password using the default settings (four words 2 - 6 characters in length, one symbol, and one number from 10 - 98)**

```./newpass```
```
Parsing word list.
Generating passphrase using 4 word(s) from a pool of 57951 words, 1 number(s) in the range of 10 to 98, and 1 symbol(s) selected from !@#$%^&*().

85^abasthrowsmorbouton
```

**Generate a password with six words and two symbols, using spaces between each element**

```./newpass -c 6 --allspace -n 0 -k 2```
```
Parsing word list.
Generating passphrase using 6 word(s) from a pool of 57951 words, 0 number(s), and 2 symbol(s) selected from !@#$%^&*().

* ardeb ) stern airest curb remelt blazed
```

**Generate a password consisting of four numbers between 100 and 30000, separated by hyphens**

```./newpass -c 0 --allspace -d - -n 4 -k 0 -m 100 -M 30000```
```
Parsing word list.
Generating passphrase using 0 word(s), 4 number(s) in the range of 100 to 30000, and 0 symbol(s).

28986-15447-12454-18503
```

**Generate a passphrase using ten two-letter words separated by spaces, and suppress all output except for the passphrase, itself**

```./newpass -c 10 -k 0 -n 0 --allspace -d " " -l 2 -L 2 -q```
```
us fe ti ex id er up is in ho
```

**Generate a passphrase using default settings, but capitalize the first letter of each word**

```./newpass --capfirst```
```
Parsing word list.
Generating passphrase using 4 word(s) from a pool of 57951 words, 1 number(s) in the range of 10 to 98, and 1 symbol(s) selected from !@#$%^&*().

65(MidgesVarierKiddieHouser
```

**Generate a passphrase using six words of 4-8 characters in length, separated by spaces, with the first letter of each word having a 50% chance of being capitalized**

```./newpass -l 4 -L 8 -c 6 -k 0 -n 0 --wordspace --randomcapfirst -r 50```
```
Parsing word list.
Generating passphrase using 6 word(s) from a pool of 158881 words, 0 number(s), and 0 symbol(s).

Bens hibernal Bast Animus ethion gumweed
```

**Select a single sixteen-character-long word with one symbol and one number >= 1000**

```./newpass -l 16 -L 16 -c 1```
```
Parsing word list.
Generating passphrase using 1 word(s) from a pool of 3887 words, 1 number(s) in the range of 1000 to 4294967295, and 1 symbol(s) selected from !@#$%^&*().

%refractorinesses49
```

**Generate a passphrase consisting of a single word of <= 4 characters in length, and a single number greater than 1,000,000,000, separated by a hyphen**

```./newpass -c 1 -L 4 -n 1 -k 0 -m 1000000000 -d - --allspace```
```
Parsing word list.
Generating passphrase using 1 word(s) from a pool of 10074 words, 1 number(s) in the range of 1000000000 to 4294967295, and 0 symbol(s).

1511339847-ibex
```

**Generate a passphrase from two words >= 20 characters in length, two numbers <= 10, and two symbols from the default symbol set, with all elements separated by underscores**

```./newpass -c 2 -l 20 -n 2 -M 10 -k 2 -d _ --allspace```
```
Parsing word list.
Generating passphrase using 2 word(s) from a pool of 559 words, 2 number(s) in the range of 0 to 10, and 2 symbol(s) selected from !@#$%^&*().

paradichlorobenzenes_&_6_spectroheliographies_#_10
```

**Generate a passphrase using two words of at least ten characters each, four symbols selected from =+-_[]{}, four numbers from 10 - 1000, while randomly capitalizing letters with a 20% probability, suppressing all output outher than the passphrase, and putting spaces between words, but not between other elements**

```./newpass -c 2 -l 10 -k4 -K '=+-_[]{}' -n 4 -m 10 -M 1000 -R -r 20 -q --wordspace```
```
468621932994sPortivenesSes ForEsiGhtednesS]
```




