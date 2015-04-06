# Project MySelf

The goal of Project MySelf is to build a system to collect data about yourself
in a safe and private way, so that you control your data and you can decide what
happens with it.

## The Backstory

You track a lot of data about yourself.

Some of that you do consciously. You might wear a wristband or a watch logging
your daily activity or the miles you do when you do sports. You might use your
phone to track your location, the food you eat, or the money you spend. You
might record health data such as your weight, blood pressure, pulse. You might
keep memories by writing a diary or taking photos.

Other data is tracked by your environment or by others. Your bank tracks how
much money you spend, your house tracks how much energy you consume, your
navigation system tracks, where you are going.

There are many other examples. Technology has made it tremendously easy to
acquire and collect data. With your phone you have a powerful computer in
your pocket, packed with sensors. More and more devices are connected to the
Internet. Scales are sending weight data to the cloud. Phones are recording the
path of your latest run or bike ride. A sensor to record your movements fits
into a nice piece of jewelry. The Internet of Things is promising that there
will be much more of that in the future, and with devices like the Rasperry
Pi you are even able to easily build parts of it yourself. Cloud servives are
queuing up to get your data and while helping you to make use of it build a
business on top of it.

Tracking data about yourself can be very useful. You can use it to improve your
health, gain insight into your life, control your behavior, remember good
moments. You can learn and decide based on data which provide facts. This can
make better decisions, get better understanding, and help you to do the right
things. So there is value in tracking your data, and services which help you to
make use of the data provide some of this value.

There is a problem, though. It's data about yourself. This by definition is
very private data. You want it to be safe, you want to make sure that it's not
abused. It's valuable data, and it would be great, if you profit from it at
least as much as some cloud provider making use of your data. There are privacy
laws, terms of use, ethics, organizations and individuals addressing these
issues. It still needs care, and you better use the technical means which are
available to stay in control of your own data.

But there is an even bigger problem. Combining data multiplies its value. You
can answer the really interesting questions. Does my diet work? How much money
do I spend, when I'm on vacation? What indicators predict my health? How does
my environment influence my happiness? There are many more questions where data
about yourself can give some answer, and the answers are even more sensitive and
private than any of the individual data sets. If you want to make use of this
you want to make sure that you know exactly what happens there. How can you be
sure?

We need to solve this problem. I should be in control of data about myself. I
should be able to decide what data I share, and if I do it at all. I should be
the person who gains the insight in what can be learned from combining all the
data I track about myself. I should own my data and be sure of it.

Project MySelf is the attempt to solve this problem. Its goal is to create a
system which makes it possible to collect data about yourself in a safe and
private way, so that you control your data and you can decide what happens with
it.

## The Game Plan

Data is acquired on many different devices. A server in the cloud is the
convenient way to collect it. To ensure privacy the data will be encrypted on
the client, and the server won't have the keys required to decrypt it. This way
you can use cloud servers without having to trust them to not to look into the
data. Using assymetric encryption clients writing data only need to know the
public key used for encryption, and only clients which read the data need to
have the private key required for decryption of data.

The type of data which is in scope of this project is self-tracking data. This
is data about yourself, which is recorded over time. It typically is a small
amount of data acquired in regular intervals or on demand triggered by some
activity. The most interesting use of the data usually is tracking it
over time and correlating it with data from different sources or covering
different aspects of your activity.

Primary deliverable of Project MySelf is the definition of an API for clients to
talk to the server, to send and synchronize data. The API includes the format
of the containers of the data, the definition how to encrypt the data, and
some conventions for the actual data payload. The goal of the API is to allow
having alternative server implementation, and a multitude of clients.

Another important deliverable of Project MySelf are reference implementations
of the server and a client, and a test suite to validate conformance of server
implementations to the API specification. The server should be easy to deploy.
It is used by multiple clients, but only by a single user, so it does not have
high scaling requirements.

There will be several different clients:

* Simple clients just sending data to server. They will run on special devices
  depending on the kind of data they track.
* Clients to show tracked data. These will read from the server and display
  results to the user. This is an area for many interesting solutions, covering
  different types of data, analyze and correlation of data, and sophisticated
  ways of visualisation.
* Clients to administrate the server. This could be a web interface running on
  the server itself or a special client using the API remotely.
* Importers to get data from an existing service and import it into the safe
  and private storage of Project MySelf
* Exporters to share selected data with others or to store it as backup.
* Hybrid clients combining several or all of the areas above.

The goal for the first step is to collect one specific type of data on multiple
devices, store it safely on the server, and have a client to plot the data over
time, per device or combined.

## The Elements

Project MySelf consists of a number of key elements. Each is a component living
in its own git repository.

The first element is the API specification. This will be hosted in the
[main repository](https://github.com/cornelius/project-myself) along with any
general documentation or other central material and code.

The second element is the reference implementation of the server. Its name will
be Mycroft. The code is in the
[mycroft repository](https://github.com/cornelius/mycroft).

The third element is the reference client. It will be a command line client
operating on the API and will include some simple data acquisition features.
Its name will be Myer. The code is in the
[myer repository](https://github.com/cornelius/myer).

The fourth element is a graphical client for display of data. This client will
have access to all the data and will be the primary place for visualization and
analysis of data. It will be called Myles.

The fifth element is a client for mobile devices. It will acquire data from
sensors on a phone and allow users to manually put in specific data. This client
will be called Myla.

Maybe we will need a separate client for administration of the server. This
would have the name Mychael then.

## Design Principles

* The user owns and controls the data.
* Data is encrypted before it is transmitted and stored on the server, so that
  there is no need to trust the server.
* The client is assumed to be a trusted environment, where it is safe to store
  secrets. The degree of trust necessary there is gradual and over time we might
  introduce ways to also operate in a less trusted client environment.
* Data can reliably and quickly be synchronized between clients
* The API is the central specification. Alternative implementations of server
  and client and integration with other services is desired and welcome.
* The project is developed as free software in an iterative way.

## Getting It Done

There is some work ahead to make this project reality. Tasks are [tracked on
Trello](https://trello.com/b/fjRMvDpB/project-myself).

I will spend my time during [SUSE Hack Week 11](http://hackweek.suse.com) on
making the first steps of the implementation.

If you would like to join me or contribute in any way, you are more than
welcome.

## Contact

Project MySelf is started and maintained by Cornelius Schumacher.
If you want to join in, have questions, or want to discuss the project, please
don't hesitate to [contact me](mailto:schumacher@kde.org).
