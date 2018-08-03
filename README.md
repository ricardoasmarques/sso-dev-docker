# sso-dev-docker

The purpose of this project is to ease the development of Single Sign On
(__SSO__) on your own Service Provider (__SP__) using __SAML__, by
providing a container-based __[Shibboleth](https://www.shibboleth.net/)__
Identity Provider (__IdP__) that uses __LDAP__ for authentication


## Setup up your environment

Clone this repository:

    $ git clone https://github.com/ricardoasmarques/sso-dev-docker

Navigate to the repository directory:

    $ cd sso-dev-docker

Run the following command to start all containers:

    # docker-compose up

Two different containers are now running, one for __LDAP__ and other for 
__Shibboleth__.

Test if everything is working as expected by accessing 
_Shibboleth SP_ and trying
to login with the following credentials:

|Shibboleth SP URL                              | User | Pass  |
|-----------------------------------------------| ----- | ----- |
|[http://localhost:9080/Shibboleth.sso/Login]() | admin | admin |
   
> If you were redirected and received a 403 error (forbidden),
this means that the authentication worked, and you are now logged in.


## Configure your own SP

To configure your own SP, you have to access the Shibboleth docker container:

    # docker-compose exec shibboleth /bin/bash

Inside de docker container, list the existing metadata files:
 
    # ls /opt/shibboleth-idp/metadata

You should replace the `my-sp-metadata.xml` file with your own metadata file,
if your SP provides an URL to obtain this file, you can do something similar to:

    # curl https://<url> --output /opt/shibboleth-idp/metadata/my-sp-metadata.xml --insecure

Each time you change the Shibboleth configuration, you have to restart jetty:

    # $JETTY_HOME/bin/jetty.sh restart

Now you have to add the IdP _signing_ and _encryption_ certificates (_.crt_) to your SP configuration, 
this information is available in the credentials directory:
 
    # ls /opt/shibboleth-idp/credentials 


After setting up all configurations, your SP should be able to authenticate.

> __TODO:__ Add some basic Service Provider implementation samples for different frameworks like python/cherrypy, etc...
