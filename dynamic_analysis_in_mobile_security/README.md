# Dynamic Analysis in Mobile Security

Recover a hidden flag from the provided APK (`com.holberton.task1_d`) using dynamic analysis.

## Analysis
The flag is an obfuscated byte array XOR-decoded against a seeded PRNG in `MainActivity`:

```java
Random random = new Random(seed);
for (int i = 0; i < obfuscatedFlagData.length; i++)
    sb.append((char) (obfuscatedFlagData[i] ^ random.nextInt(256)));
```

Only the correct seed yields readable text starting with `Holberton{`.

## Solution (Frida)
Hook `generateStringFromSeed` and sweep seeds 0–1000:

```javascript
Java.perform(function () {
    var act = null;
    Java.choose("com.holberton.task1_d.MainActivity", { onMatch: function (i) { act = i; }, onComplete: function () {} });
    for (var seed = 0; seed <= 1000; seed++) {
        var s = act.generateStringFromSeed(seed);
        if (s.indexOf("Holberton{") === 0) { console.log("seed=" + seed + " flag=" + s); break; }
    }
});
```

```bash
adb install task0_d.apk
frida -U -f com.holberton.task1_d -l hook.js
```

Resolves at **seed 837**.

## Flag
See `0-flag.txt`.
