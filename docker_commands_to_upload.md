## **1. Docker Installation and Setup**
1. **`apt update`**  
   Updates the package list on the host system.

2. **`apt install docker.io`**  
   Installs Docker on the host system.

3. **`systemctl start docker`**  
   Starts the Docker service.

4. **`systemctl enable docker`**  
   Enables Docker to start on system boot.

---

## **2. Docker Image Management**
5. **`docker images`**  
   Lists all Docker images available on the host.

6. **`docker pull ubuntu`**  
   Downloads the official Ubuntu image from Docker Hub.

![Image](https://github.com/user-attachments/assets/1e2fb663-b125-4a12-944f-e6b648616a53)
![Image](https://github.com/user-attachments/assets/23093bc8-dc7a-4530-a9fc-02b0ca1b8439)

---

## **3. Creating MySQL Container**
7. **`docker run --name mysql-container -it ubuntu`**  
   Creates and starts a new container named `mysql-container` from the Ubuntu image, with an interactive terminal.

![Image](https://github.com/user-attachments/assets/d7c33912-23f0-495f-8901-7c11df5a8215)

   **_Currently, I am inside the mysql-container._**

8. **`apt update` (inside container)**  
   Updates the package list inside the container.

9. **`apt install vim -y`**  
   Installs the `vim` text editor inside the container.

10. **`apt install inetutils-ping net-tools -y`**  
    Installs networking tools like `ping` and `ifconfig` inside the container.

11. **`exit`**  
    Exits the current shell or container.

12. **`watch "docker ps -a"` (run this in another terminal)**  
    Continuously monitors the status of all containers (running and stopped).

![Image](https://github.com/user-attachments/assets/358b6fcf-9af8-416e-b3e2-0b45291438ff)

13. **`docker start mysql-container`**  
    Starts a stopped container named `mysql-container`.

14. **`docker exec -it mysql-container bash`**  
    Opens a bash shell inside the running `mysql-container`.

---

## **4. Configuration of MySQL Container**
15. **`apt install mysql-server -y`**  
    Installs MySQL server inside the container.

![Image](https://github.com/user-attachments/assets/1b13bae7-6c4f-4e0f-9959-d3a345381b89)

16. **`service mysql start`**  
    Starts the MySQL service inside the container.

17. **`mysql`**  
    Opens the MySQL command-line interface.

18. **`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'superstrongpassword';`**  
    Changes the authentication method and password for the MySQL root user.

![Image](https://github.com/user-attachments/assets/c8fc8b09-d7fc-4acc-92d6-ce9484021b8c)

19. **`FLUSH PRIVILEGES;`**  
    Reloads the MySQL privilege tables to apply changes.

20. **`\q`**  
    Exits the MySQL shell.

21. **`mysql -u root -p`**  
    Logs into MySQL as the root user with a password.

22. **`exit`**  
    Exits the current shell or container.

---

## **5. Creating Apache Container**
23. **`docker run --name apache-container -it ubuntu`**  
    Creates and starts a new container named `apache-container` from the Ubuntu image, with an interactive terminal.

![Image](https://github.com/user-attachments/assets/eced565e-16a6-42f6-b12e-2a09e37ef50b)

    Currently, I am inside the apache-container.

24. **`apt update` (inside container)**  
    Updates the package list inside the container.

25. **`exit`**  
    Exits the current shell or container.

26. **`docker start apache-container`**  
    Starts the `apache-container`.

27. **`docker exec -it apache-container bash`**  
    Opens a bash shell inside the running `apache-container`.

![Image](https://github.com/user-attachments/assets/da3c8ee3-5a37-4595-8b93-5e81aa2fb360)

28. **`apt install apache2 -y`**  
    Installs the Apache web server inside the container.

29. **`service apache2 start`**  
    Starts the Apache web server.

30. **`apt install vim -y`**  
    Installs the `vim` text editor inside the container.

31. **`apt install php libapache2-mod-php php-cli php-mysql -y`**  
    Installs PHP and necessary modules for Apache and MySQL integration.

32. **`apt install inetutils-ping net-tools -y`**  
    Installs networking tools like `ping` and `ifconfig` inside the container.

33. **`netstat -nutlp`**  
    Displays active network connections and listening ports.

34. **`service apache2 status`**  
    Checks the status of the Apache service.

35. **`service apache2 restart`**  
    Restarts the Apache service.

36. **`vim /var/www/html/index.php`**  
    Edits the `index.php` file in the Apache web root directory.

    ```php
    <?php
        phpinfo();
    ?>
    ```

![Image](https://github.com/user-attachments/assets/19080527-665a-4ecd-950e-249b47dbdf87)
![Image](https://github.com/user-attachments/assets/049ead01-decc-425a-b6e3-2930e09c56c3)
![Image](https://github.com/user-attachments/assets/412fcc44-b508-4d4b-84cf-9a2b1ef9312f)

---

## **6. Networking Between Containers**
37. **`exit`**  
    Exits the current shell or container.

38. **`docker exec -it mysql-container bash`**  
    Opens a bash shell inside the running `mysql-container`.

39. **`ifconfig`**  
    Displays network interface configuration (inside the MySQL container).  
    Note down the IP address!

40. **`exit`**  
    Exits the current shell or container.

41. **`docker exec -it apache-container bash`**  
    Opens a bash shell inside the running `apache-container`.

42. **`ifconfig`**  
    Displays network interface configuration (inside the Apache container).  
    Note down the IP address!

43. **`ping 172.17.0.2`**  
    Pings the MySQL container from the Apache container.

![Image](https://github.com/user-attachments/assets/ef24adea-da30-4217-9951-1f362ae4eae5)

**_Now you can exit and enter into the other machine by yourself_**

44. **`ping 172.17.0.3`**  
    Pings the Apache container from the MySQL container.

![Image](https://github.com/user-attachments/assets/2beadc58-1d3e-425b-8036-675d880b617d)

**_These both should ping each other_**

**_Go to the Apache container and edit this file_**

45. **`vim /var/www/html/index.php`**  
    Edits the `index.php` file in the Apache web root directory.

    **_Add this code in this file_**

![Image](https://github.com/user-attachments/assets/6312ba37-f442-4a71-8def-673282e73a22)
![Image](https://github.com/user-attachments/assets/a1ebc7da-6897-4c7e-96c7-a97f9a130caf)

46. **`service apache2 status`**  
    Checks the status of the Apache service.

47. **`service apache2 restart`**  
    Restarts the Apache service.

48. **`netstat -nutlp`**  
    Displays active network connections and listening ports.

    **_Now go to the browser and type "http://172.17.0.3/index.php"_**

    **_If you get this "Error: Connection refused"_**  
    **_then exit from the apache-container and now_**  
    **_we will troubleshoot the issue_**

---

## **7. MySQL Configuration for Remote Access**
49. **`docker exec -it mysql-container bash`**  
    Opens a bash shell inside the running `mysql-container`.

50. **`netstat -nutlp`**  
    Displays active network connections and listening ports.

    **_It is listening on "127.0.0.1:3306" means it is only listening on the localhost but we want it to listen on all the interfaces "0.0.0.0:3306"_**.

    **_Search on any AI tool how to make MySQL listen on all the ports._**

![Image](https://github.com/user-attachments/assets/c7fddab0-d985-4e1b-9b53-dce9f33d7ac1)

51. **`vim /etc/mysql/mysql.conf.d/mysqld.cnf`**  
    Edits the MySQL configuration file.

    **_Change the bind-address from 127.0.0.1 to 0.0.0.0_**

52. **`bind-address = 0.0.0.0`**  
    Configures MySQL to listen on all network interfaces (not just `localhost`).

53. **`service mysql restart`**  
    Restarts the MySQL service to apply configuration changes.

    **_Go to "http://172.17.0.3/index.php" again_**

54. **`mysql -u root -p`**  
    Logs into MySQL as the root user with a password.

55. **`CREATE USER 'root'@'%' IDENTIFIED WITH 'mysql_native_password' BY 'superstrongpassword';`**  
    Creates a MySQL user `root` that can connect from any host.

56. **`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';`**  
    Grants all privileges to the `root` user from any host.

57. **`FLUSH PRIVILEGES;`**  
    Reloads the MySQL privilege tables to apply changes.

![Image](https://github.com/user-attachments/assets/f65f18a2-312f-4bd5-a735-df3354656060)

---

## **8. Successfully Connected Apache with MySQL**
![Image](https://github.com/user-attachments/assets/f3d1c3d7-7cf4-47d3-b478-689d91fb34d1)

---

## **9. Troubleshooting and Debugging**
58. **`netstat -nutlp`**  
    Checks which ports MySQL is listening on (e.g., `127.0.0.1:3306` vs `0.0.0.0:3306`).

59. **`service mysql status`**  
    Checks the status of the MySQL service.

60. **`service apache2 status`**  
    Checks the status of the Apache service.

61. **`172.17.0.3/index.php`**  
    Accesses the `index.php` file in the browser to test the connection between Apache and MySQL.
