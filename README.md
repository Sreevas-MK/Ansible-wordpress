# Wordpress Installation & Configuration with Ansible

This project automates the installation and configuration of a Wordpress website with MariaDB on an Amazon Linux EC2 instance using Ansible.

## Project Structure

├── hosts                     # Inventory file for Ansible

├── httpd.conf.tmpl           # Apache HTTPD configuration template

├── playbook.yaml             # Main Ansible playbook for setup

├── remove.yaml               # Ansible playbook for cleanup

├── variables.yaml            # Variables file including AWS SecretsManager lookups

├── virtualhost.conf.tmpl     # VirtualHost configuration template

└── wp-config.php.tmpl        # Wordpress wp-config.php template

## Prerequisites

- Ansible installed on the control machine.
- Access to AWS Secrets Manager with a secret containing the DB credentials:
  - `db_root_password`
  - `db_user_name`
  - `db_user_password`
  - `db_name`
- An EC2 instance running Amazon Linux.
- Private key file (`key.pem`) in the same directory as the project (or modify `hosts` file accordingly if located elsewhere).

## Configuration

1. **Inventory File (`hosts`)**  
   - Update the private IP of your EC2 instance.
   - Specify the private key file path if not in the same directory.

[amazon]
<EC2_PRIVATE_IP> ansible_user=ec2-user ansible_port=22 ansible_ssh_private_key_file="key.pem"


2. **Variables (`variables.yaml`)**
   - Update `website_hostname` and `website_documentroot` to your domain.
   - The `db_*` variables automatically fetch credentials from AWS Secrets Manager using the `amazon.aws.aws_secret` lookup.

3. **HTTPD Configuration (`httpd.conf.tmpl`)**
   - Uses `{{ httpd_port }}` from `variables.yaml` for the Listen port.

## Usage

### Run Playbook to Setup Wordpress
ansible-playbook -i hosts playbook.yaml

## Run Playbook to Cleanup
ansible-playbook -i hosts remove.yaml


## Notes

- If your private key file (`key.pem`) is in a different directory, update the `ansible_ssh_private_key_file` path in `hosts`.
- This project assumes you have the required IAM permissions to read from AWS Secrets Manager.
- Apache, PHP, and MariaDB are installed and configured automatically.
- The playbook also sets up a virtual host and document root for your Wordpress site.

## License

This project is for learning purposes and can be freely used or modified.

