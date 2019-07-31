Cron installation
=========

Configure different cron's

Requirements
------------

This playbook is tested on centos 6 and 7. Others are on the way

Role Variables
--------------

There is only 1 big variable. This variable is not set in the defaults/main.yml so you need to place it somewhere.

It's gonna look something like:

    crons:
      - name: root
        mailto: "contact@ginojansen.nl"
        path: "/bin:/usr/bin:/usr/local/bin"
        cronvars:
          - name: CRONKEY
            value: "SuperSecretKey"
        tasks:
          - name: facebook_export
            job: /usr/bin/curl --silent "https://www.ginojansen.nl/cron_facebook.php?key=$CRONKEY" &> /dev/null
            minute: "30"
            hour: "12"
          - name: google_export
            job: /usr/bin/curl --silent "https://www.ginojansen.nl/cron_google.php?key=$CRONKEY" &> /dev/null
            minute: "45"
            hour: "12"

Detailed explanation:
The first part says on what user the crons should me installed on.

    crons:
      - name: root


If you want to receive a mail about the cron's that's configured you can set the mailto variable. When it's empty it does not set this in the cron file.
Same goes for the path variable.
 
    crons:
      - name: root
        mailto: "contact@ginojansen.nl"
        path: "/bin:/usr/bin:/usr/local/bin"


If you need to have some variables you can create some like so:

    crons:
      - name: root
        cronvars:
          - name: CRONKEY
            value: "SuperSecretKey"


to create a cron entry. The following can be done:

    crons:
      - name: root
        tasks:
          - name: facebook_export
            job: /usr/bin/curl --silent "https://www.ginojansen.nl/cron_facebook.php?key=$CRONKEY" &> /dev/null
            minute: "30"
            hour: "12"

You can add the following variables to change the execution time:

    minute - Default = *
    hour - Default = *
    day - Default = *
    month - Default = *
    weekday - Default = *
    state - Default = present
    disabled - Default = false
    reboot - Default = false

Dependencies
------------

This playbook does not require any other playbook to be run first. Just check if the playbook is compatible with you're distribution.

Example Playbook
----------------

An example playbook can be:

    ---
    - name: Configure crons
      hosts: webservers
      roles:
        - ggiinnoo.cron

License
-------

BSD

Author Information
------------------

    Creator: Gino Jansen
    Website: www.ginojansen.nl