# Linux network troubleshooting

## DNS troubleshooting

These are some of useful tools for testing name resolution on a Linux systems:

- ping
- nslookup
- dig
- host

Install the tools:

```bash
sudo dnf install bind-utils
```

### Check the connectivity to the remote host

``` bash
# If this work then dns resolution is okay
ping -c 3 server01
#If the first test fail, try to ping ip address . 
#If this works, connectivity exists. Name resolution is the problem since that's where the failure appears. 
ping -c 3 192.168.1.101
```

### How to use 'nslookup' command

```bash
#This output should display the IP address for server01, along with information about which server resolves the name. 
#If this fails, it indicates a name resolution problem.
nslookup server01

#Perform a reverse lookup
 nslookup 192.168.1.101

#To see specific resource record types, use the -type= option. 
#Here's an example that queries for the MX records of the example.com domain
nslookup -type=MX example.com

```

### How to use 'host' command

```bash
# forward dns lookup:
host server01

# reverse dns lookup:
host 192.168.1.101

#Querying for SOA records relies on the -C option:
$ host -C my.gov.au

#The following example queries for the MX records of example.com:
host -t mx my.gov.au

#If you're not sure which record types you need or if you want to see them all, use the -a (any) option:
$ host -a my.gov.au

```

## References

- [DNS-name-resolution-troubleshooting-tools](https://www.redhat.com/en/blog/DNS-name-resolution-troubleshooting-tools)