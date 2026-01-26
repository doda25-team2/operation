# Use of Generative AI

## Nick

I made heavy use of generative AI for assignment 1. Most of the time I knew exactly what I wanted, and instructed Claude Sonnet 4.5 through GitHub Copilot Agent Mode to implement my instructions.

For example, I had trouble creating an empty Maven project using the CLI, so I asked Claude to initialize an empty project and `pom.xml`.

This is also not my first rodeo with GitHub Actions, Docker and Kubernetes, so I am not afraid to miss out on any learning. To implement F2, I knew exactly what I had to do, it's just way faster if Claude does it than looking up manually how to strip a prefix in Bash. I used the following prompt to create the first version of [`lib-version`'s release workflow](https://github.com/doda25-team2/lib-version/blob/main/.github/workflows/release.yml):

```
Let's now add a github workflow that pushes to the maven package registry on GitHub using secrets.GITHUB_TOKEN. It should trigger when a new release is made on GitHub.

First, it should extract the version from the release tag, then write the version into pom.xml, commit the version bump, build the jar, add it to the release artifacts, add a new tag with the version to the new commit and push it.
```

## George

My primary use was for understanding complex concepts and exploring how different components connect. I spent significant time understanding the different network layers in our application stack (Istio service mesh, Kubernetes networking, container networking), and AI was invaluable for grasping both the bigger picture and diving into specific details. I would ask clarifying questions and request explanations from different angles until the concepts fully clicked.

For code generation, I used AI for boilerplate and formatting heavy tasks. For example, I used it to generate Java classes for Prometheus metrics (Counter, Gauge, and Histogram implementations) where the structure was straightforward but tedious to write. Similarly, I relied on AI for YAML generation, particularly because I found the strict indentation and formatting requirements error prone to write manually. However, I always determined what I wanted to implement and verified the correctness of the approach, AI simply saved me from formatting frustrations.
