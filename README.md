# Installing Jenkins on Docker Container on AWS:

This guide details the steps to install Jenkins on a Docker container running on an AWS instance. We'll cover installing Java 11 (a prerequisite) and integrating Jenkins with GitHub using webhooks for streamlined development workflows.

## Prerequisites

- An AWS account with access to create EC2 instances.
- Docker installed on your AWS instance. Refer to the official Docker documentation for installation instructions: [https://docs.docker.com/](https://docs.docker.com/)

## Software

- Jenkins docker image

## Steps

1. Install Java 11:

    Connect to your AWS instance through SSH.

    Update package lists:

    ```bash
    sudo yum update -y
    ```

2. Install OpenJDK 11:

    ```bash
    sudo yum install java-11-openjdk-devel -y
    ```

3. Run Jenkins in a Docker Container:

    Pull the latest Jenkins image:

    ```bash
    docker pull jenkins/jenkins:lts
    ```

    Start a Jenkins container with persistent storage for configuration:

    ```bash
    docker run -d -p 8080:8080 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
    ```

    Explanation:

    - `-d`: Runs the container in detached mode (background).
    - `-p 8080:8080`: Maps the container's port 8080 to the host machine's port 8080, making Jenkins accessible through your browser at http://<your_aws_instance_ip>:8080.
    - `-v jenkins_home:/var/jenkins_home`: Creates a persistent volume named `jenkins_home` that maps to the `/var/jenkins_home` directory inside the container. This ensures that Jenkins configuration persists even if the container restarts.

4. Wait for Jenkins to Start:

    It might take a few minutes for Jenkins to initialize. You can check the container logs to see its status:

    ```bash
    docker logs <container_id>
    ```

5. Configure Jenkins:

    Access the web interface at http://<your_aws_instance_ip>:8080. You'll need the initial password, which can be found in the container logs using the command mentioned previously.

    Follow the on-screen instructions to complete the initial setup and install any additional plugins you need.

6. Generate SSH Key Pair (on Jenkins Server):

    Open a terminal window on your Jenkins server.

    Execute the following command, replacing `your_email@example.com` with your actual email address:

    ```bash
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```

    Use this command with caution. It generates a new SSH key pair (private and public) using the specified email address as a comment. Follow the prompts, entering a strong passphrase for added security.

    The private key will be saved to a file named `id_rsa` by default. Do not share this file with anyone!

7. Add Public Key to GitHub Repository:

    Log in to your GitHub web interface.

    Navigate to your user settings: [http://your-GitHub-repository/user/settings/](http://your-GitHub-repository/user/settings/)

    Select "SSH Keys" from the left-hand menu.

    Click "Add key."

    Enter a descriptive title for the key (e.g., "Jenkins Server Key").

    Paste the contents of your public key file (usually located at `~/.ssh/id_rsa.pub`) into the "Key" field. You can typically access the public key content using:

    ```bash
    cat ~/.ssh/id_rsa.pub
    ```

    Click "Add Key" to save the public key to your GitHub account.

8. Store Private Key Securely in Jenkins:

    [Add instructions here for storing the private key securely in Jenkins.]

