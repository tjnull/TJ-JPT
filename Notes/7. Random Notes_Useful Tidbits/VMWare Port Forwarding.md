# VMWare Port Forwarding

Use this to get port forwarding enabled from a NAT'ed VM that is hosting a server or files so you can access them with the host's internal IP.

1. Edit -> Virtual Network Editor
2. Click the NAT interface for the target VM
3. Click NAT settings. 
4. Click Add.
5. Set host port (i.e. 9091)
6. Set VM IP address.
7. Set VM port address (i.e. 9090: python -m SimpleHTTPServer 9090)
8. Click Ok.

Next you might need to add inbound/outbound rules on the host. 

Once that is all completed, you can reach the VM hosting on port 9090 but hitting the host's IP address at port 9091.
i.e. trying to get to "vmIP:9090", so go to "hostIP:9091". 

