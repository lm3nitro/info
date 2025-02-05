# $${\color{lightblue}Nikto}$$

![Pasted image 20240424131136](https://github.com/lm3nitro/Projects/assets/55665256/72ab1490-febc-4011-9812-436feeb7110a)

Nikto is an open-source web vulnerability scanner based on Perl. It scans web servers for dangerous files, outdated software, and misconfigurations. 

### Scope:

I will be installing Nikto and using it to scan a web server, see what it is able to find and report, and will be analyzing the traffic on the web server itself via Wireshark.

### Tools and Technology:
Nikto, Linux OS, Apache2, and Wireshark

## Installing Nikto:

Lets install Nikto. This can be done directly from the cli with the following command:

```
sudo apt install nikto
```

![Pasted image 20240424130448](https://github.com/lm3nitro/Projects/assets/55665256/7f1e845a-1110-40df-92a4-1adf086573e6)

To see what options are available with Nikto and get more information on it, the following command can be used:

```
nikto -h
```
![Pasted image 20240424130804](https://github.com/lm3nitro/Projects/assets/55665256/d07ebd71-beab-425f-b27f-163a03e9536a)

![Pasted image 20240424130632](https://github.com/lm3nitro/Projects/assets/55665256/9d430fdc-5247-4d27-be9f-4699b5f36407)

## Performing vulnerability scans: 

Now that I installed Nikto, I can get started with performing the vulnerability scan on the web server. There are several options that can be used for the scanning. To start, I will do just a basic command:

```
sudo nikto -host 192.168.91.129
```

![Pasted image 20240424131710](https://github.com/lm3nitro/Projects/assets/55665256/bdf7900f-2574-43c4-b2be-a1681aa1f1dc)

Based on the output, it was able to identify the outdated version of Apache that the web server is running. It was also able to get detailed information on a few directories. I like that it provides a short detail next to its findings which can assist with reporting. There are other scanning options available, below are a few. 

### Scanning Options:

Nikto also allows users to specify a specific port to scan. As an example, to scan port 8080, the following command can be used:

```
sudo nikto -h 192.168.91.129 -p 8080
```

To scan a range of ports by specifying the start and end of the port range. This command will scan all ports from 8080 to 9090:

```
sudo nikto -h 192.168.91.129 -p 8080-9090
```

Nikto can also scan a web server using the URL:

```
sudo nikto -h http://lm3nitro.web.vuln.server.com
```
To scan a URL at a specific port. Two syntax options are available:

Option 1 :
```
sudo nikto -h http://lm3nitro.web.vuln.server.com -p 8080 
```

Option 2:
```
sudo nikto -h http://lm3nitro.web.vuln.server.com:8080
```

## Traffic analysis:

Now that I scanned the web server, I took a look at Wireshark that was running on the server itself capture the traffic generated by the scan.
 
![Pasted image 20240424132637](https://github.com/lm3nitro/Projects/assets/55665256/bc18d899-1d3c-407d-917a-224d30aeb84d)

Based on the output, the server is responding back with many '404 not found'. Taking a look at the Statics IO graph, I see the following spikes:

![Pasted image 20240424132411](https://github.com/lm3nitro/Projects/assets/55665256/8a7a46bf-9bff-4948-8dac-a6307283f59e)


One of the behaviors that I was able to see from Nikto in Wireshark is that it creates many GET requests to paths that do not exit on the web server. Seeing this type of traffic can be indicative of some type of scanning, that if unknown, can be suspicious or malicious activity:


![Pasted image 20240424133216](https://github.com/lm3nitro/Projects/assets/55665256/2149daad-1577-4e09-bb6e-23cfbe04030b)

Here is a details view into the GET requests data sizes: 

![Pasted image 20240424133413](https://github.com/lm3nitro/Projects/assets/55665256/e6f71502-ff78-4c83-af0c-6f8c83f97ec2)

I was also able to Identify the application being used within Wireshark and its version:

![Pasted image 20240424132748](https://github.com/lm3nitro/Projects/assets/55665256/64b79971-dbce-4c98-a1b6-a1a751358fe7)

## Summary:

I liked that Nikto was very simplistic and easy to use. Often times, web servers are first in the line of attack. Having a tool like Nikto that can provide insights into the security posture of the server is very beneficial. Nikto can be installed both in Win and Linux OS. It's important to note that Nikto is not the best tool for stealth procedures. As seen in the Wireshark traffic analysis, it does many GET requests that can be seen and not to mention what can be found in ther server logs. Despite this,it can provide a wide array of valuable information for improving the security of a web server, even though it may not be the most discreet option.
