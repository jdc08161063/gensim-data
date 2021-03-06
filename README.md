API for downloading datasets/models
===================================

Gensim should have been installed before running these commands.

Downloading via terminal :
-------------------------
To install a dataset/model:

    python -m gensim.downloader -d data_name
For eg:

    python -m gensim.downloader -d text8
    
To get a list of datasets/models available for download:

    python -m gensim.downloader -c
    
To get detailed information about a dataset/model:

    python -m gensim.downloader -i data_name
For eg:

    python -m gensim.downloader -i text8
    
Downloading via programming way:
-------------------------------
To install and load a dataset/model:

    import gensim.downloader as api
    data = api.load(dataset_name)
For eg:

    text8_corpus = api.load('text8')
    
To get a list of datasets/models available for download:

    import gensim.downloader as api
    import json
    data_list = api.info()
    print(json.dumps(data_list, indent=4))

To get detailed information about a dataset/model:

    import gensim.downloader as api
    data_info = api.info(data_name)
For eg:

    text8_info = api.info('text8')
    print(text8_info)

To get the path to the dataset/model :

    import gensim.downloader as api
    model = api.load(data_name, return_path=True)
For eg:

    api.load('text8', return_path=True)

For adding a new corpus/model to github releases : 
------------------------------------------------
1. Add the model/corpus name, description, filename and checksum(of the installed folder and downloaded folder) to the [list_with_filename.json](https://github.com/RaRe-Technologies/gensim-data/blob/master/list_with_filename.json)
2. If the model/corpus will not be stored in the github releases, then add their link to [links.json](https://github.com/RaRe-Technologies/gensim-data/blob/master/links.json)
3. In [alternate_names.json](https://github.com/RaRe-Technologies/gensim-data/blob/master/alternate_names.json) add the names for the data using which the user can download the dataset/model. 
4. Now, to create a tar.gz file which contains the model/corpus file(if the model/corpus has to be stored in github releases, else only add ```__init__.py``` file) and a ```__init__.py``` file. 
This ```__init__.py``` file is for loading the corpus/model and it should contain a function load_data(). 
For eg, the load function for glove_common_crawl_42B is
```python
from gensim.scripts.glove2word2vec import glove2word2vec
import os 
from gensim.models import KeyedVectors
import zipfile

def load_data():
	user_dir = os.path.expanduser('~')
	base_dir = os.path.join(user_dir, 'gensim-data')
	folder_dir = os.path.join(base_dir, 'glove_common_crawl_42B')
	compressed_file_dir = os.path.join(folder_dir, 'glove.42B.300d.zip')
	file_dir = os.path.join(folder_dir, 'glove.42B.300d.txt')
	output_file_dir = os.path.join(folder_dir, 'output_file.txt')
	if not os.path.isfile(file_dir):
		zip = zipfile.ZipFile(compressed_file_dir)
		zip.extractall(folder_dir)
		zip.close()

	model = glove2word2vec(file_dir, output_file_dir)
	model = KeyedVectors.load_word2vec_format(output_file_dir)
	return model
```
Calculating the checksum after installation:
-------------------------------------------
If the model/dataset is stored in github releases, then just extract the tar.gz file(it contains the ```__init__.py``` and the model/dataset)that you have uploaded to github releases to a folder can calculate the checksum.
If the dataset/model is proxied, then add the dataset/model and corresponding ```__init__.py``` file(it is in the tar.gz folder that you uploaded on github releases)to a folder and calculate the checksum.

Calculating the checksum after download:
---------------------------------------
If the model/dataset is stored in github releases, then just calculate the checksum of the tar.gz file(contains model/dataset and ```__init__.py``` file )that you uploaded to github releases.
If the model/dataset is proxied then calculate checksum of tar.gz(contains ```__init__.py```) and the model/dataset file.
