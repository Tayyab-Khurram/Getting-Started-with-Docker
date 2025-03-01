## **Reusing Containers**

This section covers commands related to reusing Docker containers, including creating images from containers, saving/loading images, and managing containers and images.

---

### **1. Creating an Image from a Container**
2. **`docker images`**  
   Lists all Docker images available on the host.

1. **`docker commit apache-container copy-of-ubuntu-apache`**  
   Creates a new Docker image (`copy-of-ubuntu-apache`) from the `apache-container`. This allows you to save the current state of the container as an image for reuse.

2. **`docker images`**  
   Lists all Docker images available on the host, including the newly created `copy-of-ubuntu-apache`.

![Image](https://github.com/user-attachments/assets/09aec623-ccc8-4dd1-ba62-93ca0b7af984)
---

### **2. Saving Docker Images**
3. **`docker save copy-of-ubuntu-apache -o copy-of-ubuntu-apache.tar`**  
   Saves the `copy-of-ubuntu-apache` image as a `.tar` file. This file can be transferred to another machine or archived.

4. **`ll -h | grep ubuntu`**  
   Lists files in the current directory with human-readable sizes and filters for files containing "ubuntu". This helps verify the creation of the `.tar` file.

    **_now we have the file ready that  
    we can take on any other machine that has docker installed_**

7. **`docker container rm apache-container`**  
   Removes the `apache-container` container.

8. **`docker container rm mysql-container`**  
   Removes the `mysql-container` container.

![Image](https://github.com/user-attachments/assets/d8a1a5b0-b8f1-4aa2-8927-af4f2ce86d43)

6. **`docker images`**  
   Lists all Docker images after loading the `.tar` file to confirm the image is available.

9. **`docker rmi ubuntu`**  
   Removes the `ubuntu` Docker image from the local system.

10. **`docker rmi copy-of-ubuntu-apache`**  
    Removes the `copy-of-ubuntu-apache` Docker image from the local system.

![Image](https://github.com/user-attachments/assets/da9c99b4-ef87-40e3-9c8d-84ac29de7d0d)
---

### **4. Reusing the Saved Image**
11. **`ll -h | grep ubuntu`**  
    Lists files in the current directory with human-readable sizes and filters for files containing "ubuntu". This helps verify the removal of the `.tar` file.

5. **`docker load < copy-of-ubuntu-apache.tar`**  
   Loads the Docker image from the `.tar` file (`copy-of-ubuntu-apache.tar`) into the local Docker environment.

12. **`docker run --name kuch_bhi -it copy-of-ubuntu-apache`**  
    Creates and starts a new container named `kuch_bhi` from the `copy-of-ubuntu-apache` image, with an interactive terminal. This allows you to reuse the saved image with all its files and settings intact.

![Image](https://github.com/user-attachments/assets/d90e49e6-d60b-410c-b0a8-a1ef9db599f1)

#### This is how we reused the existing docker image and packed it into a `.tar` file
