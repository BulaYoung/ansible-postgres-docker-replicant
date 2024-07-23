PostgreSQL 14 Replication Setup with Ansible

This project automates the setup of PostgreSQL replication using Ansible. It configures a master and a replica (slave) PostgreSQL server using Docker containers. The project also includes a playbook to verify the replication by creating a test table on the master and checking its presence on the replica.

Master Setup

master_setup.yml

    Stops any existing PostgreSQL container.
    Removes existing data directory.
    Pulls PostgreSQL Docker image.
    Starts a new PostgreSQL container configured as a master.
    Configures replication settings and creates a replication user.
    Creates a test table and inserts data.

Replica Setup

slave_setup.yml

    Stops any existing PostgreSQL container.
    Removes existing data directory.
    Pulls PostgreSQL Docker image.
    Performs base backup from the master.
    Starts a new PostgreSQL container configured as a replica.
    Configures replication settings.

Check Replication

check_replication.yml

    Creates a test table and inserts data on the master.
    Verifies the presence of the test table and data on the replica.

Usage
First thing set ip addresses for master and slaves in roles/master/templates/pg_hba.conf.j2
roles/slave/templates/postgresql.conf.j2

Change password from default in master_setup.yml and slave_setup.yml

Set up the master:

    ansible-playbook master_setup.yml

Set up the replica:


    ansible-playbook slave_setup.yml

Check replication:


    ansible-playbook check_replication.yml

Clean all conteiners 

    ansible-playbook teardown.yml
Conclusion

This project provides a comprehensive solution to set up and verify PostgreSQL replication using Ansible and Docker. The playbooks automate the entire process, ensuring a consistent and repeatable setup.
