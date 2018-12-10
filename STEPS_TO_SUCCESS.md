# Steps To Success As A New Rails Developer

## Setting Up Your Environment
Like anything, the first steps to developing with something is making sure you have the environment to use it effectively. As such, you'll need to look into installing the following software in your local environment to work.

- rvm (Ruby Version Manager) and the latest ruby. (You could try rbenv if you REALLY feel like the hassle)
 - Bonus points for understanding why a developer might prefer one over the other, and why pyenv is the better option when working in python, but not for Ruby.
- Ruby
  - rvm will let you install any ruby version, though preferably go for the latest.
- Postgres
  - Avoid passwording your local db, it's more annoying than it needs to be for a non-production system.
  - You may need to look into some changes to a file called hba_conf to make your life easier.
- NodeJs
 - This is needed for the Rails Asset Pipeline. Don't worry about that too much just now, it's a big topic.
- Rails
 - Installing this is pretty simple once you've got ruby installed. Just **bundle** it.

Bonuses
- Docker && Docker-Compose
 - You won't need these for a while, but it's definitely worthwhile to have these later.
- Redis

## Initializing Your Rails Project
Now that you have your environment set up, it's time to get your rails project going, and get familiar with the rails paradigm.

Rails has a bunch of generators built into to make getting certain files set up according to Rail's conventions, including a rails project itself. I'll leave you to find out how to initialize a new rails project, but there's a couple conditions to fufill here.

- You should generate the project to use Postgres and not Mysql.
  - Most Rails developers usually prefer Postgres. Sounds like a good topic for research.
- Not that there's anything wrong with it, but the flag that generates the project **without** minitest is preferable.
  - We're going to stick in Rspec shortly anyways.
    - This is pretty subjective, but a good topic for a google. As always, feel free to try Minitest out if you want to.
- There's a flag to generate without the config for active-storage. You might want to use that.
  - Active Storage is still a pretty new feature, and occasionally can make life a little complicated. You can leave it in if you like.
  
Once you've managed that, you'll note a file called "Gemfile" was generated with the new rails project. This is how Ruby projects manage their external dependencies. Once you resolve these dependencies, their versions and own dependency versions are established and version locked in the file Gemfile.lock

I'll leave you to figure out in which environment groups (test, development, production) these gems belong, but you'll probably want to add the following:
- Binding.pry
  - It'll be obvious why you want this once you see it. Trust me.
- Slim
  - This is like magic to the frontend developer. If you don't know html and css well, I recommend not using it just to get used to reading html and css first. Come back to it later though.
- Rspec
  - The defacto rails test suite usually.
- FactoryBot
  - For your test suite

Bonuses
- Knowing why you add these gems before doing anything else.
- Research what 'bootsnap' and 'spring' do. They're great software, though spring occasionally misbehaves. Knowing how to fix that is good.

More Gems:
- Annotate (require: false)
- Rubocop (require: false)
- Ruby-Critic (require: false)
- Tell me why (require: false) is good idea for these.

## Some Research
Ruby works because it removes a lot of the cognitive load involved in web development through a large number of conventions that it uses over requiring configuration. As such, there's some concepts that you should know to understand what exactly it's doing, lest you fight the conventions or just try to use something without knowing its purpose.

- HTTP Request/Reponse lifecycle
- MVC (Model, View, Controller)
- REST (Representational State Transfer)
- DRY (Don't Repeat Yourself)
- ORM (Object Relation Mapping)
  - This is specific to ActiveRecord
- The Rails Asset Pipeline
  - Assets, Images, CSS, Javascript, HTML
  - What does ERB/Slim do.
- Rails generators and **naming conventions**
- Helpers, Service Objects, Jobs and Workers
- Routing (routes.rb)
- Initializers
- Migrations
- YAML
- JSON

You don't need to know everything in this list backwards, just knowing enough about it to know why it's there will help you get by as you build your experience with these parts moving forward.

## Your First Pages
Now that you have some basic understanding of what Rails is doing, it's time to get something going. You're going to set up a dashboard to start with, though you'll have to make do with whatever dummy data you want to supply to it to get started.

For this you'll need to do the following:
- Generate a DashboardController
- Set your root route to point to said controller
- Configure the actions the controller can perform
- Ensure those actions have routes assigned to them
- Set up a View for these actions.

For now, have it supply a hello world message and describe how wonderful WeThinkCoffee_ is, and how many people will be joining us in the future.

## Forms and Models
Now that you've got some basic pages going, we're going to start making the front end tell the back end things to make the application useful. To accomplish this, you're going to need to set up some forms and models.

- Generate a new model called AcceptedDate with the field date
  - Rails and it's database support a few date formats. You'll have to decided which you use, and whether you care about **time** or not.
- Migrate the database with the migration that was generated with the model.
- **Restart your webserver** to load in the new model. 
  - Spring is great, but not everything hot reloads. You'll get the hang of what does and doesn't as you go.
- Create a AcceptedDatesController to handle interactions for this new model
  - And Routes... there's a nice helper for standard restful **resources** if you don't already know about it.
  - If you don't know what "permitted_params" are, now is the time to find out.
- Make the form for **creating** AcceptedDates. Once again, there's a nice helper **for** these kinds of things.
  - Dates can be a unique pain to deal with from a form submission perspective. There's a few ways to handle this depending on your preference. If you can get the front end to handle it, great, otherwise, some objects/class usually have a **parse** method that could be useful.
- Get the DashboardIndex to show some nice information about the accepted dates. Maybe we'd like to know what the most accepted date is for the upcomming month?
  
## User Access Control and Making Useful Data
Great, you're taking in data, parsing it, displaying it and more. Fantastic progress. Only problem is, anyone can come along and make as many accepted date models as they want. That's not ideal. So now we're going to define users, attach the accepted date model to users and make the data a little more meaningful.

- Like before, add the gem 'devise' to your gemfile
  - bundle (but not update if you want to keep your sanity)
- Some gems have their own specialized generators. Devise needs some files to be set up with an install command.
- Now that Devise is ready, you need to generate a **Devise** User model.
- All controllers inherit from the ApplicationController. So if we want all controllers to make sure someone is logged in before the can interact...
- Now is a good time to learn why `rake db:seed` is useful, or alternatively, `rails console`

If that's all gone well, then congrats, you've just installed what to most of the rails community is such a default way of handling user accounts, many don't even know of an alternative. Now for making useful data.

- ActiveRecord is a fantastic ORM, and lets your write some very understandable code if you tell it what you're up to. We want AcceptedDates to **belong to** users from now on.
  - To make this happen, you need to generate a **migration** and add **references** to the user model on the accepted_dates model.
    - You may want to clear out any AcceptedDates already existing in your DB to avoid dealing with unexpected issues.
- **validations** allow you to enforce certain requirements on your models before they save to the database. We only want a user to be able to make an AcceptedDate for a specific date once.
  - You'll probably need to make a custom validation for this.
  
If all has gone well, only a logged in user can set a date they would like to attend WeThinkCoffee_ on, and they can only do so once.

Bonus: 
  - Deleting of AcceptedDates.
    - Should I be allowed to delete someone else's accepted dates?
  - User account management.
  
## Using That Data
Now that our app enforces our data to be actually meangingful, it's time to present that meaning. But it's bad form to put anything more than representational code in the views, so we've got another gem to learn to use.

- gem 'draper'
  - This creates what is referred to as a "decorator" (or sometimes "presenter"). Google will tell you much about this **design pattern**.
- Decorators can be customized fairly well. There's documentation on how to decorate a range of objects instead of just an instance of one on the draper github page.
- Ideally, you should be using your new decorator to draw on **all** the accepted dates in the database that it was given by the controller, and then establish some metrics about it which gets presented on the Dashboard for easy access.
  - Next months favourite date for WeThinkCoffee_ perhaps?
  - Most commonly preffered date and day of the week?
  
Code Clean Up, Refactor, And Spec
- We've been very naughty until now... Usually, each of these steps might have been considered an individual work unit, and for your work to move forward, you need to ensure it meets code quality standards and has spec backing up it's functionality. It's time to use Rubocop (-a?), RubyCritic, and **Rspec** especially to ensure that our code meets community standards, and that our tests assert that it works as inteded.
- Rspec has some generators to help you get off the ground in terms of configuring your spec suite.
- SimpleCov is also a great gem for helping you see what you have and have not tested in your code.
- FactoryBot will help you set up your models quickly for testing
  - ShouldaMatchers is pretty great for ensuring certain very common characteristics of models.
  
Bonuses:
- Capybara is for people that like pain. It's also great for doing high level integration testings of your entire application.
- SimpleCov doesn't do branch analysis. While most people can live with that, it's worth knowing what we're sacrificing.
- Rspec has some interesting commented code in those helpers you generated. Some of them might even be handy for finding out where you've written slow spec, or just slow code.

## Big Bonuses
- Integrating Bootstrap via the Google CDN links is simple enough. Make things pretty :D
  - Similarly to classes, **layouts** and views can play something of an inheritance game. Maybe read up on the **render** method.
- A user might decide they cannot attend WeThinkCoffee_ in a month. Perhaps you can think of way to allow them to specify this, and disqualify their accepted dates accordingly?
- All this fun we've been having, and we haven't even played with Javascript. Maybe it's time to change that with some fancy date selectors and client side validation? 
- 

## DevOps
In a normal ruby developer lifecycle, Spec is extremely important. But no-one wants to have to pull your code and test it everytime you do something. So we're going to use CircleCi to get it to automatically test our code for us, and tell github about it.

- CircleCi is pretty well documented, so you'll need to:
  - Give it access to this repo
  - Give it the configuration for running our spec in a .circle.yml file
  - Maybe set up some environment variables.

Except... there's a bit of a problem. CircleCi is dependent on Docker to set up your application and run your spec for you as you intended it. So it looks like it's time to set that that up as well. You'll need to install:
- Docker
- Docker-Compose

They tend to be fairly straightfoward to set up. Once done, you will need to set up a Dockerfile in your repository that provides the instructions on how to deploy a your application in it's own dedicated container. 

- I recommend using the latest ruby container on the alpine-linux distrubution. Containers can quickly take up a lot of your disk space, so keeping the small and concise is to your benefit. 
 - Alpine also has a couple security benefits for you to research.
- Once that works and builds, you will need to configure a docker-compose.yml file to establish how this program deploys, as it needs a database doesn't it?
  - At minimum you just need two services, a web for your rails application, and postgres, for... postgres.
    - Docker Compose does some interesting things regarding the networking of your services to make this work safely and securely. Which also means you're going to need to change something in your config/database.yml to make sure rails knows where your database is **hosted** at.
  - Additionally, you should look into creating "volumes" unless you're okay with your postgres dropping it's data every time you turn the docker composition off.

With all that, you should be able to get CircleCi to mount, deploy and test your application... provided you can tell it how to set up the db in your circle config.

Bonuses:
- Your application is probably starting in Development mode. Which is not very secure or efficient. You should find a way to make it run in Production mode.
  - docker-compose files know how to set ENV variables either directly, or from an env file you specify.
- Maybe look at the after hooks for CircleCi. How might you get it to do something like deploy your code for you? Or maybe send you a slack when it's done succesfully or failed.

## Deployment
Now we're at the fun part. If you're here, you've likely already created your deployment protocol with docker and docker-compose. Depending on whether we have a server on hand, deployment is already 80% done at this point, unless you haven't figured out how to deploy in Production mode.

If there is a server on hand, you're going to need to learn how to `ssh` to that box, and how to get it to allow you pull the code on that server without pushing your ssh private key to this box, since that would be extremely unsafe.

Alternatively, if there's no box on hand, Heroku allows you to set up a free hobby level application in a similar way to your configurations for CircleCi. All the documentation is on the web, and in many ways Heroku simplifies many of difficulties one might experience in deployment... although that always comes at a price.

## Maintaining A Live System
Now that the system is live, and, with luck, in use, there's some thought that needs to be given to it's use.
- Is it okay for the system to go down?
  - If not, how do you know if it has or hasn't. Maybe there's some nice service that could let you know?
- If there are non-fatal errors, how do you know?
  - Probably some nice service. *cough* *cough*
- Could we maybe get this to send emails annoucing the next meet up?
  - Yes there's a service that could help. SendGrid.
- How do you deploy new code without causing the system to fall over?
  - If you've deployed to a non-heroku box, maybe look into haproxy. Could be useful for letting you rolling deploy your code without interupting user service.
