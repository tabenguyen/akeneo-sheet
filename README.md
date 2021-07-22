# akeneo-sheet

## Docker Swarm exec

```
docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) bash
```

## Message consumer

```
php bin/console messenger:consume ui_job import_export_job data_maintenance_job
```

## Cronjobs

```
30 1 * * * pim:versioning:refresh
30 2 * * * pim:versioning:purge --more-than-days 90
1 * * * * akeneo:connectivity-audit:update-data
10 * * * * akeneo:connectivity-connection:purge-error
20 0 1 * * akeneo:batch:purge-job-execution
40 12 * * * akeneo:connectivity-audit:purge-error-count
30 4 * * * pim:volume:aggregate
15 0 * * * pim:data-quality-insights:schedule-periodic-tasks
*/10 * * * * pim:data-quality-insights:prepare-evaluations
*/30 * * * * pim:data-quality-insights:evaluations
0 */2 * * * akeneo:messenger:doctrine:purge-messages messenger_messages default
5 * * * * akeneo:connectivity-connection:purge-events-api-logs
```
