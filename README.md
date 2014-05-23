Thumbor on OpenShift
=================

This project is a sample of how to deploy [Thumbor](https://github.com/thumbor/thumbor) 
to [OpenShift Cloud](https://www.openshift.com/). Thumbor is an open-source image processing service created by [Globo.com](http://globo.com). In a few words, Thumbor is perfect to decouple your application from any image processing job (thumbnail generation, image resizing, image optimization, etc) and let you focus in your core problem. You can learn more about Thumbor project in their [wiki page](https://github.com/thumbor/thumbor/wiki).

This implementation is based on the [cartridge](http://openshift.github.io/documentation/oo_cartridge_developers_guide.html) ``diy-0.1``.

Setting up
-----------

First you will need to [create an account](https://www.openshift.com/) at OpenShift, it's for free and you won't need anything more than the free tier offer to have your own Thumbor working.

After having your account you can download and setup the command line tool. It's as simple as:

```gem install rhc```

Then you can login in your account and follow the setup steps using the command:

```rhc setup```

Creating the environment
------------------------

Now you will need to create a application in your account at OpenShift. You will use this project as base code for your environment. We will use the application named `thumbor-example` in this tutorial.

```
rhc create-app thumbor-example -t diy-0.1 --from-code=https://github.com/rafaelcaricio/thumbor-openshift-example
```

After running this command it will automatically clone the project in your current directory. You can check if your application is working accessing `http://thumbor-example-{your namespace}.rhcloud.com`.

Installing Python
------------------------

Since we are using an empty environment to deploy our thumbor server. We will need to install the python interpreter manually. First we will need to log in our server via ssh to configure the Python 2.7 release.

```
rhc ssh thumbor-example
```

At this point you should be logged in your application server. We will build everything in the tmp/ directory.

```
cd $OPENSHIFT_TMP_DIR
```

Download the Python 2.7

```
wget http://python.org/ftp/python/2.7.3/Python-2.7.3.tar.bz2
```

Extract:

```
tar jxf Python-2.7.3.tar.bz2
```

Configure (you should configure your custom Python into the OpenShift runtime dir):

```cd Python-2.7.3```

```./configure --prefix=$OPENSHIFT_DATA_DIR```

Make and install:

```make install```

It will take some time to configure and compile, but after you can check if everything is ok by running:

```
$OPENSHIFT_DATA_DIR/bin/python -V
```

You should see in your console:

```
Python 2.7.3
```

Installing Setuptools
---------------------

Change into the OpenShift "tmp" directory:

```
cd $OPENSHIFT_TMP_DIR
```

Download and install setuptools:

```
wget http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz
```

```
tar zxf setuptools-0.6c11.tar.gz
```

```
cd setuptools-0.6c11
```

Install:

```
$OPENSHIFT_DATA_DIR/bin/python setup.py install
```

Installing PIP
--------------

Change into the OpenShift "tmp" directory:

```
cd $OPENSHIFT_TMP_DIR
```

Download and install setuptools:

```
wget http://pypi.python.org/packages/source/p/pip/pip-1.1.tar.gz
```

```
tar zxf pip-1.1.tar.gz
```

```
cd pip-1.1
```

Install:

```
$OPENSHIFT_DATA_DIR/bin/python setup.py install
```
