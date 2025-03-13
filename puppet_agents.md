## **Puppet Agents**

Now we will create agents and will control them from the master.  

**"Puppet agent"** is a program running on a managed computer _(server)_ that communicates with a Puppet server (Master) to receive instructions. 

and then apply those instructions (configurations) to ensure the system is in the desired state. Theek hai?

---

### **1. Starting the Puppet Master**
1. **`docker start puppet-master`**  
   Starts the `puppet-master` container if it is stopped.

2. **`docker exec puppet-master -it ubuntu bash`**  
   Opens a bash shell inside the running `puppet-master` container.

3. **`service puppetserver status`**  
   Checks the status of the Puppet server.

4. **`service puppetserver restart`**  
   Restarts the Puppet server.

5. **`netstat -ntulp`**  
   Displays active network connections and listening ports.

---

### **2. Creating a Puppet Agent**
6. **`docker images`**  
   Lists all Docker images available on the host.

7. **`docker run --name puppet-agent01 -it puppet-8`**  
   Creates and starts a new container named `puppet-agent01` from the `puppet-8` image, with an interactive terminal.

8. **`docker start puppet-agent01`**  
   Starts the `puppet-agent01` container if it is stopped.

9. **`docker exec -it puppet-agent01 bash`**  
   Opens a bash shell inside the running `puppet-agent01` container.

![Image](https://github.com/user-attachments/assets/90fa5ae2-5731-4057-99a2-d3acffca76b0)

**_// We are creating this from the `puppet-8` image because we don't     want to install all of the stuff that was in the server again and again._**

10. **`apt install puppet-agent`**  
    Installs the Puppet agent on the `puppet-agent01` container.

![Image](https://github.com/user-attachments/assets/6dbbac89-8650-4230-9064-9d00771f6e69)

**_// If you do this command `puppet resource host`, this will not    work, but if you do `/opt/puppetlabs/bin/puppet resource host`, this will work. We will solve this issue._**

11. **`echo 'PATH=$PATH:/opt/puppetlabs/bin' >> ~/.bashrc`**  
    Adds the Puppet binaries directory to the PATH in the `.bashrc` file.

12. **`source ~/.bashrc`**  
    Reloads the `.bashrc` file to apply changes.

    **_// Now `puppet` has become a command, and you don't need to type `/opt/puppetlabs/bin/puppet`._**

---

### **3. Configuring Communication Between Agent and Master**
**_// Now we want to communicate agent with the master.  _**

![Image](https://github.com/user-attachments/assets/a0d5a88b-323e-4165-863a-37f3cc2cfcc3)

**_// The agent can ping the IP of the master but not the FQDN, but the agent can ping the FQDN of google, but why?  
// Because Puppet works on FQDNs not IP addresses._**

**_// We will add the IP and the FQDN "of the master" in the same way at the end of the `/etc/hosts` file of the agent._**

13. **`vim /etc/hosts` (in agent)**  
    Edits the contents of the `/etc/hosts` file.

![Image](https://github.com/user-attachments/assets/80a76521-868c-468d-888b-8769aff8c8eb)
![Image](https://github.com/user-attachments/assets/dd098450-c213-433f-bf37-6b3e898a72d5)
![Image](https://github.com/user-attachments/assets/4635bf13-789e-4370-a4f7-ad64bddea46f)

14. **`vim /etc/puppetlabs/puppet/puppet.conf` (in the master)**  
    Edits the Puppet configuration file "on the master" .

15. **`autosign = true`**  
    Adds this line to the Puppet configuration file to enable automatic signing of certificates.

![Image](https://github.com/user-attachments/assets/037d6cc3-4ffd-44df-b6dc-426b0876415f)

**_// We are telling the master to assign a certificate to anyone who comes to it and not to ask for a certificate._**

16. **`service puppetserver restart` (on the master)**  
    Restarts the Puppet server to apply configuration changes.

17. **`puppet agent --test --server 7b56e8259e07` (in the agent)**  
    Runs the Puppet agent in test mode, connecting to the Puppet master with the specified server name.

![Image](https://github.com/user-attachments/assets/7ee65263-a23b-406d-9540-b216275a0eee)

**_// Now we will define what state we want on the agent._**

18. **`history`**  
    You can check what commands we have entered till now!

### **4. Defining and Applying Configuration on Agents**
19. **`cd /etc/puppetlabs/code/environments/production/manifests` (on the master)**  
    Changes the directory to the Puppet manifests directory.

20. **`vim init.pp`**  
    Edits the `init.pp` file to define a Puppet class.

    ```bash
    class testagent {
        file { "/tmp/test.txt":
            mode => "0644",
            owner => "root",
            group => "root",
            content => "HELLO FROM SERVER ... 01\n",
        }
    }
    ```
![Image](https://github.com/user-attachments/assets/d2d3dd52-4bf6-49f8-96c1-9966c1adb9f3)

21. **`vim site.pp`**  
    Edits the `site.pp` file to define the default node configuration.

    ```bash
    node default {
        include testagent
    }
    ```

    **_// Whenever any node tries to connect to the master, it is going to include this class (`testagent`) in the agent, and the agent will apply this state to itself._**

22. **`puppet agent --test --server 7b56e8259e07`**  
    Runs the Puppet agent in test mode, connecting to the Puppet master with the specified server name.

![Image](https://github.com/user-attachments/assets/97a48428-915a-4b4e-b15a-289257b8554d)

23. **`cat /tmp/test.txt` (on the agent)**  
    Displays the contents of the `/tmp/test.txt` file.

    **_// Now if you try to change anything in the `/tmp/test.txt`, it will give you an error and not apply your changes, and they will be reverted back.  
    This is what we wanted_**

---

### **5. Automating Puppet Agent Execution**
24. **`apt install cron`**  
    Installs the `cron` package for scheduling tasks.

### What is Cron?
**_Cron is a time-based job scheduler for Linux and systems that allows you to automate tasks by scheduling them to run at specific times or intervals._**

**_Here is the syntax of using cron job in linux_**

![Image](https://github.com/user-attachments/assets/10f5fa54-d98c-42ac-bed3-dd08c8af9252)
![Image](https://github.com/user-attachments/assets/7f551fd6-9f11-41e2-b68b-d79751481fca)

**_ // What we want is that the agent automatically executes the command to fetch configuration changes from the master after every 60 seconds,  
if you don't want to apply a cron job then you can run the command manually on 1000 servers after every 60 seconds also._**

25. **`vim /etc/cron.d/agent.cron`**  
    Edits the `agent.cron` file to schedule Puppet agent execution.

26. **`* * * * * echo "Hello World from Agent" >> /var/log/cron.log  2>&1`**  
    Adds a cron job to log a message every minute.

27. **`* * * * * puppet agent --test --server 7b56e8259e07 >> /var/log/cron.log  2>&1`**  
    Adds a cron job to run the Puppet agent every minute and log the output.

28. **`crontab /etc/cron.d/agent.cron`**  
    Loads the cron jobs from the `agent.cron` file.

29. **`cron`**  
    Starts the cron service.

![Image](https://github.com/user-attachments/assets/575e7805-a3e8-4364-ae01-1448e2650f0f)

30. **`tail -f /var/log/cron.log`**  
    Displays the contents of the `/var/log/cron.log` file in real-time.

    **_// After running, you will get an error: `puppet` command not found. Edit the `agent.cron` file and add the full path to the `puppet` command._**

31. **`which puppet`**  
    Displays the full path to the `puppet` command.

