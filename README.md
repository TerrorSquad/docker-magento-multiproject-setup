# Docker Magento Multi project setup starter - Linux

## Description

This ansible playbook will set up multiple Magento 2 projects on your local machine.

For each project, it will:

- Download a wrapper docker repo - <https://github.com/TerrorSquad/docker-magento-quick-install>
- Clone the projects repo into the wrapper/src directory  and checkout the specific branch (usually master)
- Download docker images and bring up docker containers
- Install composer packages
- Import the database dump
- Change base URLs in the database to https://magento.test
- Configure nginx to work with multistore websites
- Create admin panel user
- Bring the containers down
- Move to the next project

Once it's done, you will have all your projects ready to go.
To bring a project up, enter the project directory e.g. `cd ~/Projects/Magento/Projects/mystore` and run the start script `bin/start`.

### Requirements

Software:

- Linux OS (tested on Linux Mint 20.2) anything based on Ubuntu 20.04 or later should work without issue. Quite possibly on older versions as well.
- docker
- docker-compose
- git
- ansible
- jq

Projects

- Code access
- Database access

### How to use this repo?

- Fork this repo and download it locally
- Create your projects definitions in `roles/ubuntu/vars/projects/magento2`
  - You can use the `sample` project as a base for your own project
- Create a _Projects_ directory where you will hold all your Magento projects, e.g. `mkdir -p ~/Projects/Magento/Projects`
- Download the database dumps for all the projects you want to set up. By default the path where all database files are expected to be located is `~/Projects/Magento/Databases`
- Run the ansible play `ansible-playbook ./site.yml -K -e project=project_name` to set up a project.
- You can also set up all projects at once by running `ansible-playbook ./site.yml  -K -e all=true`
- You can also pass the `-e force_recreate=yes` parameter to force the projects to be recreated. This will override project.force_recreate variable.

#### Notes

- OPTIONAL: Edit the `roles/ubuntu/var/main.yml` and modify the projects_directory variable
- OPTIONAL: Edit the `roles/ubuntu/var/projects/*/project.yml` files and configure the database path for all projects you wish to set up.
- OPTIONAL: Edit the file `roles/ubuntu/var/main.yml` file and choose which projects you wish to exclude from setting up.
- OPTIONAL: Change the details for the default admin panel user that will be created for each project.

# TODO

- Implement `-e exclude=project_name -e exclude=project2_name`
- This could also be `-e exclude="project_name,project2_name"` - string of comma separated list of projects to exclude. Or even space separated list of projects to exclude.
- Decide whether to leave out the `exlude_projects` list that exists in  `roles/ubuntu/var/main.yml`. If it stays, decide whether to add a `--merge-excludes` option to the `ansible-playbook` command. This would allow you to have a list of projects to exclude that is different from the default list.
