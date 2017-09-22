# CPU README file

This is **Wei Le** Readme file.

## Compile and Run

- Compile

	make clean; make

- Run

	./vcpu_scheduler 2


## Code Description

- First, connect to the Hypervisor using virConnectOpen() API,  and URI of the hypervisor is "qemu:///system";
- Get the number of PCPU using virNodeGetCPUMap() API;
- Get the list of active running VMs on the host, conduct primary operations in a while loop when the number of runnign VMs is larger than 0, the program sleeps for 2s at the end of each while loop to get VCPU time which has increased much enough for calculating VCPU usage;
- Inside each while loop, get the domain information for all running domains using virDomainListGetStats(). Loop over each domain information, retrieve the information for VCPU time from domain information, save VCPU time and domain information into a struct DomainStats object;
- memcpy the struct DomainStats object for each domain to an old_domain_stats object as a comparison base for the next loop;
- If it's the first round of while loop, do nothing else. If it's not the first round, compare the current struct DomainStats object and old/previous struct DomainStats object for each domain, get the VCPU usage during each sleep time for each domain and save it to struct DomainStats object;
- Get current map between VCPU to PCPU using virDomainGetVCPUs(), add VCPU usage for each domain to corresponding PCPU, and get PCPU usage for each host CPU and save it to an array;
- Compare CPU usage for each PCPU and then get the busiest PCPU and freest PCPU;
- If the difference of CPU usage between the busiest and freest PCPU is no more than 10%, the scheduler would do nothing as all PCPUs are balanced enough;
- If the difference is larger than 10%, compare all VCPUs running on busiest PCPU, find the businest VCPU which has the highest VCPU usage, and then pin this VCPU to freest PCPU.
