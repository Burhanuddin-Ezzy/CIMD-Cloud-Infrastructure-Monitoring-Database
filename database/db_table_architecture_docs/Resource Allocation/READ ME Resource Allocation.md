# Resource Allocation

This table tracks how computing resources (CPU, memory, disk space) are allocated to different applications on servers. It is critical for resource monitoring, capacity planning, cost optimization, and ensuring workloads receive the necessary resources.

[**When a New Application is Deployed on a Server**](When%20a%20New%20Application%20is%20Deployed%20on%20a%20Server.md)

### **Data Points (Columns) – Purpose, Thought Process, and Data Type**

- **`server_id` (UUID, Foreign Key)**
    
    **Purpose**: Links to the server being allocated resources.
    
    **How I Thought of Including It**: Essential for tracking how resources are distributed across servers.
    
    **Why I Thought of Including It**: Enables monitoring of per-server utilization.
    
    **Data Type Used & Why**: `UUID` to ensure uniqueness and compatibility with the **Server Metrics** table.
    
- **`app_id` (UUID, Foreign Key)**
    
    **Purpose**: Identifies the application running on the server.
    
    **How I Thought of Including It**: Needed to distinguish resource usage per application.
    
    **Why I Thought of Including It**: Helps in multi-tenancy environments where multiple applications run on shared infrastructure.
    
    **Data Type Used & Why**: `UUID` for globally unique application identification.
    
- **`workload_type` (VARCHAR(50))**
    
    **Purpose**: Specifies the workload assigned to the server (e.g., web server, database, ML training).
    
    **How I Thought of Including It**: Different workloads require different resource allocations.
    
    **Why I Thought of Including It**: Enables workload-based optimizations and autoscaling.
    
    **Data Type Used & Why**: `VARCHAR(50)` as workload types are predefined but flexible.
    
- **`allocated_memory` (INTEGER, MB or GB)**
    
    **Purpose**: Stores the total memory allocated to the server.
    
    **How I Thought of Including It**: Memory is a key resource constraint in performance.
    
    **Why I Thought of Including It**: Helps prevent overprovisioning and underutilization.
    
    **Data Type Used & Why**: `INTEGER` (memory size in MB or GB for consistency).
    
- **`allocated_cpu` (DECIMAL(5,2), Cores or %)**
    
    **Purpose**: Stores the total CPU allocated to the server.
    
    **How I Thought of Including It**: CPU allocation directly impacts application performance.
    
    **Why I Thought of Including It**: Needed for capacity planning and load balancing.
    
    **Data Type Used & Why**: `DECIMAL(5,2)` to support fractional CPU allocations.
    
- **`allocated_disk_space` (INTEGER, GB)**
    
    **Purpose**: Stores the disk space allocated to the server.
    
    **How I Thought of Including It**: Storage is a major cost factor and must be tracked.
    
    **Why I Thought of Including It**: Ensures applications have sufficient storage for logs, databases, and operations.
    
    **Data Type Used & Why**: `INTEGER` (GB for easy computation and aggregation).
    
- **`resource_tag` (VARCHAR(100))**
    
    [**Difference Between `resource_tag` and `workload_type`**](Difference%20Between%20resource_tag%20and%20workload_type.md)
    
    **Purpose**: Labels servers based on project, department, or team.
    
    **How I Thought of Including It**: Required for resource grouping and access control.
    
    **Why I Thought of Including It**: Helps in cost allocation, access management, and resource segmentation.
    
    **Data Type Used & Why**: `VARCHAR(100)`, allowing flexibility for tagging conventions.
    
- **`timestamp` (TIMESTAMP WITH TIME ZONE)**
    
    **Purpose**: Records when the resource allocation was assigned or last updated.
    
    **How I Thought of Including It**: Needed to track allocation trends and allow rollbacks to previous configurations.
    
    **Why I Thought of Including It**: Helps in resource planning, auditing, and historical analysis.
    
    **Data Type Used & Why**: `TIMESTAMP WITH TIME ZONE` to ensure accurate time tracking across different regions.
    
- **`utilization_percentage` (DECIMAL(5,2)) [Do this for each resource ? cpu, diskspace, memory]**
    
    **Purpose**: Tracks how much of the allocated resources are actually being used.
    
    **How I Thought of Including It**: Helps identify underutilized or overprovisioned resources.
    
    **Why I Thought of Including It**: Enables cost-saving optimizations by identifying wasted resources.
    
    **Data Type Used & Why**: `DECIMAL(5,2)` for precision in tracking usage as a percentage.
    
- **`autoscaling_enabled` (BOOLEAN)**
    
    [**Autoscaling Data Point Handbook**](Autoscaling%20Data%20Point%20Handbook.md)
    
    **Purpose**: Indicates whether the allocation is static or managed by an auto scaler.
    
    **How I Thought of Including It**: Important for cloud environments where resource scaling is dynamic.
    
    **Why I Thought of Including It**: Helps in automation, ensuring applications have the right resources at the right time.
    
    **Data Type Used & Why**: `BOOLEAN` to store `TRUE` or `FALSE` values indicating auto scaling status.
    
- **`max_allocated_memory` (INTEGER, MB or GB)**
    
    [**What is `max_allocated_memory`?**](What%20is%20max_allocated_memory.md)
    
    - **Purpose**: Tracks the highest memory allocation recorded for an application.
    - **How I Thought of Including It**: Applications often experience spikes in memory usage. Tracking peak allocation helps in autoscaling and capacity planning.
    - **Why I Thought of Including It**: Helps in **understanding peak demand patterns** and setting **autoscaling thresholds** to avoid performance bottlenecks.
    - **Data Type Used & Why**: `INTEGER` (MB or GB) because memory allocation is always a whole number and needs to be comparable with `allocated_memory`.
- **`max_allocated_cpu` (DECIMAL(5,2), Cores or %)**
    - **Purpose**: Stores the highest CPU allocation recorded for an application.
    - **How I Thought of Including It**: Some applications, especially ML models and databases, temporarily use **more CPU than initially allocated**.
    - **Why I Thought of Including It**: Helps in identifying **CPU-intensive workloads** and ensuring resources scale accordingly.
    - **Data Type Used & Why**: `DECIMAL(5,2)` since CPU allocations can be fractional (e.g., **1.5 cores** or **75.25% usage**).
- **`max_allocated_disk_space` (INTEGER, GB)**
    - **Purpose**: Tracks the highest disk space allocated to an application.
    - **How I Thought of Including It**: Some applications require **temporary storage bursts** (e.g., caching, database transactions).
    - **Why I Thought of Including It**: Helps in setting **storage expansion policies** and ensuring **no unexpected storage failures**.
    - **Data Type Used & Why**: `INTEGER` (GB) since disk allocation is measured in whole numbers.
- **`actual_memory_usage` (INTEGER, MB or GB)**
    - **Purpose**: Tracks the real-time memory usage of an application.
    - **How I Thought of Including It**: Allocated memory doesn’t always reflect actual usage, leading to **overprovisioning or underutilization**.
    - **Why I Thought of Including It**: Helps in **cost optimization** by identifying **underutilized resources** that can be reallocated.
    - **Data Type Used & Why**: `INTEGER` (MB or GB) because memory consumption is tracked in whole units.
- **`actual_cpu_usage` (DECIMAL(5,2), Cores or %)**
    - **Purpose**: Stores the **real-time CPU utilization** of an application.
    - **How I Thought of Including It**: Cloud providers often **bill based on actual usage**, so tracking this helps in cost analysis.
    - **Why I Thought of Including It**: Ensures applications get the **right amount of CPU** while avoiding **wasteful over-allocation**.
    - **Data Type Used & Why**: `DECIMAL(5,2)` for precision, since CPU usage can be fractional.
- **`actual_disk_usage` (INTEGER, GB)**
    - **Purpose**: Stores **current disk space usage** by an application.
    - **How I Thought of Including It**: Some apps use **temporary files**, which don’t need long-term storage allocation.
    - **Why I Thought of Including It**: Helps in preventing **unexpected disk overflows** and **reducing unnecessary storage costs**.
    - **Data Type Used & Why**: `INTEGER` (GB), as storage consumption is measured in whole numbers.
- **`cost_per_hour` (DECIMAL(10,4))**
    - **Purpose**: Tracks the **per-hour cost** of resource allocation for an application.
    - **How I Thought of Including It**: Cloud providers charge based on **CPU, memory, and disk allocation**, so tracking this is key.
    - **Why I Thought of Including It**: Helps in **budgeting, forecasting, and cost optimization** for cloud-based workloads.
    - **Data Type Used & Why**: `DECIMAL(10,4)` for **high precision** (e.g., `$0.0523 per hour`).
- **`allocation_status` (ENUM: `active`, `pending`, `deallocated`)**
    - **Purpose**: Tracks whether a resource allocation is currently **active**, awaiting approval (`pending`), or **deallocated**.
    - **How I Thought of Including It**: Some resources are **provisioned but not yet used**, while others may be **deallocated but still recorded**.
    - **Why I Thought of Including It**: Helps in **tracking resource lifecycle**, making it easier to **audit and clean up unused allocations**.
    - **Data Type Used & Why**: `ENUM` for efficiency, since the **values are predefined** and don’t need excessive storage.

- **Example of Stored Data**   
    
    | server_id | app_id | workload_type | allocated_memory | allocated_cpu | allocated_disk_space | resource_tag |
    | --- | --- | --- | --- | --- | --- | --- |
    | `s1a2b3` | `a9x8y7` | "Web Server" | 8192 MB | 2.00 Cores | 100 GB | "Finance Team" |
    | `s4c5d6` | `b3k4m5` | "ML Training" | 32768 MB | 8.50 Cores | 500 GB | "AI Research" |
  
## Dive Into Details
- [**How It Interacts with Other Tables**](How%20It%20Interacts%20with%20Other%20Tables.md)
- [**What Queries Would Be Used? (Expanded & Optimized)**](What%20Queries%20Would%20Be%20Used%20(Expanded%20&%20Optimized).md)
- [**Alternative Approaches (Expanded & Compared)**](Alternative%20Approaches%20(Expanded%20&%20Compared).md)
- [**Real-World Use Cases (Expanded & Detailed)**](Real-World%20Use%20Cases%20(Expanded%20&%20Detailed).md)
- [**Performance Considerations & Scalability (Expanded & Detailed)**](Performance%20Considerations%20&%20Scalability%20(Expanded.md))
- [**Query Optimization Techniques (Expanded & Detailed)**](Query%20Optimization%20Techniques%20(Expanded%20&%20Detailed).md)
- [**Handling Large-Scale Data**](Handling%20Large-Scale%20Data.md)
- [**Data Retention & Cleanup (Expanded & Detailed)**](Data%20Retention%20&%20Cleanup%20(Expanded%20&%20Detailed).md)
- [**Security & Compliance**](Security%20&%20Compliance.md)
- [**Alerting & Automation**](Alerting%20&%20Automation.md)
- [**How You Tested & Validated Data Integrity**](How%20You%20Tested%20&%20Validated%20Data%20Integrity.md)
- [**Thought Process Behind Decisions**](Thought%20Process%20Behind%20Decisions.md)
