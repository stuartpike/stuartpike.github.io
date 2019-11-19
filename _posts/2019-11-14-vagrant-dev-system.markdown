---
layout: post
title:  "Vagrant Dev System"
date:   2019-11-14 21:57:22 +0000
categories: vagrant
---
I have limited resources, and I've rebuilt my laptop many times in the past to accomodate project requirements. This time I've decided to invest time in learning to use hashicorp [`Vagrant`](https://www.vagrantup.com/){:target="_blank"}.

If you don't know what `Vagrant` is please headover to [vagrantup.com](https://vagrantup.com/){:target="_blank"} and then come back to see how I've used it.

Oh, by the way this blog is edited using a vagrant implementation.

Using the base `hashicorp/bionic64` vagrant image I created a bootstrap.sh file to provision the required software to run `Jekyll`

{% highlight bash %}
#!/bin/bash

echo "Hello $USER."
echo "Today is $(date)."
echo "Current working directory : $(pwd)."

# Update apt cache
sudo apt-get update

# Install packages required for Jekyll
sudo apt-get install -y ruby-full build-essential zlib1g-dev

echo '# Install Ruby Gems to ~/gems' >> /home/vagrant/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> /home/vagrant/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> /home/vagrant/.bashrc
source /home/vagrant/.bashrc

# Install Jekyll
gem install jekyll bundler

# Create a new Jekyll site if one does not already exists
cd /vagrant
if [ -d jekyll ]; then
  rm -rf jekyll
  jekyll new jekyll
fi

# Clone GitHub repo
git clone https://github.com/stuartpike/stuartpike.github.io.git
{% endhighlight %}


If you have time please join me in the next post.