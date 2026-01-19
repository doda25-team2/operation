# Continuous Experimentation

### Experiment

Since new features are being tested for the application, the most suitable experiment that can be conducted would be the A/B Testing. A/B testing is when there are two different versions of the application in order to determine which is the best. Usually, a new feature is shown in one version. Based on this, A/B Testing is the most suitable since it allows to determine which feature would be the best [1].

### Changes
The `canary` version of the `app` service introduces a redesigned user interface. This experiment is designed to test the impact of this new UI on user behavior.

### Falsifiable Hypothesis
We hypothesize that the new UI in the `canary` version will lead to a positive change in user engagement. Specifically, we predict that users interacting with the new UI will write longer, more detailed messages compared to users of the `stable` version.

This hypothesis is falsifiable: by measuring and comparing user engagement metrics between the two versions, we can determine if the new UI has the desired effect.

### Relevant Metrics
To validate our hypothesis, we will monitor the following key metrics. The `version` label will differentiate between the `stable` and `canary` deployments.

1.  **Primary Metric: Message Length**
    *   **Metric:** `sms_message_length_chars` (Histogram)
    *   **Purpose:** To measure the primary goal of improving user engagement by tracking the length of submitted messages.

2.  **Guardrail Metric: Submission Rate**
    *   **Metric:** `rate(sms_classification_requests_total[5m])` (Counter)
    *   **Purpose:** To ensure the new UI does not negatively impact the overall volume of user submissions.

3.  **Guardrail Metric: Response Time**
    *   **Metric:** `histogram_quantile(0.95, rate(sms_classification_duration_milliseconds_bucket[5m]))`
    *   **Purpose:** To ensure the new UI does not introduce any performance regressions.

### Decision Process
The A/B test will be conducted over a pre-defined monitoring period. During this time, a subset of traffic will be routed to the `canary` version.

**Monitoring:** A Grafana dashboard will be used to visualize and compare the primary and guardrail metrics for both `stable` and `canary` versions.

**Decision Criteria:**
The decision to promote or roll back the `canary` will be based on a combination of the following:
*   **Primary Metric:** Does the primary metric show a statistically significant improvement for the `canary` version?
*   **Guardrail Metrics:** Do the guardrail metrics show any significant regressions for the `canary` version?

**Action:**
*   **On Success (Promotion):** If the primary metric improves without harming guardrail metrics, the experiment is a success, and the `canary` version will be promoted to `stable`.
*   **On Failure (Rollback):** If the experiment fails to show improvement or causes a regression in key metrics, the `canary` version will be rolled back.

### Grafana Visualization
A screenshot of the Grafana dashboard used for monitoring will be added here.

**(Placeholder for Grafana Screenshot)**

### Source
[1] https://arxiv.org/abs/2308.04929