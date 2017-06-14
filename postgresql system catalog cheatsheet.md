# postgresql system catalog cheatsheet



## Sample searches 

List all existing postgres dbs.  
see <http://stackoverflow.com/questions/19678332/how-to-get-the-existing-postgres-databases-in-a-list>

    SELECT d.datname as "Name",
    FROM pg_catalog.pg_database d
    ORDER BY 1;

## Disk Usage queries

<https://wiki.postgresql.org/wiki/Disk_Usage>

May need to connect as postgres, or root.  

    SELECT nspname || '.' || relname AS "relation",
        pg_size_pretty(pg_total_relation_size(C.oid)) AS "total_size"
      FROM pg_class C
      LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace)
      WHERE nspname NOT IN ('pg_catalog', 'information_schema')
        AND C.relkind <> 'i'
        AND nspname !~ '^pg_toast'
      ORDER BY pg_total_relation_size(C.oid) DESC
      LIMIT 20;