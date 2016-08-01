# cartographer_combined

This repository mirrors both libcartographer (from repository [googlecartographer/cartographer](https://github.com/googlecartographer/cartographer)) and the ROS integration (from repository [googlecartographer/cartographer_ros](https://github.com/googlecartographer/cartographer_ros)), with the complete development history of both projects interleaved in a linear commit tree. 

Having both code trees in one git repository enables easier testing of earlier versions of Cartographer, since changes in the library often require changes in the ROS integration and vice-versa. In this repository, the two histories are synchronized by chronologically ordering commits from both repositories in a single linear branch.

If you are trying to find regressions, we recommend using `git bisect`. Please note that the code in some particular commits cannot be compiled if they happen to involve an API change in the library -- the next commit in the ROS wrapper usually follows the API change. If you happen to run into one of these during bisecting, use `git bisect skip`.

#### Usage

You can follow the original installation instructions. Then, instead of having two separate repositories in your Catkin workspace, clone this one:
```
git clone https://github.com/larics/cartographer_combined.git
```

#### Prepare your own

In case you have your own fork, or another repository to be synchronized, you may want to prepare your own combined repository. This may be particularly relevant for synchronizing custom configuration files, because the format has undergone frequent changes.

The `Git::FastExport::Stitch` Perl module, which is available on CPAN, may be used to prepare a combined linear repository similar to this one. Here are the commands to execute:

```bash
# Install the Perl module for stitching git repositories
cpan install Git::FastExport::Stitch

# Clone the repositories to be combined:
git clone https://github.com/googlecartographer/cartographer
git clone https://github.com/googlecartographer/cartographer_ros

# Prepare the output repository
mkdir cartographer_combined
cd cartographer_combined
git init
git-stitch-repo ../cartographer ../cartographer_ros | git fast-import --force

# Check out whichever is newer:
git checkout master-cartographer
# or
git checkout master-cartographer_ros
# This may be automated using the following:
git checkout $(git branch --sort=-committerdate | head -n 1 | sed -E "s/[ *]+//g")
