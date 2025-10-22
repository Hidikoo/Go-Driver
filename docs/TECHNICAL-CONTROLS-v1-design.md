## **Navigation**

🏠 **[← Back to Main README](https://github.com/Hidikoo/Photini-Go-Driver)**  
📘 **[Full Documentation (HTML)](https://hidikoo.github.io/Photini-Go-Driver/)**  
📋 **[Compliance Principles](./COMPLIANCE_PRINCIPLES.md)**  
🗺️ **[Roadmap](./roadmap.md)** | **[Vision](./roadmap-vision.md)**  
🎴 **[Flashcards & Q&A](./Go-Driver%20Flashcards%20%26%20QnA.txt)**

---
# **Technical Controls Implementation Guide**
**Photini Family Ethics & Compliance Framework**  
**Version 1.0 – October 2025 (Design Demo for Feedback)**

---

## **Executive Overview**

This document translates Photini Family's Higher-Level Principles ([COMPLIANCE_PRINCIPLES.md](./COMPLIANCE_PRINCIPLES.md)) into **actionable technical controls** embedded directly into the Photini's Go-Driver platform architecture. This is **Compliance-by-Design in practice**—not a checklist, but living code that audits itself.

**This is a design peek:** We're publishing this early to invite feedback from developers, compliance professionals, and NGO partners on our technical approach.

---

## **Part 1: Three-Layer Integration Architecture**

### **Layer 1: Database-Level Ethics (Schema as Contract)**

Your Xano database schema enforces compliance **before any business logic runs**. Every table includes built-in controls:

```sql
-- Example: PII Detection Field Added to Evidence Tables
ALTER TABLE evidence_files ADD COLUMN (
  pii_detection_score DECIMAL(3,2),  -- 0.00-1.00 confidence
  contains_pii BOOLEAN DEFAULT FALSE,
  redaction_status ENUM('unreviewed', 'cleared', 'redacted', 'quarantined'),
  audit_timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  audit_user_id FOREIGN KEY → users.id
);

-- Trigger: Flag for review if PII detected
CREATE TRIGGER pii_flag_evidence BEFORE INSERT ON evidence_files
  FOR EACH ROW
  IF NEW.pii_detection_score > 0.85 THEN
    NEW.redaction_status = 'needs_review';
    NEW.flagged_for_manager = TRUE;
  END IF;
```

**Principles Embedded:**
- **Privacy by Design:** PII scoring happens at database insert, not in application layer
- **Audit Trail:** Every change timestamped and user-attributed
- **Human-in-Loop:** Confidence thresholds trigger escalation (see Technical Annex A)

---

### **Layer 2: API-Level Access Controls**

Every Go-Driver API endpoint enforces role-based access control (RBAC) + consent verification:

```javascript
// Pseudo-code: Go-Driver API Gateway
async function getEvidenceFile(req, res) {
  const { evidenceId } = req.params;
  const { userId, roleId } = req.user;
  
  // Step 1: RBAC Check
  const userRole = await Role.findById(roleId);
  if (!userRole.permissions.includes('VIEW_EVIDENCE')) {
    return res.status(403).json({ error: 'Insufficient permissions' });
  }
  
  // Step 2: Consent Verification
  const evidenceCase = await EvidenceFile.findById(evidenceId);
  const consent = await Consent.findOne({
    caseId: evidenceCase.caseId,
    dataCategory: 'EVIDENCE_FILES',
    grantedByUserId: evidenceCase.originalOwner,
    status: 'ACTIVE'
  });
  
  if (!consent || consent.isExpired()) {
    // Automatically trigger re-consent workflow
    await RuleKeeper_AI.notifyConsentExpiry(evidenceCase.caseId);
    return res.status(403).json({ error: 'Consent expired' });
  }
  
  // Step 3: Audit Logging
  await AuditLog.create({
    action: 'EVIDENCE_ACCESS',
    userId: userId,
    resourceId: evidenceId,
    timestamp: new Date(),
    ipAddress: req.ip,
    userAgent: req.headers['user-agent']
  });
  
  // Step 4: PII Redaction (Dynamic)
  const file = await EvidenceFile.findById(evidenceId);
  if (file.redaction_status === 'redacted') {
    const redactedContent = await applyRedactionMask(file.content);
    return res.json({ content: redactedContent, redacted: true });
  }
  
  return res.json(file);
}
```

**Principles Embedded:**
- **RBAC + Consent:** Two-factor access control
- **Real-Time Audit Logging:** Every access recorded with context
- **Automatic Policy Enforcement:** Expired consent triggers workflows without delay

---

### **Layer 3: Application-Level AI Oversight**

Go-Driver's AI agents (ComplianceMonitor, RuleKeeper, etc.) run continuous compliance checks:

```javascript
// Pseudo-code: ComplianceMonitor AI Loop (runs every 15 minutes)
async function complianceMonitorLoop() {
  const fifteenMinutesAgo = new Date(Date.now() - 15 * 60 * 1000);
  
  // 1. FLAG RECENT ACCESS ANOMALIES
  const recentAccess = await AuditLog.find({
    timestamp: { $gte: fifteenMinutesAgo }
  });
  
  const anomalies = await detectAnomalies(recentAccess);
  // Anomalies: failed auth attempts, unusual access hours, role escalation, bulk downloads, etc.
  
  if (anomalies.length > 0) {
    await ManagerOffice_Dashboard.createAlert({
      type: 'SECURITY_ANOMALY',
      severity: 'HIGH',
      details: anomalies,
      autoReviewScore: 0.92,  // AI confidence
      requiresHumanApproval: true,
      escalationDeadline: new Date(Date.now() + 2 * 60 * 60 * 1000)  // 2 hours
    });
    
    // Alert founder immediately
    await sendSlackNotification(
      '#photini-compliance',
      `:warning: Compliance Alert\n${JSON.stringify(anomalies, null, 2)}`
    );
  }
  
  // 2. CHECK CONSENT EXPIRY SCHEDULE
  const expiringConsents = await Consent.find({
    expiryDate: {
      $gte: fifteenMinutesAgo,
      $lte: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)  // Next 7 days
    }
  });
  
  for (const consent of expiringConsents) {
    await initiateConsentRenewal(consent);
  }
  
  // 3. VALIDATE DATA RETENTION POLICIES
  const oldEvidence = await EvidenceFile.find({
    createdAt: { $lt: new Date(Date.now() - 7 * 365 * 24 * 60 * 60 * 1000) },  // 7 years old
    retentionStatus: 'ACTIVE'
  });
  
  if (oldEvidence.length > 0) {
    await ManagerOffice_Dashboard.createAlert({
      type: 'RETENTION_POLICY_REVIEW',
      affectedRecords: oldEvidence.length,
      requiresHumanDecision: true  // Cannot auto-delete
    });
  }
  
  // 4. LOG COMPLIANCE CYCLE COMPLETION
  await ComplianceRunLog.create({
    cycleStart: fifteenMinutesAgo,
    cycleEnd: new Date(),
    recordsReviewed: recentAccess.length,
    anomaliesDetected: anomalies.length,
    alertsCreated: anomalies.length + expiringConsents.length
  });
}
```

**Principles Embedded:**
- **Continuous Monitoring:** AI agent runs autonomously, no manual intervention needed
- **Escalation Protocol:** Human oversight for anomalies; alerts with deadlines
- **Immutable Audit Trail:** Every monitoring cycle logged

---

## **Part 2: Control Implementation Roadmap**

### **Q1 2026: Foundation Controls (MVP Phase)**

| Control | Technical Implementation | Threshold | Human Review |
|---------|-------------------------|-----------|--------------|
| **Authentication** | JWT + MFA (TOTP) | All users, all access | Failed MFA = 3 attempts = lock |
| **PII Detection** | Regex + NER model | Deploy for English, Arabic, Finnish | Score >0.85 = manual review |
| **Encryption at Rest** | AES-256 (Xano default) | All PII fields | Audit keys quarterly |
| **Access Logging** | JSON audit logs, write-once storage | Every API call | Bi-weekly log review |
| **Role-Based Access** | 20-role RBAC matrix | See Go-Driver Xano backend docs | New roles require sign-off |

**Deployment:** GitHub branch `compliance-foundation-q1` with integration tests for all controls.

---

### **Q2 2026: Advanced Controls (Pilot Testing)**

| Control | Enhancement | Threshold | Integration |
|---------|-------------|-----------|-------------|
| **AI Confidence Scoring** | BERT model fine-tuned on Go-Driver decisions | High-risk: ≥95%, Medium: ≥85%, Low: ≥70% | ComplianceMonitor AI review queue |
| **Anomaly Detection** | Isolation Forest + density analysis | ML model trained on 90 days baseline | Real-time alerts to dashboard |
| **Consent Automation** | GDPR Art. 7 & 17 workflows | Automatic expiry checks, 48h re-consent | Email + in-app notifications |
| **Trauma-Sensitive NLP** | Fine-tuned on trauma literature | Sentiment score <70% = editor review | Pre-delivery quality gate |

**Pilot:** 3 test cases from partner NGOs; refine thresholds based on real-world performance.

---

### **Q3 2026: Certification Prep Controls**

| Control | ISO Standard | Requirement | Evidence |
|---------|--------------|-------------|----------|
| **Security Governance** | ISO 27001:2022 | Document risk register, control test results | Quarterly audit reports |
| **Privacy Governance** | ISO 27701:2025 | Privacy impact assessments, consent templates | DPIA documentation |
| **AI Governance** | ISO 42001:2023 | AI model card, bias testing, incident response | Algorithm audit logs |

**Deliverable:** Full compliance audit package for external ISO assessor.

---

## **Part 3: Embedding Principles into Code**

### **Example 1: Trauma-Informed Language Filtering**

```javascript
// Pseudo-code: Trauma-Sensitive AI Communication Module
async function validateAIResponse(aiGeneratedText, context) {
  // Principle 2.4: Child Protection & Trauma Sensitivity
  
  const traumaScores = await TraumaLensAI.analyzeText(aiGeneratedText, {
    targetAudience: 'child_trauma_survivor',
    language: context.language,
    culturalContext: context.country
  });
  
  // Trauma lens checks:
  // - Avoid: victim-blaming language ("why didn't you...?")
  // - Avoid: coercion/threat language ("must", "forced to")
  // - Prefer: agency-affirming language ("you can", "you have options")
  // - Include: safety affirmations
  
  if (traumaScores.overallSensitivityScore < 0.70) {
    // Flag for human editor
    await ReviewQueue.create({
      queueType: 'TRAUMA_SENSITIVITY_REVIEW',
      content: aiGeneratedText,
      reasons: traumaScores.issues,
      recommendedRevisions: traumaScores.suggestions,
      editor: 'HumanFirst_AI'
    });
    
    return {
      approved: false,
      reason: 'Trauma sensitivity score below threshold',
      scoreDetails: traumaScores
    };
  }
  
  return { approved: true, score: traumaScores.overallSensitivityScore };
}
```

**Principle Embedded:** Section 2.4 (Child Protection & Trauma Sensitivity)

---

### **Example 2: Evidence Chain Integrity**

```javascript
// Pseudo-code: Immutable Evidence Audit Trail
async function addEvidenceToChain(fileData, caseId, addedBy) {
  // Principle 6.4: Audit Trail Requirements (7-year retention)
  
  const fileHash = crypto.createHash('sha256')
    .update(fileData.content)
    .digest('hex');
  
  const chainEntry = {
    evidenceId: uuid.v4(),
    fileHash: fileHash,
    timestamp: new Date(),
    addedBy: addedBy,
    caseId: caseId,
    previousHash: await getLastChainEntry(caseId),  // Blockchain-style linking
    metadata: {
      fileName: fileData.name,
      fileSize: fileData.size,
      mimeType: fileData.mimeType,
      checksumAlgorithm: 'SHA-256'
    }
  };
  
  // Write to immutable storage (write-once append-only log)
  await WriteOnceStorage.append('evidence_chain', chainEntry);
  
  // Verify integrity
  const verification = await verifyChainIntegrity(caseId);
  if (!verification.isValid) {
    await ManagerOffice_Dashboard.createAlert({
      type: 'EVIDENCE_CHAIN_CORRUPTION',
      severity: 'CRITICAL',
      affectedCase: caseId,
      requiresInvestigation: true
    });
  }
  
  return chainEntry;
}

// Quarterly integrity audit
async function quarterlyEvidenceIntegrityAudit() {
  const allCases = await Case.find({});
  const corruptedCases = [];
  
  for (const caseRecord of allCases) {
    const verification = await verifyChainIntegrity(caseRecord.id);
    if (!verification.isValid) {
      corruptedCases.push({
        caseId: caseRecord.id,
        corruptionDetails: verification.errors,
        discoveredAt: new Date()
      });
    }
  }
  
  if (corruptedCases.length > 0) {
    await InternalAudit_AI.reportToFounder({
      audit: 'QUARTERLY_EVIDENCE_INTEGRITY',
      status: 'FAILED',
      details: corruptedCases,
      requiresImmediateAction: true
    });
  }
}
```

**Principle Embedded:** Section 6.4 (Audit Trail Requirements)

---

### **Example 3: Human-in-Loop Decision Override**

```javascript
// Pseudo-code: Emergency Override Protocol
async function overrideAIRecommendation(caseId, recommendationId, managerId, reason) {
  // Principle 6.5: Emergency Override Protocols
  
  const recommendation = await AIRecommendation.findById(recommendationId);
  
  // Verify manager authority
  const manager = await User.findById(managerId);
  if (!manager.permissions.includes('OVERRIDE_AI_DECISIONS')) {
    throw new Error('Insufficient permissions');
  }
  
  // Log the override
  const overrideLog = await OverrideLog.create({
    recommendationId: recommendationId,
    originalAIRecommendation: recommendation.content,
    overriddenBy: managerId,
    overrideReason: reason,
    timestamp: new Date(),
    immediateEffect: true
  });
  
  // Analyze pattern
  const recentOverrides = await OverrideLog.find({
    caseId: caseId,
    timestamp: {
      $gte: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000)  // Last 7 days
    }
  });
  
  if (recentOverrides.length >= 3) {
    // ≥3 overrides in 7 days = systemic issue
    await InternalAudit_AI.createAlert({
      type: 'HIGH_OVERRIDE_FREQUENCY',
      caseId: caseId,
      pattern: {
        overridesInLastWeek: recentOverrides.length,
        averageReasonText: recentOverrides.map(o => o.overrideReason)
      },
      suggestion: 'Review AI model confidence thresholds or case complexity',
      requiresEthicsReview: true
    });
  }
  
  return overrideLog;
}
```

**Principle Embedded:** Section 6.5 (Emergency Override Protocols)

---

## **Part 4: Testing & Validation Strategy**

### **Unit Tests: Control-Level Validation**

```javascript
describe('Compliance Controls - PII Detection', () => {
  test('should flag text containing personal identifiers', async () => {
    const testText = "Child: John Doe, Age 7, Address: 123 Main St, Phone: +358-123456";
    const result = await piiDetector.scan(testText);
    
    expect(result.isPII).toBe(true);
    expect(result.detectedEntities).toContain('PERSON_NAME');
    expect(result.detectedEntities).toContain('PHONE_NUMBER');
    expect(result.confidenceScore).toBeGreaterThan(0.85);
  });

  test('should NOT flag fictional examples', async () => {
    const testText = "Example case: A fictional child named Alex, age 5.";
    const result = await piiDetector.scan(testText);
    
    expect(result.isPII).toBe(false);
  });
});
```

### **Integration Tests: End-to-End Workflow**

```javascript
describe('Compliance Workflow - Evidence Upload to Audit Trail', () => {
  test('complete evidence lifecycle with audit logging', async () => {
    // 1. Upload evidence
    const uploadResult = await evidenceService.uploadFile(
      testFile,
      caseId,
      userId
    );
    
    // 2. Verify PII detection ran
    const piiCheck = await database.query(
      'SELECT * FROM evidence_audit WHERE evidenceId = ?',
      [uploadResult.id]
    );
    expect(piiCheck.pii_detection_score).toBeDefined();
    
    // 3. Verify audit log created
    const auditLog = await AuditLog.findOne({
      action: 'EVIDENCE_UPLOAD',
      resourceId: uploadResult.id
    });
    expect(auditLog).toBeDefined();
    expect(auditLog.timestamp).toBeCloseTo(new Date(), 1000); // Within 1 second
    
    // 4. Verify immutable chain entry
    const chainEntry = await WriteOnceStorage.getLatest('evidence_chain');
    expect(chainEntry.evidenceId).toBe(uploadResult.id);
  });
});
```

---

## **Part 5: Continuous Compliance Dashboard (Go-Driver Manager's Office)**

The Manager's Office displays real-time compliance metrics:

```
┌─────────────────────────────────────────────────────────────┐
│                  COMPLIANCE DASHBOARD                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  📊 METRICS (Real-time, last 24 hours)                     │
│  ├─ Total API Calls: 12,453                               │
│  ├─ Failed Auth Attempts: 3 (blocked after 3)             │
│  ├─ PII Detected & Flagged: 5                             │
│  ├─ AI Recommendations Overridden: 2                      │
│  └─ Audit Log Entries: 12,453                             │
│                                                             │
│  ⚠️  ALERTS (Requiring Action)                              │
│  ├─ [HIGH] Access Anomaly Detected                         │
│  │   User: case_manager_12 | Time: 2025-10-22 11:15 UTC  │
│  │   Pattern: 847 evidence downloads in 3 minutes         │
│  │   Action: LOCKED | Review Deadline: 2 hours            │
│  │   [REVIEW] [OVERRIDE] [DISMISS]                        │
│  │                                                          │
│  ├─ [MEDIUM] Consent Expiry - 5 Cases                     │
│  │   Re-consent Required by: 2025-10-25                   │
│  │   Status: Reminders sent (3/5 renewed)                 │
│  │   [AUTO-RENEW] [NOTIFY]                                │
│  │                                                          │
│  └─ [LOW] ISO 27001 Control Testing Due                   │
│     Next Quarterly Audit: 2025-12-22                      │
│     [SCHEDULE] [VIEW PLAN]                                │
│                                                             │
│  🔄 COMPLIANCE CYCLES                                      │
│  ├─ Last 15-min ComplianceMonitor Cycle: ✅ 14:45 UTC     │
│  ├─ Last 24h Trauma Sensitivity Scan: ✅ 2025-10-21       │
│  └─ Last Evidence Chain Verification: ✅ 2025-10-20       │
│                                                             │
│  📈 TREND (Last 30 Days)                                  │
│  ├─ PII Detection Rate: ↓ 12% (improving)                 │
│  ├─ AI Override Rate: ↑ 4% (watch trend)                  │
│  └─ Consent Renewal Rate: ↑ 98% (excellent)              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## **Part 6: Public Transparency & Feedback**

**This is a design peek—we want your input!**

**Questions we're seeking feedback on:**

1. **Threshold Appropriateness:** Are the AI confidence scores (≥95%, ≥85%, ≥70%) realistic for your use cases?

2. **Audit Frequency:** Is 15-minute compliance monitoring too frequent (performance concern) or too infrequent?

3. **Human Override Patterns:** Should we flag <1 override per case as suspicious (maybe AI is too conservative)?

4. **Trauma Sensitivity Scoring:** How should we weight cultural context vs. universal trauma principles?

5. **Data Retention:** Is 7 years the right duration, or should some case types retain longer?

6. **False Positive Tolerance:** What's acceptable false positive rate for PII detection before it becomes friction?

**Share feedback:** [hello@photini.family](mailto:hello@photini.family) or [GitHub Issues](https://github.com/Hidikoo/Photini-Go-Driver/issues)

---

## **Next Steps**

- **Q1 2026:** Foundation controls deployment
- **Q2 2026:** Pilot testing with NGO partners
- **Q3 2026:** ISO auditor review & certification prep
- **Q4 2026:** Public compliance certifications + transparency reports

---

**Continuously Updated:** This document reflects our evolving understanding. Check back quarterly for updates.

**Version History:**
- v1.0 (Oct 22, 2025) - Initial design peek
- v1.1 (Jan 2026, planned) - Post-pilot refinements

---

**© 2025 Photini Family™**  
📧 [hello@photini.family](mailto:hello@photini.family)  
🌐 [www.photini.family](https://www.photini.family)  
💻 [GitHub: Photini-Go-Driver](https://github.com/Hidikoo/Photini-Go-Driver)

## **Navigation**

🏠 **[← Back to Main README](https://github.com/Hidikoo/Photini-Go-Driver)**  
📘 **[Full Documentation (HTML)](https://hidikoo.github.io/Photini-Go-Driver/)**  
📋 **[Compliance Principles](./COMPLIANCE_PRINCIPLES.md)**  
🗺️ **[Roadmap](./roadmap.md)** | **[Vision](./roadmap-vision.md)**  
🎴 **[Flashcards & Q&A](./Go-Driver%20Flashcards%20%26%20QnA.txt)**

---
