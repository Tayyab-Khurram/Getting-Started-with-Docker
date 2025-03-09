## **What is Puppet?**

Imagine you have **1000 servers**, and you need to:  
- Install software on all of them.  
- Configure security settings on all.  
- Ensure they all the servers have the same settings.  

#### Would you do that manually?
---

### **1. We will use Puppet instead**
- _What is Puppet?_
- Puppet is a **server of servers**.  
- It is a **Configuration Management (Master)** tool used in DevOps that will help us automate the setup, configuration, and management of the servers.

---

### **2. Setting Up the Puppet Master**
1. **`docker pull ubuntu`**  
   Downloads the official Ubuntu image from Docker Hub.

2. **`docker run --name puppet-master -it ubuntu`**  
   Creates and starts a new container named `puppet-master` from the Ubuntu image, with an interactive terminal.

3. **`docker start puppet-master`**  
   Starts the `puppet-master` container if it is stopped.

4. **`docker exec -it puppet-master bash`**  
   Opens a bash shell inside the running `puppet-master` container.

![Image](https://github.com/user-attachments/assets/ec40f11b-8739-4db2-8d43-779e2052bdf0)

5. **`apt update`**  
   Updates the package list inside the container.

6. **`apt upgrade`**  
   Upgrades all installed packages to their latest versions.

    **_'update' repository kay liye hota hai aur 'upgrade' packages kay     liye hota hai_**

![Image](https://github.com/user-attachments/assets/19aa086c-47e4-4f1a-9f2a-e55c5172a678)

7. **`apt install inetutils-ping net-tools vim wget -y`**  
   Installs essential tools like `ping`, `ifconfig`, `vim`, and `wget` inside the container.

![Image](https://github.com/user-attachments/assets/f2bd52f9-3f79-43ff-9579-11e2d0e441f4)

8. **`wget https://apt.puppetlabs.com/puppet8-release-bionic.deb`**  
   Downloads the Puppet 8 release package for Ubuntu Bionic.

9. **`dpkg -i puppet8-release-bionic.deb`**  
   Installs the Puppet 8 release package.

![Image](https://github.com/user-attachments/assets/4d82f344-583e-40e4-8224-1c448b031f66)

10. **`apt update`**  
    Updates the package list again to include the Puppet repository.

11. **`exit`**  
    Exits the current shell or container.

12. **`docker commit puppet-master puppet-8`**  
    Creates a new Docker image (`puppet-8`) from the `puppet-master` container, saving its current state.

13. **`docker exec -it puppet-master bash`**  
    Opens a bash shell inside the running `puppet-master` container.

14. **`apt install puppetserver`**  
    Installs the Puppet server.

![Image](https://github.com/user-attachments/assets/7b9c32da-3f40-4049-bff4-81548a671501)

**_now java is heavy !_**  


15. **`java --version`**  
    Checks the installed Java version (required for Puppet server).

16. **`vim /etc/default/puppetserver`**  
    Edits the Puppet server configuration file to adjust Java memory settings.

![Image](https://github.com/user-attachments/assets/bac4922a-abb7-4009-9e1d-8afc830fc016)

**_Search for "Modify this if you'd like to change the memory allocation" and reduce the memory of java_**

17. **`JAVA_ARGS="-Xms2g -Xmx2g -Djruby.logger.class=com.puppetlabs.jruby_utils.jruby.Slf4jLogger"`**  
    Default Java memory settings for Puppet server.

18. **`JAVA_ARGS="-Xms64m -Xmx512m -Djruby.logger.class=com.puppetlabs.jruby_utils.jruby.Slf4jLogger"`**  
    Reduced Java memory settings for Puppet server.

19. **`/opt/puppetlabs/bin/puppetserver ca setup`**  
    Sets up the Puppet Certificate Authority (CA).

![Image](https://github.com/user-attachments/assets/fc4fd14c-8fae-44dc-b259-fb5d68a3507a)

20. **`service puppetserver start`**  
    Starts the Puppet server.

21. **`service puppetserver status`**  
    Checks the status of the Puppet server.

![Image](https://github.com/user-attachments/assets/4a5c3196-6c11-4cfd-9a64-83308ba87aeb)

22. **`netstat -nutlp`**  
    Displays active network connections and listening ports.

23. **`vim ~/.bashrc`**  
    Edits the `.bashrc` file to add Puppet binaries to the system PATH.

24. **`PATH=$PATH:/opt/puppetlabs/bin`**  
    Adds the Puppet binaries directory to the PATH.

![Image](https://github.com/user-attachments/assets/fa11e814-9084-4739-ba20-70018ffc4763)

25. **`source ~/.bashrc`**  
    Reloads the `.bashrc` file to apply changes.

    **_now puppet has become a command !_**

26. **`puppet --version`**  
    Checks the installed Puppet version.

![Image](https://github.com/user-attachments/assets/75e8914e-f922-4903-b22b-b622c30ca358)

27. **`echo $PATH`**  
    Displays the current system PATH.

---

### **3. Puppet Resource Management**
28. **`puppet resource user`**  
    Displays information about all users on the system.

29. **`puppet resource user root`**  
    Displays information about the `root` user.

![Image](https://github.com/user-attachments/assets/333dce30-7198-43e7-a1d2-b3b8b6e7d810)

30. **`puppet resource host`**  
    Displays information about the host.

31. **`facter -p`**  
    Displays system facts (information about the system).

32. **`facter -p | grep fqdn`**  
    Filters system facts to display the Fully Qualified Domain Name (FQDN).

![Image](https://github.com/user-attachments/assets/9655bf79-542d-49d1-b4f2-52c0b4fa1259)

---

### **4. Writing and Applying Puppet Manifests**
- **What is a Puppet Manifest file?**  
- A Puppet manifest file, with a ``.pp`` extension, is a text file that contains Puppet code, you can call it configuration language, used to define the desired state of a system. (woh kis haalat mein hona chahiye).
- Manifests define the **desired state of system resources**, such as
    - ensuring a package is installed
    - a file exists with specific content
    - or a service is running. 



33. **`cd /tmp`**  
    Changes the current directory to `/tmp`.

34. **`vim test01.pp`**  
    Creates and edits a Puppet manifest file (`test01.pp`).

![Image](https://github.com/user-attachments/assets/1d00cb5e-7e5a-4fc7-ae70-a9fa87fd4f30)

**_insert the puppet code here_**

35. **`cat test01.pp`**  
    Displays the contents of the `test01.pp` file.

36. **`puppet apply test01.pp`**  
    Applies the Puppet manifest (`test01.pp`) to the system.

![Image](https://github.com/user-attachments/assets/0f024155-6d31-4e89-93a6-f2afe4dfb045)

37. **`cat config-example`**  
    Displays the contents of the `config-example` file.

38. **`vim test01.pp`**  
    Edits the `test01.pp` file to modify the Puppet code.

### **5. Puppet Manifest Example**
```bash
file { '/tmp/config-example':
    ensure => 'file',
    mode => '0644',
    owner => 'root',
    group => 'root',
    content => "Hello $facts{'hostname'} \n",
}

package {'apache2':
    ensure => 'installed'
}

service {'apache2':
    ensure => 'running',
    require => Package['apache2']
}
```

39. **`puppet apply test01.pp`**  
    Applies the updated Puppet manifest (`test01.pp`) to the system.

40. **`cat config-example`**  
    Displays the updated contents of the `config-example` file.

41. **`service apache2 status`**  
    Now Apache should be running.

---
