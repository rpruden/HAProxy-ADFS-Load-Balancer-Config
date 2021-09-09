# HAProxy-ADFS-Load-Balancer-Config
This is an HA Proxy config file for load balancing ADFS servers

While setting up HAProxy as a load balancer for ADFS I found that monitoring the health was tricky.
It took some trial and error but I was finally able to get monitoring working in a fashion that only
marks the server as healthy if the ADFS service is running. Previously, reboots of the ADFS server would
result in error 501 during the time when the server booted until the services actually started. HAProxy was
unaware of this and therefor would route traffic to it before it was ready. Using this config I have been able to prevent
HAProxy from erroneously marking the destination server as up until the ADFS service is running.

To use this config, you will want to replace the string "adfs.domain.com" with the domain name of your ADFS farm.

This will tell the health check to connect to your ADFS server's IP address without verifying SSL certificate and
using SNI to indicate the proper hostname.

If using a web application proxy to connect to ADFS, you will want to make sure that your non primary ADFS server is set as "backup" in the config. This is because the Web Application Proxy's will only sync through the primnary ADFS. If you are load balancing, you will have intermittent configuration loading errors on the web application proxys. With this config, your WAP's will stay synchronized, but in the event your primary ADFS server reboots or goes offline the secondary one will pick up right away and then fail back to the primary once it becomes available again.

Health checks are possible using whichever domain name you choose as long as you connect unsecured via port 80.
