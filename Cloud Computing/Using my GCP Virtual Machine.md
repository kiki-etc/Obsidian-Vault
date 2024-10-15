Here, I'm just keeping key facts about my VM so I can operate using my local terminal.<br>
Username: `knona378`
VM instance name: `instance-20240927-115124`
External IP Address: `34.68.121.36`
Passphrase: `kiki`
Zone: `us-central1-a`
# Transferring files to my VM
**1. Set Up SSH Access**
Ensure that your **SSH keys** are set up and you can connect to the instance. If not, use the following command to set up SSH access:
```
gcloud compute ssh username@instance-name --zone [ZONE]
```
**2. Identify VM Instance Details**
Before transferring files, find the necessary details about your VM. You can list your instances to get this information with:
```
gcloud compute instances list
```
**3. Transfer Files Using gcloud compute scp**
Use the following command to copy a file or folder from your local machine to your VM:
**Transfer a File:**
```
gcloud compute scp /local/path/to/file knona378@instance-name:/remote/path --zone [ZONE]
```
To upload an entire folder, use the --recurse flag:
```
gcloud compute scp --recurse /local/path/to/folder username@instance-20240927-115124:/home/knona378/ --zone us-central1-a
```
**4. Transfer Files from VM to Local Machine**
Download a File:
```
gcloud compute scp username@instance-name:/remote/path/to/file /local/path --zone [ZONE]
```
Download a Folder (Recursively):
```
gcloud compute scp --recurse username@instance-name:/remote/path/to/folder /local/path --zone [ZONE]
```
**5. Example Scenario**
```
gcloud compute scp --recurse ~/Documents/project user@my-vm:/home/user/ --zone us-central1-a
```
In this example: 
- ~/Documents/project is the local folder.
- user@my-vm is the username and VM instance name.
- /home/user/ is the destination path on the VM.
- us-central1-a is the zone. <br>
**6. Verifying the Transfer**
After the file transfer, you can SSH into the VM to verify that the files have been uploaded successfully:
```
gcloud compute ssh username@instance-name --zone [ZONE]
ls /remote/path
```