name: Install Terraform and Vault
  hosts: all
  become: yes

  tasks:
    - name: Add HashiCorp GPG key
      apt_key:
        url: https://apt.releases.hashicorp.com/gpg
        state: present

    - name: Add HashiCorp APT repository
      apt_repository:
        repo: "deb [arch=amd64] https://apt.releases.hashicorp.com {{ ansible_distribution_release }} main"
        state: present

    - name: Install Terraform
      apt:
        name: terraform
        state: latest

    - name: Install Vault
      apt:
        name: vault
        state: latest

  ansible-playbook install-terraform-vault.yml


---
- name: Example Playbook
  hosts: all

  vars:
    command: "s3 ls"
    key: "region"
    value: "us-west-2"

  tasks:
    - name: Run AWS CLI command with key-value pairs
      include_role:
        name: aws
      vars:
        command: "{{ command }}"
        key: "{{ key }}"
        value: "{{ value }}"
ansible-playbook playbook.yml
ansible-playbook playbook.yml --extra-vars "key=us-east-1 value=my-bucket-name"


---
- name: Example Role
  hosts: all

  tasks:
    - include_tasks: aws_task.yml
      vars:
        command: "s3 ls"
        key: "region"
        value: "us-west-2"
