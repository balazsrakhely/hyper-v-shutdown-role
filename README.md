# Hyper-V Virtual Machine Power State role

This role is for manipulating the power state of a Hyper-V virtual machine. The VM can be turned off, restarted, or turned on.

### Functionality

**1. Check if every required input parameter is present**

**2. Retrieve VM names that have a tag matching the input variable vm_tag**

**3. Call the appropriate powershell VMM command to change each virtual machine's power state**

    - IMPORTANT NOTE: if a VM's state modification process runs into an error, the ansible playbook will not fail because of it. It will continue without any problems and result in 'Success'. The error, however, is displayed and can be retrieved. If this workflow is not acceptable, modifying the scripts to fail in case of an error is easy, since the error is accessible to the playbook.

### Credentials

The role requires a hyper-v host name it can delegate the tasks to. So that host must be in the inventory, and credentials must be provided for it.

### Input variables

| Parameter | Type | Flags | Default | Description |
| --- | --- | --- | --- | --- |
| hv_host | string | required | | The Hyper-V host. This must be an inventory item. |
| vmm_server | string | required | | The VMM server name. |
| vm_tag | string | required | | The tag you want to filter the virtual machines on. |
| vm_state | string of 'on', 'off', 'restart' | required | | The desired state of the virtual machine. |

### Outputs

The ansible task returns a list of objects, containing information for each VM's state change result. The actual output that the state changing script does contains 2 fiels: 'err' and 'obj'. If the 'err' field is empty, then the script ran successfully on the given VM. If the 'err' field is not empty, then there were some problems, and the VM state probably didn't change.

This output struct is retrievable in ansible under the path of: '_state_change_result.results[\<vm-idx>].output[0]':

```json
{
    "err": "<error-message>",
    "obj": {
        "Name": "<vm-name>",
        "Status": 0
    }
}
```
