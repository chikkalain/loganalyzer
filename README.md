# loganalyzer
The system collects technical artifacts such as logs, dumps, and reports from multiple sources, then analyzes them using both traditional tools and advanced AI. Results are stored for efficient search and retrieval.

Here’s a breakdown of the flow:

1. Data Ingestion (Web Client): Users or systems upload technical artifacts from three primary sources:

 * Direct upload from a Desktop.
 * Integrated upload from Salesforce (attached to support cases).
 * Integrated upload from Jira (attached to bug reports).
 
2. Apache Web Server: All uploaded artifacts are routed to an Apache Web Server, which serves as the central entry point and manages incoming data traffic.

3. Dual-Path Analysis Pipeline: The data is then processed through two parallel analysis paths:

Path A: Traditional Specialized Analysis (The Top Route)
 * File Type Detection: The system identifies the uploaded file type (e.g., Product log file, Heap Dump, GC Log).
 * Open-Source Tool Routing: Based on the file type, the artifact is sent to an open-source tool designed for analysis, such as:
 * Heap Dumps to eclipse-mat.
 * Thread Dumps to a thread-stack-analyzer.
 * TCP dumps to Wireshark.

 Path B: The AI “Deep Dive” (The Bottom Route)
 * Artifacts are also analyzed by Llama 3.1, a large language model that performs advanced semantic analysis to identify complex patterns, anomalies, or root causes that traditional tools may overlook.
 
4. Aggregation, Storage, and Search: Analytical results from both specialized tools and the Llama 3.1 AI model are consolidated:

This hybrid approach reduces Mean Time to Resolution (MTTR) and enables more proactive support. Both the application and Llama 3.1 are hosted on-premises, ensuring that no customer logs or data are shared with cloud servers.

<img width="1376" height="768" alt="Support Diagnostics with a Hybrid AI   Open-Source Architecture" src="https://github.com/user-attachments/assets/7216539a-45d0-43b7-af47-75ecb0f4c760" />


<img width="1484" height="682" alt="Screenshot 2026-01-06 201728" src="https://github.com/user-attachments/assets/b7f97a74-679f-4145-a2bf-72f782442ae6" />


<img width="1376" height="768" alt="Pic1" src="https://github.com/user-attachments/assets/1a616192-0408-433a-a9a9-65d070d3c2d4" />


#SRE hashtag#ArtificialIntelligence hashtag#Llama3 hashtag#SupportEngineering hashtag#Architecture hashtag#LogAnalysis hashtag#OpenSearch hashtag#DevOps

