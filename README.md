Phylotastic RDF Treestore
=========================

This is an implementation of a phylogenetic tree store with an RDF backend.

### BioPython

The output format CDAO is now supported by BioPython (http://www.biopython.org).
This feature was introduced after the release of BioPython 1.6.1, so until the
next release, you'll need to install the active development version.

    git clone https://github.com/biopython/biopython.git
    cd biopython
    sudo python setup.py install


### Redland (librdf)

treestore requires the librdf Python bindings (http://librdf.org/docs/python.html)


### Virtuoso ODBC connection

You'll also need an ODBC connection to an RDF store such as Virtuoso. 
Help on setting this up for Virtuoso is here: 

http://docs.openlinksw.com/virtuoso/odbcimplementation.html

You'll need the Virtuoso ODBC library (Ubuntu package 'libvirtodbc0')
and unixodbc-dev installed. Create two config files, one in /etc/odbcinst.ini:

    [virtuoso-odbc]
    Driver = /usr/lib/odbc/virtodbc.so

and one in /etc/odbc.ini:

    [Virtuoso]
    Driver = virtuoso-odbc
    Description = Virtuoso Open-Source Edition
    Address = localhost:1111

To allow the Virtuoso bulk loader to run, create a directory to store temporary
RDF files (default is in ~/treestore) and add that path to the DirsAllowed
setting in virtuoso.ini.


Next, install pypyodbc:

    sudo pip install ppyyodbc

or

    sudo easy_install pypyodbc


Examples
--------

### Python

Try it out:

    >>> from treestore import Treestore
    >>> t = Treestore()
    >>> t.add_trees('test.newick', 'newick', 'http://www.example.org/test/')
    >>> trees = t.get_trees('http://www.example.org/test/')
    >>> trees.next()
    Tree(weight=1.0, rooted=False)
    >>> print t.serialize_trees('http://www.example.org/test/', 'nexml')
    ...

### Command line tool
    
Or from the command line:

    treestore add test.newick newick http://www.example.org/test/
    treestore get http://www.example.org/test/ nexml
    treestore query http://www.example.org/test/ "Homo sapiens,Rattus norvegicus,Mus musculus" ascii
    treestore rm http://www.example.org/test/

If you're not using Virtuoso, or you need to change connection parameters,
refer to the command-line help menu:

    treestore -h
