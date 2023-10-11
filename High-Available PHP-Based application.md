
Below the diagram of a PHP-Based application solution architecture using AWS:
![[php-based aws diagram.png]]

---

## AWS Architecture Overview: PHP-Based Application

This AWS architecture showcases the deployment of a PHP-based application within a Virtual Private Cloud (VPC) spanning multiple private subnets. It emphasizes resilience, scalability, and a seamless user experience.

### Components and Flow:

1. **Users**:
   - Users interact with the system via the internet.
  
2. **Amazon S3 (Simple Storage Service)**:
   - Represents static content storage (like images, stylesheets, JS files).
   - Directly accessed by users for faster content retrieval.

3. **Amazon CloudFront**:
   - Content Delivery Network (CDN) to serve content to users from the nearest edge location, ensuring low latency.

4. **Amazon RDS (Relational Database Service)**:
   - **Primary**: Holds the master copy of the application's relational data.
   - **Standby**: Acts as a failover in case the primary instance encounters issues. Data is asynchronously replicated from the primary.
   - **Multi-AZ**: The deployment spans multiple availability zones to ensure high availability and failover support for DB instances.

5. **Amazon EFS (Elastic File System)**:
   - Provides scalable file storage for the PHP application.
   - **EFS Mount targets in Private Subnets**: Ensures the PHP application can read/write files irrespective of the instance handling the request.

6. **EC2 Instances with Auto Scaling**:
   - Hosts the PHP-based application.
   - Resides within private subnets, ensuring security.
   - **Auto Scaling Group**: Ensures the application can handle varying loads by automatically scaling the number of instances.

7. **VPC (Virtual Private Cloud)**:
   - Provides an isolated environment for resources.
   - Contains multiple private subnets distributed across different Availability Zones for redundancy and high availability.

8. **Cached Data**:
   - Represents in-memory data storage for faster data retrieval.
   - Could represent services like ElastiCache.

### Workflow:

1. Users request static content which is served directly from S3 or via CloudFront for optimal performance.
2. Dynamic content requests are routed to the PHP application hosted on EC2 instances. These instances scale according to demand due to the Auto Scaling group.
3. The PHP application interacts with the RDS database for data persistence. In the event of a primary database failure, traffic is automatically routed to the standby instance.
4. For file operations, the PHP application utilizes EFS, ensuring persistent and scalable file storage.
