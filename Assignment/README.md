# Linux System Architecture 

## What does kernel mean ?

A kernel is the core part of an operating system. It acts as a bridge between software applications and the hardware of a computer. The kernel manages system resources, such as the CPU, memory, and devices, ensuring everything works together smoothly and efficiently. It handles tasks like running programs, accessing files, and connecting to devices like printers and keyboards.

## What does Operating System mean?

Operating System lies in the category of system software. It basically manages all the resources of the computer. An operating system acts as an interface between the software and different parts of the computer or the computer hardware. The operating system is designed in such a way that it can manage the overall resources and operations of the computer. 

Operating System is a fully integrated set of specialized programs that handle all the operations of the computer. It controls and monitors the execution of all other programs that reside in the computer, which also includes application programs and other system software of the computer. Examples of Operating Systems are Windows, Linux, Mac OS, etc.

An Operating System (OS) is a collection of software that manages computer hardware resources and provides common services for computer programs. The operating system is the most important type of system software in a computer system.

## Is Linux a Kernel or an Operating System?

Technically, Linux is a monolithic kernel, not an operating system. The correct term is- Linux distribution or Distro, such as – Ubuntu, Fedora Linux, Deepin, etc. 
and Conclusion:

Now you know what to answer when someone asks you if Linux is a kernel or an operating system. However, if someone says, “Linux is my favorite operating system.”, you don’t have to correct them by telling them this story. The GNU and Debian projects use the name “GNU/Linux”, but most people simply like to call it “Linux” to refer to the combination. So, there is nothing wrong with it, but if you want to be technically correct then use the term “Linux Distribution“.

## What is an Architecture Layer Diagram ?

The Linux architecture can be depicted as a layered structure. Starting from the bottom, these layers are hardware, kernel, shell, and applications.
<div>
  <img src="https://github.com/user-attachments/assets/a73965c3-68c2-48e3-8efb-e11f5159ce8b">
</div>


1. Hardware

The lowest level of the Linux architecture is the hardware layer. This layer comprises the physical components of a computer, such as the hard drive, RAM, motherboard, CPU, network interfaces, and peripherals. These components are the tangible pieces of your system on which the rest of the architecture is built.

2. Kernel

Directly interfacing with the hardware layer is the kernel, the heart of the Linux operating system. As the core part of the OS, the kernel is responsible for low-level tasks such as disk management, task scheduling, memory management, and controlling peripherals.

The Linux kernel is a monolithic kernel, meaning it encompasses device drivers, file systems, system server calls, and more, all in a single static binary file. Because the kernel directly interacts with the system’s hardware, it’s crucial in terms of system performance and stability.

3. Shell

One layer up from the kernel is the shell. In simplest terms, the shell is a user interface that allows users to interact with the kernel. In Linux, most interactions with the shell occur in a command-line interface (CLI), where users type commands interpreted by the shell.

There are several different shells available in Linux, each with its unique features and syntax, such as the Bourne Again Shell (bash), the C Shell (csh), and the Z Shell (zsh).

4. Applications

The topmost layer of the Linux architecture consists of applications. These are the software programs that you, as the user, interact with directly. They range from system applications like file managers, text editors, and network managers, to user applications like browsers.

While applications communicate with the hardware through the kernel, they interact with the user through the shell. For instance, when you run a command to open a file in a text editor, the shell interprets your command, the kernel fetches the file from the hardware, and the text editor (application) displays it.

Conclusion

Knowing how these components work together can be incredibly helpful, whether you’re troubleshooting issues, optimizing performance, or simply looking to understand Linux more deeply.






##What is user space ?

This diagram shows how user space processes rely on the kernel for access to hardware, and how they access it via a system call (or syscall) interface. However, the kernel itself has more to it than just a syscall API for low-level operations. As well as facilitating this interface with user-run processes, the kernel contains a process scheduler, networking stack, virtual file system, and device drivers for hardware support, to name just a few. The diagram above is a bit of an over-simplification, and the following diagram

A user space process is executed by a user in the operating system, rather than being part of the operating system itself. It might also be executed by an init system (e.g. systemd), but it isn't part of the kernel. User space is the area of memory that non-kernel applications run in. User space processes literally run in the user space part of memory. A user space process runs in user mode, which is the non-privileged execution mode that the process' instructions are executed with. User mode processes have to switch to kernel mode when they want to consume services provided by the kernel (e.g. disk I/O, network access). Switching to kernel mode involves triggering a syscall to be executed by the kernel. This mechanism is described in bit more detail below.

User mode execution of user-run processes ensures that a user space process cannot access or modify memory managed by the kernel, and can't interfere with another process' execution. This is an important security control in ensuring that user-run processes cannot corrupt or interfere with the operating system.


<div>
  <img src="https://github.com/user-attachments/assets/d92fbb96-b939-405d-8695-edb706d78891">
</div>

## What is kernel Space ?

Kernel space is the area of system memory reserved for the kernel. It is where the kernel runs and executes kernel mode instructions.Kernel mode is the CPU execution mode of the kernel, which runs in a privileged, root-access mode. When a user space application requires the services provided by the kernel, it will signal the kernel to execute a syscall, and switch to kernel mode for the duration of the syscall execution.

##  Syscalls

Syscalls are functions in the kernel that provide services to a user space application. They are the API that the kernel exposes to user space programs, which allow a program to utilise the functionality the kernel offers. Examples include starting new processes, disk I/O, and networking.

A full list of syscalls can be found by running man syscalls on a Linux system. All syscalls are accompanied by a detailed man page, all of which can be found in section 2. For example, the man page for the chdir syscall can be read by running man 2 chdir. The man pages for syscalls are a really useful primary source of documentation, so if you're not familiar with man try running man man and start exploring the syscall documentation.

Many syscalls are accompanied by small Linux programs which wrap them. For example, the chdir syscall for changing working directory can be invoked directly by running chdir in a shell. Other syscalls are designed to be used in concert with each other. There are a variety of syscalls for socket-based networking (e.g. socket, bind, listen, and accept), which combined offer a suite of socket-based functions for user space programs to utilise.

As mentioned above, syscalls are invoked via an interrupt or instruction executed by the user space process and the kernel mode execution. This system is normally wrapped by a library (e.g. glibc), which offers a slightly higher-level abstraction for programs in the form of functions that can be called. Furthermore, most programming languages come with much higher-level abstractions that allow you to deal with logical operations, rather than physical syscalls.

Nonetheless, breaking down some simple operations in their syscall components can prove interesting. The remainder of this blog post will illustrate a couple of simple examples using Golang, and demonstrate how the syscalls themselves can be observed by a user.


# Command

## Exercise 1:
  1- The pwd command is mostly used to print the current working directory on your terminal. It is  also one of the most commonly used commands. 
 Now, your terminal prompt should usually include the entire directory. If it doesn’t, this is a quick command to see which directory you’re in. Another purpose for this command is when creating scripts because it can help us find the directory in which the script was saved. The below pictures are the output with the command.
    
<div>
  <img src="https://github.com/user-attachments/assets/1fc1f1d8-8c86-467e-a47e-acd53274037c">
</div>

  2- cd command in Linux
 The cd command is used to navigate between directories. It requires either the full path or   the directory name, depending on your current working directory. If you run this command without any options, it will take you to your home folder. Keep in mind that it can only be executed by users with sudo privileges.

<div>
  <img src="https://github.com/user-attachments/assets/8f2e40e0-f0a3-44f5-9f24-dea3863dcb9b">
</div>

3- ls command is commonly used to identify the files and directories in the working directory. This command is one of the many often-used Linux commands that you should know. This command can be used by itself without any arguments and it will provide us the output with all the details about the files and the directories in the current working directory. There is a lot of flexibility offered by this command in terms of displaying data in the output. Check the below image for the output.

<div>
  <img src="https://github.com/user-attachments/assets/8093949e-768f-4e37-bd4a-80da10e192be">
</div>


      



































