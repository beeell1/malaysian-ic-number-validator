# Malaysian IC (MyKad) Number Validator
This is a simple page to demonstrate the checksum implementation of the Malaysian IC (MyKad) Number.

**The Malaysian IC Number Format**
----------------------------------

The newest version of the Malaysian IC Number consists of 12 digits as shown below:

**YYMMDD-PB-###G**

Where **YYMMDD** is the date of birth of the person (may differ in exceptional circumstances), **PB** is the code for the place of birth of the person (either the state in Malaysia or country if born abroad), **###G** is a unique number assigned by JPN, where G represents the gender of the person, odd being male, and even being female.

**There's a checksum?**
-----------------------

As of September 2023, there is no publicly known checksum that is implemented in the Malaysian IC Number, unlike the Singaporean IC Number. [See LYN's post on this matter.](https://www.lowyat.net/2017/148361/data-breach-fallout-time-review-malaysian-mykad-number/#:~:text=There%20are%20only%203%20random%20numbers%20in%20the%20MyKad%2C%20which%20in%20itself%20doesn%E2%80%99t%20actually%20hold%20any%20kind%20of%20checksum.)

However, through trial and error, it is possible that the Malaysian IC Number does implement a checksum, namely the [ISO 7064 Mod 11,2 checksum algorithm](https://en.wikipedia.org/wiki/ISO/IEC_7064). This algorithm is also used in China's Resident Identity Card Number.

Through tests done on publicly available lists of Malaysian IC Numbers, it is highly likely that the ISO 7064 Mod 11,2 checksum algorithm is used to calculate the last digit of the IC number.

**Checksum and Gender?**
------------------------

One might ask, how would the last digit of the Malaysian IC Number not only serve as the checksum, but also identify the gender? The ISO standardized checksum surely must be only dependent on the first 11 digits, and an additional parameter should be impossible, right?

One theory is that JPN's system sequentially iterates over possible unassigned IC numbers until an IC Number with the corresponding gender is found.

For example, a person born on 1 January 2000 in Sarawak would have an IC prefix of 000101-13-####. JPN's system would generate IC numbers from sequence 1 (suffix 001#), and calculate the corresponding checksum (in this case, 6). Therefore, the potential IC number would end with 0016.

However, if the person is male, this IC would not be assigned to him due to the last digit being an even number. The system would try again with the next possible sequence, until an IC Number with an odd last digit is found (0024, 0032, 0040, 0059). Therefore, the actual IC Number assigned to the person would end in 0059, providing that the number is not previously assigned to someone else.

This would also explain the issue with the MOD11 nature of the checksum. Being MOD11, possible checksum results range from 0 (zero) to 10 (ten). China's Resident Identity Card number uses the character X to represent a checksum of 10. In the Malaysian IC Number however, numbers resulting in a checksum of 10 is simply ignored and skipped.

**Why isn't this public?**
--------------------------

We don't know. Unlike Singapore, where ICA has publicly acknowledged the existence of the checksum letter (but however does not disclose the actual algorithm, though it has been reverse-engineered), Malaysia's JPN has not acknowledged the existence of any checksum.

Therefore, one is cautioned to not exclusively rely on the algorithm (or this site) to validate an IC number as of now.

**Does this work for MyPR/MyKAS?**
----------------------------------

This algorithm is tested on MyKad (Malaysian Citizen) numbers, as it is easier to obtain for testing.

MyPR (Malaysian Permanent Residents) and MyKAS (Malaysian Temporary Residents) are harder to come by, and are not thoroughly validated to work with this algorithm.

However, it is assumed that the algorithm also works with MyPR/MyKAS numbers.

**Is this legal?**
------------------

We are not lawyers, but:

Unlike source code, algorithms cannot be copyrightable. Therefore, the use of the algorithm to check the validity of Malaysian IC Numbers therefore should be legal (assuming that this algorithm is correct). However, it is definitely illegal to use this knowledge for malicious or nefarious purposes, such as impersonation or fraud.

And of course, use at your own risk.

**Conclusion**
--------------

This site was created to demonstrate the validation of Malaysian IC Numbers with the ISO 7064 Mod 11,2 checksum algorithm.

The use of this specific algorithm was discovered while trying to figure out a possible form validation method for Malaysian IC numbers.

Note that this site does not validate or verify whether an IC number is assigned to a person, rather that it fits the format which IC numbers have.

It would be tremendously useful not only for software developers, but also typical users to be able to validate and verify the integrity of an IC number, especially to prevent inadvertent typos.

Therefore, it would be beneficial if JPN acknowledges the existence of the checksum algorithm used in Malaysian IC Numbers.
