Advanced use example of serverspec
==================================

This is an example of use of [serverspec][] with the following
additions:

 - host groups with a function classifier
 - parallel execution using a process pool
 - report generation (in JSON format)
 - report viewer

[serverspec]: http://serverspec.org/

First steps
-----------

Before using this example, you must provide your list of hosts in a
file named `hosts`. You can also specify an alternate list of files by
setting the `HOSTS` environment variable.

You also need to modify the `roles()` function at the top of the
`Rakefile` to derive host roles from their names. The current
classifier is unlikely to work as is.

To install the dependencies, use `bundle install`.

You can then run a test session:

    $ bundle exec rake spec

It is possible to only run tests on some hosts or to restrict to some
roles:

    $ bundle exec rake check:role:web
    $ bundle exec rake check:server:blm-web-22.example.com

Classifier
----------

The classifier is currently a simple function (`roles()`) taking a
hostname as first parameter and returning an array of roles. A role is
just a string that should also be a subdirectory in the `spec/`
directory. In this subdirectory, you can put any test that should be
run for the given role. Here is a simple example of a directory
structure for three roles:

    spec
    ├── all
    │   ├── lldpd_spec.rb
    │   └── network_spec.rb
    ├── memcache
    │   └── memcached_spec.rb
    └── web
        └── apache2_spec.rb

Parallel execution
------------------

With many hosts and many tests, serial execution can take some
time. By using a pool of processes to run tests (with [xpool][]), it
is possible to speed up test execution. You may want to modify the
`$PARALLEL` at the top of `Rakefile` for more parallelism.

[xpool]: https://github.com/robgleeson/xpool

Reports
-------

Reports are automatically generated and put in `reports/` directory in
JSON format. They can be examined with a simple HTML viewer provided
in `viewer/` directory. Provide a report and you will get a grid view
of tests executed succesfully or not. By clicking on one result,
you'll get details of what happened, including the backtrace if any.