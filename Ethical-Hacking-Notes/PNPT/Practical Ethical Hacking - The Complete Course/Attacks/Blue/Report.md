1- Enumerate with Nmap
1.1 - use th search vulnerability on namp (nmap -sV --script vuln 192.168.138.133) and find it is vulnerable to CVE-2017-0143.
2 - Search online about the version of the machine, "Windows 7 Ultimate 7601 Service Pack 1  exploit"
3 - Find that it might be vulnerable to eternal blue
4 - search eternal blue or the cve on metasploit and run the exploit
4.1 - set the payload to me meterpreter and try different options. Do not forget to check if it vulreable either using the "check" or using the auxiliary(scanner) insted of exploit.