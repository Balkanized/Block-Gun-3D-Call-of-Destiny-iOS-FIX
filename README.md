# Block-Gun-3D-Call-of-Destiny-iOS-FIX
A patch for Wizard Game's Block Gun 3D: Call of Destiny to be supported for play on modern iOS devices.

Currently tested and working for iOS 18.7.8 only. Help is needed from those willing to test the patched IPA on their own devices.

A jailbroken device is not needed to launch/install the IPA and can be sideloaded as well.

## Development
A list of steps for making your own build of BG3D:COD with the patch. This process can be used for further modification or as an alternative to downloading the patched IPA file directly if a download is unavailable.

### Extraction
The decrypted app package was first extracted using [Eevee IPA Decrypter][https://t.me/eeveedecrypterbot] on Telegram, which is a suitable option if you do not have access to a jailbroken device (like me).
Those with a jailbroken device can also extract the IPA using [ipatool][https://github.com/majd/ipatool] if they wish to extract from the App Store directly.

### Decryption (Ghidra)
After extraction, the IPA file is converted to a ZIP file (simply by changing the name of the file extension), unzipped/extracted, and its Mach-O Binary file is exported to Ghidra for the decryption of its AARCH-64 Assembly code.
- The Mach-O Binary file can be identified on Windows as the file within the IPA's Payload directory that shares the same name of the app and appears to have no file extension/type. Insert this file into a new project on Ghidra.
- You shouldn't have Ghidra analyze the entire ~60 MB binary at once unless you know what you are doing. The next step describes which memory addresses need to be analyzed and rewritten as a function in Objective C.

### Patch
In Ghidra, press G and enter the memory address 10000aef8 to be brought to the following line:
```
10000aef8 cbz x19, LAB_10000b2cc
```
insert the following patch by right-clicking on the line and selecting Patch Instruction:
```
10000aef8 b LAB_10000b2cc
```
