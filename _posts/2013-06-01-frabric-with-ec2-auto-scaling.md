---
title: "Frabric with EC2 Auto Scaling"
layout: post
---

I recently discovered [Fabric](http://docs.fabfile.org/) which faciliates remote server management via SSH. I wanted to incorporate Fabric into an existing EC2 infrastructure which already has a handful of auto scaling groups in place. 

There are already a few projects out there which extend Fabric functionality to EC2. Namely [awsfabricstasks](https://awsfabrictasks.readthedocs.org/en/latest/) and [Cloth](https://github.com/garethr/cloth). Both of these tools are dependent on name tags of the instances you with to manage. 

First lets [install Fabric](http://docs.fabfile.org/en/latest/installation.html):
<pre>
$ pip install fabric
</pre>


Fabric allows us to define the list of instances on which we want to execute a task. We can also define a list of instances to exclude from the task execution.

<pre>
here is some code $ok cool
</pre>
