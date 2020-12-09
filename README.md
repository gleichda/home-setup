# Rasperry Pi Setup

## Preparation

https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview

## Ansible

### Install requirements

```shell
ansible-galaxy install -r requirements.yaml
```

### Ad Hoc commands

```shell
ansible all -a 'hostname' -i hosts.yaml
```

### Playbook

#### Dry Run

```shell
ansible-playbook playbook.yaml --check 
```

#### Execute

```shell
ansible-playbook playbook.yaml
```


### Errors

#### NSCFConstantString initialize

This looks something like:

```bash
objc[84723]: +[__NSCFConstantString initialize] may have been in progress in another thread when fork() was called.
objc[84723]: +[__NSCFConstantString initialize] may have been in progress in another thread when fork() was called. We cannot safely call it or ignore it in the fork() child process. Crashing instead. Set a breakpoint on objc_initializeAfterForkError to debug.
```

Try with `export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES`

#### Bad Request for Github

This looks something like:

```shell
fatal: [10.1.1.20]: FAILED! => {"msg": "An unhandled exception occurred while running the lookup plugin 'url'. Error was a <class 'ansible.errors.AnsibleError'>, original message: Received HTTP error for https://github.com/prometheus/prometheus/releases/download/v2.23.0/sha256sums.txt : HTTP Error 400: Bad Request"}
```

Make sure your netrc does not contain GitHub... E.g. via:

```shell
unset NETRC
ansible-playbook playbook.yaml

# Or just for the command:

NETRC= ansible-playbook playbook.yaml
```

### Run ansible in Docker

```shell
docker run --rm -it -v $PWD:/playbook -v $HOME/.ssh:/root/.ssh -w /playbook --entrypoint bash python:3.9-buster

pip install -r python-requirements.txt

ansible-galaxy install -r requirements.yaml

ansible-playbook playbook.yaml
```
