Wordpress + Redis + LEMP

Установка:

1. Скопировать файлы проекта, например в директорию /opt/worldpress 
В .env файле изменить версии контейнеров на актуальные, изменить пароли на свои.

Выполнить docker compose up -d для скачивания docker образов и запуска проекта.

2. Добавить в файл /var/lib/docker/volumes/worldpress/_data/wp-config.php 
настройки для Redis и настройки для повышения безопасности Wordpress.

/* Add any custom values between this line and the "stop editing" line. */

define( 'WP_REDIS_HOST', 'redis' );
define( 'WP_REDIS_PORT', '6379' );
// Необязательно: задайте префикс, если на одном Redis работает несколько сайтов
// define( 'WP_REDIS_PREFIX', 'my_wp_site_' );
define( 'DISABLE_WP_cron', true);

define('DISALLOW_FILE_EDIT', true);
define('LIMIT_LOGIN_ATTEMPTS', 5);
define('FS_CHMOD_DIR', 0755);
define('FS_CHMOD_FILE', 0644);
define('WP_AUTO_UPDATE_CORE', true);
// define('FS_METHOD', ‘ftpext’);

/* That's all, stop editing! Happy publishing. */

3. Установка плагина в WordPress: 
Перейдите в административную панель WordPress. Откройте раздел Плагины -> Добавить новый. Найдите и установите Redis Object Cache. Активируйте плагин. Перейдите в Настройки -> Redis и нажмите кнопку Enable Object Cache.

4. Далее настройте SSL в Nginx и обновление сертификата для Let's Encrypt. Configure Content Security Policy (CSP) in Nginx.

Для создания проекта использовались ссылки:
https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose#installing-wordpress-with-docker-compose  
https://melapress.com/secure-wp-config-php-file/  
https://www.malcare.com/blog/secure-site-with-wp-config/#1-disable-file-editing  

5. Нужно дабавить в файл .htaccess содержимое:

<Files wp-config\.php>
order allow,deny
deny from all
</Files>

Для большей защищенности сайта.
