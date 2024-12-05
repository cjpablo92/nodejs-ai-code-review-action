# AI Code Review Action

This GitHub Action uses OpenAI (e.g., GPT-4, GPT-3.5-turbo) to provide automated code review feedback on Node.js pull requests. It identifies bugs, suggests best practices, highlights security concerns, and recommends maintainability improvements.

## Usage

To use this action in a workflow, reference it by your repository name and a tagged release (e.g., `@v1`):

```yaml
name: Example PR Review
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  ai_review_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run AI Review
        uses: cjpablo92/ai-code-review-action@v1
        with:
          base_sha: ${{ github.event.pull_request.base.sha }}
          head_sha: ${{ github.event.pull_request.head.sha }}
          openai_api_key: ${{ secrets.OPENAI_API_KEY }}
          model: gpt-4

      - name: Post Review Comment
        run: |
          gh pr comment ${{ github.event.pull_request.number }} --body "${{ steps.ai_review_job.outputs.review }}"