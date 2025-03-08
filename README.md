## Usage

```yaml
name: Add comment via ChatGPT

on:
  issues:
    types: [opened]

jobs:
  chat:
    runs-on: ubuntu-latest
    steps:
      - id: chat
        name: Generated comment via ChatGPT
        uses: aahl/chatgpt-action@v1
        with:
          # chatgpt_api_base: https://api.deepseek.com/v1
          chatgpt_api_key: ${{ secrets.CHATGPT_API_KEY }}
          model: deepseek-v3
          prompt: |
            You are a Github issue assistant, please answer this question.
          message: |
            Title: ${{ github.event.issue.title }}
            Body: ${{ github.event.issue.body }}
  
      - name: Create comment
        uses: actions-cool/issues-helper@v3
        with:
          actions: create-comment
          token: ${{ secrets.GITHUB_TOKEN }}
          issue-number: ${{ github.event.issue.number }}
          body: ${{ steps.chat.outputs.REPLY }}
```
