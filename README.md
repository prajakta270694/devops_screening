# devops_screening

As per the scenario provided, the billing data older than 3 months is rarely accessed, yet needs to remain available with a response time of a few seconds. The goal is to reduce storage costs without compromising on data availability, access latency.

I propose an Azure-native and cost-efficient data archiving strategy using the hot and cool tiers of Azure Blob Storage, combined with Azure Data Factory and Log Analytics for data orchestration. :
Architectural diagram of the solution :
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/dceb55f6-9ca9-415d-9d1b-4116957fd3b6" />

Explanation :
1. Data Storage in Azure Cosmos DB - All billing records are initially stored in Azure Cosmos DB. It handles both recent and old records in a single collection.
2. Check Last Accessed Date of Records - Enable diagnostic settings on Cosmos DB and route the logs to Azure Log Analytics. These logs contain data plane operations like ReadDocument, which help identify when a document was last accessed.
3. Classify Data Based on Access Frequency - Use Kusto Query Language (KQL) in Log Analytics to analyze access patterns and separate records accessed within the last 3 months (frequently used) from older ones (rarely used).
4. Move Data Using Azure Data Factory (ADF) using Linked Service and Copy activity. Copy activity will more frequently accessed data to Hot tier and rarely accessed data to Cool tier.

Hot and Cool tier :
Hot tier is used when the data is live and used very frequently. In this, the data storage cost is high but access cost is low.
If we are not using data freqently then, we can move it to the Cool tier, as in the cool tier the storage cost is low and access cost is high.

CDN for Data Availability with Low Latency :
To serve archived data quickly, integrate Azure CDN with Blob Storage. Frequently accessed archived records are cached, ensuring retrieval in seconds without repeated storage reads.

I have used ChatGPT to correct my solution from all the perspective. Given below conversation link for the reference :
https://chatgpt.com/share/688b546c-19f0-8003-91ec-896f33c2247c


