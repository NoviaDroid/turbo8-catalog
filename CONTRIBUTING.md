# Submit a Turbo8 cartridge

Use SUBMIT / UPDATE ONLINE in the Turbo8 Player library. GitHub authentication uses your own account. Every submission is public and reviewed before publication.

One pull request changes one `carts/u<GitHub numeric user ID>-<32 lowercase hex>/` directory. The directory contains `submission.json` and `cartridge/` with the expanded Turbo8 ZIP. Authors must own or have permission to distribute all submitted code, artwork and audio. Code and asset permissions are recorded separately. Remixes retain attribution and source IDs.

Updates keep the cartridge ID and author ID and increase the semantic version. The verified PR author must match the author ID. Only the maintainer merges. A merge means approved; the cartridge becomes available after publication and public download verification.

The review workflow reads PR files as data using trusted base-branch tooling. It never executes submitted JavaScript. A syntax check and candidate ZIP are provided for maintainer review. Run gameplay only in an appropriate isolated player.

Maintainer tooling changes are separate from cartridge submissions. The cartridge validator intentionally rejects changes outside the selected cartridge directory. Administrators retain maintenance access.
