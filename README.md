# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main()
{
    int fd[2];
    pid_t pid;
    char message[] = "Hello from parent process";
    char buffer[100];

    if (pipe(fd) == -1)
    {
        perror("Pipe failed");
        return 1;
    }

    pid = fork();

    if (pid < 0)
    {
        perror("Fork failed");
        return 1;
    }
    else if (pid == 0)
    {
        close(fd[1]);
        read(fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd[0]);
    }
    else
    {
        close(fd[0]);
        write(fd[1], message, strlen(message) + 1);
        close(fd[1]);
        wait(NULL);
        printf("Parent process completed.\n");
    }

    return 0;
}

```



## OUTPUT

<img width="917" height="223" alt="image" src="https://github.com/user-attachments/assets/d47f3a0a-7e6f-4cc9-bf26-e8f3054cc76a" />

## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/wait.h>

int main()
{
    int fd;
    char message[] = "Hello through named pipe";
    char buffer[100];

    mkfifo("myfifo", 0666);

    if (fork() == 0)
    {
        fd = open("myfifo", O_RDONLY);
        read(fd, buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd);
    }
    else
    {
        fd = open("myfifo", O_WRONLY);
        write(fd, message, strlen(message) + 1);
        close(fd);
        wait(NULL);
    }

    unlink("myfifo");
    return 0;
}

```

## OUTPUT
<img width="646" height="86" alt="image" src="https://github.com/user-attachments/assets/b6f0f6e0-126e-4964-b803-48373a0a41fd" />


# RESULT:
The program is executed successfully.
