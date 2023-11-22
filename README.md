# Ansible Flask Deployment Project README

This README provides detailed instructions and an overview of the Ansible Flask Deployment project. The project is structured to deploy a Flask application (The Twoge Blogging App) using Ansible, covering steps from setting up the environment to running the application with Gunicorn.

Ansible build is designed to set up a Flask application served by Gunicorn on a EC-2 Unbuntu server acting as a target node with the Ansible playbook ran from another Unbuntu instance control node or a local machine.

## Project Structure

```
ansible_twoge/
│
├── inventory.ini  # Hosts inventory file
│
├── playbooks/
│   ├── twoge.yml  # Main playbook
│   ├── update-nodes.yml  # Updates packages on target nodes
│   ├── install-packages.yml  # Installs required packages
│   └── setup-flask-app.yml  # Sets up Flask application
│
├── templates/
│   ├── gunicorn.service.j2  # Template for Gunicorn service
│   └── env.j2  # Template for .env file
│
└── vars/
    └── vars.yml  # Variable definitions
```

## Steps to Setup and Run the Project

### 1. Update `vars.yml`

   - Define all necessary variables, including `git_repo`, `project_path`, database credentials, Flask and Gunicorn configurations.
   - Generate a secure Flask secret key using:
     ```
     python -c 'import secrets; print(secrets.token_hex(25))'
     ```

### 2. Prepare Templates

   - Create `gunicorn.service.j2` for the Gunicorn service configuration.
   - Create `env.j2` for setting up environment variables on target nodes.

### 3. Inventory Setup (`inventory.ini`)

   - Configure the `inventory.ini` with the details of your target hosts and SSH settings.

### 4. Running Playbooks

   - Execute the main playbook (`twoge.yml`) to deploy the Flask application:
     ```
     ansible-playbook -i inventory.ini playbooks/twoge.yml
     ```

---

### SCP Command with Dummy Data:

```bash
scp -i /path/to/your/aws_key.pem -r /path/to/your/ansible_project ubuntu@your.server.ip.address:~
```

- `/path/to/your/aws_key.pem`: Replace this with the actual path to your AWS SSH private key.
- `/path/to/your/ansible_project`: Replace this with the path to your local Ansible project directory.
- `ubuntu`: The username used to log into the remote server.
- `your.server.ip.address`: Replace this with the IP address of your remote server.

### SSH Command with Dummy Data:

```bash
ssh -i /path/to/your/aws_key.pem ubuntu@your.server.ip.address
```

- `/path/to/your/aws_key.pem`: Again, this is the path to your AWS SSH private key.
- `ubuntu`: The username for the remote server.
- `your.server.ip.address`: The IP address of the remote server.

These commands demonstrate how to copy files from a local machine to a remote server using SCP and how to connect to a remote server using SSH, with placeholders indicating where users should insert their specific details.

Transfer your Ansible project files to a remote server (e.g., an AWS EC2 instance) using SCP:

---
### So I had my info below and it didnt really expose anything but I removed it just to be safe, public repo and all but you all have seen this many times. Anyways this is just a repeat of above.
1. **Run SCP Command**:

   ```
   scp -i /path/to/your/aws_key.pem -r /path/to/your/ansible_project ubuntu@your.server.ip.address:~

   ```

2. **SSH into EC2 Instance**:

   ```
   ssh -i /path/to/your/aws_key.pem ubuntu@your.server.ip.address

   ```

3. **Verify File Transfer**:

   Use the `ls` command to verify the files in the `ansible_twoge` directory.

4. **Run Ansible Playbook**:

   ```
   ansible-playbook -i inventory.ini playbooks/twoge.yml
   ```

## Additional Helpful Terminal Commands

- **SSH Key Permissions**:
  
  Set the correct permissions for your SSH key:
  ```
  chmod 600 /path/to/your/ssh_key.pem
  ```

- **Checking Ansible Version**:

  Ensure you're using the correct version of Ansible:
  ```
  ansible --version
  ```

- **Listing Ansible Tasks**:

  List all tasks in a playbook:
  ```
  ansible-playbook playbooks/twoge.yml --list-tasks
  ```

- **Debugging Playbook Runs**:

  Run Ansible playbook in verbose mode for detailed output:
  ```
  ansible-playbook -i inventory.ini playbooks/twoge.yml -vvv
  ```

## Troubleshooting

- If you encounter any issues, check the error messages and try to resolve them based on the provided details.
- For more specific problems, please consider using the verbose mode (`-vvv`) with Ansible commands to get detailed information as this helped me alot a decent amount.

---

This README serves as a comprehensive guide for setting up, transferring, and deploying your Flask application using Ansible. It includes helpful terminal commands for tasks such as file transfer, secret key generation, and playbook execution. The guide is designed for easy copying and pasting of commands, facilitating a smoother workflow for students and practitioners.
