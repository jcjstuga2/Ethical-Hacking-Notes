## The OWASP top 10 and OWASP testing checklist
### Resources
Supper important to use the checklist on a webapp pen test.
OWASP Top 10: [https://owasp.org/www-pdf-archive/OWASP_Top_10-2017_%28en%29.pdf.pdf](https://owasp.org/www-pdf-archive/OWASP_Top_10-2017_%28en%29.pdf.pdf)

OWASP Testing Checklist: [https://github.com/tanprathan/OWASP-Testing-Checklist](https://github.com/tanprathan/OWASP-Testing-Checklist)

OWASP Testing Guide: [https://owasp.org/www-project-web-security-testing-guide/assets/archive/OWASP_Testing_Guide_v4.pdf](https://owasp.org/www-project-web-security-testing-guide/assets/archive/OWASP_Testing_Guide_v4.pdf)


### Installing OWASP Juice Shop
First install Docker following the steps here: https://airman604.medium.com/installing-docker-in-kali-linux-2017-1-fbaa4d1447fe
- Second we will install juice shop, a vulnerable website made by owasp using docker:https://github.com/juice-shop/juice-shop
- This is the challenge board for juice shop: https://pwning.owasp-juice.shop/

### SQL Injection
Common SQL Verbs
- Select, insert, delete, update, drop and union
Other common terms
- Where, and/or/not, order by.
Special Characters
![[Pasted image 20230608083714.png]]
#### Walktrough
- on the login try "test' or 1=1; -- ", on repeter and then log in using it to login as admin.
- Notes on blind injection
	- We can test the sleep 5, sleep 10 and if the response takes that time means it is vulnerable to sql injection. 