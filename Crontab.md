## check if crontab is available
rpm - qa | grep crontabs

## If not available install via yum
systemctl start crond
systemctl enable crond

## syntax available here
cat /etc/crontab 
crontab -e      (will open a file ..press i to enter)
   39    09    01   01  03   root  echo "welcome to 2030" >> india.txt
    Min hour date month dayofweek
:wq!
ls

## Check Job
crontab -l
crontab -e
  */10  09    01   jan  03   echo "welcome to 2030" >> india.txt
  Every 10 mins
  
## (to delete job)
crontab -r

## Crontab through user

useradd cloudknowledge
passwd cloudknowledge
crontab -u cloudknowledge -e   (Add parameters)
crontab -u cloudknowledge -l (to verify job)
tail -f /var/log/cron (to check logs)
crontab -r -i -u cloudknowledge (to delete with confirmation)


## Running script on crontab
vim daily.sh
   tar -czvf redhat.tar /etc
:wq!

crontab -e
  *  *  *  *  *    sh /root/daily.sh
:wq!

## Or go to #cd /etc/cron.hourly/ put the sh file here it will execute
Every hour
chmod +x sanjay.sh

## Or... Put sh file anywhere for example/mydir and go to
cd  /etc/cron.d (you will see 0hourly)
vim 0hourly
    01 * * * * root run-parts /mydir
   

## to deny normal user for crontab
vim /etc/cron.deny
   cloudknowledge
:wq!
