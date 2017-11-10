# ansible_pi_timer_box

Before running scripts need to set up pi to connect to network

then add your ansible servers public key to the client with this command:

ssh-copy-id -i ~/.ssh/id_rsa.pub [IP of new client]

Run this command to add the key so you don't have to keep typing password:
ssh-add 
If that can't connect to ssh agent start it like this:
eval $(ssh-agent)


Run ansible recipes like this:

ansible-playbook adafruit.yml -v

the -v means verbose

Update the /etc/ansible/hosts file to add clients
