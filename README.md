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

### Task 3: configure Nginx

## Part 2
