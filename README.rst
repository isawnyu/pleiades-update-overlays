========================
Pleiades Update Overlays
========================

Introduction
============

In several projects we've scripted bulk updates to Pleiades places, locations,
and names using variations on the Pleiades `dump format
<https://github.com/isawnyu/pleiades-dump>`__ as input data formats.
A specification of what we will call the Pleiades Update Overlay Format (or
Overlay Format, for short) is the subject of this document.

Process
=======

TBD.

Overlay Format
==============

Here is the description of the format columns, organized into several different
groups. Each has required columns.

1. Identifier Columns
---------------------

id: string **required**
  The local id or short name for the object. For new overlays, the value of
  this id shall be -1.

version: string **required**
  The base version to which the overlay is to be applied. (TBD: Version conflict
  management.)

2. Common Editable Columns
--------------------------

creators: string **required**
  A comma-separated list of Pleiades usernames for contributors to the
  overlay data.

description: string **required for new overlays**
  A single paragraph describing the object, avoiding newlines, identation, and
  other formatting.

tags: string
  A comma-separated list of object tags.

title: string **required for new overlays**
  A short title for the object, avoiding punctuation when possible. The short
  name (or "id") of an object is usually formed from these titles.

initialProvenance: string **required for new overlays**
  Identifies the pre-Pleiades origins of data in the overlays. For places, this
  has generally been something like "BAtlas 11 G2 Fliessem-Otrang." For new
  locations coming from a published dataset this might be "OpenStreetMap Node:
  X (version Y, changeset Z)."

details: string
  Multiple lines of text about the place, location, or name. Something like
  compiler's introduction to a map in the Barrington Atlas's map-by-map
  directory is what we're looking for: not just telling the story of a place,
  but also the story of how we've organized knowledge about the place.

3. Additional Editable Columns for Locations and Names
------------------------------------------------------

timePeriodsKeys: string **required for new overlays**
  A comma-separated list of time periods keys such as "archaic,classical" from
  the controlled vocabulary at
  http://pleiades.stoa.org/vocabularies/time-periods.

pid: string **required**
  The numerical identifier for the location's parent place.

4. Additional Editable Columns for Locations
--------------------------------------------

geometry: string
  Geometry and coordinates in GeoJSON format (Note: longitude, latitude
  order!).

featureTypes: string
  A comma-separated list of feature types such as "settlement, temple" from the
  controlled vocabulary at http://pleiades.stoa.org/vocabularies/place-types.

5. Additional Editable Columns for Names
----------------------------------------

nameAttested: string **required for new overlays**
  Attested spelling of ancient name, not necessarily the same as the "title."

nameLanguage: string **required for new overlays**
  Short identifier for language and writing system associated with the attested
  spelling from the controlled vocabulary at
  http://pleiades.stoa.org/vocabularies/ancient-name-languages.

nameTransliterated: string **required for new overlays**
  Transliteration(s) of the attested name to Roman characters following the
  Classical Atlas Project scheme.

6. Additional Editable Columns for Places
-----------------------------------------

connectsWith: string
  A comma-separated list of numeric ids of other places to which this one
  connects.

featureTypes: string
  A comma-separated list of feature types such as "settlement, temple" from the
  controlled vocabulary at http://pleiades.stoa.org/vocabularies/place-types.

Examples
========

TBD.

Example Scripts
===============

TBD.

