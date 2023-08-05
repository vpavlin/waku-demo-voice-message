FORCE: ;

get-codex-node1-ip: FORCE
	@$(eval export IP1=$(shell docker inspect codex1-codex-node-1 | sed -n 's/.*"IPAddress": "\([^"][0-9.]*\)",.*/\1/p'))

print-codex-node1-ip: get-codex-node1-ip
	@echo $(IP1)
	
get-codex-node2-ip: FORCE
	@$(eval export IP2=$(shell docker inspect codex2-codex-node-1 | sed -n 's/.*"IPAddress": "\([^"][0-9.]*\)",.*/\1/p'))

print-codex-node2-ip: get-codex-node2-ip
	@echo $(IP2)

get-codex-node1-spr: get-codex-node1-ip
	@$(eval export SPR1=$(shell curl -s http://$(IP1):8080/api/codex/v1/debug/info | jq -r '.spr'))

get-codex-node2-spr: get-codex-node2-ip
	$(eval export SPR2=$(shell curl -s http://$(IP2):8080/api/codex/v1/debug/info | jq -r '.spr'))

print-codex-node2-spr: get-codex-node2-spr
	@echo $(SPR)

connect-nodes: get-codex-node1-ip get-codex-node2-ip
	@$(eval ID1=$(shell curl -s http://$(IP1):8080/api/codex/v1/debug/info | jq -r '.id'))
	@$(eval MA1=/ip4/$(IP1)/tcp/2345)

	curl "http://$(IP2):8080/api/codex/v1/connect/$(ID1)?addrs=$(MA1)"
	@echo

stop:
	docker compose -f codex/docker-compose.yml -p codex1 down
	docker compose -f codex/docker-compose.yml -p codex2 down

prep:
	mkdir -p codex/datadir codex/codex2/datadir

start-node1:
	docker compose -f codex/docker-compose.yml -p codex1 up -d

start: prep start-node1 get-codex-node1-spr
	
	CODEX_BOOTSTRAP_NODE=$(SPR1)\
		docker compose -f codex/docker-compose.yml -p codex2 up -d

clean: 
	rm -rf codex/datadir codex/codex2/datadir