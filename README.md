# Intern CMS Platform

Initial deployment is at http://intern-cms-dev.elasticbeanstalk.com/

Mobile App repo is at https://github.com/sagekieran/mobile-cms

[REST API Endpoints](docs/API_Endpoints.md)

These install instructions are based on running [Postgres.app](http://postgresapp.com/) version 9.4 on Mac OSX Yosemite, so make sure you have it installed 

#### Python Setup

[Go here for a more in-depth install documentation](docs/python.md)

Add Postgres.app to the system PATH. For 9.4, do this by adding `PATH="/Applications/Postgres.app/Contents/Versions/9.4/bin:${PATH}"`
to your .bash_profile.


If you're already familiar with and have used virtualenv, then create a new virtualenv and install the project packages by running

```
pip install -r requirements.txt
```

#### Postgres setup

To create the kiosk_cms database in PostGres, from the repo root run

```
bash postgres.sh
```

Then run a DB migration with

```
cd kiosk_cms
python manage.py migrate
```
 
Create your admin user with 
``` 
python manage.py createsuperuser
```

#### AWS RDS Postgres usage

As this project is already set up to run on AWS Elastic Beanstalk and use a RDS Postgres instance, in order to use a RDS Postgres instance on the local server, simply 

- provide 'RDS_DB_NAME', 'RDS_USERNAME', 'RDS_PASSWORD', 'RDS_ENDPOINT', and 'RDS_PORT' as environment variables
- add your CIDR/IP to the RDS instance's DB Security Group to allow access from your machine's IP address. To do this, go to Security Groups in the RDS Dashboard, select the instance's Security Group, and below where it lists authorized connection types select CIDR/IP from the Connection Types dropdown menu. The settings for your current machine will be filled in, so click on Authorize.

#### AWS S3 Media setup

This project is set up to use an AWS S3 bucket for serving media files.
If you need help setting up your S3 bucketand an IAM user, reference [this file](docs/AWS.md)

With your bucket set up, add the following environment variables to your system, replacing the italics with the respective info. The IAM info will be in the credentials file that you downloaded when you created the IAM User.

BUCKET_NAME=*S3 Bucket Name*

AWS_SECRET_ACCESS_KEY=*IAM User Secret Access Key*

AWS_ACCESS_KEY=*IAM User Access Key ID*


#### Running the local server
Every time you run the local server, make sure that you have the virtualenv running. Your terminal wiil show the currently running virtualenv at the beginning of each command prompt line. In our case, the command prompt will read `(intern-cms)<username>:<current folder> <user>$`.

If the command prompt does not have the virtualenv name at the front, then activate it by running `workon intern-cms`.

To get Angular running for the server, Node.js and npm will need to be installed. You can get them from [http://nodejs.org/](http://nodejs.org/).

Once Node and npm are installed, install Angular and it's dependencies 
listed in bower.json and package.json with
```
npm install
```

Now Angular is where we need it to be. That only needs to be done for the initial project set up. 



Now, you'll need to have Django collect the static files(CSS, JS, icons) and put them into the folder to serve them from( This is set up in the kiosk/settings.py file. STATICFILES_DIRS is a list of folders that Django collects files from, and STATIC_ROOT is the folder where Django puts that collection of files to serve them from. 

To do this, `cd kiosk_cms` and run 
```
python manage.py collectstatic --noinput
```

To start the deveelopment server run 
```
python manage.py runserver
```
