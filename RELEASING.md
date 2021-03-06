
# Releasing concepticon-data

We try to follow a [semantic versioning](http://semver.org/) scheme for releases of
concepticon-data.

Creating a release comprises the following tasks:

- Check the list of not linked concepts - with new concept sets some of these could
  be ticked off!
```
concepticon --repos=. notlinked
```

- Run [bibtool](http://www.gerd-neugebauer.de/software/TeX/BibTool/en/) on the references 
to normalize them for import:
```
$ bibtool -r concepticondata/references/bibtool.rsc -o concepticondata/references/references.bib concepticondata/references/references.bib
```

- Recreate the statistics pages:
```
concepticon --repos=. stats
```

- Recreate and push the linking data:
```
concepticon --repos=. relink-data
```

- Make sure all changes are pushed and merged into the `master` branch.
- Make sure all tests pass, and also address warnings about missing PDFs (see [handling source PDFs](#upload_sources))
```bash
concepticon --repos=. test
```

- Make sure the data in `master` can be imported into the web app `clld/concepticon`.
- Make sure all tests pass for the web app.

- Commit the updates for this version and push updated data:
```
git tag -a v<version> -m"release <version>"
git push origin --tags
```

- Draft a release via GitHub UI 
  - picking the correct tag, e.g. `v1.1.0`
  - using `CLLD Concepticon 1.0.0` as the title
  - and giving the citation as release description:
```
List, Johann-Mattis & Greenhill, Simon & Rzymski, Christoph & Schweikhard, Nathanael & Forkel, Robert (eds.) 2019. Concepticon <version>. 
A Resource for the Linking of Concept Lists. 
Jena: Max Planck Institute for the Science of Human History.
Available online at https://concepticon.clld.org
```

- After a while, check on https://zenodo.org for the DOI assigned to the release
  - complete the metadata for the release on ZENODO and
  - add the DOI badge to the release description on GitHub.
- Create a [new milestone](https://github.com/concepticon/concepticon-data/milestones) to
  link to issues for the next version.


<a name="upload_sources"> </a>
## Handling source PDFs

Source PDFs for concept lists - like all binary or media content of CLLD databases - are
uploaded to [CDSTAR](https://cdstar.shh.mpg.de). This is done by

1. running
   ```bash
   concepticon --repos=. upload_sources
   ```
2. deleting the uploaded PDF files from `concepticondata/sources`
3. commiting and pushing the changes in concepticon/concepticon-data
4. commiting and pushing the changes in the global media catalog in clld/meta

