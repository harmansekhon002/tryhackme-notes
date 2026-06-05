- In this room, you will learn that characters are just numbers with agreed meanings. That agreement is called an **encoding**
- encoding is the specific, agreed-upon mapping between numbers and meanings, such as which number corresponds to the character “A”.
## ASCII
- American Standard Code For Information 
- it is from 0 to 127 to represent english letters digits punctuations and some control characters 
- A stands for american 
- a is representated by 61 b for 62 and c for 63
- A is representated by 41 B for 42 and C for 43
## European Languages

ASCII provided a way to encode the English alphabet; however, we need an encoding to support other European languages such as Spanish (ñ, ¿), German (ß, ü), Polish (ł, ń), Czech (č, ř), and Romanian (ș, ț), to name a few. ASCII uses 7 bits, and with an eighth bit, we get 128 more characters to cover. However, the reality is more challenging; the additional 128 characters are not enough to cover all the letters of the European languages. The ISO/IEC 8859 Series (International Standards) created several standards; each standard covered a set of languages:

- **ISO-8859-1 (Latin-1)**: Covered **Western European languages** like German (ß, ü), French (é, ç), Spanish (ñ, ¿), Italian, Portuguese, Catalan, and Nordic languages (e.g., Icelandic ð/Ð). Check this [link(opens in new tab)](https://www.charset.org/charsets/iso-8859-1).
- **ISO-8859-2 (Latin-2)**: Supported **Central/Eastern European languages** like Polish (ł, ń), Czech (č, ř), Hungarian (ő, ű), Croatian (đ), Romanian (ș, ț), and Slovak. Check this [link(opens in new tab)](https://www.charset.org/charsets/iso-8859-2).

In other words, if your document is saved using ISO-8859-1 and later read and displayed as if it were saved using ISO-8859-2, non-English letters are likely to be displayed differently.
## Unicode
- Unicode is a universal character encoding standard. 
- It assigns unique code points to characters from all modern and historical writing systems worldwide. 
- Unicode supports the interchange, processing, and display of text in diverse languages. 
- In other words, we don’t need to worry about picking a specific encoding standard that is compatible with the language we are using. 
- Furthermore, this makes it easy to use different languages in a single file or message.
- And most importantly, because this is a standard that fits all, we don’t need to worry about the encoding used by the original author, as they will also be using Unicode.
- Unicode is a character set standard that assigns a unique number to every character across all languages. Examples:

- U+0041 = Latin “A”
- U+03A9 = Greek “Ω”
- U+3042 = Japanese Hiragana “あ”
## UTF 8
- UTF-8 is very common on the modern web. 
- It encodes Unicode points into 1 to 4 bytes dynamically.
- In other words, it decides on the number of bytes based on the character complexity.
- ASCII characters (`U+0000` to `U+007F`) use exactly 1 byte, identical to the original ASCII, ensuring seamless backward compatibility. 
- Non-ASCII characters like Ω (`U+03A9`) use 2 bytes, while complex scripts or emoji like 🔥 (`U+1F525`) require 4 bytes. 
- This flexibility allows us to cover the Unicode standard without wasting bytes
# UTF 16
- UTF-16 takes a different path; it uses either 2 or 4 bytes per character. 
- Common characters, like most Latin, Cyrillic, or Chinese Hanzi, fit in 2 bytes;
- however, rarer ones, like emoji or ancient scripts, require a pair, i.e., two 16-bit units totaling 4 bytes. 
- For example, the letter `A` is encoded as `U+0041`, while the emoji 🔥 needs two and is encoded as `U+D83D U+DD25`.
## UTF32
Finally, UTF-32 is the simplest but also the most wasteful; every Unicode code point uses exactly 4 bytes. For example, `A` is encoded as `U+00000041` and 🔥 is encoded as `U+0001F525`.
