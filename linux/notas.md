#### Inicio de notas.


## Content filter. 

sort.
To sort the desired results alphabetically or numerically to get a better overview.

cat  /etc/passwd | sort



grep. 
Search for specific results that contain patterns we have defined. 

cat /etc/passwd | grep "/bin/bash" 
Another posibility is to exclude specific results. option -v , esclude users who have disabled the standard shell with the name "/bin/false" or "/usr/bin/nologin"
cat /etc/passwd | grep -v "false\|nologin"

Cut.
Therefore we use the option "-d" and set the delimiter to the colon character (:) and define with the option "-f" the position in the line we want to output.

#bash
cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1

Tr.

Another possibility to replace certain characters from a line with characters defined by us is the tool tr. As the first option, we define which character we want to replace, and as a second option, we define the character we want to replace it with. In the next example, we replace the colon character with space.

cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "
cat /etc/passwd  | grep "/bin/bash" | tr ":" "|"


Column.

Since such results can often have an unclear representation, the tool column is well suited to display such results in tabular form using the "-t."

#bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t


Awk.

As we may have noticed, the user "postgres" has one row too many. To keep it as simple as possible to sort out such results, the (g)awk programming is beneficial, which allows us to display the first ($1) and last ($NF) result of the line.

# bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'

Sed.
One of the most common uses of this is substituting text. Here, sed looks for patterns we have defined in the form of regular expressions (regex) and replaces them with another pattern that we have also defined.

The "s" flag at the beginning stands for the substitute command. Then we specify the pattern we want to replace. After the slash (/), we enter the pattern we want to use as a replacement in the third position. Finally, we use the "g" flag, which stands for replacing all matches.

#bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'



Question:
Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "https://www.inlanefreight.com" website and filter all unique paths of that domain. Submit the number of these paths as the answer.

#bash
curl https://www.inlanefreight.com/ | grep -Po "https://www.inlanefreight.com/[^'\"]*" | sort -u | wc -l

### Regular Expressions

We can use it in tools like grep or sed or others. Often regex is implemented in web applications for the validation of user input.


Grouping:
Among other things, regex offers us the possibility to group the desired search patterns. Basically, regex follows three different concepts, which are distinguished by the three different brackets:

(a)	The round brackets are used to group parts of a regex. Within the brackets, you can define further patterns which should be processed together.
[a-z]	The square brackets are used to define character classes. Inside the brackets, you can specify a list of characters to search for.
{1,10}	The curly brackets are used to define quantifiers. Inside the brackets, you can specify a number or a range that indicates how often a previous pattern should be repeated.
|	Also called the OR operator and shows results when one of the two expressions matches
.*	Also called the AND operator and displayed results only if both expressions match


#bash
grep -E "(my|false)" /etc/passwd
grep -E "(my.*false)" /etc/passwd


### Permissions

Binary Notation:                4 2 1  |  4 2 1  |  4 2 1
----------------------------------------------------------
Binary Representation:          1 1 1  |  1 0 1  |  1 0 0
----------------------------------------------------------
Octal Value:                      7    |    5    |    4
----------------------------------------------------------
Permission Representation:      r w x  |  r - x  |  r - -

SUID & SGID.
Besides assigning direct user and group permissions, we can also configure special permissions for files by setting the Set User ID (SUID) and Set Group ID (SGID) bits. These SUID/SGID bits allow, for example, users to run programs with the rights of another user. Administrators often use this to give their users special rights for certain applications or files. The letter "s" is used instead of an "x". When executing such a program, the SUID/SGID of the file owner is used.

Sticky Bit.
Sticky bits are a type of file permission in Linux that can be set on directories. This type of permission provides an extra layer of security when controlling the deletion and renaming of files within a directory. It is typically used on directories that are shared by multiple users to prevent one user from accidentally deleting or renaming files that are important to others.

For example, in a shared home directory, where multiple users have access to the same directory, a system administrator can set the sticky bit on the directory to ensure that only the owner of the file, the owner of the directory, or the root user can delete or rename files within the directory. This means that other users cannot delete or rename files within the directory as they do not have the required permissions. This provides an added layer of security to protect important files, as only those with the necessary access can delete or rename files. Setting the sticky bit on a directory ensures that only the owner, the directory owner, or the root user can change the files within the directory.

When a sticky bit is set on a directory, it is represented by the letter “t" in the execute permission of the directory's permissions. For example, if a directory has permissions “rwxrwxrwt", it means that the sticky bit is set, giving the extra level of security so that no one other than the owner or root user can delete or rename the files or folders in the directory.


### Package Manager:

apt list --installed


### Service and Process Management

systemctl start|status|stop ssh
systemctl enable|disable ssh


Once we reboot the system, the OpenSSH server will automatically run. We can check this with a tool called ps.
ps -aux | grep ssh

We can also use systemctl to list all services.
systemctl list-units --type=service

It is quite possible that the services do not start due to an error. To see the problem, we can use the tool journalctl to view the logs.
journalctl -u ssh.service --no-pager

Kill a Process.

A process can be in the following states:

Running
Waiting (waiting for an event or system resource)
Stopped
Zombie (stopped but still has an entry in the process table).

Processes can be controlled using kill, pkill, pgrep, and killall. To interact with a process, we must send a signal to it. We can view all signals with the following command:

kill -l

 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX

Signal	Description
1	SIGHUP - This is sent to a process when the terminal that controls it is closed.
2	SIGINT - Sent when a user presses [Ctrl] + C in the controlling terminal to interrupt a process.
3	SIGQUIT - Sent when a user presses [Ctrl] + D to quit.
9	SIGKILL - Immediately kill a process with no clean-up operations.
15	SIGTERM - Program termination.
19	SIGSTOP - Stop the program. It cannot be handled anymore.
20	SIGTSTP - Sent when a user presses [Ctrl] + Z to request for a service to suspend. The user can handle it afterward.

Background a Process
Sometimes it will be necessary to put the scan or process we just started in the background to continue using the current session to interact with the system or start other processes. As we have already seen, we can do this with the shortcut [Ctrl + Z]. As mentioned above, we send the SIGTSTP signal to the kernel, which suspends the process.

ping -c 10 www.hackthebox.eu

vim tmpfile
[Ctrl + Z]
[2]+  Stopped                 vim tmpfile

jobs

[1]+  Stopped                 ping -c 10 www.hackthebox.eu
[2]+  Stopped                 vim tmpfile

The [Ctrl] + Z shortcut suspends the processes, and they will not be executed further. To keep it running in the background, we have to enter the command bg to put the process in the background.

bg

Another option is to automatically set the process with an AND sign (&) at the end of the command.


ping -c 10 www.hackthebox.eu &

Foreground a Process
After that, we can use the jobs command to list all background processes. Backgrounded processes do not require user interaction, and we can use the same shell session without waiting until the process finishes first. Once the scan or process finishes its work, we will get notified by the terminal that the process is finished.

jobs

[1]+  Running                 ping -c 10 www.hackthebox.eu &


fg 1
ping -c 10 www.hackthebox.eu

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9206ms


Execute Multiple Commands
There are three possibilities to run several commands, one after the other. These are separated by:

Semicolon (;)
Double ampersand characters (&&)
Pipes (|)

The difference between them lies in the previous processes' treatment and depends on whether the previous process was completed successfully or with errors. The semicolon (;) is a command separator and executes the commands by ignoring previous commands' results and errors.

echo '1'; ls MISSING_FILE; echo '3'

&& no continue


###Systemd
Systemd is a service used in Linux systems such as Ubuntu, Redhat Linux, and Solaris to start processes and scripts at a specific time. With it, we can set up processes and scripts to run at a specific time or time interval and can also specify specific events and triggers that will trigger a specific task. To do this, we need to take some steps and precautions before our scripts or processes are automatically executed by the system.

Create a timer
Create a service
Activate the timer


Create a Timer
To create a timer for systemd, we need to create a directory where the timer script will be stored.

sudo mkdir /etc/systemd/system/mytimer.timer.d
sudo vim /etc/systemd/system/mytimer.timer

Next, we need to create a script that configures the timer. The script must contain the following options: "Unit", "Timer" and "Install". The "Unit" option specifies a description for the timer. The "Timer" option specifies when to start the timer and when to activate it. Finally, the "Install" option specifies where to install the timer.

Mytimer.timer

[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target

Here it depends on how we want to use our script. For example, if we want to run our script only once after the system boot, we should use OnBootSec setting in Timer. However, if we want our script to run regularly, then we should use the OnUnitActiveSec to have the system run the script at regular intervals. Next, we need to create our service.


Create a Service 
sudo vim /etc/systemd/system/mytimer.service

Here we set a description and specify the full path to the script we want to run. The "multi-user.target" is the unit system that is activated when starting a normal multi-user mode. It defines the services that should be started on a normal system startup.


[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload

sudo systemctl start mytimer.service
sudo systemctl enable mytimer.service


crontab
Time Frame	Description
Minutes (0-59)	This specifies in which minute the task should be executed.
Hours (0-23)	This specifies in which hour the task should be executed.
Days of month (1-31)	This specifies on which day of the month the task should be executed.
Months (1-12)	This specifies in which month the task should be executed.
Days of the week (0-7)	This specifies on which day of the week the task should be executed.

# System Update
* */6 * * /path/to/update_software.sh

# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backups
0 0 * * 7 /path/to/scripts/backup.sh



The "System Update" should be executed every sixth hour. This is indicated by the entry */6 in the hour column. The task is executed by the script update_software.sh, whose path is given in the last column.

The task execute scripts is to be executed every first day of the month at midnight. This is indicated by the entries 0 and 0 in the minute and hour columns and 1 in the days-of-the-month column. The task is executed by the run_scripts.sh script, whose path is given in the last column.

The third task, Cleanup DB, is to be executed every Sunday at midnight. This is specified by the entries 0 and 0 in the minute and hour columns and 0 in the days-of-the-week column. The task is executed by the clean_database.sh script, whose path is given in the last column.

The fourth task, backups, is to be executed every Sunday at midnight. This is indicated by the entries 0 and 0 in the minute and hour columns and 7 in the days-of-the-week column. The task is executed by the backup.sh script, whose path is given in the last column.


### Linux Containers

Lnux Containers (LXC) is a virtualization technology that allows multiple isolated Linux systems to run on a single host. It uses resource isolation features, such as cgroups and namespaces, to provide a lightweight virtualization solution. LXC also provides a rich set of tools and APIs for managing and configuring containers, contributing to its popularity as a containerization technology. By combining the advantages of LXC with the power of Docker, users can achieve a fully-fledged containerization experience in Linux systems.

Both LXC and Docker are containerization technologies that allow for applications to be packaged and run in isolated environments. However, there are some differences between the two that can be distinguished based on the following categories:

LXC is a lightweight virtualization technology that uses resource isolation features of the Linux kernel to provide an isolated environment for applications. In LXC, images are manually built by creating a root filesystem and installing the necessary packages and configurations. Those containers are tied to the host system, may not be easily portable, and may require more technical expertise to configure and manage. LXC also provides some security features but may not be as robust as Docker.

On the other hand, Docker is an application-centric platform that builds on top of LXC and provides a more user-friendly interface for containerization. Its images are built using a Dockerfile, which specifies the base image and the steps required to build the image. Those images are designed to be portable so they can be easily moved from one environment to another. Docker provides a more user-friendly interface for containerization, with a rich set of tools and APIs for managing and configuring containers with a more secure environment for running applications.

To install LXC on a Linux distribution, we can use the distribution's package manager. For example, on Ubuntu, we can use the apt package manager to install LXC with the following command:

sudo apt-get install lxc lxc-utils -y

To create a new LXC container, we can use the lxc-create command followed by the container's name and the template to use. For example, to create a new Ubuntu container named linuxcontainer, we can use the following command:

sudo lxc-create -n linuxcontainer -t ubuntu


Managing LXC Containers
When working with LXC containers, several tasks are involved in managing them. These tasks include creating new containers, configuring their settings, starting and stopping them as necessary, and monitoring their performance. Fortunately, there are many command-line tools and configuration files available that can assist with these tasks. These tools enable us to quickly and easily manage our containers, ensuring they are optimized for our specific needs and requirements. By leveraging these tools effectively, we can ensure that our LXC containers run efficiently and effectively, allowing us to maximize our system's performance and capabilities.


Command	Description
lxc-ls	List all existing containers
lxc-stop -n <container>	Stop a running container.
lxc-start -n <container>	Start a stopped container.
lxc-restart -n <container>	Restart a running container.
lxc-config -n <container name> -s storage	Manage container storage
lxc-config -n <container name> -s network	Manage container network settings
lxc-config -n <container name> -s security	Manage container security settings
lxc-attach -n <container>	Connect to a container.
lxc-attach -n <container> -f /path/to/share	Connect to a container and share a specific directory or file.

Securing LXC
Let us limit the resources to the container. In order to configure cgroups for LXC and limit the CPU and memory, a container can create a new configuration file in the /usr/share/lxc/config/<container name>.conf directory with the name of our container. For example, to create a configuration file for a container named linuxcontainer, we can use the following command:

sudo vim /usr/share/lxc/config/linuxcontainer.conf

lxc.cgroup.cpu.shares = 512
lxc.cgroup.memory.limit_in_bytes = 512M

Here are 9 optional exercises to practice LXC:

1	Install LXC on your machine and create your first container.
2	Configure the network settings for your LXC container.
3	Create a custom LXC image and use it to launch a new container.
4	Configure resource limits for your LXC containers (CPU, memory, disk space).
5	Explore the lxc-* commands for managing containers.
6	Use LXC to create a container running a specific version of a web server (e.g., Apache, Nginx).
7	Configure SSH access to your LXC containers and connect to them remotely.
8	Create a container with persistence, so changes made to the container are saved and can be reused.
9	Use LXC to test software in a controlled environment, such as a vulnerable web application or malware.

