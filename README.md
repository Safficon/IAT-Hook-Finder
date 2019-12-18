# IAT-Hook-Finder
Detects IAT hooks on x64 processes.

# Explanation:

Using NtQueryInformationProcess we can get the PEB address of a remote process.
Using ReadProcessMemory we can parse the PEB to get to the image running and all the DLLs loaded by the process.
Traversing the image's Import Directory we can get all the API names imported with their respective DLL name and the API's address in memory.

Comparing the API's address in memory against the API address exported by the DLL (by either reading the DLL file manually or loading the DLL to our own process' memory) we can confirm whether the API address in the Import Address Table was replaced or not.

Due to API sets in various operating system (Windows 7 and above), a DLL whose name is in an import descriptor of an executable is not always the DLL that is found in the memory address of the process (api-ms-win-core-*, api-ms-win-shcore-*, etc.). These sort of DLLs simply using forwarding, so it is trickier to find what is the real DLL hosting the API address.

# TODO
- 32 bit processes on x64 PC (while x64 ntdll.dll exists in the memory space)
- Loading a DLL to self process' own memory doesn't work with WinSxS file because a different path for the DLL is given. Remember that two DLL's with the same name can appear in the same process so can't use the filename only for lookup.
- Nice GUI
- Maybe also check for EAT hooking? Might require writing a EAT hooking program since there's only 2 I've seen looking up "EAT hook github", that might not work on 64 bit, idk. This hooking only allows to intercept calls to GetProcAddress anyways.
- Maybe do an inline hooking detector (through checksum? Might require a disassembly module for that)

# Useful References

Explanation of Hooks:
https://www.unknowncheats.me/forum/anti-cheat-bypass/289126-hooking-detection-bypasses-details.html

Scylla's ApiReader:
https://github.com/NtQuery/Scylla/blob/master/Scylla/ApiReader.cpp
