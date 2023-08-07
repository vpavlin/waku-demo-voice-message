FORCE: ;

get-codex-bootstrap-ip: FORCE
	@$(eval export IPB=$(shell docker inspect bootstrap-codex-node-1 | sed -n 's/.*"IPAddress": "\([^"][0-9.]*\)",.*/\1/p'))

print-codex-bootstrap-ip: get-codex-bootstrap-ip
	@echo $(IPB)

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

print-codex-node1-spr: get-codex-node1-spr
	@echo $(SPR1)

get-codex-node2-spr: get-codex-node2-ip
	$(eval export SPR2=$(shell curl -s http://$(IP2):8080/api/codex/v1/debug/info | jq -r '.spr'))

print-codex-node2-spr: get-codex-node2-spr
	@echo $(SPR2)

get-codex-bootstrap-spr: get-codex-bootstrap-ip
	$(eval export SPRB=$(shell curl -s http://$(IPB):8080/api/codex/v1/debug/info | jq -r '.spr'))

print-codex-bootstrap-spr: get-codex-bootstrap-spr
	@echo $(SPRB)

#connect-nodes: get-codex-bootstrap-ip get-codex-node1-ip get-codex-node2-ip
#	@$(eval ID1=$(shell curl -s http://$(IPB):8080/api/codex/v1/debug/info | jq -r '.id'))
#	@$(eval MA1=/ip4/$(IPB)/tcp/2345)

#	curl "http://$(IP1):8080/api/codex/v1/connect/$(ID1)?addrs=$(MA1)"
#	@echo
#	curl "http://$(IP2):8080/api/codex/v1/connect/$(ID1)?addrs=$(MA1)"
#	@echo

stop:
	docker compose -f codex/docker-compose.yml -p codex1 down
	docker compose -f codex/docker-compose.yml -p codex2 down
	docker compose -f codex/docker-compose.yml -p bootstrap down


bootstrap:
	CODEX_BOOTSTRAP_NODE="spr:CiUIAhIhAgznC0Je0ceJ3szr8HFj-wCTVmzNSTwl3WmtrgbOGDOMEgIDARo8CicAJQgCEiECDOcLQl7Rx4nezOvwcWP7AJNWbM1JPCXdaa2uBs4YM4wQ3qCepgYaCwoJBHTKAxWRAk4qKkYwRAIgLITmBK5Lj7-4XZd3UhrTy4zN5mO-w_Rk0EF32Bz5LbcCIHatVA7yLwmTd0y1zjYwtqGhcTCTI9Oqh62pcZ2zLNgC"\
		docker compose -f codex/docker-compose.yml -p bootstrap up -d

start: bootstrap get-codex-bootstrap-spr
	CODEX_BOOTSTRAP_NODE=$(SPRB)\
		docker compose -f codex/docker-compose.yml -p codex1 up -d

	CODEX_BOOTSTRAP_NODE=$(SPRB)\
		docker compose -f codex/docker-compose.yml -p codex2 up -d
