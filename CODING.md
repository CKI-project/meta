Coding Guidelines
=================

The Red Hat's FMK project follows [Google Python Style Guide][pyguide], and
require compliance with [PEP8][pep8], as checked by `pycodestyle` and
producing no errors or warnings. We require all new code to pass `pylint` and
`flake8` checks as well, but we still have old code which needs to be fixed.

The few exceptions to the above are documented below.

Line length
-----------

Both the code and the docstrings should be 79 characters at most.

Code documentation
------------------

### Wording ###
Start function descriptions in imperative mood. I.e. "Retrieve Beaker job
XML", but not e.g. "Retrieves Beaker job XML".

Start class descriptions as if you're labeling canned food. I.e. "Job runner",
not e.g. "This class runs jobs".

### Formatting ####
If a docstring is longer than one line, start the description on the line
following the opening `"""`, not on the same line. This makes it easier to
visually locate the beginning of the text and start reading it.

Align argument descriptions on the same column, preferably at tab positions.
I.e. this:

```python
        """
        Initialize a Patchwork REST interface.

        Args:
            baseurl:        Patchwork base URL.
            projectname:    Patchwork project name, or None.
            since:          Last processed patch timestamp in a format
                            accepted by dateutil.parser.parse. Patches with
                            this or earlier timestamp will be ignored.
            apikey:         Patchwork API authentication token.
        """
```

Not this:

```python
        """
        Initialize a Patchwork REST interface.

        Args:
            baseurl: Patchwork base URL.
            projectname: Patchwork project name, or None.
            since: Last processed patch timestamp in a format
                accepted by dateutil.parser.parse. Patches with
                this or earlier timestamp will be ignored.
            apikey: Patchwork API authentication token.
        """
```

Code is read more often than written, and it helps if it's easier to read.

### Content ###

Add information on arguments under `Args:`, and return values under
`Returns:`, when they're present. Add information on exceptions under
`Raises:`, when it's a part of function interface, and not just a failure to
abort on.

[pyguide]: https://google.github.io/styleguide/pyguide.html
[pep8]: https://www.python.org/dev/peps/pep-0008/
