# Day-47-Advanced-Triggers-PR-Events-Cron-Schedules-Event-Driven-Pipelines

## Task 1: Pull Request Event Types

Create .github/workflows/pr-lifecycle.yml that triggers on pull_request with specific activity types:

1. Trigger on: opened, synchronize, reopened, closed
2. Add steps that:
   - Print which event type fired: ${{ github.event.action }}
   - Print the PR title: ${{ github.event.pull_request.title }}
    - Print the PR author: ${{ github.event.pull_request.user.login }}
    - Print the source branch and target branch
      
3. Add a conditional step that only runs when the PR is merged (closed + merged = true)

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/970f9c57-00c2-42f5-8bf8-f6e0ffead028" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/9345f1d4-4d60-4ae8-966f-648ab4f753e8" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/68a17745-0f20-4e88-987d-255fa9876b97" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/503f8db6-f015-4f55-ba7e-eccd9ed1af6d" />

 ## Task 2: PR Validation Workflow
 
Create .github/workflows/pr-checks.yml — a real-world PR gate:

1. Trigger on pull_request to main
2. Add a job file-size-check that:
   - Checks out the code
   - Fails if any file in the PR is larger than 1 MB
3. Add a job branch-name-check that:
   - Reads the branch name from ${{ github.head_ref }}
    - Fails if it doesn't follow the pattern feature/*, fix/*, or docs/*
4. Add a job pr-body-check that:
   - Reads the PR body: ${{ github.event.pull_request.body }}
   - Warns (but doesn't fail) if the PR description is empty
  
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/2ced9d10-6cfb-41e2-85f1-c55145afccf6" />

## Task 3: workflow_run — Chain Workflows Together

Create two workflows:

 1 .github/workflows/tests.yml — runs tests on every push
 2. .github/workflows/deploy-after-tests.yml — triggers only after tests.yml completes successfully:
 
on:

  workflow_run:
  
    workflows: ["Run Tests"]
    
    types: [completed]
    
3. In the deploy workflow, add a conditional:
   
 - Only proceed if the triggering workflow succeeded (${{ github.event.workflow_run.conclusion == 'success' }})
- Print a warning and exit if it failed
