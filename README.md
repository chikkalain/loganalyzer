# RADAR — AI-Driven Case Triage & Artifact Intelligence

When a customer hits a critical issue, every minute counts. RADAR was built to cut through the noise — automatically ingesting technical artifacts, running them through specialized analysis tools, and layering on AI to surface root causes that traditional tooling often misses.

The result: faster resolutions, less manual digging, and support engineers who can focus on fixing problems rather than chasing them.

---

## How It Works

### 1. Artifact Ingestion

Engineers can submit artifacts from three places:

- **Desktop upload** — drag and drop directly through the web client
- **Salesforce** — files attached to an existing support case are pulled in automatically
- **Jira** — artifacts linked to bug reports flow in without any extra steps

All uploads pass through an Apache web server that acts as the central intake point.

---

### 2. Dual-Path Analysis

Once an artifact arrives, RADAR runs it through two parallel pipelines simultaneously.

**Path A — Specialized Tool Analysis**

RADAR first identifies what kind of file it's dealing with, then routes it to the right tool for the job:

| Artifact Type | Tool | What it helps with |
|---|---|---|
| Product Log File | Generic Parser | Your first stop for any investigation. Product logs capture application errors, warnings, and event sequences that tell the story of what went wrong and when. |
| Heap Dump | Eclipse MAT | When your JVM runs out of memory or slows to a crawl, a heap dump shows exactly what's sitting in memory — leaked objects, oversized allocations, and anything that shouldn't still be there. |
| Thread Dump | Thread Stack Analyzer | Captures every active JVM thread and what it was doing at that moment. Invaluable for spotting deadlocks, stuck threads, and resource contention that's silently killing performance. |
| GC Log | GC-Viewer | Garbage collection logs reveal how hard the JVM is working to reclaim memory — pause durations, collection frequency, heap pressure over time. If your app is randomly sluggish, this is usually where the answer hides. |
| TCP Dump | Wireshark | Raw network traffic captured at the packet level. When two services can't seem to talk to each other reliably, a TCP dump shows you exactly what's being sent, what's being dropped, and where the conversation breaks down. |
| SAR / NMON Report | ksar / nmon tools | System-level health data — CPU, memory, disk I/O, network utilization — recorded over time. Great for connecting the dots between what the OS was doing and what the application experienced during an incident. |
| HAR File | JSON Parser | A full recording of a browser session, including every HTTP request, response, timing, and header. When a page is slow or an API call is silently failing, the HAR file usually tells you exactly which request is the culprit. |

**Path B — AI Deep Dive (Llama 3.1)**

At the same time, the artifact is analyzed by an on-premises instance of **Llama 3.1**. The model reads through logs and reports semantically — looking for patterns, anomalies, and potential root causes that rule-based tools aren't designed to catch. This is especially useful for novel failure modes or issues that span multiple components.

---

### 3. Aggregation & Search

Results from both paths are consolidated and indexed, making them immediately searchable. Whether you want to pull up a specific case, compare similar incidents, or track down a recurring pattern across customers, everything is in one place.

---

## Why It's Built This Way

Most diagnostic tools make you choose — either you use specialized analyzers that are fast but narrow, or you use AI that's flexible but takes time to act on. RADAR runs both in parallel so you get the precision of dedicated tools alongside the contextual understanding of a large language model.

This approach directly reduces **Mean Time to Resolution (MTTR)** and gives support teams the confidence to act sooner.

---

## Privacy & Data Security

Both the RADAR application and the Llama 3.1 model run **entirely on-premises**. No customer logs, crash dumps, or diagnostic data are sent to external cloud services. This makes RADAR suitable for enterprise environments with strict data residency requirements.

---

## Screenshots

### High-Level Architecture
![Architecture Overview](https://github.com/user-attachments/assets/7216539a-45d0-43b7-af47-75ecb0f4c760)

The full system at a glance. Artifacts arrive from Desktop, Salesforce, or Jira, pass through the Apache web server, and are simultaneously routed to specialized open-source tools based on file type and to Llama 3.1 for AI analysis. Results from both paths land in Apache Ignite and get indexed in OpenSearch so they're ready to query instantly.

---

### Data Flow & Security Model
![Data Flow Diagram](https://github.com/user-attachments/assets/b7f97a74-679f-4145-a2bf-72f782442ae6)

A closer look at how data moves through the system and how it stays secure along the way. Every browser-to-server connection runs over HTTPS with SSL/TLS certificates. Authentication is handled via LDAPS on the client side. On the server, uploaded files are staged temporarily, processed, indexed in OpenSearch, and stored in Ignite — with a 90-day retention policy applied automatically. Salesforce and Jira attachments are fetched via API keys, so engineers never have to manually download and re-upload files.

---

### Artifact Upload Interface
<img width="1899" height="930" alt="image" src="https://github.com/user-attachments/assets/b0637151-4d8b-4cc8-9eff-985062fbae17" />

The main upload screen gives engineers three ways to get artifacts into RADAR without friction. The Desktop Upload panel lets you pick a file, tie it to a case number, choose the product and file type, and submit. The Salesforce and Jira panels on the right fetch attachments directly by case number or Jira ID — no downloading required. Everything you need is reachable from the left sidebar without navigating away.

---

### Speed Search
![Speed Search](https://github.com/user-attachments/assets/c2700f04-019b-421d-902c-ebfba5d8278c)

Speed Search lets you query across every connected source — Salesforce cases, Jira tickets, Bitbucket, product docs, and personal uploads — all from one box. You can narrow the scope using the checkboxes on the left if you already know where to look. For deeper investigation, the **AI Search** button hands the query to Llama 3.1, which returns ranked results with AI-generated summaries rather than a flat list of keyword matches.

---

### AI Search in Action
![AI Search](https://github.com/user-attachments/assets/e9582ad5-b39f-4abf-9aba-120d71d3cd53)

Here's a real example of what the AI Search can do. An engineer pasted in a customer case about a SQL Server cursor error on a clustered columnstore index. Llama 3.1 came back with the exact error code, a plain-English explanation of why it happens, and three specific steps to fix it. Previous conversations are saved in the left panel so you can pick up where you left off, and the version selector lets you scope the response to the product version the customer is actually running.

---

## Tech Stack

`Llama 3.1` · `Apache` · `Eclipse MAT` · `Wireshark` · `GC-Viewer` · `ksar / nmon` · `OpenSearch` · `Apache Ignite` · `Salesforce` · `Jira`
