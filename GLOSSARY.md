# Portfolio

A personal portfolio site whose centrepiece is a field of soft 3D bodies, each of which leads to a piece of the owner's work.

## Language

**Project**:
A piece of the owner's work that the site links out to.
_Avoid_: Work, item, entry, link

**Project List**:
The single ordered collection of every Project; the only thing edited to add work to the site.
_Avoid_: Enum, registry, catalogue

**Blob**:
A soft, procedurally shaped 3D body that stands for exactly one Project. Blobs squish against each other but never fuse.
_Avoid_: Shape, ball, metaball, orb

**Slot**:
One of the fixed number of places for a Blob on screen. Several Slots may show the same Project when the Project List is shorter than the number of Slots.
_Avoid_: Position, cell

**Seed**:
The value, derived from a Project and its Slot, that fixes a Blob's shape and character so it is the same on every visit.

**Field**:
The shallow space the Blobs drift and collide in.
_Avoid_: Scene, canvas, stage

**Stream**:
The endless, wrapping movement of Blobs through the Field, which brings the next Projects in the Project List into view.
_Avoid_: Scroll, carousel, feed

**Label**:
The small text that names a Blob's Project when the Blob is hovered or selected.
_Avoid_: Tooltip, caption

**Index**:
The plain-text list of every Project, always available as an alternative to the Field.
_Avoid_: Fallback list, sitemap

**Reference**:
The style description of another designer's site that this design borrows from; source material, not the specification.
