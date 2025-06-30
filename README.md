# AWS-Wordfreq-Scalable-Architecture
This project demonstrates the development, deployment, and optimization of "WordFreq," a Go-based application designed to analyze text files, identify, and count the top ten most frequent words.

## Project Overview 
The application leverages various Amazon Web Services (AWS) to create a robust, scalable, and cost-efficient architecture for text processing. This project showcases the implementation of auto-scaling, enhanced resilience, and advanced data processing techniques to handle varying workloads effectively.

### Initial Architecture

<img width="602" alt="Screenshot 2025-06-30 at 6 41 47 PM" src="https://github.com/user-attachments/assets/27fdf075-146c-4638-b50a-956c01d62897" />

### Auto-Scaling Implementation
To handle varying workloads dynamically, auto-scaling was implemented:

1. **Auto Scaling Group (ASG):** Manages the number of EC2 instances running the WordFreq worker process. Initial desired, minimum, and maximum capacities were set to 1, 1, and 8 respectively.
2. **CloudWatch Alarms:**
  - scale-out-alarm: Triggers when ApproximateNumberOfMessagesVisible in wordfreq-jobs queue is >= 50 messages for 1 minute, indicating high load.
  - scale-in-alarm: Triggers when ApproximateNumberOfMessagesVisible in wordfreq-jobs queue is <= 5 messages for 1 minute, indicating low load
3. **Scaling Policies (Simple Scaling):**
  - Simple-Scale-Out: Adds 1 capacity unit (EC2 instance) when scale-out-alarm breaches, with a 150-second wait time before another scaling action.
  - Simple-Scale-In: Removes 1 capacity unit when scale-in-alarm breaches, with a 150-second wait time.

### Performance Optimization
The project systematically optimized processing time through iterative adjustments:
1. **Initial Process (t2.micro, Simple Scaling):** Processed 120 files in 12 minutes 43 seconds, with 6 seconds 350ms per file.
2. **Second Process (t2.micro, Simple Scaling, Optimized ASG/Wait Time):** Adjusted desired/min/max capacity and wait time to 120 seconds. This reduced processing time to 7 minutes 9 seconds, with 3 seconds 500ms per file.
3. **Third Process (t3.large, Step Scaling Policy):** Upgraded instance type to t3.large (2 vCPUs, 8 GB RAM) and implemented a Step scaling policy. This significantly improved performance, reducing processing time to 5 minutes 16 seconds, with 2 seconds 630ms per file. The step scaling policy allows for more gradual and responsive capacity adjustments.

### Redesigned Architecture for Enhanced Resilience, Backup, Cost-Efficiency, and Security
A redesigned architecture addresses the vulnerabilities of a single EC2 instance, focusing on high availability, disaster recovery, cost optimization, and robust security. 

<img width="883" alt="Screenshot 2025-06-30 at 7 18 24 PM" src="https://github.com/user-attachments/assets/d9f50773-20b0-4e9a-8cff-30eb2dac2f46" />

