# vacuum_test

create extension dblink;
create extension pg_profile;
create extension pg_stat_statements;

my/inst/bin/psql -d postgres  -Aqtc "SELECT  get_report(tstzrange(now()-interval '1 day',now()));" -o 1st_report.html
for i in `seq 20000`;do my/inst/bin/psql -d postgres -f ~/vacuum_insert.sql; sleep 2m; done

alena@postgres=# select set_server_connstr('local','dbname=postgres port=5432 user=postgres');

alena@postgres=# CREATE TABLE testtab (
   id        bigint,
   unchanged integer,
   changed   integer
);

alena@postgres=# INSERT INTO testtab
   SELECT i, i, 0
   FROM generate_series(1, 10000) AS i;

alena@postgres=# CREATE INDEX testtab_unchanged_idx ON testtab (unchanged);
alena@postgres=# CREATE INDEX testtab_changed_idx ON testtab (changed);

