Here is a parser that converts GEDCOM files to a _de facto_ object model.

__[Try the demo](https://github.com/DallanQ/Gedcom/wiki/Demo)__.

De Facto object model
---------------------

_De Facto_ means _"In fact or in practice; in actual use or existence,
regardless of official or legal status."_

The parser converts GEDCOM files to an object model that includes all of the
information found in the majority of GEDCOMs in the wild.
The model includes additional tags over those in the official GEDCOM standard,
because they are commonly used in GEDCOM files, and excludes a few tags from
the official GEDCOM standard that are never or only rarely used.

Further, most of the information from GEDCOMs that cannot be represnted directly
in the model is represented as _extensions_ so it is not lost.

The object model includes classes and attributes for every GEDCOM tag sequence
appearing in more than 4% of the 7000 GEDCOMs submitted to
[WeRelate.org](http://www.werelate.org) over the past five years, with the
exception of four software-specific _schema_ tags:
_SCHEMA, _EVENT_DEFN, _PLAC_DEFN, and _EVDEF, generated by _Family Tree Maker_,
_Personal Ancestral File_, _Legacy_, and _RootsMagic_ respectively.

Additional information found in the GEDCOMs, such as the schema tags mentioned
above, is represented in the model by extending model objects with the ability to
store lists of additional tags.

The result is that object model directly represents _all_ of the information found
in nearly 50% of the GEDCOMs. This may not sound like a large percentage, but
due to the standard not being updated in over 10 years, nearly everyone adds
their own custom tags. So having a relatively simple object model represent all
tags found in nearly 50% of GEDCOMs is an accomplishment.

If we also include the additional tags storable on model objects, the
model is able to represent all of the information found in roughly 98% of the
GEDCOMs submitted to WeRelate.

The object model has the normal classes you'd expect for a GEDCOM-based object model:
people, families, source citations, sources, notes, repositories, etc.
The purpose of this project is not to propose a new object model, but to _expose_
the object model that is currently used by genealogists and make it easy to work with.

A new proposed object model could use this project to convert existing GEDCOM files
to the new model by first converting them to the de facto object model, then
transforming the objects into the proposed object model.

For more information about the object model,
__[see the wiki](https://github.com/DallanQ/Gedcom/wiki)__.

Extendible
----------

Developers can add custom extensions to the model.  An extension might annotate
people with warnings about suspicious dates for example.

Parsers
-------

The project includes three parsers:

* from GEDCOM to the de facto object model (ModelParser): the object model and custom extensions
can be saved as a json file,

* from GEDCOM to a general tree-based object model that captures _everything_ within
the GEDCOM file (TreeParser); the tree-based object model can also be saved as a json file,

* from json to the de facto object model or the tree-based object model (JsonPrser),

as well as a GEDCOM export tool:

* from the de facto object model to GEDCOM (GedcomWriter).

Round-trippable
---------------

It is possible to do a round-trip: parse a GEDCOM file into the object model,
save it to json, read it back from json, and export it back to GEDCOM, without
any loss of information for the majority (over 94%) of GEDCOM files.

The round-trip capability allows anyone to create programs that read gedcom files,
do interesting things like generate warnings for suspicious dates in the GEDCOM,
allow the user to correct the warnings, and save the information back as a GEDCOM
file without loss of information from the original GEDCOM for the vast majority of
GEDCOM files.

Building
--------

You'll need maven. `mvn install` creates the jar file.

Tools
-----

* _Gedcom2Json.java_ converts a GEDCOM to a JSON file using either the model parser or the tree parser.

* _Gedcom2Gedcom.java_ round-trips a GEDCOM file or a directory of GEDCOM files from GEDCOM, to the
object model, and back to GEDCOM (in a different directory).

* _CompareGedcom2Gedcom.java_ does the thing as Gedcom2Gedcom, and then compares the resulting GEDCOM
to the original, reporting any differences.

* _GedcomAnalyzer.java_ parses a GEDCOM file or a directory of GEDCOM files and reports tags that are
stored as extensions and errors.

* _PlaceWriter.java_ extracts all places from a directory of GEDCOM files, as an example of walking
the model using a Visitor pattern. This function was written in just a few lines due to the other
classes in this project.

The tools can be run using
`mvn exec:java -Dexec.mainClass=org.folg.gedcom.tools.<tool name> -Dexec.args="<args>"`

Roadmap
-------

* Once a GEDCOM has been imported, it still needs to be checked for referential integrity, and
missing back-references must be added.  For example, if person A references family B, but family B does
not reference person A, we need to add a reference from family B to person A.

* If we're willing to forego round-trippability, we can handle additional tags that are stored as
extensions currently.  For example, some GEDCOMs use FAMILY instead of FAM for family tags.  We
could create Family objects from FAMILY tags, just as we do with FAM tags, but they would be exported
as FAM tags, not FAMILY tags.

Other projects
--------------

Check out [other genealogy projects](https://github.com/DallanQ)
