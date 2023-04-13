# samba-file-server
This project demonstrates how to set up a Samba file server on a Fedora-based system, enabling easy and secure file sharing across a home network. Samba allows seamless sharing of files between Linux, Windows, and macOS devices, providing a convenient way to access and manage files within your local network

## Requirements

- A Fedora-based system (Fedora, CentOS, RHEL, etc.)
- Local network access for connecting multiple devices
- Basic understanding of Linux commands and terminal usage

## Installation

Follow the step-by-step instructions below to install and configure Samba on your Fedora-based system:

1. Update your system packages:
     sudo dnf update

2. Install Samba and related packages:
     sudo dnf install samba samba-client
     
3. Create a new directory to share files:
     mkdir ~/shared-folder
     
4. Change permissions for the shared folder to allow read and write access:
     chmod 0775 ~/shared-folder

5. Edit the Samba configuration file:
      sudo nano /etc/samba/smb.conf
      
6. Add the following lines at the end of the file:
[Shared-Folder]
path = /home/your_username/shared-folder
writable = yes
browsable = yes
valid users = your_username

Replace "your_username" with your Fedora username.
Save and exit the file.

7. Create a Samba user and set a password:
    sudo smbpasswd -a your_username
Replace "your_username" with your Fedora username.

8. Start the Samba service and enable it to run at startup:
     sudo systemctl start smb
     sudo systemctl enable smb

9. Configure the firewall, allow Samba traffic through the firewall:
    sudo firewall-cmd --add-service=samba --permanent
    sudo firewall-cmd --reload
    
## Usage

10. Access the shared folder from Windows:
    On the built-in file explorer use the following address -> \\your_fedora_ip\Shared-Folder
*Replace "your_fedora_ip" with your Fedora machine's local IP address.

11. To access the shared folder from another Linux machine, install the cifs-utils package and mount the shared folder:

  sudo apt-get install cifs-utils
  mkdir ~/remote-shared-folder
  sudo mount -t cifs //your_fedora_ip/Shared-Folder ~/remote-shared-folder -o username=your_username
  
12. On MacOS: 
   Open Finder.
   From the menu bar, click on "Go" and then "Connect to Server."
   In the "Server Address" field, enter the following address - "smb://your_fedora_ip/Shared-Folder"

*Replace "your_fedora_ip" with your Fedora machine's local IP address, and "your_username" with your Fedora username.



## Troubleshooting

If you encounter issues while accessing the Samba file server or sharing files, consider the following common problems and solutions:

Cannot access the shared folder: Make sure the Samba service is running on the Fedora machine. You can check the status by running "sudo systemctl status smb". If it's not running, start the service with "sudo systemctl start smb".

Permission denied when accessing the shared folder: Double-check the permissions on the shared folder and ensure the user has the appropriate access rights. You can adjust the permissions using the chmod command.

Firewall blocking access: Ensure that the firewall on the Fedora machine allows Samba traffic. You can do this by running the following commands:
    sudo firewall-cmd --add-service=samba --permanent
    sudo firewall-cmd --reload

Username and password not working: If you've forgotten your Samba password or need to reset it, use the smbpasswd command:
    sudo smbpasswd -a your_username
