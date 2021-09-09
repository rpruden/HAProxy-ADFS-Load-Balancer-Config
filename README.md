# HAProxy-ADFS-Load-Balancer-Config
This is an HA Proxy config file for load balancing ADFS servers

While setting up HAProxy as a load balance for ADFS I found that monitoring the health was tricky.
It took some trial and error but I was finally able to get monitoring working in a fashion that only
marks the server as healthy if the ADFS service is running. Previously, reboots of the ADFS server would
result in error 501 for the time when the server booted until the services actually started. HAPRoxy was
unaware of this and therefor would route traffic to it. Using this config I have been able to prevent
HAProxy from erroneously marking the destination server as up until the ADFS service is running.

To use this config, you will want to replace the string "adfs.domain.com" with the domain name of your ADFS farm.

This will tell the health check to connect to your ADFS server's IP address without verifying SSL certificate and
using SNI to indicate the proper hostname.
