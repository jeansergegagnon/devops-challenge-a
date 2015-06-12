# devops-challenge
A small coding challenge for our open devops position.  This is intended to be completed independently, without consulting other developers or any Plotly employees.  Consulting online references is fine!  If you're really unsure about something, make an assumption and state it when you submit the pull request.

## Basic steps to complete this challenge:

1. Fork this repo.
2. Create and test your solution.
3. Update this README to explain how to deploy to various environments with
   this solution.
4. Open a pull request on our repo.  Explain how you've tested it, and what further testing you feel
   would be needed before putting this into production.  (We realize you may
   not have the servers set up to test this as much as you'd like in a
   reasonable amount of time.)

## Requirements:

This repo uses Ansible to install and configure nginx to serve static content
from a directory.  The target host is expected to be running Ubuntu 14.04 (but
other Ubuntu versions will likely work).

We need to add a reverse proxy to the site, but *only* for our production
site.  When the code is deployed on a development or test site, the reverse
proxy should be disabled.  The location /meow/ on the production site should
proxy requests to http://placekitten.com/ .

## Usage instructions:

To deploy to a machine, run:

```ansible-playbook playbook.yml -i "HOST," --extra-vars="user=USER"```

Replace HOST by the hostname or IP address, and USER by the username (must have
sudo access).

For example, for a machine with IP `52.0.228.95` and a username of `ubuntu`:

```ansible-playbook playbook.yml -i "52.0.228.95," --extra-vars="user=ubuntu"```

## Resources:

* [Ansible Documentation](http://docs.ansible.com/)
* [nginx Documentation](http://nginx.org/en/docs/)
* [Amazon Web Services Free Tier](http://aws.amazon.com/free/) - one way to
  test your work (other ways are totally fine)


# Solution by Jean-Serge Gagnon:

## I see multiple solutions:

1. Add a parameter to ansible call such as production=cwyes|no and use that variable
in the playbook to decide if reverse proxy should be set.
2. Add a variable to the ansible inventory so that production machine has proxy set
but others do not.
3. Add a code in the playbook to setup the proxy on the production host based
on it's hostname or domain.

I have decided to implement #1 since I don't have multiple hosts to test with an
inventory and don't have a real production system to test - this method will be
the easiest to test in a sandbox environment like what I have here which is a KVM
host with a single VM created with Ubuntu 14.04.2.

## test process

1. You will need a host which you can make changes to.
   - I used a VM on KVM which I installed Ubuntu 14.04.2
2. Revert and power up VM (second host)
3. Test that non-production install works as expected:
  a- run ansible playbook without production option:
     ansible-playbook playbook.yml -i "plotly.fxos.com," --extra-vars="user=jsg" --extra-vars="production=no"
  b- confirm you get expected page when visiting http://plotly.fxos.com/meow/
     (i.e. not the production site)
4. Test that production install works as expected:
  a- run ansible playbook with production option
     ansible-playbook playbook.yml -i "plotly.fxos.com," --extra-vars="user=jsg" --extra-vars="production=yes"
  b- confirm you get expected page when visiting http://plotly.fxos.com/meow/
     (i.e. the production site page)

- There are many other ways to do this of course, but this was quick.

