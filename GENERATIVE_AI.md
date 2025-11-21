# Use of Generative AI

## Nick

I made heavy use of generative AI for assignment 1. Most of the time I knew exactly what I wanted, and instructed Claude Sonnet 4.5 through GitHub Copilot Agent Mode to implement my instructions.

For example, I had trouble creating an empty Maven project using the CLI, so I asked Claude to initialize an empty project and `pom.xml`.

This is also not my first rodeo with GitHub Actions, Docker and Kubernetes, so I am not afraid to miss out on any learning. To implement F2, I knew exactly what I had to do, it's just way faster if Claude does it than looking up manually how to strip a prefix in Bash. I used the following prompt to create the first version of [`lib-version`'s release workflow](https://github.com/doda25-team2/lib-version/blob/main/.github/workflows/release.yml):

```
Let's now add a github workflow that pushes to the maven package registry on GitHub using secrets.GITHUB_TOKEN. It should trigger when a new release is made on GitHub.

First, it should extract the version from the release tag, then write the version into pom.xml, commit the version bump, build the jar, add it to the release artifacts, add a new tag with the version to the new commit and push it.
```

I of course checked everything it generated and understand what I committed.
