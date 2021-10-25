# Digitally Sign Files (PowerShell Example)

From:
- https://sid-500.com/2017/10/26/how-to-digitally-sign-powershell-scripts/

---

### Creating a self-signed Certificate
Open Windows PowerShell and run the following One-Liner to create a signing certificate.
```
New-SelfSignedCertificate -DnsName patrick@sid-500.com -CertStoreLocation Cert:\CurrentUser\My\ -Type Codesigning
```

You can find your certificate in your certificate store. Run certmgr.msc.
```
C:\> certmgr.msc
```

### Import the Certifcate in Trusted Root Certification Autorities and Trusted Publisher
Now the certificate must be exported and then imported into the Trusted Root Certification Authorities and Trusted Publishers.

![DSF1](../_resources/DSF1.png)

Double click on the certificate and select Details and Copy to file …

![DSF2](../_resources/DSF2.png)

Do not export the private key. No need for.

![DSF3](../_resources/DSF3.png)

Select CER Format.

![DSF4](../_resources/DSF4.png)

Save the file wherever you want.
Now import the certificate to the Trusted Root Authorities and Trusted Publishers.

![DSF5](../_resources/DSF5.png)


### Sign a file
Next, we use Set-Authenticodesignature to sign our file. In this example, it is a. ps1 file, thus a PowerShell script.
```
Set-AuthenticodeSignature -FilePath C:\Temp\script1.ps1 -Certificate (Get-ChildItem -Path Cert:\CurrentUser\My\ -CodeSigningCert)
```

![DSF6](../_resources/DSF6.png)

Don’t worry about the Status Unknown Error. The next time you do it valid comes up. Crazy Stuff. Ok, we don’t care about this now.

![DSF7](../_resources/DSF7.png)

Nice. Finally, see what happened. Open Windows Explorer, right click on your file, select properties and click on Digital Signatures.

![DSF8](../_resources/DSF8.png)

### Testing your script
For testing your script, make sure the execution policy allows the running of PS1 scripts.
```
Get-ExecutionPolicy
```
Remotesigned, AllSigned and Unrestricted are your friends … If the policy is set to restricted then set it – for this testing environment – to AllSigned.
```
Set-ExecutionPolicy AllSigned
```

![DSF9](../_resources/DSF9.png)

---

#powershell