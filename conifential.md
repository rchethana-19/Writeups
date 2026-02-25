# THM-Confidential

## About
level : Easy
Topics: Digital Forensics-Broken Qr code
Tools used: pdfimages(in kali) 

## Steps
1.Go to the pdf saved directory
2.The command to execute
``` bash
pdfimages -f 1 -png Repdf.pdf confidential
```

## Flag
``` bash
flag{e08e6ce2f077a1b420cfd4a5d1a57a8d}
```

## Additional Learnings
If its in png format increase the resolution and use tool zbarimg
``` bash
convert qr.png -threshold 50% clean.png
zbarimg qr.png
```
