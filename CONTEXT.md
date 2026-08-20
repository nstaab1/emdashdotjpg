# emdashdotjpg

A shared library of AI-generated placeholder images that agents and developers search before generating anything new. Generation costs the requester's own API credit; the resulting image joins the library for everyone.

## Language

### The library

**Commons**:
The shared, public library of generated images. Free to read, free to contribute to, no money ever changes hands.
_Avoid_: Marketplace, store, gallery, stock library

**Entry**:
One image in the Commons, together with its embedding and dimensions. Never with its Descriptor text.

**Contribution**:
The act of a newly generated image joining the Commons. Automatic, not a separate step.

### Asking for an image

**Descriptor**:
The plain-language description of the wanted image, supplied by whoever is asking. Private to the requester — it is embedded, never stored or published.
_Avoid_: Prompt, caption, alt text, query

**Match**:
An Entry judged close enough to a Descriptor to be used instead of generating. **Fuzzy** matching accepts semantic near-neighbours; **exact** matching refuses all of them and forces a Generation.

**Threshold**:
The similarity score above which a candidate Entry counts as a Match.

**Miss**:
A request that finds no Match, and so requires a Generation.

### Making an image

**Generation**:
Creating a new image from a Descriptor by calling an image model, paid for with the requester's own key.

**Provider**:
An external image-generation service. **Adapter** is our code for talking to one.

**Blocked term**:
A word or phrase that causes a Generation to be refused before any Provider is called.

### Using an image

**Placeholder**:
The stand-in image itself, as it appears in someone's site or app.

**Eject**:
Copying images out of the Commons and into the consuming project, so that project serves them itself. Copies; never removes the Entry.
_Avoid_: Download, export, vendor, self-host
