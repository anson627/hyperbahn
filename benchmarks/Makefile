torch-relay-bench:
	node index.js --relay --torch relay --torchFile ./flame.raw --torchTime 10 \
		-- --relay --skipPing -s 4096,16384 -p 10000,20000

take-relay:
	node index.js --relay --noEndpointOverhead -o relay-$$(git rev-parse --short HEAD).json \
		-- --relay --skipPing -m 3 -s 4096,16384 -p 10000,20000
	ln -sf relay-$$(git rev-parse --short HEAD).json relay-$$(basename $$(git symbolic-ref HEAD)).json

kill-dead-benchmarks:
	pkill -f nodejs-benchmarks;

top-benchmark:
	top -d1 -cp `pgrep -f nodejs-benchmarks | tr "\\n" "," | sed 's/,$$//'`;

create-flame:
	stackcollapse-stap.pl ./flame.raw | tr -d "\0" > ./flame.folded
	flamegraph.pl ./flame.folded > ./flame.svg
	google-chrome ./flame.svg

take-relay-bad:
	node index.js --bad --relay --noEndpointOverhead -o relay-$$(git rev-parse --short HEAD)-bad.json \
		-- --bad --relay --skipPing -m 3 -s 4096,16384 -p 10000,20000
	ln -sf relay-$$(git rev-parse --short HEAD)-bad.json relay-$$(basename $$(git symbolic-ref HEAD))-bad.json

torch-relay-bad-bench:
	node index.js --bad --relay --torch relay --torchFile ./flame.raw --torchTime 10 \
		-- --bad --relay --skipPing -s 4096,16384 -p 10000,20000

.PHONY: take-relay take-relay-bad create-flame top-benchmark kill-dead-benchmarks torch-relay-bench
