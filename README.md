# PostgreSQL 12 Replication with Ansible and Docker

# Available same project on PostgreSQL 14 in second branch

This project provides a comprehensive solution to set up and verify PostgreSQL replication using Ansible and Docker. The playbooks automate the entire process of configuring master and slave servers, as well as verifying replication.

# Features

- Sets up a new PostgreSQL container as the master.
- Sets up a new PostgreSQL container as a replica.
- Configures replication settings.
- Verifies replication.
- Configures SSL for secure data transmission.

# Prerequisites

- Docker
- Ansible
- OpenSSL (for generating SSL certificates)

# Usage

Configuration

1. Set the IP addresses for the master and slaves in the following files:
   - `roles/master/templates/pg_hba.conf.j2`
   - `roles/slave/templates/postgresql.conf.j2`

2. Change the default password in `master_setup.yml` and `slave_setup.yml`.

3. Generate SSL certificates and place them in the appropriate directory on the master server.

Generate SSL Certificates

Run the following commands on the master server to generate self-signed SSL certificates:


# Create a directory for certificates
```bash
mkdir -p /var/lib/postgresql/certs
```
# Navigate to the directory
```bash
cd /var/lib/postgresql/certs
```
# Generate a private key
```bash
openssl genrsa -out server.key 2048
```
# Restrict access to the private key
```bash
chmod 600 server.key
```
# Generate a self-signed certificate
```bash
openssl req -new -x509 -key server.key -out server.crt -days 3650 -subj "/CN=$(hostname)"
```
### Set up the Master

Run the following command to set up the master server:

```bash
ansible-playbook master_setup.yml -i inventory
```

### Set up the Replica

Run the following command to set up the replica server(s):

```bash
ansible-playbook slave_setup.yml -i inventory
```

### Check Replication

Run the following command to create a test table, insert data on the master, and verify the presence of the test table and data on the replica:

```bash
ansible-playbook check_replication.yml -i inventory
```

### Clean All Containers

Run the following command to clean up all containers:

```bash
ansible-playbook teardown.yml -i inventory
```

### Conclusion

This project provides a comprehensive solution to set up and verify PostgreSQL replication using Ansible and Docker. The playbooks automate the entire process, ensuring a consistent and reliable configuration.

### Additional Notes

- Ensure that the SSL certificates are correctly copied to the master and slave containers.
- The `pg_hba.conf` and `postgresql.conf` files must be correctly configured to enable SSL and allow replication connections.
- Monitor the replication status and logs to troubleshoot any issues that may arise.


### Summary of Changes

1. Added a section on generating SSL certificates.
2. Provided instructions for setting up SSL in `pg_hba.conf` and `postgresql.conf`.
3. Included commands for setting up the master, replica, checking replication, and cleaning up containers.
4. Added additional notes for SSL configuration and troubleshooting.

This updated `README.md` should help users understand how to configure and use SSL for secure PostgreSQL replication.
