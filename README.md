
# Boilerplate for web projects

This is a boilerplate for a single repository where you can manage different applications with a single docker configuration.

##
<br/>

# Usage

Add some local domains to your computer in order to work on the same port (80 or 443).<br/>
If you used localhost you would have to use different ports (80, 81, 82) for each application.<br/>
Adding this setting to your computer helps to avoid that situation and makes it more explicit about which application we are browsing.
```
sudo nano /etc/hosts
127.0.0.1       www.myapp.local admin.myapp.local utilities.myapp.local
```

Starting the containers (You need to use the flag --build if you change the files)
```
docker compose up -d
```

Stopping the containers
```
docker compose down
```

##
<br/>

# Features

* https://www.myapp.local/
> The first application. Web. 

* https://admin.myapp.local/
> The second application. Admin.

* https://utilities.myapp.local/
> A place where you can put your useful links and another stuff you need to improve your performance

* https://utilities.myapp.local/phpMyAdmin/
> The phpMyAdmin utility to manage mysql (you can use your local application)

* https://utilities.myapp.local/phpRedisAdmin/
> The phpRedisAdmin application to manage redis (you can use this or RedisInsight)

* http://utilities.myapp.local:15672/
> The RabbitMQ Management

* http://utilities.myapp.local:8001/
> The RedisInsight Management to manage redis (you can use this or phpRedisAdmin)

##
<br/>

# The containers

* app_mysql
> The mysql instance. Optional if you use Mongo, PostgreSQL or another database.

* app_nginx
> The nginx instance. In ./docker/nginx/sites you can find the different configurations of your sites

* app_php_817
> The php instance. If you need different versions of php for your applications (legacy code) you can create an app_php_562 for instance

* app_composer
> This is a temporary container with the only goal to install the php dependencies.<br/>
> You can use it to install the dependencies of new applications or delete it if you install the dependencies using the composer inside the php container.

* app_node
> If your applications need the "npm commands", it could be useful has this instance instead of install node in the php or nginx container<br/>
> Now, to avoid this container from dying I execute `CMD ["sleep", "infinity"]`. Once you have your own Javascript applications you can use your "npm commands"

* app_redis
> The redis instance. Optional if you don't need it

* app_rabbit
> The RabbitMQ instance. Optional if you don't need it

* app_redisinsight
> This is an application to manage the content of the redis instance. It's optional, you can do it with your own application or with https://utilities.myapp.local/phpRedisAdmin/ 

##
<br/>

# To consider

* If you need to add new php containers, consider the file ./docker/nginx/conf.d/default.conf where you have to add the new upstreams that you have to add in the `fastcgi_pass` attribute of your site nginx configuration.

* In `apps/utilities/phpRedisAdmin/includes/config.inc.php` line 30 there is hardcoded the host of the mysql instance

* In `apps/utilities/phpRedisAdmin/includes/config.sample.inc.php` line 8 there is hardcoded the host of the redis instance

* Depending on your browser, you will likely receive a warning since the certificate you created isn’t signed by one of your browser’s trusted certificate authorities. It is expected, you have to accept to continue.

##
<br/>

# Feedback

Please, if there is something wrong, something that could be improved, or if you need additional features, let's talk. I will be happy to improve or help you.

Thank you and I hope it can be useful.

##
<br/>

# Next steps

* Instead of using naive applications, it might be interesting to use "real" applications using known frameworks.

* Generate ssl certificate using mkcert (https://github.com/FiloSottile/mkcert) to get browser trust.
