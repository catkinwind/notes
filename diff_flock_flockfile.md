# Differences for flockfile and flock

1. flockfile is thread safe. It is associated with FILE object.
2. flock is associated with process. It is associated with file descriptor.

All actual reading/writing to files is done by the system calls read(2)/write(2), pread(2)/pwrite(2) and mmap(2). The libc functions like printf(3), puts(3), etc. actually use the system calls (read(2), write(2),..) to do the actual reading/writing. But the libc functions are often more convenient because they (among other things like formatted IO) do the buffering for you, and do a good job at choosing optimal buffer sizes.

OTOH one drawback of this buffering is that you do not control when data is actually read/written, and how much is done at a time. This makes the libc (stdio) IO-functions incompatible with low-level locking functions like fcntl(2), lockf(2) and flock(2), even though it is possible to get the file-descriptor used by a FILE\*. To be able to do file locking together with the stdio functions, flockfile(3) is provided by libc (stdio), which implements it's own locking. This stdio-file locking is entirely seperated from the low-level locking with fcntl(2), flock(2) and lockf(3) and the kernel does not even 'know' about the locks from libc (stdio). This makes it possible to make flockfile(3) thread-safe, which is impossible/difficult to do with the system-call (kernel-level) lock-functions (see this question).

The use of flockfile(3) to lock files or parts of files, you should not get a file-descriptor for the FILE\* and use the system calls for IO (read(2), write(2),..) because those functions have no way to be aware of the stdio-locks.
