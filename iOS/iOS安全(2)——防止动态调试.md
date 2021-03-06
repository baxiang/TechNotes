防止GDB、lldb依俯。关键字 ptrace
```

#import <dlfcn.h>  
#import <sys/types.h>  
  
typedef int (*ptrace_ptr_t)(int _request, pid_t _pid, caddr_t _addr, int _data);  
#if !defined(PT_DENY_ATTACH)  
#define PT_DENY_ATTACH 31  
#endif  // !defined(PT_DENY_ATTACH)  
  
void disable_gdb() {  
    void* handle = dlopen(0, RTLD_GLOBAL | RTLD_NOW);  
    ptrace_ptr_t ptrace_ptr = dlsym(handle, "ptrace");  
    ptrace_ptr(PT_DENY_ATTACH, 0, 0, 0);  
    dlclose(handle);  
}  
  
int main(int argc, charchar *argv[])  
{  
#ifndef DEBUG  
    disable_gdb();  
#endif  
    @autoreleasepool {  
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([WQMainPageAppDelegate class]));  
    }  
}
```
sysctl 检查是否被调试
```
int isDebuggerPerforming() {
    struct kinfo_proc infos_process;
    size_t size_info_proc = sizeof(infos_process);
    pid_t pid_process = getpid(); // pid of the current process
    //
    int mib[] = {CTL_KERN,        // Kernel infos
        KERN_PROC,       // Search in process table
        KERN_PROC_PID,   // the process with pid =
        pid_process};    // pid_process
    //
    //Retrieve infos for current process in infos_process
    int ret = sysctl(mib, 4, &infos_process, &size_info_proc, NULL, 0);
    if (ret) return 0;             // sysctl failed
    //
    struct extern_proc process = infos_process.kp_proc;
    int flags_process = process.p_flag;
    return (flags_process & P_TRACED) != 0;        // value of the debug flag
}
```
xcode 编译，在Other Linker Flags那里添加一条这个字段

```
-Wl,-sectcreate,__RESTRICT,__restrict,/dev/null
```
