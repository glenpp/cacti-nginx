# Nginx on Cacti via SNMP



While monitoring Apache is well catered for, this article following on from my previous one on SNMP basics aims to provide the necessary Cacti graphs for monitoring Nginx.

## Nginx Stats URL

Like with Apache, Nginx has an easy to configure status URL which provides the few statistics about Nginx that we monitor here. This is rather simple to configure and all that was needed is in one vhost to put:

```
location /nginx_status {
        stub_status on;
        access_log off;
        allow ::1;
        allow 127.0.0.1;
        deny all;
}
```

After reloading the configuration a request to http://localhost/nginx_status should provide a basic text output that with a bit of processing with grep and sed can easily be turned into a list of numbers for snmpd to digest. Fortunately I've done that for you :-)

I put the extension script nginx-stats in /etc/snmp/ and set it executable then add the lines from snmpd.conf.cacti-nginx to /etc/snmp/snmpd.conf to call the script for appropriate SNMP requests.

Restart snmpd and you should be able to get basic stats via SNMP.

## Cacti Templates

I have generated some basic Cacti Templates for Nginx.

Simply import the template cacti_host_template_nginx.xml, and add the graphs you want to the appropriate device graphs in Cacti. It should just work if your SNMP is working correctly for that device (ensure other SNMP parameters are working for that device).