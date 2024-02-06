# Task 1: Confirming the Café Websites

1. Copy these values from the Credentials panel:
   - CafeInstance1IPAddress
   - PrimaryWebSiteURL
   - SecondaryWebsiteURL
   - CafeInstance2IPAddress

2. Open a new browser tab and paste the value for PrimaryWebSiteURL.
   - Confirm the café application webpage opens.

3. Open another tab and paste the value for SecondaryWebsiteURL.
   - Confirm the second EC2 instance has similar configurations.

4. Choose "Menu," select an item, and submit an order to confirm both instances are running the café application.

# Task 2: Configuring a Route 53 Health Check

1. Create a health check named "Primary-Website-Health":
   - Monitor Endpoint with IP address for CafeInstance1.
   - Path: cafe
   - Request interval: Fast (10 seconds)
   - Failure threshold: 2

2. Choose "Create alarm" and configure email notification.

3. Confirm the health check status on the Monitoring tab.

4. Check your email and confirm the subscription.

# Task 3: Configuring Route 53 Records

## Task 3.1: Creating an A Record for the Primary Website

1. Create an A record:
   - Record name: www
   - Record type: A - Routes traffic to an IPv4 address
   - Value: CafeInstance1IPAddress
   - TTL: 15 seconds
   - Routing policy: Failover
   - Failover record type: Primary
   - Health check ID: Primary-Website-Health
   - Record ID: FailoverPrimary

2. Choose "Create records."

## Task 3.2: Creating an A Record for the Secondary Website

1. Create another A record:
   - Record name: www
   - Record type: A - Routes traffic to an IPv4 address
   - Value: CafeInstance2IPAddress
   - TTL: 15 seconds
   - Routing policy: Failover
   - Failover record type: Secondary
   - Leave Health check ID empty
   - Record ID: FailoverSecondary

2. Choose "Create records."

# Task 4: Verifying DNS Resolution

1. Select one A record, copy the Record name.

2. Open a new browser tab, paste the A record name, add /cafe, and load the page.
   - Confirm the café primary website loads with correct Server Information.

# Task 5: Verifying Failover Functionality

1. In the EC2 Console, stop CafeInstance1.

2. Confirm Primary-Website-Health shows failed health checks.

3. Refresh the opened website tab; confirm it now serves from CafeInstance2.

4. Check your email for AWS Notifications with details on the alarm.
