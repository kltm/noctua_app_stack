# Noctua App Stack

Install app stack using ansible on a single machine

### Requirements 

- The steps below were successfully tested using:
    - MacOs (10.15.3)
    - Docker (19.03.5)
    - Docker Compose (1.25.2)
    - Ansible (2.10.3), Python (3.8.5), docker (4.3.1)
    
    Note: Docker was given 3 CPUs and 8G RAM. (on mac see Docker Preferences | Resources)

#### Installing ansible and docker dependencies

The ansible docker plugin is needed to buid images ...

```sh
pip install ansible
pip install docker 
```

#### Deploying app stack: 

Clone this repo.

```sh
git clone https://github.com/abessiari/noctua_app_stack.git
cd noctua_app_stack
```

Modify vars.yaml as needed. Minimally you need to modify the following variables:
  - uri
  - username
  - password
  - barista_lookup_host
    - On mac if using wireless, you can use `ipconfig getifaddr en0`

Clone noctua/minerva git repositorie and build noctua and minerva docker images

```sh
ansible-playbook build_images.yaml
docker image list | grep minerva
docker image list | grep noctua 
```

Staging artifacts needed by stack.
  - Build and stage the journal.
  - Stage repos
    - noctua-form, noctua-landing-page, noctua-models, go-site

```sh
ansible-playbook stage.yaml
```

Bring up stack using docker compose.

```sh
# assuming stage_dir is in current directory
docker-compose -f stage_dir/docker-compose.yaml up -d
```

Bring down stack using docker compose.

```sh
# assuming stage_dir is in current directory
docker-compose -f stage_dir/docker-compose.yaml down
```
