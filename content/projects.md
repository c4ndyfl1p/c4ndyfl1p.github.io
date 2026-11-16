+++
title = "Projects"
slug = "projects"
description = "Things I've built, made, broken and written about: security and cryptography, software, digital fabrication, and people & technology."
+++

A map of what I've been up to, grouped by thread. Most entries link to a longer post.

**Jump to:** [Security & cryptography](#security) · [Software](#software) · [Fabrication & making](#fabrication) · [People & technology](#people)

<!-- TODO: add a CV link here once there's a PDF in static/, e.g. [CV (PDF)](/cv.pdf) -->

## Security & cryptography {#security}

- **[Master's thesis: lattice-based zero-knowledge proofs with VOLE]({{< relref "posts/thesis.md" >}}).** Zero-knowledge proofs that a post-quantum (ML-KEM / Kyber) ciphertext is well formed, without revealing its secrets, plus a commit-and-sample trick that makes batched proofs smaller. Supervised by Peter Scholl and Diego F. Aranha at Aarhus University. [Read the thesis (PDF)](/voles_ZK_thesis-11.pdf).
- **Cryptopals, set 2.** Writeups breaking AES in ECB mode:
  [detecting ECB vs CBC]({{< relref "posts/cryptopals-set2-challenge11.md" >}}),
  [byte-at-a-time ECB decryption]({{< relref "posts/cryptopals-set2-challenge12.md" >}}),
  and [forging an admin profile by cutting and pasting ciphertext blocks]({{< relref "posts/cryptopals-set2-challenge13.md" >}}).
  Code on [GitHub](https://github.com/c4ndyfl1p/matasano-cryptopals).
- **[Mein Browserland #1]({{< relref "posts/MeinBrowserland1.md" >}}).** Why the web is so hard to secure, and how cookies and authentication work in the browser.
- **[Tor: the second-generation onion router]({{< relref "posts/tor.md" >}}).** A report from the Computer Security and Privacy course project.

## Software {#software}

- **[बोलLang]({{< relref "posts/bolang.md" >}}).** A Firefox extension with a Flask backend that swaps some English words on the pages you read for Danish (or German, Spanish...), so you pick up vocabulary while browsing.
  Code on [GitHub](https://github.com/c4ndyfl1p/bolang).
- **[Real-time web apps with Django]({{< relref "posts/realTimeWebAppBidirectionCommunication.md" >}}).** How HTTP works and the options for two-way communication between server and browser, from polling to WebSockets.
- **[Self-hosting notes](/notes/).** Running my own cloud on a Raspberry Pi:
  [why self-host]({{< relref "notes/SelfhostingAndHomeLab.md" >}}),
  [setting up the server]({{< relref "notes/HomelabSetup.md" >}}),
  [an external disk for Nextcloud data]({{< relref "notes/wiki-nexcloudMediaDirectory.md" >}}),
  and [reinstalling Nextcloud]({{< relref "notes/wiki-reinstallingNextcloud.md" >}}).

## Fabrication & making {#fabrication}

- **[A dental interface prototype]({{< relref "posts/SLAprinting.md" >}}).** An ongoing research project on typing with your tongue (Engineering Interactive Technologies, Aarhus University). I work on the fabrication side: taking dental impressions, 3D-scanning and modelling them, and SLA-printing the appliance that holds a flexible capacitive circuit.
- **[Getting carried away with my Ender 3]({{< relref "posts/3dprintingAndWizardy.md" >}}).** CAD designs and print experiments on an old single-nozzle printer: exposed infill, mid-print colour changes, G-code edits and post-processing.
- **[Face shields in the first COVID wave]({{< relref "posts/3dprintingFaceShields.md" >}}).** Printing face shield frames for frontline workers with my first 3D printer.

## People & technology {#people}

The thread that runs through most of the above: how technology shapes the people who use it, and how to give them more control over it.

- **[AI and art]({{< relref "posts/aiAndArt.md" >}}).** A research project built on podcast interviews with artists who use AI in their practice. [Watch the interview with Hasan](https://www.youtube.com/watch?v=NNwySmrM9IU).
- **[Monitoring urban damage with InSAR in Gaza]({{< relref "posts/insarGaza.md" >}}).** Using satellite radar coherence to map likely structural damage in Gaza in October 2023. A GIS course project at Ashoka University that won 3rd prize as a poster at the NASA-SARI meeting. [Report (PDF)](/pdfs/insar-gaza-report.pdf).
- **Who owns the internet?** An essay in two parts:
  [what the internet is and who controls it]({{< relref "posts/whoOwnsTheInternet.md" >}}),
  and [how it turned into an ad-funded economy]({{< relref "posts/whoOwnsTheInternet2.md" >}}).
- **[Destination Outside]({{< relref "posts/Destination-outside.md" >}}).** Short fiction from the first lockdown, about life lived through screens.
- See also the [dental interface prototype](#fabrication), a study of a new way to type, and [बोलLang](#software), which teaches you a language while you browse.
