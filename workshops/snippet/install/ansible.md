<!-- Code for installing Ansible -->
### Installing Ansible

The following are steps to install Ansible in your Linux host.

Use the command below

`wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Advanced-Observability/master/setup_ansible.sh | bash`

Positive
: Once it's finished, run the following command to check the status of ansible 
`ansible --version`

You should see that it's successfully installed along with it's dependencies

```bash
ansible 2.9.13
config file = /etc/ansible/ansible.cfg
configured module search path = [u'/home/advanced-observability-workshop/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
ansible python module location = /usr/lib/python2.7/dist-packages/ansible
executable location = /usr/bin/ansible
python version = 2.7.17 (default, Jul 20 2020, 15:37:01) [GCC 7.5.0]
```