# Week 5: Docker Basics & Advanced Challenge

Welcome to the Week 5 Docker Challenge! In this task, you will work with Docker concepts and tools taught by Shubham Bhaiya. This challenge covers the following topics:

- **Introduction and Purpose:** Understand Docker’s role in modern development.
- **Virtualization vs. Containerization:** Learn the differences and benefits.
- **Build Kya Hota Hai:** Understand the Docker build process.
- **Docker Terminologies:** Get familiar with key Docker terms.
- **Docker Components:** Explore Docker Engine, images, containers, and more.
- **Project Building Using Docker:** Containerize a sample project.
- **Multi-stage Docker Builds / Distroless Images:** Optimize your images.
- **Docker Hub (Push/Tag/Pull):** Manage and distribute your Docker images.
- **Docker Volumes:** Persist data across container runs.
- **Docker Networking:** Connect containers using networks.
- **Docker Compose:** Orchestrate multi-container applications.
- **Docker Scout:** Analyze your images for vulnerabilities and insights.

Complete all the tasks below and document your steps, commands, and observations in a file named `solution.md`. Finally, share your experience on LinkedIn using the provided guidelines.

---

## Challenge Tasks

### Task 1: Introduction and Conceptual Understanding
1. **Write an Introduction:**  
   - In your `solution.md`, provide a brief explanation of Docker’s purpose in modern DevOps.
   - Compare **Virtualization vs. Containerization** and explain why containerization is the preferred approach for microservices and CI/CD pipelines.

---
**Docker's Purpose in Modern DevOps:** 
Docker is a containerization tool that enables developers and DevOps engineers to package applications and their dependencies into lightweight, portable containers. It runs on the Docker Engine, which interacts with the host operating system's kernel.

On Linux, Docker directly uses the underlying kernel features for efficient container execution. On Windows, Docker Desktop provides a similar experience by running a lightweight Linux VM using WSL 2 (Windows Subsystem for Linux) or Hyper-V, since Windows does not have a native Linux kernel.

The main advantage of Docker over traditional virtualization is its lightweight nature. Unlike virtual machines that require a full guest OS and dedicated resources, Docker containers share the host OS kernel, reducing overhead. This leads to faster startup times, lower resource consumption, and cost efficiency compared to VMs, which require separate OS installations and more dedicated hardware resources.

---
**Virtualization vs. Containerization:** 
| Feature            | Virtualization | Containerization |
|-------------------|---------------|----------------|
| **Isolation**    | Strong (Each VM has its own OS) | Moderate (Shares host OS kernel) |
| **Startup Time** | Slow (Minutes) | Fast (Seconds) |
| **Resource Usage** | High (Requires full OS for each VM) | Low (Shares OS, lightweight) |
| **Portability** | Limited (OS-dependent) | High (Runs anywhere with Docker) |
| **Scalability** | Harder to scale (More overhead) | Easy to scale (Lightweight and fast) |
| **Use Case** | Monolithic applications | Microservices, CI/CD Pipelines |



## Why Containerization is Preferred for Microservices and CI/CD Pipelines

1. **Lightweight & Fast Deployment:** Containers start up in seconds compared to VMs, improving efficiency in **CI/CD pipelines**.
2. **Scalability:** Containers allow **horizontal scaling**, making them ideal for **microservices architectures**.
3. **Portability:** Containers run consistently across different environments (development, testing, production), reducing deployment issues.
4. **Resource Efficiency:** Containers share the host OS kernel, using less memory and CPU compared to VMs.
5. **Continuous Integration & Continuous Deployment (CI/CD):** Containers allow quick builds, tests, and rollbacks, making them a perfect fit for automated **CI/CD workflows**.



**Conclusion:**
While virtualization is useful for running multiple OS instances on a single host, **containerization is the preferred approach** for modern cloud-native applications, **microservices**, and **CI/CD pipelines** due to its efficiency, portability, and scalability.



---

### Task 2: Create a Dockerfile for a Sample Project
1. **Select or Create a Sample Application:**  
   - Choose a simple application (for example, a basic Node.js, Python, or Java app that prints “Hello, Docker!” or serves a simple web page).

***We will use this Java App:***
https://github.com/LondheShubham153/java-quotes-app

2. **Write a Dockerfile:**  
   - Create a `Dockerfile` that defines how to build an image for your application.
   - Include comments in your Dockerfile explaining each instruction.

      ***Dockerfile:***
       ```bash
      #Base Image for Openjdk
      FROM openjdk:17-jdk-alpine
      
      #WORKING DIR
      WORKDIR /app
      
      #COPY Main File first
      COPY /src/Main.java /app
      
      #COPY the required quotes.txt
      COPY quotes.txt quotes.txt
      
      #Run this to install libs and to compile code
      RUN javac Main.java
      
      #Port required for this app.
      EXPOSE 8000
      
      #Run and serves the app
      CMD [ "java","Main" ]
       ```

   - Build your image using:
     ```bash
     docker build -t mubashirahmed324/java-quotes-app:latest .
     ```

2. **Verify Your Build:**  
   - Run your container locally to ensure it works as expected:
     ```bash
     docker run -d -p 8000:8000 --name java-quotes-app mubashirahmed324/java-quotes-app:latest
     ```
   - Verify the container is running with:
     ```bash
     docker ps

     ubuntu@ip-122-21-1-420:~/week5/java-quotes-app$ docker ps
     CONTAINER ID   IMAGE                                     COMMAND       CREATED          STATUS          PORTS                                       NAMES
     c0f6b805d556   mubashirahmed324/java-quotes-app:latest   "java Main"   25 seconds ago   Up 24 seconds   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   java-quotes-app
     ```
   - Check logs using:
     ```bash
     docker logs <container_id>

     docker logs c0f6b8
     
     Result:
     ubuntu@ip-172-31-0-130:~/week5/java-quotes-app$ docker logs c0f6b8
     Server is running on port 8000...
     ```

---

### Task 3: Explore Docker Terminologies and Components
1. **Document Key Terminologies:**  
   - In your `solution.md`, list and briefly describe key Docker terms such as image, container, Dockerfile, volume, and network.


***Key Docker Terms***

- **Image:** A lightweight, stand-alone, and executable package that includes everything needed to run a piece of software (code, runtime, libraries, dependencies, etc.).
- **Container:** A running instance of a Docker image that provides an isolated environment for applications.
- **Dockerfile:** A script that contains a set of instructions for building a Docker image, defining the base image, dependencies, and commands.
- **Volume:** A storage mechanism used to persist data across container restarts, ensuring data is not lost when a container is removed.
- **Network:** A virtual network that enables communication between containers or between a container and the host system, essential for microservices architecture.



   - Explain the main Docker components (Docker Engine, Docker Hub, etc.) and how they interact.


***Docker Components and Their Interaction***

**1. Docker Engine**
The core of Docker that runs and manages containers. 
It includes:
- ***Docker Daemon***: Runs in the background to manage containers.
- ***Docker CLI***: A command-line tool to control Docker.

**2. Docker Hub**
An online registry where Docker images are stored and shared. 
You can:
- ***Pull images*** to use in your projects.
- ***Push images*** to share with others.

**3. Docker Compose**
A tool to manage multi-container applications using a simple YAML file (.yml). 
It helps:
- ***Run multiple services*** together.
- ***Define dependencies*** between containers.

**4. Docker Networking**
Allows containers to communicate with each other and the outside world.
Types:
- ***Bridge***: Default, used for containers on the same host.
- ***Host***: Containers use the host’s network.

**5. How These Components Work Together**
1. ***Docker Engine*** pulls images from ***Docker Hub***.
2. It runs containers based on those images.
3. ***Docker Compose*** manages multiple containers.
4. ***Docker Networking*** enables communication between containers.
---

### Task 4: Optimize Your Docker Image with Multi-Stage Builds
1. **Implement a Multi-Stage Docker Build:**  
   - Modify your existing `Dockerfile` to include multi-stage builds.  
   - Aim to produce a lightweight, **distroless** (or minimal) final image.

      ```bash
      # Stage 1: Build Stage
      FROM openjdk:17-jdk-alpine AS builder
      
      # Set working directory
      WORKDIR /app
      
      # Copy source files
      COPY src/ ./src/
      COPY quotes.txt .
      
      # Compile Java source file
      RUN javac -d . src/Main.java
      
      # Stage 2: Runtime Stage (Distroless)
      FROM gcr.io/distroless/java17-debian12
      
      # Set working directory
      WORKDIR /app
      
      # Copy compiled Java class
      COPY --from=builder /app/Main.class /app/Main.class
      
      # Copy quotes.txt file
      COPY --from=builder /app/quotes.txt /app/quotes.txt
      
      # Expose required port
      EXPOSE 8000
      
      # Run Java application correctly (remove -jar)
      CMD ["/usr/bin/java", "Main"]
      ```

3. **Compare Image Sizes:**  
   - Build your image before and after the multi-stage build modification and compare their sizes using:
     ```bash
     docker images
   

     Result:
     REPOSITORY                                     TAG             IMAGE ID       CREATED          SIZE
     mubashirahmed324/multi-stage-java-quotes-app   latest          fd1485a03aab   9 seconds ago    226MB
     mubashirahmed324/java-quotes-app               latest          6be8b811c49c   57 minutes ago   326MB
     ```
     
4. **Document the Differences:**  
   - Explain in `solution.md` the benefits of multi-stage builds and the impact on image size.

## Differences & Benefits of Multi-Stage Builds
Multi-stage builds help reduce Docker image size by removing unnecessary build dependencies from the final image.

**Key Benefits:**

- ***Smaller Image Size*** – The multi-stage image (226MB) is 100MB smaller than the non-multi-stage one (326MB).
- ***Better Security*** – Unused tools and dependencies are removed, reducing vulnerabilities.
- ***Faster Deployment*** – Smaller images load and start up faster in production.
- ***Optimized Performance*** – Uses only the required runtime environment, improving efficiency.

---

### Task 5: Manage Your Image with Docker Hub
1. **Tag Your Image:**  
   - Tag your image appropriately:
     ```bash
     docker tag <your-username>/sample-app:latest <your-username>/sample-app:v1.0

     i.e.
     docker tag my-java-app:latest mubashirahmed324/my-java-app:v1.0
     ```
2. **Push Your Image to Docker Hub:**  
   - Log in to Docker Hub if necessary:
     ```bash
     docker login
     ```
   - Push the image:
     ```bash
     docker push <your-username>/sample-app:v1.0

     i.e.
     docker push mubashirahmed324/my-java-app:v1.0

     Result:
     The push refers to repository [docker.io/mubashirahmed324/my-java-app]
     28f6bd086ef8: Pushed
     c200b4a2bd0d: Pushed
     fc625878c069: Pushed
     fb9cc081b244: Pushed
     34f7184834b2: Mounted from library/openjdk
     5836ece05bfd: Mounted from library/openjdk
     72e830a4dff5: Mounted from library/openjdk
     v1.0: digest: sha256:c7dd3ad428b28d06ecc1cfd928acc4f40a2c4afa89cd884342b51105dc47e80b size: 1780
     ```
3. **(Optional) Pull the Image:**  
   - Verify by pulling your image:
     ```bash
     docker pull <your-username>/sample-app:v1.0

     i.e.
     docker pull mubashirahmed324/my-java-app:v1.0

     Result:
     v1.0: Pulling from mubashirahmed324/my-java-app
     Digest: sha256:c7dd3ad428b28d06ecc1cfd928acc4f40a2c4afa89cd884342b51105dc47e80b
     Status: Image is up to date for mubashirahmed324/my-java-app:v1.0
     docker.io/mubashirahmed324/my-java-app:v1.0
     ```
---

### Task 6: Persist Data with Docker Volumes
1. **Create a Docker Volume:**  
   - Create a Docker volume:
     ```bash
     docker volume create java_app_volume
     ```
2. **Run a Container with the Volume:**  
   - Run a container using the volume to persist data:
     ```bash
     docker run -d -v my_volume:/app/data <your-username>/sample-app:v1.0

     i.e.
     docker run -d -v java_app_volume:/data -p 8000:8000 --name multi-stage-java my-java-app
     ```
3. **Document the Process:**  
   - In `solution.md`, explain how Docker volumes help with data persistence and why they are useful.

***Docker Volumes for Data Persistence***
Docker volumes are used to persist data outside a container's lifecycle. When a container stops or is removed, its internal data is lost. Volumes solve this by storing data separately from the container, allowing it to be reused across restarts and multiple containers.

- Persistent Storage: Keeps data safe even if the container is deleted.
- Sharing Data: Multiple containers can access the same volume.
- Better Performance: Volumes are optimized for Docker and perform better than bind mounts.
- Easier Backups: Data stored in volumes can be easily backed up and restored.

---

### Task 7: Configure Docker Networking
1. **Create a Custom Docker Network:**  
   - Create a custom Docker network:
     ```bash
     docker network create my_network

     i.e.
     docker network create java-app-network
     ```
2. **Run Containers on the Same Network:**  
   - Run two containers (e.g., your sample app and a simple database like MySQL) on the same network to demonstrate inter-container communication:
     ```bash
     docker run -d --name sample-app --network my_network <your-username>/sample-app:v1.0
     docker run -d --name my-db --network my_network -e MYSQL_ROOT_PASSWORD=root mysql:latest

     i.e.
     docker run -d --name multi-stage-java --network java-app-network mubashirahmed324/my-java-app:v1.0
     docker run -d --name java-database --network java-app-network -e MYSQL_ROOT_PASSWORD=root mysql:latest

     To check and verify:
     docker inspect java-app-network
     ```
3. **Document the Process:**  
   - In `solution.md`, describe how Docker networking enables container communication and its significance in multi-container applications.

   ***Docker Networking for Container Communication***
   Docker networking allows containers to communicate with each other and external systems efficiently. By default, Docker provides different network types to suit various use cases:

   - Bridge Network (Default): Enables communication between containers on the same host.
   - None: Complete isolation
   - Host Network: Removes network isolation, using the host machine’s network directly.
   - User Defined Bridge Network: Connects containers across multiple Docker hosts, ideal for distributed applications.
   
   ***Importance in Multi-Container Applications***
   - Service-to-Service Communication: Containers can interact without exposing services to the host machine.
   - Scalability: Supports dynamic service discovery in microservices architectures.
   - Security: Network isolation ensures controlled access between containers.
---

### Task 8: Orchestrate with Docker Compose
1. **Create a docker-compose.yml File:**  
   - Write a `docker-compose.yml` file that defines at least two services (e.g., your sample app and a database).
   - Include definitions for services, networks, and volumes.
2. **Deploy Your Application:**  
   - Bring up your application using:
     ```bash
     docker-compose up -d
     ```
   - Test the setup, then shut it down using:
     ```bash
     docker-compose down
     ```
3. **Document the Process:**  
   - Explain each service and configuration in your `solution.md`.

---

### Task 9: Analyze Your Image with Docker Scout
1. **Run Docker Scout Analysis:**  
   - Execute Docker Scout on your image to generate a detailed report of vulnerabilities and insights:
     ```bash
     docker scout cves <your-username>/sample-app:v1.0
     ```
   - Alternatively, if available, run:
     ```bash
     docker scout quickview <your-username>/sample-app:v1.0
     ```
     to get a summarized view of the image’s security posture.
   - **Optional:** Save the output to a file for further analysis:
     ```bash
     docker scout cves <your-username>/sample-app:v1.0 > scout_report.txt
     ```

2. **Review and Interpret the Report:**  
   - Carefully review the output and focus on:
     - **List of CVEs:** Identify vulnerabilities along with their severity ratings (e.g., Critical, High, Medium, Low).
     - **Affected Layers/Dependencies:** Determine which image layers or dependencies are responsible for the vulnerabilities.
     - **Suggested Remediations:** Note any recommended fixes or mitigation strategies provided by Docker Scout.
   - **Comparison Step:** If possible, compare this report with previous builds to assess improvements or regressions in your image's security posture.
   - If Docker Scout is not available in your environment, document that fact and consider using an alternative vulnerability scanner (e.g., Trivy, Clair) for a comparative analysis.

3. **Document Your Findings:**  
   - In your `solution.md`, provide a detailed summary of your analysis:
     - List the identified vulnerabilities along with their severity levels.
     - Specify which layers or dependencies contributed to these vulnerabilities.
     - Outline any actionable recommendations or remediation steps.
     - Reflect on how these insights might influence your image optimization or overall security strategy.
   - **Optional:** Include screenshots or attach the saved report file (`scout_report.txt`) as evidence of your analysis.

---

### Task 10: Documentation and Critical Reflection
1. **Update `solution.md`:**  
   - List all the commands and steps you executed.
   - Provide explanations for each task and detail any improvements made (e.g., image optimization with multi-stage builds).
2. **Reflect on Docker’s Impact:**  
   - Write a brief reflection on the importance of Docker in modern software development, discussing its benefits and potential challenges.

---

## 📢 How to Submit

1. **Push Your Final Work:**  
   - Ensure that your complete project—including your `Dockerfile`, `docker-compose.yml`, `solution.md`, and any additional files (e.g., the Docker Scout report if saved)—is committed and pushed to your repository.  
   - Verify that all your changes are visible in your repository.

2. **Create a Pull Request (PR):**  
   - Open a PR from your working branch (e.g., `docker-challenge`) to the main repository.  
   - Use a clear and descriptive title, for example:  
     ```
     Week 5 Challenge - DevOps Batch 9: Docker Basics & Advanced Challenge
     ```
   - In the PR description, include the following details:
     - A brief summary of your approach and the tasks you completed.
     - A list of the key Docker commands used during the challenge.
     - Any insights or challenges you encountered (e.g., lessons learned from multi-stage builds or Docker Scout analysis).

3. **Share Your Experience on LinkedIn:**  
   - Write a LinkedIn post summarizing your Week 5 Docker challenge experience.  
   - In your post, include:
     - A brief description of the challenge and what you learned.
     - Screenshots, logs, or excerpts from your `solution.md` that highlight key steps or interesting findings (e.g., Docker Scout reports).
     - The hashtags: **#90DaysOfDevOps #Docker #DevOps**
     - Optionally, links to any blog posts or related GitHub repositories that further explain your journey.

---

## Additional Resources

- **[Docker Documentation](https://docs.docker.com/)**  
- **[Docker Hub](https://docs.docker.com/docker-hub/)**  
- **[Multi-stage Builds](https://docs.docker.com/develop/develop-images/multistage-build/)**  
- **[Docker Compose](https://docs.docker.com/compose/)**  
- **[Docker Scan (Vulnerability Scanning)](https://docs.docker.com/engine/scan/)**  
- **[Containerization vs. Virtualization](https://www.docker.com/resources/what-container)**

---

Happy coding and best of luck with this Docker challenge! Document your journey thoroughly in `solution.md` and refer to these resources for additional guidance.
