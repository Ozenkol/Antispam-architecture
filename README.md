# Summary: Konstantin Khitsko's Talk on Mail.ru Anti-Spam System

## 1. Problem Background
- **Mail.ru** processes **400 million emails/day**, 10–15% of which are spam.
- **Types of spam**:
  - Unethical bulk mailings (stolen email lists),
  - Fraud (Nigerian scams, phishing),
  - Phishing (stealing credentials).
- **Challenges**:
  - Up to **1 million emails/sec** peak load,
  - Varying email sizes (KB to tens of MB),
  - Spammers adapt (hide spam in images, PDFs, external links),
  - **Noisy feedback**: users often mark legitimate mailings as spam.

## 2. Requirements
- **Processing speed**: ≤350 ms per email (queues grow otherwise),
- **Scalability**: must handle peak traffic,
- **Flexibility**: fast rule deployment (dozens of updates/day),
- **Reliability**: fault tolerance (monthly data center failure drills).

## 3. System Modeling
- **Spam detection** = ongoing arms race.
- **Mathematical models**:
  - **Reputation systems** (tracking sender history),
  - **ML models**:
    - Linear models → Transformer-based models,
    - Regular retraining due to spammers' evolving tactics.
  - **Rules/Heuristics**: 6000+ rules applied selectively based on email features.

## 4. Architecture Influence
- **Built on Kubernetes**: 150+ services, 4000+ pods.
- **Key components**:
  - Load balancer,
  - Email parser (extracts text, headers, URLs, images),
  - Business logic layer (Lua-based rules + ML models),
  - Databases (reputation lists, email history),
  - Monitoring and auto-scaling.

## 5. Engineering Approaches
- **Fault tolerance**:
  - Failover routing,
  - Monthly disaster recovery tests.
- **Speed optimization**:
  - Parallel feature extraction,
  - Caching reputation data.
- **Fighting spammers**:
  - OCR to read hidden text in images,
  - Behavioral analysis (bots vs real users).

## 6. Solution Optimality
- **Strengths**:
  - Handles **20 billion spam emails/year**,
  - High flexibility: quick rule/model updates,
  - Scales efficiently under load.
- **Weaknesses**:
  - Some false positives (requires manual support intervention),
  - High infrastructure costs (4000+ pods).

## 7. Conclusion
- **Mail.ru's anti-spam system** combines ML, heuristics, and reputation scoring.
- **Main challenge**: spammers adapt quickly, but system reduces their profitability.
- **Future direction**: more NLP and computer vision (spam hiding in images/PDFs).

### Personal thoughts:
- The system's scale is impressive, but managing **6000+ rules** poses risks.
- Curious why **blockchain-based sender verification** (as proposed by Bill Gates) isn’t used.
- Spam will never fully disappear, but can be made less profitable.

### Open questions:
- How often do **false positives** impact major senders (e.g., banks)?
- Why not apply **harsher methods** (e.g., automatic IP banning)?
- What is the **current share** of emails processed by transformers vs older linear models?
- Would be nice to see **more metrics** on ML models' effectiveness.
