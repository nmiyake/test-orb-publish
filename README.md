test-orb-publish
================
`test-orb-publish` is a playground used for testing the process of publishing CircleCI Orbs from a mono-repo that
contains multiple orbs.

The [nmiyake/orb-publisher](src/orb-publisher/orb.yml) orb is the result of this experimentation. This orb defines jobs
that can be used to lint, build and publish orbs in a monorepo. This design of the orb has the following assumptions:
* Orbs are defined in `src/{ORB_NAME}/orb.yml`
* Tags for the repo are of the form `{ORB_NAME}/v?{VERSION}` (for example, `orb-publisher/1.0.0` or `orb-publisher/v1.0.0`) 

The orb publisher has the following properties:
* On the development branch, only orbs that are modified by the build are published, and they are published as 
  `${ORB_NAME}@dev:${CIRCLE_BRANCH}-${CIRCLE_SHA1}`
* When a tag is added to the repository, the orb name and version are parsed from the tag, and a publish operation is 
  performed for the matching orb using the specified version

Philosophically, this orb publishing workflow assumes a workflow where tags are applied by some external mechanism 
(either a human or some kind of automation that determines the next version based on changelogs etc.). This externalizes
the determination of whether a new version is a patch, minor or major change and makes the workflow analogous to other
monorepo publishing workflows.

The `test-orb-publish` repository uses the `nmiyake/orb-publisher` orb itself to publish its own orb definitions. The
other orbs in the repository are essentially no-op orbs that exist only to validate the monorepo workflow.

The [CircleCI configuration for the github.com/CircleCI-Public/circleci-orbs](https://github.com/CircleCI-Public/circleci-orbs/blob/staging/.circleci/config.yml)
provided the main building blocks for this orb.
