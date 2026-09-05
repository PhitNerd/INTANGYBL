# Veros scoring worker

Server-side half of the Veros entry simulation. The browser posts
`{ door: "ld" | "it", answers: { q1: "...", ... } }` and gets back `{ profile }`.
The prompt, the model choice, and the API key all live here.

## Deploy (replaces the existing worker at intangybl.rvance609.workers.dev)

    cd worker
    npx wrangler login
    npx wrangler secret put ANTHROPIC_API_KEY     # paste the key when prompted
    npx wrangler deploy

`name = "intangybl"` in wrangler.toml matches the existing worker name, so
deploying overwrites it in place and the URL stays the same.

## Test

    curl -s -X POST https://intangybl.rvance609.workers.dev/ \
      -H 'Origin: https://intangybl.com' -H 'Content-Type: application/json' \
      -d '{"door":"ld","answers":{"q1":"Ops manager, 6 direct reports","q2":"Slack, Jira, Gmail"}}'

A request without an allowed `Origin` header returns 403.
