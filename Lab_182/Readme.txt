Task 1: Creating a new EBS volume
Steps:
  1. Open the AWS Management Console and navigate to EC2 by entering and selecting EC2 in the Search bar.
  2. In the left navigation pane, select Instances.
  3. Note the Availability Zone for the existing EC2 instance named Lab.
  4. In the left navigation pane, under Elastic Block Store, choose Volumes.
  5. Click on "Create volume" and configure the options as follows:
     - Volume type: General Purpose SSD (gp2)
     - Size (GiB): 1
     - Availability Zone: Choose the same as your Lab instance (e.g., us-west-2a).
  6. In the Tags section, add a tag:
     - Key: Name
     - Value: My Volume
  7. Click on "Create volume."

Task 2: Attaching the volume to an EC2 instance
Steps:
  1. Select the newly created volume (My Volume).
  2. From the Actions menu, choose Attach volume.
  3. From the Instance dropdown list, select the Lab instance.
  4. The Device name is set to /dev/sdf. Click on "Attach volume."

Task 3: Connecting to the Lab EC2 instance
Steps:
  1. Open the AWS Management Console and navigate to EC2 by entering and selecting EC2 in the Search bar.
  2. In the navigation pane, choose Instances.
  3. Select the Lab instance and click on "Connect."
  4. On the EC2 Instance Connect tab, choose "Connect."

Task 4: Creating and configuring the file system
Commands:
  df -h  # View available storage
  sudo mkfs -t ext3 /dev/sdf  # Create ext3 file system
  sudo mkdir /mnt/data-store  # Create directory for mounting
  sudo mount /dev/sdf /mnt/data-store  # Mount the new volume
  echo "/dev/sdf   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab  # Update fstab for auto-mount
  cat /etc/fstab  # View fstab configuration
  df -h  # View available storage again
  sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"  # Create a file with some text
  cat /mnt/data-store/file.txt  # Verify the text has been written

Task 5: Creating an Amazon EBS snapshot
Steps:
  1. On the EC2 Management Console, choose Volumes, and select My Volume.
  2. From the Actions menu, choose Create snapshot.
  3. In the Tags section, add a tag:
     - Key: Name
     - Value: My Snapshot.
  4. Click on "Create snapshot."
  5. In the left navigation pane, choose Snapshots to monitor snapshot status.
  6. To delete the file created earlier, run sudo rm /mnt/data-store/file.txt.

Task 6: Restoring the Amazon EBS snapshot
Task 6.1: Creating a volume by using the snapshot
Steps:
  1. On the EC2 Management Console, select My Snapshot.
  2. From the Actions menu, choose Create volume from snapshot.
  3. For Availability Zone, choose the same as before.
  4. In the Tags section, add a tag:
     - Key: Name
     - Value: Restored Volume.
  5. Click on "Create volume."
  6. To view the new volume, choose Volumes in the left navigation.

Task 6.2: Attaching the restored volume to the EC2 instance
Steps:
  1. Select the Restored Volume.
  2. From the Actions menu, choose Attach volume.
  3. From the Instance dropdown list, choose the Lab instance.
  4. The Device name is set to /dev/sdg. Click on "Attach volume."

Task 6.3: Mounting the restored volume
Commands:
  sudo mkdir /mnt/data-store2  # Create directory for mounting
  sudo mount /dev/sdg /mnt/data-store2  # Mount the restored volume
  ls /mnt/data-store2/file.txt  # Verify the presence of the file

Note: Ensure to replace placeholders like Availability Zone, Volume names, and other specific details based on your AWS environment.