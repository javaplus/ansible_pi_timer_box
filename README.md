# ansible_pi_timer_box

Before running scripts need to set up pi to connect to network

then add your ansible servers public key to the client with this command:

ssh-copy-id -i ~/.ssh/id_rsa.pub [userid]@[IP of new client]
```
EX: ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.1.117
```

Run the ssh-add command with no arguments to add the key so you don't have to keep typing password:
```
ssh-add 
```
If you see this error when running ssh-add command: *Could not open a connection to your authentication agent.*, then run this command:
```
eval $(ssh-agent)
```


Run ansible recipes like this:

ansible-playbook adafruit.yml -v

the -v means verbose

Update the /etc/ansible/hosts file to add clients

Reboot all:

ansible all -a "/sbin/reboot" --become -f 5 
