Steps for executing the script:

Check ssh :
ssh -i ~/.ssh/key-1.pem username@ip
then enable passwordless authentication for all servers
check if control node is able to connect managed nodes using:
ansible -i inventory.ini ubuntu -m ping

when only ubuntu instances are there, you can give directly like this
ansible-playbook -i inventory.ini main.yaml -u ubuntu --ask-become-pass -K

but when multiple flavours of os then:
ansible-playbook -i inventory.ini main.yaml --ask-become-pass -K
and make sure ansible_user=ubuntu is mentioned for ubuntu and ansible_user=ec2-user for amazon-linux in inventory.ini

or if there is a list of ubuntu servers then ansible.cfg can be configured, but that works only if there exists a list of identical flavours of os

What does -K / --ask-become-pass do?
It prompts you to enter the privilege escalation password (usually the sudo password) for the user specified with -u. This is required when your playbook includes tasks that use become: true to escalate privileges (like running as root).

In short:
    • -K = --ask-become-pass
    • It asks for the sudo password of the user username
    • Used when your playbook includes become: true tasks