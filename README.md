## TESTS:
	-- Windows 10 x64
	-- Windows 7 x64
	-- Windows Server 2008 x32
	-- Windows xp x32
BUILD:
	g++ main.cpp -lntdll -o ProcessHide.exe
	g++ main.cpp -lntdll -o ProcessHide.exe -m32
NOTES:
	* This can be used multiple times on the same monitoring process
		if and only if the process is using NtQuerySystemInformation
		to get the processes list and is linked against ntdll.dll
		like task manager and process hacker, actually the more it's
		used the slower the monitoring process will be, also eventually
		after a lot of uses , theoretically, the monitoring process will
		crash because the stack will be overflowed especially if it's
		x32 process because of the nature of x32 calling convention
	* This theoretical problem can be handled by modifying the shellcode
		to search for a null separated list of processes names instead of
		searching for one process name at a time, also a detection if the
		shellcode is already used or this is the first time should be done
		if so the current process name will just be added to the already
		written shellcode processes names array
	* The same code can be used for ProcessExplorer but because ProcessExplorer
		isn't dynamically linked against ntdll.dll, the IAT hooking will not work,
		istead we can use a much powerfull hooking technique which is inline hooking,
		first get the base of ntdll (second item at PEB_LDR_DATA doubly-linked list)
		and parse its export table to get the address of NtQuerySystemInformation
		and from there redirect the call to the shellcode, also we will need
		a disassembler for that
