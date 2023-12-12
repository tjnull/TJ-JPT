# SCP Commands

## Copy to remote server (using PuttySCP):

`pscp -P 22222 C:\Users\User\<localfile> user@remotehost:/folder/<remote_Directory>`

## Normal SCP:

Copy from remote server:
`scp -P 22222 user@myhost:\home\user\<file.exe> C:\Users\Victim`

With key file:
`scp -i pkey user@myhost:\home\user\<file> /home/destination/`