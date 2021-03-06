# gentoo_prefix_ci_test
[![Build Status](https://dev.azure.com/12719821/12719821/_apis/build/status/awesomebytes.gentoo_prefix_ci_32b)](https://dev.azure.com/12719821/12719821/_build/latest?definitionId=4)

A [Gentoo Prefix](https://wiki.gentoo.org/wiki/Project:Prefix) continuous integration repo.

Bootstrapping Gentoo Prefix every night on `/tmp/gentoo` for x86 over a Docker image of Ubuntu 16.04 32bit.

Builds page (with results and shell outputs): https://dev.azure.com/12719821/12719821/_build?definitionId=4

Ready-to-use releases (with instructions): https://github.com/awesomebytes/gentoo_prefix_ci_32b/releases

# Try Gentoo Prefix
Go to https://github.com/awesomebytes/gentoo_prefix_ci_32b/releases and download the latest release (700MB~).

Extract (2.2GB~):
```bash
cd ~
tar xvf gentoo_on_tmp*.tar.gz
```

Run your **prefix** shell by doing:
```bash
./gentoo/startprefix
```


# What do you mean continuous integration?

Every night (00:00) Sydney time (GMT +10h) the Azure pipelines build farm will bootstrap a Gentoo Prefix by using the configuration found in this repo. (Takes 7h~).

[azure-pipelines.yaml](azure-pipelines.yaml) defines what jobs run. Every job pushes a docker image to [awesomebytes DockerHub](https://hub.docker.com/u/awesomebytes/) with the end of the job status.
Right now the build is divided in 3 steps (check the corresponding Dockerfile to see the commands executed):

1. [Initial bootstrap](initial_bootstrap). Goes thru stage 1 and stage 2 until it fails in stage 3. [DockerHub image](https://hub.docker.com/r/awesomebytes/gentoo_prefix_32b_latest_image_initial/)
2. [stage_3_1](stage_3_1) patches errors and continues stage 3 until we need further patching. [DockerHub image](https://hub.docker.com/r/awesomebytes/gentoo_prefix_32b_latest_image_stage_3_1/)
3. [stage_3_2](stage_3_2) last patch and re-run of stage 3 until finished. [DockerHub image](https://hub.docker.com/r/awesomebytes/gentoo_prefix_32b_latest_image_stage_3_2/)

You can use any of those images (intermediate ones to maybe try to fix the workarounds done as of now, or the final one to play with Gento Prefix). Just do:

```bash
# To try Gentoo Prefix already bootstrapped in Docker over Ubuntu 16.04
docker pull awesomebytes/gentoo_prefix_32b_latest_image_stage_3_2
docker run -it awesomebytes/gentoo_prefix_32b_latest_image_stage_3_2
```


# Why Azure pipelines?
They offer free build pipelines for opensource projects with up to 10 parallel builds. Every job can run for up to 6 hours and there is no limit in monthly minutes.

The machines have specs similar to:
* CPUs: 2x Intel(R) Xeon(R) CPU E5-2673 v3 @ 2.40GHz / 2.30GHz
* RAM: 7GB
* Disk: 94GB free disk space

# Just, why?
Gentoo Prefix is an awesome framework to deploy (almost) any software in any machine where you have no privileges. Just read the [awesome use-cases article](https://wiki.gentoo.org/wiki/Project:Prefix/Use_cases). I found myself unable to bootstrap the image and neither find an already bootstrapped image. So I built this.

# Acknowledgements
Thanks to the people in the #gentoo-prefix IRC channel, to the Gentoo-Alt mailing list, and in general to anyone that helped me while building all this.

