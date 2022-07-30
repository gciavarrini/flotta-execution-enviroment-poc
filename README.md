# flotta-execution-enviroment-poc
The goal is to execute an ansible playbbok in flotta using the execution enviroment.

## References
* [Introduction](https://ansible-builder.readthedocs.io/en/latest/index.html) to Execution Environment
* [Execution Environments](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html)
* Execution Enviroment image  https://quay.io/repository/ansible/ansible-runner?tab=tags&tag=stable-2.10-devel
* [ Creating Ansible execution environments using Ansible Builder](https://infohub.delltechnologies.com/l/dell-powermax-ansible-modules-best-practices-1/creating-ansible-execution-environments-using-ansible-builder)
* [Building off of existing base EEs provided by Red Hat Ansible Automation Platform](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_builder_guide/assembly-building-off-existing-ee
)
* 
# Test

## Preliminary Steps
1. Add a device with DeviceID `edgedevice1`
2. Label the `edgedevice1` using:\
    `kubectl label edgedevice edgedevice1 app=ee-ansible`
3. Create configMap `ee-configmap-env`\
   `kubectl create configmap ee-configmap-env --from-env-file configmaps/ee-configmap.properties`
4. identify the edgedevice docker
    `docker ps --filter label=flotta`
5. connect to the device
    `docker exec -it <CONTAINERID> /bin/bash`
6. View log\
 `journalctl -xefu yggdrasild`

In another terminal(1)
1. `su - flotta -s sh`
2. `mkdir ~/volumeDir`
3. Add playbook `playbook1.yml`
   
Deploy workload deploy workload available in `workloads` folder:\
   `kubectl apply -f <FILENAME>`

# Result

From terminal(1) 
`podman ps`
`podman logs -tf`

```bash
[WARNING]: Unable to parse /runner/inventory as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that
the implicit localhost does not match 'all'

PLAY [127.0.0.1] ***************************************************************

TASK [Gathering Facts] *********************************************************
ok: [127.0.0.1]

TASK [ping] ********************************************************************
ok: [127.0.0.1]

PLAY RECAP *********************************************************************
127.0.0.1                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
# Open Questions
- How to enable `oneShot` in workload spec?