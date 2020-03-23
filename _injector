#include <iostream>
#include <Windows.h>

//there is filename which dll want to inject
const char* dllName = "C:\\filename.dll";

int main()
{
	HANDLE hProcess, hThread;
  //there is program id where you can find in task manager for select injected program
	DWORD pid;
	PVOID address;
	SIZE_T nbytesWritten;
	std::cin >> pid;
	hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, pid);

	if (hProcess == NULL)
	{
		std::cout << GetLastError() << std::endl;
		return -1;
	}

	address = VirtualAllocEx(hProcess, NULL, strlen(dllName) + 1, MEM_COMMIT | MEM_RESERVE, PAGE_READWRITE);

	if (address == NULL)
	{
		std::cout << GetLastError() << std::endl;
		return -1;
	}

	if(!WriteProcessMemory(hProcess, address, dllName, strlen(dllName) + 1, &nbytesWritten))
	{
		std::cout << GetLastError() << std::endl;
		return -1;
	}

	hThread = CreateRemoteThread(hProcess, NULL, 0, (LPTHREAD_START_ROUTINE)LoadLibraryA, address, 0, NULL);
	
	if (hThread == NULL)
	{
		std::cout << GetLastError() << std::endl;
		return -1;
	}
}
