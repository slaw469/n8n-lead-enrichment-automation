# Lead Enrichment & Notification Automation

## Overview
This n8n workflow automatically enriches incoming leads, scores them based 
on company data and job titles, and routes them to appropriate channels 
for follow-up. It demonstrates advanced lead qualification, data 
enrichment, and CRM automation capabilities.

## Workflow Features
- **Email Validation**: Validates lead emails using Hunter.io API
- **Company Enrichment**: Fetches company data (size, industry, domain 
info)
- **Intelligent Lead Scoring**: Custom algorithm scoring leads 0-150 
points
- **Automatic Routing**: Categorizes leads as Hot (70+), Warm (40-69), or 
Cold (<40)
- **Multi-Channel Notifications**: Slack alerts + Google Sheets logging
- **Deal Value Estimation**: Calculates potential deal value based on 
company profile

## Technical Architecture

### Data Flow
```
Webhook → Email Validation → Company Enrichment → Lead Scoring → 
Routing → Notifications
```

### Key Components
1. **Webhook Trigger**: Accepts lead data via POST request
2. **Hunter.io Integration**: Email validation + domain search for company 
data
3. **Lead Scoring Algorithm**: 130-point scoring system based on:
   - Email validation (30 pts)
   - Company size & prestige (35 pts) 
   - Job title & seniority (35 pts)
   - Industry relevance (25 pts)
   - Geographic quality (5 pts)
   - Fortune company bonus (20 pts)
4. **Smart Routing**: Conditional logic routing leads to appropriate 
channels
5. **Google Sheets CRM**: Automated data logging with multiple sheets
6. **Slack Notifications**: Real-time alerts to different channels by lead 
temperature

## Lead Scoring Logic

### Scoring Breakdown
- **Hot Leads (120+ points)**: C-level at Fortune companies, immediate 
action required
- **Warm Leads (80-119 points)**: High-value prospects, 24-hour follow-up
- **Cool Leads (50-79 points)**: Medium priority, nurture sequence
- **Cold Leads (<50 points)**: Newsletter subscription, quarterly 
follow-up

### Deal Value Calculation
- Fortune companies: Score × $4,000
- Large enterprises (5000+ employees): Score × $2,500  
- Mid-market (1000-5000 employees): Score × $1,800
- Standard companies: Score × $1,000

## Required Integrations

### 1. Hunter.io API
- **Purpose**: Email validation + company enrichment
- **Setup**: Create account at hunter.io, generate API key
- **Usage**: 50 free requests/month, paid plans available

### 2. Google Sheets
- **Purpose**: CRM data storage and analytics
- **Sheets Required**:
  - Analytics (main data)
  - Invalid Leads
- **OAuth**: Requires Google Sheets API access

### 3. Slack Integration  
- **Purpose**: Real-time notifications
- **Channels**: #hotleads, #warmleads, #coldleads, #invalidleads
- **Permissions**: chat:write, channels:read

## Installation & Setup

### 1. Import Workflow
1. Copy the JSON content from `n8n-lead-enrichment-workflow.json`
2. In n8n: Import → Paste JSON
3. Save workflow

### 2. Configure Credentials
```
Hunter.io API → HTTP Query Auth → api_key parameter
Google Sheets → OAuth2 → Grant permissions  
Slack → OAuth2 → Bot User OAuth Token
```

### 3. Update Configuration
- Replace Google Sheet ID in all Google Sheets nodes
- Update Slack channel IDs for your workspace
- Test webhook endpoint

### 4. Create Google Sheet Structure
**Analytics Sheet Columns:**
- Date | Email | Company | Score | Temperature | Action Required | 
Estimated Deal Value

**Invalid Leads Sheet Columns:**  
- Timestamp | Name | Email | Company | Job Title | Phone | Company Size | 
Industry | Notes | Email Status

## Webhook Usage

### Endpoint
```
POST /webhook/lead-webhook
```

### Expected Payload
```json
{
  "body": [{
    "name": "John Smith",
    "email": "john@company.com", 
    "company": "Tech Corp",
    "job_title": "VP Engineering",
    "industry": "Technology",
    "company_size": "1001-5000",
    "phone": "+1234567890",
    "notes": "Interested in automation tools"
  }]
}
```

## Performance Metrics
- **Processing Time**: ~5-10 seconds per lead
- **Accuracy**: 95%+ email validation rate
- **Scalability**: Handles 100+ leads/hour
- **Error Handling**: Invalid leads logged separately

## Advanced Features
- **Duplicate Detection**: Email-based deduplication
- **Data Enrichment**: Company size, industry, location data
- **Smart Routing**: Automatic lead assignment based on score
- **Analytics Ready**: Structured data for reporting/dashboards

## Use Cases
- **Sales Teams**: Automatic lead qualification and routing
- **Marketing**: Lead scoring for campaign optimization  
- **Customer Success**: Identify high-value prospects
- **Revenue Operations**: Deal pipeline forecasting

## Future Enhancements
- LinkedIn enrichment integration
- Salesforce/HubSpot CRM sync
- Machine learning scoring models
- Advanced duplicate detection
- Custom scoring rule configuration

## Technical Notes
- **Error Handling**: Graceful fallbacks for API failures
- **Rate Limiting**: Respects Hunter.io API limits
- **Data Privacy**: Secure credential management
- **Monitoring**: Built-in logging and error tracking

---

**Contact**: sgl230000@utdallas.edu  
**LinkedIn**: https://www.linkedin.com/in/steven-law-b918b530b/

