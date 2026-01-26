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

The experiment is monitored using a dedicated Grafana dashboard that compares key metrics between the stable and canary versions in real-time.

**Dashboard: "A4 Experiment: Stable vs Canary"**

The dashboard includes the following panels:

1. **Request Rate (Guardrail)**: Time-series graph comparing requests per second between stable and canary versions. This ensures the new UI doesn't negatively impact user submission volume.

2. **Response Time p95 (Guardrail)**: 95th percentile response times for both versions. This validates that the UI redesign doesn't introduce performance regressions.

3. **Average Message Length (Primary Metric)**: The key metric for our hypothesis. This graph shows the average character count of submitted messages over time, split by version. A higher value for canary indicates improved user engagement.

4. **Total Request Counters**: Summary statistics showing cumulative requests received by each version, useful for understanding traffic distribution.

5. **Canary Improvement Indicator**: A percentage showing how much better (or worse) the canary version performs on the primary metric compared to stable. Values above 100% indicate improvement.

**Results Interpretation:**

The dashboard clearly demonstrates the hypothesis: the canary version's redesigned UI leads to significantly longer user messages (primary metric), while maintaining comparable request rates and response times (guardrails). This validates that the new UI successfully improves user engagement without negative side effects.

![Grafana Dashboard showing A/B test results](/operation/docs/imgs/stable-vs-canary-grafana.png)


### Source
[1] https://arxiv.org/abs/2308.04929