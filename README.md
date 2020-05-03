# DnsR

The dnsr program is a program used to resolve subdomains. You can resolve subdomains without the need for any resolvers.txt
To use this program, python3 software language must be installed on your computer.

The DnsR program uses the ip address of many dns services for dns resolve. 

```
CloudFlare : 1.1.1.1, 1.0.0.1
Google DNS : 8.8.8.8, 8.8.4.4
Yandex DNS : 77.88.8.8, 77.88.8.1
Verisign DNS :  64.6.64.6, 64.6.65.6
Quad9 DNS : 9.9.9.9, 149.112.112.112
Comodo DNS :  8.26.56.26, 8.20.247.20
OpenDNS : 208.67.222.222, 208.67.220.220
CleanBrowsing : 185.228.168.9, 85.228.169.9
Alternate DNS : 198.101.242.72, 23.253.163.53
AdGuard DNS : 176.103.130.130, 176.103.130.131
```

The DnsR program also adds the ip addresses of the target's name servers to the list above. For example, if the target website uses amazonaws DNS servers, the IP addresses of those DNS servers are used to resolve subdomains.
This method will have a greater impact on the operation of the Program.


# Install

```

git clone https://github.com/Phoenix1112/DnsR.git

cd DnsR

pip3 install -r requirements.txt

```

# Usage

The program is very simple to use. You can use the DnsR program in two different ways.

```
python3 DnsR.py --list subdomains.txt --output resolved_results.txt

cat subdomains.txt | python3 DnsR.py --stdin --output resolved_results.txt

```

no banner was used in the program.  because I wanted you to be able to use DnsR output with another program. When you want to use subdomains with the pipe method You must use the --stdin parameter. so the program will read subdomains from stdin.

By default, 20 threads are used in the program. You can increase the number of threads at any time with the --thread parameter.

```
python3 DnsR.py --list subdomains.txt --thread 50 --output resolved_results.txt

cat subdomains.txt | python3 DnsR.py --stdin --thread 50 --output resolved_results.txt

```

The features of the DnsR program are not limited to these. You can also use the --blacklist parameter to filter wildcard subdomains. To use this parameter, you need to know the wildcard cname record or wildcard ip address in advance.


example:
--------
```

dig cname admin.www.example.com

admin.www.example.com  CNAME  x.example.com


dig cname ftp.www.example.com

ftp.www.example.com  CNAME  x.example.com


dig cname cloud.www.example.com

cloud.www.example.com  CNAME  x.example.com

```

In the example above, we see that each cname record points to the x.example.com dns record. this is a wildcard dns record. 
With the DnsR program, you can filter subdomains by filtering according to such dns records.

```
cat subdomains.txt

admin.www.example.com
ftp.www.example.com 
cloud.www.example.com
account.example.com
video.example.com



python3 DnsR.py --list subdomains.txt --blacklist x.example.com --output resolved_results.txt


output:
------
account.example.com
video.example.com

```

Looking at the results above, we filtered the subdomains pointing to the dns record x.example.com. You can enter multiple blacklist values by placing commas between them. If you want, you can write ip addresses to blacklist value. You can also type shortened  ip addresses or shortened cname records.Sometimes there is more than one subdomain with an incorrect A record in a subdomain list. these ip addresses usually have the same beginning but different ends. You can also filter them with the blacklist parameter.

```
cat subdomains.txt

admin.example.com
ftp.example.com
account.example.com


dig a admin.example.com

admin.example.com  A  192.55.65.88


dig a ftp.example.com

ftp.example.com  A  192.55.12.66


dig a account.example.com

account.example.com  a  207.66.88.21



python3 DnsR.py -l subdomains.txt -b 192.55,x.example.com,192.77.88.6


output:
------

account.example.com

```
