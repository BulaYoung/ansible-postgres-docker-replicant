#README
#PostgreSQL Replication Setup with Ansible

This project automates the setup of PostgreSQL replication using Ansible. It configures a master and a replica (slave) PostgreSQL server using Docker containers. The project also includes a playbook to verify the replication by creating a test table on the master and checking its presence on the replica.
project/
├── playbooks/
│   ├── master_setup.yml
│   ├── slave_setup.yml
│   └── check_replication.yml
└── roles/
    ├── master/
    │   ├── tasks/
    │   │   └── main.yml
    │   └── templates/
    │       └── pg_hba.conf.j2
    └── slave/
        ├── tasks/
        │   └── main.yml
        └── templates/
            ├── postgresql.conf.j2
            └── recovery.conf.j2
Playbooks
Master Setup

ansible-playbook master_setup.yml

    Stops any existing PostgreSQL container.
    Removes existing data directory.
    Pulls PostgreSQL Docker image.
    Starts a new PostgreSQL container configured as a master.
    Configures replication settings and creates a replication user.
    Creates a test table and inserts data.

Replica Setup

ansible-playbook slave_setup.yml

    Stops any existing PostgreSQL container.
    Removes existing data directory.
    Pulls PostgreSQL Docker image.
    Performs base backup from the master.
    Starts a new PostgreSQL container configured as a replica.
    Configures replication settings.

Check Replication

ansible-playbook check_replication.yml

    Creates a test table and inserts data on the master.
    Verifies the presence of the test table and data on the replica.

Usage

Set up the master:

bash

    ansible-playbook playbooks/master_setup.yml

Set up the replica:

bash

    ansible-playbook playbooks/slave_setup.yml

Check replication:

bash

    ansible-playbook playbooks/check_replication.yml

Conclusion

This project provides a comprehensive solution to set up and verify PostgreSQL replication using Ansible and Docker. The playbooks automate the entire process, ensuring a consistent and repeatable setup.
