"if you have to ask, you're probably doing something wrong." -- by Ramond Chen (The old new things)

here explains: https://msdn.microsoft.com/en-us/library/ms686774.aspx

Usually it's a multiple of 1MB for both Windows and Linux.
For thread on Linux / x86-32, the default size of stack is 2MB. For IA64 platform, it's maybe 8MB.

ulimit -S will return Linux's setting.
int pthread\_attr\_setstacksize(pthread\_attr\_t \*attr, size_t stacksize);

could set the stack size for a thread.
