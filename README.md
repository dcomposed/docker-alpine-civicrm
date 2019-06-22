# CiviCRM 5.14.1 + Drupal 7

# Auto configuration parameters :

Same as drupal 7 image :

- DATABASE_HOST=localhost       ( Name of the mariadb service )
- DATABASE_PORT=3306            ( Port of the mariadb service )
- DRUPAL_DB_NAME=drupal         ( Name of the drupal database )
- DRUPAL_DB_USERNAME=drupal     ( Username to connect to the drupal database )
- DRUPAL_DB_PASSWORD=password   ( Password to connect to the drupal database )
- ADMIN_USERNAME=admin          ( Drupal admin username )
- ADMIN_PASSWORD=password       ( Drupal admin password )
- ADMIN_EMAIL=admin@exemple.org ( Drupal admin email )
- SITE_NAME="drupal"            ( Drupal site name )
- TRUSTED_HOST=localhost        ( Drupal trusted domain )

New parameters :

- CIVICRM_DB_NAME=civicrm       ( Name of the civicrm database )
- CIVICRM_DB_USERNAME=civicrm   ( Username to connect to the civicrm database )
- CIVICRM_DB_PASSWORD=password  ( Password to connect to the civicrm database )

# Compose file exemple

```

version: '3.1'

services:

  civicrm:
    image: dotriver/civicrm
    environment:
      - DATABASE_HOST=mariadb
      - DATABASE_PORT=3306
      - DRUPAL_DB_NAME=drupal
      - DRUPAL_DB_USERNAME=drupal
      - DRUPAL_DB_PASSWORD=password
      - CIVICRM_DB_NAME=civicrm
      - CIVICRM_DB_USERNAME=civicrm
      - CIVICRM_DB_PASSWORD=password
      - SITE_EMAIL=email@example.com
      - TRUSTED_HOST=172.17.0.1:8080
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=password
      - ADMIN_EMAIL=admin@example.com
    ports:
      - 8080:80
    networks:
      default:
    deploy:
      resources:
        limits:
          memory: 256M
      restart_policy:
        condition: on-failure
      mode: global

  mariadb:
    image: dotriver/mariadb
    environment:
      - ROOT_PASSWORD=password
      - DB_0_NAME=drupal
      - DB_0_PASS=password
      - DB_1_NAME=civicrm
      - DB_1_PASS=password
    ports:
      - 3306:3306
      - 8081:80
    volumes:
      - mariadb-data:/var/lib/mysql/
      - mariadb-config:/etc/mysql/
    networks:
      default:
    deploy:
      resources:
        limits:
          memory: 256M
      restart_policy:
        condition: on-failure
      mode: global

volumes:
    mariadb-data:
    mariadb-config:

```
