# AI-Research-Agent
An autonomous research assistant built on AWS Bedrock, Lambda, and S3. It generates structured Markdown briefs on any topic using Claude 3.5 Sonnet and optional Serper web enrichment.

#  AI Research Agent — AWS AI Agent Global Hackathon

## Overview
**AI Research Agent** is a serverless autonomous AI system built on **Amazon Bedrock**, **Lambda**, and **S3**.  
It generates structured research briefs on any topic, with sections such as summary, key findings, risks, and sources.  
Simply send a topic (e.g., “AI in healthcare”), and the agent automatically:

1. Uses **Claude 3.5 Sonnet (via Bedrock)** to reason and write a Markdown report  
2. Optionally enriches content using **Serper** search results  
3. Saves the report in **Amazon S3**  
4. Returns a presigned URL for viewing and sharing the result instantly  

It’s a fully autonomous knowledge miner — a practical demonstration of AWS’s generative AI stack powering reasoning agents.

---



How It Works
1. API Gateway
Public endpoint for POST requests.

Forwards the payload (e.g. {"topic":"AI in healthcare"}) to the Lambda function.

2. Lambda (Python)
Core logic lives in lambda_function.py.

Extracts the topic → optionally calls Serper for snippets → builds a structured prompt → sends to Claude 3.5 Sonnet via Bedrock → saves output to S3.

3. Amazon Bedrock
Hosts the Claude 3.5 Sonnet reasoning model.

Responds with a multi-section research summary.

4. Amazon S3
Stores Markdown reports in /reports/ folder.

Filenames include timestamps so nothing is overwritten.

Returns a presigned URL valid for 1 hour for quick viewing.

5.Serper (Search)
Adds fresh, real-world snippets from Google to enrich Claude’s knowledge base.


Setup Summary

Environment Variables

Key	Description
BEDROCK_REGION	AWS region (usually us-east-1)
S3_BUCKET	Your S3 bucket name
SERPER_API_KEY	Optional Serper.dev API key

AWS Services Used

Lambda (Python 3.12)

API Gateway

Amazon Bedrock (Claude 3.5 Sonnet)

Amazon S3

CloudWatch (for logs)

Example Request
curl -X POST "https://zhccijflxd.execute-api.us-east-1.amazonaws.com/default/research-agent" \
  -H "Content-Type: application/json" \
  -d "{\"topic\": \"AI in healthcare\"}"

Example Response:
{
  "topic": "AI in healthcare",
  "s3_key": "reports/ai-in-healthcare-20251022-235900.md",
  "presigned_url_1h": "https://...amazonaws.com/reports/ai-in-healthcare-20251022-235900.md?...",
  "preview": "AI in healthcare is transforming the medical field..."
}

Output Example

The S3 file contains structured Markdown like:

# AI in Healthcare

## Summary
Artificial Intelligence (AI) is revolutionizing healthcare by...

## Key Findings
- Improved diagnostics through medical imaging
- AI-driven personalized medicine

## Opportunities & Risks
- + Better patient outcomes
- – Data privacy and bias issues

## Implementation Notes
Use AWS Bedrock agents for scalable medical knowledge pipelines.

## Sources
- [WHO: AI in Health Report](https://www.who.int/)
- [NIH: AI in Medical Research](https://www.nih.gov/)



