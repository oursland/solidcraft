# Encrypted design files

A SolidCraft design can be saved encrypted with GnuPG. The file is an OpenPGP message: its
plaintext is an ordinary `.sc` design container, exactly as a later build writes it, and a
holder of one of the recipient keys decrypts it with GnuPG. A key on a YubiKey's OpenPGP
smartcard is used where it lives; GnuPG asks for its PIN, and SolidCraft never sees the secret
key.

The file keeps its `.sc` name. It is not a ZIP: a program that reads designs as ZIP packages
cannot open it, so the file is marked as encrypted in the app (status line and the document tab's
tooltip) and old builds refuse it. `git diff` sees binary ciphertext, not design JSON; use a
plain `.sc` or `.json` design when a readable diff matters.

## Saving

- `file.save` / `file.save_as` take `encrypt: true` and `encryption_recipients` (GnuPG key ids,
  fingerprints or user ids, separated by commas). With no recipients, GnuPG's default recipient
  (the user's own key, so their YubiKey decrypts it) is used.
- In the app, Preferences › Security › *Encrypt saved designs with GnuPG* does the same for every
  save; its *Recipients* field is optional.
- The container is built first (signed when signing is on), then GnuPG encrypts those exact bytes.
  A signature therefore covers the design, not the ciphertext; the encryption is not a signature
  and does not say who wrote the design.
- An encrypted design saves encrypted again by default, even when the preference is off: opening
  an encrypted design and pressing Save must not silently write it in the clear. `encrypt: false`
  explicitly writes a plaintext copy (Save As is the natural way to do that).
- The save reply and the status line name the recipient key ids (`encryption: {method,
  recipients}`).

## Opening

- `doc.open` detects an OpenPGP message by its content, not its name, and asks GnuPG to decrypt
  it. A smartcard key asks for its PIN through pinentry; the open runs on the file-loading worker,
  so the window keeps drawing.
- The decrypted container is read like any other: model cache, signed provenance, previews and
  migrations all work; the design opens as saved. The recipients GnuPG names are kept, so a save
  encrypts to the same people.
- `file.verify` on an encrypted design decrypts it and then checks its signature (the reply says
  `encrypted: true`).
- The browser has no GnuPG: an encrypted design opens only in the desktop app. Signing and
  verification still work in the browser for unencrypted files.

## Recovery and backups

- Autosaves of an encrypted design are encrypted with the same recipients, so a crash leaves no
  plaintext design in the recovery folder. Opening a recovered design decrypts it like any other
  (with the same PIN prompt).
- A crash report of an encrypted design does not copy the design into the report: its
  `design.sc` is left out and the report says why. The command and parameters are still recorded,
  and a replay needs the encrypted file (and its key) beside it.
- The replaced file is kept as `<name>.sc.bak`, encrypted as that file was.

## Limits and hostile input

- The design file limit applies to the ciphertext; the decrypted plaintext is checked against the
  same limits before it is read (the container reader's entry count, part sizes, expanded and
  resolved budgets and duplicate-name checks).
- GnuPG runs as a separate program: a missing GnuPG, a missing secret key, a cancelled pinentry,
  a wrong PIN and an unusable recipient are reported as sentences, never as a crash. The
  ciphertext and the plaintext pass through files in a private temporary folder that is removed
  afterwards; the payload never appears in a command line.
- Encrypting to recipients is not authentication. A design that must prove who wrote it is
  signed (see [signed provenance](cose-provenance.md)); signing then encrypting gives both.
