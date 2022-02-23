# Django Exam Website

## Table of Contents
- [Django Exam Website](#django-exam-website)
  * [Introduction](#introduction)
    + [Overview](#overview)
    + [How it Works](#how-it-works)
  * [Installation and Usage](#installation-and-usage)
    + [Deploying App to IBM Cloud Foundry](#deploying-app-to-ibm-cloud-foundry)
  * [Help, Something Went Wrong](#help-something-went-wrong)
  * [Acknowledgements](#acknowledgements)

## Introduction
Good morning folks! Today I am presenting my very cool app known as the Django Exam Website. Unsurprisingly, this website is powered by a Python Django backend, and the frontend is made dynamic courtesy of Django's own HTML Templating capabilities. This is a beginner Django project meant to show Django capabilities.

### Overview
This Django website allows you to enroll in courses and take exams in those courses.

![Index](https://github.com/lawruixi/Django-Exam-Website/blob/main/images/home.png)

Once enrolled in a course, you can take an exam and view the results of the exam.

![Results](https://github.com/lawruixi/Django-Exam-Website/blob/main/images/exam.png)

It also allows for more customization of the courses via the Django Admin page, which allows one to add courses, students, questions, choices and more.

### How it Works
The Django backend uses a Relational Database Management Scheme as shown [here](https://github.com/lawruixi/Django-Exam-Website/blob/main/images/er_diagram.png). By defining the appropriate Models and attributes within the `models.py` file, Django automatically creates and manages the Relational Database as data is created, updated or deleted. Django also comes with handy in-built data retrieval functions, allowing one to filter and sort data easily. 

The logic for server routing is done in `views.py` and `urls.py`. Here there are a bunch of strings / regex expressions, along with a list of URLs. Django matches the URL to an appropriate string, then calls the appropriate function / view in `views.py`. The HTML Templates are found in `/onlinecourse/templates/onlinecourse/`.

The Admin page is also automatically created by Django. By extending the various Django Admin classes, you can easily customize the Admin pages yourself. 

The frontend is assembled using Django's templating format (if you're familiar with Jinja2, you should have no problems here), allowing one to dynamically create HTML pages with little effort. 

When pushing to the IBM Cloud, we need an external server to serve the static files. For this we use an Nginx Cloud Foundry server, which can be used to host and serve the static files. The settings for the STATIC_ROOT directory we are using is specified in `myproject/settings.py`.

But enough about that, let's try it out!

## Installation and Usage
So first, clone the repo.
```bash
$> git clone https://github.com/lawruixi/Django-Exam-Website 
$> cd Django-Exam-Website
```
We'll need `python3` and `pip`, so install that if it is not already installed, using your package manager or otherwise:

```bash
$> sudo apt update && sudo apt install python3 python3-pip
```
The other dependencies required and in `1requirements.txt`, so go ahead and install those too.
```bash
$> pip3 install -r requirements.txt
```
Great! The project is configured to already set up all the tables required for the RDBMS. (The views are defined in the `view.py` file.) Since this is the case, we can go ahead and use `manage.py` to make the [migrations](https://docs.djangoproject.com/en/4.0/topics/migrations/).
```bash
$> python3 manage.py makemigrations
$> python3 manage.py migrate
```
In order to access the `/admin` page, we will be needing a superuser. We can do this with the following call:
```bash
$> python3 manage.py createsuperuser
```
which will ask for username, email and password. Once this is done, we should be able to run the server:
```bash
$> python3 manage.py runserver
```
which will run the server (listening on port `8000` by default). This can be changed by specifying the port number after the command. We can now visit the webpage on `127.0.0.1:80000/`. 

Note: If you are unable to access the webpage due to `Disallowed Hosts` error, you will probably have to include the `hostname` by editing the ALLOWED_HOSTS list in `myproject/settings.py`. If you are accessing the webpage from a web browser, note that `https://` or `http://` should not be included in the ALLOWED_HOSTS.

Once you visit the website, you'll notice there are no courses or instructors yet. We can do this by adding them in the `admin/` page. Visit `127.0.0.1:8000/admin` and login using the superuser credentials created earlier. We then come to Django's admin site, which allows us to add courses, instructors, and questions.

Once you are done, congratulations! Setup is done and you can now play around with the website.

### Deploying App to IBM Cloud Foundry
Your app can be deployed to IBM Cloud Foundry today! (No, this is not a paid promotion by the [Developing Applications with SQL, Databases and Django](https://www.coursera.org/learn/developing-applications-with-sql-databases-and-django) project as part of the [IBM Full Stack Cloud Developer Professional Certificate](https://www.coursera.org/professional-certificates/ibm-full-stack-cloud-developer) on [Coursera](www.coursera.org), I swear.) Simply follow the following steps:

1. Create an [IBM Cloud](https://cloud.ibm.com) Account.
2. Create a Cloud Foundry Org, then a Cloud Foundry Space (under Account > Manage > Cloud Foundry Orgs).
3. Once that is done, use the `ibmcloud cli` as follows:
    - Login using the cli:
      ```bash
      ibmcloud login -u <username> -p <password>
      ```
      (or omit `-p password` if you don't want your password showing up in the bash history)
    - Target the appropriate region:
      ```bash
      ibmcloud target -r REGION
      ```
      where region is the region code (e.g. `us-south`)
    - Target the organization and space:
      ```bash
      ibmcloud target -o <organization> -s <space>
      ```
    - Install `cf` if not already done so
      ```bash
      ibmcloud cf install
      ```
    - List Cloud Foundry domains:
      ```bash
      ibmcloud cf domains
      ```
    - Edit the `manifest.yml` and `myproject/settings.py` files:
        - Choose a domain, and edit the routes in the format of `HOST.DOMAIN`, where HOST is a unique identifier.
    - Push the app.
      ```bash
      ibmcloud cf push
      ```
And that's all.

## Help, Something Went Wrong
Uh oh. Don't worry! If you are unable to complete the instructions, there is a website already prepared for you [here](http://lawruixi.us-south.cf.appdomain.cloud)! The following users are available:
| Username      | Password |
| --------      | -------- |
| Learn Ahr     | `abcdef`   |
| Skoll Lahr    | `abcdef`   |
| Emma Teur     | `abcdef`   |
| Bea Ginner    | `abcdef`   |
| Innis Ructeur | `abcdef`   |
| Proph Fesseur | `abcdef`   |

The following user account is _not_ available:
- `lawruixi`

And no, once again, I'm not giving out my password. This also means you can't access the admin site, but oh well. 

That's all from me today. Have fun playing around~

## Acknowledgements
Through many trials and tribulations, walking through the deepest fire and flames, one student rose against all odds and managed to create a project. This project was so remarkable, so earth-shattering that many all around the world shuddered when they heard her achievements. All bowed to her as she showed off her CS knowledge and prowess, and everyone who visited her GitHub link were shocked and stunned so hard they could only offer her fleeting words of praise from their mouths. All they could think of was how grateful they were to be living in the same era as this student.

But this isn't about that student. This is about another student. After a long time, unlike the successful student, this one only managed to make a small little Django project which they pushed onto their GitHub. They thought no one would see it, because they had almost no recognition, no fame, no GitHub followers. They went through pain, searching StackOverflow, searching Coursera forums, but none yielded the answers they needed. And even at the end, no one was there to even see the fruits of their labours.

Except you.

You, the reader of this README, has bore witness. You have seen the incredible amounts of effort put into this project, the amount of time, dedication, blood, sweat and tears that went into producing such a refined product. And all I can say is...

Thank you.
