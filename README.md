# Dynamic Analysis in Mobile Security

Recover a hidden flag from the APK `com.holberton.task1_d` using dynamic analysis (Frida, ADB, Objection).

The flag is an obfuscated byte array in `MainActivity`, XOR-decoded against a seeded PRNG.
Only the correct seed yields text starting with `Holberton{`. Hooking `generateStringFromSeed`
and sweeping seeds 0-1000 with Frida resolves at seed 837.

Flag is in `0-flag.txt`.
