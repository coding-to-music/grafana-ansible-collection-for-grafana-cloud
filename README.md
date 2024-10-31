# grafana-ansible-collection-for-grafana-cloud

# 🚀 Setup multiple servers with grafana-metrics-collector to grafana-cloud using Ansible 🚀

https://github.com/coding-to-music/grafana-ansible-collection-for-grafana-cloud

From / By https://grafana.com/blog/2022/09/01/how-to-get-started-with-the-new-grafana-ansible-collection-for-grafana-cloud/

## GitHub

```java
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/grafana-ansible-collection-for-grafana-cloud.git
git push -u origin main
```

## Environment variables:

```java

```

# How to get started with the new Grafana Ansible collection for Grafana Cloud

By Ishan Jain

Ishan Jain • 2022-09-01 • 3 min

More than 20,000 companies around the world use Ansible as their Infrastructure as Code and configuration management tool. With the rising popularity towards managing infrastructure using IaC and config management tools, Ansible is one of the best open source tools to choose from.

That is why we are excited to announce a new Grafana Ansible collection available to all Grafana Cloud users, including those in the generous free tier. (If you don’t already have a Grafana Cloud account, you can sign up for free today!). The new Grafana Ansible collection supports managing various resources in Grafana Cloud, such as plugins, dashboards, API keys, and much more.

# How to install the Grafana Ansible collection for Grafana Cloud

The Grafana Ansible collection is available on Ansible Galaxy.

You can install the collection using the command below:

```java
ansible-galaxy collection install grafana.grafana
```

To install a particular version of the Ansible collection, use the command below:

```java
ansible-galaxy collection install grafana.grafana:1.0.0
```

You can also include it in a requirements.yml file and install it via

```java
ansible-galaxy collection install -r requirements.yml
```

using the format:

```java
yaml

---

collections:

- name: grafana.grafana
```

# Grafana Ansible collection modules

The Grafana Ansible collection currently has eight modules. These modules are currently offered to manage resources on Grafana Cloud only:

- grafana.grafana.alert_contact_point - Manage Grafana Alerting contact points in Grafana.
- grafana.grafana.alert_notification_policy - Sets the notification policy tree in Grafana Alerting.
- grafana.grafana.cloud_api_key – Manage Grafana Cloud API keys.
- grafana.grafana.cloud_plugin – Manage Grafana Cloud plugins.
- grafana.grafana.cloud_stack – Manage Grafana Cloud stack.
- grafana.grafana.dashboard – Manage dashboards in Grafana.
- grafana.grafana.datasource – Manage data sources in Grafana.
- grafana.grafana.folder – Manage folders in Grafana.

More modules will be added as we grow the Grafana Ansible collection.

# Start using the Grafana Ansible collection

Once you have the Grafana Ansible collection installed, you can start using the modules available in the collection. You can call modules by their Fully Qualified Collection Namespace (FQCN), such as grafana.grafana.cloud_stack:

```java
- name: Using grafana collection
  hosts: localhost
  tasks: - name: Create a Grafana Cloud stack
  grafana.grafana.cloud_stack:
  name: mystack
  stack_slug: mystack
  org_slug: myorg
  cloud_api_key: "{{ cloud_api_key }}"
  region: eu
  state: present
```

You can learn more by reading the documentation for the Grafana Ansible collection.

## Learn more about the new Grafana Ansible collection

The new Grafana Ansible collection is available for users with a Grafana Cloud account. (Don’t have a Grafana Cloud account? Sign up for free today!)

As we release the first major version of the Grafana Ansible collection and continue to improve upon it, we encourage users to participate in the project, for example:

Submit bugs and feature requests, and help us verify them.
Submit and review source code changes in GitHub pull requests.
Add new modules for more Grafana resources.
For more information or to get started, be sure to check out the Grafana Ansible collection GitHub repository or contact our team.

If you’re not already using Grafana Cloud — the easiest way to get started with observability — sign up now for a free 14-day trial of Grafana Cloud Pro, with unlimited metrics, logs, traces, and users, long-term retention, and access to one Enterprise plugin.

# Install or uninstall Grafana Alloy using Ansible

https://grafana.com/docs/alloy/latest/set-up/install/ansible/

You can use the Grafana Ansible Collection to install and manage Alloy on Linux hosts.

### Before you begin

These steps assume you already have a working Ansible setup and a pre-existing inventory.
You can add the tasks below to any new or existing role.

Steps

To add Alloy to a host:

Create a file named `alloy.yml` and add the following:

```yaml
- name: Install Alloy
  hosts: all
  become: true

  tasks: - name: Install Alloy
  ansible.builtin.include_role:
  name: grafana.grafana.alloy
  vars:
  config: |
  prometheus.scrape "default" {
  targets = [{"__address__" = "localhost:12345"}]
  forward_to = [prometheus.remote_write.prom.receiver]
  }
  prometheus.remote_write "prom" {
    endpoint {
    url = "<YOUR_PROMETHEUS_PUSH_ENDPOINT>"
      }
    }
```

This snippet has a sample configuration to collect and send Alloy metrics to Prometheus

Replace the following:

`<YOUR_PROMETHEUS_PUSH_ENDPOINT>`: The Remote write endpoint of your Prometheus Instance.

Run the Ansible playbook. Open a terminal window and run the following command from the Ansible playbook directory.

```java
ansible-playbook alloy.yml
```

Validate

To verify that the Alloy service on the target machine is active and running, open a terminal window and run the following command:

```bash
sudo systemctl status alloy.service
```

If the service is active and running, the output should look similar to this:

```java
alloy.service - Grafana Alloy
  Loaded: loaded (/etc/systemd/system/alloy.service; enabled; vendor preset: enabled)
  Active: active (running) since Wed 2022-07-20 09:56:15 UTC; 36s ago
Main PID: 3176 (alloy-linux-amd)
  Tasks: 8 (limit: 515)
  Memory: 92.5M
    CPU: 380ms
  CGroup: /system.slice/alloy.service
    └─3176 /usr/local/bin/alloy-linux-amd64 --config.file=/etc/grafana-cloud/alloy-config.yaml
```

Next steps
https://grafana.com/docs/alloy/latest/configure/linux/

# Configure Grafana Alloy on Linux

To configure Alloy on Linux, perform the following steps:

Edit the default configuration file at `/etc/alloy/config.alloy`.

Run the following command in a terminal to reload the configuration file:

```bash
sudo systemctl reload alloy
```

To change the configuration file used by the service, perform the following steps:

Edit the environment file for the service:

Debian or Ubuntu: edit `/etc/default/alloy`
RHEL/Fedora or SUSE/openSUSE: edit `/etc/sysconfig/alloy`
Change the contents of the CONFIG_FILE environment variable to point at the new configuration file to use.

Restart the Alloy service:

```bash
sudo systemctl restart alloy
```

## Pass additional command-line flags

By default, the Alloy service launches with the run command, passing the following flags:

```bash
--storage.path=/var/lib/alloy
```

To pass additional command-line flags to the Alloy binary, perform the following steps:

Edit the environment file for the service:

Debian-based systems: edit /etc/default/alloy

RedHat or SUSE-based systems: edit /etc/sysconfig/alloy

Change the contents of the CUSTOM_ARGS environment variable to specify command-line flags to pass.

```bash
Restart the Alloy service:
```

```bash
sudo systemctl restart alloy
```

To see the list of valid command-line flags that can be passed to the service, refer to the documentation for the run command.

## Expose the UI to other machines

By default, Alloy listens on the local network for its HTTP server. This prevents other machines on the network from being able to access the UI for debugging.

To expose the UI to other machines, complete the following steps:

Follow Pass additional command-line flags to edit command line flags passed to Alloy

Add the following command line argument to CUSTOM_ARGS:

```bash
--server.http.listen-addr=<LISTEN_ADDR>:12345
```

Replace the following:

`<LISTEN_ADDR>`: An IP address which other machines on the network have access to. For example, the IP address of the machine Alloy is running on.

To listen on all interfaces, replace `<LISTEN_ADDR>` with `0.0.0.0.`

# how would I have the secrets stored in a .env or other place for ansible

Storing secrets securely is crucial. Here's a couple of ways to manage secrets for Ansible:

## Method 1: Using Ansible Vault

Create an Encrypted File:

Use Ansible Vault to create a .yml file to store your secrets.

```bash
ansible-vault create secrets.yml
```

Add your secrets in the secrets.yml file:

```yaml
grafana_username: your_username
grafana_api_key: your_api_key
```

Referencing Vaulted Variables in Playbook:

Edit your playbook to reference these vaulted variables:

```yaml
---
- name: Install and configure Grafana Alloy Agent
  hosts: all
  become: yes
  vars_files:
    - secrets.yml
  tasks:
    - name: Create Grafana Alloy configuration file
      copy:
        dest: /etc/grafana-agent/config.yml
        content: |
          # Grafana Agent Configuration
          metrics:
            wal_directory: /var/lib/grafana-agent
            global:
              scrape_interval: 60s
            configs:
              - name: integrations
                remote_write:
                  - url: https://prometheus-us-central1.grafana.net/api/prom/push
                    basic_auth:
                      username: "{{ grafana_username }}"
                      password: "{{ grafana_api_key }}"
```

Run the Playbook:

```bash
ansible-playbook your_playbook.yml --ask-vault-pass
```

## Method 2: Using Environment Variables

Create a .env File:

Add your secrets to a .env file.

```bash
# .env
GRAFANA_USERNAME=your_username
GRAFANA_API_KEY=your_api_key
```

Reference Environment Variables in Playbook:

Edit your playbook to reference these environment variables.

```yaml
---
- name: Install and configure Grafana Alloy Agent
  hosts: all
  become: yes
  tasks:
    - name: Create Grafana Alloy configuration file
      copy:
        dest: /etc/grafana-agent/config.yml
        content: |
          # Grafana Agent Configuration
          metrics:
            wal_directory: /var/lib/grafana-agent
            global:
              scrape_interval: 60s
            configs:
              - name: integrations
                remote_write:
                  - url: https://prometheus-us-central1.grafana.net/api/prom/push
                    basic_auth:
                      username: "{{ lookup('env', 'GRAFANA_USERNAME') }}"
                      password: "{{ lookup('env', 'GRAFANA_API_KEY') }}"
```

Source the .env File:

```bash
export $(grep -v '^#' .env | xargs)
```

Run the Playbook:

```bash
ansible-playbook your_playbook.yml

ansible-playbook -i inventory configure-alloy.yml 
```

Both methods will help you manage your secrets securely and integrate them into your Ansible playbooks. 
