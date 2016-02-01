Keystone.js Blog
===

## Getting started

Clone this repo and `cd` into it. Install dependencies with `npm install`. Fire up `mongod` in a separate terminal. Use `node keystone` to start the server on port 3000. 


# Tutorial:

# KeystoneJS

Install the Keystone.js [Yeoman generator](https://github.com/keystonejs/generator-keystone):

```
$ npm install -g yo generator-keystone
$ mkdir keystone-blog && cd keystone-blog
$ yo keystone
Welcome to KeystoneJS.

? What is the name of your project? My Site
? Would you like to use Jade, Swig, Nunjucks or Handlebars for templates? [jade 
| swig | nunjucks | hbs] jade
? Which CSS pre-processor would you like? [less | sass | stylus] sass
? Would you like to include a Blog? Yes
? Would you like to include an Image Gallery? Yes
? Would you like to include a Contact Form? Yes
? What would you like to call the User model? User
? Enter an email address for the first Admin user: connor.leech@mondo.com
? Enter a password for the first Admin user: admin
? Would you like to include gulp or grunt? [gulp | grunt] gulp
? Would you like to create a new directory for your project? No
? ------------------------------------------------
    KeystoneJS integrates with Mandrill (from Mailchimp) for email sending.
    Would you like to include Email configuration in your project? Yes
? ------------------------------------------------
    Please enter your Mandrill API Key (optional).
    See http://keystonejs.com/guide/config/#mandrill for more info.
    
    You can skip this for now (we'll include a test key instead)
    
    Your Mandrill API Key: 
? ------------------------------------------------
    KeystoneJS integrates with Cloudinary for image upload, resizing and
    hosting. See http://keystonejs.com/docs/configuration/#services-cloudinary f
or more info.
    
    CloudinaryImage fields are used by the blog and gallery templates.
    
    You can skip this for now (we'll include demo account details)
    
    Please enter your Cloudinary URL: 
? ------------------------------------------------
    Finally, would you like to include extra code comments in
    your project? If you're new to Keystone, these may be helpful. Yes
```

![](http://i.imgur.com/om8nn7E.png)

After the install completes you will see a welcome message:

```
Your KeystoneJS project is ready to go!

For help getting started, visit http://keystonejs.com/guide

We've included a test Mandrill API Key, which will simulate email
sending but not actually send emails. Please replace it with your own
when you are ready.

We've included a demo Cloudinary Account, which is reset daily.
Please configure your own account or use the LocalImage field instead
before sending your site live.

To start your new website, run "node keystone".
```

Run `node keystone` and open a browser to `http://localhost:3000/`.

If there is trouble, for instance:

```
Mongo Error:

[Error: failed to connect to [localhost:27017]]
```

Make sure you have `mongod` running in a separate terminal window!

### Send emails with our blog using Mandrill

Most applications these days need to send emails in order to communicate with users. Setting this up from scratch takes a lot of boilerplate. Thankfully our starter app comes with [Mandrill](http://mandrill.com/) (from Mailchimp) integration out of the box. All we need to do is add an API key.

* [Sign up for an account](https://mandrill.com/signup/)
* Head over to the settings tab and create a New API Key
![](http://i.imgur.com/j7PqmZd.png)
* Go into the **.env** file for your project and paste your key into `MANDRILL_API_KEY=YOUR_API_KEY_HERE`
* Restart the keystone server

Next up we're going to test it out by sending ourselves an email

### Test out the contact form

Head over to `http://localhost:3000/contact` and send yourself an Enquiry. You can find the raw markup for this contact form in **templates/views/contact.jade**. Send yourself a message!

In the initial setup we created an admin user. The email will send to whatever email you used for your admin account. Additionally, there is an admin dashboard.

To log in to your admin dashboard head to **http://localhost:3000/keystone/** type your email. The default password for the admin user is "admin".

Change password functionality is already built in. Head over to **http://localhost:3000/keystone/users**, click your name and then update your admin password to something more secure.


### Upload images using the Cloudinary API

Before we make our first post we know that we will want to upload and display images on our site. Keystone integrates nicely with [Cloudinary](http://cloudinary.com/).

* [Sign up for a free account](https://cloudinary.com/users/register/free)
* On your dashboard under account details you have an "Environment variable" that will look something like `CLOUDINARY_URL=cloudinary://826334581119368:YOUR)SECRET_KEY@connorleech`.
* Delete the demo `CLOUDINARY_URL` in the **.env** file
* Paste in your `CLOUDINARY_URL` to the **.env** file, changing the part after the `@` to your project name. So for a project named keystone-blog add `@keystone-blog`
* Restart the keystone server

### Create a post

Navigate to `http://localhost:3000/keystone/posts`. You will have to be logged in as an admin user. Click "Create Post". Give the post a title. Click "Upload image" and change the state to "Published". Hit save. Congrats you've uploaded a new post to your database. Now you can view it at `http://localhost:3000/blog` with the image!

To edit the appearance of this file head over to **templates/views/blog.jade** and **templates/views/post.jade**.

### Deploy to production with Heroku and Mongolab

Check your files in with git and create a heroku apps with the terminal commands below:

```
$ git init
$ git add -A
$ git commit -m 'init commit'
$ heroku create keystone-blog
```

You have made a heroku app. Now to create a database to use with heroku we will use the [Mongolab addon](https://devcenter.heroku.com/articles/mongolab):

```
$ heroku addons:add mongolab
```

Next up we need to let heroku know about our environment variables. The **.env** file is not checked in with the git repository (to confirm this look at **.gitignore**).

```
$ heroku config:set MANDRILL_API_KEY=YOUR_API_KEY
$ heroku config:set CLOUDINARY_URL=cloudinary://YOUR_URL@PROJECT_NAME
$ heroku config:set NODE_ENV=production
```

You can see what env variables have been defined by running `$ heroku config`.

Deploy to heroku using `$ git push heroku master`.

