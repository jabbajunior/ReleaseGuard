# Project Learnings

This project helped me understand what is involved in building a small CI/CD pipeline from the ground up. The technical work was valuable, but the biggest lessons came from managing assumptions, state, portability, and scope.

## Pipeline Design

One of the clearest lessons was that maintaining a unified test suite is easier than keeping separate versions of similar tests. Reusing the same core tests for local validation and container validation reduces drift and makes the pipeline easier to trust.

I also learned that every feature added to a pipeline creates more outcomes to handle. More functionality is not always better, especially in an MVP. It is often more useful to clearly document the assumptions a solution makes and the cases it does not support than to try to build a perfect system in limited time.

## Portability and Script Structure

Portability is a major concern with Bash scripts. A script can behave differently depending on whether it is run from the repository root, called by absolute path, or executed inside GitHub Actions. This project made it clear that scripts should either enforce their expected working directory or document that requirement carefully.

Modularizing repeated Bash logic into sourced helper scripts also helped keep the project manageable. Shared logging and configuration code reduced duplication, but organizing those shared pieces cleanly was its own challenge.

## State and GitHub Actions

Managing local state in GitHub Actions was more difficult than expected. Repository checkouts can overwrite local files, and CI/CD state can be hard to preserve unless the workflow design is very explicit.

This project assumes that CI and CD run on the same self-hosted runner so the CD step can reuse the image and state files created by CI. That assumption works for this learning project, but it is also an important limitation to document.

I also learned that Docker image identifiers can be confusing. For example, Docker's `--iidfile` option records an image ID, while other commands may expose digests or tags. Being precise about which identifier is being stored matters when promoting or validating an image.

## Workflow and Tooling

Before learning about `workflow_dispatch`, manually testing GitHub Actions through commits was a major pain point. Adding manual workflow triggers made iteration much easier and reduced unnecessary commit noise.

The project also reinforced how important logs are for understanding pipeline behavior. Clear logs make it easier to debug failures, explain what happened during a run, and confirm that the expected deployment steps occurred.

## Documentation and Demo Preparation

Writing documentation required balancing completeness with readability. It was tempting to explain every dependency and implementation detail, but the README needed to stay focused on what a user needs to install, run, and understand the project.

Recording the demo was also challenging. The goal was to explain what the project accomplishes in a short amount of time while still making the CI/CD flow understandable to someone seeing it for the first time.

---
## What I Would Change Next Time

If I built this project again, I would spend more time researching how CI/CD pipelines are usually structured before designing the first version. My initial assumption was that CI would run and then directly call CD as the next step. That led me toward a different architecture than the one I ended up with after learning how CI and CD are commonly separated.

I would also narrow the supported execution path earlier. Supporting both local script execution and GitHub Actions made the project more flexible, but it also added a lot of portability concerns. If the goal were only to demonstrate the pipeline through GitHub Actions, I could simplify the scripts and reduce the number of runtime assumptions to handle.

State management is another area I would design more carefully from the start. Writing state files into the local `state` directory worked for this learning project, but it is not a scalable approach. A stronger version would use a more intentional artifact or deployment state strategy instead of depending on local files on one runner.

I would also create lightweight design notes for the MVP before implementing. That would make it easier to separate required functionality from extra ideas. I would also plan a more consistent commit style at the start, since the commit history became less organized as the project changed direction. At the same time, adding features such as GitHub Actions support helped me learn more, so I think the extra exploration was still valuable for the purpose of this project.

