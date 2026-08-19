 ## Data Processing Brief

DrugDev-AI is an AI-powered learning and regulatory intelligence assistant for pharmaceutical professionals. It provides three integrated workflows: Ask, Learn, and Monitor, combining Retrieval-Augmented Generation (RAG), structured learning, semantic search, neural reranking, and live monitoring of official regulatory sources.

### What personal data does the system process?

The current MVP processes only limited personal data voluntarily provided by users:

Questions entered in Ask mode
Learning onboarding information (professional background, career goals, prior experience, preferred learning style, available study time)
Conversation history within the active session
Learning progress within the application session
Optional Monitor search keywords

| Personal data           | Why it is personal data                                                                                                            |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| User questions          | Questions may contain information that directly or indirectly identifies the user.                                                 |
| Learning profile        | Background, career goals, experience, learning preferences, and available study time describe an identifiable individual.          |
| Conversation history    | Previous interactions can be linked to an individual user and therefore constitute personal data.                                  |
| Learning progress       | Progress through modules reflects an individual's educational activity and therefore relates to an identifiable person.            |
| Monitor search keywords | Search terms may reveal professional interests or work responsibilities and can therefore be linked to an identifiable individual. |


The regulatory knowledge base consists entirely of publicly available regulatory documents and guidance from organizations such as the EMA, FDA, WHO, ICH and the European Commission. These documents do not contain personal data and therefore are not considered personal data under GDPR.

### Where does the data come from?

All personal information originates directly from the user through the Streamlit interface. Regulatory documents originate from official public sources, while Monitor retrieves publicly available regulatory updates through official APIs and RSS feeds.

### What is the data used for?

Answer regulatory questions
Personalize learning recommendations
Generate lessons and quizzes
Maintain conversation context
Retrieve relevant regulatory updates
Improve user experience during the active session

### Data Controllers and Processors

Personal data is processed by:

DrugDev-AI application
OpenAI (language generation and embeddings)
Cohere (neural reranking)
Pinecone (vector search)
Official regulatory APIs (Monitor mode)

### Storage, Processing and International Transfers

Storage

The application itself runs locally through Streamlit. Conversation data is intended to exist only for the active session. Regulatory documents are stored in the application's vector database (Pinecone).

Processing

User prompts are transmitted to OpenAI for language generation and embeddings, Cohere for reranking, and Pinecone for semantic retrieval.

International transfers

Depending on the deployment region and vendor configuration, OpenAI, Cohere, and Pinecone may process data outside the European Economic Area. Before production deployment, the applicable international transfer mechanism (such as Standard Contractual Clauses or participation in the EU–US Data Privacy Framework) should be verified.

### Does the system make decisions affecting people?

No.

DrugDev-AI does not score people, rank individuals, approve or reject requests, or make automated decisions. It provides educational explanations, grounded answers, learning recommendations and regulatory summaries. Users remain fully responsible for interpreting information and making any professional decisions.

---
---

## Personal Data Inventory

| Data category                                                | Source | Purpose(s)                      | Retention period (known/estimated) | Crosses EU border? |
| ------------------------------------------------------------ | ------ | ------------------------------- | ---------------------------------- | ------------------ |
| User questions                                               | User   | Generate grounded answers       | Session only                       | Yes (OpenAI API)   |
| Learning profile (background, goals, experience, study time) | User   | Personalize learning path       | Session only                       | Yes                |
| Conversation history                                         | User   | Maintain conversational context | Session only                       | Yes                |
| Quiz responses                                               | User   | Evaluate learning progress      | Session only                       | Yes                |
| Monitor search keywords                                      | User   | Retrieve regulatory updates     | Session only                       | Possibly           |


Purpose limitation

No evidence was identified that personal data collected for one purpose is reused for unrelated purposes. The onboarding information is used solely for personalization of the learning experience.

---
---

## Role Map

| Entity                                   | GDPR Role                                                | Processing activity                         | DPA required?       |
| ---------------------------------------- | -------------------------------------------------------- | ------------------------------------------- | ------------------- |
| **Client (DrugDev-AI operator)**         | Controller                                               | Determines purposes and means of processing | N/A                 |
| **Development team (Project developer)** | Processor during development / Controller if self-hosted | Development, testing and maintenance        | Internal governance |
| **OpenAI**                               | Processor                                                | Language generation and embeddings          | Yes                 |
| **Cohere**                               | Processor                                                | Neural reranking                            | Yes                 |
| **Pinecone**                             | Processor                                                | Vector database                             | Yes                 |




International transfers

Personal data may leave the EEA when transmitted to OpenAI, Cohere, or Pinecone depending on deployment location.

Transfer mechanism: TBD – verify Standard Contractual Clauses (SCCs), adequacy decision, or EU-US Data Privacy Framework participation before production deployment.

---
---

## Lawful Basis Assessment

| Purpose                              | Proposed lawful basis                  | Justification                                         | Legal review           |
| ------------------------------------ | -------------------------------------- | ----------------------------------------------------- | ---------------------- |
| Answer user questions                | Article 6(1)(b) – Contract             | The user requests an answer; processing is necessary to deliver the requested service         | No                     |
| Personalized learning                | Article 6(1)(b) – Contract             | Required to deliver the requested learning experience | No                     |
| Conversation memory                  | Article 6(1)(b) – Contract             | Necessary for conversational continuity               | No                     |
| Usage analytics and token monitoring | Article 6(1)(f) – Legitimate Interests | Necessary for service reliability, debugging and cost monitoring. Subject to Legitimate Interests Assessment     | **TBD – LIA required** |


### Legitimate Interests Assessment

Legitimate interest

The provider has a legitimate interest in monitoring API usage, system performance and operational costs to maintain a secure and reliable service.

Necessity Test

Token analytics and operational logs are the least intrusive way to understand service performance and prevent misuse. The same objective cannot reasonably be achieved without collecting limited operational metadata.

Balancing test

The privacy impact is low because analytics relate primarily to technical usage rather than profiling individuals. Nevertheless, retention limits and transparency should be documented.

Conclusion

TBD – Legal review. Article 6(1)(f) is likely appropriate, subject to legal review and documentation of the Legitimate Interests Assessment.

---
---

## Risk and Rights Analysis

### Special-category data (Article 9)

DrugDev-AI is not intended to process special-category data. Users may voluntarily enter health or employment-related information in free-text prompts, but the application neither requests nor analyses such information for profiling. No Article 9 processing is intentionally performed.

### Automated decision-making (Article 22)

The system does not make automated decisions with legal or similarly significant effects. It provides educational recommendations and explanations only. Users remain responsible for all professional decisions, and there is no automated approval, rejection, scoring, or profiling affecting legal rights.

### DPIA trigger

The current MVP does not appear to trigger a mandatory DPIA.

Potential EDPB criteria include:

Innovative technology ✔

Cross-border processing ✔

| EDPB Criterion                              | Present?                                             |
| ------------------------------------------- | ---------------------------------------------------- |
| Evaluation or scoring                       | ❌                                                    |
| Automated decision-making with legal effect | ❌                                                    |
| Systematic monitoring                       | ❌                                                    |
| Sensitive data                              | ❌                                                    |
| Large-scale processing                      | ❌                                                    |
| Matching datasets                           | ❌                                                    |
| Vulnerable data subjects                    | ❌                                                    |
| Innovative technology                       | ✔                                                    |
| International transfers                     | ✔ (not itself an EDPB criterion, but increases risk) |


Only one of the EDPB high-risk criteria (innovative technology) is clearly met. Cross-border processing increases compliance obligations but is not itself one of the nine EDPB criteria. On the information available, a mandatory DPIA is therefore unlikely for the current MVP, although this should be reconsidered if persistent user accounts, large-scale deployment, or profiling features are introduced.

The system does not evaluate people, perform large-scale profiling, process special-category data, or make automated decisions. A DPIA may become appropriate if future versions introduce persistent user accounts, detailed learning analytics, or enterprise monitoring.

### Data subject rights friction

Potential operational challenges include:

Right of access to conversation history if persistent accounts are introduced.
Right to erasure if conversation history becomes stored beyond the active session.
Transparency regarding transmission of prompts to third-party AI providers.

The current MVP minimizes these challenges by limiting persistent storage.

---
---

## Law Stacking Check

### AI Act cross-check

DrugDev-AI is most appropriately classified as a Limited Risk AI system. It generates AI-produced content and therefore primarily triggers Article 50 transparency obligations rather than Annex III high-risk requirements.

### ePrivacy

The current MVP does not use cookies, advertising trackers, or communication metadata. ePrivacy obligations are therefore not currently triggered.

### Data Act

Not applicable.

DrugDev-AI does not process connected-device or IoT data.

---
---

# Compliance Memo

To: Data Protection Officer / Legal Counsel

Subject: GDPR Readiness Assessment – DrugDev-AI

## Bottom line

Proceed with conditions.

DrugDev-AI presents a relatively low GDPR risk because it processes only limited user-provided personal data, does not perform profiling or automated decision-making, and primarily serves as an educational and decision-support tool. However, several governance measures should be implemented before production deployment.

## Top three actions

1. Establish Data Processing Agreements (DPAs) with all external AI providers.

Before processing production data, ensure that appropriate Article 28 GDPR agreements are in place with OpenAI, Cohere, and Pinecone, and verify international transfer mechanisms where applicable.

2. Publish a transparent privacy notice.

Inform users what personal information is processed, which third-party providers receive prompts, how long information is retained, and how users may exercise their GDPR rights.

3. Document operational governance.

Prepare a retention policy, a Legitimate Interests Assessment for operational analytics, and internal documentation describing data flows, security measures, and incident response procedures.

## Residual risks

Even after implementing these recommendations, several residual risks remain.

First, prompts sent to external AI providers may contain personal information voluntarily entered by users, making careful user guidance and contractual safeguards essential.

Second, international transfers remain dependent on third-party provider compliance and evolving legal requirements.

Third, future product features such as persistent user accounts, adaptive learning analytics, or enterprise deployment could substantially increase GDPR obligations and may require a Data Protection Impact Assessment.

Finally, although the application provides grounded answers with citations, users may still over-rely on AI-generated explanations without consulting the original regulatory documents. Continued emphasis on transparency, user education, and clear disclaimers will help reduce this risk and support responsible use of the system.

### What this memo is not

This assessment is a preliminary GDPR compliance review prepared for educational purposes. It is not a legal opinion, a formal Data Protection Impact Assessment, or evidence of regulatory compliance. Production deployment should include review by qualified legal counsel and, where appropriate, a Data Protection Officer.

---
---

## Reinforce

### Accountability Principle

| Documentation                            | Current status                    |
| ---------------------------------------- | --------------------------------- |
| Privacy Notice                           | ❌ Missing                         |
| Data Processing Agreements (DPAs)        | ❌ Required before production      |
| Records of Processing Activities (RoPA)  | ❌ Missing                         |
| Retention Schedule                       | ❌ Missing                         |
| Legitimate Interests Assessment (LIA)    | ⚠ Required for analytics          |
| Data Protection Impact Assessment (DPIA) | ✔ Not currently required for MVP  |
| Incident Response Procedure              | ❌ Missing                         |
| Security Documentation                   | ⚠ Partial                         |
| AI Transparency Notice                   | ✔ Planned under AI Act compliance |


### Data Flow Requiring Legal Review

The use of OpenAI, Cohere, and Pinecone introduces international processing considerations. Before providing definitive legal advice, a lawyer would need information regarding:

actual hosting regions,
applicable transfer mechanisms,
existence of Article 28 DPAs,
retention policies,
vendor subprocessors.

Preparing this information in advance would significantly simplify legal review.

---
---

## Stretch

### Data Protection by Design Checklist

| Design principle        | Current state                                                                            | Pass / Fail / Unknown |
| ----------------------- | ---------------------------------------------------------------------------------------- | --------------------- |
| Data minimisation       | Only user-entered information is processed; regulatory corpus contains no personal data. | **Pass**              |
| Purpose binding         | User inputs are used solely for answering questions, learning, and monitoring.           | **Pass**              |
| Access controls         | Local application; production access controls not yet defined.                           | **Unknown**           |
| Retention enforcement   | Session-based storage only; no documented deletion policy.                               | **Partial**           |
| Subject rights workflow | No documented process for handling GDPR requests.                                        | **Unknown**           |
| Incident response       | No documented breach notification procedure.                                             | **Unknown**           |



Recommended Improvements

The MVP already follows several privacy-by-design principles through limited data collection and session-based processing. Before production deployment, the project should add documented retention policies, formal access controls, procedures for responding to data subject requests, and an incident response process capable of meeting the GDPR's 72-hour breach notification requirement. These measures would significantly strengthen accountability while preserving the project's current privacy-conscious architecture.