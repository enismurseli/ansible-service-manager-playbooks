# Ansible Playbook: service-restart-windows

This Ansible playbook is used to restart a service on the specified target hosts.

## Playbook Variables

The playbook expects the following variables to be defined:

- `target_name`: The name of the target hosts where the service should be restarted.
- `service_map`: A mapping of service names and their corresponding details.
- `service_map_key`: The key used to access the specific service details in the `service_map`.
- `requester_mail`: Email address of the requester (optional).

## Playbook Steps

1. Set facts:
   - `wapp_sync`: A variable set to "no" by default.
   - `show_details_callback`: Set to `true`.
   - `send_mail_callback`: Set to `true`.
   - `mail_to`: List of email addresses of the requester.

2. Block of tasks:
   - Get the current status of the service specified in the `service_map`.
   - Stop the service if it is currently running.
   - Pause for 60 seconds.
   - Get the service status again.
   - Fail the playbook if the service is still running.
   - Start the service if it was successfully stopped.
   - Get the service status after starting.
   - Fail the playbook if the service is not started.

3. Rescue block (executed only if an exception occurs in the main block):
   - Get the service status again.
   - Get the process ID of the service.
   - Kill the service process with the obtained process ID.
   - Pause for 300 seconds.
   - Get the service status after killing.
   - Fail the playbook if the service is still running.
   - Start the service if it was successfully stopped.
   - Get the service status after starting.
   - Fail the playbook if the service is not started.

## Execution

To execute this playbook, use the following command:

```shell
ansible-playbook -i inventory.ini service_restart.yml

Replace inventory.ini with your inventory file and service_restart.yml with the name of the playbook file.

Note: Make sure you have the necessary permissions to stop and start services on the target hosts.

## Requirements
Ansible 2.10 or later.
Proper network connectivity between the Ansible control node and the target hosts.
License
This playbook is released under the MIT License. See the LICENSE file for more details.