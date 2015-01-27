# errpt2logstash
Send events from AIX error report (errpt) to a logstash server

# INSTALL:
cp errpt2logstash.pl /usr/local/bin/errpt2logstash.pl

chown root:system /usr/local/bin/errpt2logstash.pl

chmod 750 /usr/local/bin/errpt2logstash.pl

cp errpt2logstash.conf /etc/errpt2logstash.conf

chown root:system /etc/errpt2logstash.conf

chmod 660 /etc/errpt2logstash.conf

<b>modify the configuration file (errpt2logstash.conf) -> add your logstash server and port</b>

odmadd errpt2logstash.add

# DEINSTALL:
odmdelete -q 'en_name=errpt2logstash' -o errnotify

rm /usr/local/bin/errpt2logstash.pl

rm /etc/errpt2logstash.conf

# [Example] logstash-input.conf
input {
  tcp {
    port => 5555
    type => errpt
    codec => json
  }
} 

# TEST:
Logstash Server:

/opt/logstash/bin/logstash agent -e 'input {tcp { port => "5555" codec => json }} output { stdout { codec => rubydebug }}'

AIX Server:

errlogger "Hello World"

logger -plocal0.crit "Hello World" 
