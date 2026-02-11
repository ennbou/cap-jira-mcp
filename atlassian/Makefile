.PHONY: run

run:
	docker run --rm -p 9001:9000 \
		--env-file ./.env \
		ghcr.io/sooperset/mcp-atlassian:latest \
		--transport sse --port 9000 -vv