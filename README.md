# LinuxAssign3

## Part 1

### Task 1: create a system user

1. Create a user with specified home directory and a non-login shell, run:

```
sudo useradd -r -d /var/lib/webgen -s /usr/sbin/nologin webgen
```

2. Create bin and HTML directories along with their parent directory, run:

```
sudo mkdir -p /var/lib/webgen/bin /var/lib/webgen/HTML
```

3. Change the ownership of /var/lib/webgen/ to webgen user within webgen group, run:

```
sudo chown -R webgen:webgen /var/lib/webgen
```

4. Create the specified files, run:

```
sudo touch /var/lib/webgen/bin/generate_index /var/lib/webgen/HTML/index.html
```

5. Change the ownership of these 2 files to webgen user within webgen group, run:

```
sudo chown -R webgen:webgen /var/lib/webgen
```

### Task 2: create service and timer unit files

1. To create generate_index.service file in system directory, run to create and edit:

```
sudo vim /etc/systemd/system/generate_index.service
```

> Script is in generate_index.service.

2. To create generate_index.timer file in system directory, run to create and edit:

```
sudo vim /etc/systemd/system/generate_index.timer
```

> Script is in generate_index.timer.

3. After creating the two unit files above, to reload the systemd daemon to recognize the new unit files, run:

```
sudo systemctl daemon-reload
```

4. To enable and start the timer, run:

```
sudo systemctl enable generate_index.timer
sudo systemctl start generate_index.timer
```

5. To verify the timer running status, run:

```
sudo systemctl status generate_index.timer
```

> The output should look like:
> ![Timer Status](/images/timer_status.png)

6. To enable and start the service, run:

```
sudo systemctl enable generate_index.service
sudo systemctl start generate_index.service
```

7. To verify the service running status, run:

```
sudo systemctl status generate_index.service
```

> The output should look like:
> ![Service Status](/images/service_status.png)

8. To check the logs and confirm the service execution, run:

```
sudo journalctl -u generate_index.service
```

> The output should look like:
> ![Service Log](/images/service_log.png)

### Task 3: configure Nginx and index.html

1. To modify nginx.conf, run:

```
sudo nvim /etc/nginx/nginx.conf
```

2. Inside the script, change the user to:

```
user webgen;
```

The result should look like:
![User Webgen](/images/user_webgen.png)

4. To create a separate server block file, create the following directories first:

```
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

> This approach makes it easier to manage configurations, without implementing on the nginx.conf directly.

5. Then to include it in nginx.conf, use `nvim` into this file like step 1, then include the following inside `http` block:

```
http {
    ...
    include /etc/nginx/sites-enabled/*;
}
```

6. To create and edit a separate server block file, run:

```
sudo nvim /etc/nginx/sites-available/webgen
```

7. Add the following script to block file:

```
server {
    listen 80;
    listen [::]:80;

    server_name localhost;

    root /var/lib/webgen/HTML;
    index index.html;

        location / {
        try_files $uri $uri/ =404;
    }
}
```

8. The two scripts above should look like:

- nginx.conf:
  ![Nginx Conf](/images/nginx_conf.png)

- webgen block:
  ![Server Block](/images/server_block.png)

9. To enable the server block, create symlink:

```
sudo ln -s /etc/nginx/sites-available/webgen /etc/nginx/sites-enabled/webgen
```

10. To start nginx service and verify status, run:

```
sudo systemctl start nginx
sudo systemctl status nginx
```

> If the service fail at start, run `sudo nginx -t` to check syntax error

11. To ensure Nginx can access index.html while running as `webgen` user, set necessary permissions:

```
sudo chown -R webgen:webgen /var/lib/webgen
sudo chmod -R 755 /var/lib/webgen
```

12. To test the config result on web browser, connect to server ip address:

```
http://143.198.98.101/
```

The browser should look like:
![index.html](/images/html.png)

### Task 4: set up firewall

1. To install ufw, run:

```
sudo pacman -Syu ufw
```

2. Make sure ssh connection is allowed before enable ufw, run:

```
 sudo ufw allow 22
```

3. Then enable ufw, run:

```
sudo ufw enable
```

Now ufw is active and enabled on system startup.

4. Start ufw service, run:

```
sudo systemctl start ufw
```

5. To allow HTTP at port 80, run:

```
sudo ufw allow 80
```

6. To allow HTTPS at port 443, run:

```
sudo ufw allow 443
```

7. To enable limit on SSH to prevent brute-force attack, run:

```
sudo ufw limit 22
```

8. To apply all the changes above, run:

```
sudo ufw enable
```

9. To verify the changes and check the status of ufw, run:

```
sudo ufw status verbose
```

The output should look like:
![Ufw Output](/images/ufw.png)

### Task 5: system information page

![Final Output](/images/final_output.png)

## Part 2
