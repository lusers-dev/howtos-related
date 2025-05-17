# Linux Users - Python3 Update Alternatives

A Word of WARNING!

The `update-alternatives` command in Linux is used to manage symbolic links that associate a generic name with a specific file or version of a program. When it comes to changing the default Python3 version using `update-alternatives`, it involves altering the symbolic links that determine which version of Python3 is invoked when the `python3` command is used in the terminal. While changing the default Python3 version through `update-alternatives` might not directly affect the functionality of the entire Linux operating system, it can have implications for various system components and user-level programs that rely on Python3.


`python3-update-alternatives.txt`

```shell
## find the OS default version for python3
$ which python3
/usr/bin/python3
ls -l /usr/bin/python3
/usr/bin/python3 -> python3.9


## look for the latest python available
$ dnf search python3.1

python3.11.x86_64 : Version 3.11 of the Python interpreter
...
python3.11-pip.noarch : A tool for installing and managing Python packages
...

## install latest python available version
$ sudo yum install python3.11 python3.11-pip


## find path of installed python version
$ which python3.11 
/usr/bin/python3.11

## update alternative path
$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1
$ sudo update-alternatives --install /usr/bin/pip3 pip3 /usr/bin/pip3.11 1

## verify change
$ update-alternatives --list
...
pip3                  	auto  	/usr/bin/pip3.11
python3               	auto  	/usr/bin/python3.11


## verify change
$ ls -l /usr/bin/python3
/usr/bin/python3 -> /etc/alternatives/python3

ls -l /etc/alternatives/python3
/etc/alternatives/python3 -> /usr/bin/python3.11


```

Here's how changing the default Python3 version might affect the functionality of the Linux operating system:

- System Components and Services: Some system components, scripts, and services are written in Python and might depend on a specific version. If you change the default Python version, these components might break or exhibit unexpected behavior if they are not compatible with the new Python version. For example, critical system scripts, package managers, and init scripts may use Python, and changing the default version could impact their execution.

- Package Management: Linux distributions often rely on Python scripts for package management tools like `apt` (Debian/Ubuntu) or `dnf` (Fedora). If the default Python version is changed and these tools are not compatible with the new version, package installation, updates, and management might become problematic.

- Third-Party Software: Many third-party applications and tools, especially in the developer and data science domains, rely on specific Python versions and libraries. If you switch the default Python version, these applications might not work correctly or may require adjustments to use the new Python version.

- Virtual Environments and Pip: Changing the default Python version could affect virtual environments and the Python package manager (`pip`). Virtual environments allow users to isolate Python environments for different projects, each with its own set of dependencies. If the default Python version changes, it might not align with the version used in existing virtual environments, leading to compatibility issues.

- Scripts and User Programs: Users often write scripts and programs that rely on specific Python versions. Changing the default version might break these scripts if they are not compatible with the new Python version.

- Shell Startup Files: Users can customize their shell startup files (e.g., `.bashrc`, `.zshrc`) to set specific Python versions for their sessions. Changing the default Python version might not automatically update these settings, causing confusion and inconsistencies.

- Development Environments: If you're a developer, changing the default Python version might disrupt your development environment if your projects rely on different Python versions.

- Library Compatibility: Different Python versions can have varying library support and compatibility. If a particular library is critical to your workflow and is not compatible with the new Python version, it might cause issues.



