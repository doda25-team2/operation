# Continuous Experimentation

### Experiment

Since new features are being tested for the application, the most suitable experiment that can be conducted would be the A/B Testing. A/B testing is when there are two different versions of the application in order to determine which is the best. Usually, a new feature is shown in one version. Based on this, A/B Testing is the most suitable since it allows to determine which feature would be the best [1].

### Changes

The changes occur in which the baseline version contains the normal buttons whereas the changed version has a new feature with a different button that has a completely different style.

### Falsifiable Hypothesis

The new version does not improve performance or behavior compared to the baseline.

### Relevant Metrics

The relevant metrics are the following:

- ```sms_classification_requests_total{service="frontend"}``` - Counter tracking total SMS classification requests
- ```sms_classification_results_total{result="spam"}``` - Counter for spam classifications
- ```sms_classification_results_total{result="ham"}``` - Counter for ham classifications
- ```sms_classification_duration_milliseconds{service="frontend",operation="classify"}``` - Histogram tracking response time distribution



### Decision Process


### Visualization 

The following visualizations is used in order to support the decision process:

### Conclusion

### Source

1. https://arxiv.org/abs/2308.04929