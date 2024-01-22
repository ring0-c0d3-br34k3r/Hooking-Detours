# API-Hooking-Detours


API Hooking in Malware? 

Malware authors may use API Hooking techniques to intercept API function 
calls in order to gain control over a target system and perform unauthorized 
activities. By hooking key API functions, malware can manipulate data, monitor 
user actions, bypass security mechanisms, and perform other malicious actions 
without the user’s knowledge or consent.


Detours API Hooking : 

"Detours is a library to intercept arbitrary Win32 binary functions on x86 machines. 
The interception code is applied dynamically at runtime. Convolutive methods replace 
the first few instructions of the target function with an unconditional jump to the 
user-provided wrap function. The instruction from the target function is placed in 
the trampoline. The trampoline address is placed in the target indicator. The wrap 
function can either replace the target function, or expand its semantics by calling 
the target function as a subroutine through the target cursor to the trampoline"

Note : 
      this is sort of the way that was demonstrated above, albeit in a more 
	    sophisticated and elegant way. The function that drives all of this is 
	    the function.DetourAttach(…)
___________________________________________________________________________________


# Hooking using Detours Library :

                    Detours Library GitHub Link : https://github.com/microsoft/Detours
1 - DetourTransactionBegin        https://github.com/microsoft/Detours/wiki/DetourTransactionBegin  
  - Begin a new transaction for attaching or detaching detours. This function should be called first 
    when hooking and unhooking.

2 - DetourUpdateThread            https://github.com/microsoft/Detours/wiki/DetourUpdateThread
  - Update the current transaction. This is used by Detours library to Enlist a thread in the current 
    transaction || The current thread is updated with DetourUpdateThread(GetCurrentThread()).

3 - DetourAttach                  https://github.com/microsoft/Detours/wiki/DetourAttach
  - Install the hook on the target function in a current transaction. This won't be committed until  
     is called.DetourTransactionCommit || The hook function is attached to the original function 
     using DetourAttach() with the function pointer and the hook function as parameters

4 - DetourDetach                  https://github.com/microsoft/Detours/wiki/DetourDetach
  - Remove the hook from the targetted function in a current transaction. This won't be committed 
     until  is called.DetourTransactionCommit || The hook function is detached from the original 
     function using DetourDetach()

5 - DetourTransactionCommit       https://github.com/microsoft/Detours/wiki/DetourTransactionCommit
  - Commit the current transaction for attaching or detaching detours.




if for example :     MessageBox(NULL, "OrcaRootki$ is Dead", "Original MsgBox", MB_OK | MB_ICONWARNING);
if its going to print for us "OrcaRootki$ is Dead" We Will intercept it halfway like "MITM Attack"
Then Were going to change that message And We make a Hook for him from "OrcaRootki$ is Dead" to 
"OrcaRootki$ is Great"  // thats the hook 




for exmple : 

MessageBoxA(NULL, "OrcaRootki$ is Dead", "Original MsgBox", MB_OK | MB_ICONWARNING);
we will put MessageBoxA in gpMessageBoxA for hooking 

A function pointer type, FnMessageBoxA, is defined to match the signature of the original MessageBoxA function :
typedef BOOL(WINAPI* FnMessageBoxA)(HWND, LPCSTR, LPCSTR, UINT);
The original function pointer, gpMessageBoxA, is declared and initialized with the MessageBoxA function :
FnMessageBoxA gpMessageBoxA = MessageBoxA;

nice 

The function that will run instead MessageBoxA when hooked || // will take the place of the MessageBoxA
MyHookedMessageBoxA(HWND hWnd, LPCSTR lpText, LPCSTR lpCaption, UINT uType)          // The hook function, MyHookedMessageBoxA, is defined. This function will intercept calls to MessageBoxA and provide custom behavior

Now We're going to grab our fake or The Hooked Function "MyHookedMessageBoxA" And we're going to make it the real Function 
Then we'll have hooking for MessageBoxA and our function will take its place "MyHookedMessageBoxA"

Running MyHookedMessageBoxA instead of gpMessageBoxA that is MessageBoxA :
	DetourAttach((PVOID)&gpMessageBoxA, MyHookedMessageBoxA));
	
	
	
	
//  Intercept and modify the MessageBoxA function is DONE

//  The API Hooking can be leveraged to intercept MessageBoxA calls, 
//  print message details, and even modify parameters before calling 
//	the original function. This level of control opens up possibilities 
//	for advanced debugging, behavior analysis, and security research
	
//	It is a powerful technique, if misused "For Malware Dev"
	
	
	



Happy Hacking




