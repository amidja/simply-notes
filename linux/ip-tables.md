
Learn IPTables
https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands	
http://fideloper.com/iptables-tutorial


Save rules to /etc/iptables/iptables.rules
$ sudo iptables-save | sudo tee /etc/iptables/iptables.rules

Restore rules from a file
sudo iptables-restore < /etc/iptables/iptables.rules

Logging Dropped Packets 

Create new chain
sudo iptables -N LOGGING

# Ensure unmatched packets jump to new chain
sudo iptables -A INPUT -j LOGGING

# Log the packets with a prefix
sudo iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables Packet Dropped: " --log-level 7

# Drop those packets
sudo iptables -A LOGGING -j DROP


sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT

sudo iptables -D FORWARD -i wlan0 -o eth0 -j ACCEPT
	

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT

sudo iptables -P FORWARD DROP
sudo iptables -P INPUT DROP

sudo iptables -A FORWARD -i wlan0 -o eth0 -p tcp--dport 22 -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -p udp --dport 53 -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -p tcp --dport 53 -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -p tcp--dport 3128 -j ACCEPT


End State:
-P INPUT ACCEPT
-P FORWARD DROP
-P OUTPUT ACCEPT
-A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -i wlan0 -o eth0 -p tcp -m tcp --dport 22 -j ACCEPT
-A FORWARD -i wlan0 -o eth0 -p udp -m udp --dport 54 -j ACCEPT
-A FORWARD -i wlan0 -o eth0 -p tcp -m tcp --dport 54 -j ACCEPT
-A FORWARD -i wlan0 -o eth0 -p tcp -m tcp --dport 3128 -j ACCEPT

sudo iptables -t nat -S
