========================
Pleiades Update Overlays
========================

Introduction
============

In several content development projects (DARMC coordinate imports, Scott
Vanderbilt's buildout of Hadrian's Wall milecastles, etc) work we've scripted
broad, bulk updates to Pleiades places, locations, and names (henceforth
"objects") using variations on the Pleiades `dump format
<https://github.com/isawnyu/pleiades-dump>`__ as input data formats. To open
this avenue of bulk updates to more Pleiades partners we document herein what
we will call the Pleiades Update Overlay Format (or Overlay Format, for short).
This format is designed for Pleiades users who would like to create or update
40-1000 objects at a time.

Overlay Process
===============

An object overlay consists of an array of key:value pairs (or items) where the
keys correspond to editable attributes of an object. The 'id' item identifies
the Pleiades object and the 'version' item identifies the version of that
object for which the overlay is intended. Multiple object overlays using the
same set of items can be collected and applied as a batch. These batches are
submitted in the form of a CSV file.

For example, the following overlay changes the description of version 3 of
Pleiades place 109390 (thereby creating a new version, 4) to "The territory of
an ancient Gallic people." and credits the contribution to two Pleiades users::

  id: 109390
  version: 3
  description: "The territory of an ancient Gallic people."  
  creators: user1, user2

The overlay would be delivered to Pleiades in the form of the following CSV
file::

  id,version,description,creators
  109390,3,"The territory of an ancient Gallic people.","user1, user2"

Other overlays can be sent as additional rows in the same file. On the
Pleiades side, the overlay is extracted from the CSV file and the history of
place 109390 is checked. If the current version is 3, the key:value items are
applied to the object and it is recataloged. If the current version does not
much the one in the overlay, the overlay is "stale". At the present time, we
must reject such stale overlays and send them back to their source to be
refreshed.

Overlay Format Specification
============================

The names of keys, the types of their values, and whether and when they are
required are explained here.

All keys and values must be encoded as UTF-8.

1. Identification Items
-----------------------

id: string **required, n/a for new locations or names**
  The local id or short name for the object. For overlays that create new
  objects, the value shall be prefaced with a single underscore so that it
  may be referred to by other overlays.

pid: number **required for locations and names, n/a for places**
  The id of the parent place.

version: number **required for updates, n/a for new overlays**
  The base version to which the overlay is to be applied.

2. Common Overlay Items
-----------------------

creators: string **required**
  A comma-separated list of Pleiades usernames for contributors to the
  overlay data. These will be **appended** to the object metadata.

description: string **required for new overlays**
  A single paragraph describing the object, avoiding newlines, identation, and
  other formatting. This **replaces** the existing value.

tags: string
  A comma-separated list of object tags that **replaces** the existing value.

title: string **required for new overlays**
  A short title for the object, avoiding punctuation when possible. The short
  name (or "id") of an object is usually formed from these titles. **Replaces**
  the existing value.

initialProvenance: string **required for new overlays, n/a for updates**
  Identifies the pre-Pleiades origins of data in the overlays. For places, this
  has generally been something like "BAtlas 11 G2 Fliessem-Otrang." For new
  locations coming from a published dataset this might be "OpenStreetMap Node:
  X (version Y, changeset Z)."

details: text
  Multiple lines of text about the place, location, or name. Something like
  compiler's introduction to a map in the Barrington Atlas's map-by-map
  directory is what we're looking for: not just telling the story of a place,
  but also the story of how we've organized knowledge about the place.
  **Replaces** the existing value.

references: JSON array
  References to **append** to the existing ones. A reference is a 3 part thing
  and shall be formatted as a comma-separated list of Link, Label, Type enclosed
  within square brackets. Multiple references can be encoded as a comma-separated
  list of these square bracketed items. See example at the end of this document.

3. Additional Overlay Items for Locations and Names
---------------------------------------------------

timePeriods: string **required for new locations, names; n/a for places**
  A comma-separated list of time periods identifiers such as
  "archaic,classical" from the controlled vocabulary at
  http://pleiades.stoa.org/vocabularies/time-periods. **Replaces** the
  existing value.

4. Additional Overlay Items for Locations
-----------------------------------------

geometry: JSON object
  Geometry and coordinates in GeoJSON format (Note: longitude, latitude
  order!). **Replaces** the existing value.

featureTypes: string
  A comma-separated list of feature type identifiers such as "settlement,
  temple" from the controlled vocabulary at
  http://pleiades.stoa.org/vocabularies/place-types. **Replaces** the existing
  value.

paaid: string **required for new overlays**
  The id of the overlay's proper positional accuracy assessment document, such
  as "generic-osm-accuracy-assessment" for
  http://pleiades.stoa.org/features/metadata/generic-osm-accuracy-assessment.

5. Additional Overlay Items for Names
-------------------------------------

nameAttested: string **required for new overlays**
  Attested spelling of ancient name, not necessarily the same as the "title."
  **Replaces** the existing value.

nameLanguage: string **required for new overlays**
  Short identifier for language and writing system associated with the attested
  spelling (such as "akk" – Akkadian, or "la" – Latin) from the controlled
  vocabulary at http://pleiades.stoa.org/vocabularies/ancient-name-languages.
  **Replaces** the existing value.

nameTransliterated: string **required for new overlays**
  Comma-separated list of transliteration(s) or romanizations of the attested
  name to Roman characters following the Classical Atlas Project scheme. For
  example: "Rabḍ al-Ḫandaq, Chandax".  **Replaces** the existing values.

6. Additional Overlay Items for Places
--------------------------------------

connectsWith: string
  A comma-separated list of numeric ids of other places to which this one
  connects. **Replaces** the existing value.

featureTypes: string
  A comma-separated list of feature type identifiers such as "settlement,
  temple" from the controlled vocabulary at
  http://pleiades.stoa.org/vocabularies/place-types. **Replaces** the existing
  value.

Overlay Examples
================

Adding a New Location to a Place
--------------------------------

To add a new point location citing OpenStreetMap to a place::

  pid: 462310
  id: -1
  title: Domus Romana
  description: West corner of the museum built on the site of a Roman Villa. Time periods following the Barrington Atlas (BAtlas 47 inset Melita).
  paaid: generic-osm-accuracy-assessment
  timePeriods: classical, hellenistic-republican
  featureTypes: villa
  geometry: { "type": "Point", "coordinates": [ 14.4001243, 35.8851671 ] }
  references: ["http://www.openstreetmap.org/browse/node/385317114", "osm:node=385317114", "Cites"]
  initialProvenance: OpenStreetMap Node: 385317114 (version 1, changeset 976247)

The result, a new location (version 1) at /places/462310/domus-romana, might
have its details field updated like so::

  id: domus-romana
  version: 1
  pid: 462310
  details: This villa was uncovered in 1920.
  references: ["http://www.heritagemalta.org/museums/domusromana/domushistory.html", "Domus Romana Museum Website, Heritage Malta", "See Further"]

Adding a Place and a Location Simultaneously
--------------------------------------------

  id: _1
  title: Road Station 
  description: An unnamed road station between A and B.

  pid: _1
  title: NW corner
  description: the NW corner of the site.
  timePeriods: archaic, classical
  featureTypes: station
  paaid: survey-of-site-x-2010
  geometry: { "type": "Point", "coordinates": [ 14.4001243, 35.8851671 ] }
  references: ...
  initialProvenance: ...

And the CSV file::

  id,pid,title,description,...
  _1,,Road Station,An unnamed road station between A and B.,...
  ,_1,NW corner,the NW corner of the site.,...

Example Scripts
===============

TBD.

