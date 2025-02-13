Reconnaissance also known as Information gathering 

<<<<<<< HEAD
The more information you can collect the more options during the actual pentest.

# Passive Information Gathering
*Collecting public information about the target, and not interacting with it, in a suspicious way*
- IP Addresses and DNS Information
- Domain names and information
- Email Addresses & social media (of company and workers)
- technologies that target uses
- subdomains (if any in scope)
-----
## useful Commands & url's
**Commands**
`host <url>` returns IPv4, IPv6 and possibly Email.
`whatweb <url>` Identifies technologies used on a website.
`whois <url>` Queries the WHOIS database for domain registration details.

----
**Websites**
`<url>/robots.txt`
stops webcrawalers from indexing sites, but can reveal directories.

`<url>/sitemap /(s)`
Contains a structured list of all indexed pages

https://httrack.com

https://netcraft.com 

### Vid 1 - Website Recon & Footprinting 
The video shows the website for passive scanning https://hackersploit.org
along with some of ways to get basic information on a target

**command**
He talks about the command  ` host Ip-Address || url-address`  
which gives a bit of basic info about the website.
*Note:* if there are more then 2 addresses the address is probably using a [[Proxy web servers|proxy server]]

another command is the `Whatweb Ip-Address || url-address` 
This will try to tells us what technologies were used to create the website
*Note* The info can be missing or wrong.

**search engine tools**
Another is the `robots.txt` url where you list sites that a search engines Webcrawlers from visiting those sites.
another url is the `sitemap(s).xml` where you can see the different indexed pages.


**Download the website**

### Vid 2 - WhoIs Enumeration
when trying to gather information silently host is a good way to obtain the 
### Vid 3 - Website Footprinting with Netcraft
### Vid 4 - DNS Recon
### Vid 5 - WAF With Wafw00f

### Vid 6 - Subdomain Enumeration With Sublis3r
### Vid 7 - Google Dorks
### Vid 8 - Email Harvesting With theHarvester
### Vid 9 - Leaked Password Databases


----
# Active Information Gathering
*Collecting information directly from the target, that can lead to directly exploitable information*
- find Open ports
- Learn internal infrastructure (network & technologies)
- Enumerating information
----

### Vid 1 - DNS Zone Transfers
### Vid 2 - Host Discovery with Nmap
### Vid 3 - Port Scanning with Nmap






## Active




## 

>>>>>>> origin/main






