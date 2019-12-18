# IAT-Hook-Finder
Detects IAT hooks on x64 processes.

# Explanation:

Using NtQueryInformationProcess we can get the PEB address of a remote process.
Using ReadProcessMemory we can parse the PEB to get to the image running and all the DLLs loaded by the process.
Traversing the image's Import Directory we can get all the API names imported with their respective DLL name and the API's address in memory.

Comparing the API's address in memory against the API address exported by the DLL (by either reading the DLL file manually or loading the DLL to our own process' memory) we can confirm whether the API address in the Import Address Table was replaced or not.

TODO:
- 32 bit processes on x64 PC (while x64 ntdll.dll exists in the memory space)
- Loading a DLL to self process' own memory doesn't work with WinSxS file because a different path for the DLL is given. Remember that two DLL's with the same name can appear in the same process so can't use the filename only for lookup.
- Nice GUI
- Maybe also check for EAT hooking? Might require writing a EAT hooking program since 
