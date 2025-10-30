## Infrastructure

This is WIP.

## Ansible setup

In WSL..

# Python 3.13 install

```
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.13 python3.13-ven -y
```

# Python 3.13 venv setup

```
cd ~
python3.13 -m venv py313
source ~/py313/bin/activate
pip install --upgrade pip
echo 'source ~/py313/bin/activate' >> ~/.bashrc
```

# Python deps

This should be within the folder this README.md lives in.

For the sake of keeping it simple, we'll assume this repo is cloned into the following folder: `~/git/pompyboard`

```
cd ~/git/pompyboard/infra/ansible
pip install -r requirements.txt
```

# Ansible collections and roles

Install ansible collections and roles:

```
ansible-galaxy install -r requirements.yml --force
```

# Adding or creating roles

Ideally you can find pre-existing roles on [Galaxy](https://galaxy.ansible.com/) which would be preferred as it saves time.

Although, you can create a fresh role ansible-galaxy's CLI tool:

```
ansible-galaxy role init roles/<role name>
```
