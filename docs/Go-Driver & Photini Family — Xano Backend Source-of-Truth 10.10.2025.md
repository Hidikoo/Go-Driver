# Go-Driver & Photini Family — Xano Backend Source-of-Truth


**Version:** 3.0 | **Last Updated:** October 10, 2025 | **Status:** Living Document
**Note:** Internal wireframes and architecture diagrams referenced in changelog are not included in public repository.


---

## **Table of Contents** 

### **Document Structure**
1. [Architecture Context](#1-architecture-context)
2. [Update Protocol & Change Management](#2-update-protocol--change-management)
3. [Wireframes and UI/UX Blueprints](#3-wireframes-and-uiux-blueprints)
4. [Compliance Hooks & Blueprint Use](#4-compliance-hooks--blueprint-use)
5. [Database (Full Schema, Fields, Roles, Modules)](#5-database-full-schema-fields-roles-modules)
6. [API Endpoints & Compliance Logic](#6-api-endpoints--compliance-logic) 


### **Quick Reference Tables** 
- [Database Tables Overview](#table-overview)
- [API Groups Summary](#api-groups-overview)
- [Role Permissions Matrix](#role-permissions-matrix)
- [Field Relationships Map](#field-by-field-relationships-overview)

**Related Documentation:**
- [COMPLIANCE_PRINCIPLES.md](./COMPLIANCE_PRINCIPLES.md)
- [TECHNICAL-CONTROLS-v1-design.md](./TECHNICAL-CONTROLS-v1-design.md)
- [Go-Driver-Flashcards-n-QnA.md](./Go-Driver-Flashcards-n-QnA.md)
- [roadmap-vision.md](./roadmap-vision.md)
- [roadmap.md](./roadmap.md)

***

## **Development Status Note**

**Backend:** Production-ready architecture documented (Xano). All tables, roles, and MVP API endpoints defined.

**Frontend/UI:** Development paused as of October/November 2025. Original Chrome Extension UI plans migrated to **photini.family** website + **WeWeb** no-code frontend framework. UI/UX wireframes and interface build will resume once:
- ECtHR case work (priority: child's legal case) concludes
- Financial stability secured (job applications in progress)

**Current Focus:** Nordic discrimination research, legal advocacy, funding applications (Antler Norway, etc.)

**For Developers:** Backend schema and API endpoints are fully documented and ready for frontend integration when development resumes in 2026.

***

---

## 1. Architecture Context

- See included visual wireframes and ASCII drafts for Go-Driver UI/UX and system separation (Manager’s Office, Production Line, Compliance modules, Go-Driver’s Seat, AI-Log controls, etc.).
- Modular, privacy-first collaboration backbone with clear user role boundaries, AI/human oversight. [architecture_vrs_2.jpg, Go-Driver-s-Visual-ASCII-Wireframe_draft.txt, GO-DRIVER-ASCII_Main_draft_9.10.2025.txt]

---

## 2. Update Protocol & Change Management

---

### 2.1 Document Maintenance Guidelines

**When to Update:**
- ✅ New API endpoints added/modified
- ✅ Database schema changes
- ✅ Role/permission updates
- ✅ Function stack modifications
- ✅ Compliance logic changes

**Update Process:**
1. **Version increment:** Update version number and timestamp
2. **Section updates:** Modify affected sections (maintain table format)
3. **Cross-references:** Update related sections and quick reference tables
4. **Changelog:** Document changes in Section 6.2
5. **Validation:** Test API changes match documentation

**Responsible Parties:**
- **Primary:** Backend Developer/API Designer
- **Review:** System Administrator, Compliance Officer
- **Approval:** Project Manager

---

### 2.2 Change Log Template

```markdown
## Version X.X - YYYY-MM-DD
**Added:**
- New API endpoint: [METHOD] /endpoint/{id}
- New database field: table.field_name

**Modified:**
- Updated function stack for endpoint X
- Changed role permission for Y

**Deprecated:**
- Endpoint Z marked for removal in v4.0

**Security/Compliance:**
- Enhanced audit logging for evidence_file
- Added GDPR compliance check for user data
```

---

### 2.3 Change Log

---

#### 2.3.1 Version 3.0 - October 10, 2025

**Major Release: Complete Documentation Foundation**

**Added:**
- Complete database schema (11 tables, 60 records)
- Full role system (20 roles: human + AI service accounts)
- Module architecture (8 modules with feature flags)
- API endpoint documentation framework
- Compliance and security standards
- Update protocol and change management

**Database:**
- All field relationships documented
- Role-based access control matrix
- Privacy by design compliance matrix
- Performance metrics and scaling considerations

**API:**
- User authentication endpoints (3) - Production ready
- RAG co-driver endpoints (5) - Draft status
- Function stack documentation
- Role permission matrix
- Request/response examples

**Documentation:**
- Table of contents with navigation
- Quick reference tables
- Developer guidelines
- Testing checklists

---

## 3. Wireframes and UI/UX Blueprints (IN PROGRESS)

- Includes ASCII and image wireframes for Go-Driver main, modules, compliance and logs, user workflows and AI control.
- Documents Manager’s Office, Production Line, Work in Progress, Go-Driver's Seat, Modular Data Sanctuary, audit flow.

---

### 3.1 Development Roadmap & Scaling Strategy

**Phase 1: MVP (Current)**
- ✅ User authentication system
- ✅ Role-based access control
- ✅ Module architecture foundation
- 🚧 Go-Driver RAG co-driver (Draft)

**Phase 2: Production (Q1 2026)**
- 📋 Case management workflow
- 📋 Evidence handling system
- 📋 Full RAG document processing
- 📋 Admin dashboard

**Phase 3: Scale (Q2-Q3 2026)**
- 📋 Multi-tenant architecture
- 📋 Advanced AI features
- 📋 Integration APIs
- 📋 Mobile applications

**Version 3.1 - [FUTURE DATE]**
Next Major Update: Backend Logic Overhaul

Planned Changes:
- Complete API function stack rebuild
- Enhanced case management for 3000-5000 page cases
- New API integration endpoints
- Chunk API implementation
- Case splitting and linking functionality
- Multi-format evidence processing
- Performance optimizations for large datasets

Database Enhancements:
- Pagination strategies for large case data
- Archive/retention policies
- Performance indexing
- Data chunking for massive documents

API Improvements:
- Standardized error handling
- Enhanced audit logging
- Rate limiting optimization
- Bulk operation endpoints


**🎯 Strategic Planning Notes for Your BE Overhaul:**

High-Priority Considerations for 3000-5000 Page Cases:

1. **Database Architecture:
   - Consider **document chunking in `rag_document` table
   - Pagination strategy for evidence lists
   - Lazy loading for case content
   - Archive tables for old case data

2. API Design:
   - Streaming responses for large case data
   - Batch processing endpoints
   - Progress tracking for long operations
   - Timeout handling for massive queries

3. Performance Planning:
   - Database indexing strategy
   - Caching layer for frequently accessed cases
   - Background processing for AI operations
   - File storage optimization

4. Integration Strategy:
   - Modular API design for new/chunk APIs
   - Backward compatibility during transition
   - Data migration planning
   - Testing strategy for large datasets

---

## 4. Compliance Hooks & Blueprint Use (IN PROGRESS)

---

### 4.1 Compliance & Security Architecture**

**Privacy by Design Matrix:**

| Table          | PII Level | Encryption | Retention | GDPR Rights | Audit Level |
|----------------|-----------|------------|-----------|-------------|-------------|
| user           | High      |    ✅     | 7 years   | Full        | Complete    |
| evidence_file  | Critical  |    ✅     | Permanent | Limited     | Forensic    |
| case           | High      |    ✅     | 10 years  | Partial     | Complete    |
| rag_codriver   | Medium    |    ✅     | 2 years   | Full        | Standard    |
| nexus_feedback | Low       |    ❌     | 1 year    | Full        | Basic       |

- Roles, permissions: auditable links, verifiable access/revocation
- Triggers/milestones: API/DB design for audit snapshot, manual/AI review
- Evidence flow: Chain-of-custody, destructive actions admin-only
- Audit trails: Linked records for all key events/flows
- Documentation best practices for iterative handoff

---

## 5. Database (Full Schema, Fields, Roles, Modules)

---

### 5.1 Database Performance & Analytics Dashboard

**Current Database Health:**
- **Total Storage:** 860 kB across 11 tables
- **Active Records:** 60 (20 users, 20 roles, 8 modules, 11 assignments, 1 org)
- **Empty Tables Ready for Production:** 6 tables (rag_document, nexus_feedback, nexus_file, evidence_file, rag_codriver, case)
- **Growth Capacity:** Scalable architecture ready for 1000x expansion

---

### 5.2 Table Overview                

Table               |  Description / Usage / Purpose /                                                                      
--------------------+------------------------------------------------------------------+------------------------------------------
user                | Core user management and authentication for whole Photini Family (one workspace, several modules). User identity, authentication, core data.        
rag_document        |  RAG document storage for AI capabilities. RAG document storage (AI KB).Designed as a template for whole Photini Family.
nexus_module        |  Platform module definitions and configurations for every Photini Family techs member and big advocacy projects like Emergency and Case Finland projects.          
nexus_feedback      |  User feedback and system responses for whole Photini Family.                
nexus_company_info  |  Company/organization information made now only for Nexus (name will be changed to Photini and other family members after registration.                    
nexus_file          |  File management and storage. All files and attachments uploaded to the Nexus platform. Supports association with users, modules, visibility, audit, and compliance. Design for whole Photini Family.                 
nexus_role          |  Role-based access control definitions to different modules.             
user_app_role       |  User-to-role assignments for applications (modules). Linking user to role/app/module for granular permissions, audit, onboarding/revocation.          
evidence_file       |  Evidence file storage for legal cases in Go-Driver MVP              
rag_codriver        |  RAG co-driver AI assistant data (instructions, manifests, protocols, beta-test cases Go-Driver and Case Finland cases) in Emergency module and Go-Driver MVP             
case                |  Legal case management. Stores core information for each human rights case, including unique identifiers, ownership, status, and related metadata.                 


#### 5.2.1 Tables   |  ID   |  Storage  |  Records  |                                           
--------------------+-------+-----------+-----------|
user                |  #1   |  147 kB   |  20       |         
rag_document        |  #2   |  41 kB    |  0        |       
nexus_module        |  #3   |  115 kB   |  8        |   
nexus_feedback      |  #4   |  41 kB    |  0        |              
nexus_company_info  |  #5   |  115 kB   |  1        |                
nexus_file          |  #6   |  41 kB    |  0        |                     
nexus_role          |  #7   |  115 kB   |  20       |          
user_app_role       |  #8   |  106 kB   |  11       |       
evidence_file       |  #9   |  41 kB    |  0        |         
rag_codriver        |  #10  |  41 kB    |  0        |               
case                |  #11  |  57 kB    |  0        |      


#### 5.2.2 User -table #1.

Field                   |  Type         |  Notes                                
------------------------+---------------+---------------------------------------
id                      |  integer      |  Primary key, auto-increment          
created_at              |  datetime     |  Account creation timestamp           
name                    |  string       |  User display name                    
email                   |  string       |  Login identifier, unique             
password                |  string       |  Encrypted password                   
file_type               |  string       |  Profile attachment MIME type         
file_url                |  string       |  Profile attachment storage URL       
original_source         |  string       |  File origin/source reference         
attachment_name         |  string       |  Original attachment filename         
document_date           |  datetime     |  Document/attachment timestamp        
ai_summary              |  text         |  AI-generated profile summary         
ai_version              |  string       |  AI model version used                
summary_timestamp       |  datetime     |  When AI summary was created          
updated_at              |  datetime     |  Last profile modification            
visibility              |  string       |  Access control setting               
AI_vector               |  vector/JSON  |  Vector embeddings for AI features    
internal_documentation  |  text         |  Internal admin notes                 
role                    |  string/int   |  User role assignment (FK: nexus_role)
auth_token              |  string       |  Authentication token for sessions    

Key Relationships:

role → nexus_role.id (Many-to-one)
id → Referenced by user_app_role.user_id, case.owner_id, evidence_file.reviewed_by_user_id, etc.

This table supports 20 active users with full authentication, role-based access control, AI integration capabilities, and audit trails (mock data created by Xano for test purposes).


#### 5.2.3 rag_document -table #2.

Field                   |  Type           |  Notes                                             
------------------------+-----------------+----------------------------------------------------
id                      |  integer        |  Primary key, auto-increment                       
case_id                 |  integer        |  Associated case reference (FK: case)              
doc_id                  |  string         |  Document identifier/reference code                
content                 |  text           |  Main document content for RAG processing          
project                 |  string         |  Project association/categorization                
tags                    |  array[string]  |  Classification tags for search/filtering          
user_id                 |  integer        |  Document owner/creator (FK: user)                 
created_at              |  datetime       |  Document creation timestamp                       
updated_at              |  datetime       |  Last modification timestamp                       
visibility              |  string         |  Access control setting (public/private/restricted)
AI_vector               |  vector/JSON    |  Vector embeddings for RAG retrieval               
internal_documentation  |  text           |  Internal admin notes and metadata       

Key Relationships:

case_id → case.id (Many-to-one)
user_id → user.id (Many-to-one)
id → Referenced by RAG retrieval systems and AI processing

Purpose: This table stores documents for Retrieval Augmented Generation (RAG) features, enabling AI-powered document search, contextual responses, and knowledge base functionality. Currently empty and ready for document ingestion to support Go-Driver's AI capabilities.          


#### 5.2.4 nexus_module -table #3.

Field          |  Type           |  Notes                                 
---------------+-----------------+----------------------------------------
id             |  integer        |  Primary key, auto-increment           
created_at     |  datetime       |  Module creation timestamp             
name           |  string         |  Module name, unique identifier        
status         |  string         |  Current status (active/beta/blueprint)
description    |  text           |  Module functionality description      
activity_log   |  array[string]  |  Activity history and status updates   
feature_flags  |  array[string]  |  Feature toggles and capabilities      

Key Relationships:

id → Referenced by user_app_role.app_id, nexus_file.nexus_module_id, evidence_file.nexus_module_id

Used for modular permissions and feature access control

**Current Module Data (8 Records):**

ID |  Name        | Status    | Description                                                                            |  Activity Log                                     |  Feature Flags               
---+--------------+-----------+----------------------------------------------------------------------------------------+---------------------------------------------------+------------------------------
2  | Phoní        | blueprint | Global insights module for data-driven advocacy and research                           |  "Initialized. Tracking global metrics."          |  "ai-translation", "map"     
3  | Bridge       | blueprint | Secure communication and collaboration tools for partners and teams                    |  "In beta. Pilot with NGO partners ongoing."      |  "chat", "file-share"        
4  | Beacon       | blueprint | AI-powered guidance, support, and translation for all platform users                   |  "Deployed. Monitoring usage analytics."          |  "ai-guidance", "onboard"    
5  | Compliance   | alfa      | Dashboard for regulatory, audit, and human rights compliance monitoring                |  "Set up for initial reporting."                  |  "reporting", "audit"        
6  | Rhea         | blueprint | Human-in-the-loop and moderation network for safeguarding and escalation               |  "Team onboarding. Child rights pilot."           |  "htl", "moderation"         
7  | Case Finland | beta      | Regional showcase for innovative policy, pilot programs, and case studies              |  "Launching next sprint with local data feeds."   |  "showcase", "region"        
8  | Evidence RAG | active    | Evidence management with RAG capabilities. (My son's evidence / case Finland evidence.)|  "Active development for Go-Driver integration."  |  "rag", "evidence"           
9  | Go-Driver    | beta      | AI assistant with wheel for productivity and collaboration                             |  "Beta testing with core users."                  |  "ai-assistant", "chrome-ext"

Purpose: This table defines the modular architecture of the Photini Family platform, enabling feature-based access control, development tracking, and scalable functionality deployment.


#### 5.2.5 nexus_feedback -table #4.

Field                 |  Type      |  Notes                                              
----------------------+------------+-----------------------------------------------------
id                    |  integer   |  Primary key, auto-increment                        
feedback_text         |  text      |  Main feedback content/message                      
status                |  string    |  Processing status (pending/reviewed/resolved)      
user_id               |  integer   |  Registered user who submitted feedback (FK: user)  
module_id             |  integer   |  Related module/app for feedback (FK: nexus_module) 
visitor_email         |  string    |  Email for anonymous/guest feedback submissions     
submitted_as_visitor  |  boolean   |  Flag indicating guest submission vs registered user
created_at            |  datetime  |  Feedback submission timestamp                      

Key Relationships:

user_id → user.id (Many-to-one, optional for registered users)
module_id → nexus_module.id (Many-to-one, optional for module-specific feedback)
Supports both authenticated user feedback and anonymous visitor submissions

Purpose: This table collects user feedback, bug reports, and suggestions for the Photini Family platform. It supports both registered user feedback (linked to user accounts) and anonymous visitor feedback (using email only). Currently empty and ready to collect user input for system improvements and compliance monitoring.

Feedback Categories:
Module-specific feedback (linked via module_id)
General platform feedback
Bug reports and technical issues
Feature requests and suggestions
Compliance or safety concerns


#### 5.2.6 nexus_company_info -table #5.

Field        |  Type      |  Notes                                   
-------------+------------+------------------------------------------
id           |  integer   |  Primary key, auto-increment             
created_at   |  datetime  |  Organization record creation timestamp  
name         |  string    |  Organization/company name               
address      |  string    |  Physical address or location            
phone        |  string    |  Primary contact phone number            
email        |  string    |  Primary contact email address           
business_id  |  string    |  Official registration/business ID number

Key Relationships:

Standalone table for now, no direct foreign key relationships
Referenced for organizational context and compliance reporting
Used for legal entity information and contact details

Purpose: This table stores organizational information for the Photini Family platform, including legal entity details, contact information, and business registration data. Currently contains 1 record representing the primary organization running the platform.

Usage Context:
- Legal compliance and audit requirements
- Contact information for external partners and authorities/websites and possibility to link company details to modules when available
- Business registration and official documentation
- Footer/about page information
- Regulatory reporting and transparency
- Partnership and collaboration coordination

Data Status: Active with 1 organizational profile configured (115 kB storage indicates rich organizational data including potentially logos, documents, or extended descriptions).


#### 5.2.7 nexus_file -table #6.

Field                   |  Type           |  Notes                                             
------------------------+-----------------+----------------------------------------------------
id                      |  integer        |  Primary key, auto-increment                       
case_id                 |  integer        |  Associated case reference (FK: case)              
doc_id                  |  string         |  Document identifier/reference code                
file_name               |  string         |  Original filename at upload                       
content                 |  text           |  File content description or extracted text        
project                 |  string         |  Project association/categorization                
tags                    |  array[string]  |  Classification tags for search/filtering          
file_type               |  string         |  MIME type (pdf, jpg, docx, etc.)                  
file_url                |  string         |  Storage URL or file path                          
original_source         |  string         |  Source origin/upload context                      
document_date           |  datetime       |  Original document date/timestamp                  
redacted_version_url    |  string         |  URL to privacy-safe redacted version              
reviewed_by_user_id     |  integer        |  User who reviewed/approved file (FK: user)        
ai_summary              |  text           |  AI-generated file summary                         
ai_version              |  string         |  AI model version used for processing              
summary_timestamp       |  datetime       |  When AI summary was generated                     
nexus_module_id         |  integer        |  Associated module (FK: nexus_module)              
user_app_role_id        |  integer        |  Role context for file access (FK: user_app_role)  
user_id                 |  integer        |  File owner/uploader (FK: user)                    
created_at              |  datetime       |  File upload timestamp                             
updated_at              |  datetime       |  Last modification timestamp                       
visibility              |  string         |  Access control setting (public/private/restricted)
AI_vector               |  vector/JSON    |  Vector embeddings for AI search                   
internal_documentation  |  text           |  Internal admin notes                              

Key Relationships:

case_id → case.id (Many-to-one)
user_id → user.id (Many-to-one)
nexus_module_id → nexus_module.id (Many-to-one)
user_app_role_id → user_app_role.id (Many-to-one)
reviewed_by_user_id → user.id (Many-to-one)

Purpose: This table manages all file uploads and attachments for the Photini Nexus platform. It supports association with users, modules, cases, and provides granular access control, audit trails, and compliance features including redacted versions for privacy protection. The schema has been designed for whole Photini Family's use.

Features:

- Chain of custody: Full audit trail with reviewer tracking
- Privacy compliance: Redacted versions for sensitive content
- AI integration: Automated summaries and vector search
- Modular access: Role-based file visibility
- Multi-format support: Documents, images, evidence files

Currently empty and ready for file management across all Photini Family applications.
                         

#### 5.2.8 nexus_role -table #7   

Field        |  Type      |  Notes                                
-------------+------------+---------------------------------------
id           |  integer   |  Primary key, auto-increment          
name         |  string    |  Role name, unique identifier         
Description  |  text      |  Full role description and permissions
created_at   |  datetime  |  Role creation timestamp           

Key Relationships:

id → Referenced by user_app_role.role_id, user.role

Central to role-based access control (RBAC) system
Defines permissions for all platform access
Current Role Data (20 Records):  

**Roles Table (nexus_role-export)**

id  |name	             |Description
----+------------------------+---------------------------------------------------------------------------------------------------
1   |Manager	             |Operational leader responsible for supervising module activities, overseeing project implementation, managing user assignments, and ensuring compliance with organizational protocols
2   |Investor	             |Accredited financial contributor with access to project overviews, milestone reports, impact metrics, and selected progress dashboards
3   |Auditor	             |Independent reviewer or partner authorized to access detailed audit logs, compliance records, and sensitive case data for oversight, investigation, or legal reporting
4   |NGO	             |NGO partner with ability to submit cases and participate in audits.
5   |Visitor	             |Anonymous or general public user with access to open modules, public dashboards, selected impact stories, and general information only
6   |Admin	|Full system administrator
7   |Rhea Guardian           |Trusted guardian/advocate with privileges to submit or moderate urgent safety reports, monitor protected cases, and interface with support networks.
8   |Beta-Tester             |Early-access partner invited to test modules, provide live feedback, report bugs, and recommend usability improvements
9   |Supporter	             |Registered supporter or donor
10  |Media	             |Verified journalist or media partner with access to designated public stories, case summaries, platform statistics, and press resources for responsible reporting
11  |Embassy/Consulate	     |Users for political asylum applications
12  |AI Evidence	     |AI role for emergency evidence_rag and evidenge_file with out admin role (POST and PATCH in API group)
13  |AI Evidence Admin	     |AI role for emergency evidence_rag and evidenge_file for admin role (GETs, POST and PATCH in API group)
14  |AI Photini nexus	     |AI role for Photini Family building app Photini Nexus
15  |AI Photini Beacon       |AI role for Photini Family's app Beacon
16  |AI  Photini Bridge	     |AI role for Photini Family's app Bridge
17  |AI Photini Phoní­	     |AI role for Photini Family's app PhonÃ­
18  |AI Rhea Network	     |AI role for Photini Family's app for Rhea Network
19  |AI Photini Admin	     |AI admin role for Photini Family
20  |AI Co-Driver	     |AI role for Co-Driver assistant

Purpose: This table defines the complete role-based access control system for the Photini Family platform, supporting both human users (IDs 1-11) and AI service accounts (IDs 12-20) with granular permission structures for compliance, security, and operational efficiency.


#### 5.2.9 user_app_role -table #8.   

Field       |  Type      |  Notes                                     
------------+------------+--------------------------------------------
id          |  integer   |  Primary key, auto-increment               
user_id     |  integer   |  User reference (FK: user)                 
role_id     |  integer   |  Role reference (FK: nexus_role)           
app_id      |  integer   |  Module/app reference (FK: nexus_module)   
created_at  |  datetime  |  Role assignment timestamp                 
removed_at  |  datetime  |  Role revocation timestamp (null if active)

Key Relationships:

user_id → user.id (Many-to-one)
role_id → nexus_role.id (Many-to-one)
app_id → nexus_module.id (Many-to-one)
Junction table linking users, roles, and modules for granular permissions

Purpose: This table manages granular role assignments for users within specific applications/modules. It enables:
Per-module permissions: Users can have different roles in different modules
Precise access control: Role-based permissions within specific app contexts
Audit trails: Full history of role assignments and revocations
Modular security: Each Photini Family app can have independent permission structures
Seamless onboarding/revocation: Time-stamped assignment tracking

Current Status (11 Active Assignments):
- Links 20 users across 8 modules with appropriate role-based permissions
- Supports both human users and AI service accounts
- Enables module-specific access patterns (e.g., Admin role in Go-Driver, Visitor role in public modules)
- 106 kB storage indicates rich permission metadata and audit history

Example Use Cases:
- User A: Manager role in Compliance module, Supporter role in Beacon module
- User B: Admin role in Go-Driver module, Auditor role in Evidence RAG module
- AI Co-Driver: AI Co-Driver role specifically in Go-Driver module
- Beta Tester: Testing role across multiple beta modules
This table is central to the platform's security architecture, enabling fine-grained, auditable, and revocable permissions across the entire Photini Family ecosystem.


#### 5.2.10 evidence_file -table #9. 

Field                 |  Type           |  Notes                                               
----------------------+-----------------+------------------------------------------------------
id                    |  integer        |  Primary key, auto-increment                         
case_id               |  integer        |  Associated case reference (FK: case)                
doc_id                |  string         |  Document identifier/reference code                  
file_name             |  string         |  Original filename at upload                         
content               |  text           |  File content description or extracted text          
project               |  string         |  Project association/categorization                  
tags                  |  array[string]  |  Classification tags for evidence search/filtering   
file_type             |  string         |  MIME type (pdf, jpg, docx, mp4, etc.)               
file_url              |  string         |  Storage URL or secure file path                     
original_source       |  string         |  Source origin/chain of custody information          
document_date         |  datetime       |  Original document date/timestamp                    
redacted_version_url  |  string         |  URL to privacy-safe redacted version                
reviewed_by_user_id   |  integer        |  User who reviewed/approved evidence (FK: user)      
ai_summary            |  text           |  AI-generated evidence summary                       
ai_version            |  string         |  AI model version used for processing                
summary_timestamp     |  datetime       |  When AI summary was generated                       
nexus_module_id       |  integer        |  Associated module (FK: nexus_module)                
user_app_role_id      |  integer        |  Role context for evidence access (FK: user_app_role)


Key Relationships:

case_id → case.id (Many-to-one, required)
reviewed_by_user_id → user.id (Many-to-one)
nexus_module_id → nexus_module.id (Many-to-one)
user_app_role_id → user_app_role.id (Many-to-one)

Purpose: This table manages evidentiary attachments specifically for human rights cases, mirroring the structure of nexus_file but with enhanced chain of custody and legal compliance features.

Evidence-Specific Features:
- Legal chain of custody: Full audit trail with reviewer tracking
- Privacy compliance: Mandatory redacted versions for sensitive content
- Case linkage: Always tied to specific human rights cases
- AI processing: Automated evidence summaries while preserving originals
- Access control: Role-based evidence visibility for legal proceedings
- Never delete: Evidence files require special admin approval for any destructive actions

Compliance Standards:
- GDPR compliance: Redacted versions protect PII
- Legal admissibility: Maintains original file integrity with timestamps
- Audit trails: Every access and modification logged
- Role restrictions: Evidence access limited by user roles and case assignments

Currently empty and ready for secure evidence management across human rights cases. This table is critical for legal proceedings and maintains the highest security and compliance standards in the Photini Family ecosystem.
           

#### 5.2.11 rag_codriver -table #10.

Field                   |  Type           |  Notes                                             
------------------------+-----------------+----------------------------------------------------
id                      |  integer        |  Primary key, auto-increment                       
case_id                 |  integer        |  Associated case reference (FK: case)              
doc_id                  |  string         |  Document identifier/session reference             
content                 |  text           |  Session content, AI suggestions, or co-driver data
project                 |  string         |  Project association/categorization                
tags                    |  array[string]  |  Classification tags for session filtering         
file_type               |  string         |  Content type (session, suggestion, protocol, etc.)
file_url                |  string         |  Storage URL for session data/attachments          
original_source         |  string         |  Source origin (Go-Driver, API, user input)        
attachment_name         |  string         |  Associated file or attachment name                
document_date           |  datetime       |  Original session/document timestamp               
ai_summary              |  text           |  AI-generated session summary                      
ai_version              |  string         |  AI model version used for co-driver features      
summary_timestamp       |  datetime       |  When AI summary was generated                     
created_at              |  datetime       |  Session creation timestamp                        
updated_at              |  datetime       |  Last session modification                         
visibility              |  string         |  Access control (private/shared/team)              
AI_vector               |  vector/JSON    |  Vector embeddings for RAG retrieval               
internal_documentation  |  text           |  Internal notes and session metadata               

Key Relationships:

case_id → case.id (Many-to-one, optional)
Used by Go-Driver AI assistant for session management
Supports both case-specific and general productivity sessions

Purpose: This table stores documents and content for Retrieval Augmented Generation (RAG) co-driver features, including:
- AI session data: Go-Driver conversation histories and context
- Instructions & protocols: AI guidance documents and workflows
- Suggestions & recommendations: AI-generated productivity insights
- Manifests & configurations: Co-driver behavior and preference settings

Co-Driver Features:
- Session continuity: Maintains conversation context across interactions
- Smart suggestions: AI-powered recommendations based on user patterns
- Protocol adherence: Ensures AI follows ethical and compliance guidelines
- Context awareness: Links sessions to relevant cases and projects
- Privacy protection: User-controlled visibility and data retention

Integration with Go-Driver:
- Chrome extension backend: Stores session data for Go-Driver panel
- RAG capabilities: Enables contextual AI responses using historical data
- Productivity tracking: Monitors and improves user workflow patterns
- Compliance monitoring: Ensures AI suggestions align with ethical standards

Currently empty and ready to support Go-Driver's AI assistant capabilities with full RAG functionality for enhanced productivity and ethical collaboration.
                   

#### 5.2.12 case -table #11.

Field               |  Type      |  Notes                                               
--------------------+------------+------------------------------------------------------
id                  |  integer   |  Primary key, auto-increment                         
created_at          |  datetime  |  Case creation timestamp                             
case_id             |  string    |  Unique case identifier/reference number             
title               |  string    |  Case title or summary description                   
owner_id            |  integer   |  Case owner/primary responsible user (FK: user)      
status              |  string    |  Current case status (active/pending/archived/closed)
priority            |  integer   |  Priority level (1=highest, 5=lowest)                
updated_at          |  datetime  |  Last case modification timestamp                    
external_reference  |  string    |  External case reference (court, organization, etc.) 
sanitized           |  boolean   |  Data sanitization flag for privacy compliance       


Key Relationships:

owner_id → user.id (Many-to-one, required)
id → Referenced by evidence_file.case_id, nexus_file.case_id, rag_document.case_id, rag_codriver.case_id
Central hub linking all case-related evidence, documents, and sessions

Purpose: This table stores core information for each human rights case, serving as the central organizing structure for all case-related activities in the Photini Family platform.

Case Management Features:
- Unique identification: Both internal ID and external case reference numbers
- Ownership tracking: Clear assignment of primary responsibility
- Status management: Workflow tracking from creation to resolution
- Priority system: Resource allocation and urgency management
- Privacy compliance: Sanitization flag for sensitive data protection
- Audit trail: Complete timestamp history of case lifecycle

Case Workflow:
- Creation: New cases assigned to responsible users
- Active management: Status updates, priority adjustments, evidence collection
- Collaboration: Multiple users can access based on role permissions
- Documentation: All evidence files and documents linked to case
- Resolution: Status changes through defined workflow stages

Integration Points:
- Evidence files: All evidentiary attachments linked via case_id
- Documents: RAG documents and general files associated with cases
- AI processing: Co-driver sessions can reference specific cases
- Compliance: Sanitization and external reference tracking for legal requirements

Currently empty and ready to serve as the central case management system for human rights advocacy work across the Photini Family ecosystem. This table is critical for legal and advocacy operations, maintaining complete case histories with full audit trails.

---

### 5.3 Troubleshooting & Maintenance Guide

**Common Database Issues & Solutions:**

| Issue                               | Symptoms              | Solution                        | Prevention                   |
|-------------------------------------|-----------------------|---------------------------------|------------------------------|
| Role conflicts                      | Access denied errors  | Check user_app_role assignments | Regular permission audits    |
| Missing relationships               | Data integrity errors | Verify foreign key constraints  | Automated validation checks  |
| Performance slow                    | Query timeouts        | Index optimization              | Monitor query performance    |
| Storage growth                      | High storage usage    | Archive old records             | Implement retention policies |

---

### 5.4 Field-by-Field Relationships Overview


**Visual Database Relationship Diagram:**
- **Source:** `Xano-DB-relationships-10.10.2025.pdf`
- **Generated:** October 10, 2025
- **Shows:** All 11 tables with field relationships, foreign keys, and data flow

**Key Relationship Patterns:**
```
User-Centric Model:
user → user_app_role → nexus_module
  ↓         ↓             ↓
case → evidence_file → nexus_file
  ↓         ↓             ↓
rag_document → rag_codriver → AI processing
```

**Complex Case Relationships (3000-5000 pages):**
```
case (main record)
├── evidence_file[] (hundreds of files)
├── rag_document[] (thousands of pages)
├── rag_codriver[] (Go-Driver sessions)
├── nexus_file[] (supporting documents)
└── case_splits[] (future: linked/split cases)
```

**Performance Considerations:**
- **Large cases:** Pagination required for evidence_file listings
- **Chunked loading:** rag_document content loaded in segments
- **Indexing strategy:** case_id indexed across all related tables
- **Archive strategy:** Old case data moved to separate storage

---

#### 5.4.1 Data Flow & Integration Patterns

**User Journey Mapping:**
```
Registration → Role Assignment → Module Access → Case Creation → Evidence Upload → AI Processing
     ↓              ↓              ↓              ↓              ↓              ↓
   user        user_app_role    nexus_module     case      evidence_file   rag_codriver
```

**AI Integration Pipeline:**
```
Document Upload → Content Extraction → Vector Embedding → RAG Processing → AI Summary
      ↓                 ↓                 ↓                ↓               ↓
 nexus_file        rag_document       AI_vector      rag_codriver    ai_summary
```


#### 5.4.2 user -table #1.

Field       |  Type        |  Required  |  Constraints    |  Relationships   |  Sample Value    
------------+--------------+------------+-----------------+------------------+------------------
id          |  integer     |  Yes       |  PK, auto-incr  |  —               |  1               
email       |  string      |  Yes       |  Unique         |  —               |  "user@site.com" 
password    |  string      |  Yes       |  —              |  —               |  —               
role        |  string/int  |  Yes       |  —              |  FK: nexus_role  |  2               
created_at  |  datetime    |  Yes       |  —              |  —               |  "2025-08-21T..."


#### 5.4.3 rag_document table #2.

Field     |  Type           |  Required  |  Constraints    |  Relationships  |  Sample Value 
----------+-----------------+------------+-----------------+-----------------+---------------
id        |  integer        |  Yes       |  PK, auto-incr  |  —              |  1            
case_id   |  integer        |  Yes       |  —              |  FK: case       |  3            
docid     |  string         |  Yes       |  —              |  —              |  "doc_001"    
content   |  text           |  Yes       |  —              |  —              |  "..."        
tags      |  array[string]  |  No        |  —              |  —              |  ["AI","case"]
AIvector  |  vector/JSON    |  No        |  —              |  —              |  {…}          


#### 5.4.4 nexus_module -table #3.

Field       |  Type       |  Required  |  Constraints    |  Relationships  |  Sample Value    
------------+-------------+------------+-----------------+-----------------+------------------
id          |  integer    |  Yes       |  PK, auto-incr  |  —              |  1               
name        |  string     |  Yes       |  Unique         |  —              |  "caseMgr"       
config      |  JSON/text  |  No        |  —              |  —              |  {...}           
updated_at  |  datetime   |  Yes       |  —              |  —              |  "2025-08-22T..."


#### 5.4.5 nexus_feedback -table #4.

Field       |  Type      |  Required  |  Constraints    |  Relationships  |  Sample Value
------------+------------+------------+-----------------+-----------------+--------------
id          |  integer   |  Yes       |  PK, auto-incr  |  —              |  1           
user_id     |  integer   |  Yes       |  —              |  FK: user       |  1           
feedback    |  text      |  Yes       |  —              |  —              |  "Good UI"   
created_at  |  datetime  |  Yes       |  —              |  —              |  "..."       


#### 5.4.6 nexus_company_info -table #5.

Field    |  Type     |  Required  |  Constraints    |  Relationships  |  Sample Value   
---------+-----------+------------+-----------------+-----------------+-----------------
id       |  integer  |  Yes       |  PK, auto-incr  |  —              |  1              
name     |  string   |  Yes       |  —              |  —              |  "Photini Org"  
address  |  string   |  No        |  —              |  —              |  "Patra, Greece"
contact  |  string   |  No        |  —              |  —              |  "+30 123456789"


#### 5.4.7 nexus_file -table #6.

Field       |  Type      |  Required  |  Constraints    |  Relationships     |  Sample Value  
------------+------------+------------+-----------------+--------------------+----------------
id          |  integer   |  Yes       |  PK, auto-incr  |  —                 |  1             
filename    |  string    |  Yes       |  —              |  —                 |  "evidence.pdf"
fileurl     |  string    |  Yes       |  —              |  —                 |  "https://..." 
user_id     |  integer   |  Yes       |  —              |  FK: user          |  1             
module_id   |  integer   |  No        |  —              |  FK: nexus_module  |  3             
case_id     |  integer   |  No        |  —              |  FK: case          |  2             
created_at  |  datetime  |  Yes       |  —              |  —                 |  "..."        


#### 5.4.8 nexus_role -table #7.

Field        |  Type       |  Required  |  Constraints    |  Relationships  |  Sample Value
-------------+-------------+------------+-----------------+-----------------+--------------
id           |  integer    |  Yes       |  PK, auto-incr  |  —              |  1           
role_name    |  string     |  Yes       |  Unique         |  —              |  "Admin"     
permissions  |  JSON/text  |  No        |  —              |  —              |  {...}        


#### 5.4.9 user_app_role -table #8.

Field        |  Type      |  Required  |  Constraints    |  Relationships     |  Sample Value
-------------+------------+------------+-----------------+--------------------+--------------
id           |  integer   |  Yes       |  PK, auto-incr  |  —                 |  1           
user_id      |  integer   |  Yes       |  —              |  FK: user          |  1           
role_id      |  integer   |  Yes       |  —              |  FK: nexus_role    |  2           
app_id       |  integer   |  Yes       |  —              |  FK: nexus_module  |  3           
assigned_at  |  datetime  |  Yes       |  —              |  —                 |  "..."       


#### 5.4.10 evidence_file -table #9.

Field        |  Type        |  Required  |  Constraints    |  Relationships  |  Sample Value  
-------------+--------------+------------+-----------------+-----------------+----------------
id           |  integer     |  Yes       |  PK, auto-incr  |  —              |  1             
case_id      |  integer     |  Yes       |  —              |  FK: case       |  2             
user_id      |  integer     |  Yes       |  —              |  FK: user       |  1             
filename     |  string      |  Yes       |  —              |  —              |  "evidence.jpg"
fileurl      |  string      |  Yes       |  —              |  —              |  "https://..." 
tags         |  array[str]  |  No        |  —              |  —              |  ["doc","file"]
reviewed_by  |  integer     |  No        |  —              |  FK: user       |  4             


#### 5.4.11 rag_codriver -table #10.

Field            |  Type     |  Required  |  Constraints    |  Relationships  |  Sample Value  
-----------------+-----------+------------+-----------------+-----------------+----------------
id               |  integer  |  Yes       |  PK, auto-incr  |  —              |  1             
session_id       |  string   |  Yes       |  —              |  —              |  "sess_ab123"  
suggestion_data  |  JSON     |  No        |  —              |  —              |  {...}         
summary          |  text     |  No        |  —              |  —              |  "session info"


#### 5.4.12 case -table #11.

Field         |  Type      |  Required  |  Constraints    |  Relationships  |  Sample Value       
--------------+------------+------------+-----------------+-----------------+---------------------
id            |  integer   |  Yes       |  PK, auto-incr  |  —              |  1                  
owner_id      |  integer   |  Yes       |  —              |  FK: user       |  1                  
title         |  string    |  Yes       |  —              |  —              |  "Child Rights Case"
status        |  string    |  Yes       |  —              |  —              |  "Active"           
priority      |  int       |  No        |  —              |  —              |  2                  
created_at    |  datetime  |  Yes       |  —              |  —              |  "..."              
external_ref  |  string    |  No        |  —              |  —              |  "ECHR-2025-001"    

Legend:
PK = Primary key, auto-incr = auto-increment, FK = Foreign key, JSON = flexible data type, required = must be present for record creation.


---

## 6. API Endpoints & Compliance Logic


### 6.1 API Groups Overview abd Endpoint Status Summary


**Overview**

| Group         | Endpoints | Status     | Purpose                          | Primary Tables              |
|---------------|-----------|------------|----------------------------------|-----------------------------|
| user_auth     |    3      | Active     | Authentication & user management | user, nexus_role            |
| rag_codriver  |    5      | Draft/Beta | AI assistant sessions            | rag_codriver, case          |
| case_mgmt     |    6+     | Planned    | Case management workflow         | case, evidence_file         |
| evidence      |    4+     | Planned    | Evidence handling                | evidence_file, nexus_file   |
| rag_document  |    4+     | Planned    | Document RAG processing          | rag_document                |
| admin         |    8+     | Planned    | System administration            | user_app_role, nexus_module |


**API Endpoint Status Summary**

| Endpoint                  | Status      | Priority | Development Phase | Notes                                  |
|---------------------------|-------------|----------|-------------------|----------------------------------------|
| **User Authentication**   |             |          |                   |                                        |
| POST /auth/login          | ✅ Active  | High     | Production        | Core authentication                    |
| GET /auth/me              | ✅ Active  | High     | Production        | User context                           |
| POST /auth/signup         | ✅ Active  | High     | Production        | User registration                      |
| **RAG Co-Driver**         |             |          |                   |                                        |
| GET /rag_codriver         | 🚧 Draft   | High     | Development       | Session listing                        |
| POST /rag_codriver        | 🚧 Draft   | High     | Development       | Session creation                       |
| GET /rag_codriver/{id}    | 🚧 Draft   | Medium   | Development       | Single session                         |
| PATCH /rag_codriver/{id}  | 🚧 Draft   | Medium   | Development       | Session updates                        |
| DELETE /rag_codriver/{id} | 🚧 Draft   | Low      | Development       | Admin only                             | 
| **Case Management**       |             |          |                   |                                        |
| GET /case                 | 📋 Planned | High     | Q1 2026           | Case listing with massive data support |
| POST /case                | 📋 Planned | High     | Q1 2026           | Case creation                          |
| GET /case/{id}            | 📋 Planned | High     | Q1 2026           | **3000-5000 pages + logs**             |
| PATCH /case/{id}          | 📋 Planned | Medium   | Q1 2026           | Case updates                           |
| POST /case/{id}/split     | 📋 Planned | Medium   | Q2 2026           | **Case splitting functionality**       |
| POST /case/{id}/link      | 📋 Planned | Medium   | Q2 2026           | **Case linking functionality**         |
| **Evidence Management**   |             |          |                   |                                        |
| GET /evidence             | 📋 Planned | High     | Q1 2026           | Evidence listing                       |
| POST /evidence            | 📋 Planned | High     | Q1 2026           | **Multi-format support**               |
| POST /evidence/batch      | 📋 Planned | Medium   | Q2 2026           | Bulk upload                            |
| **Integration APIs**      |             |          |                   |                                        |
| POST /api/new-format      | 🔮 Future  | Medium   | Q2 2026           | **New API integration**                |
| POST /api/chunk           | 🔮 Future  | Medium   | Q2 2026           | **Chunk API integration**              |

**Legend:**
- ✅ **Active:** Production ready
- 🚧 **Draft:** In development, testing phase
- 📋 **Planned:** Designed, awaiting development
- 🔮 **Future:** Concept phase, future iteration

---

### 6.2 Compliance & Security Standards

---

#### 6.2.1 Global Function Stack Components

1️⃣ Authentication Layer
```
Input → Token Validation → User Context → Permission Check → Process
```

2️⃣ Validation Layer
```
Raw Input → Schema Check → Data Sanitization → Business Rules → Proceed
```

3️⃣ Audit Layer
```
Action Start → Log Entry → Process → Result Log → Response
```

4️⃣ Error Handling
```
Error Occurs → Sanitize Error → Log Details → Return Safe Message
```

#### 6.2.2 Role-Based Access Matrix

| Endpoint             | Visitor | Supporter | Manager | Admin | AI Co-Driver |
|----------------------|---------|-----------|---------|-------|--------------|
| GET /auth/me         |   ✅   |    ✅     |   ✅   |  ✅   |     ✅      |
| POST /rag_codriver   |   ❌   |    ✅     |   ✅   |  ✅   |     ✅      |
| DELETE /rag_codriver |   ❌   |    ❌     |   ✅   |  ✅   |     ✅      |
| POST /evidence       |   ❌   |    ❌     |   ✅   |  ✅   |     ✅      |
| GET /admin/*         |   ❌   |    ❌     |   ❌   |  ✅   |     ❌      |


#### 6.2.3 Compliance Checklist

Data Protection:
- ✅ PII encryption at rest
- ✅ Audit logs for all data access
- ✅ Data retention policies enforced
- ✅ Right to erasure implemented

Security:
- ✅ Rate limiting on all endpoints
- ✅ SQL injection prevention
- ✅ CORS properly configured
- ✅ HTTPS enforcement

Evidence Handling:
- ✅ Chain of custody maintained
- ✅ Immutable evidence records
- ✅ Admin approval for deletions
- ✅ Audit trail for all evidence operations


#### 6.2.4 API Integration Patterns

**Frontend-Backend Communication Flow:**
```
Go-Driver UI → Authentication → Permission Check → Database Query → Response Formatting → UI Update
     ↓              ↓              ↓               ↓              ↓                ↓
Chrome Extension  JWT Token    Role Matrix    Xano Tables   JSON Response   User Interface
```

**Third-party Integration Points:**
- **GitHub:** Document sync and version control
- **Email Services:** Notifications and alerts
- **File Storage:** Evidence and document uploads
- **AI Services:** RAG processing and analysis
- **Analytics:** Usage tracking and insights

---

### 6.3 Development & Testing Standards

---

#### 6.3.1 API Testing Checklist

- [ ] Authentication flows tested
- [ ] Role permissions validated
- [ ] Input validation confirmed
- [ ] Error responses verified
- [ ] Compliance logic tested
- [ ] Audit logs reviewed


#### 6.3.2 Documentation Requirements

- Endpoint documentation:** Method, path, parameters, responses
- Function stack:** Step-by-step processing logic
- Compliance notes:** Security and privacy considerations
- Example requests/responses:** Working code samples
- Error scenarios:** Common failure cases and handling

Documentation Best Practices
- Table field descriptions at the top.
- API endpoints grouped and described with required/optional fields.
- Function stack step-by-step for critical endpoints.
- Audit snapshot for every action.
- Privacy: never expose PII in demo/test outputs.
- Update version as schema evolves (see CHANGELOG.md).


#### 6.3.3 Change Management Process

1. API Design:** Document new endpoints in this section
2. Implementation:** Code endpoints according to documented specs
3. Testing:** Validate against documented requirements
4. Documentation Update:** Refresh this document with any changes
5. Deployment:** Release with updated documentation

Validation & Function Stack
- All endpoints use function stacks: validation, role checks, audit logging, trigger actions for compliance.
- Precondition checks for required fields (e.g., case title, ownerid, role assignment).
- Role checks for evidence/emergency pipelines.
 -Shape JSON responses predictably: include id, docid, project, content, tags, timestamp.
- Evidence files are never auto-DELETED—require manual confirmation for any deletion.

Testing & Audit
- Run API tests via Postman or frontend UI (Go-Driver).
- Check audit logs for every CRUD action (user, time, action).
- Export anonymized logs for compliance or bug reports.
- Use STOP/UNDO for safe update/rollback scenarios.

---

### 6.4 User Authentication API Group

---

#### 6.4.1 POST /auth/login (#33)
Status: Active | Access: Public

| Field     | Type     | Required | Validation                  | Notes               |
|-----------|----------|----------|-----------------------------|---------------------|
| email     | string   |   ✅     | Email format, exists in DB  | Login identifier    |
| password  | string   |   ✅     | Min 8 chars, encrypted      | User password       |

Function Stack:
1. Input validation: Email format, required fields
2. User lookup: Find user by email in `user` table
3. Password verification: Compare encrypted passwords
4. Token generation: Create JWT with user context
5. Audit log: Record login attempt with timestamp
6. Response: Return user object + auth token

Response Format:

```json
{
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "role": 6
  },
  "auth_token": "jwt_token_here",
  "expires_at": "2025-10-11T11:00:00Z"
}
```

Compliance Logic:
- ✅ Failed login attempts logged
- ✅ Rate limiting (5 attempts/minute)
- ✅ No PII in error messages

---

#### 6.4.2 GET /auth/me (#35)
Status: Active | Access: Private (Token Required)

| Field         | Type     | Required | Validation           | Notes     |
|---------------|----------|----------|----------------------|-----------|
| Authorization | header   |   ✅     | Bearer token format  | JWT token |

Function Stack:
1. Token validation: Verify JWT signature and expiration
2. User lookup: Get user record by token payload
3. Role enrichment: Fetch current role and permissions
4. Response shaping: Return safe user data (no password)

Response Format:

```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "role": {
    "id": 6,
    "name": "Admin",
    "permissions": ["read", "write", "admin"]
  },
  "modules": [
    {"id": 9, "name": "Go-Driver", "role": "Admin"}
  ]
}
```

Compliance Logic:
- ✅ Token expiration enforced
- ✅ No password field in response
- ✅ Module access based on user_app_role

---

#### 6.4.3 POST /auth/signup (#34)
Status: Active | Access: Public

| Field        | Type    | Required | Validation                 | Notes                             |
|--------------|---------|----------|----------------------------|-----------------------------------|
| email        | string  |   ✅    | Email format, unique       | New user email                    |
| password     | string  |   ✅    | Min 8 chars, complexity    | User password                     |
| name         | string  |   ✅    | 2-50 chars                 | Display name                      |
| role_request | integer |   ❌    | Valid role ID              | Requested role (pending approval) |

Function Stack:
1. Input validation: Email uniqueness, password strength
2. User creation: Insert new record in `user` table
3. Role assignment: Default to "Visitor" (ID: 5) if no role specified
4. Token generation: Create JWT for immediate login
5. Audit log: Record new user registration
6. Email notification: Send welcome email (if configured)

Compliance Logic:
- ✅ Email uniqueness enforced
- ✅ Password encrypted before storage
- ✅ Default role: Visitor (limited access)
- ✅ Role escalation requires admin approval

---

### 6.5 RAG Co-Driver API Group 

---

#### 6.5.1 GET /rag_codriver (#37)
Status: Draft | Access: User (Token Required)

| Field           | Type   | Required | Validation    | Notes            |
|-----------------|--------|----------|---------------|------------------|
| Authorization   | header |   ✅    | Bearer token  | JWT token         |
| case_id         | query  |   ❌    | Valid case ID | Filter by case    |
| project         | query  |   ❌    | String        | Filter by project |
| limit           | query  |   ❌    | 1-100         | Results limit     |

Function Stack:
1. Authentication: Validate user token
2. Permission check: User can access co-driver data
3. Query building: Apply filters (case_id, project)
4. Data retrieval: Fetch from `rag_codriver` table
5. Response shaping: Format results with metadata

Response Format:

```json
{
  "data": [
    {
      "id": 1,
      "case_id": 5,
      "project": "Go-Driver Beta",
      "content": "AI session summary...",
      "created_at": "2025-10-10T10:00:00Z"
    }
  ],
  "meta": {
    "total": 1,
    "limit": 25,
    "page": 1
  }
}
```

#### 6.5.2 POST /rag_codriver (#38)
Status: Draft | Access: User (Token Required)

| Field         | Type    | Required | Validation      | Notes           |
|---------------|---------|----------|-----------------|-----------------|
| Authorization | header  |   ✅    | Bearer token    | JWT token       |
| content       | text    |   ✅    | Max 10000 chars | Session content |
| case_id       | integer |   ❌    | Valid case ID   | Link to case    |
| project       | string  |   ❌    | Max 100 chars   | Project name    |
| tags          | array   |   ❌    | Max 10 tags     | Classification  |

Function Stack:
1. Authentication: Validate user token
2. Input validation: Required fields, data limits
3. Permission check: User can create co-driver sessions
4. Case validation: If case_id provided, verify access
5. Data insertion: Create record in `rag_codriver`
6. AI processing: Queue for vector embedding (async)
7. Response: Return created object with ID

Compliance Logic:
- ✅ Content length limits prevent abuse
- ✅ Case access verified before linking
- ✅ Audit trail for all creations


#### 6.5.3 GET /rag_codriver/{rag_codriver_id} (#36)
**Status: Draft | Access: User (Token Required)

| Field           | Type    | Required | Validation        | Notes               |
|-----------------|---------|----------|-------------------|---------------------|
| Authorization   | header  |   ✅    | Bearer token      | JWT token           |
| rag_codriver_id | path    |   ✅    | Valid ID, integer | Co-driver record ID |

Function Stack:
1. Authentication: Validate user token
2. Permission check: User can access specific co-driver record
3. Data retrieval: Get single record from `rag_codriver` table
4. Ownership validation: Verify user has access to this session
5. Response shaping: Return full record details

Response Format:

```json
{
  "id": 1,
  "case_id": 5,
  "project": "Go-Driver Beta",
  "content": "Detailed AI session content...",
  "tags": ["productivity", "ai-assistant"],
  "created_at": "2025-10-10T10:00:00Z",
  "updated_at": "2025-10-10T11:30:00Z",
  "ai_summary": "Session focused on case analysis...",
  "visibility": "private"
}
```

Compliance Logic:
- ✅ User can only access their own sessions or shared ones
- ✅ Case-linked sessions require case access permissions
- ✅ Audit log for record access


#### 6.5.4 PATCH /rag_codriver/{rag_codriver_id} (#39)
Status: Draft | Access: User (Token Required)

| Field           | Type   | Required | Validation          | Notes                   |
|-----------------|--------|----------|---------------------|-------------------------|
| Authorization   | header |   ✅    | Bearer token        | JWT token               |
| rag_codriver_id | path   |   ✅    | Valid ID, integer   | Co-driver record ID     |
| content         | text   |   ❌    | Max 10000 chars     | Updated session content |
| project         | string |   ❌    | Max 100 chars       | Updated project name    |
| tags            | array  |   ❌    | Max 10 tags         | Updated classification  |
| visibility      | string |   ❌    | private/shared/team | Updated access control  |

Function Stack:
1. Authentication: Validate user token
2. Record lookup: Get existing record from `rag_codriver`
3. Permission check: User can edit this specific record
4. Input validation: Validate updated fields
5. Data update: Update record with new values
6. AI processing: Re-queue for vector embedding if content changed
7. Audit log: Record modification details
8. Response: Return updated object

Response Format:

```json
{
  "id": 1,
  "case_id": 5,
  "project": "Go-Driver Beta - Updated",
  "content": "Updated AI session content...",
  "tags": ["productivity", "ai-assistant", "updated"],
  "updated_at": "2025-10-10T14:00:00Z",
  "ai_summary": "Updated session summary...",
  "message": "Co-driver session updated successfully"
}
```

Compliance Logic:
- ✅ Only record owner or Admin can modify
- ✅ Version history maintained for audit
- ✅ AI re-processing triggered for content changes
- ✅ Case permissions re-validated if case_id changed


#### 6.5.5 DELETE /rag_codriver/{rag_codriver_id} (#40)
Status: Draft | Access: User (Token Required) | Special Role Check

| Field           | Type   | Required | Validation        | Notes               |
|-----------------|--------|----------|-------------------|---------------------|
| Authorization   | header |   ✅    | Bearer token      | JWT token           |
| rag_codriver_id | path   |   ✅    | Valid ID, integer | Co-driver record ID |

Function Stack:
1. Authentication: Validate user token
2. User lookup: Get user record (return as: user1)
3. Precondition: user1 != null
4. Role lookup: Get user_app_role record (return as: user_app_role1)
5. Precondition: (user_app_role1.role_id = 6 OR user_app_role1.role_id = 20)
   - Role 6 = Admin
   - Role 20 = AI Co-Driver
6. Record deletion: Delete record from `rag_codriver`
7. Audit log: Record deletion with user context
8. Response: Return user1 object

Response Format:

```json
{
  "user": {
    "id": 1,
    "name": "John Doe",
    "role": "Admin"
  },
  "message": "Co-driver session deleted successfully",
  "deleted_id": 1,
  "deleted_at": "2025-10-10T15:00:00Z"
}
```

Compliance Logic:
- ⚠️ RESTRICTED: Only Admin (role_id=6) or AI Co-Driver (role_id=20) can delete
- ✅ Soft delete preferred - mark as deleted rather than hard delete
- ✅ Full audit trail with user identification
- ✅ Case-linked sessions may require additional approval
- ✅ Cannot be undone - requires confirmation

---




**This single file merges backend, API, logic, role/module content, and design/UX blueprint for Go-Driver & Photini Family. Always update this with schema or role/module changes for ongoing compliance and technical development.**






