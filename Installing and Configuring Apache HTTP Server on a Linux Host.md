# Installing and Configuring Apache HTTP Server on a Linux Host

## Introduction

Installing common system services like the Apache HTTP Server is a large function of being a system administrator. In this hands-on lab, you will be tasked with installing Apache HTTP Server and configuring it to meet the requirements specified. This includes configuring it to be persistent through system reboots and making sure that it adheres to standard system and network security.

## Solution

Log in to the terminal using the credentials provided for the lab:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```

When prompted, enter `y` to continue connecting, then enter the password provided for the lab.

### Install the Apache HTTP Server

Install the `httpd` package:
```
sudo yum install -y httpd
```

### Modify the `httpd.conf` File

 1. Open the `httpd.conf` file in an editor:
    ```
    sudo vim /etc/httpd/conf/httpd.conf
    ```
 1. Navigate to the `DocumentRoot` block (you can use `/Doc` to search for it).
 1. Update the `DocumentRoot` to point to the `/web_content` directory, replacing the existing `/var/www/html` path:
    ```
    DocumentRoot "/web_content"
    ```
 1. Below the `DocumentRoot` block, add a directory tag for the `/web_content` (you can copy and paste the existing `"/var/www"` tag for this):
    ```
    <Directory "/var/www">
      AllowOverride None
      # Allow open access:
      Require all granted
    </Directory>

    <Directory "/web_content">
      AllowOverride None
      # Allow open access:
      Require all granted
    </Directory>
    ```
 1. Write and quit to save your changes and exit the `httpd.conf` file:
    ```
    :wq
    ```

### Create the `/web_content` Directory and Update Its SELinux Security Context

 1. Create the `/web_content` directory:
    ```
    sudo mkdir /web_content
    ```
 1. List the details of the `/web_content` directory:
    ```
    ll -dZ /web_content/
    ```
    You should see that the file context mapping is currently set to `default_t`. You want this mapping to match the mapping for `/var/www/html`.
 1. List the details of the `/var/www/html/` directory:
    ```
    ll -dZ /var/www/html/
    ```
    You should see that the file context mapping is currently set to `httpd_sys_content_t`.
 1. Update the SELinux file context mapping for `/web_content` to match `/var/www/html` and ensure it persists through a file system relabel:
    ```
    sudo semanage fcontext -a -t httpd_sys_content_t "/web_content(/.*)?"
    ```
 1. Restore the default security context for the `/web_content` directory:
    ```
    sudo restorecon -R -v /web_content/
    ```
    The output indicates that this command will change the `default_t` context to `httpd_sys_content_t`.

### Update Firewalld to Allow Access to the `httpd` Service

 1. Add the HTTP and HTTPS services to the default zone in firewalld and ensure they persist through a reboot:
    ```
    sudo firewall-cmd --permanent --add-service=http --add-service=https
    ```
 1. List the firewall details:
    ```
    sudo firewall-cmd --list-all
    ```
    Note that the firewall is in the default `public` zone, and it currently only allows the `dhcpv6-client` and `ssh` services.  
 1. Reload the firewall rules to apply them to the current session:
    ```
    sudo firewall-cmd --reload
    ```
 1. List the firewall details again to verify the HTTP and HTTPS services are now allowed:
    ```
    sudo firewall-cmd --list-all
    ```

### Start and Enable the Apache HTTP Server

 1. Start and enable `httpd`:
    ```
    sudo systemctl enable httpd --now
    ```
 1. Check the status of the `httpd` service:
    ```
    sudo systemctl status httpd
    ```
    You should see that the service is active and enabled.

#### (Optional) Test the Web Server

 1. Open `/web_content/index.html` in an editor:
    ```
    sudo vim /web_content/index.html
    ```  
 1. Add any text to the file, then write and quit to save your changes:
    ```
    :wq
    ```
 1. Access the web server URL in a browser by pasting the public IP address provided for the lab into a browser bar.

    You should see the text you entered into the `index.html` file.

## Conclusion

Congratulations — you've completed this hands-on lab!
