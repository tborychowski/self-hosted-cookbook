# Carddav
- [plugins.roundcube.net](https://plugins.roundcube.net/#/packages/roundcube/carddav)
- [Github repo](https://github.com/blind-coder/rcmcarddav)


### How to make it work with Monica

Password field in the rouncdube db is too short for the API token from Monica, so we need to make it accept longer passwords:

1. First get password from `mailcow-dockerized/mailcow.conf`:
    ```sh
    cat mailcow-dockerized/mailcow.conf | grep DBPASS
    ```

2. Then modify the db:
    ```sh
    docker-compose exec mysql-mailcow sh
    mysql -u mailcow -p <DBPASS>
    use mailcow;

    # see all addressbooks:
    select * from mailcow_rc1carddav_addressbooks;

    # see table properties
    describe mailcow_rc1carddav_addressbooks;

    # change password field type from varchar to text
    ALTER TABLE mailcow_rc1carddav_addressbooks MODIFY password text;
    ```
