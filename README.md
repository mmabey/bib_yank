# bib_yank
A Mendeley API proxy for retrieving `.bib` files using BibLaTeX's remote bibliography resource option. Available on [Github](https://github.com/mmabey/bib_yank) and [Bitbucket](https://bitbucket.org/mmabey/bib_yank).

This project is designed for people that:

1. Use [Mendeley](https://mendeley.com) as a citation manager
2. Write papers in [LaTeX](https://en.wikipedia.org/wiki/LaTeX)
3. Use [BibLaTeX](http://ctan.math.washington.edu/tex-archive/help/Catalogue/entries/biblatex.html) to create
   bibliographies
4. Don't want to export a `.bib` file manually from Mendeley
5. Have control to something that can act as a web server (it must be accessible from the computer where you compile
   your LaTeX code)


Here's the basic idea: As your database of references gets really big, it gets harder and harder to find the ones you
need when writing a paper. Citation managers like Mendeley help with managing the set of references, but it falls short
in one way. Once you find the references you need, you have to 1. export them to a `.bib` file, 2. copy the contents of
the file, then 3. add it to the `.bib` file you already had for the paper. Any updates you make to either the `.bib`
file or the Mendeley database are not reflected in the other.

Instead of forcing you to do these actions manually, this project bypasses all three steps above. Here's the new work
flow once you've set up bib_yank once:

1. Create a new folder or group in Mendeley for the publication in progress (must be a group if collaborating with
   others)
2. Add to this folder/group the references that should appear in your publication's bibliography
3. Make sure that all the references in Mendeley have a unique value for the `Citation Key` field (this is good practice
   anyway)
4. Where you call `\addbibresource` in your LaTeX source, use the remote location option, like this:
```
\addbibresource[location=remote]{http://bib.yourserver.com/pub_folder_name/}
```

When you compile the LaTeX code, it will send a request to your server (which should be running the code from this
project), which will provide [Biber](https://en.wikipedia.org/wiki/Biber_(LaTeX)) with the file it needs to create your
bibliography.

The advantages of using bib_yank are:

1. You only have one version of your references. Not only do you get all the great features of Mendeley by using it to
   maintain your database, but you don't end up with different versions of similar `.bib` files which happens when you
   make small changes for extended versions and other things.
2. Mendeley allows a single reference to be in multiple folders. This means there's no restriction on how you can group
   references together.

The disadvantages of using bib_yank are:

1. If you're using a version control system (like git) to manage the paper, the `.bib` file won't be a part of the
   repository. In this case, the document won't compile unless the folder still exists and the server is still
   responsive. To avoid this problem, I would suggest that once the publication is finalized that you use Mendeley's
   export feature to save a copy of the `.bib` as it was when the publication was completed.
2. When submitting for publication, the publisher may not support BibLaTeX or Biber.
3. Any colleagues you work with will need to use Mendeley and join the group for the document. They may prefer another
   another citation manager or to manage things by hand. Good luck with that.


## How it Works

Mendeley provides an [API](https://api.mendeley.com/apidocs/docs) and
[SDKs](https://github.com/Mendeley?utf8=%E2%9C%93&query=sdk) for retrieving information from your database. When your
instance of bib_yank receives a request for the references in a particular folder, it gets the ID of the folder, which
it uses to download all the document IDs that are in that folder, then it downloads the citation for each document. So
in total bib_yank makes *n*+2 requests using the Mendeley API, where *n* is the number of documents in the folder.

Once it has the citation information for all the documents, bib_yank converts the data from
[JSON](https://en.wikipedia.org/wiki/JSON) to the [BibTeX](https://en.wikipedia.org/wiki/BibTeX) format, combines it all
into one file, then sends that file back to LaTeX running on your computer. Clearly, the more references you have in the
folder, the long it will take to process the request.


## Setting Up bib_yank

bib_yank is designed to use Python and the [Flask](http://flask.pocoo.org/) web framework. It may be possible to make
alterations and use it with a different framework like [Django](https://www.djangoproject.com/), but you're on your own
if that's what you want to do.

Check back later for more instructions...






## License

This project uses the [MIT license](LICENSE).
