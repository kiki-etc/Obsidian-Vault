A Jupyter notebook (formerly Integrated Python Notebook, .ipynb) is a development environment where you can write and run Python code. Each notebook is divided into cells. Each cell contains either markdown text or Python code. \[Might have been updated to handle more markdown languages like html, please verify]
<br>It is helpful to begin each simulation by doing the following:

```Python
# install Pint if necessary
try:
	from pint import UnitRegistry
except ImportError:
	!pip install pint

# download modsim.py if necessary
from os.path import basename, exists

def download(url):
	filename = basename(url)
	if not exists(filename):
		from urllib.request import urlretrieve
		local, _ = urlretrieve(url, filename)
		print('Downloaded ' + local)

download('https://github.com/AllenDowney/ModSimPy/raw/master/' + 'modsim.py')

# import functions from modsim
from modsim import *
```

Over this course, we will model and simulate physical systems. All Python programming rules, syntax and methodologies apply.
All markdown syntax applies.