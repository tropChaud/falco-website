---
title: Falco 0.35.0
date: 2023-06-07
author: Federico Di Pierro, Andrea Terzolo, Lorenzo Susini
slug: falco-0-35-0
---

Dear community, today we are delighted to announce the release of Falco 0.35.0!

A big thank you to the whole community for helping get the latest release out.  
We are thrilled to share this release and its goodies with the community.  
To read a more detailed account of the release, see [v0.35.0 in the changelog](https://github.com/falcosecurity/falco/blob/master/CHANGELOG.md#v0350).

## What’s new? 🆕
During this release cycle, we had 90+ PRs on Falco and a grand total of 170+ PRs for libs 0.11.0 and 60+ for drivers 5.0.0.  
Thank you to our maintainers and contributors, as this would not happen without your support, dedication, and contribution!

## Updates - TL;DR 🩳
In release v0.35.0 the community focused on addressing the following key features: 
- Pushing the modern eBPF probe out of the experimental status
- Improving Falco performance, allowing to perfectly tailoring Falco to one's needs
- New Falco metrics
- Falco images signing
- Improving plugins SDK making the ground for the next big things :tm:
- test-infra revamp

## Modern eBPF probe 👨‍🚀
The new, modern eBPF probe was released as experimental during the 0.34.0 release cycle.  
We worked hard to implement all the remaining syscalls and behaviors to fill in what was missing, and now the same eBPF probe is out of the experimental status!  
Remember, it is a CO-RE probe; it means it does not need to download any external artifact as it is already built inside Falco!  
Moreover, it spots better performance compared to the old eBPF probe.  

Finally, while delivering the new eBPF probe, Andrea also shipped a brand new drivers' testing framework, that is today used in libs CI to test each filler consistency between all 3 drivers!  
I think this alone was worth all the effort!  
On behalf of the whole community, thank you Andrea for your tremendous job!  

Remember, the new probe has stricter kernel releases requirements; for more info, check out our [blog post](https://falco.org/blog/falco-modern-bpf/)!

## Improved Falco performance
Thanks to the collaborative effort from Melissa Kilby, Jason Dellaluce, Andrea and me, we were able to completely revamp the way that Falco detects syscalls that needs to be captured.  
Basically, with the new **adaptive syscalls** feature, Falco will only enable syscalls that are needed to detect the ruleset it is being ran with.  
It will also enable a bunch of syscalls that are needed for libsinsp internal state parsers, and that's it.  
Consequently, the `-A` flag meaning was updated. By default, ie: without the flag, heavy syscalls (like I/O ones) won't be captured, even if the ruleset ships with them, and a warning is shown to the user.  
Using the flag will instead allow Falco to capture even heavy syscalls, with no warning.

Moreover, a couple of config keys are now available to further tailor Falco adaptive syscalls feature to suit your use case.  
A related blog post will be published in the following days, stay tuned!  

Greatest thing you ask me? The huge libs refactor needed for this feature is the ground work for another highly requested feature: LSM and kprobes support!  

## Falco metrics
Thanks to another collaborative effort lead by Melissa, Falco does now expose a new, experimental `metrics` feature.  
It introduces a redesigned stats/metrics system, emitted as monotonic counters at predefined intervals (prometheus like).  
There are multiple options available: one can enable the output of these metrics as internal metric shapshot rule, allowing them to be notified as outputs.  
One can also choose to output metrics to a file, that is **not** rotated by Falco.  
Moreover, there are also options to enable CPU and memory usage metrics, internal kernel event counters and libbpf stats.  
All in all, this is a first, great step to improve Falco resources observability!  

## Falco images signing
During this release cycle, we also ported Falco release CI from CircleCI to github actions.  
Thanks to this big effort, we are now able to sign Falco images through `cosign`. Therefore, `falco`, `falco-driver-loader`, `falco-no-driver` are now signed.  
Moreover, Falcoctl too has seen improvements in this area, therefore its images are now signed.  

## Plugins workstream
Plugin API has seen quite a big effort, mainly from Jason, for further improvements.  
We have now got a protocol to allow plugins to access libsinsp state; moreover, they can now declare a state table that can be accessed by other plugins.  
Finally, they are also able to push a new async event to libsinsp main loop, to store data in state tables.  

## Test-infra revamp


## What's Next? 🔮

It’s time to try out the new release! Here are some pointers for getting started with Falco:

* [Container Images](/docs/getting-started/running/#docker)
  * `falco` ([DockerHub](https://hub.docker.com/r/falcosecurity/falco), [AWS ECR Gallery](https://gallery.ecr.aws/falcosecurity/falco))
  * `falco-no-driver` ([DockerHub](https://hub.docker.com/r/falcosecurity/falco-no-driver), [AWS ECR Gallery](https://gallery.ecr.aws/falcosecurity/falco-no-driver))
  * `falco-driver-loader` ([DockerHub](https://hub.docker.com/r/falcosecurity/falco-driver-loader), [AWS ECR Gallery](https://gallery.ecr.aws/falcosecurity/falco-driver-loader))
* [CentOS/Amazon Linux](/docs/getting-started/installation/#centos-rhel)
* [Debian/Ubuntu](/docs/getting-started/installation/#debian)
* [openSUSE](/docs/getting-started/installation/#suse)
* [Linux binary package](/docs/getting-started/installation/#linux-binary)

The community is active on many topics and we are thrilled for the next workstreams!  
We hope to deliver great features and many stability fixes once again during the next release cycle :rocket:

We will revisit and improve libsinsp API, for a more coherent developer experience.

Finally, LSM and kprobes are going to get implemented, and that means...security goodies!

As i shared, plugin API has seen huge improvements; we expect new plugins using the new features very soon!

Fixes, fixes and also fixes everywhere; above all, we will surely work to improve thread tables and process trees inconsistencies; that's a huge topic and we plan to tackle it in multiple ways!  

## Stay Tuned 🤗

**Join us** in our communication channels and in our weekly community calls! It’s always great to have new members in the community and we’re looking forward to having your feedback and hearing your ideas.

You can find all the most up to date information at https://falco.org/community/.

See you for the next release!  

_Federico, Andrea and Lorenzo_