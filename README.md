# gradingmagic

## What is it?

`gradingmagic` is a suite of dumb little scripts that help to automate the grading &
feedback process on D2L.

D2L has a bulk-feedback submission mechanism, but to be truly useful it seems to
require that the files containing feedback from the instructor must have the
same name as the corresponding student's original submission.

Run `gradingmagic` once on a directory full of unzipped student submissions, and
it will create a `.md` file for each one. Edit each `.md` file with your
feedback, and when you're done, run `gradingmagic` again to generate a properly
formatted `.zip` that you can submit back to D2L using the `Add feedback files
button` in the dropbox.

To make these easiest to use, it is recommended you add them to your `$PATH`.

## Dependencies

* `pandoc`
* `zip`

## Usage

### gradingmagic

Usage: `gradingmagic [path] [output_tag]`

`path` is optional -- if no path is given, the script will default to the
current directory.

`output_tag` will appear in the name of the output files, and is always appended
with `_feedback`. It will default to whatever the `basename` of `path` is, if no
value is given. Note that this is bash, and I haven't included support for
position-independent parameters, so to use `output_tag`, you need to specify a
`path`.

Creates empty Markdown templates, and converts them to PDF after you've edited
them, and then zips those PDFs up into a zip file that you can upload to D2L.
The empty templates are created in the `path` directory, and in order for them
to be compiled, the must be moved to `[path]/feedback`. The `feedback` tool can
automate the process of editing and moving them to `[path]/feedback`, or you can
just run `mv *md feedback/`.

### feedback

Usage: `feedback`

No arguments. Must be run from whatever your equivalent is of the local `Lab1`
folder in the example workflow below. It will open a students submission
automatically (using `xdg-open`, although this may need to be replaced with
`gnome-open` or something depending on your system) so you can grade it, as well
as opening the corresponding Markdown template in VIM. When you close a file
with VIM, it is immediately moved to `[path]/feedback`. You can quit using
`feedback` and any point and pick up where you left off, since the Markdown
files are moved to `[path]/feedback` as you finish editing each one.
`.md` to `[path]/feedback` 

### gengrades

Usage: `gengrades [feedback_path] [exported_grades_csv] [max_points]`

`gengrades` generates a CSV file with grades that are extracted from the
feedback Markdown files that you create. `exported_grades_csv` must be obtained
from D2L -- it contains unique identifiers for each student that must be used in
any CSV you want to _upload_ to D2L.

`gengrades` uses the name of `feedback_path`'s *parent directory* as the *name
of the grade item*.

As of now, it supports numerical grades only.

See [example_feedback.md](./example_feedback.md) for an example feedback
Markdown with grading syntax for automatic final score calculation.

## Example Workflow

Imagine you have a dropbox folder called `Lab1`, with a maximum point value of
30 points.

1. Download zipped submissions for dropbox titled `Lab1`.
2. Unzip contents of zip file to local directory called `Lab1`
3. `cd Lab1/`
4. `gradingmagic`
5. `feedback`
6. `gradingmagic`
7. `gengrades ./feedback [path to CSV grades exported from D2L] 30` <- `30` is
   the max points for this lab.

## Notes

To avoid duplicate feedback items, it's best if you set your D2L dropbox to only
allow one student submission (i.e. new submission overwrites older submission).
