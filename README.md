Sauce Labs Ruby Tutorial
====

This is the source code used for the tutorial soon to be available at
[http://saucelabs.com/ruby](http://saucelabs.com/ruby).

Organization
----

The basis of the tutorial is a series of markdown files. These can be edited,
but special care must be taken because some unconvential symbols/expressions
are used to create special behavior when the files are rendered on Sauce's
website.

Images are in img/

Conventions
----

*Internal Links:* Tutorial pages can be linked to each other by wrapping a URL with `##`, as
in the following example:

    [Link to intro page](##00-Introduction.md##)

The link will be processed to point to the correct place in the Sauce website.

The same convention can be used for images. It is assumed they are in the `img`
directory so that prefix is not necessary:

    ![A Cool image](##my_cool_image.png##)

(This will display the image located at `img/my_cool_image.png`).

*Platform-specific sections:* Blocks of text which should only show up for specific platforms should be
wrapped like so:

    <!-- SAUCE:BEGIN_PLATFORM:MAC|LINUX -->
    .... some mac/linux only text here ....
    <!-- SAUCE:END_PLATFORM -->

Platform names can be `MAC`, `LINUX`, or `WIN`, and can be unioned by using the
pipe symbol.

*Sauce credentials:* You can insert the Sauce username or access key like so:

        vendor\bin\sauce_config.bat <!-- SAUCE:USERNAME --> <!-- SAUCE:ACCESS_KEY -->

Which will show up as &lt;username&gt; and &lt;access_key&gt; if the user is
not logged in.

*Sauce login widget:* If the user is not logged in, you can generate an inline signup form with:

    <!-- SAUCE:LOGIN -->

*Reusing template code:* It's possible to reuse markdown code in multiple tutorial files. For example,
if I have a file in `include/reuse-me.md`, I can have it generated in
a tutorial markdown file like so:

    ... some text ...
    <!-- SAUCE:INCLUDE:reuse-me -->
    ... more text ...

When the file is rendered on Sauce's website, the contents of
`include/reuse-me.md` will be inserted (before any other processing) in that
location. Note that "include/" and ".md" are part of the convention and are
assumed.

*Project-specific variables:* You can put arbitrary key/value mapping in
`properties.json` and reference the keys anywhere in the project; these keys
will be replaced with values on Sauce's website. For example, with the following
`properties.json` file:

```json
{
    "foo": "bar"
}
```

And the following markdown code:

```markdown
The specific value of the foo-like substance under discussion is <!-- SAUCE:PROP:foo ->.
```

Will be rendered as:

    The specific value of the foo-like substance under discussion is bar.

Contributing
----

If you find errors in the tutorials or ways that they can be improved, please
fork and send a pull request. We'd love your help in making these the best
instructions ever on how to get started with Ruby and Sauce
Labs!

Authors
----

*  Dylan Lacey ([jlipps](http://github.com/DylanLacey/))
*  Steven Hazel ([rubarb](http://github.com/sah/))