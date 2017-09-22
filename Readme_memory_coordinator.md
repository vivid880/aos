# Memory README file

This is **Wei Le** Readme file.

## Compile and Run

- Compile
	make clean; make

- Run
	./memory_coordinator 2


## Code Description

- First, connect to the Hypervisor using virConnectOpen() API,  and URI of the hypervisor is "qemu:///system";
- Get the list of active running VMs on the host, conduct primary operations in a while loop when the number of runnign VMs is larger than 0, the program sleeps for 2s at the end of each while loop to get the updated memory stats for each domain in the next loop;
- Inside each while loop, get the memory stats for each domain using virDomainMemoryStats(), get the domain with most unused memory and the domain with least unused memory, save unused memory, current balloon size, and domain into a struct DomainInfo object for these two domains;
- For domain with most unused memory, if its unused memory is more than 250MB, decrease its acutual balloon size by half of unused memory using virDomainSetMemory();
- For domain with least unused memory, if its unused memory is less than 100MB, increase its actual ballon size by 150Mb using virDomainSetMemory(), but the actual balloon size is up to 2GB.