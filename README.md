# List of All Pakistan Mobile Numbers
Major mobile carriers in Pakistan along with their carrier code as as follows

|  Carrier  |  Carrier Codes  |
| --- | ---|
|Jazz| 0300, 0301, 0302, 0303, 0304, 0305, 0306, 0307, 0308, 0309|
|Zong | 0311, 0312, 0313, 0314, 0315, 0316, 0317, 0318 |
|Warid | 0320, 0321, 0322, 0323, 0324, 0325 |
|UFone | 0330, 0331, 0332, 0333, 0334, 0335, 0336, 0337 |
|Telenor | 0340, 0341, 0342, 0343, 0344, 0345, 0346, 0347 |
|SCOM | 0355 |

**Click on the image to watch YouTube video**

[![How to Hack Wifi?](https://i.ytimg.com/vi/tgWkfi6gIUg/maxresdefault.jpg)](https://www.youtube.com/watch?v=tgWkfi6gIUg)

## Download
Github has a limit of 100MB on a single file in code repository, so I have to zip the files using `7z`. See the section below on installing `7z` on Mac.
```sh
git clone https://github.com/hack0x90/list-of-all-pakistan-mobile-numbers.git
cd list-of-all-pakistan-mobile-numbers
# Uzip all 7z files
FILES=($" *.7z")
for f in $FILES; do 7z e $f ; done

# Optionally merge all files into one
cat *.txt >> pak-mobile-numbers-lists.txt
```

### Installing 7z on Ubuntu
```bash
sudo apt install p7zip-full
```

### Installing 7z on Mac using Homebrew
```bash
brew install p7zip

```
## Anatomy of Mobile Number
Mobile numbers in Pakistan are 11 digits long. First 4 digits are the carrier code, and the remaining 7 diits is the phone number.
```
Carrier Code + Phone Number
(4 digits)   + (7 digits )
0300         + 1234567
```
## Methodology
To generate a comprehensive list of all possible mobile numbers, I will go through each carrier and create a `hashcat` mask and genrate list of all possible combinations.
First three digits of the carrier code are fixed for each carrier, only the forth digit changes.

```
# Jazz
First 3 digits are 030
Fourth digit ranges from 0 to 9
Mask → 030?d?d?d?d?d?d?d?d

# Zong
First 3 digits are 031
Fourth digit ranges from 1 to 8
Since `d` cannot be used for the fourth digit, custom charset has to be created.
Mask → -1 12345678 031?1?d?d?d?d?d?d?d

# Warid
First 3 digits are 032
Fourth digit ranges from 0 to 5
Mask → -1 012345 032?1?d?d?d?d?d?d?d

# UFone
First 3 digits are 033
Fourth digit ranges from 0 to 7
Mask → -1 01234567 033?1?d?d?d?d?d?d?d

# Telenor
First 3 digits are 034
Fourth digit ranges from 0 to 7
Maks → -1 01234567 034?1?d?d?d?d?d?d?d

# SCOM
First 4 digits are fixed as 0355
Mask → 0355?d?d?d?d?d?d?d
```
## Generating the List
Following `hashcat` commands will produce the comprehensive list of all Pakistan mobile phone numbers.
```bash
# Jazz list
hashcat -a 3 --stdout "030?d?d?d?d?d?d?d?d" >> jazz_numbers.txt

# Zong list
hashcat -a 3 --stdout -1 12345678 "031?1?d?d?d?d?d?d?d" >> zong_numbers.txt

# Warid list
hashcat -a 3 --stdout -1 012345 "032?1?d?d?d?d?d?d?d" >> warid_numbers.txt

# UFone list
hashcat -a 3 --stdout -1 01234567 "033?1?d?d?d?d?d?d?d" >> ufone_numbers.txt

# Telenor list
hashcat -a 3 --stdout -1 01234567 "034?1?d?d?d?d?d?d?d" >> telenor_numbers.txt

# SCOM list
hashcat -a 3 --stdout "0355?d?d?d?d?d?d?d" >> scom_numbers.txt
```
