### Find subdomaisn with Assetfinder
This will find subdomains in a quick way.
- How to install
	- sudo apt install assetfinder
- "assetfinder tesla.com", "assetfinder tesla.com >> teslasubs.txt"
- "assetfinder --subs-only tesla.com"
- A good script using assetfinder, named "run.sh"
#!/bin/bash

url=$1

if [ ! -d "$url" ];then
	mkdir $url
fi

if [ ! -d "$url/recon" ];then
	mkdir $url/recon
fi

echo "[+] Harvesting subdomains with assetfinder..."
assetfinder $url >> $url/recon/assets.txt

cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt 
rm  $url/recon/assets.txt

### Find subdomains with Amass
How to install
	- sudo apt install amass
"amass enum -d tesla.com"

- an extention to assetfinder script:
#!/bin/bash

url=$1

if [ ! -d "$url" ];then
	mkdir $url
fi

if [ ! -d "$url/recon" ];then
	mkdir $url/recon
fi

echo "[+] Harvesting subdomains with assetfinder..."
assetfinder $url >> $url/recon/assets.txt

cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt 
rm  $url/recon/assets.txt

echo "[+] Harvesting subdomains with amass..."
amass enum -d $url >> $url/recon/f.txt
sort -u $url/recon/f.txt >> $url/recon/final.txt
rm $url/recon/f.txt


### Finding Alive Domains with Httprobe
- sudo apt install httprobe 
- "cat tesla.com/recon/final.txt | httprobe"
	- This will test if the domais are alive meaning it will respond. 
- "cat tesla.com/recon/final.txt | httprobe -s -p https:443 | sed 's/https\?:\///' | tr -d ':443'"

#!/bin/bash

url=$1

if [ ! -d "$url" ];then
	mkdir $url
fi

if [ ! -d "$url/recon" ];then
	mkdir $url/recon
fi

echo "[+] Harvesting subdomains with assetfinder..."
assetfinder $url >> $url/recon/assets.txt

cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt 
rm  $url/recon/assets.txt

#echo "[+] Harvesting subdomains with amass..."
#amass enum -d $url >> $url/recon/f.txt
#sort -u $url/recon/f.txt >> $url/recon/final.txt
#rm $url/recon/f.txt

echo "[+] Probing for alive domains..."
cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 | sed 's/https\?:\///' | tr -d ':443'  >> $url/recon/alive.txt




### Screenshotting Websites with GoWitness
### TCM enumeration automation
https://pastebin.com/MhE6zXVt
### Additional Resources
The Bug Hunter's Methodology - [https://www.youtube.com/watch?v=uKWu6yhnhbQ](https://www.youtube.com/watch?v=uKWu6yhnhbQ)

Nahamsec Recon Playlist - [https://www.youtube.com/watch?v=MIujSpuDtFY&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA](https://www.youtube.com/watch?v=MIujSpuDtFY&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA)
