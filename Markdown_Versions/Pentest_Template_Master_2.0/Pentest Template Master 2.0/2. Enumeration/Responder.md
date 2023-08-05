Cannot use in the OSCP Exam. Fun to use on assessments.
Note: Multirelay.py does not work in python3 since the UserDict library has been depricated


# Source: https://github.com/lgandx/Responder

## Make changes to config to turn off services:

nano /usr/share/responder/Responder.conf

## Starting Responder:

- responder -I [Interface] -A
- responder -I [Interface] -i [IP Address] or -e [External IP] -A

## Tools in Responder: 

Location: /usr/share/Responder/tools

## Check for systems with SMB Signing not enabled

- python3 RunFinger.py -i 172.21.0.0/24

