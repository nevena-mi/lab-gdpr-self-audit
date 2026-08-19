
## Data Processing Brief

DrugDev-AI is an AI-powered learning and regulatory intelligence assistant for pharmaceutical professionals. It provides three integrated workflows: Ask, Learn, and Monitor, combining grounded question answering, structured learning, and live monitoring of official regulatory sources.

**DrugDev-AI does process personal data.** 

The personal data comes primarily from information that users voluntarily enter while asking questions or using the learning features. This matters for GDPR because some of this information can describe or be linked to an identifiable individual, even though the application is not designed to collect identity documents, health records, or other highly sensitive information.

The regulatory knowledge base and live regulatory sources are different: they consist of publicly available regulatory and scientific information and are not used to build profiles about individual users.

### What personal data does the system process?

The current MVP processes limited personal data voluntarily provided by users:

- Questions entered in Ask mode
- Learning onboarding information, including professional background, career goals, prior experience, preferred learning style, and available study time
- Conversation history within the active session
- Learning progress within the application session
- Quiz answers and quiz results
- Optional Monitor search keywords

| Personal data | Why it is personal data |
| --- | --- |
| User questions | Questions may contain information that directly or indirectly identifies the user or describes their professional or personal circumstances. |
| Learning profile | Background, career goals, experience, learning preferences, and available study time describe an identifiable individual. |
| Conversation history | Previous interactions can be linked to an individual user and therefore constitute personal data. |
| Learning progress | Progress through modules reflects an individual's educational activity and therefore relates to an identifiable person. |
| Quiz answers and results | Quiz activity describes an individual's learning performance and can therefore relate to an identifiable person. |
| Monitor search keywords | Search terms may reveal professional interests, responsibilities, or areas of work and can therefore become personal data when linked to a user. |

The system does **not intentionally request special-category data under Article 9 GDPR**. However, because Ask mode accepts free-text questions, a user could voluntarily enter information about health, ethnicity, political opinions, religious beliefs, sexual orientation, trade-union membership, or another sensitive topic. Such information would then be processed by the system even though it was not requested. The production design should therefore warn users not to submit unnecessary sensitive personal information.

The regulatory knowledge base consists of publicly available regulatory documents and guidance from organizations such as the EMA, FDA, WHO, ICH, and the European Commission. These materials are used as regulatory reference content rather than as information about DrugDev-AI users.

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

For the current MVP and a future production deployment, the GDPR roles are understood as follows:

- **DrugDev-AI operator / production client — Controller:** determines why user information is processed and how DrugDev-AI is used.
- **Project developer — Processor when developing or maintaining the system for a separate client:** may process personal data only on the client's documented instructions. If the developer operates DrugDev-AI directly as their own service, the developer would instead act as controller.
- **OpenAI — Processor:** receives relevant user prompts and learning content for language generation and receives text requiring embeddings where applicable.
- **Cohere — Processor:** receives the search query and retrieved text required for neural reranking.
- **Pinecone — Processor:** stores the regulatory vector index and receives query vectors and retrieval requests.
- **ClinicalTrials.gov, openFDA, and EMA — Public information sources:** Monitor queries these services for public regulatory information. Based on the current design, they are not used to process DrugDev-AI user profiles.

Article 28 Data Processing Agreements would need to be verified or established with vendors acting as processors before production processing of personal data.

### Storage, Processing and International Transfers

#### Storage

The current MVP runs locally through Streamlit. Conversation history, learning profile information, quiz state, and learning progress are maintained for the active application session and are not intentionally persisted as long-term user records.

The regulatory knowledge base is stored in Pinecone as vectorized regulatory text. It is built from public regulatory documents rather than user data.

The current project documentation does not establish the exact production hosting region for every third-party provider. This is therefore a **documented compliance gap to verify before production deployment**, rather than an assumption that all processing occurs either inside or outside the EEA.

#### Processing

Different data is sent to different services:

- **OpenAI:** receives user questions and relevant retrieved context when generating answers, lessons, quizzes, or other AI-generated content. It also processes text submitted for embeddings.
- **Cohere:** receives search queries and candidate regulatory text required for reranking.
- **Pinecone:** processes query vectors against the regulatory knowledge base.
- **Monitor sources:** ClinicalTrials.gov, openFDA, and EMA receive search terms used to retrieve public regulatory information.

#### International transfers

The exact international-transfer status of OpenAI, Cohere, and Pinecone is **TBD — vendor region and contractual configuration must be verified before production**.

If personal data is transferred outside the EEA, the controller must identify and document the applicable GDPR Chapter V transfer mechanism, such as:

- an adequacy decision, including relevant participation in the EU–US Data Privacy Framework where applicable; or
- Standard Contractual Clauses (SCCs), together with any required transfer risk assessment and supplementary measures.

The production system should not assume that use of a cloud API automatically provides an adequate transfer mechanism.

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

| Purpose | Proposed lawful basis | Justification | Flag for legal review? |
| --- | --- | --- | --- |
| Answer user questions | Article 6(1)(b) – Contract | The user explicitly requests the Ask service. Processing the submitted question and relevant conversation context is necessary to generate and return the requested answer. | No |
| Generate personalized learning path | Article 6(1)(b) – Contract, provisionally | The user explicitly requests a personalized learning experience and provides onboarding information for that purpose. Only information necessary to select and adapt the learning path should be processed. | **Yes — confirm contractual necessity** |
| Generate lessons and quizzes | Article 6(1)(b) – Contract | Generation and assessment are core functions the user requests when choosing Learn mode. Processing the active module, lesson content, and quiz responses is necessary to provide that functionality. | No |
| Maintain conversation memory within the active session | Article 6(1)(b) – Contract, provisionally | Short-term session memory is used only to maintain continuity in the conversation requested by the user. Persistent or cross-session memory would require a separate assessment. | **Yes — confirm contractual necessity** |
| Retrieve Monitor results | Article 6(1)(b) – Contract | Search keywords supplied by the user are necessary to execute the Monitor search they explicitly requested. | No |
| Runtime token and cost monitoring | Article 6(1)(f) – Legitimate Interests | Limited operational metadata is used to monitor API consumption, system reliability, and costs. It is not intended for behavioural profiling or decision-making about users. | **TBD – LIA/legal review required** |

### Legitimate Interests Assessment

#### Legitimate interest — purpose test

The provider has a concrete business interest in understanding API consumption, identifying unexpected operational costs, monitoring system reliability, and maintaining a sustainable service. These activities support the operation and security of DrugDev-AI rather than advertising or behavioural profiling.

#### Necessity test

The runtime cost tracker records limited technical information such as timestamp, application mode, model, operation, token counts, search units, and estimated cost. It does not need to store the user's question, generated answer, learning profile, or other content.

Collecting this limited metadata is a proportionate way to determine how much individual system operations cost and to identify abnormal usage. The same cost-management purpose could not be achieved with equivalent accuracy without recording some request-level operational information.

#### Balancing test

The impact on users is low because the analytics are limited to technical usage metadata and are not used to evaluate users, infer personal characteristics, advertise to them, or determine access to the service. The data collected should nevertheless be minimized, protected by retention limits, and described in the privacy notice.

If analytics later expand to behavioural analytics, user-level profiling, product experimentation, or persistent tracking across sessions, this balancing assessment would no longer be sufficient and the lawful basis should be reassessed.

#### Conclusion

Article 6(1)(f) appears to be a plausible lawful basis for the narrowly defined runtime cost and operational analytics described above. A formal LIA and legal review should be completed before production deployment.

---
---

## Risk and Rights Analysis

### Special-category data (Article 9)

DrugDev-AI is not intended to process special-category data. Users may voluntarily enter health or employment-related information in free-text prompts, but the application neither requests nor analyses such information for profiling. No Article 9 processing is intentionally performed.
Because Ask mode accepts unrestricted free-text input, incidental Article 9 data cannot be fully prevented. A user could, for example, include information about a medical condition when asking a regulatory question. The production interface should therefore inform users not to submit unnecessary sensitive personal data, and such information should not be reused for profiling, analytics, or unrelated purposes.

### Automated decision-making (Article 22)

The system does not make automated decisions with legal or similarly significant effects. It provides educational recommendations and explanations only. Users remain responsible for all professional decisions, and there is no automated approval, rejection, scoring, or profiling affecting legal rights.

### DPIA trigger

The current MVP does not appear to trigger a mandatory DPIA based on the information available.

The EDPB criteria can be applied as follows:

| EDPB Criterion | Present? | Reason |
| --- | --- | --- |
| Evaluation or scoring of people | ❌ | DrugDev-AI does not score or rank users as individuals. Quiz evaluation measures learning responses but does not produce a consequential personal assessment. |
| Automated decision-making with legal or similarly significant effects | ❌ | The system provides information and learning recommendations only. It does not approve, reject, or determine access to employment, healthcare, credit, education, or another significant service. |
| Systematic monitoring | ❌ | Users are not continuously monitored or tracked across contexts in the current MVP. |
| Special-category or highly personal data | ❌ / potential incidental occurrence | DrugDev-AI does not request Article 9 data, although users could voluntarily include such information in free-text prompts. |
| Large-scale processing | ❌ | The MVP does not currently operate at a scale that indicates large-scale processing of personal data. |
| Matching or combining datasets | ❌ | The system does not combine separate personal datasets about users. |
| Data concerning vulnerable data subjects | ❌ | The intended users are pharmaceutical professionals, researchers, and learners; the system is not specifically targeted at vulnerable groups. |
| Innovative use of technology | ✔ | The application combines generative AI, retrieval, reranking, and personalized learning. |
| Processing that prevents individuals exercising a right or using a service or contract | ❌ | DrugDev-AI does not determine access to a service or prevent users from exercising their rights. |

Only **one criterion — innovative use of technology — is clearly present** for the current MVP. Incidental entry of special-category data remains a possible risk, but it is neither requested nor part of the intended processing purpose.

International transfers to third-party AI vendors are a separate GDPR compliance consideration. They increase overall privacy risk but are **not one of the nine EDPB DPIA criteria** and should therefore not be counted toward the two-criterion DPIA threshold.

On the current facts, a mandatory DPIA is therefore unlikely. This conclusion should be reassessed if DrugDev-AI introduces persistent user profiles, behavioural or learning analytics, large-scale deployment, systematic monitoring, profiling, or processing of special-category data.

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