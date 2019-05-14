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

  Chef client
  - To boot strap a node we do this through the CDK
  `knife bootstrap elbanby1c.mylabserver.com -N '<give it a name>' -x <user-name-on-that-machine> -P '<there-admin-password>' --sudo
`
  - To confirm use `knife node list`
  - To create a cookbook
  `chef generate cookbook`
  - To run one the cook book locally for testing
  `sudo chef-client --local-mode ./recipes/default.rb`

  - To upload a cookbook from host to chef server
  `knife upload cookbooks/<your-cookbook>`

  ```In case you run into an error try this
  cd ~/generated-chef-repo
  mkdir .chef
  echo 'cookbook_path ["#{File.dirname(__FILE__)}/../cookbooks"]' > .chef/knife.rb
  knife ssl fetch
  ```

  - To check your run lists
  `knife node show <node-name>`

  - To add a runlist
  `knife node run_list add web-node1 'recipe[firt_cookbook_nginx::default]'`

  - To overwirte the runlist
  `knife node run-list set <node-name> <cookbook-name>`

  - To run knife ssh based on a query
  `knife ssh 'name:web-node1' 'sudo chef-client' -x user`

  - To reference a file in your recipe

  ```
    #             to location
    cookbook_file "/etc/vimrc" do
    # From location
      source "default/vimrc"
    end
  ```

  - To create a file an reference its content for example vimrc

   * `chef generate file vimrc`  // file/default/vimrc is the result

   * `vim files/default/vimrc` // configure as you please

   * `sudo chef-client --local-mode -o 'recipe[first_cookbook_tooling::vim_recipe]'` run the specified recipe but use -o to reference a run list, since we have referenced files.

  - To create a user recipe in chef
  ```
  user "jenkins" do
    comment "A jenkins user for CI/CD"
    password "secure_password"
  end
  ```

  - To create role in chef (a repeatbale runalist)
  ` knife role create base`

  - Add your recipe to the roles of the base file

  - To add a role to a recipe thats already defined
  `knife node run_list add web-node1 'role[base]' --before 'recipe[firt_cookbook_nginx::default]'`
