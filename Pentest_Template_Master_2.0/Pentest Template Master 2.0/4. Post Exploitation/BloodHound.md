## Source: 

https://github.com/BloodHoundAD/BloodHound

## Pre-Compiled Binaries

https://github.com/BloodHoundAD/BloodHound/releases

## SharpHound: 

https://github.com/BloodHoundAD/SharpHound3

## Bloodhound for python 
Note: Only compatiable with BloodHound 3.0 or newer

https://github.com/fox-it/BloodHound.py

## Install on Kali:

apt install bloodhound

## Gather Data

- import-module .\sharphound.ps1
- invoke-bloodHound -CollectionMethod All -domain target-domain -LDAPUser username -LDAPPass password