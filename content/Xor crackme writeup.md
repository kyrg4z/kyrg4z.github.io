---
draft: "false"
title: Xor crackme writeup
tags:
  - reversing
  - ctf
  - ghidra
  - xor
  - writeup
difficulty: easy
tools: ghidra, python
---

[Link to the crackme](https://crackmes.one/crackme/6946be18523504d8c842eb7e)

This writeup is just a bit higher level understanding of the code. 

Using ghidra we can see the decompiled version of the code. 

First I renamed the vars to the actual keys as shown here. 
![[xor1.png]]

```c
  byte *pbVar1;  // just pointer to the byte of termporary variable 1 
  byte *pbVar2;  // ghidra auto generates names for them 
  byte *pbVar3;
  byte *pbVar4;
  byte *pbVar5;
  long in_FS_OFFSET;
  byte key1 [32]; // renamed variables to the actual keys 
  byte key2 [32];
  byte key3 [32];
  byte key4 [24];
  long canary;
```

Scrolling down we can see a section where all key variables are given a value. 

![[xor2.png]]

<details>
<summary>unsorted code</summary>

```c
key1[0x10] = 0;
pbVar5 = key2;
key2[0x10] = 0;
pbVar1 = *(byte **)(param_2 + 8);
pbVar4 = key3;
pbVar3 = key4;
key3[0x10] = 0;
key4[0x10] = 0;
key1[0] = 0x5e;
key1[1] = 0x36;
key1[2] = 0x32;
key1[3] = 0x28;
key1[4] = 0x41;
key1[5] = 0x79;
key1[6] = 0x26;
key1[7] = 0x33;
key1[8] = 0x60;
key1[9] = 0x72;
key1[10] = 0x37;
key1[0xb] = 0x6a;
key1[0xc] = 0x7c;
key1[0xd] = 0x51;
key1[0xe] = 0x7d;
key1[0xf] = 0x3e;
key2[0] = 0x36;
key2[1] = 0x69;
key2[2] = 0x75;
key2[3] = 0x37;
key2[4] = 0x28;
key2[5] = 0x69;
key2[6] = 0x55;
key2[7] = 0x42;
key2[8] = 0x70;
key2[9] = 0x44;
key2[10] = 0x24;
key2[0xb] = 0x39;
key2[0xc] = 0x4b;
key2[0xd] = 0x6c;
key2[0xe] = 0x49;
key2[0xf] = 0x43;
key3[0] = 0x3a;
key3[1] = 0x76;
key3[2] = 0x54;
key3[3] = 0x33;
key3[4] = 0x3f;
key3[5] = 0x5b;
key3[6] = 0x5a;
key3[7] = 0x7d;
key3[8] = 99;
key3[9] = 0x56;
key3[10] = 0x27;
key3[0xb] = 0x6f;
key3[0xc] = 0x66;
key3[0xd] = 0x38;
key3[0xe] = 0x3f;
key3[0xf] = 0x43;
key4[0] = 0x33;
key4[1] = 0x4b;
key4[2] = 0x70;
key4[3] = 0x2a;
key4[4] = 0x33;
key4[5] = 0x2b;
key4[6] = 0x4e;
key4[7] = 100;
key4[8] = 0x6a;
key4[9] = 0x78;
key4[10] = 0x5f;
key4[0xb] = 0x29;
key4[0xc] = 0x40;
key4[0xd] = 0x6b;
key4[0xe] = 100;
key4[0xf] = 0x4e;
```
</details>


if we sort them out we get 
```
Key 2: [94, 54, 50, 40, 65, 121, 38, 51, 96, 114, 55, 106, 124, 81, 125, 62]
Key 3: [51, 75, 112, 42, 51, 43, 78, 100, 106, 120, 95, 41, 64, 107, 100, 78]
Key 4: [58, 118, 84, 51, 63, 91, 90, 125, 99, 86, 39, 111, 102, 56, 63, 67]
Key 5: [54, 105, 117, 55, 40, 105, 85, 66, 112, 68, 36, 57, 75, 108, 73, 67]
```


Now after the keys we can see a loop down. 
![[xor4.png]]

This is verification loop. It compares each byte of the input versus a computed value derived from four internal key arrays (key2, key3, key4, key5)
Important note is that key1 is the user input. (pbVar1)


This condition is reading 4 bytes from memory, xores them together and applies 0x20 comparing with the input 
```c
`*pbVar1 != (byte)(*pbVar2 ^ *pbVar3 ^ *pbVar5 ^ *pbVar4 ^ 0x20)`
```


So the loop expects for each index i to be: 
```
key == key_2[i] ^ key_3[i] ^ key_5[i] ^ key_4[i] ^ 0x20
```

The loop runs 16 times because 
```
`} while (pbVar2 != key1 + 0x10);`
```
key1 is a pointer to 16 byte array and 0x10 is just 16 

so key1 + 0x10 = address 16 bytes after the start of key1

## Python solution 
To satisfy this solution we can reconstruct the expected input 

```python
key2= [94, 54, 50, 40, 65, 121, 38, 51, 96, 114, 55, 106, 124, 81, 125, 62]
key3= [51, 75, 112, 42, 51, 43, 78, 100, 106, 120, 95, 41, 64, 107, 100, 78]
key4= [58, 118, 84, 51, 63, 91, 90, 125, 99, 86, 39, 111, 102, 56, 63, 67]
key5= [54, 105, 117, 55, 40, 105, 85, 66, 112, 68, 36, 57, 75, 108, 73, 67]
for i in range(16):
    res = key2[i] ^ key3[i] ^ key5[i] ^ key4[i] ^ 0x20
    print(chr(res), end="")

```






## Decompiled main from Ghidra 
```c

undefined8 main(int argc,long param_2)

{
  byte *pbVar1;
  byte *pbVar2;
  byte *pbVar3;
  byte *pbVar4;
  byte *pbVar5;
  long in_FS_OFFSET;
  byte key1 [32];
  byte key2 [32];
  byte key3 [32];
  byte key4 [24];
  long canary;
  
  pbVar2 = key1;
  canary = *(long *)(in_FS_OFFSET + 0x28);
  if (argc == 2) {
    key1[0x10] = 0;
    pbVar5 = key2;
    key2[0x10] = 0;
    pbVar1 = *(byte **)(param_2 + 8);
    pbVar4 = key3;
    pbVar3 = key4;
    key3[0x10] = 0;
    key4[0x10] = 0;
    key1[0] = 0x5e;
    key1[1] = 0x36;
    key1[2] = 0x32;
    key1[3] = 0x28;
    key1[4] = 0x41;
    key1[5] = 0x79;
    key1[6] = 0x26;
    key1[7] = 0x33;
    key1[8] = 0x60;
    key1[9] = 0x72;
    key1[10] = 0x37;
    key1[0xb] = 0x6a;
    key1[0xc] = 0x7c;
    key1[0xd] = 0x51;
    key1[0xe] = 0x7d;
    key1[0xf] = 0x3e;
    key2[0] = 0x36;
    key2[1] = 0x69;
    key2[2] = 0x75;
    key2[3] = 0x37;
    key2[4] = 0x28;
    key2[5] = 0x69;
    key2[6] = 0x55;
    key2[7] = 0x42;
    key2[8] = 0x70;
    key2[9] = 0x44;
    key2[10] = 0x24;
    key2[0xb] = 0x39;
    key2[0xc] = 0x4b;
    key2[0xd] = 0x6c;
    key2[0xe] = 0x49;
    key2[0xf] = 0x43;
    key3[0] = 0x3a;
    key3[1] = 0x76;
    key3[2] = 0x54;
    key3[3] = 0x33;
    key3[4] = 0x3f;
    key3[5] = 0x5b;
    key3[6] = 0x5a;
    key3[7] = 0x7d;
    key3[8] = 99;
    key3[9] = 0x56;
    key3[10] = 0x27;
    key3[0xb] = 0x6f;
    key3[0xc] = 0x66;
    key3[0xd] = 0x38;
    key3[0xe] = 0x3f;
    key3[0xf] = 0x43;
    key4[0] = 0x33;
    key4[1] = 0x4b;
    key4[2] = 0x70;
    key4[3] = 0x2a;
    key4[4] = 0x33;
    key4[5] = 0x2b;
    key4[6] = 0x4e;
    key4[7] = 100;
    key4[8] = 0x6a;
    key4[9] = 0x78;
    key4[10] = 0x5f;
    key4[0xb] = 0x29;
    key4[0xc] = 0x40;
    key4[0xd] = 0x6b;
    key4[0xe] = 100;
    key4[0xf] = 0x4e;
    do {
      if (*pbVar1 != (byte)(*pbVar2 ^ *pbVar3 ^ *pbVar5 ^ *pbVar4 ^ 0x20)) {
        puts("Nope.");
        goto LAB_00101176;
      }
      pbVar2 = pbVar2 + 1;
      pbVar5 = pbVar5 + 1;
      pbVar4 = pbVar4 + 1;
      pbVar3 = pbVar3 + 1;
      pbVar1 = pbVar1 + 1;
    } while (pbVar2 != key1 + 0x10);
    puts("Pass valid!");
  }
  else {
    puts("usage ./crackme \"<key>\"");
  }
LAB_00101176:
  if (canary == *(long *)(in_FS_OFFSET + 0x28)) {
    return 0;
  }
                    /* WARNING: Subroutine does not return */
  __stack_chk_fail();
}


```

