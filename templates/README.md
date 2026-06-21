# templates/

Canonical per-tool config. `agent-substrate sync` renders every file under this
directory into the consumer repo at the same relative path, prepending a
`# Generated from @marcelle-labs/agent-substrate@{version}` header.

A consumer may override any canonical file by placing a file at the same relative
path under its own `.agents/` directory; the consumer copy wins.
