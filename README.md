# peryton

Self hosts service

# what is deployed ?

- wireguard + wg-easy : create a vpn to control the environment access
- dnsmask : manage the DNS and allow to use a blacklist
- caddy : reverse prody to services
- gogs : git repository
- fast-QCM : a QCM server to manage student evaluation


# deploy 

ansible-playbook -u user -i hosts playbook.yml 