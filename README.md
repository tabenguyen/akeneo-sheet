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
30 1 * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle pim:versioning:refresh
30 2 * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle pim:versioning:purge --more-than-days 90
1 * * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle akeneo:connectivity-audit:update-data
10 * * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle akeneo:connectivity-connection:purge-error
20 0 1 * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle akeneo:batch:purge-job-execution
40 12 * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle akeneo:connectivity-audit:purge-error-count
30 4 * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle pim:volume:aggregate
15 0 * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle pim:data-quality-insights:schedule-periodic-tasks
*/10 * * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle pim:data-quality-insights:prepare-evaluations
*/30 * * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle pim:data-quality-insights:evaluations
0 */2 * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle akeneo:messenger:doctrine:purge-messages messenger_messages default
5 * * * * docker exec -it $(docker ps -q -f name=akeneo-pim_fpm) php bin/consle akeneo:connectivity-connection:purge-events-api-logs
```
