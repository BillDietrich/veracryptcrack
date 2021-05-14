# veracryptcrack

Simple slow VeraCrypt container cracker using wordlist/dictionary.  Linux-only.

You don't need to know the encryption algorithm settings of the VeraCrypt volume to decrypt it.

When password is cracked - container will be mounted.

To check everything works correctly before spending more time, run script against veracrypt test.container, it shouldn't take more than 1 minute (because the third line in the wordlist is the correct password).

May be useful if you forget PIM, which keyfile you used, or mixed up a few characters in a password.

When done, use 'wipe' or 'srm' to securely overwrite your wordlist.

Good luck recovering.


## Notes by Bill Dietrich

### Source

Originally from https://github.com/RhZjQyMWI/veracryptcrack

Found it from https://pay.reddit.com/r/VeraCrypt/comments/fatkq2/forgot_my_password_earlier_today_so_wrote/ by /u/IndependentHorror5 :

> Managed to get back into the container within ~45 minutes of automated cracking which was preceded with ~2 hours of manual trying.
>
> This was not a pure bruteforce, but rather speeding up based on some possibly known parameters about the container which was not opened for a while.
>
> Knew there was a PIM but did not know which one and a long password which I couldn't remember clearly. So i've added 20-25 possible PIM combinations to PIM list, numbers that have some meaning to me and generated a wordlist from multiple combinations of passwords I could have used for that container. wordlist.txt had around 500 or more combinations. Came back from lunch and saw a warning that the volume can't be mounted, because it's already mounted. :)

### Run via:

```shell
chmod u+x veracrypt_crack.sh

# Make sure you have necessary software installed:
timeout --version
veracrypt --text --version

sudo ./veracrypt_crack.sh

# On my slow laptop, with 5-second timeout, it ran for 52 minutes
# and failed to open the test container.  Wordlist has 208 passwords
# in it, multiplied by 3 PIM values for each password.

# Changed timeout to 10 seconds and tried again.
# Worked in 3 minutes.
# Spoiler: test.container PIM is "1337", password is "crackmeifyoucan".
# And once mounted, can see hash is "HMAC-SHA-512 (Dynamic)"
# and encryption is "AES-Twofish-Serpent".
```

### Improvements made

* Check return code from VeraCrypt, stop when succeed.
* Made output clearer.
* Changed from bash to sh so ctrl+C works.


#### Notes

5/2021 I'm told that the code doesn't work properly on a TrueCrypt container.  The return codes from VeraCrypt must differ for VeraCrypt and True Crypt containers ?  Comment out the return-code-checking to use with trueCrypt containers, maybe.

```shell
veracrypt --text --help

veracrypt --text test.container /media/veracrypt7
# returns 0 if successfully mounts, 1 if already mounted, 124 if password wrong
```

Relevant: https://github.com/NorthernSec/VeraCracker


## Privacy Policy

This code doesn't collect, store or transmit your identity or personal information in any way.
