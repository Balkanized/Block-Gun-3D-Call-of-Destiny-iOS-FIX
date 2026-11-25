# Block-Gun-3D-Call-of-Destiny-iOS-FIX
A patch for Wizard Game's Block Gun 3D: Call of Destiny to be supported for play on modern iOS devices.

Currently tested and working for iOS 18.7.8 only. Help is needed from those willing to test the patched IPA on their own devices.

A jailbroken device is not needed to launch/install the IPA and can be sideloaded as well.

## Download
The patched file is currently not available for download due to GitHub file size limits. It must be built using the instructions in the Development section.

## Development
A list of steps for making your own build of BG3D:COD with the patch. This process can be used for further modification or as an alternative to downloading the patched IPA file directly if a download is unavailable.

### Extraction
The decrypted app package was first extracted using [Eevee IPA Decrypter](https://t.me/eeveedecrypterbot) on Telegram, which is a suitable option if you do not have access to a jailbroken device (like me).
Those with a jailbroken device can also extract the IPA using [ipatool](https://github.com/majd/ipatool) if they wish to extract from the App Store directly.

### Decryption (Ghidra)
After extraction, the IPA file is converted to a ZIP file (simply by changing the name of the file extension), unzipped/extracted, and its Mach-O Binary file is exported to [Ghidra](https://github.com/nationalsecurityagency/ghidra) for the decryption of its AARCH-64 Assembly code.
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
This changes the conditional statement (cbz), IVar1 != 0, to an unconditional statement (b) by returning IVar1 before UIToolBar, UITextView and UIFont can be called upon, which are deprecated functions on modern iOS and cause the crash state to be triggered after returning null.

### Repackaging
Save your Ghidra project and export (File > Export Program) with the Original File format selected(Ghidra ZIP .gzf file type will not work).
In the original unzipped file where the Mach-O Binary was extracted from, replace the unmodified Mach-O Binary with the newly patched file exported from Ghidra.
- The new binary should be considerably smaller in size than the original one, I believe that is because 32 bit support is dropped once Ghidra exports the file.

Compress/Rezip the Payload folder (NOT the header folder) and set its destination to somewhere outside the header folder.
Convert the ZIP to IPA by renaming the new Payload.zip folder to bg3dcod.ipa (the name can be whatever you want as long as the extension is changed to .ipa).

### Uploading
If done correctly, you now possess the patched IPA and can upload it to your iOS device by sideloading. If your device is jailbroken, you have other options as well.
- [Sideloadly](https://sideloadly.io/) was used to test the patch originally, but other sideloading applications should work too.
- Enjoy Block Gun 3D: Call of Destiny like it's 2014 again!

## Acknowledgements

- Block Gun 3D: Call of Destiny iOS FIX by Balkanized.
- Original Game by Matthew Paxman's Wizard Games Inc ©2014.
- Music by Kevin Macleod (www.incompetech.com).
