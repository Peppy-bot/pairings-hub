# Pairings Hub

A repository of Peppy **pairings** (`peppy_schema: "pairing/v1"`).

A pairing is the same kind of document as a contract, a shape both sides inherit
by reference rather than duplicate, except that it names *two* roles and both
directions of a bidirectional relationship. Two instances, one per role, are
paired 1:1 over it.

## Adding a pairing

Create a new `.json5` file under the relevant category:

```json5
{
  peppy_schema: "pairing/v1",
  manifest: {
    name: "<pairing_name>",
    tag: "<tag>", // tag is a pairing identifier like "v1", not semver (dots forbidden)
    description: ["<what the pairing is for and what each role does>"],
  },
  roles: ["<role_a>", "<role_b>"],       // exactly two
  topics: [ /* each entry's `emitted_by` names one of the two roles, and its `description` says what it carries */ ],
}
```

Pairings declare topics only: no services, no actions.

`description` is a list of paragraphs, and peppy refuses a pairing without it. The first paragraph is the summary that `peppy repo search` prints; `peppy repo show` prints them all, with the description of each topic. A field of a message format can have a `$description` too: `opening: { $type: "f64", $description: "0 closed, 1 fully open." }`. Notes for the maintainers of a pairing stay comments.

## Use

This repo is consumed by `peppy repo refresh` alongside node, contract, and launcher repositories.

## Adding an item to this repository

This repository publishes what `peppy_repository.json5` says it publishes, and nothing else. An item
that is not listed there is invisible to peppy, so after adding, moving, or renaming a pairing, run:

```sh
peppy repo index .
```

Commit the updated `peppy_repository.json5` alongside your change, and run
`peppy repo index --check` before pushing: it fails if the index has drifted
from the repository, naming the file and the identity involved.

Generation refuses, naming both files, if your change claims a `name:tag` another one already
publishes. Rename yours: within one repository, a `name:tag` is claimed by exactly one file.
