# Lab setup in Kali Linux

### Step 0: Pimpmykali

`git clone https://github.com/Dewalt-arch/pimpmykali`
`sudo ./pimpmykali.sh`
`N`
### Step 1: Installing required packages

`sudo apt update`

`sudo apt upgrade`

`sudo apt install docker.io`

`sudo apt install docker-compose`

Restart your Kali VM.

### Step 2: Unpack the labs

Copy the labs to a directory on your system (e.g. /home/kali/labs)

`cd /home/kali/labs`

`unzip bugbounty-v1.0.zip`

`cd bugbounty`

`sudo docker-compose up`

The first time you run this it will take some time because it needs to download the docker images to your machine. Next time you run it, it should only take 5-30 seconds.

### Step 3: Setup permissions

In a different terminal, navigate to where you unzipped the lab (e.g. /home/kali/labs/bugbounty) and run the set-permissions.sh script. This is used for labs that require write access, such as the file upload attacks.

`./set-permissions.sh`

Browse to `http://localhost`

The first time you load the lab the database will need to be initialized, just follow the instructions in the red box by clicking the link, then coming back to the homepage.

Enjoy your labs!