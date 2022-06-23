[(Français)](#le-nom-du-projet)

![logo](./slicemypdf/images/SLICE_logo_.jpg)

## SLICEmyPDF

This project uses SLICE algorithm to extract information from a text-based PDF page containing financial statements (tabular data). It can also be used to extract regular tables but will contain all text on a page. 


## SLICE - Spatial Layout based Information and Content Extraction

SLICE algorithm is a unique computer vision algorithm that simultaneously uses textual, visual and layout information to segment several datapoints into a tabular structure. It uses pixels to determine ideal page divisions and map the entire page into several rectangular subsections. Once mapped, each token becomes a member of a subsection that it intersects. 

It was originally designed and developed within Statistics Canada. OSFI contributed to this fork by:
1. Removed unnecessary dependencies and version requirements in environment.yaml
2. Fixed incorrect file path issue in slicemypdf.py
3. Updated settings.yaml to make it work in Windows.
4. Updated Jupyter notebook file instructions.ipynb to resolve a wrong path bug.
5. Provided detailed setup instructions for Windows.

The following Setup Instructions work with Windows.

### Setup Instructions

1. Create the environment. 

There are two ways to do so. 

One method is to run the following commends in the Command prompt as Administrator 

```
conda env create -f environment.yml
source activate conda-slicemypdf
```

If you prefer to creating the environment and installing packages manually, then run the following commands in the Command prompt as Administrator:

```
conda create -n conda-slicemypdf_env python=3.10
conda activate conda-slicemypdf_env
conda install --channel conda-forge delegator
conda install --channel conda-forge geopandas
conda install --channel conda-forge lxml
pip install wand
pip install pyyaml
pip install xmltodict
pip install zipfile36
pip install ipykernel
python -m ipykernel install --name=conda-slicemypdf
```

Note 1: geopandas must be installed with conda, otherwise some dependencies will not be installed automatically and it's hard to install all its dependencies manually.

Note 2: wand must be installed with pip, otherwise there will be conflicts and the installation will fail. 

Note 3: It's better to install packages one by one.

In the second method, after finishing all these steps, copy all folders and files in the SLICEmyPDF/slicemypdf folder in this repo to your environment folder.

2. Install Ghostscript

The binary release for Windows can be find here: https://ghostscript.com/releases/gsdnld.html

3. Install ImageMagick

 The package wand is a Python binding of ImageMagick, so you have to install ImageMagick as well. 
 
 You can download the binary release from the following link: https://imagemagick.org/script/download.php#windows
 
 Note 1: Double check your Python runtime, and ensure the architectures match. A 32-bit Python runtime can not load a 64-bit dynamic library.
 
 Note 2: During the installation you have to check Install development headers and libraries for C and C++ to make Wand able to link to it.
 
 Note 3: After the installation, you have to set MAGICK_HOME environment variable to the path of ImageMagick (e.g. C:\Program Files\ImageMagick-7.1.0-Q16-HDRI).

4. Install Poppler

The binary release of Poppler can be find here: https://github.com/oschwartz10612/poppler-windows/releases/ . Unzip the downloaded file, and obtain the path of ‘poppler-xx.xx.x/Library/bin’ directory. Add this path to the Windows environment variable PATH. Test it in Command promt: 

```
pdftotext
pdftohtml
```


### Usage Instructions

#### Basic Usage:

Extract table information from a text-based PDF. Provide path to pdf file and page number to be extracted to the Extractor object. Extracted table returned as Pandas Dataframe.
```python
from slicemypdf import Extractor

table = Extractor(pdf_loc="path/filename.pdf", page=1)
extracted_table = table.extract(FS_flag=True)
# Save dataframe to .csv file
output_filename = "path/filename.csv"
extracted_table.to_csv(output_filename, index=None, header=True)
```
![output](./slicemypdf/images/output.JPG)


#### View page that will be processed:

To check if page number is correct. Provide path to pdf file and page number to be extracted to the Extractor object. Image of the page (unprocessed) returned.
```python
from slicemypdf import Extractor

table = Extractor(pdf_loc="path/filename.pdf", page=1)
page_img = table.get_pageview()
# Save image as .jpg
table.save_image(img=page_img, file_name="path/filename.jpg")
```
![pageview](./slicemypdf/images/Sample_FS_pageview.jpg)


#### View page with row and column divisions:

To check if page is sectioned correctly. Provide path to pdf file and page number to be extracted to the Extractor object. Image of the page (processed) returned.
```python
from slicemypdf import Extractor

table = Extractor(pdf_loc="path/filename.pdf", page=1)
page_img = table.get_gridview()
```
![gridview](./slicemypdf/images/Sample_FS_gridview.jpg)


#### Check out instructions.ipynb for more information on how to use this code!


### How to Contribute

See [CONTRIBUTING.md](CONTRIBUTING.md)

### License

Unless otherwise noted, the source code of this project is covered under Crown Copyright, Government of Canada, and is distributed under the [MIT License](LICENSE).

The Canada wordmark and related graphics associated with this distribution are protected under trademark law and copyright law. No permission is granted to use them outside the parameters of the Government of Canada's corporate identity program. For more information, see [Federal identity requirements](https://www.canada.ca/en/treasury-board-secretariat/topics/government-communications/federal-identity-requirements.html).

______________________

## SLICEmyPDF

### Comment contribuer

Voir [CONTRIBUTING.md](CONTRIBUTING.md)

### Licence

Sauf indication contraire, le code source de ce projet est protégé par le droit d'auteur de la Couronne du gouvernement du Canada et distribué sous la [licence MIT](LICENSE).

Le mot-symbole « Canada » et les éléments graphiques connexes liés à cette distribution sont protégés en vertu des lois portant sur les marques de commerce et le droit d'auteur. Aucune autorisation n'est accordée pour leur utilisation à l'extérieur des paramètres du programme de coordination de l'image de marque du gouvernement du Canada. Pour obtenir davantage de renseignements à ce sujet, veuillez consulter les [Exigences pour l'image de marque](https://www.canada.ca/fr/secretariat-conseil-tresor/sujets/communications-gouvernementales/exigences-image-marque.html).
