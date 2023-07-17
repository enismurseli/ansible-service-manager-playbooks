# Ansible Playbook: service-restart-linux

This Ansible playbook is used to stop and start a service on the specified target hosts.

## Playbook Variables

The playbook expects the following variables to be defined:

- `target_name`: The name of the target hosts where the service should be stopped and started.
- `process_filter`: A filter to identify the specific process related to the service.
- `service_name`: The name of the service to be stopped and started.

## Playbook Steps

1. Set facts:
   - `string1`: A command string to retrieve the running processes related to the service.
   - `string2`: A command string to extract the process IDs (PIDs) from the output.

2. Block of tasks:
   - Stop the service using the `service` command and wait for it to stop.
   - Check if any processes are still running for the service.
   - Fail the playbook if there are multiple processes running.
   - Rescue block (executed only if an exception occurs in the main block):
     - Kill the running processes using the `kill` command with signal -15 (SIGTERM).
     - Wait for the processes to be terminated.
     - Forcefully kill any stuck processes using signal -9 (SIGKILL).
     - Check if any processes are still running after killing.
     - Fail the playbook if there are still running processes.

3. Start the service using the `service` command.
   - Check if the service is running by retrieving the processes related to it.
   - Fail the playbook if there are no processes or multiple processes running.

## Execution

To execute this playbook, use the following command:

```shell
ansible-playbook -i inventory.ini service_restart.yml
