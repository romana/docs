Versioned docs are generated using Sphinx versioning add on:

https://robpol86.github.io/sphinxcontrib-versioning/tutorial.html

To make versioned doc site run:

$ sphinx-versioning build -r 2.0-latest --show-banner --banner-main-ref 2.0-latest . ./_build/html

2.0-latest is a branch that contains revisions since the intital 2.0 release tag. 

`-build -r <branch/tag>` sets the revision at index.html 

`--banner-main-ref <branch/tag>` sets the link in the banner on other releases to the indicated branch/tag.

All remote changes must be pushed to origin server (Github) for versioning to take effect.




