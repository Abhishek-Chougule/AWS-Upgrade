# Server Upgrade Documentation for AWS Instance (V14)

## Introduction

This documentation outlines the process of upgrading an AWS instance from a c5.xlarge to a recommended c5.2xlarge configuration. In addition, it includes steps to separate Redis and add swap memory to optimize performance and address high CPU utilization issues.

## Prerequisites

Before you begin the upgrade, ensure you have the following prerequisites in place:

1. AWS account credentials with administrative access.
2. Access to the AWS Management Console or AWS CLI for instance management.
3. Existing AWS instance (c5.xlarge).
4. A clear understanding of your server's configuration and applications running on it.

## Upgrade Steps

### 1. Separate Redis for V14

In the interest of improving system performance and security, it is recommended to separate Redis from the primary server. Here's how to do it:

#### a. Launch a New AWS Instance for Redis:

1. Log in to the AWS Management Console.
2. Navigate to the EC2 Dashboard.
3. Click "Launch Instance" to create a new instance.
4. Choose an appropriate Redis instance type (e.g., Amazon ElastiCache or EC2 with Redis installed).
5. Configure the security groups, network settings, and storage as required.
6. Launch the instance and ensure Redis is up and running.

#### b. Update Application Configuration:

1. Update your application's configuration to point to the new Redis instance.
2. Modify security groups and access control settings to allow communication with Redis.

### 2. Upgrade the Existing Instance

To upgrade your existing c5.xlarge instance to c5.2xlarge, follow these steps:

1. Log in to the AWS Management Console.

2. Navigate to the EC2 Dashboard.

3. Select your existing c5.xlarge instance.

4. Stop the instance to prepare for the upgrade.

5. Click "Actions" > "Instance Settings" > "Change Instance Type."

6. Select the "c5.2xlarge" instance type and confirm the change.

7. Start the upgraded instance.

### 3. Add Swap Memory

Adding swap memory can help alleviate RAM usage issues. Here's how you can add swap memory to your server:

1. SSH into the upgraded c5.2xlarge instance.

2. Run the following commands to create a swap file:

   ```shell
   sudo fallocate -l 1G /swapfile
   sudo chmod 600 /swapfile
   sudo mkswap /swapfile
   sudo swapon /swapfile
   ```

3. To make the swap permanent, add the following line to the `/etc/fstab` file:

   ```
   /swapfile   none    swap    sw    0   0
   ```

4. Adjust swap settings for improved performance. Edit `/etc/sysctl.conf` and add or modify the following lines:

   ```
   vm.swappiness = 10
   vm.vfs_cache_pressure = 50
   ```

5. Apply the changes with the command:

   ```shell
   sudo sysctl -p
   ```

6. Ensure the swap is active and working:

   ```shell
   sudo swapon --show
   ```

7. Verify that the system is using swap when needed, reducing RAM usage.

## Conclusion

By following these steps, you have successfully separated Redis for V14, upgraded your AWS instance, and added swap memory to optimize its performance. These actions should help alleviate CPU and RAM utilization issues, resulting in a more reliable and responsive server.
