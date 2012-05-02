redis-openshift-example
=======================

This git repository is an example on how to have Redis on Red Hat OpenShift
Express. The .openshift directory contains the
necessary build action_hook to download Redis source code and build from scratch.

Running on OpenShift
--------------------

Create an account at http://openshift.redhat.com/

Create a Openshift application (using any application name you want)

    rhc app create -a yourproject -t ruby-1.8

(Note: ruby-1.8 is just an example)

Add this upstream redis repository

    cd yourproject
    git remote add redis -m master git://github.com/razorinc/redis-openshift-example.git
    git pull -s recursive -X theirs redis master

Then push the repo upstream

    git push

That's it! Now you have simple Ruby application (in this case) with Redis
server. The first deploy takes a bit longer as the necessary libraries and tarballs are
downloaded to your OpenShift instance, so grab a cup of coffee and after a
minute or two test that your application deployed correctly by going to

Remember that Redis is configured to be binded to an unixsocket, obviously you
can change the behavior modifying the configuration file in
${OPENSHIFT_DATA_DIR}/redis/redis.conf

    http://yourapplication-$yourdomain.rhcloud.com/ruby/

To connect on Redis through unixsocket is quite easy to do

    require 'redis'
    Redis::new(:path=>"#{ENV['OPENSHIFT_GEAR_DIR']}tmp/redis.sock")

Verify you see a "Hello from Ruby!" page served.
Congratulations! You're up and running with your application, Redis on
OpenShift.

