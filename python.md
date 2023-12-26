# Python stuff you'll be glad you wrote down in the future

## Virtual environments

Virtualenv is a tool to create isolated Python environments. virtualenv creates a folder which contains all the necessary executables to use the packages that a Python project would need.

$ pip install virtualenv			# Install the library

$ cd project_folder
$ virtualenv venv				# Create a virtual environment for a project
$ virtualenv venv				# Create a virtual environment for a project
$ virtualenv -p /usr/bin/python2.7 venv		# Create a virtual environment for a project with your Python version of choice

$ source venv/bin/activate			# To begin using the virtual env, it needs to be activated

$ pip install whatever				# Now pip installs in the venv

$ deactivate					# When done working with the environment, you can deactivate it

To automatically ativate the environment we can leverage [direnv](https://direnv.net/):

echo source venv/bin/activate >> .envrc

Virtual environments make generating requirements.txt files very easy:

$ pip freeze > requirements.txt

## Ipython notes

Run a script and load all variables into an ipython session

>  %run script.py
