# Nilai - AI-Powered Government Scheme Discovery Platform

> Bridging 800 million Indians to their rightful government schemes through AI

[![AWS Hackathon 2026](https://img.shields.io/badge/AWS%20Hackathon-2026-orange)]()
[![Track 6](https://img.shields.io/badge/Track-Communities%20%26%20Public%20Impact-blue)]()

---

## The Problem

**60-70%** of eligible Indians never access government schemes because:
- They don't know schemes exist
- Complex eligibility criteria
- Language barriers
- Scattered information across multiple websites

**Result**: ₹10+ lakh crore welfare budget underutilized annually

---

## Our Solution

**Nilai** (நிலை - "Status/Rights" in Tamil) uses AI to help citizens discover and apply for government schemes in their native language.

### How It Works

1. **Ask in your language** (Tamil/Hindi/English)
   - "I'm a farmer affected by drought"
   - "Need scholarship for my daughter"

2. **AI finds matching schemes**
   - Understands intent, not just keywords
   - Ranks by relevance and benefit amount

3. **Check eligibility instantly**
   - AI evaluates all criteria
   - Explains why you qualify (or don't)

4. **Apply with guidance**
   - Step-by-step instructions
   - Document checklist
   - Track application status

---

## Why AI, Not Rules?

**User Query**: *"crops died, no rain"*

❌ **Rule-based system**: Searches "crops" + "rain" → Returns irrigation schemes (wrong)

✅ **Nilai AI**: Understands drought context → Returns drought relief schemes (correct)

**AI handles**:
- Natural language understanding
- Multi-factor eligibility (age + income + location + category)
- Ambiguity and context
- Multiple languages and code-mixing

---

## AWS Architecture
```
User (React PWA)
    ↓
API Gateway (REST APIs)
    ↓
Lambda Functions
    ↓
Amazon Bedrock (Claude 3.5) ← AI Engine
    ↓
DynamoDB + S3 (Data Storage)
```

### Services Used

- **Amazon Bedrock**: Natural language understanding, scheme matching
- **AWS Lambda**: Serverless backend logic
- **DynamoDB**: Scheme database, user profiles, applications
- **S3**: Encrypted document storage
- **API Gateway**: RESTful APIs
- **CloudWatch**: Monitoring and logging

---

## Impact Goals

**Phase 1 (3 months)**
- 10,000 users
- 5,000 successful applications
- ₹2 crore benefits unlocked

**Phase 3 (18 months)**
- 10 million users
- ₹1000 crore benefits unlocked
- All India coverage

---

## Repository Contents

- **[requirements.md](./requirements.md)** - Functional requirements (15 features)
- **[design.md](./design.md)** - Technical architecture and data models

---

## Key Features

✅ Conversational scheme discovery in Tamil/Hindi/English  
✅ AI-powered eligibility checking with explanations  
✅ Guided application with auto-fill  
✅ Community impact dashboard  
✅ Offline-capable Progressive Web App  
✅ AES-256 encryption, India data localization  

---

## Example

**Query** (Tamil): *"எனக்கு வேலை இல்லை"* (I don't have a job)

**AI Response**:
1. MGNREGA - ₹7,500/month (guaranteed work)
2. PM-SVANidhi - ₹10,000 loan (self-employment)
3. Skill Training - Free upskilling

**Explanation**: Apply to #1 first for immediate income

---

## Security & Compliance

- Data stored in AWS Mumbai region (India)
- Compliant with IT Act 2000, Aadhaar Act 2016
- All data encrypted at rest and in transit

---

**"Every eligible citizen deserves easy access to government schemes"**
