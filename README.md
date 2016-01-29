# gradingmagic

## What is it?

`gradingmagic` is a dumb little script that helps to automate the grading &
feedback process on D2L.

D2L has a bulk-feedback submission mechanism, but to be truly useful it seems to
require that the files containing feedback from the instructor must have the
same name as the corresponding student's original submission.

Run `gradingmagic` once on a directory full of unzipped student submissions, and
it will create a `.md` file for each one. Edit each `.md` file with your
feedback, and when you're done, run `gradingmagic` again to generate a properly
formatted `.zip` that you can submit back to D2L using the `Add feedback files
button` in the dropbox.

## Dependencies

* `pandoc`
* `zip`

## Usage

Usage: `gradingmagic [path] [output_tag]`

`path` is optional -- if no path is given, the script will default to the
current directory.

`output_tag` will appear in the name of the output files, and is always appended
with `_feedback`. It will default to whatever the `basename` of `path` is, if no
value is given.

## Notes

To avoid duplicate feedback items, it's best if you set your D2L dropbox to only
allow one student submission (i.e. new submission overwrites older submission).
