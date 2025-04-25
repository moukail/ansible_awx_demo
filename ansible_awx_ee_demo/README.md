https://ansible.readthedocs.io/projects/builder/en/latest/
https://ansible.readthedocs.io/projects/navigator/
https://pipx.pypa.io/stable/

### navigator
```bash
pipx install ansible-navigator
ansible-navigator --version

ansible-navigator collections --execution-environment-image ghcr.io/ansible-community/community-ee-base:latest
ansible-navigator exec "ansible localhost -m setup" --execution-environment-image ghcr.io/ansible-community/community-ee-minimal:latest --mode stdout
ansible-navigator run test_localhost.yml --execution-environment-image ghcr.io/ansible-community/community-ee-minimal:latest --mode stdout --container-options='--user=0'

ansible-navigator run test_localhost.yml --execution-environment-image postgresql_ee --mode stdout --pull-policy missing --container-options='--user=0'
ansible-navigator run test_remote.yml -i inventory --execution-environment-image postgresql_ee:latest --mode stdout --pull-policy missing --enable-prompts -u root -k -K
```

### set up virtual environment to install ansible-builder
```bash
python3 -m venv .venv
source .venv/bin/activate
pip3 install ansible-builder
# or
pipx install ansible-builder
ansible-builder --version
```

###
```bash
ansible-builder build --tag postgresql_ee --container-runtime docker

docker images
docker tag postgresql_ee docker.io/moukail/postgresql_ee:latest
docker login
docker push docker.io/moukail/postgresql_ee:latest

docker run -it moukail/postgresql_ee
$ ansible --version
$ ansible-galaxy collection list
```

### test
```bash
ansible-navigator run your-playbook.yml \
  --execution-environment \
  --container-engine docker \
  --eei my-awx-ee
```