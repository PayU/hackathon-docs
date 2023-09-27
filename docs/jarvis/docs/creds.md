# Onboarding your team

Follow the next steps in order to gain access to Jarvis API and the SFTP folder for uploading sensitive or company related data to power up your AI game.

## Prerequisites
#### Keybase username
Obtain a valid [Keybase](https://keybase.io/) username. We will share the credentials to Jarvis securely with this user.

#### SSH keys
- Generate RSA key-pair, these keys will be useful to authenticate with our SFTP folder.
- Use the following cmd:  
  ```
  ssh-keygen -t rsa -b 4096 -f <filename>  
  ```  
  Example:  
  ```
  ssh-keygen -t rsa -b 4096 -f hackathon_sftp
  ```  
- The above will generate 2 files:
    1. `hackathon_sftp.pub` - public key to upload to One drive folder.
    2. `hackathon_sftp` - private key used to login to the SFTP.
       
#### Team name
Solve one of programming hardest challenges, and decide on a name to represent your team in the competition! :)

## Signing up

1. Go to: [Gen AI Hackathon 2023 Teams](https://payuglobal-my.sharepoint.com/:f:/g/personal/alex_ugol_payu_com/EgFom9-l_VtAj74LFA3jzMABylhi_i2sthuYOapr9Y-Pmw?e=uECjjN) in OneDrive.
2. Create a new folder, name it with your [team name](#team-name).
3. Upload to your new folder the [ssh public key](#ssh-keys).
4. Create `keybase.txt` file and add the Keybase username to the file.
5. You can follow the convention in the [example team folder](https://payuglobal-my.sharepoint.com/:f:/g/personal/alex_ugol_payu_com/EgFom9-l_VtAj74LFA3jzMABylhi_i2sthuYOapr9Y-Pmw?e=uECjjN).
## Sharing credentials
Our magic behind the scenes will setup your access privileges, and the [keybase username](#keybase-username) will be sent the following details:  
1. `app-id` & `private-key` values used for authentication with Jarvis API.  
2. `SFTP username` - used together with your generated RSA private key to connect to SFTP and upload files.  

   Good luck :)