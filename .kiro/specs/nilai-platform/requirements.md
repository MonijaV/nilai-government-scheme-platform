# Requirements Document: Nilai Platform

## Introduction

Nilai is an AI-powered government scheme discovery platform that democratizes access to 1000+ Indian government welfare schemes through intelligent discovery, personalized matching, and guided application support. The platform addresses the critical problem where 60-70% of eligible citizens never access schemes due to awareness gaps, complexity barriers, and language/literacy challenges.

This requirements document focuses on the Phase 1 MVP scope (Months 1-3), targeting 10,000 users across Tamil Nadu with 20-30 schemes, while establishing the foundation for future nationwide expansion.

## Glossary

- **Nilai_Platform**: The complete AI-powered government scheme discovery system
- **Scheme_Discovery_Engine**: The AI-powered component that matches users to relevant government schemes
- **Eligibility_Checker**: The component that verifies user eligibility against scheme criteria
- **Application_Assistant**: The component that guides users through scheme application processes
- **Bedrock_Service**: Amazon Bedrock AI service using Claude 3.5 Sonnet model
- **Scheme_Database**: DynamoDB table storing government scheme information
- **User_Profile**: DynamoDB record containing user demographic and document data
- **Application_Record**: DynamoDB record tracking scheme application status
- **Intent_Extractor**: AI component that interprets natural language queries
- **Eligibility_Criteria**: Set of conditions (age, income, location, etc.) required for scheme qualification
- **Document_Store**: S3 bucket storing encrypted user documents
- **API_Gateway**: RESTful API endpoints for frontend-backend communication
- **PWA**: Progressive Web Application (React.js frontend)
- **NLU**: Natural Language Understanding
- **OCR**: Optical Character Recognition (not in Phase 1)
- **Code_Mixing**: Use of multiple languages in a single query (e.g., Tamil-English)

## Requirements

### Requirement 1: Intelligent Scheme Discovery

**User Story:** As a citizen, I want to ask questions about government schemes in my native language, so that I can discover schemes I qualify for without knowing their exact names or categories.

#### Acceptance Criteria

1. WHEN a user submits a natural language query in Tamil, Hindi, or English, THE Scheme_Discovery_Engine SHALL extract the user's intent and context
2. WHEN the Intent_Extractor processes a query, THE Scheme_Discovery_Engine SHALL identify relevant demographic factors (age, occupation, location, income level, disability status)
3. WHEN intent is extracted, THE Scheme_Discovery_Engine SHALL query the Scheme_Database for matching schemes
4. WHEN multiple schemes match, THE Scheme_Discovery_Engine SHALL rank results by relevance score based on user profile and query context
5. WHEN displaying results, THE Scheme_Discovery_Engine SHALL present scheme name, brief description, key benefits, and eligibility summary in the user's query language
6. WHEN a user asks a follow-up question, THE Scheme_Discovery_Engine SHALL maintain conversation context for coherent multi-turn dialogue
7. WHEN processing code-mixed queries (Tamil-English or Hindi-English), THE Scheme_Discovery_Engine SHALL correctly interpret intent across language boundaries
8. WHEN the Bedrock_Service responds, THE Scheme_Discovery_Engine SHALL complete processing within 30 seconds
9. IF the Bedrock_Service fails or times out, THEN THE Scheme_Discovery_Engine SHALL return a graceful error message and log the failure

### Requirement 2: Smart Eligibility Verification

**User Story:** As a citizen, I want the system to automatically check if I qualify for schemes, so that I don't waste time on schemes I'm ineligible for.

#### Acceptance Criteria

1. WHEN a user selects a scheme, THE Eligibility_Checker SHALL retrieve all Eligibility_Criteria for that scheme
2. WHEN checking eligibility, THE Eligibility_Checker SHALL evaluate all criteria simultaneously (age AND income AND location AND category)
3. WHEN user data is incomplete, THE Eligibility_Checker SHALL identify missing information and prompt the user to provide it
4. WHEN all criteria are evaluated, THE Eligibility_Checker SHALL return a clear eligibility decision (Eligible, Not Eligible, or Partially Eligible)
5. WHEN returning a decision, THE Eligibility_Checker SHALL provide an explanation of which criteria were met and which were not met
6. WHEN a user is ineligible, THE Eligibility_Checker SHALL suggest alternative schemes the user may qualify for
7. WHEN eligibility depends on document verification, THE Eligibility_Checker SHALL request specific documents from the user
8. WHEN the Bedrock_Service performs reasoning, THE Eligibility_Checker SHALL achieve greater than 95% factual correctness
9. IF eligibility criteria conflict or are ambiguous, THEN THE Eligibility_Checker SHALL flag the scheme for manual review and notify the user

### Requirement 3: Guided Application Assistant

**User Story:** As a citizen, I want step-by-step guidance to complete scheme applications, so that I can successfully apply without confusion or errors.

#### Acceptance Criteria

1. WHEN a user starts an application, THE Application_Assistant SHALL generate a personalized checklist of required documents
2. WHEN displaying application steps, THE Application_Assistant SHALL present them in sequential order with clear instructions in the user's language
3. WHEN a user uploads a document, THE Application_Assistant SHALL validate file format, size, and completeness
4. WHEN form fields can be auto-filled from User_Profile, THE Application_Assistant SHALL pre-populate those fields
5. WHEN a user enters form data, THE Application_Assistant SHALL validate inputs in real-time and display error messages for invalid entries
6. WHEN an application is submitted, THE Application_Assistant SHALL create an Application_Record with status "Submitted"
7. WHEN an application status changes, THE Application_Assistant SHALL update the Application_Record and display the new status to the user
8. WHEN a user views their applications, THE Application_Assistant SHALL display all Application_Records with current status and submission date
9. IF document upload fails, THEN THE Application_Assistant SHALL retry up to 3 times before displaying an error message

### Requirement 4: Multilingual Support

**User Story:** As a citizen who speaks Tamil, Hindi, or English, I want the entire platform experience in my preferred language, so that I can use the platform comfortably.

#### Acceptance Criteria

1. WHEN a user first accesses the platform, THE Nilai_Platform SHALL detect browser language and set default language to Tamil, Hindi, or English
2. WHEN a user changes language preference, THE Nilai_Platform SHALL persist the selection and apply it to all UI elements
3. WHEN displaying static UI elements, THE Nilai_Platform SHALL render text in the selected language using i18n translation files
4. WHEN the Bedrock_Service generates dynamic content, THE Nilai_Platform SHALL request responses in the user's selected language
5. WHEN displaying scheme information, THE Nilai_Platform SHALL show scheme names, descriptions, and eligibility criteria in the selected language
6. WHEN error messages are displayed, THE Nilai_Platform SHALL present them in the user's selected language
7. WHEN form labels and validation messages are shown, THE Nilai_Platform SHALL render them in the selected language
8. WHEN processing code-mixed input, THE Bedrock_Service SHALL understand and respond appropriately regardless of language mixing

### Requirement 5: User Profile Management

**User Story:** As a citizen, I want to create and manage my profile with demographic and document information, so that the system can accurately match me to schemes.

#### Acceptance Criteria

1. WHEN a new user registers, THE Nilai_Platform SHALL create a User_Profile with unique user ID
2. WHEN collecting profile data, THE Nilai_Platform SHALL request only essential information (name, age, location, occupation, income range)
3. WHEN a user uploads documents, THE Nilai_Platform SHALL encrypt files using AES-256 and store them in the Document_Store
4. WHEN storing User_Profile data, THE Nilai_Platform SHALL encrypt sensitive fields in the Scheme_Database
5. WHEN a user updates profile information, THE Nilai_Platform SHALL validate changes and update the User_Profile
6. WHEN a user views their profile, THE Nilai_Platform SHALL display all stored information with options to edit or delete
7. WHEN a user requests data deletion, THE Nilai_Platform SHALL remove all User_Profile data and associated documents within 30 days
8. WHEN collecting data, THE Nilai_Platform SHALL obtain explicit user consent before storing any information
9. IF a user attempts to access the platform without authentication, THEN THE Nilai_Platform SHALL redirect to the login page

### Requirement 6: Scheme Database Management

**User Story:** As a platform administrator, I want to manage the scheme database with accurate and up-to-date information, so that users receive correct scheme details.

#### Acceptance Criteria

1. WHEN a new scheme is added, THE Scheme_Database SHALL store scheme name, description, eligibility criteria, benefits, application process, and official links
2. WHEN scheme information is updated, THE Scheme_Database SHALL maintain version history with timestamps
3. WHEN storing eligibility criteria, THE Scheme_Database SHALL structure them as machine-readable rules (age range, income threshold, location codes, category tags)
4. WHEN a scheme expires or is discontinued, THE Scheme_Database SHALL mark it as inactive and exclude it from search results
5. WHEN schemes are queried, THE Scheme_Database SHALL return only active schemes unless specifically requested otherwise
6. WHEN scheme data is retrieved, THE Scheme_Database SHALL include all multilingual content (Tamil, Hindi, English)
7. WHEN the Scheme_Discovery_Engine queries schemes, THE Scheme_Database SHALL support filtering by multiple criteria simultaneously
8. WHEN scheme metadata is stored, THE Scheme_Database SHALL include tags for category (agriculture, education, health, etc.) and target beneficiary (farmer, student, woman, senior citizen, etc.)

### Requirement 7: Document Management and Security

**User Story:** As a citizen, I want my personal documents and data to be securely stored and encrypted, so that my privacy is protected.

#### Acceptance Criteria

1. WHEN a user uploads a document, THE Document_Store SHALL encrypt the file using AES-256 before storage
2. WHEN documents are transmitted, THE Nilai_Platform SHALL use TLS encryption for all data in transit
3. WHEN storing documents in S3, THE Document_Store SHALL organize files by user ID and document type
4. WHEN a document is accessed, THE Document_Store SHALL verify user authentication and authorization before retrieval
5. WHEN documents are retrieved, THE Document_Store SHALL decrypt files only for authorized requests
6. WHEN a user deletes a document, THE Document_Store SHALL permanently remove the file and all references
7. WHEN storing data in DynamoDB, THE Nilai_Platform SHALL encrypt sensitive fields (Aadhar number, income, phone number)
8. WHEN data is stored, THE Nilai_Platform SHALL ensure all storage occurs in AWS Mumbai region for data localization compliance
9. IF unauthorized access is attempted, THEN THE Nilai_Platform SHALL log the attempt and deny access

### Requirement 8: AI Response Quality and Safety

**User Story:** As a citizen, I want accurate and trustworthy AI responses, so that I can make informed decisions about government schemes.

#### Acceptance Criteria

1. WHEN the Bedrock_Service generates responses, THE Scheme_Discovery_Engine SHALL achieve greater than 90% intent recognition accuracy
2. WHEN providing scheme information, THE Bedrock_Service SHALL achieve greater than 95% factual correctness with no hallucinations
3. WHEN the AI is uncertain, THE Bedrock_Service SHALL acknowledge uncertainty and suggest contacting support rather than providing incorrect information
4. WHEN generating eligibility explanations, THE Bedrock_Service SHALL cite specific criteria from the Scheme_Database
5. WHEN responding to queries, THE Bedrock_Service SHALL avoid making promises or guarantees about scheme approval
6. WHEN detecting potentially sensitive topics, THE Bedrock_Service SHALL respond with empathy and appropriate guidance
7. WHEN users ask about schemes outside the database, THE Bedrock_Service SHALL clearly state that information is not available and suggest alternative resources
8. WHEN generating responses, THE Bedrock_Service SHALL complete processing within 30 seconds for 95% of requests

### Requirement 9: Performance and Reliability

**User Story:** As a citizen, I want the platform to respond quickly and reliably, so that I can efficiently discover and apply for schemes.

#### Acceptance Criteria

1. WHEN a user submits a query, THE Nilai_Platform SHALL return results within 30 seconds for 95% of requests
2. WHEN the platform experiences high traffic, THE API_Gateway SHALL scale Lambda functions automatically to maintain performance
3. WHEN API requests are made, THE API_Gateway SHALL respond with appropriate HTTP status codes and error messages
4. WHEN system errors occur, THE Nilai_Platform SHALL log errors to CloudWatch with sufficient detail for debugging
5. WHEN the platform is operational, THE Nilai_Platform SHALL maintain greater than 99% uptime during business hours (6 AM - 10 PM IST)
6. WHEN database queries are executed, THE Scheme_Database SHALL return results within 1 second for 99% of queries
7. WHEN documents are uploaded, THE Document_Store SHALL complete uploads within 10 seconds for files up to 5 MB
8. WHEN the Bedrock_Service is unavailable, THE Nilai_Platform SHALL display a maintenance message and queue requests for retry
9. IF technical errors exceed 1% of total requests, THEN THE Nilai_Platform SHALL trigger alerts to the operations team

### Requirement 10: Community Impact Dashboard

**User Story:** As a citizen or administrator, I want to see the collective impact of the platform, so that I can understand how the community is benefiting from government schemes.

#### Acceptance Criteria

1. WHEN a user accesses the dashboard, THE Nilai_Platform SHALL display total benefits unlocked (in rupees) at village, district, and state levels
2. WHEN displaying statistics, THE Nilai_Platform SHALL show the number of successful applications by scheme category
3. WHEN presenting impact data, THE Nilai_Platform SHALL visualize the top 10 most popular schemes with application counts
4. WHEN calculating success rates, THE Nilai_Platform SHALL display the percentage of applications that resulted in scheme approval
5. WHEN showing geographic data, THE Nilai_Platform SHALL present impact metrics on a map visualization by district
6. WHEN aggregating data, THE Nilai_Platform SHALL update dashboard metrics daily at midnight IST
7. WHEN displaying user-specific data, THE Nilai_Platform SHALL show only anonymized and aggregated statistics to protect privacy
8. WHEN filtering dashboard data, THE Nilai_Platform SHALL allow users to select time ranges (last 7 days, 30 days, 90 days, all time)

### Requirement 11: Application Tracking and Notifications

**User Story:** As a citizen, I want to track my application status and receive updates, so that I know the progress of my scheme applications.

#### Acceptance Criteria

1. WHEN an application is submitted, THE Application_Assistant SHALL assign a unique application ID and display it to the user
2. WHEN a user views application status, THE Nilai_Platform SHALL display current status (Submitted, Under Review, Approved, Rejected, Pending Documents)
3. WHEN application status changes, THE Application_Record SHALL update the status field and timestamp
4. WHEN displaying application history, THE Nilai_Platform SHALL show all status changes with dates in chronological order
5. WHEN an application requires additional documents, THE Nilai_Platform SHALL display a clear message indicating which documents are needed
6. WHEN a user has multiple applications, THE Nilai_Platform SHALL display them in a list sorted by submission date (most recent first)
7. WHEN filtering applications, THE Nilai_Platform SHALL allow users to view applications by status (All, Active, Completed)
8. WHEN an application is approved or rejected, THE Nilai_Platform SHALL display the final decision with explanation

### Requirement 12: Progressive Web Application (PWA) Features

**User Story:** As a citizen with limited internet connectivity, I want to access the platform offline and install it on my device, so that I can use it even with poor network conditions.

#### Acceptance Criteria

1. WHEN a user visits the platform, THE PWA SHALL be installable on mobile devices and desktops
2. WHEN the PWA is installed, THE Nilai_Platform SHALL display an app icon on the user's home screen
3. WHEN offline, THE PWA SHALL cache essential UI components and display previously viewed content
4. WHEN network connectivity is restored, THE PWA SHALL sync any pending actions (form submissions, document uploads)
5. WHEN loading pages, THE PWA SHALL display a loading indicator for network requests
6. WHEN the user is offline, THE PWA SHALL display a clear message indicating limited functionality
7. WHEN caching data, THE PWA SHALL store up to 50 MB of content locally
8. WHEN the PWA is updated, THE Nilai_Platform SHALL prompt users to refresh for the latest version

### Requirement 13: API Design and Integration

**User Story:** As a developer, I want well-designed RESTful APIs, so that the frontend can reliably communicate with backend services.

#### Acceptance Criteria

1. WHEN the frontend makes API requests, THE API_Gateway SHALL authenticate requests using JWT tokens
2. WHEN API endpoints are called, THE API_Gateway SHALL validate request parameters and return 400 Bad Request for invalid inputs
3. WHEN Lambda functions process requests, THE API_Gateway SHALL return appropriate HTTP status codes (200, 201, 400, 401, 403, 404, 500)
4. WHEN errors occur, THE API_Gateway SHALL return structured error responses with error codes and messages
5. WHEN API responses are returned, THE API_Gateway SHALL include CORS headers for cross-origin requests
6. WHEN rate limiting is applied, THE API_Gateway SHALL limit requests to 100 per minute per user
7. WHEN API endpoints are versioned, THE API_Gateway SHALL support version prefixes (e.g., /v1/schemes)
8. WHEN requests exceed rate limits, THE API_Gateway SHALL return 429 Too Many Requests with retry-after header

### Requirement 14: Monitoring and Observability

**User Story:** As a platform administrator, I want comprehensive monitoring and logging, so that I can identify and resolve issues quickly.

#### Acceptance Criteria

1. WHEN Lambda functions execute, THE Nilai_Platform SHALL log all invocations to CloudWatch with execution time and status
2. WHEN errors occur, THE Nilai_Platform SHALL log error details including stack traces and request context
3. WHEN API requests are made, THE API_Gateway SHALL log request/response details with timestamps
4. WHEN monitoring metrics, THE Nilai_Platform SHALL track API latency, error rates, and throughput
5. WHEN Bedrock_Service calls are made, THE Nilai_Platform SHALL log token usage and response times
6. WHEN database operations execute, THE Nilai_Platform SHALL log query performance and throttling events
7. WHEN critical errors occur, THE Nilai_Platform SHALL trigger CloudWatch alarms for immediate attention
8. WHEN analyzing logs, THE Nilai_Platform SHALL retain logs for 90 days for compliance and debugging

### Requirement 15: Compliance and Legal Requirements

**User Story:** As a platform operator, I want to comply with Indian data protection and digital laws, so that the platform operates legally and ethically.

#### Acceptance Criteria

1. WHEN collecting user data, THE Nilai_Platform SHALL comply with IT Act 2000 and Aadhaar Act 2016 requirements
2. WHEN storing Aadhaar numbers, THE Nilai_Platform SHALL encrypt them and limit access to authorized personnel only
3. WHEN users register, THE Nilai_Platform SHALL display a privacy policy and terms of service
4. WHEN obtaining consent, THE Nilai_Platform SHALL require explicit opt-in for data collection and processing
5. WHEN users request data access, THE Nilai_Platform SHALL provide all stored personal data within 30 days
6. WHEN users request data deletion, THE Nilai_Platform SHALL delete all personal data within 30 days
7. WHEN data is processed, THE Nilai_Platform SHALL ensure all data storage and processing occurs within India (AWS Mumbai region)
8. WHEN third-party services are used, THE Nilai_Platform SHALL ensure they comply with Indian data localization requirements

## Phase 1 MVP Success Criteria

The Phase 1 MVP (Months 1-3) SHALL be considered successful when:

1. The platform has 10,000 registered users
2. Users have submitted 5,000 applications through the platform
3. The platform achieves a Net Promoter Score (NPS) greater than 60
4. Total benefits unlocked reach ₹2 crore (20 million rupees)
5. Average response time is less than 30 seconds for 95% of requests
6. Technical error rate is less than 1% of total requests
7. AI intent recognition accuracy exceeds 90%
8. AI factual correctness exceeds 95%
9. Application success rate (approved applications / total applications) exceeds 70%
10. The platform supports 20-30 schemes (Tamil Nadu + Central schemes)

## Future Phase Considerations

While not part of Phase 1 MVP requirements, the design SHALL accommodate future enhancements:

- **Phase 2 (Months 4-9)**: Amazon Textract for OCR, Amazon Translate API for dynamic translation, SNS for notifications, WhatsApp bot integration, expansion to 200+ schemes across 5 states
- **Phase 3 (Months 10-18)**: Voice IVR system, Amazon Polly for text-to-speech, expansion to 1000+ schemes nationwide, mobile app (iOS/Android)

The architecture and data models SHALL be designed to support these future capabilities without requiring major refactoring.
