# Configuration of AWS console for Kubernetes, Jenkins and Ansible

Steps :

1. Create and launch an EC2 instance 
2. Install AWS Cli Bundle and Python in your local system.
    
    ```Shell
    curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
    sudo apt-get install unzip python
    unzip awscli-bundle.zip
    ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
    ```
3. Install kubectl on ubuntu instance

    ```Shell
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
    ```
4. Install kops on ubuntu instance

    ```Shell
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
    ```
5. Create IAM roles for EC2FullAccess , Route53FullAccess , S3FullAccess , IAMFullAccess

6. Attach IAM Role with your EC2 Instance 

7. Create an S3 bucket for storing intermediate data and check whether your IAM role is working correctly with this command 
      
    ```shell
    aws s3 ls
    ```

8. Create a Route53 private hosted zone in AWS console:
    ```shell
    name: pipeline01
    ```

9. Expose environment variables:
    ```shell
     export KOPS_STATE_STORE=s3://devops-pipeline-01
     ```

10. Create SSH Key Pairs:
    ```shell
    ssh-keygen
    ```

11. Create kubernetes cluster definition on S3 bucket:
    ```shell
    cluster --cloud=aws --zones=ap-south-1b --name=devops.pipeline.01 --dns-zone=pipeline01 --dns private
    ```
    
12. Create Kubernetes Cluster 
    ```shell
    kops update cluster devops.pipeline.01 --yes
    ```

13. 

