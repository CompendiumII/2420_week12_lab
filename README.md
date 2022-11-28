# 2420_week12_lab

## NGINX Installation
Let's start by installing NGINX. Connect to your droplet, and utilize the command:
```
sudo apt install nginx
```

![nginx installation](https://user-images.githubusercontent.com/46077062/204196022-bc6de52f-cac2-4f4c-a995-df1d6d271686.PNG)

This will prompt you with a [Y/n] option, and after typing y and hitting enter, NGINX will be installed. 

After installing NGINX, we can go ahead and exit our droplet, as the next couple of steps are done in WSL.

## File Creation

### HTML Document

We will create an HTML document to serve.

We can VIM an HTML file and make a very basic display.

![HTML code](https://user-images.githubusercontent.com/46077062/204196870-89ae731a-98db-4a9b-a20a-e2f129ccf3cc.PNG)

### Server Block

We will create a server block file.

we can VIM a file with the IP address of our droplet.


![Server Block](https://user-images.githubusercontent.com/46077062/204197097-5c70246d-634b-463b-a19d-9aa2176ed8b3.PNG)


## Moving Files to Droplet


### Uploading Files

Now that we have created the files, we need to upload them to our droplet.
We can utilize the file transfer protocol SFTP to move our files from WSL to our droplet.

SFTP into your droplet:
```
sftp -i ~/.ssh/DO2_key kevin@164.92.86.62
```
And then move the files in:
```
put index.html
put 164.92.86.62
```

![Uploading Files](https://user-images.githubusercontent.com/46077062/204197392-8e49dec4-5f71-4439-9687-47cd8bc8f78d.PNG)

### Changing Permissions

We need to change the permissions for our files.

Change the ownership of our HTML file to your user:
```
sudo chown -R kevin:kevin /var/www/164.92.86.62/html
```
And then we can give all permissions to our folder:
```
sudo chmod -R 777 /var/www
```

### Creating Required Directories

We need to make some directories for our files to go into.
We will create a directory with our droplet IP address, and then make an HTML directory within it.

![making directories](https://user-images.githubusercontent.com/46077062/204197992-a8eae1a7-ad88-4962-a54f-b6ec645ea082.PNG)

### Moving Files

We will need to move the files that we uploaded to the directories created.
The HTML file will be moved into the directory we just created, and our server block will be moved to:
```
/etc/nginx/sites-available
```

![moving files](https://user-images.githubusercontent.com/46077062/204198348-3ee29859-179d-46e0-a9b0-c0a80a01565f.PNG)

## Creating Symbolic Link

We will create a symbolic link for our server block that we created.

We will use the following to create the link:
```
sudo ln -s /etc/nginx/sites-available/164.92.86.62 /etc/nginx/sites-enabled
```

And then we can check to make sure there aren't any errors:
```
sudo nginx -t
```

![linking block](https://user-images.githubusercontent.com/46077062/204198564-c3a0c73b-d0c5-4a50-9504-eba20fd59406.PNG)


### Restarting Service

We will now restart our service to make sure that all of the changes we have made are applied:
```
sudo system restart nginx
```

## Checking Document Serve Status

Visit 164.92.86.62 on your browser and the HTML created earlier will be displayed on this page.

![website runs](https://user-images.githubusercontent.com/46077062/204199025-4128a670-b898-4040-af29-c0ab655fed2e.PNG)


## Enabling Firewall

We will be using UFW as our firewall service. 
To enable this, we will use:
```
sudo ufw enable 
```
This will give you a [Y/n] prompt, type y.

![enabling firewall](https://user-images.githubusercontent.com/46077062/204199391-a2c7f5e7-7f0b-4b7b-9fb7-8c465349c33d.PNG)

### Adding UFW Rules

We need to add a couple of rules to allow us to connect to our droplet again, as well as ensuring our HTML is still being served.
We will be using the following:
```
sudo ufw allow "OpenSSH"
sudo ufw allow "Nginx HTTP"
```

![adding ufw rules](https://user-images.githubusercontent.com/46077062/204199641-452be980-9c4b-4b75-9d48-6ab01bb00820.PNG)

### Checking UFW Status

To make sure our firewall is setup correctly, the following should be allowed by UFW:
```
OpenSSH
Nginx HTTP
OpenSSH (v6)
Nginx HTTP (v6)
```

![checking ufw status](https://user-images.githubusercontent.com/46077062/204199779-1930dd1b-473a-4f8b-ae37-1047f45048e8.PNG)

## Checking For Functionality

We can exit out of our droplet and reconnect to it to ensure we are still able to after making the firewall changes. If everything goes well, we should be able to connect and still be able to visit the IP address to see our HTML.






