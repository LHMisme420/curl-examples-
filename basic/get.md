# GET request

Simple GET:
```bash
curl https://api.example.com/users
curl "https://api.example.com/users?active=true&limit=50"
curl -s https://api.example.com/users | jq

**basic/post-json.md**
```markdown
# POST JSON

```bash
curl -X POST https://httpbin.org/post \
  -H "Content-Type: application/json" \
  -d '{
    "username": "luke",
    "role": "jedi"
  }'
curl -X POST -H "Content-Type: application/json" -d '{"username":"luke"}' https://httpbin.org/post
curl -s -X POST -H "Content-Type: application/json" -d '{"username":"luke"}' https://httpbin.org/post | jq

**basic/post-form.md**
```markdown
# POST form data (x-www-form-urlencoded)

```bash
curl -X POST https://httpbin.org/post \
  -d "username=leia&planet=alderaan"
curl -X POST https://httpbin.org/post \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=leia&planet=alderaan"

**basic/upload-file.md**
```markdown
# Upload file (multipart)

```bash
curl -X POST https://httpbin.org/post \
  -F "file=@/path/to/report.pdf" \
  -F "description=Monthly report"
curl -F "files=@file1.jpg" -F "files=@file2.png" https://httpbin.org/post

**basic/download-file.md**
```markdown
# Download file

Save to disk:
```bash
curl -O https://example.com/bigfile.zip

### auth/
**auth/basic-auth.md**
```markdown
# Basic Authentication

```bash
curl -u username:password https://api.example.com/private
curl -u username https://api.example.com/private
curl --netrc https://api.example.com/private

**auth/bearer-token.md**
```markdown
# Bearer Token (OAuth2 / JWT)

```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6..." \
  https://api.example.com/protected
TOKEN="eyJhbGciOiJIUzI1NiIs..."
curl -H "Authorization: Bearer $TOKEN" https://api.example.com/protected

**auth/api-key-header.md**
```markdown
# API Key in header

Common patterns:

```bash
# X-API-Key
curl -H "X-API-Key: abc123yourkey" https://api.example.com/data

# apikey header
curl -H "apikey: your-secret-key" https://api.example.com/data

**auth/oauth2-bearer.md**
```markdown
# Full OAuth2 flow (client credentials)

```bash
# Get token
TOKEN=$(curl -s -X POST https://auth.example.com/oauth/token \
  -d "grant_type=client_credentials" \
  -u "client_id:client_secret" | jq -r .access_token)

# Use token
curl -H "Authorization: Bearer $TOKEN" https://api.example.com/data

### advanced/
**advanced/follow-redirects.md**
```markdown
# Follow redirects (-L)

```bash
curl -L https://bit.ly/some-short-link

**advanced/timeout-and-retries.md**
```markdown
# Timeout & retries

```bash
curl --connect-timeout 5 \
     --max-time 30 \
     --retry 3 \
     --retry-delay 2 \
     https://api.example.com/slow-endpoint

**advanced/ssl-no-verify.md**
```markdown
# ⚠️ Disable SSL verification (DANGEROUS – only for debugging)

```bash
curl -k https://self-signed-bad-ssl.com
# or
curl --insecure https://self-signed-bad-ssl.com

### real-world/
**real-world/github-api-create-issue.md**
```markdown
# GitHub API – Create issue

```bash
curl -X POST https://api.github.com/repos/octocat/Hello-World/issues \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  -d '{
    "title": "Found a bug",
    "body": "I'm having a problem with this.",
    "labels": ["bug"]
  }'

**real-world/openai-chat-completion.md**
```markdown
# OpenAI Chat Completion (GPT-4o)

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Explain quantum computing in 2 sentences"}],
    "temperature": 0.7
  }'

**real-world/stripe-charge.md**
```markdown
# Stripe – Create charge

```bash
curl https://api.stripe.com/v1/charges \
  -u sk_live_XXXXXXXXXXXXXXXXXXXXXXXX: \
  -d amount=999 \
  -d currency=usd \
  -d source=tok_visa \
  -d description="Example charge"

**real-world/discord-webhook.md**
```markdown
# Discord Webhook

```bash
curl -H "Content-Type: application/json" \
  -d '{"username": "AlertBot", "content": "Server is down!"}' \
  https://discord.com/api/webhooks/1234567890/AbCdefGhIJK
curl -X PUT -T report.pdf "https://my-bucket.s3.amazonaws.com/report.pdf?X-Amz-Algorithm=..."

Just run this in your terminal to create everything instantly:

```bash
mkdir -p curl-examples/{basic,auth,advanced,real-world}
# Then paste all the files above using your editor or a script
