# Learning Chef

Chef server
 - Install using `curl -O <url>`
 - `rpm -Uvh <package-downloaded>`
 - `chef-server-ctl reconfigure` To run some cook books to install the chef server
 - To check which services are running `chef-server-ctl service-list`
 - To create a user `chef-server-ctl user-create <user-name> <first-name> <last-name> '<email>' '<password>' --filename <path/pemfile.pem (this must exist on current host)>`

 - To create an Org `chef-server-ctl org-create <orgname> '<org full name>' --association_user <admin-username> --filename <path/org-validator.pem>`

 - To install management GUI for chef server
 `chef-server-ctl install chef-manage` then `chef reconfigure`
 `chef-manage-ctl reconfigure` then `yes`


ChefDK
  - Install the package using `curl -O <url>`
  - `sudo rpm -Uvh <package-downloaded>`
  - `chef -v`
  - to make sure configs are correct including ruby `chef shell-init bash`
  - to run the configs `eval "$(chef shell-init bash)"`
  - Add to .bash_profile `eval "$(chef shell-init bash)" >> ~/.bash_profile`
  - `chef generate --help`
  - to generate a repo where we store cookbooks`chef generate repo`
  - Use knife as the point of contact to the server. It id responsible for infra
  - `knife configure`
  - The organization URL `https://elbanby2c.mylabserver.com/organizations/testorg`
  - `scp <user>@<ip>:<path/pemfile.pem> <whatever path kenife specify typically>`
  - `knife ssl fetch` to solve the open SSL certificates problem
