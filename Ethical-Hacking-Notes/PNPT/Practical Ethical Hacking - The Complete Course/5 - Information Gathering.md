## Passive Reconnaissance Overview
1 - Location information:
	- Stelite images
	- Drone recon
	- building layout (badge readers, break areas, security, fencing)
2 - Job information
	- Employees (Name, job title, phone number, manger, ...)
	- Pictures (badge photos, desk photos, computer photos, ...)

## Web/Host
- Target validation  (Whois, nslookup, dnsrecon)
- Finiding Subdomains (Google fu, dig, Nmap, ...)
- Fingerprinting - *Whats runing on a host/website* (Nmap, Netcat, Wappalyzer)
- Data Breaches (HaveIBeenPwned, Breach-Parse, WeLeakInfo)


## Discovering Email Addresses
- https://hunter.io/
- https://phonebook.cz/
- https://www.voilanorbert.com/
- https://chrome.google.com/webstore/detail/clearbit-connect-free-ver/pmnhcgfcafcnkbengdcanjablaabjplo (Best option, needs to be used on gmail, using the extention)
- https://tools.emailhippo.com/ - Free email address verification tool
- https://email-checker.net/ - Email Checker

## Gathering Breached Credentials with Breach-Parse
 - https://github.com/hmaverickadams/breach-parse
	 - This contains 40 gb of emails and passwords. Then it can be ran from terminal and will gather email, passwords and usernames from a specific domain. Example: ./breach-parse.sh tesla.com tesla.txt

## Hunting Breached Credentials with DeHashed
- https://www.dehashed.com/ - Paid service
	- 	https://academy.tcm-sec.com/courses/1152300/lectures/33636088 - How to use
- After getting a hashed password you can go to https://hashmob.net/search and see if that hash is already cracked. After go to google and look for "the hash found(sdfdsf)" and see if something comes up. 

## Hunting Subdomains
- We start by installing sublist3r "apt install sublist3r"
	- to run "sublist3r -d tesla.com"
- https://crt.sh/ - another website to find subdomains, search .tesla.com.
- https://github.com/owasp-amass/amass - is a better tool than sublit3r. Example "amass enum -d tesla.com". Can be used for :
![[Pasted image 20230416111114.png]]
	- After getting all the possible subdomaisn, you can use a tool called "tomnomnom" https://github.com/tomnomnom/httprobe - This will take a list of domaisn and check if they are alive/accessible. 

## Identifying Website Technologies
- https://builtwith.com/ - This will output the technologie the domai is running. Like widgets, frameworks (important info), CDN (content delivery network), content management system.
- wappalyzer firefox extention (https://addons.mozilla.org/en-GB/firefox/addon/wappalyzer/) - This will return the same information but more clear. You can try to look for vulnerabilities from the platforms versions. Example (Webserver - nginx 1.14.0).
- Whatweb on linux terminal - "whatweb https://tesla.com" - does not give as much informtion

## Information Gathering with Burp Suite
- Using burp to intercet the trafic from a website/domain we can gather important information from request responses. such as versions and technologies used. Example: PHP/7.3.7

## Google Fu
- Reference: https://support.google.com/websearch/answer/2466433?hl=en
- site:tesla.com -www -ir - this on google will look for sites with tesla.com not starting with www or ir. 
- filetype: docx or pdf
## Utilizing Social Media
- Look for the comapny page, people and look for photos, try to look for badges, anything related to the site or even layouts. Twitter, Linkdin.


