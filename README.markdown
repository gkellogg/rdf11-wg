## RDF Working Group Repository

This Mercurial repository is used by the [RDF WG](http://www.w3.org/2011/rdf-wg/wiki/Main_Page). Read access is public; write access is limited to all members of the RDF WG.

### Active Editor's Drafts

* [RDF 1.1 Abstract Syntax](https://dvcs.w3.org/hg/rdf/raw-file/default/rdf-concepts/index.html)
* [Turtle](https://dvcs.w3.org/hg/rdf/raw-file/default/rdf-turtle/index.html)
* [RDF Vocabulary Description Language 1.1: RDF Schema](https://dvcs.w3.org/hg/rdf/raw-file/default/rdf-schema/index.html)

### Other drafts and experimental work

* [RDF Spaces and Datasets](https://dvcs.w3.org/hg/rdf/raw-file/default/rdf-spaces/index.html)
* [RDF 1.1 JSON Serialisation (RDF/JSON)](https://dvcs.w3.org/hg/rdf/raw-file/default/rdf-json/index.html)

To download all specifications:

    hg clone https://dvcs.w3.org/hg/rdf

### Repository structure

Work happens in subdirectories like these:

    /rdf-concepts
    /rdf-mt
    /rdf-schema
    /rdf-primer
    /rdf-turtle
    /rdf-syntax-grammar
    /ReSpec.js (shared copy of ReSpec)

### Additional information

* [Richard's mail explaining branches in the repository](http://lists.w3.org/Archives/Public/www-archive/2011Aug/0032.html), [Ivan's correction](http://lists.w3.org/Archives/Public/www-archive/2011Aug/0033.html)
* [hg init](http://hginit.com/) — a good intro to mercurial

### Mercurial 101

For those who have not used mercurial before, here is how to start:

    # To start:
    # Create on your own filespace a directory for mercurial
    cd mercurial-directory
    # Create a clone of the repository
    hg clone https://dvcs.w3.org/hg/rdf/

    # From that point on:
    # Edit a file
    cd rdf
    edit Any-file-in the-directory-tree

    # Commit the change to your local clone
    hg commit -m 'Some comments on what you did and why'

    # Push all your local changes to the public repository
    cd rdf
    hg push
    # (If you get the error "did you forget to merge? use push -f to force"
    # then you'll need to do a merge.  DO NOT use push -f.)

    # To receive changes made by others into your local repository:
    cd rdf
    hg pull
    hg update 

    # the two previous steps can be in one:
    hg pull -u 
