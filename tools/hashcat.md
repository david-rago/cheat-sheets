Creating MD5 hash in Linux with:
```bash
echo -n "hashcat" | md5sum | awk '{print $1}'
```
`-n` important option to add, avoid adding a newline character
The result in this case is `8743b52063cd84097a65d1633f5c74f5`

Now we are going to use hashcat to crack this MD5 digest (hash value)
```bash
hashcat -a 3 -m 0 -d 2 8743b52063cd84097a65d1633f5c74f5 -O
```
* `-a` attack mode to brute force
* `-m` hash mode, for MD5 is `0`, for more info check with `hashcat -h` 
* `-d` set device, in this case `2` means GPU
* `-O` GPU optimization setting, sets best kernel for GPU

Once you do this command it will try all possible combinations until the hash crack is done.
In my case once cracked it outputted the value `8743b52063cd84097a65d1633f5c74f5:hashcat`

Apply increment
In this case we know the password starts with `Pass` but we don't know the length, we can do the following command to automatically increment the mask with all possible combinations.
```bash
hashcat -a 3 -m 0 -d 2 47b7bfb65fa83ac9a71dcb0f6296bb6e Pass?a?a?a?a?a?a?a?a?a?a?a --increment --increment-min=5 --increment-max=10 -O
```