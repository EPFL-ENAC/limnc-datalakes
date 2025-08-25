help:
	@echo Helper for datalakes

db-port-forward:
	@echo Forwarding database port...
	kubectl -n epfl-limnc-datalakes-dev port-forward svc/datalakes-postgresql-dev 5432:5432

db-restore-local:
	@echo Restoring database...
	psql -h 127.0.0.1 -U postgres -d postgres -f datalakes-no-owner.sql --set ON_ERROR_STOP=on
