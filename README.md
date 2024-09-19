
# Setup GitLab Runner with Docker Compose using Ansible

This project provides an Ansible role to set up a GitLab Runner using Docker Compose.


## Prerequisites

- **Install Docker:** Ensure Docker is installed on your GitLab Runner server.

- **Ansible:** Ensure Ansible is installed on your system.

- **GitLab Registration Token-** from your GitLab instance. You can obtain this token from the **CI/CD settings** of your GitLab project.
- **SSH -**  You should have ssh access to your GitLab Runner server.

- **Install the community.docker Collection -** You can install community.docker collection with this command:

```bash
ansible-galaxy collection install community.docker 
```

## Setup Instructions

### Step 1: Clone the Repository

To begin, clone this repository to your local machine:

```bash
git clone https://github.com/siavashmhi/Autorunner.git
cd Autorunner
```

### Step 2: Change runner.yml variable file.

You have to set your gitLab url and registration token in this file.

```bash
cat inventory/group_vars/all/runner.yml

gitlab_instance_url: "https://gitlab.com/"  # Your GitLab instance URL (change)
gitlab_register_token: "your_token"  # Replace with your GitLab access token (change)
gitlab_runner_image: "gitlab/gitlab-runner:latest"
gitlab_runner_name: "ansible-runner" # docker container name 
docker_executor_image: "docker:latest"
runner_executor: "docker"
runner_description: "your_description"
runner_tag: "ansible_runner" # runner tag
```

### Step 3: Change hosts.yml file.

You have to set GitLab Runner ip address in this file.

```bash
cat inventory/hosts.yml 

all:
  children:
    gitlab-runners:
      hosts:
        runner-server:
          ansible_host: 192.168.1.1 #change
          ansible_user: root #change
          ansible_port: 22 #change

```

### Step 4: Run ansible-playbook command for setup GitLab Runner.

```bash
# check your server access.
ansible all -m ping -i inventory/hosts.yml

# run ansible playbook. 
ansible-playbook -i inventory/hosts.yml playbooks/runner.yml
```