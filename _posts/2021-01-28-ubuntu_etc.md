
## To turn on syslog message 
  
```
sudo vim /etc/rsyslog.d/50-default.conf
```
  
uncomment the below content
```
*.=info;*.=notice;*.=warn;\
       auth,authpriv.none;\
       cron,daemon.none;\
       mail,news.none          -/var/log/messages
```

Then, restart the rsyslog 
```
sudo service rsyslog restart
ll /var/log/messages  # to check the file is generated
```
    
  

## Issues

#### apt-install is not available

#### pip commands are not working at all

    https://richwind.co.kr/172


  
