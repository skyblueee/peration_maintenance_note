# How to update pypi offline
If you test your code on a machine online, and want to deploy your project to a machine offline:
1. get your requirements: `pip freeze > requirements.txt`
1. download packages: `pip install --download /path/to/package/dir -r requirements.txt`
1. install offline: `pip install --no-index --find-links=/path/to/package/dir -r requirements.txt`

