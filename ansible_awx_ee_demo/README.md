https://ansible.readthedocs.io/projects/builder/en/latest/

### set up virtual environment to install ansible-builder
```bash
python3 -m venv .venv
source .venv/bin/activate
pip3 install ansible-builder
```

###
```bash
ansible-builder build --tag postgresql_ee

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