---
id: 13
title: "Decoupling personal thought from the cloud using the Logos tech Stack"
status: approved
author: "xAlisher"
created: "2026-03-06"
source_issue: 13
---

# Decoupling personal thought from the cloud using the Logos tech Stack

## Description
Encrypted notes manager utilising Logos stack to store, edit and sync encrypted notes.

## Social Problem & Logos Stack Solution
Most of commonly used notes managers storing data in centralised databases - leaking metadata and not reliable.

## Previous Attempts & Hypotheses
Three  "waves" of private note apps, and their failures:

Wave 1: "Trust Us" (Evernote, Notion)
• Hypothesis: Centralized servers are fine if the company is "good."
• Reality: Data leaks, and "The Big Off Switch." If the company dies, your thoughts die with them.

Wave 2: "Do It Yourself" (Obsidian)
• Hypothesis: Keep files on your hard drive and sync them yourself.
• Reality: Syncing is a nightmare. Users end up using iCloud or Google Drive to move files, which just hands their data back to Big Tech through the back door.

Wave 3: "Blockchain is a Hard Drive"
• Hypothesis: Put everything on-chain.
• Reality: Too slow, too expensive, and impossible to scale.

The Immutable hypothesis
I believe the "missing link" is not a better app, but a better pipe. By using the Logos Stack, we don't ask you to trust a company or a clunky manual sync. We use Logos Messaging to "whisper" updates between your devices and Logos Storage to make sure your vault lives forever on a decentralized web, accessible only by your Keycard.

## Functional Specifications
Functionality
- Hardware-rooted privacy: Notes are encrypted via Keycard; keys never touch the OS or the cloud.
-  P2P Sync: Uses Logos Messaging for real-time, serverless note updates across devices.
-  Permanent archive: Long-term backups are pushed to Logos Storage for censorship-resistant persistence.

Usability 
- No email or password. Tap or insert a Keycard to instantly decrypt and load your vault.
- Split-pane Interface: A clean, familiar Markdown editor for a "no-learning-curve" experience.
- App relies on Logos storage to store notes and Logos messaging to sync updates 

Reliability
- Local-first speed: Reads and writes happen on the local device instantly; syncing happens in the background.
- Lightweight Messaging: Only encrypted "diffs" (changes) are sent over the network to save bandwidth.

 Supportability
- Open protocol: Built on the open-source Logos stack, ensuring no vendor lock-in and community-driven security audits.

## Risks & Deployment Strategy
Phase 1: Local-First
• Objective: Build a high-performance Markdown editor that functions entirely offline.
• Key Tech: Local storage using IndexedDB or SQLite.
• Logos Integration: Keycard integration for initial vault unlocking.
• Outcome: A functional "siloed" notes app where the user owns the local keys.

Phase 2: Sync
• Objective: Multi-device synchronization without a central relay.
• Key Tech: Logos Messaging
• Implementation: When a note is saved, it’s encrypted and broadcast over a private P2P topic. Other devices owned by the same Keycard identity listen for these messages to update their local state.
• Outcome: Real-time sync that is invisible to outside observers.

Phase 3: Immutable memory
• Objective: Decentralized backup and "Cold Storage."
• Key Tech: Logos Storage.
• Implementation: Periodically, the app bundles the encrypted note vault into an archive and pushes it to the Logos Storage network.
• Outcome: Even if the user loses all their devices, they can recover their entire knowledge base using just their Keycard.