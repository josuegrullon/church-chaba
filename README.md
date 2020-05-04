<p align="left"><img src="not-found"  width="00"> TODO logo idea: ice-brick texture </p> 

<p align="center">

</p>

<img src="App-Front-Page.jpg" alt="drawing" width="250"/>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <img src="App-Church-Service-Page.jpg" alt="drawing" width="250"/>   


## About Chaba

Chaba is an app supporting online expansion of 'church' in its broadest sense. It comprises 

- streaming church services through common desktop and mobile web-browsers using hls (latency 15s) and 
   rtmp (flashvideo, with latency of 2s, but which has fewer browser compatibility).
    For the streaming source, OBS can be used
- audio streaming church services 
- Chat area next to the streaming frame
- file manager area for up and download for in-browser playing of church service recordings
- authentication of church members and guests
- [Unfinished] Chat-App with User Management and user status management for encouraging communication between church memebers  
- [Unfinished] Management of "calendar" and "About"-contents of the app  

## Learning Chaba

Take a look into the INSTALL.txt to get insight into installation routine. For an overview of reused packages take a look
into the docker-compose.yml. For now, as the app is still in alpha state, developer resources will pe put into development
first, befor writing a thorough documentation

## Contributing

Thank you for considering contribution to the chaba online church app! 

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within chaba, please send a notification via my github account.

## License

The Chaba online church app is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).


# Installation

## STEP 1: prerequisite: install docker and docker-compose 
    sudo apt-get update
    sudo apt-get install docker.io
    sudo systemctl start docker
    sudo systemctl enable docker
    sudo apt-get install docker-compose
    sudo adduser your_user docker
    sudo service docker restart

## STEP 2: install chaba and its dependancies  
    Hint: for now installation must take place inside /var/www. Will be fixed soon
    cd /var/www
    git clone https://github.com/dioniswe/chaba.git
    composer install
    npm install
    sudo chown -R www-data:www-data .
    npm run dev

## STEP 3: configure and initialize application
### STEP 3.1 prepare your server
chaba occupies the ports 80 (nginx), 3306, 8000 (video-streaming), 8008 (icecast), 6379 (redis) and 6001 (laravel-echo) by default.
Stop any other running applications listening on these ports or reconfigure chaba for using other ports. For common
freshly installed linux server you can use my shutdown script:

    ./shutdown-services.sh 


### STEP 3.2 configure your chaba application.
Modify .env: set desired credentials for

    ICECAST_SOURCE_PASSWORD to authenticate audio streaming sources
    ICECAST_MOUNT_NAME for the audio streaming url path

    CHABA_ADMIN_USER for generating the chaba admin user (responsible for app contents of startpage and recordings)
    CHABA_ADMIN_PASSWORD= for setting the chaba admin password

    CHABA_CONGREGATION_USER for generating the chaba congregation user (the website user)
    CHABA_CONGREGATION_PASSWORD=for setting the chaba congregation password
    
Bring up the containers finally

    docker-compose up -d

## STEP 4 after building, initialize your laravel application

    docker-compose run chaba php artisan key:generate
    docker-compose run chaba php artisan migrate

## Step 5 (optional) install google fonts locally
    mkdir  public/vendor/fonts
    npm install -g google-font-installer
    gfi download Nunito -d public/vendor/fonts


# Usage

Stream to your 'Radio'-Section using any icecast2 compatible client (i.e Butt). In settings use your server's domain 
your configured port (default port 8008) and your configured source authentication key

Stream to your 'Church-Service'-Section using any rtmp-compatible client (i.e OBS). In settings use your server's domain 
your configured port (default port 8000) and your configured streaming key

Chatting works straight

For the recordings an admin user has been created on laravel initialization who is privileged to upload files.
The congregation user is privileged to download and play files


  

